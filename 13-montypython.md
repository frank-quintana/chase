# Using while, if, elif, else (Monty Python)

### Lab Objective

The objective of lab is to write a simple guessing program that asks the question, "Finish the movie title, 'Monty Python's The Life of \_\_\_\_\_", then collects up to 3 responses from the user. Responses to the user's input should include: "Correct!", "Sorry, try again!", and "Sorry, the answer was Brian."

To complete this task, use a while loop with embedded if, elif, and else statements. When executed, the program should operate like the one shown below:

**EXAMPLE**

![Solution 1](https://alta3.com/static/images/python/python_while_if_elif_else_001.png)

Write this program to run under Python 2.x.

### Procedure

0. Take a moment to clean up your Remote Desktop. For now, close all other terminal spaces or windows you might have open.

0. Open a new terminal, then *cd ~/bin* to the */home/student/bin* directory

    `student@beachhead:/$` `cd ~/bin`
    
0. Launch the leafpad editor. By including the **&** sign, we won't 'lockup' our terminal (try it without, and you'll see the terminal becomes unavailable for use until leafpad is closed).

    `student@beachhead:~/bin$` `leafpad&`

0. Save the file as *monty_python2.py* (see the screenshot below).

    ![Leaf Text Editor2](https://alta3.com/static/images/python/python_while_if_elif_else_004.png)

0. So, at this time we have a decision to make. You can write this program on your own, or follow along and write it line-by-line.

0. Always start by coding what you know needs to be coded. The first line should be a shebang line that calls Python 2.x.

    ```
    #!/usr/bin/env python
    ```

0. Great. We're going to use a counter to control our while loop in this lab, so let's start our counter (which we'll call **round**, equal to 0).

    ```
    round = 0
    ```

0. The defining control structure of this program is a loop.  Make a simple while loop that will loop forever (or until we execute a `break`).

    ```
    while(True):
    ```

0. If the user is going to get 3 tries, then some kind of counter variable should be initialized to zero, and increased at the top of the while loop. We're also inside of a while statement, so we need to indent (with the spacebar NOT with the tab key), four space. We could actually use any amount of space, as long as we are consistant, but four whitespaces what we'll learn here (it's also what Google uses).

    ```
        round = round + 1
    ```

0. We're still inside of the while-loops, so indent four spaces. Then get the question on the screen with *print()*.

    ```
        print('Finish the movie title, "Monty Python\'s The Life of ______"')
    ```

0. After you print the question to the screen, a variable should accept the user's input with *raw_input()* *Note: **raw_input()** is unique to Python 2.x, in 3.x, you need to shorten **raw_input()** to **input()**.

    ```
        answer = raw_input()
    ```

0. The user just gave us input, so now we need to check to see if that input is valid. Write an if statement to see if it is valid. Again, still inside of a for-loop, so you still want to indent 4 white spaces.

    ```
        if (answer == 'Brian'):
    ```

0. Now, not only are we inside of a while-loop, but we're also inside of an if statement. Therefore, we want to indent eight-white spaces (4 whitespaces for the while loop + 4 white spaces for the if statment). Remember, spacebar, **not** tab. We went to write code on what to if the user typed the right answer, *Brian*.

    ```
            print('Correct')
    ```

0. Next, we want to esacpe the loop that we are stuck in. In Python, this can be done with the **break** statement. You want to indent 8 spaces here.

    ```
            break
    ```

0. Great. That would be the end of this **if** statement. Next, let's write some logic to throw in an **elif** statement. This is short for, 'else-if', as in, "our condition wasn't matched in the if statement, but it maybe matched in this statement". Here, we want to code what is going to happen if we hit the third round, and the user didn't type 'Brian'. We're still in the while-loop, so indent four spaces.


    ```
        else:
    ```

0. Our last line of code is just our, "Sorry! Try again!", message. Be sure to indent 8 whitepspaces.

    ```
            print('Sorry! Try again!')
    ```
    
0. If you followed along, your working program should look like the following:

    ```
    #!/usr/bin/env python3
    round = 0           # integer round initiated to 0
    while(True):        # sets up an infinite loop condition
        round = round + 1     # increase the round counter
        print('Finish the movie title, "Monty Python\'s The Life of ______"')
        answer = input()    # string answer collected from user
        if (answer == 'Brian'): # logic to check if user gave correct answer
            print('Correct!')
            break             # break statement escapes the while loop
        elif (round == 3):    # logic to ensure round has not yet reached 3
            print('Sorry, the answer was Brian.')
            break             # break statement escapes the while loop
        else:                 # if answer was wrong, and round is not yet equal to 3
            print('Sorry. Try again!')
     ```

0. When you think you have a working program, choose **File > Save** and save as, `/home/student/mycode/monty_python2.py`

0. Before we can execute our program, we should change our program's permission set so it is executable. You'll always need to do this before executing newly written code for the first time. Back in the terminal, type the following:

    `student@beachhead:~/bin$` `chmod u+x monty_python2.py`
    
0. You can test your program with the following command. 

    `student@beachhead:~/bin$` `./monty_python2.py`

0. If your code fails, try to fix the error. Try not to make more than a single change before running the program again. It's difficult to troubleshoot if you make more than one tweak at a time.

    > If your code fails, it is likely due to an indentation error. Remove all TABs from your code. Indent 4 whitespaces (spacebars) inside the while loop, and a total of 8 whitespaces (spacebars) inside an if, elif, or else statement while inside of the while-loop. All of your code needs to be in-line.

0. Complete the *code customization requests* below:

0. **1st Code Customization Request** - Try to write the logic that would allow a user to provide a lowercase or uppercase answer, and still get credit. You should be able to come up with this solution based on what you've already learned.

0. **2nd Code Customization Request** - If the user provides the string `shrubbery` as input, the script should print to the screen, `You gave the super secret answer!`, and exit.

0. When the code customizations are complete, save your working script.

0. The link below shows the current solution, along with the bonus solutions. If you write a different/better solution, comment it up, and let the instructor know. We'll add it to the solutions page.

    [SOLUTIONS](../13a-montypython)
