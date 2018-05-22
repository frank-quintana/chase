# Migrating 2.x to 3.x Py

### Lab Objective

The objective of lab is to get some practice migrating code from 2.x to 3.x. While the future of Python is version 3.x, lots of legacy 2.x code still exists. It is important to understand the differences between the two.

Several code blocks are given below. Copy each one, execute it with the Python3 interpreter, and watch it fail. Subsequent steps will reveal a solution on fixing the code. 

### Procedure

0. Clean up your remote desktop (close any previously open windows).

0. Open the leafpad text editor

0. Click **File > Save As** 

0. Save the file as, */home/student/mycode/my2to3.py* 

0. Great, now copy and paste the following code block into your open leaf text editor. This is **code written for Python 2.x**, however, we're going to **run it in a Python 3.x** environment to watch it break.

    ```
    #!/usr/bin/env python3
    round = 0           # integer round initiated to 0
    while(True):        # sets up an infinite loop condition
        print('What is the IPv4 address used to broadcast on a local network? ')
        answer = raw_input()    # string answer collected from user
        round = round + 1     # increase the round counter
        if (answer == '255.255.255.255'): # logic to check if user gave correct answer
            print('Correct!')
            break             # break statement escapes the while loop
        elif (round == 3):    # logic to ensure round has not yet reached 3
            print('Sorry, the answer was 255.255.255.255')
            break             # break statement escapes the while loop
        else:                 # if answer was wrong, and round is not yet equal to 3
            print('Sorry. Try again!')
    ```
  
0. In the Leaf Text editor, choose **File > Save**

0. Open a terminal, then change the directory to `/home/student/mycode`

    `student@beachhead:/$` `cd /home/student/mycode`

0. Change permissions on your script so it can be executed.

    `student@beachhead:~/mycode$` `chmod u+x my2to3.py`

0. You can test your program with the following command. 

    `student@beachhead:~/mycode$` `./my2to3.py`

0. So that didn't work. Study the error. Go back to Leaf Text Editor, make a 'fix', save, and execute your code again. Try not to make more than a single change before running the program again. It's difficult to troubleshoot if you make more than one tweak at a time.

0. The solution is shown below, so don't go any further until you've figured out the solution, or are *really* stuck.

0. In Python 3.x, the raw_input() function was replaced with the input() function. Therefore, replace all instances of **raw_input()** with **input()**, then save your work.

0. Once again, run the code within the Python 3.x interpreter.

    `student@beachhead:~/bin$` `./guess2to3.py`

0. Good job! That's it for this lab.
