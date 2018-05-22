# Lab 36 - Ansible 101
### Lab Objective
Ansible is a powerful configuration management tool.
This lab allows the student to demonstrate common ansible techniques like: ad-hoc commands, gathering facts, and jinja2 templating.

We'll be using the **template module** from the Ansible library in this lab. If you want to understand Ansible, then start by reading about this module: http://docs.ansible.com/ansible/latest/template_module.html

### Procedure

 0. Take a moment to clean up your Remote Desktop. For now, close all other terminal spaces or windows you might have open.

 0. Open a new terminal, then *cd ~* to the */home/student/* directory

    `student@beachhead:/$` `cd ~`

 0. Download and run the lab setup script `setup.sh`

    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/jinja2/setup.sh -O setup.sh`  
    `student@beachhead:~$` `bash setup.sh`

 0. Ping each of the newly created lab hosts that we will be configuring with ansible. Press **CTRL+C** (CTRL and C at the sametime) to stop the ping.

    `student@beachhead:~$` `ping 10.10.1.2`  
    `student@beachhead:~$` `ping 10.10.1.3`  
    `student@beachhead:~$` `ping 10.10.1.4`  
    `student@beachhead:~$` `ping 10.10.1.5`  
    `student@beachhead:~$` `ping 10.10.1.6`  

 0. Ansible uses ssh for the transport mechanism for pyhton modules to be uploaded and executed. This means we need to setup key-based ssh access to our lab hosts. Take a peak at the `ssh-keygen` command's man page.

    `student@beachhead:~$` `man ssh-keygen`

    * what does this command do?
    * which flag do we need to used to specify an output file?
    * What does the `-N` flag do?

 0. Generate a new ssh keyfile for use with our lab hosts.

    `student@beachhead:~$` `ssh-keygen -f ansible_rsa -N ''`

 0. Having a new ssh key is great, but we need to get the public key on the remote hosts for it to be useful. Take some time to skim the `ssh-copy-id` manual page.

    `student@beachhead:~$` `man ssh-copy-id`

    * what does the `-i` flag do?

 0. Place the keyfile we just generated onto all the target hosts. You'll need to indicate **Yes** and then type the password **alta3**.

    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.2`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.3`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.4`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.5`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.6`  

 0. You should now be able to ssh into each target host without a password prompt. First SSH to **ender**, issue **ls** and then **exit**.

    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.2`  
    `root@ender:~#` `ls`
    `root@ender:~#` `exit`
    
 0. SSH to **peter**, issue **ls** and then **exit**
 
    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.3`  
    `root@peter:~#` `ls`
    `root@peter:~#` `exit`
    
 0. SSH to **valentine** and then **exit**. Issuing **ls** is now optional. The point was to proove there is nothing hiding in the home directory. That point will be important later.
 
    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.4`  
    `root@valentine:~#` `exit`  
 
 0. SSH to **bean** and then **exit**.
 
    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.5`  
    `root@bean:~#` `exit`
    
 0. SSH to **alai** and then **exit**.
 
    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.6`  
    `root@alai:~#` `exit`  

 0. Next let's create an ansible inventory file called `hosts`. Type the following:

    `student@beachhead:~$` `leafpad&`
 
 0. Copy and paste the file below into leafpad. This is our `hosts` file, and instructs Ansible how to access teh hosts we want to configure. Notice that it describes the machines IP addresses, along with where the SSH key can be found, that is used to access that machine.

    ``` ini
    [lab-hosts]
    ender     ansible_host=10.10.1.2 ansible_ssh_private_key_file=ansible_rsa
    peter     ansible_host=10.10.1.3 ansible_ssh_private_key_file=ansible_rsa
    valentine ansible_host=10.10.1.4 ansible_ssh_private_key_file=ansible_rsa
    bean      ansible_host=10.10.1.5 ansible_ssh_private_key_file=ansible_rsa
    alai      ansible_host=10.10.1.6 ansible_ssh_private_key_file=ansible_rsa
    ```

 0. Save the file in **/home/student/** as **hosts**

 0. Test connectivity (over ssh) to all of the hosts in our inventory. Just one command to ping them all.

    `student@beachhead:~$` `ansible -i hosts all -u root -m ping`

    The output should look like this:

    ```
    10.10.1.4 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    10.10.1.3 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    10.10.1.2 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    10.10.1.6 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    10.10.1.5 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    ```

 0. Ansible runs a survey python module on each host before running other tasks. This is called the `setup` module and can also be referred to as "Gathering Facts". All of these values are available as variables in your playbooks. Let's gather facts about a single node `ender`. 

    `student@beachhead:~$` `ansible -i hosts ender -u root -m setup`

    * what is the target's cpu architecture?
    * what distribution is the target linux? What version is it?
    * what version on python is installed on the target?

 0. Ansible also provides the ability to run ad-hoc commands across each host. Run a few example commands against all five lab machines and verify the output. The first command issues the command **hostname**
    
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'hostname'`

 0. This next command issues the command **whoami** across all five lab machines.
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'whoami'`
    
 0. Now we issue **ip addr** across all five machines.
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a '/sbin/ip addr'`
 
 0. Finally, let's try using ansible to issue a Python command. While 'just' a hostname will be returned, understand that it was gathered by the python command we issue! Way cool.
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'python -c "from socket import gethostname; print gethostname()"'`

 0. Try to come up with a few of your own commands to run on the target hosts. This simple one-off command style can be a very powerful information gathering tool.

 0. Our final demonstration will be with playbooks and jinja2 templates. Curl and read the playbook and template which will be deployed. First **read** the playbook, written in YAML.

    `student@beachhead:~$` `curl https://alta3.com/static/projects/ansible/jinja2/jinja-test.yml`

 0. Now **download** the playbook.

    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/jinja2/jinja-test.yml`

 0. Now **read** the jinja2 template

    `student@beachhead:~$` `curl https://alta3.com/static/projects/ansible/jinja2/jinja-test.j2`

 0. Now **download** the jinja2 template

    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/jinja2/jinja-test.j2`

 0. Next, run the playbook:

    `student@beachhead:~$` `ansible-playbook -i hosts jinja-test.yml`
 
 0. Okay, so what just happened? To start, read through the results. It looks like we're being told that **alai**, **bean**, **ender**, **peter**, **valentine**. 
 
 0. SSH into **ender** and check out the ~/nursery.ini file.
 
    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.3`
 
 0. Display the home directory to discover **nursery.ini** (this wasn't here before)
 
    `root@peter:~$` `ls`
 
 0. Display **nursery.ini**
 
    `root@peter:~$` `cat nursery.ini` 
 
 0. Exit from **peter**
 
    `root@peter:~$` `exit` 
 
 0. Wait. We just learned that we could run an ad-hoc command to display the output on all the systems. Duh.
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'cat ~/nursery.ini'`
 
 0. Interesting. They all now have that **~/nursery.ini**. So what happens if we run the same command again? They'll have two files? Just the single one? Let's find out. Once again, run the playbook:
 
    `student@beachhead:~$` `ansible-playbook -i hosts jinja-test.yml`
 
 0. Appreciate the results. No changes occured. Ansible is that good.
 
 0. Change the values of the variables in the jinja-test.yml file. Open it in leafpad (it's in /home/student/jinja-test.yml), change the word **lamb** to **alpaca**.
 
 0. Change the word **snow** to **cottonballs**
 
 0. Choose **File > Save**
 
 0. Once again, run the playbook:
 
    `student@beachhead:~$` `ansible-playbook -i hosts jinja-test.yml`
 
 0. Re-examine the output on the target machines to verify the changes were deployed.
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'cat ~/nursery.ini'`
    
 0. Awesome. Every single machine had their ~/nursery.ini file changed. Think what that means for system administrators? Key / password updating, config changes, firewall changes, removing old users. Every machine. Right away. Although, if we can set up machines this quickly... maybe it's not even worth retrofitting / modifying old machines. Maybe it's better just to start fresh every single time.
 
 0. Run the playbook again. Maybe this time it's more clear. How does the output of the playbook running differ if you don't change any variables?
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'cat ~/nursery.ini'`
 
 0. If all you needed was a proof of concept, then you've made it. Skip to the last step (*teardown.sh*). If you're looking to test your understanding of Ansible (i.e. you're expected to use it), then keep reading.
 
 0. Time for YOU to develop a playbook which will deploy the following `ovs.j2` file into `/tmp/ovs.ini`.  You can use the jinja-test.yml playbook as a starting point. First read **ovs.j2**
 
    `student@beachhead:~$` `curl https://alta3.com/static/projects/ansible/jinja2/ovs.j2`  
 
 0. Now download it.
 
    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/jinja2/ovs.j2`  
    
 0. Make sure these values get deployed into the resulting `ovs.ini` file. You will accomplish this by **ONLY** editing your playbook. The playbook is the file **jinja-test.yml**. Start by replacing the 'mary had a little lamb' vars, with the mappings provided below:  
 
    * `tunnel_types: vxlan`
    * `bridge_mappings: external:br-ex`
    * `firewall_driver: iptables_hybrid`
    * `external_network_name: super_big_net`
    * `ovs_external_bridge_name: my_ovs_bridges`
    * One mapping is missing from this list! Run the playbook, watch it fail, then add the missing mapping to the playbook and it should work!
 
 0. Lost? You're next step is to edit **jinja-test.yml** and make it use **ovs.j2** *it currently uses jinja-test.j2*. This can be modified in the **src:** line.
 
 0. Now change the destination to where we want the file to appear on the target system **/tmp/ovs.ini**
 
 0. If you haven't already, be sure you have rewritten your **vars:** so the keys found in your playbook match those found in **ovs.j2**
 
 0. After completion, **File > Save**.
 
 0. Run the playbook!
 
    `student@beachhead:~$` `ansible-playbook -i hosts jinja-test.yml`
 
 0. If it worked, you should be able to run the command below to view `/tmp/ovs.ini` on all `lab-hosts` systems.
 
    `student@beachhead:~$` `ansible -i hosts all -u root -m command -a 'cat /tmp/ovs.ini'` 
 
 0. Ready to go even further? Check out the Ansible module page, and see if you can figure out how to use a module or two. Copy is a good one to start with. *To note, copy always references the source as the machine you're running the playbook from.* http://docs.ansible.com/ansible/latest/list_of_all_modules.html You might also try removing the **ovs.ini** or **nursery.ini** files that are on our target systems.
 
 0. Great job! Teardown your lab

    `student@beachhead:~$` `bash teardown.sh`
