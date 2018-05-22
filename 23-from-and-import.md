# Lab 23 - Working with from and import
### WEDNESDAY - &#x2B50;REQUIRED&#x2B50;

### Lab Objective

The objective of this lab is to practice using the **from** and **import** statements. We'll use these two commands to create a functional script that allows us to query on network interfaces. We'll also learn a bit about how Python deals with scope as we learn to import modules.

We recommend all students take a moment and reference the Python Software Foundation's Offical description of the *subprocess module*. Links to this module description can be found at the bottom of the lab, as well as here: https://docs.python.org/2/library/subprocess.html

You might also check out Sharat's practical guide to the subprocess module. You'll find this blog uses 'human talk' much more effectively than the Python Software Foundation's Offical description. 
http://sharats.me/the-ever-useful-and-neat-subprocess-module.html

In short, the *subprocess module* allows us to spawn new processes, connect to their input/output/error pipes, and obtain their return codes.

This lab is designed to walk you through the steps of writing this program, with the finished solution included at the bottom.

This program should run in Python 2.x.

### Procedure

0. Clean up your remote desktop (close any previously open windows).

0. Open the Leaf text editor.
    
0. Click **File > Save As...** (arrow 2 in the screenshot above).

0. Along the left side of the screen, click **student** (arrow 3 in the screenshot below).

    ![Leaf Text Editor2](https://alta3.com/static/images/python/python_from_import_002.png)
    
0. Now click on the **bin** folder (arrow 4 in screenshot above). *Note: If you don't have a /home/student/bin folder, it is because you missed a lab. Go ahead and create one with the Create Folder button (upper-right corner)*

0. Save the file as *subprocess.py* (see the screenshot below if you are lost).

    ![Leaf Text Editor3](https://alta3.com/static/images/python/python_from_import_003.png)

0. Now let's write this program, line-by-line, together. That way we can discuss what each line does as we build it. Start with a simple shebang line to call Python 2.x.

    `#!/usr/bin/env python`

0. Before you code the next line, consider again checking out the **subprocess module** description page at https://docs.python.org/2/library/subprocess.html focus your attention on the call function, and the check_output function. If your lost, the screenshot below points to the subprocess call function.

    ![subprocess Module](https://alta3.com/static/images/python/python_from_import_004.png)

0. So our next line of code, we want to **only import** the **call** and **check_output** functions **from the subprocess module**. By saying we *only* want to import the **call** and **check_ouput** functions, we're keeping our code as tiny as is possible. If this was our first time writing this code, and we weren't really sure what functions we might need, we might consider using the **import subprocess** command, however, we know that only need **call** and **check_output**. 

    `from subprocess import call`
    
0. The next function we use is **call**, this is made possible by our last line of code. This line of code will actually issue **ip link show up**, which reveals those interfaces that are currently in an up state.

    `call(["ip", "link", "show", "up"])`

0. Our next line is a simple print statement (we're writing this in Python 2.x so it is proper to say statement here).

    `print("interface:")`

0. Next, let's prompt the user for an interface they'd like more detail on.

    `interface = raw_input()`

0. Now let's use the input collected by the user to issue **ip addr show dev**, which will reveal IPv4 and IPv6 details.

    `call(["ip", "addr", "show", "dev", interface])`

0. Finally, let's use the input collected by the user to issue **ip route show dev**, which will reveal IPv4 and IPv6 details.

    `call(["ip", "route", "show", "dev", interface])`

0. If you're confident you've coded the program correctly, choose **File > Save**

0. Open a terminal, then change the directory to */home/student/bin*

    `student@beachhead:/$` `cd /home/student/bin`

0. Change permissions on your script so it can be executed.

    `student@beachhead:~/bin$` `chmod u+x subprocess.py`

0. You can test your program with the following command. 

    `student@beachhead:~/bin$` `./subprocess.py`

0. Provided that you called your file, **subprocess.py**, then this failed. The solution is to change then name, delete **subprocess.pyc** and you will be ok. Our **import** statement is trying to import from our file, and not the module.

    `student@beachhead:~/bin$` `mv subprocess.py intscript.py`

0. If you tried to run **intscript.py**, it still won't work. That's because our script has been compiled, and still points to that script, **subprocess.pyc**. Test that assertion.

    `student@beachhead:~/bin$` `./intscript.py`

0. It still didn't work. Let's delete **subprocess.pyc**, then try again.

    `student@beachhead:~/bin$` `rm subprocess.pyc`

0. Execute your script again. This time it *should* work.

    `student@beachhead:~/bin$` `./intscript.py`

0. **ONLY IF YOUR CODE FAILS** - Try to fix the error. The completed solution is shown below:

    ```
    #!/usr/bin/env python
    from subprocess import call
    call(["ip", "link", "show", "up"])
    print("interface:")
    interface = raw_input()
    call(["ip", "addr", "show", "dev", interface])
    call(["ip", "route", "show", "dev", interface])
    ```

0. If your code executes, enter **ens3** when prompted for user-input.
    
0. This should return information about the interface **ens3**. Your output should look simillar to the output below.

    ![subprocess Output](https://alta3.com/static/images/python/python_from_import_005.png)

0. Go back to the Leaf Text Editor, and replace your code block with the code shown below:

    ```
    #!/usr/bin/env python
    import subprocess
    subprocess.call(["ip", "link", "show", "up"])
    print("interface:")
    interface = raw_input()
    subprocess.call(["ip", "addr", "show", "dev", interface])
    subprocess.call(["ip", "route", "show", "dev", interface])
    ```

0. Notice that we're now being a bit messier with our **import** statement. Rather than saying which functions we need from the subprcoess module, we're just importing the entire module. The result is that the **call** fucntion now needs to be referenced as **subprocess.call**

0. If you're confident you've coded the program correctly, choose **File > Save As...**

0. Make sure you title your program, **intscript02.py** then **click Save**

0. Back at the terminal, change permissions on your script so it can be executed.

    `student@beachhead:~/bin$` `chmod u+x intscript02.py`

0. Execute intscript02.py

    `student@beachhead:~/bin$` `./intscript02.py`
    
0. Huh. It failed. The problem is that we still have a script laying around called **subprocess.py** within our current directory. Now that our script lacks the qualifers of **call** and **check_output**, the **import** statement is finding a *match* on our local script. Let's get rid of the problem, and promise **always to use unique names when naming our custom Python scripts**.

    `student@beachhead:~/bin$` `rm subprocess.*`

0. Once again, execute **intscript02.py**. It should work the same way as **intscript.py**

    `student@beachhead:~/bin$` `./intscript02.py`

0. You're encouraged to augment this script, or write one of your own using the subprocess module. If you write something clever, make sure it is well documented, and let the instructor know. We'll give you credit and include it with this lab. The following is a list of things you might try to build:

    - A script that issues **ip** commands we learned about on Day 1
    - Further explore the **subprocess** module
        - Explore check_output
        - Explore Popen
    - Make this script functional in Python 3.x to do this, you'll need to learn about the **run** function
        - Python 3.x subprocess documentation: https://docs.python.org/3.6/library/subprocess.html#subprocess.check_output
    - Write a script that asks a user which interface they'd like to run Wireshark on
        - Wireshark documentation: https://www.wireshark.org/docs/wsug_html_chunked/ChCustCommandLine.html

0. Great job! That's it for this lab.

#### Additional Learning / References

The following is a list of pages we thought might be helpful for our students to know about:

* [The Ever Useful and Neat subprocess Module](http://sharats.me/the-ever-useful-and-neat-subprocess-module.html)

* [Python.org's Subprocess Module Documentation](https://docs.python.org/2/library/subprocess.html)
