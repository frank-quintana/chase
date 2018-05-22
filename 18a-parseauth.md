# SOLUTION - Parse a Log File for Failed Auth Attempts
These are solutions to the previous lab. Click next if you're looking for the next lab challenge.

### Q01 SOLUTION 01
This is the solution for the original problem with in the lab.

<pre>
#!/usr/bin/env python3
# parse keystone.common.wsgi and return number of failed login attempts
loginfail = 0 
keystone_file = open('/home/student/bin/keystone.common.wsgi','r')
keystone_file_lines=keystone_file.readlines()
for i in range(len(keystone_file_lines)):
    if "- - - - -] Authorization failed" in keystone_file_lines[i]:
        loginfail += 1
print('The number of failed log in attempts is ' + str(loginfail))
</pre>


### BONUS QUESTION 01 SOLUTION 01
Below should be the code that displays successful logins. It hasn't been created yet, so write it, and get your code posted here!

<pre>
#!/usr/bin/env python3

import re

loginfail = 0
loginsucc = 0


keystone_file = open('/home/student/bin/keystone.common.wsgi','r')
keystone_file_lines=keystone_file.readlines()

for i in range(len(keystone_file_lines)):
    if "- - - - -] Authorization failed" in keystone_file_lines[i]:
        loginfail += 1
        print("Failed Login Attempt Detected")
        bad_ip = re.search(r'\d+\.\d+\.\d+\.\d+',keystone_file_lines[i])
        print ("The offending IP address is - " + bad_ip.group() + "\n")
        continue

    if "-] Authorization failed" in keystone_file_lines[i]:
        loginsucc += 1
        print("Successful Login Attempt Detected")
        good_ip = re.search(r'\d+\.\d+\.\d+\.\d+',keystone_file_lines[i])
        print ("Login from IP address - " + good_ip.group() + "\n")

#Calculate login failure rate and print results summary
loginfailrate = 100* loginfail/(loginfail+loginsucc)
print("The number of failed login attempts is: ",loginfail)
print("The number of successful login attempts is: ",loginsucc)
print("The login failure rate is: " + str(loginfailrate) + "%")
</pre>

### BONUS QUESTION 02 SOLUTION 01 - Solved Oct 6, 2017
Below should be the code that returns the IP address associated with a failed login. Thanks, Carlos!

<pre>
#Carlos Tapia
#This program expands on lab 26, it finds the ip addresses of failed authentications
#and displays them
#!/usr/bin/env python3
import re

loginfail = 0
keystone_file = open('/home/student/bin/keystone.common.wsgi','r')
keystone_file_lines=keystone_file.readlines()
list = []	#Create an empty list, this will be used to send the ips found
for i in range(len(keystone_file_lines)):
	if "- - - - -] Authorization failed" in keystone_file_lines[i]:
		loginfail += 1 # this is the same as loginfail = loginfail + 1
		ip = re.search(r'\d+\.\d+\.\d+\.\d+', keystone_file_lines[i]) #RegEx expression to match an ip address
		list.append(ip.group()) #append the ip address to the list
print('The number of failed log in attempts is ' + str(loginfail))
print('The ip(s) that failed are:\n ')
for i in range(len(list)):	#loop based on the length of the list to print each ip address
	print((list[i]))
</pre>
