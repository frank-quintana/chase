# SOLUTIONS - Troubleshooting while, if, elif, else
These are solutions to the previous lab. Click next if you're looking for the next lab challenge.

### Q01 SOLUTION 01

Below is the fixed calc script

``` python
#!/usr/bin/env python
# A program that prompts a user for two terms and operation (plus or minus)
# the program then shows the result.
# The user may enter 'q' to exit the program.
calc1 = 0.0
calc2 = 0.0
operation = ''
while (calc1 != 'q'):    # Missing loop colon
    print('\nWhat is the first term (enter a number)? Or, enter q to quit: ')
    calc1 = raw_input()
    if calc1 == 'q':    # Should be a lower case 'q'
        break
    calc1 = float(calc1)
    print('\nWhat is the second term (enter a number)? Or, enter q to quit: ')
    calc2 = raw_input()
    if calc2 == 'q':
        break
    calc2 = float(calc2)
    print('Enter an operator to perform on the two terms (+ or -): ')
    operation = raw_input()
    if operation == '+':
        print('\n' + str(calc1) + ' + ' + str(calc2) + ' = ' + str(calc1 + calc2)) # new line should be in single quotes
    elif operation == '-':      # this is an elif statement, not an ifel statement
        print('\n' + str(calc1) + ' - ' + str(calc2) + ' = ' + str(calc1 - calc2))
    else:
        print('\n Not a valid entry. Restarting...')
```


### Q02 SOLUTION 01 - RUNS IN PYTHON3

This is the Python3 Version. All that needed updated was changing the *raw_input()* function to the *input()* function.

``` python
#!/usr/bin/env python
# A program that prompts a user for two operators and and operation (plus or minus)
# the program then shows the result.
# The user may enter 'q' to exit the program.
calc1 = 0.0
calc2 = 0.0
operation = ''
while (calc1 != 'q'):  
    print('\nWhat is the first operator? Or, enter q to quit: ')
    calc1 = input()   # Change from raw_input() to input()
    if calc1 == 'q': 
        break
    calc1 = float(calc1)
    print('\nWhat is the second operator? Or, enter q to quit: ')
    calc2 = input()   # Change from raw_input() to input()
    if calc2 == 'q':
        break
    calc2 = float(calc2)
    print('Enter an operation to perform on the two operators (+ or -): ')
    operation = input()    # Change from raw_input() to input()
    if operation == '+':
        print('\n' + str(calc1) + ' + ' + str(calc2) + ' = ' + str(calc1 + calc2)) # new line should be in single quotes
    elif operation == '-':      # this is an elif statement, not an ifel statement
        print('\n' + str(calc1) + ' - ' + str(calc2) + ' = ' + str(calc1 - calc2))
    else:
        print('\n Not a valid entry. Restarting...')
```
