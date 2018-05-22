# Looping with for

### Lab Objective
We want to become adept at working with for looping. We'll try to use some network-centric ideas while studying these concepts in Python3.

### Procedure

0. Take a moment to clean up your Remote Desktop. For now, close all other terminal spaces or windows you might have open.

0. Open a new terminal, then create a new directory to work in.

    `student@beachhead:/$` `mkdir /home/student/mycode/introfor/`

0. Move into the new directory. 

    `student@beachhead:/$` `cd /home/student/mycode/introfor/`

0. Launch the leafpad editor.

    `student@beachhead:~/mycode/introfor$` `leafpad &`

0. Copy and paste the following code into the file.

    ``` python
    #!/usr/bin/env python3
    vendors = ['cisco', 'juniper', 'big_ip', 'f5', 'arista']
    for x in vendors:
        print("The vendor is:" + vendors)
    print("\nOur loop has ended.")
    ```  

0. Choose **File > Save As** and save the file as, `/home/student/mycode/introfor/forloop1.py`

0. Change permissions on your script so the user (student) has permissions to execute (x) the script.

    `student@beachhead:~/mycode/introfor$` `chmod u+x forloop1.py`

0. Run *forloop1.py*.

    `student@beachhead:~/mycode/introfor$` `./forloop1.py`
    
0. We can even put logic inside of loops. Neat! Launch the leafpad editor.

    `student@beachhead:~/mycode/introfor$` `leafpad &`

0. Copy and paste the following code into the file. Notice, we now have a loop, and if logic inside of the loop.

    ``` python
    #!/usr/bin/env python3
    vendors = ['cisco', 'juniper', 'big_ip', 'f5', 'arista', 'alta3', 'zach', 'stuart']
    approved_vendors = ['cisco', 'juniper', big_ip']
    for x in vendors:
        print("\nThe vendors is " + x, end="")
        if x not in approved_vendors:
            print(" - NOT AN APPROVED VENDOR!", end="")
    print("\nOur loop has ended.")
    ``` 
    
0. Choose **File > Save As** and save the file as, `/home/student/mycode/introfor/forloop2.py`

0. Change permissions on your script so the user (student) has permissions to execute (x) the script.

    `student@beachhead:~/mycode/introfor$` `chmod u+x forloop2.py`

0. Run *forloop2.py*.

    `student@beachhead:~/mycode/introfor$` `./forloop2.py`
