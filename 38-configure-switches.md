# 38. Configure the Switches

### Procedure

 0. We'll start by configuring switch 1. Right-click that switch, and select `console`
 
 0. Let's not shortcut past a learning opportunity. We're going to take the following switch configuration line-by-line. Explaining it as we go. If you've ever worked within network gear, you'll feel at home.

 0. Login to the switch as admin (This should not require a password the first time if you remember the credentials we were given in the previous lab. 

    `localhost login:` `admin`  


    ### vEOS Config  
 
 0. Configure your vEOS and give it a hostname `SW1`
 
    `localhost>` `enable`  
    `localhost#` `configure terminal`  
    `localhost(config)#` `hostname SW1`  

    ### Management Config  
 
 0. Configure your management interface for the Arista switch. 
 
    `SW1(config)#` `username admin secret alta3`  
    `SW1(config)#` `ip route 0.0.0.0 0.0.0.0 172.16.2.100`  
    `SW1(config)#` `interface Management 1`  
    `SW1(config-if-Ma1)#` `ip address 172.16.2.10 255.255.255.0`  
    `SW1(config-if-Ma1)#` `mtu 1450`  
    `SW1(config-if-Ma1)#` `exit`  
    
    ### API Config   
 
 0. Enable the restful interface to the Arista switch. 
 
    `SW1(config)#` `management api http-commands`  
    `SW1(config-mgmt-api-http-cmds)#` `no shutdown`  
    `SW1(config-mgmt-api-http-cmds)#` `protocol https`  
    `SW1(config-mgmt-api-http-cmds)#` `protocol http`  
    `SW1(config-mgmt-api-http-cmds)#` `exit`  
 
    ### SSH configuration  
 
 0. Enable SSH access to the Arista switch with the following commands, which represent best practice. 
 
    `SW1(config)#` `management ssh`  
    `SW1(config-mgmt-ssh)#` `idle-timeout 0`  
    `SW1(config-mgmt-ssh)#` `authentication mode keyboard-interactive`  
    `SW1(config-mgmt-ssh)#` `server-port 22`  
    `SW1(config-mgmt-ssh)#` `no fips restrictions`  
    `SW1(config-mgmt-ssh)#` `no hostkey client strict-checking`  
    `SW1(config-mgmt-ssh)#` `no shutdown`  
    `SW1(config-mgmt-ssh)#` `login timeout 120`  
    `SW1(config-mgmt-ssh)#` `log-level info`  
    `SW1(config-mgmt-ssh)#` `exit`  
    `SW1(config)#` `exit`  
    `SW1#` `write memory`  
   
 0. Wait a moment for write to memory to finish.
 
 0. When you're done close the console (LXTerminal).
 
 0. Right-click on SW2 and select, **Console**.
 
    > If you get an error like, "Waiting for host 127.0.0.1 ^ for interrupt", then the terminal just is waiting for you to press the ENTER key a few times.
 
 0. Okay, great. The second switch (SW2) is almost identical to the first. So just copy and paste all of the config you see below into SW2 (after connecting to its console).

    ```
    !=== SW2 ===!
    
    !--- vEOS config ---!
    enable
    configure terminal
    hostname SW2
    
    !--- MGMT ---!
    username admin secret alta3
    ip route 0.0.0.0 0.0.0.0 172.16.2.100
    interface Management 1
    ip address 172.16.2.20 255.255.255.0
    mtu 1450
    exit
    
    !--- API ---!
    management api http-commands
      no shutdown
      protocol https
      protocol http
    exit
    
    !--- SSH ---!
    management ssh
      idle-timeout 0
      authentication mode keyboard-interactive
      server-port 22
      no fips restrictions
      no hostkey client strict-checking
      no shutdown
      login timeout 120
      log-level info
    exit
    exit
    write memory
    ```

 0. Finally, let's test connectivity into the switches.

    `student@beachhead:~$` `ssh admin@172.16.2.10`

    `student@beachhead:~$` `exit`

    `student@beachhead:~$` `ssh admin@172.16.2.20`
