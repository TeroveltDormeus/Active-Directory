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

Navigate to <a href="https://www.splunk.com/">Splunk

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


Install Splunk Universal Forwarder on target-PC (Please note: installing Splunk Universal Forwarder and Sysmon on the ADDC VM will follow these exact same steps)

Run Splunk VM (the following steps will not work if VM is not running)

In the target-PC, open the browser and type 192.168.10.10:8000

Splunk listens on port 8000

In the target-PC, visit <a href="https://www.splunk.com/">Splunk

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

Install Sysmon

In the target-PC, navigate to Sysmon Downloads

Select "Download Sysmon"

Navigate to Sysmon's Configuration File on GitHub

This is the sysmon configuration that we will be using

Select "Raw"

<img src="https://i.imgur.com/wYIJXsJ.png">

Save in downloads

Extract the Sysmon file

Navigate to downloads directory

Right click "Sysmon"

Select "Extract All"

Select "Extract"

Copy the file path

<img src="https://i.imgur.com/jbKBi8o.png">

Open PowerShell as administrator

Change directory into the file path

Run: cd (Paste the file path of the extracted directory here)

ex. cd C:\Users\hacki\Downloads\Sysmon 

Run: .\Sysmon64.exe -i ..\sysmonconfig.xml


-i flag indicates that we want to specify a configuration file

../ allows us to go back one directory

sysmon config file is locaed under downloads directory so ../ will allow us to do that

Hit "Enter"


Select "Agree" to install Sysmon

Screen will display "Sysmon64 started"

Close PowerShell

Configure Splunk Unviersal Forwarder to specify what data we want to send to our Splunk Server

Do this by configuring file called "inputs.conf"

Navigate to File Explorer > Local Disk (C:) > Program Files > SplunkUniversalForwarder > etc > system > default > inputs.conf

Do NOT want to edit the inputs.conf file under the default directory

Create a new file under the local directory and name it inputs.conf

Open Notepad as administrator and enter the below information:

This instructs Splunk Universal Forwarder to push events related to Application, Security, System, and Sysmon over to Splunk Server

Take note of "index=endpoint", whatever falls under these categories will be sent over Splunk and placed under the index "endpoint". If our Splunk Server does not have an index named endpoint, it will not receive any of these events.


[WinEventLog://Application]

index = endpoint

disabled = false

[WinEventLog://Security]

index = endpoint

disabled = false

[WinEventLog://System]

index = endpoint

disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]

index = endpoint

disabled = false

renderXml = true

source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational

Save file

Save under local directory

File Explorer > Local Disk (C:) > Program Files > SplunkUniversalForwarder > etc > system > local > inputs.conf

File Name: inputs.conf

Save as type; All Files

Save

Restart

Any time you update "inputs.conf" file, you must restart Splunk's Universal Forwarder service
Open "Services" as administrator

Find "SplunkForwarder"

Double click "SplunkForwarder"

Select "Log On"

Select "Local System Account"

Select "Apply"

Select "Ok"

Right click "SplunkForwarder" and select "Restart" or click "Restart" on the top left


<img src="https://i.imgur.com/7i7XS3b.png">

Select "Start" the service

Now, we have our Sysmon & Splunk Universal Forwarder installed along with our updated inputs.conf file.

Finalize Splunk Server configuration

On the target-PC, go to Splunk web portal

Search 192.85.22.7:8000 in browser

Splunk VM must be running in order for portal to work

Log on with credentials created during Splunk install on Splunk Server

<img src="https://i.imgur.com/QPhHbBN.png">

Once logged on, create "endpoint" index

Select "Settings"

Select "Indexes"

Select "New Index"

Index Name: endpoint

Select "Save"


<img src="https://i.imgur.com/y2PZCbi.png">

Next, enable Splunk Server to receive data

Select "Settings"

Select "Forwarding and receiving"

Under "Receive data", select "Configure receiving"

Select "New Receiving Port"

Listen on this port: 9997

If you recall during the set up, 9997 is the default port

Select "Save"

Should now see data coming in from target-PC

Select "Search & Reporting"

Search "index=endpoint"

Scroll down to "host" and you will see 1 host, which is the target-PC

Once Windows Server is configured, you will see 1 host

<img src="https://i.imgur.com/I9qOK1l.png">

5. Configure Windows Server

Rename the host name of machine

In the Start Menu search, search "PC"

Select "Properties"

Select "Rename this PC"

Rename to "ADDC01"

Select "Next"

Select "Restart Now"

Update static IP to 192.85.22.105

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

<img src="https://i.imgur.com/4WYWyub.png">

run ipconfig in cmd to make sure

<img src="https://i.imgur.com/JtVNtCt.png">

Install Splunk Universal Forwarder on target-PC (Please note: installing Splunk Universal Forwarder and Sysmon on the ADDC VM will follow these exact same steps)

Run Splunk VM (the following steps will not work if VM is not running)

In the target-PC, open the browser and type 192.168.10.10:8000

Splunk listens on port 8000

In the target-PC, visit Splunk

Log on to account

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

Hostname or IP: 192.168.10.7

Default port for Splunk when receiving events is: 9997
Select "Next"

Select "Install"

UniversalForwarder Setup pop up will flash orange

Select "Finish"

Install Sysmon

In the ADDC01 machine, navigate to Sysmon Downloads

Select "Download Sysmon"

Navigate to Sysmon's Configuration File on GitHub

This is the sysmon configuration that we will be using

Select "Raw"

<img src="https://i.imgur.com/9GwkYM3.png">


tag Sysmon download and sysmonconfig.xml file

insert Sysmon raw png

Save in downloads

Extract the Sysmon file

Navigate to downloads directory

Right click "Sysmon"

Select "Extract All"

Select "Extract"

Copy the file path

<img src="https://i.imgur.com/GmSadnH.png">

Open PowerShell as administrator

Change directory into the file path

Run: cd (Paste the file path of the extracted directory here)

ex. cd C:\Users\hacki\Downloads\Sysmon

Run: .\Sysmon64.exe -i ..\sysmonconfig.xml

-i flag indicates that we want to specify a configuration file

../ allows us to go back one directory

sysmon config file is located under downloads directory so ../ will allow us to do that

Hit "Enter"

Select "Agree" to install Sysmon

Screen will display "Sysmon64 started"

Close PowerShell

Configure Splunk Unviersal Forwarder to specify what data we want to send to our Splunk Server

Open PowerShell as administrator

Change directory into the file path

Run: cd (Paste the file path of the extracted directory here)

ex. cd C:\Users\hacki\Downloads\Sysmon

Run: .\Sysmon64.exe -i ..\sysmonconfig.xml

-i flag indicates that we want to specify a configuration file

../ allows us to go back one directory

sysmon config file is located under downloads directory so ../ will allow us to do that

Hit "Enter"

Select "Agree" to install Sysmon

Screen will display "Sysmon64 started"

Close PowerShell

Configure Splunk Unviersal Forwarder to specify what data we want to send to our Splunk Server

Do this by configuring file called "inputs.conf"

Navigate to File Explorer > Local Disk (C:) > Program Files > SplunkUniversalForwarder > etc > system > default > inputs.conf

Do NOT want to edit the inputs.conf file under the default directory

Create a new file under the local directory and name it inputs.conf

Open Notepad as administrator and enter the below information:

This instructs Splunk Universal Forwarder to push events related to Application, Security, System, and Sysmon over to Splunk Server

Take note of "index=endpoint", whatever falls under these categories will be sent over Splunk and placed under the index "endpoint". If our Splunk Server does not have an index named endpoint, it will not receive any of these events.

[WinEventLog://Application]

index = endpoint

disabled = false

[WinEventLog://Security]

index = endpoint

disabled = false

[WinEventLog://System]

index = endpoint

disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]

index = endpoint

disabled = false

renderXml = true

source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational

Save file

Save under local directory

File Explorer > Local Disk (C:) > Program Files > SplunkUniversalForwarder > etc > system > local > inputs.conf
File Name: inputs.conf

Save as type; All Files

Save

Restart

Any time you update "inputs.conf" file, you must restart Splunk's Universal Forwarder service

Open "Services" as administrator

Find "SplunkForwarder"

Double click "SplunkForwarder"

Select "Log On"

Select "Local System Account"

Select "Apply"

Select "Ok"

Right click "SplunkForwarder" and select "Restart" or click "Restart" on the top left

<img src="https://i.imgur.com/H1gX34X.png">

Select "Start" the service

Now, we have our Sysmon & Splunk Universal Forwarder installed along with our updated inputs.conf file.

Now, we should see data coming in from ADDC01 as well as the target-PC

Log onto Splunk web portal by searching 192.168.10.10:8000 in browser

Splunk VM must be running in order for portal to work

Log on with credentials created during Splunk install on Splunk Server

Select "Search & Reporting"

Search "index=endpoint"

Scroll down to "host" and you will see 2 hosts


<img src="https://i.imgur.com/dQQ40tI.png">


Phase 3: Active Directory and Control Domain

1. Setup Active Directory Domain Services on Windows Server

Install ADDS aka Active Directory Domain Services

On Windows Server, open "Server Manager"

Select "Manage" > Add Roles & Features > Next > Role-based of feature-based installation > Next > select ADDC01 > Next > select "Active Directory Domain Services > Add features > Next > Next > Install

Once installed, select "close"

Promote the server & create a new domain

Open Server Manager

Select the flag icon

Select “Promote the server to a domain controller"

Select “Add a new forest” since we are creating a new domain

Root domain name: yourdomainname.local

Domain name must have top level domain; cannot just be "yourdomainname"
yourdomainname.test, etc

Attackers love to attack domain controllers since it has access to everything. Access to this file grants access to everything related to AD including passwords hashes.

Create a password

Install

Machine will automatically restart

Log in

On logon screen, will see domainame\Administrator

This indicates that we have successfully installed ADDS and promoted our server to a domain controller.

Create users

Open Server Manager

Select "Tools"

Select "Active Directory Users and Computers"

Here we can create objects such as users, computers, groups, etc.

Right click your domain name

Go to new a create new user

Make a username and password

Insert new user png

Active Directory is now set up and our server is now promoted to domain controller.

2. Configure Windows 10 Machine to Join New Domain

Log onto Windows 10 machine and join our newly created domain & authenticate using Jenny Smith account

Log onto Windows 10 machine (target-PC)

Search PC > select "Properties" > Advanced system settings > Computer Name > Change

Under "Member of", select "Domain"

insert pc name and domain

You will get an error message; this is related to the DNS we configured earlier. Originally, we had our Preferred DNS server set to 8.8.8.8, which is Google's DNS server. We want to update the Preferred DNS server so that it is pointing to our domain controller, which is 192.168.10.7.

Update the Preferred DNS Server to the domain controller's IP address:

Navigate to network icon at the bottom right of the window

Select "Open Network & Internet Settings"

Select "Change adapter options"

Right click the adapter, Ethernet

Select "Properties"

Select "Internet Protocol Version 4 (TCP/IPv4)" > "Properties"

Select "Use the following IP address"

Configure as shown below:

Insert Win10 IPV4 configs

Confirm updated DNS Server in Command Prompt

Open Command Prompt

Run: ipconfig /all

insert cmd image where I put ipconfig/all

Now, we can join the domain

Search PC > select "Properties" > Advanced system settings > Computer Name > Change
Under "Member of", select "Domain"

Select "Ok"

Will be prompted to enter credentials

User the administrator account of the server to log on as it has the appropiate permissions

The administrator account will grant access to join domain


You will get a pop up welcoming you to the domain.

Computer will prompt you to restart

Once you are back on the log on screen, confirm that new user has successfully joined the domain

Select "Other User"

Log on with your newly created user Jenny Smith or Maria Rodriguez

Type "cutti4l" and the password


Summary

We have successfully configured our Active Directory server, created new users, joined our computer to a new domain, and logged in as a domain user.


Phase 4: Brute Force Attack
1. Configure Network

Log onto Kali Linux machine with default credentials

Configure a static IP address to 192.168.10.250

Right click the Ethernet icon

Select "Edit Connections"

insert kali net dropdown png

Select Wired connection 1

Select the cog icon

Select IPv4 Seetings

Configure and save:

insert Linux ip config

To reflect changes, select the Ethernet icon and select "Disconnect

Select the Ethernet icon again and select "Wired connection 1"

Confirm that the static IP has been updated

Open terminal

Run: ip a

insert ip a png

Verify connectivity

Run: ping google.com

Run: ping 192.168.10.10

Update & upgrade repositories


Run: sudo apt-get update && sudo apt-get upgrade -y

2. Set up the Attack

Set up attack, once update & upgrade is complete

First, create a new directory called AD Project

Run: mkdir AD-Project

This will create a directory on our desktop. All the files that we will create & use, will be put into this directory.

 Enable RDP
 

Before we launch the attack, we want to enable Remote Desktop Protocol aka RDP

Open Windows target-PC

Enable remote desktop on target-PC

Go to the PC’s properties

Search PC

Select Properties

Select Advanced system settings

Will prompt you to logon with administrator credentials

Select Remote tab

Select Allow remote connections to this computer

insert rdp png

Select Select Users…

Select Add

Add your users

Type "cutti4l" for Jenny Smith, then select "Check Names"

Select OK

Here you will see both users added

insert rdp user added png

Select OK

Select Apply

Select OK

We have successfully enabled remote desktop onto target-PC.

You can also enable RDP on your ADDC machine if you'd like.


4. Attack

Open Kali Linux machine

Open terminal

Make two files in AD-Project directory

insert png^

Run hydra -L user.txt -P pass.txt rdp://192.85.22.111 -V

insert hydra command png

View telemetry generared on Splunk

Open Splunk in the Windows 10 (target-PC) machine

Search "192.85.22.7:8000" in browser

Select Search & Reporting

Narrow down search

Search "index=endpoint cutti4l"

insert event code png

Select 5379

This will automatically update our query

Look at the time of all events

Will see that they all happened around the same time, which is a clear indicator of a the RDP attack

Save as new dashboard in splunk

Go to dashboard and go to top left icon that has two columns 

 Add chart of choice and input
 
SPL Query index=endpoint cutti4l EventCode=5379

Insert chart png

Congratulations, you've reached the end of the guide! Your setup should be fully operational, with successful communication between four Virtual Machines: the Windows 10 Target Machine, Kali Linux Attacker Machine, Splunk Server, and Windows Server (ADDC). You should now be able to view events in Splunk, test its functionality, and
Hydra RDP tool.

A huge thanks to MyDFIR on YouTube for the tutorial. I learned so much throughout this process and was able to turn my theoretical knowledge into hands-on experience.

References

Active Directory Project by MyDFIR

Sysmon

Splunk Universal Forwarder Documentation

Splunk Documentation












