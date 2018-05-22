# The Shebang Line

### Lab Objective

The objective of this lab is to teach students proper usage of the shebang line. Maybe you've seen the shebang before. Shebang is a Linux feature that can take various forms, depending on the scripting language, but its purpose is to declare the script's ability to be run like an standalone executable. For example, a Python program without a shebang line requires you to preface the script with the word *python*. Scripts with a properly declared shebang line do not require this. 

Remember, the **\*.py** extension is a recommendation. Not a MUST have. Therefore, we should always include a shebang line, as it allows others to immediately identify the type of script (code) they're looking at.

Unless you are using cygwin, Windows has no shebang support. However, Windows can auto-associate the **\*.py** file extension with Python. This allows Windows users to double-click on a Python script in Windows Explorer and see the script execute through Python.

### Procedure

0. Open a new terminal.

0. Move to your home directory.

    `student@beachhead:/$` `cd`

0. To observe best practice, all of our scripts *should* be in a directory called **mycode** in our home directory. Let's create mycode now.

    `student@beachhead:~$` `mkdir /home/student/mycode`

0. Let's move all of our python scripts to the new directory. If you don't have any yet, that's okay. The follow command won't hurt.

    `student@beachhead:~$` `mv *.py /home/student/mycode`

0. Change to the new directory.

    `student@beachhead:~$` `cd mycode`

0. Make sure all of your files have moved to the new directory. The follow command should display all files and their details.

    `student@beachhead:~/mycode$` `ls -ll`
    
0. Use the text editor Leaf (or Vim) to create a file called `no_shebang.py`

    `student@beachhead:~/mycode$` `leafpad&`

0. Copy the following code into the file.

    ```
    print('\'ello!')
    print('Did you say, hello?')
    print('No, I said ello, but that\'s close enough.')
    ```

0. Choose **File > Save As** and save the file in **/home/student/mycode** as **no_shebang.py**

0. Back in the terminal, issue the following command and look at the file **no_shebang.py**. Notice it is not executable.

    `student@beachhead:~/mycode$` `ls -ll`

0. From now on, we should get in the habit of making our script executable. Issue the following command and look at `no_shebang.py`, you should notice that it is *not* executable.
    
0. Let's change that. Issue the chmod command to change the permissions for this file.

    `student@beachhead:~/mycode$` `chmod u+x no_shebang.py`

0. Make sure the script is now executable.

    `student@beachhead:~/mycode$` `ls -ll`

0. Try issuing just the name of the file. **This should fail.**

    `student@beachhead:~/mycode$` `./no_shebang.py`

0. Now try with the name *python*. **This should work.**

    `student@beachhead:~/mycode$` `python no_shebang.py`
    
0. Now try with the name *python3*. **This should work.**

    `student@beachhead:~/mycode$` `python3 no_shebang.py`

0. Linux doesn't care that the file ends in **\*.py**. Nothing about this file is instructing it to run Python code, which is why we have to be explicit.

0. Go back to leafpad, and select **File > Save As**, and save the file as, **shebang_python2.py**

0. Edit the code to include the following line at the top of your code, **#!/usr/bin/env python** this line of code is called a 'shebang' line. As in, a Python 2.x shebang line will default your code to run in Python 2.x

    ```
    #!/usr/bin/env python
    print('ello!')
    print('Did you say, hello?')
    print('No, I said ello, but that\'s close enough.')
    ```

0. Now, when we run this script, the bash shell will read the first line, and hand the program over to Python2.x.

0. Choose **File > Save**

0. Don't forget to make the file executable with **chmod u+x shebang_python2.py**

    `student@beachhead:~/mycode$` `chmod u+x shebang_python2.py`

0. Try issuing **just** the name of the file prefaced by only a **./**. **This should work.**

    `student@beachhead:~/mycode$` `./shebang_python2.py`

0. Now try with the name *python*. **This should also work.**

    `student@beachhead:~/mycode$` `python shebang_python2.py`

0. Now try with the name *python3*. **This should also work.** With this command, we are actually overriding the shebang line, and forcing the script to be run as a Python3.x program (not python2.x). This works because the bash shell will never parse *shebang_python2.py*. Instead, we're handing it straight to *python3* for execution.

    `student@beachhead:~/mycode$` `python3 shebang_python2.py`

0. Go back to leafpad editor, and chose **File > Save As**, name the file **shebang_python3.py**, and save it in **/home/student/mycode** 

0. Tweak your Python2 shebang line to become a Python3 shebang line.

    ```
    #!/usr/bin/env python3
    print('ello!')
    print('Did you say, hello?')
    print('No, I said ello, but that\'s close enough.')
    ```

0. Choose **File > Save**

0. Our new shebang line will no instruct the Linux shell to run this code using Python3.x

0. Make the file executable

    `student@beachhead:~/mycode$` `chmod u+x shebang_python3.py`

0. Try issuing just the name of the file prefaced by only a **./**. **This should work.**

    `student@beachhead:~/mycode$` `./shebang_python3.py`

0. Now try with the name *python*. **This should also work.** However, understand we are forcing the program to run with Python 2.x, and not Python 3.x (as the shebang line has declared).

    `student@beachhead:~/mycode$` `python shebang_python3.py`

0. Now try with the name *python3*. **This should also work.** Note that we are overriding the shebang line, and forcing the script to be run as a Python3.x program (which it would have done anyway because of the shebang line)

    `student@beachhead:~/mycode$` `python3 shebang_python2.py`
    
0. One last example to drive the Shebang line home. In Leafpad, choose **File > New**, then copy and paste the following code into a new leafpad document.

    ```
    #!/bin/rm
    # This script calls on the executable called **rm** (remove), which is
    # installed to the */bin* directory. When this script is run,
    # bash will execute this script with *rm*, which will remove (delete)
    # the script :p
    ```

0. Choose **File > Save As** and save as, **deleteme.py** in **/home/student/mycode**

0. Down in the terminal, make the file executable.

    `student@beachhead:~/mycode$` `chmod u+x deleteme.py`

0. Ensure the script exsits.

    `student@beachhead:~/mycode$` `ls deleteme.py`

0. Run the script.

    `student@beachhead:~/mycode$` `./deleteme.py`

0. Confirm the script no longer exists.

    `student@beachhead:~/mycode$` `ls deleteme.py`
    
0. What you just learned is that the shebang line can be used to call any program. Here we called the **rm** program, which is *remove* (as in delete). **PRETTY COOL!**

0. Not every one of the classroom examples includes the shebang line (space is usually a concern within PowerPoint), but you shouldn't be intimidated by encountering this line, or adding it to your own scripts! Also, do your best to save your scripts to /home/student/mycode (it's good practice).

0. Good job! That's it for this lab.


#### Additional Learning / References

The following are a list of pages we thought might be helpful for our students to know about:

* [#!/usr/bin/env python" vs "#!/usr/local/bin/python](https://mail.python.org/pipermail/tutor/2007-June/054816.html)

* [Using the Dot-Slash in Commands](http://www.linfo.org/dot_slash.html)
