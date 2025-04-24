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

<img src="https://i.imgur.com/0GZQuMB.png">



<p/>

<h2>Install Windows 10 iso <a href="https://www.microsoft.com/en-us/software-download/windows10">Windows 10 Image</h2>

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




