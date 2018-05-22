# 40. NAPALM validation

This lab will use a validation file to test the actual configuration of devices with expected configuration. You will run a provided python script and test SW1. The test will fail and you must figure out why and take corrective action.


 0. `student@beachhead:/$` `cd ~`
 
 0. Now lets create a file in vim, **sw1_validate.yml**, and in it place the text below (begins with --- get_facts)
 
 0. `student@beachhead:~$` `vim sw1_validate.yml`


    
        ---
         - get_facts:
             os_version: 4.17 #the actual version is much longer than this, so this will cause an error
        
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
       


 0. Lets run a script to see the validation first hand:

    `student@beachhead:~$` `python3`

    ` >>>` `from napalm import get_network_driver`  
    ` >>>` `import pprint as pp`  
    ` >>>` `driver = get_network_driver('eos')`  
    ` >>>` `device = driver('172.16.2.10', 'admin', 'alta3')`  
    ` >>>` `device.open()`   
    ` >>>` `pp.pprint(device.compliance_report("/home/student/sw1_validate.yml"))`  

     ```
     {'complies': False,
      'get_facts': {'complies': False,
                    'extra': [],
                    'missing': [],
                    'present': {'os_version': {'actual_value': '4.17.8M-6296892.4178M',
                                               'complies': False,
                                               'expected_value': 4.17,
                                               'nested': False}}},
      'get_interfaces_ip': {'complies': True,
                            'extra': [],
                            'missing': [],
                            'present': {'Management1': {'complies': True,
                                                        'nested': True}}},
      'ping_SW2': {'complies': True,
                   'extra': [],
                   'missing': [],
                   'present': {'success': {'complies': True, 'nested': True}}},
      'skipped': []}
     ```

 0. Answer the following questions:
   - **Q: Why did the failure occur in `'complies': False`?**
     - We *expected a value of 4.17*, when the *actual_value* is *4.17.8M-6296892.4178M*
   - **Q: Would we have to do to fix this?**
     - Edit the file *sw1_validate.yml* to containe the *os_version: 4.17.8M-6296892.4178M* 

 0. Go ahead and exit python.

    ` >>>` `device.close()` # proper way to shutdown conection to device  
    ` >>>` `exit()` # exit python
     
 0. So the switch is correct, and it is our validation file that is wrong! Edit`sw1_validate.yml` so that this validation passes (*change 4.17 to 4.17.8M-6296892.4178M*).  

    `student@beachhead:~$` `vim sw1_validate.yml`  

 0. Now run the script again, and make sure the validation returns `complies: True`.
 
    `student@beachhead:~$` `python3`

    ` >>>` `from napalm import get_network_driver`  
    ` >>>` `import pprint as pp`  
    ` >>>` `driver = get_network_driver('eos')`  
    ` >>>` `device = driver('172.16.2.10', 'admin', 'alta3')`  
    ` >>>` `device.open()`   
    ` >>>` `pp.pprint(device.compliance_report("/home/student/sw1_validate.yml"))`
    
 0. Great job! That's it for this lab.

<tiny>&copy; Alta3 Research napalm-05.md</tiny>
