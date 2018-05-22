# Install GNS3 

### Lab Objective

Build out emulated networking gear known as GNS3.

### Procedure

 0. Take a moment to clean up your Remote Desktop. For now, close all other terminal spaces or windows you might have open.

 0. Open a new terminal, then change directory to the `/home/student/` directory

    `student@beachhead:~$` `cd ~`
    
 0. The next script with add a repository, run apt-get to install updates and packages, and get the necessary installation files. It will install GNS3, and create a directory for you callled, `~/napalm-net/` where we'll save our GNS3 work. 
 
    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/gns_setup.sh -O gns_setup.sh`
    
 0. Now run the setup script to install GNS3 in the Alta3 remote desktop environment.
 
    `student@beachhead:~$` `sh gns_setup.sh` 

 0. Twice during this process, you'll need to press the **ENTER** key (when prompted). When the script finishes, you'll see a checksum value, ensure the one printed on your screen matches the one below.
 
    > EXPECTED RESULT: 
    > `ae8ef61b656cb7a9cd4e1c39eee2af36e60e0bdc00a204ff418d60097ccb2b8352a93d561089876bccd3cc5343002d1b605fcfaf01a3d77685fdd34d3bf25d8f  /home/student/napalm-net/vEOS-lab-4.17.8M.vmdk`

 0. Next we will start up GNS3 and get it configured. 
 
    `student@beachhead:~$` `sudo gns3 &`  
  
    > ignore the warning about running gns3 as root. Sometimes there is also a warning about not being able to connect to the gns3 server. We havent set the server up yet so that warning is not useful information right now. From the Help dropdown of GNS3, open the setup wizard to begin configuring GNS3. 
 
 
 0. Setup Wizard > Run the topologies on my computer > Next
    
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_1.png)
    <br>    
 0. Setup Wizard > Local Server Config > NO CHANGES > Next  
     
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_2.png)
    <br>    
 0. Setup Wizard > Local Server Status > Connection Successful > Next
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_3.png)
    <br>
 0. Setup Wizard > Summary > Finish
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_4.png)
    <br>
 0. New Appliance Template > Import an appliance template file
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_5.png)
    <br>
 0. Open appliance > Choose napalm-net folder > double click the `arista-veos.gns3a` file
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_6.png)
    <br>
 0. Add appliance > Summary of appliance > Next
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_7.png)
    <br>
 0. Add appliance > Server > Select Run the appliance on your computer > Next
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_8.png)
    <br>
 0. Add appliance > Requirements OK you can continue, (thanks for your permission computer) > Next 
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_9.png)
    <br>
 0. Add appliance > Required files > Choose vEOS 4.17.8 > Next
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_10.png)
    <br>
 0. Appliance >  Would you like to install vEOS 4.17.8 > Yes
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_11.png)
    <br>
 0. Add appliance > Qemu binary NO CHANGES > Next
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_12.png)
    <br>
 0. Add appliance > Summary > Next
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_13.png)
    <br>
 0. Add appliance > Usage: note the username/password > Finish
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_14.png)
    <br>
 0. Add appliance > Installed! > OK
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_15.png)
    <br>
 0. Project > New Project > Name: NAPALM-NET, Location: NO CHANGES > OK
 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_up_GNS3/Setup_gns3_wizard_16.png)
    <br>

 0. Now we will fix GNS3 console launcher 

 0. Edit > Preferences > General > Console applications > Console settings > Edit. 
 Then change it to: `lxterminal -t "%d" -e "telnet %h %p"` > OK
    
    ![](https://alta3.com/static/images/Napalm_Install/Fix_GNS3_console_launcher/Fix_launcher_1.png)
    <br> 
 
 0. Apply > OK
