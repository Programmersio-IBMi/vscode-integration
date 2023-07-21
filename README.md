# vscode-integration
A setup guide to get started on VS Code and IBMi. This guide includes, 

- Installing the required extensions
- Setup your local folder
- Connect to the IBMi
- Coding in VS Code
- Compiling in VS Code
- Terminal (TN5250)

# First things first
It is best to sync the settings of your VS Code preferences to your company's GitHub account. So that when you reinstall VS Code (for some weird reasons), you  will have an option to reset your preferences and plugins. Follow the below steps to get started.

1. Open VS Code and click on the gear icon on the bottom left

   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/a5da6327-44be-4acd-82ba-72acafc4e710)

2. Click "Turn on Settings sync". You will be prompted with the list of settings you want to sync. Tick whatever settings you want to sync. I prefer to sync them all.
   
3. Click "Sign in & Turn on" >> Click Sign in with GitHub.
   
   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/c23ea382-6db1-4bb5-838f-e23654344f1b)


4. Click open on the browser window (that just got opened)
   
   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/833d02b6-c24d-4351-9a82-7fcdc10abf42)


5. Click open again inside VS Code and you're all set.

   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/cade90b4-b908-4df7-9b68-4326942fc16f)

# Install the required extensions.
1. Go to the extensions panel (Ctrl + Shift + X) and install the extension "IBM i Development Pack". This is a collection that contains 13 extensions useful for coding in IBMi.

   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/9283fecb-376a-417a-9664-dc87d8c5fd28)

   Most of the extensions are open source, except for the ones created by IBM. You can view about the authors and the respective licenses on the [market place page.](https://marketplace.visualstudio.com/items?itemName=HalcyonTechLtd.ibm-i-development-pack)

3. If you're having any issues installing the plugin, you need to make the below changes. Click the bottom left corner as shown in the screenshot below. 

    ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/1a190d14-1e02-43a5-a890-82b9c6da5c0c)
    On the right side pane, click "Trust Window" (mine is already trusted)

# Setup your local folder
It is important to setup a local folder if you wish to keep a copy of the sources in your local machine as a backup.

1. Go to the Explorer Tab (Ctrl + Shift + E) and click on "Open Folder".
   
2. Navigate to your local folder. For this tutorial, I have created a new folder called "My Source Codes". Be sure to "Trust the authors of the files in this folder" as shown below.

   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/908f2668-b951-4fe5-9bb2-482fb18cf5a9)

# Connect to IBMi
1. It is important to have SSH installed in our IBM server. SSH, SFTP, and related programs are provided by the [5733-SC1 Licensed Program Product](https://www.ibm.com/support/pages/node/1128123/) by default, but it doesn't hurt to check.
   
   In your IBMi, check whether SSH Server is already started. Run the CL command `NETSTAT *CNN` and search for any activity on port 22. If you don't see any activity then you can start SSH with the CL command `STRTCPSVR *SSHD`. Typing `WRKACTJOB` should now show jobs running function “PGM-sshd.” To end SSH, type `ENDTCPSVR *SSHD`.

2. Once the SSH daemon is started on your IBMi head back to VS Code. Now you should see a new IBMi icon present on the left most panel. Click on it and select "Connect to an IBM i" button.
   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/d9a096e3-a4de-4d74-8f63-04135cecfc16)
  
3. Enter the connection details such as connection name, IP address, User Name & Password and click connect. 

   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/2057b033-d16a-4e85-905e-9665bdfed2fd)

4. Once a connection is made, depending upon your IBMi server configuration, you might or might not see some notifications at the bottom left like below. We will go through one by one.

   ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/a4c94ffc-6e04-475b-a27f-1ad6b7192ce0)

      a. **Debug PTF Installed**
   
   This means your IBMi has the debug PTF installed in it. We can make use of this PTF to debug the programs from outside of IBMi (In our case it is VS Code). Click this [link](#) to see how to setup debug in VS Code.

      b. **Current Library is set to XXXXXX**
   
   We can setup the current library for this VS Code connection to our liking. You can either change now or if you decide to change later, you can do so by accessing the "User Library List" pane on the left.
         ![image](https://github.com/Programmersio-IBMi/vscode-integration/assets/139198015/5687de01-26c0-4b9a-98e0-670f3466e39b)
   

      c. **IBM recommends using bash as your default shell.**

   You can set the bash as your default shell for this session instead of the usual QSH (QShell). Why? Because BASH has cooler features like command history (using up arrow keys to access previous commands). If you wish to do later, you may refer to this [guide](https://ibmi-oss-docs.readthedocs.io/en/latest/troubleshooting/SETTING_BASH.html)

      d. **Deploy Direcotry**

   You can set you default deploy directory to the specified IFS folder. Remember the previously created folder "My Source Codes"? Whatever code you write and save in this location will have the ability to be pushed to your IBMi IFS folder that you specify in the "Deploy Directory". Once the source code has come to your IBMi you can easily issue a `CPYFRMIMPF` CL command to copy the sources to your IBMi libraries. 
