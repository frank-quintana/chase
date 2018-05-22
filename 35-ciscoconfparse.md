# Lab 35 - Using Python to Search Network Config Files - ciscoconfparse
### THURSDAY - &#x2B50;REQUIRED&#x2B50;

### Lab Objective

The objective of this lab is to learn how to use a Python tool called *ciscoconfparse*. This *ciscoconfparse* is a Python library, which parses through Cisco IOS-style (and other vendor) configurations. It can:

- Audit existing router / switch / firewall / wlc configurations
- Retrieve portions of the configuration
- Modify existing configurations
- Build new configurations

If you've ever edited one of these, you know they can be VERY long and very repetitive. The library examines an IOS-style config and breaks it into a set of linked parent / child relationships. You can perform complex queries about these relationships.

The following graphic shows what is meant by parent / child relationships within the IOS configuration files.

![Parent Child IOS](https://alta3.com/static/images/python/python_cisco_001.png)

### Procedure

0. Open a terminal and move to your home directory.

    `student@beachhead:/$` `cd /home/student/bin`

0. Install the ciscoconfparse package by issuing the following command:

    `student@beachhead:~/bin$` `sudo python3 -m pip install ciscoconfparse`

0. Take a look at the [PiPY page on the ciscoconfparse project](https://pypi.python.org/pypi/ciscoconfparse)

0. Now let's download a real, albeit shortened, Cisco IOS configuration document.

     `student@beachead:~/bin$` `wget goo.gl/NuDPC6 -O ios_audit.conf`

0. This one is short, so go ahead and print it to the screen. In production, this might be many thousands of lines long.

     `student@beachead:~/bin$` `cat ios_audit.conf`

0. Let's work with this ciscoconfparse tool, and see what we can make it do. Start leafpad.

     `student@beachead:~/bin$` `leafpad&`

0. Below is some code that has been written for us already. We'll just use it, learn how it works, then manipulate that code to get the results we want.

    ```
    #!/usr/bin/env python3
    from ciscoconfparse import CiscoConfParse

    def standardize_intfs(parse):

        ## Search all switch interfaces and modify them
        #
        # r'^interface.+?thernet' is a regular expression, for ethernet intfs
        for intf in parse.find_objects(r'^interface.+?thernet'):

            has_stormcontrol = intf.has_child_with(r' storm-control broadcast')
            is_switchport_access = intf.has_child_with(r'switchport mode access')
            is_switchport_trunk = intf.has_child_with(r'switchport mode trunk')

            ## Add missing features
            if is_switchport_access and (not has_stormcontrol):
                intf.append_to_family(' storm-control action trap')
                intf.append_to_family(' storm-control broadcast level 0.4 0.3')

            ## Remove dot1q trunk misconfiguration...
            elif is_switchport_trunk:
                intf.delete_children_matching('port-security')

    ## Parse the config
    parse = CiscoConfParse('ios_audit.conf') # this is our input file

    ## Search and standardize the interfaces...
    standardize_intfs(parse)
    parse.commit()     # commit() **must** be called before searching again

    ## regular expression usage in has_line_with() to find if the config has a matching line 
    if not parse.has_line_with(r'^service\stimestamp'):
        ## prepend_line() adds a line at the top of the configuration
        parse.prepend_line('service timestamps debug datetime msec localtime show-timezone')
        parse.prepend_line('service timestamps log datetime msec localtime show-timezone')

    ## Write the new configuration
    parse.save_as('ios_audit.conf.new')
    ```

0. Copy and paste the code above into the leafpad document.

0. Choose **File > Save As** then save the code as **cisco_parser.py** within **/home/student/bin/**

0. Down in the terminal, change permissions on your script.

    `student@beachhead:~/bin$` `./cisco_parser.py`

0. Let's see what the results are. Open the old config file (no changes).

    `student@beachhead:~/bin$` `leafpad ios_audit.conf`

0. Open the new file (the one with changes made by our script).

    `student@beachhead:~/bin$` `leafpad ios_audit.conf.net`
    
0. Answer the following questions.

    - **Q1: The new file has two new lines located at the top of the configuration file. Which line(s) of code in our script added this?**
      - parse.prepend_line('service timestamps debug datetime msec localtime show-timezone')
      - parse.prepend_line('service timestamps log datetime msec localtime show-timezone')
    - **Q2: Why did only FastEthernet0/1 and FastEthernet0/3 get storm-control features? Which line(s) of code in our script added this?**
      - if is_switchport_access and (not has_stormcontrol):
      -    intf.append_to_family(' storm-control action trap')
      -    intf.append_to_family(' storm-control broadcast level 0.4 0.3')

0. If you work with Cisco gear, you should definately keep playing with this program. Below are some things to try:

    - Make the script **cisco_parser.py** remove the **switchport negotiate** setting
    - Make the script **cisco_parser.py** produce a file called **ios_audit.conf.new2**
    - Make the script **cisco_parser.py** white your name to the top of the file
    
0. There you go! Just another way Python can make networking administration easier. That's it for this lab.
