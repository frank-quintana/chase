# Using leafpad

### Lab Objective

In addition to vim, you may also use the leafpad text editor. It is not difficult to use. Let's practice launching it now.

### Procedure

0. From your remote desktop, open a terminal session.

    ![Open terminal session](https://alta3.com/static/images/python/python_vim_001.png) 

0. Move to the student home directory.

    `student@beachhead:/$` `cd`

0. Now create a text file within the leafpad environment, running it in the background.

    `student@beachhead:~$` `leafpad &`

0. Leafpad will open. Type a few sentences. Be sure to include some carriage returns, like the following:

    >
      I'm Guybrush Threepwood!
    >
      I want to be a mighty pirate!
    
0. Select: File > Save As 

0. As a general rule, always save to the `/home/student` directory. Try saving this file as `monkey.island`

0. Close leafpad.

0. Back in the terminal window, type:

    `student@beachhead:~$` `cat monkey.island`

0. The text you saved should appear on the screen.

0. Edit `/home/student/monkey.island` with leafpad.

    `student@beachhead:~$` `leafpad monkey.island`

0. Add the following text:

    >
      I'm Guybrush Threepwood!
    >
      I want to be a mighty pirate!
    >
      And defeat the dread ghost pirate LeChuck!
  
0. Select: File > Save

0. Exit leafpad.

0. Back in the terminal window, type:

    `student@beachhead:~$` `cat monkey.island`

0. The text and your changes should appear.

0. Remove the file.

    `student@beachhead:~$` `rm monkey.island`

0. There is a quick launch for leafpad in the lower left-hand of your screen. It looks like a tiny green leaf. Try clicking it.

0. Select: Options > Font

0. Try increasing the font size. Hit `Ok` when done.

0. Select: Options > Line numbers

0. Cool! Line numbers can make troubleshooting code easier.

0. Great! Now close leafpad.

0. For most labs, you may use vim or leafpad. Both are 'just' text editors we selected from a sea of possible text editors. If an Alta3 lab says use vim, or use leafpad, it may likely be easier to complete the task using that method.
