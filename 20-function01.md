# Conjunction Junction Junction... Python Functions

### Lab Objective

The objective of this lab is to explore the creation of Python functions. Functions allow you to eliminate reduncy and quickly perform repetitious tasks. The reuse of code also allows for shorter programs in that a functions can be called indefinately.

This program should function in Python 3.x.

### Procedure

0. Clean up your remote desktop (close any previously open windows).

0. Open a terminal, make a new directory, and cd into the directory.

    `student@beachhead:/$` `mkdir /home/student/mycode/netfunct01`  

    `student@beachhead:/$` `cd /home/student/mycode/netfunct01`

0. Open Leafpad text editor (press the green leaf in the lower left corner of the desktop).
        
0. Let's assume you need to do something across a bunch of switches-- 20 or 30.  In this example, we'll create a function that mimics pushing commands to a file.

0. Copy and paste the following code into leafpad:

        #!/usr/bin/env python3
        
        # function to push commands
        def commandpush(devicecmd): # devicecmd==list 
            for coffeetime in devicecmd.keys():
                print('Handshaking. .. ... connecting with ' + coffeetime )
                # we'll learn to write code that connects to devices here
                for mycmds in devicecmd[coffeetime]:
                    print('Attempting to sending command --> ' + mycmds )
                    # we'll learn to write code that sends cmds to device here
        
        ### Start our script
        work2do = {"10.1.0.1":["interface eth1/2", "no shut"], "10.2.0.1":["interface eth1/1", "shutdown"], "10.3.0.1":["interface eth1/5", "no shutdown"]} # data that replaces data stored in file
        
        print("Welcome to the network device command pusher") # welcome message
        
        ## get data set
        print("\nData set found\n") # replace with function call that reads in data from file
        
        ## run 
        commandpush(work2do) # call function to push commands to devices

0. Save the script as, `/home/student/mycode/netfunct01/main.py`

0. Change permissions on the file.

    `student@beachhead:~/mycode/netfunct01$` `chmod u+x main.py`
    
0. Run the code.

    `student@beachhead:~/mycode/netfunct01$` `python3 main.py`
    
0. It *should* work. If it doesn't. Figure out why.

0. **1st CUSTOMIZATION REQUEST:** The output is sloppy. Issue a new line after **all** of the commands finish issuing for a device (or before 'Handshaking' begins). Comment the code more fully.

0. When you're finished save your code.

0. If you're tracking your code in github, issue the following commands:
    - `cd ~/mycode`
    - `git add *`
    - `git commit -m "your commit message"`
    - `git push origin master`
