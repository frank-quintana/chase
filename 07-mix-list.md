# Data within mixed lists

### Lab Objective

The objective of this lab is to begin to experiment with lists, and dealing with mixed types of data.

### Procedure

0. Open a new command window.

0. Move to the `/home/student/mycode/` directory. Note, the directory `/home/student/mycode/` should be version controlled repo within GitHub. If you missed this step, you should perform the git / github lab.

    `student@beachhead:/$` `cd /home/student/mycode/`

0. Make a new directory in which we can store this project, `/home/student/mycode/mix-list/`

    `student@beachhead:~/mycode$` `mkdir /home/student/mycode/mix-list/`
    
0. Move into the new directory

    `student@beachhead:~/mycode$` `cd mix_list`
    
0. Great! Let's write a python script.

    `student@beachhead:~/mycode/mix-list$` `leafpad`

0. Leafpad should open. Begin with a shebang line that will let your program run in Python 3. Copy the line below into the top line of your program.

    `#!/usr/bin/env python3`

0. Now create a list. In practice, we might read data in from an excel file, or an email. Lists are made with square brackets. The line below should be your second line.

    `my_list = [ "192.168.0.5", 5060, "UP" ]`

0. Now let's write a line of code to return the IP address from out list. Notice that the first item in the list, is indexed as zero. Lists always begin at zero!

    `print("The first item in the list (IP): " + my_list[0] )`

0. Now return the port 5060. The problem is 5060 is an integer, not a string. Strings are surrounded with quotes. We may not combine unlike data types within Python. So, below is the solution. We can force an integer value to be a string with the str() function.

    `print("The second item in the list (port): " + str(my_list[1]) )`

0. Now return the last item in the list, this is a string value "UP".

    `print("The last item in the list (state): " + my_list[2] )`

0. Put comments on the code you've written so far. Use previous lab examples as a guide. Remember, comments begin with `#` and always be descriptive

0. Select: **File > Save as** and save this file as `mix_list.py`

0. Exit leafpad.

0. We can always run our program using the python3 command.

    `student@beachhead:~/mycode/mix_list$` `python3 mix_list.py`

0. The program should run. If it did not, then once again open your script in leafpad, and find the bug.

0. Once your program is printing your list to the screen, open it again, and add the following line at the bottom of your code:

    `new_list = [ 5060, "80", 55, "10.0.0.1", "10.20.30.1", "ssh" ]`

0. Your job is to use a single print() function to display all of the data from your list. You can be creative how you return the data, but make the output something like this:

    `When I ssh into IP addresses 10.0.0.1 or 10.20.30.1 I am unable to ping ports 5060, 80, or 55.`
    
0. Once you've written code you think might work, save your code, exit leafpad, and confirm it runs. If you have any problems, talk to the instructor.

    `student@beachhead:~/mycode/lab_input$` `python3 mix_list.py`

0. Given your code runs, open it once again. Now add comments to the additional working lines.

0. Save your finalized script.
