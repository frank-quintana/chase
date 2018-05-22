# 41. NAPALM Changes, Validation, and Rollback

 0. In this lab, we're going to build out a series of Python scripts that target specific tasks. Please study EACH NEW SCRIPT and if there is any code you do not understand, please ask the teacher to review.

    >vim hint: `i` to insert, `ESC:wq` to save

 0. Copy and paste the code below into `myconfig.py`. It is similar to the code in lab 39, only this time we added a new line, `device.get_config()`, and print that data to standard out.

     `student@beachhead:~$`  `vim myconfig.py`   



    ```python 
    # import code from NAPALM
    from napalm import get_network_driver
    
    # Tell NAPALM to speak "eos" commands to our switches (very cisco-like)
    driver = get_network_driver('eos')
    
    # Hard-code the switch credentials
    device = driver('172.16.2.10', 'admin', 'alta3') 
    
    #Equates to: ssh into the switch, login, and enable
    device.open() 
    
    # Print STARTUP, RUNNING, and CANDIDATE configs 
    print(device.get_config()) 
    ```
 0. Run the above script and see if you can find STARTUP, RUNNING, and CANDIDATE configs inside the json-like hunk of data.
 
    `student@beachhead:~$`  `python3 myconfig.py`
   
    > {`'startup'`: '! Command: show startup-config\n! Startup-config last modified at  Tue Mar 20 21:16:38 2018 by admin\n! device: SW1 (vEOS, EOS-4.17.8M)\n!\n! boot system flash:/vEOS-lab.swi\n!\ntransceiver qsfp default-mode 4x10G\n!\nhostname SW1\n!\nspanning-tree mode mstp\n!\nno aaa root\n!\nusername admin role network-admin secret sha512 $6$g4BMDSykJrMVwToE$LYMOlnWjVKtqEaq2dQ4oWjv6z6oSUyPOMImRwZWox85LghEmFhsUQ9BwZOLp7QnkdVEuk0YGG5HE1IpRNfvTf.\n!\ninterface Ethernet1\n!\ninterface Ethernet2\n!\ninterface Ethernet3\n!\ninterface Ethernet4\n!\ninterface Ethernet5\n!\ninterface Ethernet6\n!\ninterface Ethernet7\n!\ninterface Ethernet8\n!\ninterface Ethernet9\n!\ninterface Ethernet10\n!\ninterface Ethernet11\n!\ninterface Ethernet12\n!\ninterface Management1\n   mtu 1450\n   ip address 172.16.2.10/24\n!\nip route 0.0.0.0/0 172.16.2.100\n!\nno ip routing\n!\nmanagement api http-commands\n   protocol http\n   no shutdown\n!\nmanagement ssh\n   shutdown\n!\n!\nend\n', `'running'`: '! Command: show running-config\n! device: SW1 (vEOS, EOS-4.17.8M)\n!\n! boot system flash:/vEOS-lab.swi\n!\ntransceiver qsfp default-mode 4x10G\n!\nhostname SW1\n!\nspanning-tree mode mstp\n!\nno aaa root\n!\nusername admin role network-admin secret sha512 $6$g4BMDSykJrMVwToE$LYMOlnWjVKtqEaq2dQ4oWjv6z6oSUyPOMImRwZWox85LghEmFhsUQ9BwZOLp7QnkdVEuk0YGG5HE1IpRNfvTf.\n!\ninterface Ethernet1\n!\ninterface Ethernet2\n!\ninterface Ethernet3\n!\ninterface Ethernet4\n!\ninterface Ethernet5\n!\ninterface Ethernet6\n!\ninterface Ethernet7\n!\ninterface Ethernet8\n!\ninterface Ethernet9\n!\ninterface Ethernet10\n!\ninterface Ethernet11\n!\ninterface Ethernet12\n!\ninterface Management1\n   mtu 1450\n   ip address 172.16.2.10/24\n!\nip route 0.0.0.0/0 172.16.2.100\n!\nno ip routing\n!\nmanagement api http-commands\n   protocol http\n   no shutdown\n!\n!\nend\n', `'candidate`': ''}
     
    > Notice that the `startup` and `running` configs have content but the `candidate` is empty. 

 0. For educational purposes, create this script to print out pure json.
 
    `student@beachhead:~$`  `vim config_2_json.py`

    ```python 
    from napalm import get_network_driver
    import json
    driver = get_network_driver('eos')
    device = driver('172.16.2.10', 'admin', 'alta3') 
    device.open() # start the connection
    config = device.get_config()
    print("UGLY JASON-LIKE BLOB:\r" )
    print(device.get_config())
    print("REAL JASON:\r" )    
    print(json.dumps( config ,indent=4, separators=(',', ': ')))
    ```

 0. Now run `config_2_json.py` to see the configs in JSON:
 
    `student@beachhead:~$`  `python3 config_2_json.py`

    > **UGLY JSON-LIKE BLOB:**  
{`'running':` '! Command: show running-config\n! device: SW3 (vEOS, EOS-4.17.8M)\n!\n! boot system flash:/vEOS-lab.swi\n!\ntransceiver qsfp default-mode 4x10G\n!\nhostname SW3\n!\nspanning-tree mode mstp\n!\nno aaa root\n!\nusername admin role network-admin secret sha512 $6$LH6hfO1CqguRBpkk$7OgT5zhOX2rx27dovb9gfXcpqz9uscLidFGjgGesGX/qpTei1aJPhJfQ9TugGomE00RYywma/qUZC9RDboKe/0\n!\ninterface Ethernet1\n!\ninterface Ethernet2\n!\ninterface Ethernet3\n!\ninterface Ethernet4\n!\ninterface Ethernet5\n!\ninterface Ethernet6\n!\ninterface Ethernet7\n!\ninterface Ethernet8\n!\ninterface Ethernet9\n!\ninterface Ethernet10\n!\ninterface Ethernet11\n!\ninterface Ethernet12\n!\ninterface Management1\n   mtu 1450\n   ip address 172.16.2.20/24\n!\nip route 0.0.0.0/0 172.16.2.100\n!\nno ip routing\n!\nmanagement api http-commands\n   protocol http\n   no shutdown\n!\n!\nend\n', `'candidate':` '', `'startup':` '! Command: show startup-config\n! Startup-config last modified at  Thu Mar 22 14:28:10 2018 by admin\n! device: SW3 (vEOS, EOS-4.17.8M)\n!\n! boot system flash:/vEOS-lab.swi\n!\ntransceiver qsfp default-mode 4x10G\n!\nhostname SW3\n!\nspanning-tree mode mstp\n!\nno aaa root\n!\nusername admin role network-admin secret sha512 $6$LH6hfO1CqguRBpkk$7OgT5zhOX2rx27dovb9gfXcpqz9uscLidFGjgGesGX/qpTei1aJPhJfQ9TugGomE00RYywma/qUZC9RDboKe/0\n!\ninterface Ethernet1\n!\ninterface Ethernet2\n!\ninterface Ethernet3\n!\ninterface Ethernet4\n!\ninterface Ethernet5\n!\ninterface Ethernet6\n!\ninterface Ethernet7\n!\ninterface Ethernet8\n!\ninterface Ethernet9\n!\ninterface Ethernet10\n!\ninterface Ethernet11\n!\ninterface Ethernet12\n!\ninterface Management1\n   mtu 1450\n   ip address 172.16.2.20/24\n!\nip route 0.0.0.0/0 172.16.2.100\n!\nno ip routing\n!\nmanagement api http-commands\n   protocol http\n   no shutdown\n!\n!\nend\n'}

    > **REAL JSON:**  
    >{  
    `"running":` "! Command: show running-config\n! device: SW3 (vEOS, EOS-4.17.8M)\n!\n! boot system flash:/vEOS-lab.swi\n!\ntransceiver qsfp default-mode 4x10G\n!\nhostname SW3\n!\nspanning-tree mode mstp\n!\nno aaa root\n!\nusername admin role network-admin secret sha512 $6$LH6hfO1CqguRBpkk$7OgT5zhOX2rx27dovb9gfXcpqz9uscLidFGjgGesGX/qpTei1aJPhJfQ9TugGomE00RYywma/qUZC9RDboKe/0\n!\ninterface Ethernet1\n!\ninterface Ethernet2\n!\ninterface Ethernet3\n!\ninterface Ethernet4\n!\ninterface Ethernet5\n!\ninterface Ethernet6\n!\ninterface Ethernet7\n!\ninterface Ethernet8\n!\ninterface Ethernet9\n!\ninterface Ethernet10\n!\ninterface Ethernet11\n!\ninterface Ethernet12\n!\ninterface Management1\n   mtu 1450\n   ip address 172.16.2.20/24\n!\nip route 0.0.0.0/0 172.16.2.100\n!\nno ip routing\n!\nmanagement api http-commands\n   protocol http\n   no shutdown\n!\n!\nend\n",  
    `"candidate":` "",  
    `"startup":` "! Command: show startup-config\n! Startup-config last modified at  Thu Mar 22 14:28:10 2018 by admin\n! device: SW3 (vEOS, EOS-4.17.8M)\n!\n! boot system flash:/vEOS-lab.swi\n!\ntransceiver qsfp default-mode 4x10G\n!\nhostname SW3\n!\nspanning-tree mode mstp\n!\nno aaa root\n!\nusername admin role network-admin secret sha512 $6$LH6hfO1CqguRBpkk$7OgT5zhOX2rx27dovb9gfXcpqz9uscLidFGjgGesGX/qpTei1aJPhJfQ9TugGomE00RYywma/qUZC9RDboKe/0\n!\ninterface Ethernet1\n!\ninterface Ethernet2\n!\ninterface Ethernet3\n!\ninterface Ethernet4\n!\ninterface Ethernet5\n!\ninterface Ethernet6\n!\ninterface Ethernet7\n!\ninterface Ethernet8\n!\ninterface Ethernet9\n!\ninterface Ethernet10\n!\ninterface Ethernet11\n!\ninterface Ethernet12\n!\ninterface Management1\n   mtu 1450\n   ip address 172.16.2.20/24\n!\nip route 0.0.0.0/0 172.16.2.100\n!\nno ip routing\n!\nmanagement api http-commands\n   protocol http\n   no shutdown\n!\n!\nend\n"  
    >}

    > This confirms that `startup:`, `running:`, and `candidate:` are keys!  
    > Here is the line of code that stores the JSON-like blob in a variable called `config`:  
    >  `config = device.get_config()`  
    > To EXTRACT the running config from the JSON-like blob, add an additional line of code like this:  
    >  `running_config = config['running']`  

    > Now that we are a little smarter about `myconfig.py`, we realize that the name `myconfig.py` was a poor naming choice. The name should have been `get_my_ugly_almost_json_glob_of_running_startup_candidate_configs.py`, but that is too much to type, so lets create `getjson.py` and abandon `myconfig.py`. While we are at it, let's gather the IP address of our target switch on the command line by adding a few extra lines of code.

 0. Using vim, create `getjson.py` file:

    `student@beachhead:~$`  `vim getjson.py`  
  
    ```python
    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 2:
       print("You supplied ", len(sys.argv)-1, " arguments but 1 is needed")
       print("getjson.py requires: IP")
       print("example: python3 getjson.py  a.b.c.d")
       sys.exit()
    ip = sys.argv[1]
    from napalm import get_network_driver
    driver = get_network_driver('eos') 
    device = driver(ip, 'admin', 'alta3')
    device.open() # start the connection
    print(device.get_config()) 
    ```

 0. Let's use the `>` operator to pipe our switch configs to a file. 
 
     > **CAREFUL! If you triple-click to copy, the ">" will become "\&gt;", so simply replace `&gt;` with `>`.

    `student@beachhead:~$`  `python3 getjson.py 172.16.2.10 > sw1_current_config`
     
    `student@beachhead:~$`  `python3 getjson.py 172.16.2.20 > sw2_current_config`  

 0. Using vim, create an `sw1_validate.yml` file.

    `student@beachhead:~$` `vim sw1_validate.yml`

        ---
         - get_facts:
             os_version: 4.17.8M-6296892.4178M
        
         - get_interfaces_ip:
             Management1:
               ipv4:
                 172.16.2.10:
                   prefix_length: 24
                 _mode: strict
         - ping:
             _name: ping_SW2
             _kwargs:
               destination: 172.16.2.20
               source: 172.16.2.10
             success:
               packet_loss: 0
             _mode: strict

    > Compare `sw1_validate.yml` aboive with `sw2_validate.yml` below. Notice th difference is that SW1 and SW2 IP addresses are swapped around.

 0. Using vim, create an `sw2_validate.yml` file.
 
    `student@beachhead:~$` `vim sw2_validate.yml`

        ---
         - get_facts:
             os_version: 4.17.8M-6296892.4178M
        
         - get_interfaces_ip:
             Management1:
               ipv4:
                 172.16.2.20:
                   prefix_length: 24
                 _mode: strict
         - ping:
             _name: ping_SW2
             _kwargs:
               destination: 172.16.2.10
               source: 172.16.2.20
             success:
               packet_loss: 0
             _mode: strict
         
    > Great! We have our current json-like configs AND we have the validation files. 
    
 0. Using vim, create the following script, `myvalidator.py`
    
      `student@beachhead:~$`  `vim myvalidator.py`     
 
    ```python
    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 3:
       print("You supplied ", len(sys.argv)-1, " arguments but 2 are needed")
       print("myvalidator.py requires: IP ADDRESS and VALIDATION FILENAME")
       print("example: python3 validate.py  a.b.c.d  filename")
       sys.exit()
    ip = sys.argv[1]
    path_quest = sys.argv[2]
    driver = get_network_driver('eos')
    device = driver(ip, 'admin', 'alta3') 
    device.open()  
    while not os.path.isfile(path_quest):
       print("SORRY ",path_quest," is not a file ")
       path_quest = input("What is the path to the compliance file you want to use? ") 
    truthyz = device.compliance_report(path_quest)
    print("We determined that the target device...")
    if truthyz['complies']:
      print("COMPLIES YAY-YAY-YAY-YAY!")
    else:
      print("Does NOT comply with the target validation file")
    device.close()
    ```
     
 0. Run `myvalidator.py` with proper arguments.
 
      `student@beachhead:~$`  `python3 myvalidator.py 172.16.2.10 sw1_validate.yml`
      
      `student@beachhead:~$`  `python3 myvalidator.py 172.16.2.20 sw2_validate.yml`

    > Both should comply, so make corrections if not.
 
 0. Lets make a script that converts that ugly, non-human-friendly json-like glob into a running config that we can understand and edit. 

    `student@beachhead:~$`  `vim getrun.py`

    ```python

    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 2:
       print("You supplied ", len(sys.argv)-1, " arguments but 1 is needed")
       print("getrun.py requires: IP")
       print("example: python3 getrun.py  a.b.c.d")
       sys.exit()
    ip = sys.argv[1]
    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver(ip, 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection 
    config = device.get_config()
    # This next line EXTRACTS the running config from the glob
    running_config = config['running']
    print(running_config)
    ```

    >Notice the new line of code: `running_config = config['running']` which extracts the running config from the ugly-json-like-glob.



 0. Let's run our new script `getrun.py`. You should see a classic switch config
 
    `student@beachhead:~$`  `python3 getrun.py 172.16.2.20`

    ```
    ! Command: show running-config
    ! device: SW3 (vEOS, EOS-4.17.8M) 
    !
    ! boot system flash:/vEOS-lab.swi
    !
    transceiver qsfp default-mode 4x10G
    !
    hostname SW3
    !
    spanning-tree mode mstp
    !
    no aaa root
    !
    username admin role network-admin secret sha512 $6$LH6hfO1CqguRBpkk$7OgT5zhOX2rx27dovb9gfXcpqz9uscLidFGjgGesGX/qpTei1aJPhJfQ9TugGomE00RYywma/qUZC9RDboKe/0
    !
    interface Ethernet1
    !
    interface Ethernet2
    !
    interface Ethernet3
    !
    interface Ethernet4
    !
    interface Ethernet5
    !
    interface Ethernet6
    !
    interface Ethernet7
    !
    interface Ethernet8
    !
    interface Ethernet9
    !
    interface Ethernet10
    !
    interface Ethernet11
    !
    interface Ethernet12
    !
    interface Management1
       mtu 1450
       ip address 172.16.2.20/24
    !
    ip route 0.0.0.0/0 172.16.2.100
    !
    no ip routing
    !
    management api http-commands
       protocol http
       no shutdown
    !
    !
    end
    ```
    >As a networking person, you choke back tears of joy as you look at a config which you can recognize. Its nice to come home every now and then isn't it?

 0. Let's save `getrun.py` output to a file for both switches    

    `student@beachhead:~$`  `python3 getrun.py 172.16.2.10 > sw1.config`  

    `student@beachhead:~$`  `python3 getrun.py 172.16.2.20 > sw2.config`  

 0. Edit `sw2.config`, changing the hostname from `SW2` to `SW4` 

    `student@beachhead:~$`  `vim sw2.config`
    
 0. Copy `sw2.config` to a file called `merge.me`
 
    `student@beachhead:~$`  `cp sw2.config merge.me`
 
 0. Create a new python script called `merge.py`, hard-coding SW2 ip address into the script to keep it simple

    `student@beachhead:~$`  `vim merge.py`

    ```python
    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver('172.16.2.20', 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection
    #The next two lines of code are new
    device.load_merge_candidate("/home/student/merge.me")
    device.commit_config()
    ```
 
 0. Run `merge.py` to merge your change into SW2.
 
    `student@beachhead:~$`  `python3 merge.py`   

 0. Let's see if our change actually occurred.
 
    `student@beachhead:~$`  `python3 getrun.py 172.16.2.20` 

     or if we use grep....

    `student@beachhead:~$`  `python3 getrun.py 172.16.2.20 | grep host`       

 0. Lets create a collback script because we *DEEPLY REGRET* :) that last change!  

    `student@beachhead:~$`  `vim rollback.py`

    ```
    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver('172.16.2.20', 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection
    device.rollback()
    ```

 0. Now perform the rollback!

    `student@beachhead:~$`  `python3 rollback.py`

 0. Let's see if our rollback worked
 
    `student@beachhead:~$`  `python3 getrun.py` 



### Here are the scripts we used in this section

----

Name: get_json.py  

    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 2:
       print("You supplied ", len(sys.argv)-1, " arguments but 1 is needed")
       print("getjson.py requires: IP")
       print("example: python3 getjson.py  a.b.c.d")
       sys.exit()
    ip = sys.argv[1]
    from napalm import get_network_driver
    driver = get_network_driver('eos') 
    device = driver(ip, 'admin', 'alta3')
    device.open() # start the connection
    print(device.get_config()) 

----

Name: get_run.py

    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 2:
       print("You supplied ", len(sys.argv)-1, " arguments but 1 is needed")
       print("getrun.py requires: IP")
       print("example: python3 getrun.py  a.b.c.d")
       sys.exit()
    ip = sys.argv[1]
    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver(ip, 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection 
    config = device.get_config()
    # This next line EXTRACTS the running config from the glob
    running_config = config['running']
    print(running_config)

----


Name: get_start.py

    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 2:
       print("You supplied ", len(sys.argv)-1, " arguments but 1 is needed")
       print("getrun.py requires: IP")
       print("example: python3 getrun.py  a.b.c.d")
       sys.exit()
    ip = sys.argv[1]
    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver(ip, 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection 
    config = device.get_config()
    # This next line EXTRACTS the running config from the glob
    running_config = config['start']
    print(running_config)

----

Name: merge.py

    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver('172.16.2.20', 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection
    #The next two lines of code are new
    device.load_merge_candidate("/home/student/merge.me")
    device.commit_config()

----

name: rollback.py

    from napalm import get_network_driver # import code from NAPALM
    driver = get_network_driver('eos') # get hte driver for Arista devices
    device = driver('172.16.2.20', 'admin', 'alta3') # apply the switch credentials
    device.open() # start the connection
    device.rollback()


-----
Name: myvalidate.py

    import os.path
    import sys
    from napalm import get_network_driver # import code from NAPALM
    if len(sys.argv) != 3:
       print("You supplied ", len(sys.argv)-1, " arguments but 2 are needed")
       print("myvalidator.py requires: IP ADDRESS and VALIDATION FILENAME")
