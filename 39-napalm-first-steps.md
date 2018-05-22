# 39. NAPALM first steps

### Lab Objective

Use apt to Install Napalm and use the commands to look into the current configuration, change the configuration, and revert back to a stable configuration in Napalm.  

   **Install Napalm**  
    
 0. These simple steps will get you started with Napalm by installing it.
 
    `student@beachhead:~$` `python3 -m pip install --user napalm`  

    **Basic napalm -- Fetch running config**

 0. If you need to see what your current napalm configuration that is running looks like, follow these steps.

    `student@beachhead:~$` `python3`  
    `>>>` `from napalm import get_network_driver`  
    `>>>` `import pprint as pp`  
    `>>>` `driver = get_network_driver('eos')`  
    `>>>` `device = driver('172.16.2.10', 'admin', 'alta3')`  
    `>>>` `device.open()`  
    `>>>` `device.get_facts()`  
    `>>>` `pp.pprint(device.get_interfaces())`  
    `>>>` `config = device.get_config()`  
    `>>>` `running_config = config['running']`  
    `>>>` `print(running_config)`  
    `>>>` `path = "/home/student/sw1.conf"`  
    `>>>` `sw1_conf_file = open(path, "w")`  
    `>>>` `sw1_conf_file.write(running_config)`  
    `>>>` `sw1_conf_file.close()`  
    `>>>` `exit()`  

    **Oh, no!! What Happened? Accidental configuration change -- Disable ssh**

 0. It looks like someone accidentally disabled ssh to the switch. 

    `student@beachhead:~$` `ssh admin@172.16.2.10`  
    `SW1#` `en`  
    `SW1#` `show management ssh`  
    `SW1#` `conf t`  
    `SW1#` `management ssh`  
    `SW1#` `shutdown`  
    `SW1#` `exit`  
    `SW1#` `show management ssh`  
    `SW1#` `exit`  
    `SW1#` `exit`  
    `student@beachhead:~$` `ssh admin@172.16.2.10`  
   
    > Fails "Connection refused"

    **Basic napalm -- Check for changes and revert to known good**

 0. Here is how you would go about reverting to the previous configuration, before the ssh was disabled. 

    `student@beachhead:~$` `python3`  
    `>>>` `from napalm import get_network_driver`  
    `>>>` `driver = get_network_driver('eos')`  
    `>>>` `device = driver('172.16.2.10', 'admin', 'alta3')`  
    `>>>` `device.open()`  
    `>>>` `device.load_replace_candidate(filename='/home/student/sw1.conf')`  
    `>>>` `print(device.compare_config())`  
    `>>>` `device.commit_config()`  
    `>>>` `exit()`  
    `student@beachhead:~$` `ssh admin@172.16.2.10`  
    
    > It's back up! 

    <tiny>© Alta3 Research napalm-04.md</tiny>
