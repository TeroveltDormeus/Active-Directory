# Creating a Windows 10 Virtual Machine in VirtualBox

## Step 1: Download Windows 10 ISO
1. Visit the official [Windows 10 download page](https://www.microsoft.com/software-download/windows10).
2. Select the **Edition**.
3. Select the **Product Language**.
4. Choose **64-bit Download**.
5. Once the download pop-up appears, select:
   - **Create installation media (USB flash drive, DVD, or ISO file) for another PC**.
6. When prompted with "Choose which media to use," select **ISO file**.
7. Save the ISO file to your computer.

---

## Step 2: Set Up a New Virtual Machine in VirtualBox
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

---

## Step 3: Install Windows 10 on the Virtual Machine
1. Start the VM.
2. Follow the installation prompts to complete the setup of Windows 10.

---

### Notes
- The VM created will be used as your **target machine**.
- Ensure your computer meets the necessary requirements to allocate the specified resources to the VM.

However, the instructions are repetitive, especially the steps involving navigating to each VM and adjusting the network settings. Here's a consolidated and streamlined version of the instructions:

---

### Configuring NAT Networks in Oracle VirtualBox

1. Open **VirtualBox**.

2. Navigate to **Tools > Network > NAT Networks**.

3. Click **Create** to set up a new NAT Network.
   - Refer to the provided screenshot for the specific configuration details.
   - After configuring the NAT Network, click **Apply**.
<p/>
  
   <img src="https://i.imgur.com/LfKiqRX.png">



4. For each virtual machine (VM):
   - Go to **Settings > Network**.
   - Configure the network settings as specified in the screenshot.
   - Repeat this step for all VMs.

---

