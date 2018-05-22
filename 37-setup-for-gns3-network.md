# Setup for GNS networking

### Process

 0. The following are steps we need to issue on our host machine (beachhead) in order to ensure our GNS3 application has access to our local host. The following steps are examples of network function virtualization, using OpenVSwitch. Unfortunately, we can't go terribly deep into this topic within this course, but we left them in (rather than script them), because we thought some people might actually enjoy seeing these types of changes being made. 

 0. Do NOT close GNS3, but open a new terminal.
 
 0. Change to the student home directory `/home/student/`.
 
    `student@beachhead:/$` `cd ~`
 
 0. Download and run the ovs.sh script to setup networking for your machine.
 
    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/ovs.sh -O ovs.sh`  
    `student@beachhead:~$` `bash ovs.sh`

 0. If you run into problems, you can confirm that naplam-net has been correctly configured.

    `student@beachhead:~$` `sudo ovs-vsctl show`

    > This shows what it should look like. Note, that the order your ports appear in may be different.
 
    ```
    Bridge napalm-net
        Port "tap0"
            Interface "tap0"
        Port "host0"
            Interface "host0"
                type: internal
        Port napalm-net
            Interface napalm-net
                type: internal
    ovs_version: "2.5.4"
    ```

 0. Those steps gave connectivity to the GNS3 environment running on our local host. That will be critical, if we're eventually going to drive our virtualized appliances with Python or Ansible.

 0. Click on the "Browse all Devices" icon (looks like 4 icons in one button) > Click the dropdown and choose "Installed appliances".
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_1.png)     <br>
 0. Click and drag the cloud icon the center area.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_2.png)     <br>
 0. Click the sidebar icon that looks like two arrows (Browse Switches on hover).
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_3.png)     <br>
 0. Click and drag the "AristavEOS4" into the center area. Do this twice.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_4.png)
    <br>
 0. Click and drag the "Ethernet Switch" to the center area, and note the Topology Summary to the right. This becomes important to us shortly. 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_5.png)
    <br>
 0. Click and drag the VPCS icon into the center area. Do this twice. 
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_6.png)
    <br>
 0. Click on the sidebar icon that looks like an ethernet cable.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_ethernet_snake.png)
    <br>
 0. Left click your ethernet connection on the cloud, and select tap0, then connect it to ethernet0 within Ethernetswitch-1. Continue connecting devices until your topology looks like the one below:
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_eth_cable_look_like_this.png)

 0. Hold the `Ctrl` key and click on both vEOS switches. Now right-click on either on of the switches > Click Configure > Advanced Settings.  
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_8.png)
    <br>
 0. Edit the Additional Settings to be `-nographic -machine smm=off`.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_10.png)
    <br>    

 0. Click Apply > Click OK.
    > This fixes a [known issue with the bootup of vEOS switches](https://www.gns3.com/qa/arista-veos-4-20-wont-boot-prope)
    > If you get an error like this right click on the switch and reboot it. 
    `[  197.324266] RIP  [<ffffffff8105d6df>] finish_task_switch+0x86/0x2e5`
    `[  197.324266] Kernel version: 3.18.28.Ar-6765725.4 #1 SMP PREEMPT Wed Nov 15 09:47:13 PST 2017`

 0. Right click on the first switch > Start.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_14.png)
    <br>
 0. Right click on the the first switch > click Console.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_open_switch_console.png)
    <br>
 0. Right click on the second switch > Start.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_14.png)
    <br>
 0. Right click on the the second switch > click Console.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_open_switch_console.png)
    <br>
 0. Boot time should be about 7 minutes. Take a break. When you come back, you'll have to two viritualized production switches up and running.
    ![](https://alta3.com/static/images/Napalm_Install/Setting_UP_GNS_Networking/Build_a_network_topology/network_top_16.png)
    <br>
 0. Login as admin on both, no password.

    `localhost login:` `admin`  
    `localhost>`
    
 0. Congrats! You deployed a topology.
