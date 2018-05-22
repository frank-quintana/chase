# 43. NAPALM Challenge

 0. Run a python script to pull the current config from both switches. You'll need to write this.
 
    `Hint: Use skills from the previous lab(s). Handy python scripts are also on the bottom of this lab.`
 
 0. Edit and save both switch config files so that... 

    `Eth1 on both switches to mode trunk with vlan 200 allowed`
    
    `Eth2 on both switches = switchport mode access vlan 200`

 0. Run this script to push your changes. 
 
    ```
    from __future__ import print_function

    import napalm
    import sys
    import os

    if len(sys.argv) != 3:
       print("You supplied ", len(sys.argv)-1, " arguments but 2 are needed")
       print("change_config.py requires: IP and MERGE_FILE")
       print("example: python3 change_config.py  A.B.C.D  MERGE_FILE")
       sys.exit()
    ip = sys.argv[1]
    mergefile = sys.argv[2]

    while not os.path.isfile(mergefile):
      print("SORRY ", mergefile," is not a file ")
      mergefile = input("What is the path to the compliance file you want to use? ") 

    driver = napalm.get_network_driver('eos')
    device = driver(hostname= ip , username='admin', password='alta3')
    print('Opening to ',ip)
    device.open()
    print('Loading replacement candidate ', mergefile)

    #Prepare to diff by loading candidate
    device.load_replace_candidate(filename=mergefile)

    # the configuration you can check the changes:
    print('\nDiff: ')
    print(device.compare_config())

    # You can commit or discard the candidate changes.
    choice = input("\nWould you like to commit these changes? [yN]: ")
    if choice == 'y':
      print('Committing ...')
      device.commit_config()
    else:
      print('Discarding ...')
      device.discard_config()

    # close the session with the device.
    print('Bye, see you later')
    device.close()
    print('Done.')
    ```

### Here are scripts that may be handy for this section.

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


