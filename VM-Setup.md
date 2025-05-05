 **Creating a Windows 10 Virtual Machine in VirtualBox**

Step 1: Download Windows 10 ISO
1. Visit the official [Windows 10 download page](https://www.microsoft.com/software-download/windows10).
2. Select the **Edition**.
3. Select the **Product Language**.
4. Choose **64-bit Download**.
5. Once the download pop-up appears, select:
   - **Create installation media (USB flash drive, DVD, or ISO file) for another PC**.
6. When prompted with "Choose which media to use," select **ISO file**.
7. Save the ISO file to your computer.



Step 2: Set Up a New Virtual Machine in VirtualBox
1. Open **VirtualBox** and click **New** to create a new VM.
2. Configure the following:
   - **Name**: Choose a name for your VM (e.g., `Windows`).
   - **Folder**: Select the location where the VM will be saved.
   - **ISO Image**: Select the ISO file you just downloaded.
   - Check **Skip Unattended Installation** to install the operating system manually.
3. Configure VM specifications as per your computer's capabilities:
   - **Base Memory**: 8192 MB RAM.
   - **Processors**: 3 CPUs.
   - **Virtual Hard Disk**: 50 GB.
4. Click **Finish**.



Step 3: Install Windows 10 on the Virtual Machine
1. Start the VM.
2. Follow the installation prompts to complete the setup of Windows 10.



Notes
- The VM created will be used as your **target machine**.
- Ensure your computer meets the necessary requirements to allocate the specified resources to the VM.


---


1. **Download Kali Linux**  
   [Download Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines)  
   - Select the Pre-Built Virtual Machine option.
   - Choose either **64-bit (preferred)** or **32-bit**, depending on your machine.

2. **Verify Your Machine's Specifications**  
   - Press the **Windows key**, type **"System"**, and select **"System Information"**.
   - Look for **"System Type"** to determine if your machine is 64-bit or 32-bit.

3. **Download the VirtualBox Image**  
   - If your machine is 64-bit, download **VirtualBox 64-bit**.

4. **Extract Kali Linux**  
   - If you encounter issues extracting the files, download [7-Zip](https://www.7-zip.org/) to extract the files.

5. **Import into VirtualBox**  
   - Once extracted, double-click the `.vbox` file to automatically import it into VirtualBox.

6. **Run the Kali Linux VM**  
   - Open VirtualBox, select the Kali VM, and click **"Start"** to boot it up.


---
Here are the steps to download and set up Windows Server 2022 as a virtual machine in VirtualBox:

1. **Download Windows Server 2022**  
   [Download Windows Server 2022](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)  
   - Fill out the required form.
   - Select **"64-bit edition"** under **"ISO downloads"**.

2. **Create a New Virtual Machine in VirtualBox**  
   - Once the ISO is downloaded, open VirtualBox and select **"New"** to create a new VM.  
   - Configure the following:  
     - **Name**: Choose a name for your VM (e.g., **ADDC**, as it will be used for the Active Directory Domain Controller).  
     - **Folder**: Select where you want the VM to reside.  
     - **ISO Image**: Select the ISO image you just downloaded.  
     - **Skip Unattended Installation**: Check this box to install the OS manually.

3. **Configure VM Specifications**  
   Configure the VM based on your computer's capabilities:  
   - **Base Memory**: Set to **8192 MB RAM**.  
   - **Processors**: Assign **4 CPUs**.  
   - **Virtual Hard Disk**: Allocate **50 GB**.

4. **Finish and Start the VM**  
   - Complete the setup process.  
   - Start the VM and follow the installation prompts.

5. **Install Windows Server 2022**  
   - Click **"Install Now"** when prompted.  
   - Select **"Windows Server 2022 Standard Evaluation (Desktop Experience)"**.  
   - Accept the license terms.  
   - Choose **"Custom: Install Windows Microsoft Server Operating System only (advanced)"**.  
   - Click **"Next"** to install Windows.

6. **Complete Setup**  
   - Once the setup is complete, create a password as prompted.  
   - Click **"Finish"**.  
   - Log in using the password you just created.  
   - When asked **"Do you want to allow your PC to be discoverable by both PCs and devices on this network?"**, select **"No"**.

---


Here is the step-by-step process to download and set up Ubuntu Server as a virtual machine in VirtualBox:

1. **Download Ubuntu Server**  
   [Download Ubuntu Server](https://ubuntu.com/download/server)  

2. **Create a New Virtual Machine in VirtualBox**  
   - Open VirtualBox and select **"New"** to create a new VM.  
   - Configure the following:  
     - **Name**: Choose a name for your VM (e.g., **Splunk** since it will be used as your Splunk Server).  
     - **Folder**: Select the directory where you want the VM to be stored.  
     - **ISO Image**: Select the Ubuntu Server ISO image you just downloaded.  
     - **Skip Unattended Installation**: Check this box to manually install the OS.

3. **Configure VM Specifications**  
   Configure the VM to meet the following requirements:  
   - **Base Memory**: Set to **8000 MB RAM**.  
   - **Processors**: Assign **2 CPUs**.  
   - **Virtual Hard Disk**: Allocate **100 GB**.  
   - Note: Since Splunk will ingest data and run searches, additional resources may be needed based on your usage.

4. **Finish and Start the VM**  
   - Complete the setup process.  
   - Start the VM in VirtualBox.

5. **Install Ubuntu Server**  
   - Select **"Try or Install Ubuntu Server"** when prompted.  
   - Follow the installation prompts and leave the default settings.  
   - Fill out the "Profile Setup" form to create your credentials.  

6. **Reboot and Log In**  
   - Once installation is complete, select **"Reboot Now"**.  
   - If a **"Failed unmounting"** error appears, press **"Enter"** to continue.  
   - After the reboot, log in using the credentials you created.

7. **Run Update and Upgrade Commands**  
   - Open the terminal and run the following command to update and upgrade all repositories:  
     ```bash
     sudo apt-get update && sudo apt-get upgrade -y
     ```  
   - Once the process is complete, press **"Enter"**.

### Summary  
You should now have Oracle VM VirtualBox Manager installed with four VMs running:
- **Windows 10**
- **Kali Linux**
- **Windows Server**
- **Splunk Server (Ubuntu Server)**






