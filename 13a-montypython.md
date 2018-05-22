# SOLUTIONS - Using while, if, elif, else (Monty Python)
These are solutions to the previous lab. Click next if you're looking for the next lab challenge.


### Q01 SOLUTION 01


``` python
#!/usr/bin/env python
round = 0           # integer round initiated to 0
while(True):        # sets up an infinite loop condition
  print('Finish the movie title, "Monty Python\'s The Life of ______"')
  answer = raw_input()    # string answer collected from user
  round = round + 1     # increase the round counter
  if (answer == 'Brian'): # logic to check if user gave correct answer
    print('Correct!')
    break             # break statement escapes the while loop
  elif (round == 3):    # logic to ensure round has not yet reached 3
    print('Sorry, the answer was Brian.')
    break             # break statement escapes the while loop
  else:                 # if answer was wrong, and round is not yet equal to 3
    print('Sorry. Try again!')
```


### BONUS Q01 - SOLUTION 01 - ACCEPTS Brian AND brian

``` python
#!/usr/bin/env python
round = 0           # integer round initiated to 0
while(True):        # sets up an infinite loop condition
  print('Finish the movie title, "Monty Python\'s The Life of ______"')
  answer = raw_input()    # string answer collected from user
  round = round + 1     # increase the round counter
  if (answer == 'Brian') or (answer =='brian'): # logic to check if user gave correct answer both 'Brian' and 'brian' match
    print('Correct!')
    break             # break statement escapes the while loop
  elif (round == 3):    # logic to ensure round has not yet reached 3
    print('Sorry, the answer was Brian.')
    break             # break statement escapes the while loop
  else:                 # if answer was wrong, and round is not yet equal to 3
    print('Sorry. Try again!')
```


### BONUS Q02 - SOLUTION 01 - ACCEPTS ANY AND ALL CASES

> Below is a second solution that changes the string answer into having **only** lower case characters. This change is performed with a basic Python tool, called the lower() function. We haven't studied it in detail yet, but it does offer another solution to the bonus problem.

``` python
#!/usr/bin/env python
round = 0           # integer round initiated to 0
while(True):        # sets up an infinite loop condition
  print('Finish the movie title, "Monty Python\'s The Life of ______"')
  answer = raw_input()    # string answer collected from user
  answer = answer.lower() # We haven't learned this yet, but the lower function makes all the characters lowercase
  round = round + 1     # increase the round counter
  if (answer == 'brian'): # logic to check if user gave correct answer 'brian' -- all lowercase is all we need to match on.
    print('Correct!')
    break             # break statement escapes the while loop
  elif (round == 3):    # logic to ensure round has not yet reached 3
    print('Sorry, the answer was Brian.')
    break             # break statement escapes the while loop
  else:                 # if answer was wrong, and round is not yet equal to 3
    print('Sorry. Try again!')
```


### BONUS Q03 - SOLUTION 01 - PYTHON3 FRIENDLY

This version attaches a function to the quiz question and then curly brackets "{}" are used to insert a value into the string. This technique looks a lot like jinja templates used with ansible, so give this a try. Change the line of code:  
`blank = input("{} Finish the movie title, Monty Python's The Life of ".format(x+1))` until you see how it works. Another technique this version uses is "falling through", which means when the task at hand completes, the logic just "falls through" to the additional steps which in this case is the "Sorry" message.

``` 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-       #Directive to forgive presence of UTF-8 characters
print("You have 3 tries!")
for x in range(0, 3):         #We have three tries so just interate three times
  blank = input("{} Finish the movie title, Monty Python's The Life of ".format(x+1))
           # The {} indicates where <.format(x+1)> will insert its value
  if (blank.lower() == 'brian'):  # make the answer case insensitive
    print("Correct!")
    quit()
  elif (x<2):
    print("Sorry. Try again")
print('Sorry, the answer was Brian.')
```
