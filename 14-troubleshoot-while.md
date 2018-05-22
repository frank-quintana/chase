# Troubleshooting while, if, elif, else

### Lab Objective

The objective of lab is to troubleshoot the following code block. The program, which prompts the users for two operators, as well as an operation to perform on the operators, doesn't work! Your job is to figure out why. Be sure to note the patterns in errors that emerge for common problems.

This program was written for OLD Python 2.x, so by the end of this lab, you must have it running in Python 3.x!

### Procedure

0. Take a moment to clean up your Remote Desktop. For now, close all other terminal spaces or windows you might have open.

0. Open a new terminal, then create a new directory to work in.

    `student@beachhead:/$` `mkdir /home/student/mycode/broken01/`

0. Move into the new directory. 

    `student@beachhead:/$` `cd /home/student/mycode/broken01/`

0. Launch the leafpad editor.

    `student@beachhead:~/mycode/broken01$` `leafpad`

0. Copy and paste the following code into the file.

    ``` python
    #!/usr/bin/env python
    # A program that prompts a user for two operators and and operation (plus or minus)
    # the program then shows the result.
    # The user may enter 'q' to exit the program.
    calc1 = 0.0
    calc2 = 0.0
    operation = ''
    while (calc1 != 'q')
        print('\nWhat is the first operator? Or, enter q to quit: ')
        calc1 = raw_input()
        if calc1 == 'Q':
            break
        calc1 = float(calc1)
        print('\nWhat is the second operator? Or, enter q to quit: ')
        calc2 = raw_input()
        if calc2 == 'q':
            break
        calc2 = float(calc2)
        print('Enter an operation to perform on the two operators (+ or -): ')
        operation = raw_input()
        if operation == '+':
            print(\n + str(calc1) + ' + ' + str(calc2) + ' = ' + str(calc1 + calc2))
        ifel operation == '-':
            print('\n' + str(calc1) + ' - ' + str(calc2) + ' = ' + str(calc1 - calc2))
        else:
            print('\n Not a valid entry. Restarting...')
    ```  

0. Choose **File > Save As**

0. Save the file as, `/home/student/mycode/broken01/ifixed.py`

0. Change permissions on your script so the user (student) has permissions to execute (x) the script.

    `student@beachhead:~/mycode/broken01$` `chmod u+x ifixed.py`

0. Run *ifixed.py* with the Python interpreter, it will **not** run.

    `student@beachhead:~/mycode/broken01$` `./ifixed.py`

0. It is always useful to note the specific error that occured. It can be helpful in revealing where the bug may lie.  On which line number did the error occur?

0. Go back to the Leaf text editor, and study `ifixed.py`. Your goal is to try to fix the broken code. When you think you've fixed the code, save your changes, and once again run the Python interpreter. This may take several trial and error cycles. The answer is shown below, but do your best to figure it out yourself before hopping to the solution.

0. If you're struggling and need a hint...

    - Check out *line 8*... something is missing

        `while (calc1 != 'q')`

    - It won't cause an run-time error, but there is a design flaw in *line 11*

        `if calc1 == 'Q':`

    - Something is incorrect within *line 22* 

        `print(\n + str(calc1) + ' + ' + str(calc2) + ' = ' + str(calc1 + calc2))`

    - The last error is in *line 23*

0. Once the code runs, there is one final task to try. Rather than run this program in a Python 2 interpreter, let's run it in a Python 3 interpreter. Again, it won't work, and you'll need to fix it. Even with the Python 2.x shebang line, the command below will force the use of Python 3. **This will not work**.

    `student@beachhead:~/mycode/broken01$` `python3 broken.py`

0. So your first hint, is to start by updating the shebang line to python3.

0. If you're struggling and need another hint... there's a problem with *line 10*, *line 15*, and *line 20*

0. Once your program runs in Python3, save it. Then commit to git, and push to github.

0. Great job! That's it for this lab.
