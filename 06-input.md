# Collecting Input()

### Lab Objective

The objective of this lab is to begin to experiment with how we can collect data from the user for use within our programs.

### Procedure

0. Open a new command window. Make sure `/home/student/mycode/` exists. We'll use this a bunch.

    `student@beachhead:/$` `mkdir /home/student/mycode/`

0. Move to the `/home/student/mycode/` directory.

    `student@beachhead:/$` `cd /home/student/mycode/`

0. Make a new directory in which we can store this project, `/home/student/mycode/lab_input/`

    `student@beachhead:~/mycode$` `mkdir /home/student/mycode/lab_input/`
    
0. Move into the new directory

    `student@beachhead:~/mycode$` `cd lab_input`
    
0. Great! Let's write a python script.

    `student@beachhead:~/mycode/lab_input$` `leafpad`

0. Leafpad should open. Begin with a shebang line that will let your program run in Python 3. Copy the line below into the top line of your program.

    `#!/usr/bin/env python3`
    
0. Now we want to use the input() function to collect data from our user. Make this your second line.

    `user_input = input('Please enter an IPv4 IP address:')`

0. The input from the user is stored as the value `user_input`. Let's use the print() function to return that value to our user. Make the following code your 3rd line.

    `print("You told me the IPv4 address is:" + user_input)`
    
0. The finished program should look like the following:

    ```
    #!/usr/bin/env python3
    user_input = input('Please enter an IPv4 IP address:')
    print("You told me the IPv4 address is:" + user_input)
    ```
    
0. Select: **File > Save as** and save this file as `input_ip.py`

0. Exit leafpad.

0. We can always run our program using the python3 command.

    `student@beachhead:~/mycode/lab_input$` `python3 input_ip.py`

0. The program should run. If it did not, then once again open your script in leafpad, and find the bug. You'll do lots and lots of debugging when you code. So it's no big deal.

0. Once you have working program, comment it up. You don't have to use the comments below, it's just an example. Always be specific.

    ```
    #!/usr/bin/env python3
    # Written By: Alice Anderson
    # Collect IP address information from a user, and display it back to them.
    
    ## Collect input from the user
    user_input = input('Please enter an IPv4 IP address:') # user_input stores the IPv4 ip address
    
    ## Print out the input collected from the user
    print("You told me the IPv4 address is:" + user_input)
    ```

0. Once you have some comments in your code, add two more lines of code at the bottom of your script. They should be written as follows:
  - The first line should collect and store input from the user. Ask the user for the 'vendor name' associated with the device. Use any variable name you would like.
  - Use a second line of code to print the input you just collected from the user. 

0. Save your code, exit leafpad, and confirm it runs. If you have any problems, talk to the instructor.

    `student@beachhead:~/mycode/lab_input$` `python3 input_ip.py`

0. Given your code runs, open it once again. Now add comments to the additional working lines.

0. Save your finalized script.

0. Congrats! That is it for this lab.
