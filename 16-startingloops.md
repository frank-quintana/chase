# Starting to Learn Loops

### Lab Objective

The objective of this lab is to introduce the concept of looping.

## Lab Procedure

0. Open a new command window.

0. Move to the */home/student/* directory.

    `student@beachhead:/$` `cd /home/student/`

0. Enter the Python 3.x environment.

    `student@beachhead:~$` `python3`

0. Remember that the print function is ended by a newline. We'll see that in this example. We'll also learn that the *range* fuction generates a list of integers from zero up to (but not including) the declared integer. Try out both of these ideas.

    `>>>` `for i in range(4):`

0. You'll notice the interpreter has changed from *>>>* to *...*, this indicates you are within a loop. Now issue the print function- however, **be sure to indent four white spaces**.

    `...`     `print(i)`

0. To indicate this is the end of the loop, simply press **ENTER**.

    `...` 

    ![python-loop1](https://alta3.com/static/images/python/python_print_input_output_009.png)

0. If we wanted to change how printed output 'ends' (other than a new line), we can assign an arbitrary string to the keyword parameter "end." This string will be used for ending the output of the values of a print call:

    `>>>` `for x in range(10):`

0. Once again, indent four spaces:

    `...`     `print(x, end=' ')`

0. Press **ENTER** to exit the loop.

    `...` 

    ![python-endline](https://alta3.com/static/images/python/python_print_input_output_010.png)


0. Try this again, but change the end parameter.

    `>>>` `for z in range(10):`
    
0. Indent 4 spaces before you issue the *print* statement.

    `...`     `print(z, end=' :) ')`

0. Press **ENTER** to exit the loop.

    `...`
    
    ![python-loopsmile](https://alta3.com/static/images/python/python_print_input_output_011.png)
    
0. The output of the print function is send to the standard output stream (sys.stdout) by default. By redefining the keyword parameter "file" we can send the output into a different stream e.g. sys.stderr or a file. Let's send output to a file called *ourfile.txt*, where *w* is for *write*.

    `>>>` `foo = open("ourfile.txt","w")`

0. Now write data to the file *ourfile.txt*. Note we're not using the file parameter to place data in *foo* (in the last line we defined *foo* as *ourfile.txt*).

    `>>>` `print('You must construct additional pylons.', file=foo)`
    
0. Close the file so the print statement in the buffer is written out to the file.

    `>>>` `foo.close()`

0. We can see that we don't get any output in the interactive shell. The output is sent to the file "ourfile.txt." Let's check on that now. Quit the Python3 interactive shell.

    `>>>` `exit()`

0. Display the contents of the file *ourfile.txt*.

    `student@beachhead:~$` `cat ourfile.txt`
    
0. Remove *ourfile.txt*

    `student@beachhead:~$` `rm ourfile.txt`
    
0. Check out the history of your Python session.

    `student@beachhead:~$` `cat .python_history`

0. If you're brand new to Python, you might want to redo this lab at some point. Loops are basic skills you want to get proficent with. However, that's the end of this lab. Good job!
