# Running Netmiko

### Lab Objective
The objective of this lab is to introduce Netmiko, and learn how to write a network automation script. Netmiko is an Python Toolkit that enables simplied SSH access to common network devices. It is in very common use across the Python for Network Automation landscape. So, we certainly want to learn how to use it. In addition, we also want to learn how to write original code, to augment a program we already have running.

Before you get started, make sure you have:
- Working GNS3 environment
- At least one Arista switch that can be contaced via SSH
- A \*.xls for a source of input. Download one in this lab, or run a previous lab that teached how to use pyexcel to create your own

In this lab, we'll roleplay that we are a company that deploys additional networking gear to support major sports events that we sponsor, such as the USOpen (a major Golf tournament late August through September). 

### Procedure

0. Ensure GNS3 is running. The configuration is upto you, but this lab assumes the Arista topology shown below:

    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_eth_cable_look_like_this.png)

0. Open a new terminal window, then make a directory for us to work in.

    `student@beachhead:/$` `mkdir /home/student/mycode/usopen`

0. Move into our new directory.

    `student@beachhead:/$` `cd /home/student/mycode/usopen`

0. We need an \*.xls file to work with. You may have performed a previous lab that taught you how to use Python to create an \*.xls file. If you did, you should run it run it now. Give it the IP addresses `172.16.2.10` and `172.16.2.20`, input the drivers as `arista_eos`. Name the resulting file `ip_list`.

0. **IF YOU DID NOT DO THE EXCEL PYTHON LAB** then go back and do it, **or** issue the command below to download a completed XLS file.

    `student@beachhead:~/mycode/usopen$` `wget https://alta3.com/static/images/python/ip_list.xls -O ip_list.xls`

0. You may already have this installed, but this program depends on having `pyexcel`, `pyexcel-xls`, and `netmiko` installed with pip.

    `student@beachhead:~/mycode/usopen$` `python3 -m pip install pyexcel pyexcel-xls netmiko`

0. Open leafpad.

0. Turn on Line Numbers via **Options > Line Numbers**

0. Copy and paste the following code into leafpad.

        #!/usr/bin/env python3
        ## USOpen Tournament Switch Checker -- 2018.05.01
        ''' usopen.py
        This script is being designed to provide the following automated tasks:
        - ping check the router (import os)
        - login check the router (import netmiko)
        - determine if interfaces in use are up (import netmiko)
        - Apply new configuration (import netmiko) # not yet built
        
        The IPs and device type should be made available via an Excel spreadsheet
        
        '''
        import os
        
        ## pyexcel and pyexce-xls are required for our program to execute
        # python3 -m pip install --user pyexcel
        # python3 -m pip install --user pyexcel-xls
        import pyexcel
        
        # python3 -m pip install --user netmiko
        from netmiko import ConnectHandler
        
        ## retrieve data set from excel
        def retv_excel(par):
            d = {}
            records = pyexcel.iget_records(file_name=par) # create a record object that is an open excel sheet
            for record in records:
                d.update( { record['IP'] : record['driver'] } ) # adds a new IP and driver key:value pair to our dictionary
            return d # return the completed dictionary
        
        ## Ping router - returns True or False
        def ping_router(hostname):
        
            response = os.system("ping -c 1 " + hostname)
            
            #and then check the response...
            if response == 0:
                return True
            else:
                return False


        ## Check interfaces - Issue "show ip init brief"
        def interface_check(dev_type, dev_ip, dev_un, dev_pw):
            try:
                open_connection = ConnectHandler(device_type=dev_type, ip=dev_ip, username=dev_un, password=dev_pw)
                my_command = open_connection.send_command("show ip int brief")       
            except:
                my_command = "** ISSUING COMMAND FAILED **"
            finally:
                return my_command


        ## Login to router - SSH Check with Netmiko class ConnectHandler
        def login_router(dev_type, dev_ip, dev_un, dev_pw):
            try:
                open_connection = ConnectHandler(device_type=dev_type, ip=dev_ip, username=dev_un, password=dev_pw)
                return True        
            except:
                return False

        ## Main function - This is the code that runs our functions
        def main():
        
            ## Determine where *.xls input is
            file_location = str(input("\nWhere is the file location? "))
        
            ## Entry is now a local dictionary containing IP(key):driver(value)
            entry = retv_excel(file_location)
        
            ## Use a loop to check each device for SSH accessability
            print("\n***** BEGIN SSH CHECKING *****")
            for x in entry.keys():
                if login_router(str(entry[x]), x, "admin", "alta3"):
                    print("\n\t**IP: - " + x + " - SSH connectivty UP\n")
                else:
                    print("\n\t**IP: - " + x + " - SSH connectivty DOWN\n")
               
            ## Use a loop to check each device for ICMP responses
            print("\n***** BEGIN ICMP CHECKING *****")
            for x in entry.keys():
                if ping_router(x):
                    print("\n\t**IP: - " + x + " - responding to ICMP\n")
                else:
                    print("\n\t**IP: - " + x + " - NOT responding to ICMP\n")
        
            ## Use a loop to check each device for ICMP responses
            print("\n***** BEGIN SHOW IP INT BRIEF *****")
            for x in entry.keys():
                print("\n" + interface_check(str(entry[x]), x, "admin", "alta3"))
        
        ## Call main()
        main()

0. Now save the script at `usopen.py`

0. Exit Leafpad.

0. Run the script.

    `student@beachhead:~/mycode/usopen$` `python3 usopen.py`
    
0. When prompted, type the location of `ip_list.py` (if it is local, just type `ip_list.py`)

0. The result should look simmilar to below.

    ![usopen.py Output](https://alta3.com/static/images/python/us_open.png)
    
0. So we have a working Netmiko script. Let's write a new stand alone function for our program, that also uses Netmiko.

0. Open a new leafpad document.

    `student@beachhead:~/mycode/usopen$` `leafpad`
    
0. Our goal is to write a function that we can import into `usopen.py`, and then use it within our `main()` function, to extend our program.

0. Let's start by describing the new functionality we'd like to build. How about a function that reads config data, line-by-line, from a text document. This will allow users to config the switches with a configuration file they provide. Start by listing out all of the new code we need to write.
    - At the end of the script, user is asked if they want to provide a new configuration file? **YES / NO**
    - If **NO** the script ends
    - If **YES** the user should be asked where the new configuration file is
    - IF **YES** the user should be asked what IP address they'd like to bootstrap-- the driver should be grabbed if that IP exists
    - If **YES** the IP address, driver and file location should be passed as an argument to the function `bootstrapper`
    - If **YES** the new function `bootstrapper` should apply the new running configuration
    - If **YES** the new function `bootstrapper` should return TRUE when complete
    - IF **YES** the new function `bootstrapper` should return FALSE if fails
    - Print to the user if the new configuration application was successful or failed
    - For simplicity sake, assume the same static UN / PW we've used so far.

0. Now we have a very clear picture of what we're trying to do. We have some tweaks to our main function, and know more-a-less exactly what the function `bootstrapper` needs to do. In fact, it sounds like we now have a **TWO** person *team* project. Someone *could* work on `bootstrapper`, while someone else made the appropriate tweaks to *main*. Unforunately, you're going to be doing all the work solo.

0. Let's write `bootstrapper.py`. The objective should be clear from our description above, so you can write it yourself. Or use the code below. If you use the code below, please review the comments. *To be clear, this code goes in a new blank document*.

        from netmiko import ConnectHandler
        ## Define our new function called bootstrapper and the expected arugments (all five)
        def bootstrapper(dev_type, dev_ip, dev_un, dev_pw, config):
            try:
                config_file = open(config, 'r') # open the file object described by config argument
                config_lines = config_file.read().splitlines() # create a list of the file lines without \n
                config_file.close() # close the file object
            
                open_connection = ConnectHandler(device_type=dev_type, ip=dev_ip, username=dev_un, password=dev_pw)
                open_connection.enable() # this sets the connection in enable mode
                output = open_connection.send_config_set(config_lines) # pass the config to the send_config_set() method
                print(output) # print the config to the screen # output to the screen
                open_connection.send_command_expect('write memory') # write the memory (okay if this gets done twice)
                open_connection.disconnect() # close the open connection
           
                return True # Everything worked! - "Return TRUE when complete"
            except:
                return False # Something failed during the configuration process - "Return FALSE if fails"
            
0. Save the code as `/home/student/mycode/usopen/bootstrapper.py`

0. Exit leafpad.

0. Make the file executable.

    `student@beachhead:~/mycode/usopen$` `chmod u+x bootstrapper.py`

0. Now we need to make some tweaks to `usopen.py` so that our new function is imported, and then called.

    `student@beachhead:~/mycode/usopen$` `leafpad usopen.py`

0. You can make the changes yourself, or copy & paste the code block below to replace `/home/student/mycode/usopen/usopen.py`

        #!/usr/bin/env python3
        ## USOpen Tournament Switch Checker -- 2018.05.01
        ''' usopen.py
        This script is being designed to provide the following automated tasks:
        - ping check the router (import os)
        - login check the router (import netmiko)
        - determine if interfaces in use are up (import netmiko)
        - Apply new configuration (import netmiko)
        
        The IPs and device type should be made available via an Excel spreadsheet
        
        '''
        import os
        import bootstrapper
        
        ## pyexcel and pyexce-xls are required for our program to execute
        # python3 -m pip install --user pyexcel
        # python3 -m pip install --user pyexcel-xls
        import pyexcel
        
        # python3 -m pip install --user netmiko
        from netmiko import ConnectHandler
        
        ## retrieve data set from excel
        def retv_excel(par):
            d = {}
            records = pyexcel.iget_records(file_name=par) # create a record object that is an open excel sheet
            for record in records:
                d.update( { record['IP'] : record['driver'] } ) # adds a new IP and driver key:value pair to our dictionary
            return d # return the completed dictionary
        
        ## Ping router - returns True or False
        def ping_router(hostname):
        
            response = os.system("ping -c 1 " + hostname)
            
            #and then check the response...
            if response == 0:
                return True
            else:
                return False
        
        ## Check interfaces - Issue "show ip init brief"
        def interface_check(dev_type, dev_ip, dev_un, dev_pw):
            try:
                open_connection = ConnectHandler(device_type=dev_type, ip=dev_ip, username=dev_un, password=dev_pw)
                my_command = open_connection.send_command("show ip int brief")       
            except:
                my_command = "** ISSUING COMMAND FAILED **"
            finally:
                return my_command
        
        
        ## Login to router - SSH Check with Netmiko class ConnectHandler
        def login_router(dev_type, dev_ip, dev_un, dev_pw):
            try:
                open_connection = ConnectHandler(device_type=dev_type, ip=dev_ip, username=dev_un, password=dev_pw)
                return True        
            except:
                return False

        ## Main function - This is the code that runs our functions
        def main():
        
            ## Determine where *.xls input is
            file_location = str(input("\nWhere is the file location? "))
        
            ## Entry is now a local dictionary containing IP(key):driver(value)
            entry = retv_excel(file_location)
        
            ## Use a loop to check each device for SSH accessability
            print("\n***** BEGIN SSH CHECKING *****")
            for x in entry.keys():
                if login_router(str(entry[x]), x, "admin", "alta3"):
                    print("\n\t**IP: - " + x + " - SSH connectivty UP\n")
                else:
                    print("\n\t**IP: - " + x + " - SSH connectivty DOWN\n")
        
            ## Use a loop to check each device for ICMP responses
            print("\n***** BEGIN ICMP CHECKING *****")
            for x in entry.keys():
                if ping_router(x):
                    print("\n\t**IP: - " + x + " - responding to ICMP\n")
                else:
                    print("\n\t**IP: - " + x + " - NOT responding to ICMP\n")
        
            ## Use a loop to check each device for ICMP responses
            print("\n***** BEGIN SHOW IP INT BRIEF *****")
            for x in entry.keys():
                print("\n" + interface_check(str(entry[x]), x, "admin", "alta3"))
                
            ## Determine if new config should be applied && if so apply new config
            print("\n***** NEW BOOTSTRAPPING CHECK *****")
            ynchk = input("\nWould you like to apply a new configuration? y/N ")
            if (ynchk.lower() == "y") or (ynchk.lower() == "yes"):  # if user input yes or y
                conf_loc = str(input("\nWhere is the location of the new config file? "))
                conf_ip = str(input("\nWhat is the IP address of the device to be configured? "))
                
                if bootstrapper.bootstrapper(entry[conf_ip], conf_ip, "admin", "alta3", conf_loc):
                    print("\nNew configuration applied!")
                else:
                    print("\nProblem in applying new configuration!")
        
        ## Call main()
        main()

0. Save and exit.

0. Run our augmented program, `usopen.py`. Indicate `ip_list.xls`. We don't yet have a config to apply, but indicate `Yes` (we have a new config to apply) and apply the file location `dragons.here`. Supply the IP address `172.16.2.10` (the IP of switch 1). We can test our ability to catch errors.

0. So our error was caught. No dragon files exist. Download the following a *basic switch 1 config*.

    `student@beachhead:~/mycode/usopen$` `wget https://alta3.com/static/images/python/sw1-clean.conf -O sw1-clean.conf`

0. Re-run our augmented program, `usopen.py`. Indicate `ip_list.xls`. Then, this time, indicate `y`, and apply the file location `sw1-clean.conf`. Supply the IP address `172.16.2.10` (the IP of switch 1).

0. You should get an indication, `New switch configuration applied!`, if not, find your error.

0. You can try this lab again if you'd like, and this time target switch2 (`172.16.2.20`). Switch 2 config is available for download here. If you run the script against switch 2, **becareful not to overwrite switch1 config with switch2 config** or vice-versa.

    `student@beachhead:~/mycode/usopen$` `wget https://alta3.com/static/images/python/sw2-clean.conf -O sw2-clean.conf`

0. That's it for this lab, but what an introduction to Netmiko & writing orignial Python Network Automation scripts! Hopefully it's clear that automating networks with Python can be done via stare-and-compare, but ideally, requires a strong Python skillset. Don't hesitate to try writing your own function and calling it from the `usopen.py` script! If you need help, ask the instructor on ways to get started.
