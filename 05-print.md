# Starting to Use Print()

### Lab Objective

The objective of this lab is to begin to experiment with Python I/O functionalities, i.e. input/output. This ensures the interaction or communication with other components, e.g. a database or a user. Input often comes - as we have already seen - from the keyboard and the corresponding Python command, or better, the corresponding Python function for reading from the standard input with input(). 

In previous examples we saw that you can write into the standard output by using print. It's worth taking a detailed look at the print function. In Python 3.x, it's important to emphasize "print function" and not "print statement." You can easily find out how crucial this difference is if you take an arbitrary Python program written in version 2.x and if you try to let it run with a Python3 interpreter. In most cases you will receive error messages. One of the most frequently occurring errors will be related to print, because most programs contain print statements.

### Procedure

0. Open a new command window.

0. Let's get in the habit of organizing our work. For now, make `/home/student/mycode/` directory. We'll delete it at the end of this lab.

0. Move to the `/home/student/` directory with `cd`

    `student@beachhead:/$` `cd`

0. Make the new directory `/home/student/mycode`

    `student@beachhead:~$` `mkdir /home/student/mycode/`

0. Move to the `/home/student/mycode` directory.

    `student@beachhead:/$` `cd /home/student/mycode`

0. Great. When you write your own code, always make a directory to work in. It pays to stay organized. Enter the Python 2.x environment.

    `student@beachhead:~/mycode$` `python`

0. Write the following into the interpreter.
    
    `>>>` `print 42`

0. This should cause *42* to be output to the screen, as shown below:

    ![python-print](https://alta3.com/static/images/python/python_print_input_output_001.png)

0. Exit the Python 2.x environment.

    `>>>` `exit()`

0. Enter the Pyhton 3x environment.

    `student@beachhead:~/mycode$` `python3`

0. Write the following into the interpreter. It should cause an error.
    
    `>>>` `print 42`

0. You should see an error like the one below:

    ![python-error](https://alta3.com/static/images/python/python_print_input_output_002.png)
 
0. This is a familiar error message for most Python coders; we have forgotten the parentheses. The "print" function is, as we have already mentioned, a function in version 3.x (it was a statement in 2.x). Like any other function print expects its arguments to be surrounded by parentheses. So parentheses are an easy remedy for this error. Issue the following command to 'fix' this error.

    `>>>` `print(42)`

0. But this is not the only difference to the old print. The output behaviour has changed as well. Review the following usage of the **print()** function.

    - print(value1, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

0. The print function can print an arbitrary number of values (value1, value2, ...) which are separated by commas. These values are separated by blanks. Try playing around with this concept by issuing the command below.

    `>>>` `print('192.168.30.5', 'A1:02:CC:04', '255.255.255.0')`

0. Now let's look at two examples of print calls. We'll try printing two values in both cases, i.e. a string and a float number. First set *a* to the value of a float:

    `>>>` `a=3.14`
    
0. Now try printing a string and a float value:

    `>>>` `print('a = ', a)`
    
0. The result should be *a = 3.14* (your screen should look like the one below).

    ![python-set-var](https://alta3.com/static/images/python/python_print_input_output_003.png)

0. Issue the same command, but this time include a *\n* (new line) statement.
    
    `>>>` `print('a = \n', a)`

0. Your result should be the same as the one shown below:

    ![python-set-var-carriage-return](https://alta3.com/static/images/python/python_print_input_output_004.png)

0. We can learn from the second print of the example that a blank between two values, i.e. "a = \n" and "3.14", is always printed, even if the output is continued in the following line. This is different to Python 2, as there will be no blank printed, if a new line has been started. It's possible to redefine the seperator between values by assigning an arbitrary string to the keyword parameter "sep", e.g. an empty string or a smiley. Execute the following series of steps to demonstrate this concept. First print the character *a* and the character *b*.

    `>>>` `print('a','b')`

    ![python-a-b](https://alta3.com/static/images/python/python_print_input_output_005.png)

0. Use the *sep* parameter to declare that the values should be seperated by no white spaces.

    `>>>` `print('a','b',sep='')`

    ![python-sep](https://alta3.com/static/images/python/python_print_input_output_006.png)

0. Use the *sep* parameter to declare that the values should be separated by a dot.

    `>>>` `print(192,168,178,42,sep='.')`
    
    ![python-sep-dot](https://alta3.com/static/images/python/python_print_input_output_007.png)

0. Use the *sep* parameter to declare that the values should be separated by a dot.

    `>>>` `print('a','b',sep=':)')`

    ![python-smile](https://alta3.com/static/images/python/python_print_input_output_008.png)
    
0. Use the *end* parameter to declare how that line should end. By default, all print functions end with a new-line, or \n.

    `>>>` `print('I am thinking',end='...\n\n\n')`

0. Still not clear? Try something else...

    `>>>` `print('I am thinking',end='!!!')`
    
0. Notice what happened because we didn't include syntax for new-line.

0. Leave the Python interpreter.

    `>>>` `exit()`

0. Let's practice deleting a directory. First move back to `/home/student/`

    `student@beachhead:~/mycode$` `cd`

0. Now delete `/home/student/mycode/`. The `-r` below will delete the directory if it also contains files.

    `student@beachhead:~$` `rm -r /home/student/mycode/`

0. Good job. Run this lab again if anything wasn't clear.
