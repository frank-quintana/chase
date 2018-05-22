# for loops and range()

### Lab Objective

The objective of this lab is to practice using the for loop and the range() function. In this lab, you'll build a simple calculator that performs factorial computations. When run, the user should be prompted for a number. The program should then perform a factorial on that value.

For example, if the user entered **Enter a factorial value (x!): 5**, the program would calculate (5 * 4 * 3 * 2 * 1) and then display **5! = 120** as the answer.

Try to use a for loop and the range() function to complete this lab. The range() function has two sets of parameters, as follows:

<pre>

range(stop)
  stop: Number of integers (whole numbers) to generate, starting from zero. eg. range(3) == [0, 1, 2].

range([start], stop[, step])
  start: Starting number of the sequence.
  stop: Generate numbers up to, but not including this number.
  step: Difference between each number in the sequence.

</pre>


This program should function in Python 3.x.


### Procedure

0. Clean up your remote desktop (close any previously open windows).

0. Open a terminal. Make a new direcotry.

    `student@beachhead:/$` `mkdir /home/student/mycode/fact/`

0. Change the directory to `/home/student/mycode/fact`

    `student@beachhead:/$` `cd /home/student/mycode/fact/`

0. Open leafpad text editor

    `student@beachhead:~/mycode/fact$` `leafpad&`

0. Save the empty text file to **/home/student/mycode/fact/** with the name **factorial.py**

0. Change permissions on your script so it can be executed.

    `student@beachhead:~/mycode/fact$` `chmod u+x factorial.py`
    
0. Okay great, we're ready to go. Like most of the labs, you can write it line-by-line along with the lab, or invent your own solution. If you write you own solution, be sure to comment it well, and let the instructor know.

0. Start by writing what you know. You know this program should run in Python 3.x, so let's start with a Python 3.x Shebang line.
    
    ```
    #!/usr/bin/env python3
    ```

0. Great. On the next line, we'll request that the user give us some input.
    
    ```
    x = int(input("Enter a number: "))
    ```
    
0. Before we add another layer of complexity to our program, we need to initialize a variable. Above the print line add this line of code:

    ```
    f= 1
    ```

0. Using a variable named `i`, create a `for` loop that iterates starting at 1, and going upto (but not inclusive of) the value input by the user, or **x**. To perform this kind of simple counting iteration (i.e. 1, 2, 3, 4, .... x), we can use the range() function. See the top of this lab for usage of range().

    ```
    for i in range(1, x):
    ```

0. At this point we need to indent 4 whitespaces, as we are 'inside' of the for loop. The code we want to write, is **f = f * i** hopefully the math makes sense. The first iteration, f = 1 * 1, the second time, f = 1 * 2, the third time, f = 2 * 3, and the fourth time, f = 6 * 4, and so on... this loop will continue until one-less than the number input by the user (**x-1**)

    ```
            f = f * i                 
    ```

0. Okay, we're done inside the loop, so no more indenting. Instead, lets print our results. 

    ```
    print(str(x) + '! = ' + str(f))                 
    ```

0. Choose **File > Save**

0. You can test your program as you develop it with the following command. 

    `student@beachhead:~/mycode/fact$` `./factorial.py`

0. Input the value of 4.

0. Huh. It didn't work as expected. 4! is actually 24, **not** 6.

0. Do you know why? If not, go back to the top of the lab, and read how the range function works. You'll find range(start,stop) goes upto (but not inclusive of) the stop position. Therefore, our code isn't executing the last (and final) time, which would give us a value of 24.

0. The fix is to change a single line in your code to read as follows:

    ```
    for i in range(1, x+1):  # the change here was to make x into x+1
    ```

0. Choose **File > Save**

0. Run your code again.

    `student@beachhead:~/mycode/fact$` `./factorial.py`
    
0. Make a few tests. Make sure that **6! = 720** and that **8! = 40320**.

0. Below is how your code should look (minus the helpful comments). If you couldn't write this program yourself, read through the comments and make sure you understand how it works.

    ```
    #!/usr/bin/env python3     
    x = int(input("Enter a number: ")) 
    f = 1                          
    for i in range(1, x+1):                                                                                
        f = f * i                 
    print(str(x) + '! = ' + str(f))
    ```

0. That's it for this lab.
