# IPv4 Testing with if

### Lab Objective
The objective of this lab is to build skills using the `if` statement. In order to write useful programs, we almost always need the ability to check conditions and change the behavior of the program accordingly. Conditional statements give us this ability. The simplest form is the if statement, which has the genaral form:

        if BOOLEAN EXPRESSION:
            STATEMENTS
        A few important things to note about if statements:

The colon (:) is significant and required. It separates the header of the compound statement from the body. The line after the colon must be indented. It is standard in Python to use four spaces for indenting. All lines indented the same amount after the colon will be executed whenever the BOOLEAN_EXPRESSION is true.

### Procedure

0. Open a new command window.

0. Move to the `/home/student/mycode/` directory. Note, the directory `/home/student/mycode/` should be version controlled repo within GitHub. If you missed this step, you should perform the git / github lab.

    `student@beachhead:/$` `cd /home/student/mycode/`

0. Make a new directory in which we can store this project, `/home/student/mycode/if-test2/`

    `student@beachhead:~/mycode$` `mkdir /home/student/mycode/if-test2/`
    
0. Move into the new directory

    `student@beachhead:~/mycode$` `cd if-test2`
    
0. Great! Let's write a python script.

    `student@beachhead:~/mycode/if-test2$` `leafpad`

0. Copy the following text into leafpad.

        #!/usr/bin/env python3
        ipchk = '192.168.0.1'
        
        if ipchk:
           print('Looks like the IP address was set: ' + ipchk)

0. Save as, `/home/student/mycode/if-test2/iftest2.py`

0. Change permissions on the script.

    `student@beachhead:~/mycode/if-test2$` `chmod u+x iftest2.py`

0. Run the script.

    `student@beachhead:~/mycode/if-test2$` `./iftest2.py`
    
0. As `ipchk` has a value, it tests as true, so the print statement should run. Let's tweak the program again. Open your script.

    `student@beachhead:~/mycode/if-test2$` `leafpad iftest2.py`

0. Alter your script so that `ipchk` is set by the user.

        #!/usr/bin/env python3
        ipchk = input('Apply an IP address: ') # this line now prompts the user for input
        
        if ipchk: # if any data is provided, this will test true
           print('Looks like the IP address was set: ' + ipchk) # indented under if

0. Save your script and exit leafpad.

0. Run the script `iftest2.py`

    `student@beachhead:~/mycode/if-test2$` `./iftest2.py`

0. Try running the script a few times. If you hit **ENTER** (and don't input data), the if statement will test false (and fails to print anything to the screen). You'll also notice that any value causes our script to test true, not *just* IP addresses.

0. Edit the script again.

    `student@beachhead:~/mycode/if-test2$` `leafpad iftest2.py`
    
0. Let's add an **else** statement to our script. If a user fails to input data, let's print an indication to the screen that says as much.

        #!/usr/bin/env python3
        ipchk = input('Apply an IP address: ') # this line now prompts the user for input
        
        if ipchk: # if any data is provided, this will test true
           print('Looks like the IP address was set: ' + ipchk) # indented under if
        else: # if data is NOT provided
           print('You did not provide input.') # indented under else
           
0. You know the drill. Save and exit.

0. Run the script twice. The first time *just hit **ENTER*** (do not provide any data). This will cause the if statement to test false, and the program should tell us as much. The second time, provide an IP address (or any data, we still haven't fixed this problem).

    `student@beachhead:~/mycode/if-test2$` `./iftest2.py`

0. Validity checking an IP address is totally possible, but a bit more tricky. We'll save that for a later lab. For now, let's return a warning if a user tries to apply the IP address of our gateway `192.168.70.1`. Edit the script.

    `student@beachhead:~/mycode/if-test2$` `leafpad iftest2.py`

0. Replace or edit your script with the one below. Notice that we added an **elif** line. Our script now reads, "If ipchk is 192.168.70.1 then print(), else-if ipchk has any value then print(), else ipchk has no value then print()".

        #!/usr/bin/env python3
        ipchk = input('Apply an IP address: ') # this line now prompts the user for input
        
        if ipchk == '192.168.70.1': # if a match on '192.168.70.1'
           print('Looks like the IP address of the Gateway was set: ' + ipchk + ' This is not recommended.') # indented under if
        elif ipchk: # if any data is provided, this will test true
           print('Looks like the IP address was set: ' + ipchk) # indented under if
        else: # if data is NOT provided
           print('You did not provide input.') # indented under else

0. Save and exit one last time.

0. Run the script a bunch of times. The first time, enter `192.168.70.1`, and our program should return a warning. The second time, enter any other IP address, and the elif logic should print, finally run the program and just press **ENTER**. The script should still know that you didn't provide any input.

    `student@beachhead:~/mycode/if-test2$` `./iftest2.py`

0. We'll spend time on checking / validating IP addresses, but possible solutions include *regex* and *error handling*, both topics we haven't yet studied. Here's one possible solution: https://stackoverflow.com/questions/319279/how-to-validate-ip-address-in-python no big deal if it isn't clear, yet. Promise it will be soon.

0. That's it for this lab.
