# Making a Github account

### Lab Objective
In this lab, we're going to explore SCM, or Software Control Management platforms. Git is the defacto tool for tracking / version controlling your work. Git is a tool that we run on our local machine, GitHub is a HTTPS browser-friendly platform that syncs with git, and makes your code available to the world. This is a good thing.

In this lab, you'll make a Github account. This is free, and will allow you to save the code you develop this week.

### Lab Procedure

0. Open the Firefox browser from within the remote desktop.

0. Webpages change daily, so ask the instructor for help if the instructions seem off, but we'll do our best to describe the process.

0. Navigate https://github.com

0. Click on `sign-up` on the landing page.

0. Enter the following information.

  - **Username** - This handle, will be shared with career professionals.
  - **Email Address** - Use a email address you check often.
  - **Password** - Always make passwords unique, and ultimately changae them often.

0. Now click on the **Create an account** button at the bottom of the screen

0. Choose `Unlimited public repositories for free`

0. At the bottom of the screen, now click the green `Continue` button

0. Answer the questions as best you can, to help the github metrics, and then click `Submit` at the bottom of the screen.

0. You'll need to verify your email address. So go check the email address you used to sign up, and click on whatever link or button they sent to you.

0. Now that we have a verified GitHub account, let's try tracking some work. In the upper right hand corner, click on your profile icon, then on `Your Profile`

0. If you'd like, you can set a bio and a profile. This account is public, and the world will look at your code, so be professional.

0. In the center of the screen, click on the word `Repositories`, which is followed by a `0`

0. Click on the green `New` button, with the little book icon on it. This creates a repository. Repositories track work.

0. Set the **Repository name** as `mycode`

0. Set the **Description** as something like, `Learning to track my code`

0. The repository should be set to **Public**

0. The README option is always best practice, so check the box. The objective is to describe what your repo contains.

0. Change `Add a license: None` to `GNU General Public License v3.0`. Here's the TLDR, "We're comfortable with the world borrowing our code, provided they give us credit, should it get used."

0. Now click the green `Create repository` button at the bottom of the screen.

0. You rnew repo will appear. Click on the file, `README.md`.

0. You'll find a tiny pencil on the right side of the screen, click on this.

0. Copy and paste the following template into your **README.md**, then customize it. The goal is to advertise to the world what the purpose of your repository is. Don't hold back. If you're a 1st time student, interested in Python and Network Automation-- let the GitHub community know!

0. FYI, files on GitHub are written in **markdown** which is a web-friendly way to format text. Read about it here: https://guides.github.com/features/mastering-markdown/

        # mycode (Project Title)
        
        One Paragraph of your project description goes here. Describe what you're trying to do. What is the purpose of putting up this repo?
        
        ## Getting Started
        
        These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.
        
        ### Prerequisites
        
        What things you need to install the software and how to install them. For now, maybe copy in how to install python and python3 using apt.
        
        ```
        Give examples
        ```
        
        ### Installing
        
        A step by step series of examples that tell you have to get a development env running. Eventually, maybe we could list Python package requirements here. You can remove this section for now, or make a note to yourself to fill it out later.
        
        ```
        Give the example
        ```
        
        And repeat other install steps...
        
        ```
        until finished
        ```
        
        End with an example of getting some data out of the system or using it for a little demo
        
        ## Running the tests
        
        Explain how to run the automated tests for this system. We don't have any, so just make a note that they are none at this time.
        
        ## Deployment
        
        Here we might put additional notes about how to deploy on various systems. For now, put a note that we're testing our code on Linux Ubuntu 16.04 LTS. 
        
        ## Built With
        
        * [Python](https://www.python.org/) - The coding language used
        
        ## Contributing
        
        Here you could make notes about how others might contribute to your code. For right now, a friendly message saying you'll accept any and all help from other coders would be great!
        
        ## Authors
        
        * **Your Name** - *Initial work* - [YourWebsite](https://example.com/)
        
        ## License
        
        This project is licensed under the GNUv3 - see the [LICENSE.md](LICENSE.md) file for details
        
        ## Acknowledgments
        
        * Hat tip to anyone who's code was used
        * Inspiration
        * etc
 
0. When you finish, click the green `Commit changes` button at the bottom of the screen.

0. Generally, we don't want to use Github as a development tool. Just for viewing. But to get our README.md started, this is fine. From now on, we'll only add to our repo from the command line.

0. At the top of the screen, click on your username to return to your homepage.

0. You should see your repository `mycode`

0. Try following the Alta3 account, and one of the Alta3 Instructors. Navigate to: https://github.com/alta3

0. On the left hand side of the screen, click follow.

0. Navigate to: https://github.com/rzfeeser

0. On the left hand side of the screen, click follow.

0. You'll now see activity by the owners of these accounts. You might try asking some fellow students for their usernames.

0. Now we want to sync our command line (where we run git) with GitHub. The goal is to get our code to end up in this **mycode** repo we created.

0. Open a new terminal window.

0. The first thing we'll need to do is generate a new RSA key (essentially a password) and sync this key with GitHub. This will prevent us from having to always type our password.

    `student@beachhead:/$` `cd /home/student/.ssh`
    
0. Create a new unique keypair to use.

    `student@beachhead:~/.ssh$` `ssh-keygen`
    
0. When you are prompted for the file name type `/home/student/.ssh/id_rsa_github`

0. Keep hitting enter throughout this process until the pair has been created.

0. Print out to the screen your new public key. We'll need to paste it into GitHub.

    `student@beachhead:~/.ssh$` `cat id_rsa_github.pub`

0. Copy everything that was just printed to the screen to your clipboard (highlight, right-click, copy)

0. Back in GitHub, click on your profile icon on the top-right corner.

0. Select **Settings** from the drop down menu.

0. Select **SSH and GPG Keys** on the left

0. Click the green **New SSH Key** button

0. Paste the key from your clipboard into Github.

0. Click the **Add** button.

0. You'll likely need to input your GitHub password to complete adding a new SSH Key.

0. Check your email and confirm that your key has been added. If your key was formatted incorrectly, it will be rejected immediately or via email. If your key is rejected, try again or ask the instructor for help.

0. Back in your terminal, move back to your home directory.

    `student@beachhead:~/.ssh$` `cd /home/student/`
    
0. Let's add the newly generated ssh key to our ssh-agent. This means it will be loaded and used automatically when we attempt to make ssh connections, i.e. to github.
   
   `student@beachhead:~$`  `ssh-add ~/.ssh/id_rsa_github`
   
0. Use this next command to verify that our new key has been loaded and is ready to use.

   `student@beachhead:~$`  `ssh-add -l`

0. Fantastic. Let's set up `git` for firsttime use.  Replace the name and email address with **your own!**

    `student@beachhead:~$` `git config --global user.name "Mona Lisa"`
    `student@beachhead:~$` `git config --global user.email "monalisa@alta3.com"`
    
0. Test that your new key works with github.  You should get a friendly message saying your key worked.

    `student@beachhead:~$` `ssh git@github.com -T`

0. Customize the command below so that <github-username> is replaced with your github username. The angle brackets are **not** needed.

    `student@beachhead:~$` `git clone https://github.com/<github-username>/mycode.git`
  
0. You should now have a new folder (mycode) in your home directory. Move into it.

    `student@beachhead:~$` `cd /home/student/mycode/`
    
0. Let's edit README.md

    `student@beachhead:~/mycode$` `leafpad /home/student/mycode/README.md`

0. At the top of your **README.md** file, within your project description, add a sentence about wanting to learn how to version control projects with git.

0. Select: **File > Save**

0. Exit leafpad.

0. The following steps are ones you should memorize. You'll be issuing these commands all the time. First issue `git status`, which will reveal if you added something and forgot about it.

    `student@beachhead:~/mycode$` `git status`

0. Next we'll describe what we want to add to our repo. We'll wildcard this, so everything in the `~/mycode/` directory is added.

    `student@beachhead:~/mycode$` `git add *`
    
0. Time to perform a commit. This makes a new version.

    `student@beachhead:~/mycode$` `git commit -m "First commit to learn about version controlling"`

0. Now push your new version to Github for tracking.

    `student@beachhead:~/mycode$` `git push origin master`

0. Huh. It asks for a username & password (the expection was that the key would take care of this). For now, go up to Github, and see if your README.md file has changed. **Remember, we're don't want to edit any files in GitHub anymore.** Just validate GitHub has the new data.

0. You should get in the habit of issuing the following commands each time you have a success, or end for the day.

    - `git status`
    - `git add /home/student/mycode/*`
    - `git commit -m "reason for commit"`
    - `git push origin master`
    - Type in username & password
  
0. Becuase we took the time too set up a keypair with Github, we can actually tweak that process. Currently, origin points to the https url: https://github.com/<username>/<project>.git , rather than the ssh url: git@github.com:<username>/<project>.git
  
0. We can change where origin points with the following command (from the `/home/student/mycode/` directory). Replace `<username>` with your GitHub username (suchas `alta3`).
  
      `student@beachhead:~/mycode$` `git remote set-url origin git@github.com:<username>/mycode.git`

0. That's it! Now when you push to GitHub, you shouldn't be prompted for a username or password.

0. If you ever run into a problem using git / GitHub, let the instructor know. It is critical to start learning to version control code.

0. You might want to spend some time clicking around on the following guides. They're quite short, and although you might not understand what they're talking about, they'll begin exposing you to the way we 'speak' when using git & verson controlling software. Some getting started guides relating to GitHub can be found here: https://guides.github.com/
