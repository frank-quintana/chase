# 41. NAPALM basic python script

In this lab you will use NAPALM to capture the complete running config of two network devices and store those configs.
Then you will manually make changes to those same two devices and test connectivity to make sure the changes work.  
Finally you will restore the system back to its origional state by loading the running config as you captured 
it in the beginning of the lab.  


 0. Read the python script below, line by line and make sure you understand how it works. Then use vim to copy this python script to getrun.py.

    `student@beachhead:~$` `vim getrun.py`

    ```
    import sys
    import napalm
    
    if len(sys.argv) != 5:
       print("You supplied ", len(sys.argv)-1, " arguments but 4 are needed")
       print("getrun.py requires: OS, IP, USER and PW of device")
       print("example: python3  getrun.py  eos  a.b.c.d  username  password")
       sys.exit()
    os = sys.argv[1]
    ip = sys.argv[2]
    user = sys.argv[3]
    passwd = sys.argv[4]
    from napalm import get_network_driver
    import pprint as pp
    driver = get_network_driver(os)
    device = driver(ip,user,passwd)
    device.open()
    config = device.get_config()
    running_config = config['running']
    print(running_config)
    device.close()
    ```

    > The key NAPALM functions to note here are:  
       `get_network_driver()`,  
       `device.open()`,
       `device.get_config()`
       `device.close()`  


 0. Now save your running config for both your switches. WARNING: If you are cutting and pasting the config below, you must edit the `>` which for HTML reasons will show up as `\&gt;` and that WILL NOT WORK! :)

    `python3  getrun.py eos  172.16.2.10  admin  alta3 > sw1.run`  
    `python3  getrun.py eos  172.16.2.20  admin  alta3 > sw2.run`  

 0. Make sure that PC-1 is indeed connected to SW1, eth2. To do this follow these steps

    a. hover your mouse over the link between SW1 amd PC-1
    b. The link will turn red, so don't move your mouse. Keep it right there.
    c. In about 3 seconds, and a small popup should indicate SW1 Eth2 and PC Eth0 like this:

       `Link from vEOSl port Ethernet2 to PC-1 port Ethernet0`
       `                        ^                      ^     `
    d. If connected differently, please correct your wiring.

 0. At the SW1 console, manually configure sw1 eth2 to be vlan mode access on vlan 200 (Make sure PC-1 is indeed connected on eth2)

    ```
    SW1#config t  
    SW1(config)#int ethernet 2    
    SW1(config-if-Et2)#switchport mode access  
    SW1(config-if-Et2)#switchport access vlan 200
    SW1(config-if-Et2)#exit
    SW1(config)#exit
    ```
 
 0. Manually configure eth 1 on SW1 to be a trunk with vlan 200 allowed.

    ```
    SW1#configure terminal 
    SW1(config)#interface ethernet 1
    SW1(config-if-Et1)#switchport mode trunk 
    SW1(config-if-Et1)#switchport trunk allowed vlan 200
    ```

 0. At the SW2 console, manually configure eth 1 on SW2 to be a trunk with vlan 200 allowed.

    ```
    SW2#configure terminal 
    SW2(config)#interface ethernet 1
    SW2(config-if-Et1)#switchport mode trunk 
    SW2(config-if-Et1)#switchport trunk allowed vlan 200
    ```
 0. At the SW2 console, manually configure sw1 eth2 to be vlan mode access on vlan 200 (Make sure PC-2 is indeed connected on eth2).  

    ```
    SW2#config t  
    SW2(config)#int ethernet 2    
    SW2(config-if-Et2)#switchport mode access  
    SW2(config-if-Et2)#switchport access vlan 200
    SW2(config-if-Et2)#exit
    SW2(config)#exit
    ```
 
 0. Config PC-1

    a. Right click PC-1 and click `Stop`  
    b. Right click PC-1 and click `Edit config`, replace the config with this:

        ```
        ip 10.55.0.10 255.255.255.0
        set pcname PC-1

        ```
     c. Right click PC-1 and click `Start`

 0. Config PC-2

    a. Right click PC-2 and click `Stop`  
    b. Right click PC-2 and click `Edit config`, replace the config with this:

        ```
        ip 10.55.0.20 255.255.255.0
        set pcname PC-2

        ```
    c. Right click PC-2 and click `Start`


 0. Test connectivity at PC-1. Right click PC-1 and open its console

     `ping 10.55.0.20`


 0. Now use vim to save the following script that you will use to restore the config you saved above.

     `vim setconfig.py`
    

    ```
    import sys
    import napalm
    
    if len(sys.argv) != 6:
       print("You supplied ", len(sys.argv)-1, " arguments but 5 are needed")
       print("getrun.py requires: OS, IP, USER, PW, restore_file_name of device")
       print("example: python3  getrun.py  eos  a.b.c.d  username  password")
       sys.exit()
    os = sys.argv[1]
    ip = sys.argv[2]
    user = sys.argv[3]
    passwd = sys.argv[4]
    filename = sys.argv[5]
    from napalm import get_network_driver
    import pprint as pp
    driver = get_network_driver(os)
    device = driver(ip,user,passwd)
    device.open()
    device.load_replace_candidate(filename)
    print(device.compare_config())
    device.commit_config()
    device.close()
    exit()

    ```
    
 0. Restore the config to its previous state.

    `python3 setconfig.py eos 172.16.2.10 admin alta3 sw1.run`  
    `python3 setconfig.py eos 172.16.2.20 admin alta3 sw2.run`  

    > The key NAPALM command here is `device.compare_config()`.  This function compares the current configuration with candatate configuration and computes the difference which is then used to apply to the device with `device.commit_config()`  

 0. You're done with this lab, unless you want to explore the **rollback()** function. If that's you, keep reading.

 0. In this lab, we used **commit_config()** to restore a previous config. However, NAPALM automatically creates a rollback point before commiting a new config. So, given you just commited a config. Log into SW1 (manually), and make some changes. A hostname, or a VLAN change is fine.
 
 0. Now edit `setconfig.py` so it includes the **rollback()** function.
 
     ```
    import sys
    import napalm
    
    if len(sys.argv) != 5: # NEED ONE LESS ARGUMENT
       print("You supplied ", len(sys.argv)-1, " arguments but 5 are needed")
       print("getrun.py requires: OS, IP, USER, PW, restore_file_name of device")
       print("example: python3  getrun.py  eos  a.b.c.d  username  password new_config.txt")
       sys.exit()
    os = sys.argv[1]
    ip = sys.argv[2]
    user = sys.argv[3]
    passwd = sys.argv[4]
    ## filename = sys.argv[5] ## THIS LINE IS NO LONGER NEEDED. WE DONT NEED TO BRING A FILENAME
    from napalm import get_network_driver
    import pprint as pp
    driver = get_network_driver(os)
    device = driver(ip,user,passwd)
    device.open()
    device.rollback() # THIS LINE IS NEW! THIS WILL INSTRUCT NAPALM TO APPLY THE ROLLBACK CONFIG SAVED IN THE ARISTA SWITCH
    device.close()
    exit()
    ```
 
 0. Now try running `python3 setconfig.py eos 172.16.2.10 admin alta3`. It'll do the same thing, but for a different reason. This time, we're not bringing a config file filled with changes, intead, we're applying the rollback point found inside of the switch. 

<tiny>&copy;  2018 Alta3 Research (napalm-06a.md)</tiny>
