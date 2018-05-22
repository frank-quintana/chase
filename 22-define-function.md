# Lab 22 - Defining Functions
### WEDNESDAY - &#x2B50;REQUIRED&#x2B50;

### Lab Objective

The objective of this lab is to practice with functions. We will create a screen formatting program that will make function programming visual, so you can see on the screen how your functions work. When we are finished, we will have a tool that we can use for programs we will write later to make our program applealing to users who have no python skills.

Linux has a C library, called curses, which as the name implies, make you want to curse while you are trying to learn how to use it. So there is a good news, bads new story here. The good news is that curses lets you control the screen, the bad news is that the learning curse is severe. Then along comes a python program called "blessings" and suddenly, terminal management has been made availabe with an easier learning curve.

So, let's learn a little about both, and functions as we go.


### Procedure

0. Start a new terminal window (close any other you had open). Then change to the **~/bin** folder (our development directory).

    `student@beachhead:/$` `cd ~/bin`

0. To see a ***good*** example of terminal managment, issue this command.

    `student@beachhead:~/bin$` `htop`
    
0. Just leave htop run and adjust the width of your terminal. Notice how htop adapts to this?  It is iportant to understand that this is running in a command window, not a GUI. This means that htop can run when a lot of the other systems are crashed and the only way into a server is SSH.  Exit htop with **Ctl**+**c** (as in, press CTRL and C at the sametime), and lets learn how to write programs that can do this. We'll also use functions as a way to make our code reusable and easier to read.

    `press` `Ctrl` `and` `c` `at the same time to stop htop`

0. Notice that the screen has changed back to your linux prompt. When we take control of the screen, we must put it back the way we found it. It turns out that this is insanely hard to control the screen, and even harder to put it back. Fortunately, Eric Krose already solved that problem for us by writing a python wrapper around curses, so we will need to import some code and save a lot of time. 

0. Let's install pip3:

    `student@beachhead:~/bin$` `sudo apt install python3-pip`

0. Now install blessings. Blessings is code we don't have on our local system, that pip3 can go snag for us.

    `student@beachhead:~/bin$` `sudo pip3 install blessings`

    >Interestingly, blessings automatically runs 1to2.py, converting blessings from 2 to 3.

0. Oh, here's the thing. Nothing you were taught above was WRONG, but if you run Python 2.x AND Python 3.x, sometimes you end up with pip installing code to the wrong path. For example, it's highly possible pip3 installed blessings to the Python 2.x library because we ran that upgrade command. So, let's just run the line below, which forces Python 3.x to run the code called 'pip' as a module.

    `student@beachhead:~/bin$` `sudo python3 -m pip install blessings`

0. Run Leafpad.

    `student@beachhead:~/bin$` `leafpad&`

0. Add the these four lines of code and save to **/home/student/bin/pyfunc.py**

    ```
    #!/usr/bin/env python3
    from blessings import Terminal
    t = Terminal()
    print(t.clear())
    ```
    >1. The second line of code inputs a program that does all the hard work, interfacing with curses.  
    >2. The third line show off first-class function capability of python. When a function can be passed around just like a varible, we call those kinds of functions "first-class" functions. By assigning `t` to `Terminal()`, t can be used as shorthand notation for Terminal(). This saves you the misery, the agony, and sheer pain of typing Terminal() when you could just type t. (Tech humor, you should be smiling).  
    >3. The last line of code, uses a dot to connect the t function to a subfunction of terminal. Connecting functions together like this makes code very readable if the naming is chosen wisely. In this case, `t.clear` does cause us to think that this function will clear the terminal.
     
0. Let's make the code executable

    `student@beachhead:~/bin$` `chmod u+x pyfunc.py`
    
0. Now lets run the code. Hmmm, all it did was clear the screen? Lets add some more code so see what blessings can do.

    `student@beachhead:~/bin$` `./pyfunc.py`  

    >The blessings githgub has a great [**readme**](https://github.com/erikrose/blessings)  
    
0. Add this line to the bottom of your code and save:

    ```
    print(t.bold('Hi there!'))
    ```

0. Run the program and we see bold **Hi There**.

    `student@beachhead:~/bin$` `./pyfunc.py`    

0. Add these two lines to the bottom of your existing code and save:

    ```
    print(t.move_down)
    print(t.bold_red_on_bright_green('It hurts my eyes!'))
    ```
    > So we see that t.move_down moves the cursor down one line, and t.bold_red_on_bright_green is just funny.
    
0. Add this line to the bottom of your existing code and save

    ```
    print(t.move_down+t.bold_underline_black_on_yellow('Look! A 1997 web page! No, the font would have to be blinking'))
    ```
    >This line of code shows us that we can combine commands with underscores. This mashup idea came from another program that Eric Krose used and he liked the idea. It does produce a very compact way to write code.
    
0. Lets add three more lines of code to your existing codeblock. The full code so far should look like this:

    ``` python
    #!/usr/bin/env python3
    from blessings import Terminal
    t = Terminal()
    print(t.clear())
    print(t.bold('Hi there!'))
    print(t.move_down)
    print(t.bold_red_on_bright_green('It hurts my eyes!'))
    print(t.move_down+t.bold_underline_black_on_yellow('Look! A 1997 web page! No, the font would have to be blinking'))
    print(t.move_down)
    print(" Terminal width: ",t.width)
    print(" Terminal height: ",t.height)
    ```

    >OK, now we are getting serious! This is very handly. We can do math with the blessings module. If we know the hight and width of the terminal, we can adapt our output to fit the screen, dynamically if we wish to do so.
    
0. Can we pack all of that on one line? Sure can. Add this to your existing code

    ```
    print(t.move_down+"A one-liner way to show terminal width and height",t.reverse,t.width,"by",t.height," ")
    ```

0. So let's combine all those ideas we learned above by adding these lines of code to your codeblock.

    ```
    with t.location(20, t.height - 1):
       print(t.reverse + t.blink('This is at the bottom and printed in REVERSE.'))
    ```    
    > The `with` statement is doing math! Blessings accepts coordinates at (y,x), so this means the terminal will move the cursor 20 characters from the left of the screen, and then 1 line up from the bottom as calculated by the terminal height. Here is a handy way to make our screen responsive to the size of the terminal.
    
0. Here is the code we have so far:

    ``` python
    #!/usr/bin/env python3
    from blessings import Terminal
    t = Terminal()
    print(t.clear())
    print(t.bold('Hi there!'))
    print(t.move_down)
    print(t.bold_red_on_bright_green('It hurts my eyes!'))
    print(t.move_down+t.bold_underline_black_on_yellow('Look! A 1997 web page! No, the font would have to be blinking'))
    print(t.move_down)
    print(" Terminal width: ",t.width)
    print(" Terminal height: ",t.height)
    print(t.move_down+"A one-liner way to show terminal width and height",t.reverse,t.width,"by",t.height," ")
    with t.location(20, t.height - 1):
        print(t.reverse + t.blink('This is at the bottom and printed in REVERSE.'))
    ```

0. Whew! We now have our environment ready to experiment with functions in a very visable way.  Lets start over with all new code. Enter this code in your favorite editor and run it.

    ``` python
    #!/usr/bin/env python3
    from blessings import Terminal
    t = Terminal()
    print(t.clear())
    print(t.bold_black_on_yellow('Print yellow text'))
    ```
    >Nothing special, just a bold yellow line!
    
0. Lets write that code again, this time with our first function. It is easier to start fresh with a new code block, so run this code: 

    ``` python
    #!/usr/bin/env python3
    from blessings import Terminal
    t = Terminal()
    print(t.clear())
    
    def yellow_np():
       print(t.bold_black_on_yellow('Print yellow text'))
    ```

    > The **def** line defines a new function called `yellownp()`. The empty `()` at the end of yellownp means that no parameters are being passed into this function. The functioname yellownp was chosen to stand for yellow with no parameters.  
    > Wait, the screen cleared, but the yellow line did not print??? Let's take care of that.

0. Functions will not run unless you **call** them. There is nothing fancy about calling a function, just use its name a **def**ined in the `def` line.

    ``` python
    #!/usr/bin/env python3
    from blessings import Terminal
    t = Terminal()
    print(t.clear())
    
    def yellow_np():
       print(t.bold_black_on_yellow('Print yellow text'))
    
    yellow_np()
    ```

    >That worked!  
    >The name chosen for this function is in *snake_case*, which means lowercase words separated  with underscores to improve readability. As an alternative, *CamelCase* uses upper and lowercase characters (humps on a camel) to improve readabilty. It really does not matter, but if you want to conform to [PEP8](https://www.python.org/dev/peps/pep-0008/] your *python-coolness-factor* is higher when you use snake case.  
    >Finally, notice that a paramterless function is ***very*** easy to call, but always does exactly the same thing.  Its just not that flexible 
    
0. Let's add a function that accepts parameters. Lines 9, 10, and 14 are new. Add those lines to your existing code:

   ``` python
    1 #!/usr/bin/env python3
    2 from blessings import Terminal
    3 t = Terminal()
    4 print(t.clear())
    5 
    6 def yellow_np():
    7    print(t.bold_black_on_yellow('Print yellow text'))
    8 
    9 def yellow_wp(p):
   10    print(t.bold_black_on_yellow(p))
   11 
   12 yellow_np()
   13 
   14 yellow_wp("I want this to print in YELLOW")
   ```

    >So line 14 calls the code at line 9. This function expects a parameter, so we send it a string in line 14. The parameter is named `p` in line 9. At line 10, the value of `p` prints in yellow.  
    >Using parameters makes the function more flexible, but now calling the function is more complex because we must include the parameter in the function call.

0.  Now lets write a function that accepts a parameter and returns a value. We want a function that takes a string, and returns an integer value of the x coordinate that we could use to print the string in the top right corner so that the last character is completely right justified, that is, touching the right margin of the screen. The name chosen for this function needs to be very short for reasons that you have yet to see, so let's go with the name `trx`

    #### Lines 12,13,14,18,19 are new 
    ``` python
     1 #!/usr/bin/env python3
     2 from blessings import Terminal
     3 t = Terminal()
     4 print(t.clear())
     5 
     6 def yellow_np():
     7    print(t.bold_black_on_yellow('Print yellow text'))
     8 
     9 def yellow_wp(p):
    10    print(t.bold_black_on_yellow(p))
    11 
    12 def trx(p):
    13     x = t.width - len(p)
    14     return int(x)
    15 
    16 yellow_wp("I want this to print in YELLOW")
    17 
    18 x = trx("I want this to print in YELLOW")
    19 print(x)
    ```

0. Now let's use the `trx` function to help take control of the screen. We want to make that yellow line print in the top right corner of the screen. We know that the `trx( string)` will give us the x coordinate, and the y coordinate will simply be 0, so let's write the code.

    #### Line2 15 - 20 replace the previous lines 15-19. 
    ``` Python
     1 #!/usr/bin/env python3
     2 from blessings import Terminal
     3 t = Terminal()
     4 print(t.clear())
     5 
     6 def yellow_np():
     7    print(t.bold_black_on_yellow('Print yellow text'))
     8 
     9 def yellow_wp(p):
    10    print(t.bold_black_on_yellow(p))
    11 
    12 def trx(p):
    13     x = t.width - len(p)
    14     return int(x)
    15 
    16 def top_right(p):
    17     with t.location(trx(p), 0):
    18         yellow_wp(p)
    19 
    20 top_right("I want this in the top right!")
    ```
    >We notice that line 6 and 7 are really not going to be used, so we will drop those lines in the next step.

0. Now let's add a promp line in the bottom left of our screen. We will call it `prompt(p)` While we are at it, let's remove lines 5-7 above. Lines 17-22, 26 are new.

    ``` python
     1 #!/usr/bin/env python3
     2 from blessings import Terminal
     3 t = Terminal()
     4 print(t.clear())
     5 
     6 def yellow_wp(p):
     7    print(t.bold_black_on_yellow(p))
     8 
     9 def trx(p):
    10     x = t.width - len(p)
    11     return int(x)
    12 
    13 def top_right(p):
    14     with t.location(trx(p), 0):
    15         yellow_wp(p)
    16 
    17 def prompt(p):
    18     with t.location(0,t.height-2):
    19        print(t.reverse(p))
    20     with t.location(len(p) + 1,t.height-2):
    21        orders = input()
    22     return orders
    23 
    24 top_right("I want this in the top right!")
    25 
    26 orders = prompt("Prompt on bottom left:")
    ```

    > This is looking good, but lines 18 and 20 are not very readable.  This is perfect chance to show off first-class functions, yet keep our code from turning into spaghetti-code. 

0. Lines 17 - 21 are new. these are new first-class functions that just add a lot of readabilty to the code. Strong strucure is evident here. When you write code, try write it like this, thinking that the person who is going to fix this code in a few months just might be you. A few extra lines to keep it readable is worth the minor effort.

    ``` python
      1 #!/usr/bin/env python3
      2 from blessings import Terminal
      3 t = Terminal()
      4 print(t.clear())
      5 
      6 def yellow_wp(p):
      7    print(t.bold_black_on_yellow(p))
      8 
      9 def trx(p):
     10     x = t.width - len(p)
     11     return int(x)
     12 
     13 def top_right(p):
     14     with t.location(trx(p), 0):
     15         yellow_wp(p)
     16 
     17 def prompt_location():
     18     return t.location(0,t.height-2)
     19 
     20 def response_location(p):
     21     return t.location(len(p) + 1,t.height-2)
     22 
     23 def prompt(p):
     24     with prompt_location():
     25        print(t.reverse(p))
     26     with response_location(p):
     27        orders = input()
     28     return orders   
     29 
     30 top_right("I want this in the top right!")
     31 
     32 orders = prompt("Prompt on bottom left:")
    ```

    >We see no new behavior from this code, its just a lot prettier.
    
0. For our final change, before you take over, let's add a while loop that will take input, and allow to you enter an os command of `mtr 8.8.8.8` and still allow you to escape to exit.

    ``` python
    #!/usr/bin/env python3
    import os
    import sys 
    from blessings import Terminal
    t = Terminal()
    print(t.clear())

    def close(event):
        master.withdraw()
        sys.exit()

    def yellow_wp(p):
       print(t.clear_bol,t.bold_black_on_yellow(p))

    def trx(p):
        x = t.width - len(p)-1
        return int(x)

    def top_right(p):
        with t.location(trx(p), 0): 
            yellow_wp(p)

    def prompt_location():
        return t.location(0,t.height-2)

    def response_location(p):
        return t.location(len(p) + 1,t.height-2)

    def prompt(p):
        with prompt_location():
           p = p + " (q + Enter to quit):"
           print(t.reverse(p),t.clear_eol)
        with response_location(p):
           orders = input()
        return orders   

    orders = ""
    while orders != 'q': 
        orders = prompt("Your orders")
        orders
        top_right(orders)
        if orders[:3]=='mtr':
          with t.location(0,3):
             os.system(orders)

    yellow_wp("Thanks for all the fish")
    ```
    
0. Above is the program with a while loop that will keep you in the program until you type `q` then type the enter key. Run the program and at the prompt, enter `mrt 8.8.8.8`. Then q to quit mtr, plus `q` and the enter key to exit the pythoin program.

0. Now add additional code to this program that will accept `df` as input and print the results neatly in the center of the screen. Keep your code clean, readable and function-oriented. Also, feel free to make any modifications to this program.

0. Wow. That was quite a bit of coding. If you made it through that lab, good job! Click next to move on.
