# vscode-integration 
A setup guide to get started on VS Code and IBMi. This guide includes, 

1. Installing the required extensions
2. Setup your local folder
3. Connect to the IBMi
4. Coding in VS Code
5. Compiling in VS Code
6. Terminal (TN5250)

# First things first
It is best to sync the settings of your VS Code preferences to your company's GitHub account. So that when you reinstall VS Code (for some weird reasons), you  will have an option to reset your preferences and plugins. Follow the below steps to get started.

1. Open VS Code and click on the accounts icon on the bottom left

   ![1](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/502aa3eb-eb38-490e-a335-b9024bb68006)


2. Click "Turn on Settings sync". You will be prompted with the list of settings you want to sync. Tick whatever settings you want to sync. I prefer to sync them all.
   
3. Click "Sign in & Turn on" >> Click Sign in with GitHub.
   
   ![2](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/b4744a5a-1c36-48bf-9581-d9cd85f5e0d9)



4. Click open on the browser window (that just got opened)
   
   ![3](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/374802bf-7a8c-4ff1-9a4f-62b1128af7ba)


5. Click open again inside VS Code and you're all set.

   ![4](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/339ec392-b124-4d45-bedb-ee98a1eeaad2)


# 1) Install the required extensions.
1. Go to the extensions panel (Ctrl + Shift + X) and install the extension "IBM i Development Pack". This is a collection that contains 13 extensions useful for coding in IBMi.

   ![5](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/9c34aa29-5645-4394-933d-613f936e07fa)

   Most of the extensions are open source, except for the ones created by IBM. You can view about the authors and the respective licenses on the [market place page.](https://marketplace.visualstudio.com/items?itemName=HalcyonTechLtd.ibm-i-development-pack)

3. If you're having any issues installing the plugin, you need to make the below changes. Click the bottom left corner as shown in the screenshot below. 

    ![6](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/17982f39-5045-483e-95b8-a5bec8162209)

    On the right side pane, click "Trust Window" (mine is already trusted)

# 2) Setup your local folder
It is important to setup a local folder if you wish to keep a copy of the sources in your local machine as a backup.

1. Go to the Explorer Tab (Ctrl + Shift + E) and click on "Open Folder".
   
2. Navigate to your local folder. For this tutorial, I have created a new folder called "MySourceCodes". Be sure to "Trust the authors of the files in this folder" as shown below.

   ![7](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/3cf60692-32d9-47c6-b715-45e328c0f96e)


# 3) Connect to IBMi
1. It is important to have SSH installed in our IBM server. SSH, SFTP, and related programs are provided by the [5733-SC1 Licensed Program Product](https://www.ibm.com/support/pages/node/1128123/) by default, but it doesn't hurt to check.
   
   In your IBMi, check whether SSH Server is already started. Run the CL command `NETSTAT *CNN` and search for any activity on port 22. If you don't see any activity then you can start SSH with the CL command `STRTCPSVR *SSHD`. Typing `WRKACTJOB` should now show jobs running function “PGM-sshd.” To end SSH, type `ENDTCPSVR *SSHD`.

2. Once the SSH daemon is started on your IBMi head back to VS Code. Now you should see a new IBMi icon present on the left most panel. Click on it and select "Connect to an IBM i" button.
   ![8](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/52aa4180-9910-487f-bdd8-90938fbf077d)

  
3. Enter the connection details such as connection name, IP address, User Name & Password and click connect. 

   ![9](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/dc3bcb89-0b24-4c79-a021-9114ac8d5ef0)


4. Once a connection is made, depending upon your IBMi server configuration, you might or might not see some notifications/warnings/error messages at the bottom right like below. If you get multiple notifications and they all disappeared before you read them fully, fret not. All the notification will reside in the bottom right "bell" icon. We will go through one by one.

   ![10](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/cc4c759a-9d2f-4ee1-94f9-059ee6c477f7)


      a. **Debug PTF Installed**
   
   This means your IBMi has the debug PTF installed in it. We can make use of this PTF to debug the programs from outside of IBMi (In our case it is VS Code). Click this [link](#) to see how to setup debug in VS Code.

      b. **Current Library is set to XXXXXX**
   
   We can setup the current library for this VS Code connection to our liking. You can either change now or if you decide to change later, you can do so by accessing the "User Library List" pane on the left.
         ![11](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/def6aecc-59a7-4e77-8438-18f02eb87334)

   

      c. **IBM recommends using bash as your default shell.**

   You can set the bash as your default shell for this session instead of the usual QSH (QShell). Why? Because BASH has many more cooler features and command history (using up arrow keys to access previous commands) is one of them. If you wish to do later, you can do so by issuing `/QOpenSys/pkgs/bin/chsh -s /QOpenSys/pkgs/bin/bash` command (Ctrl + Shift + J to open PASE Terminal from VS Code) or you may refer to this [guide](https://ibmi-oss-docs.readthedocs.io/en/latest/troubleshooting/SETTING_BASH.html)

      d. **Deploy Directory**

   You can set you default deploy directory to the specified IFS folder. Remember the previously created folder "My Source Codes"? Whatever code you write and save in this location will have the ability to be pushed to your IBMi IFS folder that you specify in the "Deploy Directory". Once the source code has come to your IBMi you can easily issue a `CPYFRMIMPF` CL command to copy the sources to your IBMi libraries.

      e. **QCCSID is set to 65535. Disabling SQL support.**

   In order to have the ability to run SQL Queries from within VS Code, we need to set the CCSID of this session to 37. If you get any error related to SQL Queries and CCSID, then you need to head over to your IBMi and set the CCSID as 37. The default is 65535.

      f. **Code for IBM i may not function correctly until your user has a home directory...**

   In order to have a deploy directory(where your source codes will be deployed), it is must to create your own user profile directory inside the /home directory in the IFS. You might be shown with this warning message if the home directory is not set correctly. The fix is to create a user_profile directory. If you're comfortbale with Shell command, you can use PASE (Ctrl + Shift + J from VS Code) to create a directory. The command is `mkdir /home/<your_ibmi_username>`. If you're comfortable with IBMi green screen, then you can issue a `MKDIR DIR('/home/<your_ibmi_username>')` CL command. I would suggest you to use lower case while creating your directory as UNIX Shell is case sensitive. 
   





**Further Reading**
_[Since VS Code is using SSH to connect with IBMi, it is better to run the SSHD in a separate subsystem for better management of resources](https://www.ibm.com/support/pages/starting-ssh-daemon-dedicated-subsystem-environment)_

**To edit**
1. Starting SSH Daemon
2. Connecting the IBMi mulitple times. 
