# Parse a Log File for Failed Auth Attempts

### Lab Objective
In this lab we'll write a Python program that parses the Keystone log file (typically found in) **/var/log/keystone/keystone-wsgi-public.log** for failed authorization attempts (specifically for logging into Horizon, the web GUI for OpenStack). If you haven't worked with OpenStack, then you should know that Keystone is the service that authorizes a user's credentials. As you will soon observe, the log files are long and difficult to parse (for human eyes).

In the real world, we'd want some sort of script (or callable function) that could parse the log file for us, looking for patterns (via RegEx matching) that indicate a failed login attempt.

If you're new to Python, this might be a difficult program to write. That's okay! Do your best. The solution can be found at the bottom of the lab and is well commented. Even if you cannot write the code yourself, be sure to review all of the comments before executing the solution.

This program should run in Python 3.x

### Procedure

0. Take a moment to clean up your remote Desktop. For now, close all other terminal spaces or windows you might have open.

0. Open a new terminal, then *cd /home/student/bin* to the bin directory

    `student@beachhead:/$` `cd /home/student/bin`

0. Let's review what a **FAILED** login attempt to Horizon looks like, as entered into **/var/log/keystone/keystone-wsgi-public.log** by Keystone.

    ```
    2017-07-30 17:16:12.925 3281 INFO keystone.common.wsgi [req-10b2861f-a302-4767-b484-39731f9371a8 - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:16:13.111 3281 WARNING keystone.common.wsgi [req-10b2861f-a302-4767-b484-39731f9371a8 - - - - -] Authorization failed. The request you have made requires authentication. from 172.16.1.5
    ```
0. It looks like the words, **Authorization failed.** might be a good 'match' parameter.

0. Let's review what a **successful** login attempt to Horizon looks like, as entered into **/var/log/keystone/keystone-wsgi-public.log** by keystone. All of the following information is written to the log file for a single successful log in attempt.

    ```
    2017-07-30 17:17:06.007 3277 INFO keystone.common.wsgi [req-9b361800-9d19-4c4c-8e2c-7f75014e1962 - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:17:06.105 3277 INFO keystone.token.providers.fernet.utils [req-9b361800-9d19-4c4c-8e2c-7f75014e1962 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:06.123 3281 INFO keystone.token.providers.fernet.utils [req-29d72fde-a21e-4bb1-8b00-8509aaf0cdc6 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:06.175 3281 INFO keystone.common.wsgi [req-29d72fde-a21e-4bb1-8b00-8509aaf0cdc6 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:17:06.181 3281 INFO keystone.token.providers.fernet.utils [req-29d72fde-a21e-4bb1-8b00-8509aaf0cdc6 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:06.253 3281 WARNING keystone.common.wsgi [req-29d72fde-a21e-4bb1-8b00-8509aaf0cdc6 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Authorization failed. The request you have made requires authentication. from 172.16.1.5
    2017-07-30 17:17:06.264 3277 INFO keystone.common.wsgi [req-b38b1443-de6e-4ffc-b663-cba3993db962 - - - - -] GET http://controller:5000/v3/
    2017-07-30 17:17:06.269 3281 INFO keystone.token.providers.fernet.utils [req-0951a578-a558-4e4b-a85b-aec470e94b04 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:06.292 3281 INFO keystone.common.wsgi [req-0951a578-a558-4e4b-a85b-aec470e94b04 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] GET http://controller:5000/v3/users/63dee1ed626b4040bcd43b3492997a8c/projects
    2017-07-30 17:17:06.330 3278 INFO keystone.token.providers.fernet.utils [req-31b61e9e-81c4-4282-9b1f-5d4cac27bbd2 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:06.384 3278 INFO keystone.common.wsgi [req-31b61e9e-81c4-4282-9b1f-5d4cac27bbd2 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:17:06.399 3278 INFO keystone.token.providers.fernet.utils [req-31b61e9e-81c4-4282-9b1f-5d4cac27bbd2 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:06.564 3278 INFO keystone.token.providers.fernet.utils [req-31b61e9e-81c4-4282-9b1f-5d4cac27bbd2 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:07.210 3281 INFO keystone.token.providers.fernet.utils [req-d33d467e-e454-41f9-a107-8b62abbf43e8 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:17:07.242 3281 INFO keystone.common.wsgi [req-d33d467e-e454-41f9-a107-8b62abbf43e8 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] GET http://controller:5000/v3/users/63dee1ed626b4040bcd43b3492997a8c/projects
    ```

0. Uh, oh. So there is a mentioned of a **Authorization failed** in a successful login (see line 6 in the above example). However, is a difference. A successful login lists (**-] Authorization failed**). Whereas a **truely failed login** lists not one, but several dashes (**- - - - -] Authorization failed**). This seemingly small difference is plenty for our RegEx parsing tools to pick up on.

0. The following is a snippet from the same log file, **/var/log/keystone/keystone-wsgi-public.log**, however, we're now looking at 3 failed 3 failed login attempts (two with the username admin, and one with username root), and then a single successful login (with the username admin).

    ```
    2017-07-30 17:20:20.455 3279 INFO keystone.common.wsgi [req-b28d7cf9-8591-4629-99a1-bdcd85b046b4 - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:20.506 3279 WARNING keystone.common.wsgi [req-b28d7cf9-8591-4629-99a1-bdcd85b046b4 - - - - -] Authorization failed. The request you have made requires authentication. from 172.16.1.5
    2017-07-30 17:20:22.553 3280 INFO keystone.common.wsgi [req-ef70efb4-0b60-4738-b8ba-204c657aebcc - - - - -] GET http://controller:5000/v3/
    2017-07-30 17:20:22.564 3279 INFO keystone.common.wsgi [req-2a491a01-a525-46e6-ba26-b2d53ab02567 - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:22.727 3279 INFO keystone.token.providers.fernet.utils [req-2a491a01-a525-46e6-ba26-b2d53ab02567 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:24.228 3280 INFO keystone.common.wsgi [req-e34f17f4-4957-4117-a174-73fb8de19d29 - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:24.293 3280 WARNING keystone.common.wsgi [req-e34f17f4-4957-4117-a174-73fb8de19d29 - - - - -] Authorization failed. The request you have made requires authentication. from 172.16.1.5
    2017-07-30 17:20:29.055 3278 INFO keystone.common.wsgi [req-98982a97-90ca-411f-843d-c4dd5186fe5a - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core [req-98982a97-90ca-411f-843d-c4dd5186fe5a - - - - -] Could not find user: root
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core Traceback (most recent call last):
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/keystone/auth/plugins/core.py", line 173, in _validate_and_normalize_auth_data
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     user_name, domain_ref['id'])
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/keystone/common/manager.py", line 124, in wrapped
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     __ret_val = __f(*args, **kwargs)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/keystone/identity/core.py", line 433, in wrapper
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     return f(self, *args, **kwargs)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/keystone/identity/core.py", line 443, in wrapper
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     return f(self, *args, **kwargs)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/cache/region.py", line 1053, in decorate
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     should_cache_fn)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/cache/region.py", line 657, in get_or_create
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     async_creator) as value:
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/core/dogpile.py", line 158, in __enter__
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     return self._enter()
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/core/dogpile.py", line 98, in _enter
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     generated = self._enter_create(createdtime)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/core/dogpile.py", line 149, in _enter_create
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     created = self.creator()
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/cache/region.py", line 625, in gen_value
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     created_value = creator()
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/dogpile/cache/region.py", line 1049, in creator
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     return fn(*arg, **kw)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/keystone/identity/core.py", line 902, in get_user_by_name
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     ref = driver.get_user_by_name(user_name, domain_id)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core   File "/usr/lib/python2.7/dist-packages/keystone/identity/backends/sql.py", line 253, in get_user_by_name
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core     raise exception.UserNotFound(user_id=user_name)
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core UserNotFound: Could not find user: root
    2017-07-30 17:20:29.083 3278 ERROR keystone.auth.plugins.core 
    2017-07-30 17:20:29.089 3278 WARNING keystone.common.wsgi [req-98982a97-90ca-411f-843d-c4dd5186fe5a - - - - -] Authorization failed. The request you have made requires authentication. from 172.16.1.5
    2017-07-30 17:20:35.852 3279 INFO keystone.common.wsgi [req-2b0ef7e5-a560-4bdd-9815-a7b554fbebb6 - - - - -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:35.957 3279 INFO keystone.token.providers.fernet.utils [req-2b0ef7e5-a560-4bdd-9815-a7b554fbebb6 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:35.967 3280 INFO keystone.token.providers.fernet.utils [req-299cb3c0-2c36-492d-92d7-4d50f0e5ed55 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:36.010 3280 INFO keystone.common.wsgi [req-299cb3c0-2c36-492d-92d7-4d50f0e5ed55 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:36.016 3280 INFO keystone.token.providers.fernet.utils [req-299cb3c0-2c36-492d-92d7-4d50f0e5ed55 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:36.097 3280 WARNING keystone.common.wsgi [req-299cb3c0-2c36-492d-92d7-4d50f0e5ed55 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Authorization failed. The request you have made requires authentication. from 172.16.1.5
    2017-07-30 17:20:36.108 3278 INFO keystone.common.wsgi [req-4964e49e-5345-4baa-b74a-84198dc9dfb0 - - - - -] GET http://controller:5000/v3/
    2017-07-30 17:20:36.120 3277 INFO keystone.token.providers.fernet.utils [req-78b59c40-4bc8-4991-8fcb-f7b66c2513a7 - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:36.171 3277 INFO keystone.common.wsgi [req-78b59c40-4bc8-4991-8fcb-f7b66c2513a7 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] GET http://controller:5000/v3/users/63dee1ed626b4040bcd43b3492997a8c/projects
    2017-07-30 17:20:36.219 3278 INFO keystone.token.providers.fernet.utils [req-8487814f-ba80-4ecd-a691-f9d7185e11be - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:36.263 3278 INFO keystone.common.wsgi [req-8487814f-ba80-4ecd-a691-f9d7185e11be 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] POST http://controller:5000/v3/auth/tokens
    2017-07-30 17:20:36.277 3278 INFO keystone.token.providers.fernet.utils [req-8487814f-ba80-4ecd-a691-f9d7185e11be 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:36.388 3278 INFO keystone.token.providers.fernet.utils [req-8487814f-ba80-4ecd-a691-f9d7185e11be 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:37.038 3281 INFO keystone.token.providers.fernet.utils [req-409620fb-dd6b-493a-9b9f-259943f1564b - - - - -] Loaded 2 encryption keys (max_active_keys=3) from: /etc/keystone/fernet-keys/
    2017-07-30 17:20:37.098 3281 INFO keystone.common.wsgi [req-409620fb-dd6b-493a-9b9f-259943f1564b 63dee1ed626b4040bcd43b3492997a8c - - 19b6c35443a340c5a8648c97c46fdff7 -] GET http://controller:5000/v3/users/63dee1ed626b4040bcd43b3492997a8c/projects
    ```

0. We'll write a program that parses this data for us, and based on patterns we observed, displays for us how many failed attempts there have been to OpenStack.

0. The above keystone *keystone.common.wsgi* file can be downloaded by typing the following:

    `student@beachhead:~/bin$` `wget -O /home/student/bin/keystone.common.wsgi "goo.gl/uGeQ4n"`
    
0. You now have a local copy of *keystone.common.wsgi* (by the way, this is a production log snippet). Print it out to the screen.

    `student@beachhead:~/bin$` `cat keystone.common.wsgi`

0. Your job now is to write a program that goes line-by-line, searching on the expression **- - - - -] Authorization failed**. When the expression is encountered, a counter should be increased. At the conclusion of the program, the value of the counter (or the number of failed logins) should be displayed to the user.

0. Open leafpad text editor.

    `student@beachhead:~/bin$` `leafpad&`

0. The objective should be clear, so write this on your own, or follow along. We'll start with a Shebang line.

    ```
    #!/usr/bin/env python3
    ```
    
0. Next let's set a counter called **loginfail** and initialize it at **0**

    ```
    loginfail = 0
    ```

0. Great, next we want to create a file-object called, **keystone_file**, by using the open() function on the file we just downloaded, with read privledges (which is the tiny r at the end).

    ```
    keystone_file = open('/home/student/bin/keystone.common.wsgi','r')
    ```

0. Next we want to use **readlines()** which allows us to create a list called **keystone_file_lines** where each item in the list is a new line read from our file, **keystone_file**. Cool!

    ```
    keystone_file_lines=keystone_file.readlines()
    ```
    
0. Let's create a for loop. 

    ```
    for i in range(len(keystone_file_lines)):
    ```
    
0. Indent 4 white sapces, since we are inside of a for loop. If the pattern, **- - - - -] Authorization failed** is found within the list keystone_file_lines...

    ```
        if "- - - - -] Authorization failed" in keystone_file_lines[i]:
    ```
    
0. ... then increase the log in failure counter, **loginfail**. This is inside a for-loop, inside of an if statement, so indent a total of 8 whitespaces.

    ```
            loginfail += 1 # this is the same as loginfail = loginfail + 1
    ```

0. Finally, print the number of failed log in attempts to the screen, which is now stored in **loginfail**. You don't need to indent here.

    ```
    print('The number of failed log in attempts is ' + str(loginfail))
    ```

0. Your completed solution should look like the one below (minus the helpful comments)

    ```
    #!/usr/bin/env python3
    # parse keystone.common.wsgi and return number of failed login attempts
    loginfail = 0 
    keystone_file = open('/home/student/bin/keystone.common.wsgi','r')
    keystone_file_lines=keystone_file.readlines()
    for i in range(len(keystone_file_lines)):
        if "- - - - -] Authorization failed" in keystone_file_lines[i]:
            loginfail += 1 # this is the same as loginfail = loginfail + 1
    print('The number of failed log in attempts is ' + str(loginfail))
    ```

0. At the command line, try executing your code.

    ```
    student@beachhead:~/bin$` `cd ./keystone_counter.py`
    ```

0. **Bonus Objective 01:** If you are looking to push yourself a bit further, try also writing code to match on the number of successful logins. Display this information to the user.

0. **Bonus Objective 02:** The log also shows the IP address of the user that failed to login (at the very end of a filed login line). If you're looking to continue the challenge, write code that returns the IP address associated with a failed login.

0. The solution to this lab, along with solutions to the bonus objectives can be found here: [**SOLUTIONS**](https://alta3.com/python/18a-parseauth/)

0. That's it for this lab! If wrote it yourself, or if you *just* understand the solution, good job! Parsing log files is is a very real world problem, and you just wrote your own real world solution.
