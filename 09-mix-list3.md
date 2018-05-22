# Lists of Lists

### Lab Objective

The objective of this lab is to learn how to create, append, and extend lists. We'll practice doing this by making lists of network devices, but lists are essential parts of nearly every programming language. You can make lists of *anything*. 

This program should function in Python 3.x.

### Procedure

0. Clean up your remote desktop (close any previously open windows).

0. Open a terminal, make a new directory, and cd into the directory.

    `student@beachhead:/$` `mkdir /home/student/mycode/mix-list3`  

    `student@beachhead:/$` `cd /home/student/mycode/mix-list3`  

0. Open Leafpad text editor (press the green leaf in the lower left corner of the desktop).
        
0. Create a simple list of network devices and print them out using this code:

    ``` python
    #!/usr/bin/env python3
    list1 = ['cisco_nxos', 'arista_eos', 'cisco_ios']
    print(list1)
    ```

0. Choose **File > Save As** and save the file as, `/home/student/mycode/mix-list3/mix-list3.py`

0. Change permissions on your script so it can be executed.

    `student@beachhead:~/mycode/mix-list3$` `chmod u+x mix-list3.py`

0. In the terminal, try running your program.

    `student@beachhead:~/mycode/mix-list3$` `./mix-list3.py`

    ```
    ['cisco_nxos', 'arista_eos', 'cisco_ios']
    ```

0. Let's print out the second item in the list. Remember that in Python, the first number is always 0!

    ``` python
    #!/usr/bin/env python3
    list1 = ['cisco_nxos', 'arista_eos', 'cisco_ios']
    print(list1[1])
    ```

0. Choose **File > Save**

0. In the terminal, try running your program.

    `student@beachhead:~/mycode/mix-list3$` `./mix-list3.py`

    ```
    arista_eos
    ```

0. Let's add an item to our list.

    ``` python
    #!/usr/bin/env python3
    list1 = ['cisco_nxos', 'arista_eos', 'cisco_ios']
    print(list1)
    list1.extend(['juniper'])
    print(list1)
    ```

0. Choose **File > Save**

0. In the terminal, try running your program.

    `student@beachhead:~/mycode/mix-list3$` `./mix-list3.py`

    ```
    ['cisco_nxos', 'arista_eos', 'cisco_ios']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper']
    ```

0. Let's add a list within our list. 
    
    ``` python
    #!/usr/bin/env python3
    list1 = ['cisco_nxos', 'arista_eos', 'cisco_ios']
    print(list1)
    list1.extend(['juniper'])
    print(list1)
    list1.append(['10.1.0.1', '10.2.0.1', '10.3.0.1'])
    print(list1)
    ```

0. Choose **File > Save**

0. In the terminal, try running your program.

    `student@beachhead:~/mycode/mix-list3$` `./mix-list3.py`

    ```
    ['cisco_nxos', 'arista_eos', 'cisco_ios']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper', ['10.1.0.1', '10.2.0.1', '10.3.0.1']]
    ```

0. Return the value of item 5 in list1.

    ``` python
    #!/usr/bin/env python3
    list1 = ['cisco_nxos', 'arista_eos', 'cisco_ios']
    print(list1)
    list1.extend(['juniper'])
    print(list1)
    list1.append(['10.1.0.1', '10.2.0.1', '10.3.0.1'])
    print(list1)
    print(list1[4]) 
    ```
    
0. Choose **File > Save**

0. In the terminal, try running your program.

    `student@beachhead:~/mycode/mix-list3$` `./mix-list3.py`

    ```
    ['cisco_nxos', 'arista_eos', 'cisco_ios']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper', ['10.1.0.1', '10.2.0.1', '10.3.0.1']]
    ['10.1.0.1', '10.2.0.1', '10.3.0.1']
    ```
    
    - **Q1: Why are there three items here instead of one?**
        - A1: Because we placed a list INSIDE of a list! The append function adds 'something' at the end of a list. Since we specified a list with the square brackets [], we placed a list inside of another list! Cool.


0. Let's try printing the first IP address.

    ``` python
    #!/usr/bin/env python3
    list1 = ['cisco_nxos', 'arista_eos', 'cisco_ios']
    print(list1)
    list1.extend(['juniper'])
    print(list1)
    list1.append(['10.1.0.1', '10.2.0.1', '10.3.0.1'])
    print(list1)
    print(list1[4])
    print(list1[4][0])
    ```

0. Choose **File > Save**

0. Change permissions on your script so it can be executed.

    `student@beachhead:~/mycode/mix-list3$` `chmod u+x list_comp_even.py`  

0. In the terminal, try running your program.

    `student@beachhead:~/bin$` `./list_comp_even.py`

    ```
    ['cisco_nxos', 'arista_eos', 'cisco_ios']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper']
    ['cisco_nxos', 'arista_eos', 'cisco_ios', 'juniper', ['10.1.0.1', '10.2.0.1', '10.3.0.1']]
    ['10.1.0.1', '10.2.0.1', '10.3.0.1']
    10.1.0.1
    ```

0. **Challenge:** Create a list of at least five animals with three letter names and have your program output them to the screen. No need to be fancy, just load some animals in a list, and print them out, like the list below. If you get stuck, as the instructor for some help. When you finish. Save your code as `/home/student/mycode/mix-list3/animal-list.py`

    ```
    Fox Fly Ant Bee Cod Cat Dog Yak Cow Hen Koi Hog Jay Kit
    ```

0. Great job, that's it for this lab. Advance to the next lab, or feel free to play around with lists a bit more.
