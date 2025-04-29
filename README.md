# Active-Directory

The lab aims to provide a hands-on experience in setting up a virtualized environment for cybersecurity testing and exploration. Participants will create and configure multiple virtual machines (VMs) running Windows 10, Kali Linux, Windows Server, and Ubuntu Server. The lab focuses on teaching essential skills such as network configuration, security tool installation (e.g., Splunk, Sysmon), endpoint monitoring, and conducting security tests (including brute force attacks with Crowbar). Additional objectives include joining Windows machines to an Active Directory domain, enabling Remote Desktop, and automating tasks using PowerShell scripting. Overall, the lab offers practical exposure to cybersecurity concepts, tools, and techniques in a controlled, safe environment.

<h3>Tools</h3>


- Oracle VM VirtualBox Manager: For creating and managing virtual machines (VMs).
- Active Directory: For centralized domain management and user authentication.
- PowerShell: For scripting and automation of tasks.
- Splunk Server: For log analysis and monitoring, and SIEM to collect and analyze system logs.
- Splunk Universal Forwarder: For collecting and forwarding log data to Splunk.
- Sysmon: For endpoint monitoring and logging system activity on Windows.
- Windows Server 2022: For Active Directory Domain Services setup
- Ubuntu Server: As a Splunk server
- Microsoft Windows 10: As the target machine
- Kali Linux: As an attacker machine for simulation using the Hydra tool.


Step 1 Draw a diagram of the project on draw.io to visually understand the project.


<img src="https://i.imgur.com/0GZQuMB.png">



<p/>

<h2>Install Oracle VirtualBox <a href="https://www.virtualbox.org/">VirtualBox ISO</h2>
<h2>Install Windows 10 iso <a href="https://www.microsoft.com/en-us/software-download/windows10">Windows 10 ISO</h2>

Select the edition

Select the product language

Select 64-bit Download

Once the download pop up appears, select "Create installation media (USB flash drive, DVD, or ISO file) for another PC"

Select "ISO file" when pop up asks "Choose which media to use"

Save ISO file

In VirtualBox, click "New" to create a new VM


Name: choose a name for VM (I named it Windows)
 Please note: this machine will be used as our target machine


Folder: select where you want VM to live

ISO Image: select ISO image that you just downloaded

Check "Skip Unattended Installation" to install OS manually

<img src="https://i.imgur.com/poZYXiX.png">

Configure VM specifications (this can vary depending on your computer's specifications):for me

Select 8192 MB RAM for base memory, 3 CPU for processors, 50 GB for virtual hard disk

Finish

Start VM and follow the installation prompts


Select "I don't have a product key"

Select "Windows 10 Pro"

Accept the license terms

Select "Custom: Install Windows only ("advanced"
)Select "Next" to allow Windows to install

<h2>Download Kali Linux <a href="https://www.kali.org/get-kali/#kali-virtual-machines">Kali Linux Image</h2>

Select the Pre-Built Virtual Machine

Choose either 64-bit (preferred) or 32-bit depending on your machine

To verify your machines specifications, select Windows key > type "System" > select "System Information" > view "System Type"

Click 64-bit, then choose VirtualBox 64

Extract Kali Linux If you have any trouble extracting Kali Linux, download <a href="https://7-zip.org/">7Zip

Extract it with 7ZIp

Once extracted, double click on the "vbox" file so that it's automatically imported into VirtualBox

Now, go to VirtualBox and run the Kali VM by clicking "Start"



<h2>Download Windows Server 2022 <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022">Windows Server 2022 ISO</h2>

Fill out the form

Select "64-bit edition" under "ISO downloads"

Once downloaded, go to VirtualBox and select "New" to create a new VM

Name: choose a name for VM (I named it ADDC since we will be using this as our Active Directory Domain Controller)

Folder: select where you want VM to live

ISO Image: select ISO image that you just downloaded

Check "Skip Unattended Installation" to install OS manually

Configure VM specifications (this can vary depending on your computer's specifications):

Select 8192 MB RAM for base memory, 4 CPU for processors, 50 GB for virtual hard disk

Finish

Start VM and follow the installation prompts

Select "Install Now"

Select "Windows Server 2022 Standard Evaluation (Desktop Experience)"

Accept the license terms

Select "Custom: Install Windows Microsoft Server Operating System only (advanced)"

Select "Next" to allow Windows to install

Once setup is completed, you will be prompted to create a password

Select "Finish"

Log in with recently created password - Select "No" when asked "Do you want to allow your PC to be discoverable by both PCs and devices on this network?"


<h2>Download Ubuntu Server <a href="https://ubuntu.com/server">Ubuntu Server ISO</h2>

Name: choose a name for VM (I named it Splunk since we will be using this as our Splunk Server)

Folder: select where you want VM to live

ISO Image: select ISO image that you just downloaded

Check "Skip Unattended Installation" to install OS manually

Configure VM specifications (this can vary depending on your computer's specifications): - Select 8000 MB RAM for base memory, 2 CPU for processors, 100 GB for virtual hard disk - Splunk will require more specs as it will be ingesting data and we'll be running searches on it

Finish

Start the VM

Select "Try or Install Ubuntu Server"

Follow installation prompts; leave as default

Fill out "Profile Setup" form, which is where you will choose your credentials

Once installed, select "Reboot Now"

Click "Enter" when "Failed unmounting" error pops up

Once rebooted, logon with recently created credentials

run command: sudo apt-get update && sudo apt-get upgrade -y

this command will update & upgrade all of our repositories

Once completed, hit "Enter"

Summary

You should now have Oracle VM VirtualBox Manager installed, along with four VMs running Windows 10, Kali Linux, Windows Server, and Splunk Server.

<h2>Phase 2</h2>


In Virtual Box, navigate to Tools > Network > NAT Networks > Create
Refer to screenshot below for configuration details


<img src="https://i.imgur.com/8Vl8gjI.png">


Click "Apply"

Navigate to each VM > Settings > Network

Refer to screenshot below for configuration details.

Repeat the above 2 steps for each VM

Navigate to each VM > Settings > Network

Refer to screenshot above for configuration details.

Next, start Splunk VM

run: ip a

this will display current IP address, which will need to be updated to static IP 192.168.10.10/24

To set a static IP on Splunk server:

Run: sudo nano /etc/netplan/00-installer-config.yaml

tab to auto-complete, should only have one file


network: 

   ethernet:
   
     enp0s3:
       dhcp4: no
       addresses: [192.168.10.10/24]
       nameservers:
             addresses: [8.8.8.8]
       routes:
        - to: default
          via: 192.168.10.1
 version: 2

 
 Save configuration
 
Run: sudo netplan apply

Run: ip a

This will verify if IP address persists and is set to 192.168.10.10/24

Reboot

Verify connection by running: ping google.com

The steps above did not work for me as I had a different configuration file, but this turned out to be a great opportunity to troubleshoot and learn more. The configuration file on my Splunk server was 50-cloud-init.yaml, meaning our IP updates wouldn’t persist after a reboot. To resolve this, I created a custom 00-installer-config.yaml file inside /etc/netplan/ and configured it with the updated IP. This ensured the static IP would remain after a reboot instead of reverting to DHCP. I also had to back up and delete the old 50-cloud-init.yaml file. Reboot and verify IP address once done.


Install Splunk

Navigate to Splunk download free trial of Splunk Enterprise

Select Linux as OS

Select ".deb" file

Save into preferred directory

Navigate back to Splunk VM

Run: sudo apt-get install virtualbox

this will display what options are available

Run: sudo apt-get install virtualbox-guest-additions-iso

Type "y"

Click "Enter"

Once installed, run: clear

Navigate to Devices (on the top left of the screen) > Shared Folders > Shared Folders Settings

Add a folder

Folder Path: select path where we saved our Splunk installer

Foler Name: leave as default (AD-Project in our case)

Select: Read-only, Auto-mount, and Make Permenant

Click "Ok"

In Splunk, run: sudo reboot

Once rebooted, log back in

Run: sudo adduser vboxsf

ex. sudo adduser username vboxsf

If you receive a "group vboxsf does not exist" error, we may need some additional guest installations

Run: sudo apt-get install virtualbox

This will display what options are available

Run: sudo apt-get install virtualbox-guest-utils

Run: sudo reboot

Run: sudo adduser username vboxsf

User should be added to group now

Create a new directory called "Share"

Run: mkdir Share

Run: ls

Newly created directory "Share" should be listed

<img src="https://i.imgur.com/lJkvpEx.png">

Next, mount our shared folder onto our directory called "Share"

Run: sudo mount -t vboxsf -o uid=1000,gid=1000 <directory name where .deb file is located> share/

ex. sudo mount -t vboxsf -o uid=1000,gid=1000 AD-Project share/

If you forget what your shared folder name is:

Navigate to Devices > Shared Folders > Shared Folders Settings

If you get an error, exit session and re-do steps

Run: exit

Error may occur because when you add your user into a new group, it may not reflect until you log out

Run: ls -la

Should now see that our "share" is now highlighted

<img src="https://i.imgur.com/rEJvr94.png">

Change directories into that share

Run: cd share

Run: clear

Run: ls -la

This will display all the files listed in this directory, including our Splunk Installer

To install Splunk:

Run: sudo dpkg -i splunk (hit tab to auto-complete)

Once you see "Complete", should be good to change into directory where Splunk is located onto our server

Change directory

Run: cd /opt/splunk

Run: ls -la

Here you will notice that all users and groups belong to Splunk, which is a good thing as it limits the permissions to that user

<img src="https://i.imgur.com/MyWKI0P.png">

Change into user Splunk

Run: sudo -u splunk bash

Now, we are acting as user Splunk

Run: cd bin

Files listed in here are all binaries that Splunk can use

the one that we will use is "./splunk start"

Run: ./splunk start

This will run the installer

Click "q"

Type "y" to accept

Select an administrator username and password

Once installation is completed:

Run: exit

To exit out of user Splunk

Run:cd bin

Run: sudo ./splunk enable boot-start -user splunk

Whenever the VM reboots, Splunk will run with user splunk


<h4>Configure Windows Machine</h4>


In the Start Menu search, search "PC"

Select "Properties"

Select "Rename this PC"

Rename to "target-PC"

Select "Next"

Select "Restart Now"

Update static IP to 192.85.22.111

Open command prompt

Run: ipconfig

this will display current IPv4

Navigate to network icon at the bottom right of the window

Select "Open Network & Internet Settings"

Select "Change adapter options"

Right click the adapter, Ethernet

Select "Properties"

Select "Internet Protocol Version 4 (TCP/IPv4)" > "Properties"

Select "Use the following IP address"

Configure as shown below:


<img src="https://i.imgur.com/fTdgL5K.png">


Run:  ipconfig to view updated IPv4

<img src="https://i.imgur.com/YGDV90O.png">


nstall Splunk Universal Forwarder on target-PC (Please note: installing Splunk Universal Forwarder and Sysmon on the ADDC VM will follow these exact same steps)

Run Splunk VM (the following steps will not work if VM is not running)

In the target-PC, open the browser and type 192.168.10.10:8000

Splunk listens on port 8000

In the target-PC, visit Splunk

Create an account

Select "Products"

Select "Free Trials & Downloads"

Scroll down to Universal Forwarder & select "Get My Free Download"

Select the compatible OS

Once download is completed, double click the msi file

Select "Check this box to accept the License Agreemnent"

Select "An on-premises Splunk Enterprise Instance"

Select "Next"

Choose your credentials

For "Receiving Indexer", this is going to be our Splunk Server's IP address

Hostname or IP: 192.168.10.10

Default port for Splunk when receiving events is: 9997

Select "Next"

Select "Install"

UniversalForwarder Setup pop up will flash orange

Select "Finish"








