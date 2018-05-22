# Using & Installing Python

### Lab Objective

The objective of this lab is to learn how to install and use Python within your environment. The first few steps verify the Python 2.x and 3.x installations on your remote desktops. The closing steps are informational, and we encourage you to try installing Python 2.x and/or 3.x in your local environment (provided you have permissions).

### Procedure

0. Python 2.7 and 3.5 is installed on Ubuntu 16.04 by default. Let's verify that Python 2 and 3 are installed. Start by opening a command prompt.

0. In the new command window, type:

    `student@beachhead:/$` `cd ~`

0. This moved you to the user student's home directory. Try installing python.

    `student@beachhead:~$` `sudo apt install python -y`

0. Python should already be installed. We are just showing how to do it.

0. Launch Python 2.7.

    `student@beachhead:~$` `python`

0. Your screen should now look like the following:

    ![python-27](https://alta3.com/static/images/python/python_install_001.png)
    
0. If your Python version is 'newer' than 2.7, that's okay. Exit the Python 2.7 environment with the exit function.

    `>>>` `exit()`

0. Try installing Python 3.x.

    `student@beachhead:~$` `sudo apt install python3 -y`

0. Launch Python 3.x.

    `student@beachhead:~$` `python3`

0. Try out the `print()` function.

    `>>>` `print('Just testing')`

0. Again, if the Python version is newer than 3.5, that's okay. Exit the environment with the exit function.

    `>>>` `exit()`
    
0. The future of Python is 3.x, however there is plenty of working 2.x (legacy) code in the world. Throughout the rest of the Python labs we'll learn about both versions.

0. Let's check on the version of Python 2.x installed without entering the Python 2.x environment.

    `student@beachhead:~$` `python --version`
    
    ![python-2-version](https://alta3.com/static/images/python/python_install_003.png)

0. Let's check on the version of Python 3.x installed without entering the Python 3.x environment.

    `student@beachhead:~$` `python3 --version`

    ![python-3-version](https://alta3.com/static/images/python/python_install_004.png)

0. Instructions to install Python on a Windows platform have been included with the class manual- however, it is a fairly straightforward process. The latest Python 2 and 3 builds for Windows, Mac OS X, Linux/UNIX are available at: https://www.python.org/downloads/

0. Anything you type in the Python interpereter is saved (written out when you `exit()`). Let's take a look at the python interpreter history file so far.

    `student@beachhead:~$` `cat .python_history`

0. We'll study pip later, but for now, make sure that the module is available to python3.

    `student@beachhead:~$` `sudo apt install python3-pip -y`
    
0. Good job, that's it for this lab. Before proceeding, you might want to take a moment and install 3.x (and maybe 2.x) on your local machine. *Note: If you are using a corporate laptop, it is possible that your local security settings will not allow the installation of Python.*

#### Additional Learning / References

The following are a list of pages we thought might be helpful for our students to know about:

* [Download the Latest versions of Python](https://www.python.org/downloads/)
