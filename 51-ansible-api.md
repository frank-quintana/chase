# Ansible API

The goal of this lab is to become familiar with the ability to run ansible playbooks from python code.

### Procedure

 0. Take a moment to clean up your Remote Desktop. For now, close all other terminal spaces or windows you might have open.

 0. Open a new terminal, then *cd ~* to the */home/student/* directory

    `student@beachhead:/$` `cd ~`

 0. Download and run the lab setup script `setup.sh`

    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/jinja2/setup.sh -O setup.sh`  
    `student@beachhead:~$` `bash setup.sh`

 0. Place the keyfile generated previously onto all the target hosts. You'll need to indicate **Yes** and then type the password **alta3**.

    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.2`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.3`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.4`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.5`  
    `student@beachhead:~$` `ssh-copy-id -i ansible_rsa root@10.10.1.6`  

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

 0. Save the file in **/home/student/bin/** as **hosts**

 0. Download and inspect the ansible example api script `api.py`

    `student@beachhead:~$` `wget https://alta3.com/static/projects/ansible/api/api.py -O api.py`  

    * What will this script do?
    * Which hosts will it run against?
    * How will the output be displayed?

 0. Run the `api.py` script

    `student@beachhead:~$` `python api.py`  

 0. Update the script to run against all of the 5 hosts by updating the line below:

    `host_list = ['ender']`

    Change `ender` to `all` and re-run the playbook.

    `student@beachhead:~$` `python api.py`  

 0. Update the script to run a playbook which installs `sl` via apt on all 5 hosts.  
    After completing this update and running your new playbook verify the change.

    `student@beachhead:~$` `ssh -i ansible_rsa root@10.10.1.2`  
    `root@ender:~#` `sl`

 0. Great job! Teardown your lab

    `student@beachhead:~$` `bash teardown.sh`
