# Testing if conditionals

### Lab Objective

The objective of this lab is to write a program to collect data from a user, and test `if` that data matches an expected value.

### Procedure

0. Open a new command window.

0. Move to the `/home/student/mycode/` directory. Note, the directory `/home/student/mycode/` should be version controlled repo within GitHub. If you missed this step, you should perform the git / github lab.

    `student@beachhead:/$` `cd /home/student/mycode/`

0. Make a new directory in which we can store this project, `/home/student/mycode/if-test/`

    `student@beachhead:~/mycode$` `mkdir /home/student/mycode/if-test/`
    
0. Move into the new directory

    `student@beachhead:~/mycode$` `cd if-test`
    
0. Great! Let's write a python script.

    `student@beachhead:~/mycode/if-test$` `leafpad`

0. Leafpad should open. Begin with a shebang line that will let your program run in Python 3. Copy the line below into the top line of your program.

    `#!/usr/bin/env python3`

0. Now create a string `hostname` that we can test against.

    `hostname = 'MTG'`

0. Now write some test logic with the `if` statement.

    `if hostname == 'MTG':`
    
0. Now what to do if this statement is found to be true, *don't forget to indent*!

    `    print('The hostname was found to be mtg')`
    
0. Save your program as, `/home/student/if-test/if-hostname.py`, and exit.

0. Run your program.

    `student@beachhead:~/mycode/if-test$` `python3 if-hostname.py`
    
0. If your program was written correctly, `The hostname was found to be mtg` should appear on the screen. If it does not, debug your program. This may take many, many, iterations. But it will work. Welcome to programming.

0. One your program is working, make the following changes.

0. **1st Code Customization Request** -  Make `hostname` defined by collecting input from the user. Include a nice question asking for a hostname.

0. **2nd Code Customization Request** - Ensure that the user can enter `mtg`, `MTG`, `mTg`, `MTg`, `mTG`, or `MtG`, and they would all test true.

0. **3rd Code Customization Request** - If `hostname` test true, print a second line that says `hostname matches expected config` to the screen.

0. **4th Code Customization Request** - Before the program ends, always print `Exiting the script.`

0. **5th Code Customization Request** - Comment all the code

0. When you've completed a working script, be sure to save it, then commit to git and push to github.

0. That's it for this lab.
