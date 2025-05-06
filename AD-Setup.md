

Here are the detailed steps to set up the target-PC and install Splunk Universal Forwarder:

---

### **1. Rename the PC**
1. Open the **Start Menu** and search for **"PC"**.
2. Select **"Properties"**.
3. Click **"Rename this PC"**.
4. Enter the new name as **"target-PC"**.
5. Select **"Next"**.
6. Choose **"Restart Now"** to apply the changes.

---

### **2. Update Static IP**
1. Open **Command Prompt** and run:
   ```bash
   ipconfig
   ```
   - This displays the current IPv4 address.

2. Navigate to the **network icon** in the bottom-right corner of the screen.
   - Select **"Open Network & Internet Settings"**.
   - Choose **"Change adapter options"**.

3. Right-click the adapter (e.g., Ethernet) and select **"Properties"**.

4. Highlight **"Internet Protocol Version 4 (TCP/IPv4)"** and click **"Properties"**.

5. Select **"Use the following IP address"** and configure the following:
   - **IP Address**: `192.85.22.111`
   - **Subnet Mask**: `255.255.255.0`
   - **Default Gateway**: `<provide gateway here>` (e.g., `192.85.22.1`).

6. Click **"OK"** to save the changes.

7. Open **Command Prompt** again and run:
   ```bash
   ipconfig
   ```
   - Verify that the updated IPv4 address reflects the new IP.

---

### **3. Install Splunk Universal Forwarder**
1. **Download Splunk Universal Forwarder**:
   - Navigate to the [Splunk Universal Forwarder Download Page](https://www.splunk.com/en_us/download/universal-forwarder.html).
   - Choose the installer appropriate for your operating system (e.g., `.msi` for Windows).

2. **Install Splunk Universal Forwarder**:
   - Run the downloaded installer.
   - Follow the installation prompts:
     - Accept the license agreement.
     - Choose the destination folder.
     - Enter Splunk credentials if prompted.
   - Complete the installation.

---

### **4. Repeat Steps for ADDC VM**
- Follow the exact same steps to install Splunk Universal Forwarder and configure Sysmon on the **ADDC VM**.

---

### Step-by-Step Guide to Setting Up Splunk Universal Forwarder and Sysmon on Target-PC

---

### **1. Ensure Splunk VM is Running**
   - Check that your Splunk VM is live and accessible. Splunk listens on port `8000`.

---

### **2. Access Splunk Server on Target-PC**
   - Open a web browser on the target-PC and navigate to:
     ```
     http://192.168.10.10:8000
     ```
   - This will take you to the Splunk web interface.

#### **Create an Account on Splunk**
   - Create a new account with credentials to access the server.

---

### **3. Download Splunk Universal Forwarder**
   - Navigate to **Products > Free Trials & Downloads** on the Splunk web interface.
   - Scroll down to **Universal Forwarder** and select **"Get My Free Download"**.
   - Choose the compatible OS (e.g., Windows) and download the `.msi` file.

#### **Install Splunk Universal Forwarder**
1. Double-click the downloaded `.msi` file to begin installation.
2. Accept the License Agreement by checking the box.
3. Select **"An on-premises Splunk Enterprise Instance"**.
4. Click **"Next"** and set your credentials.
5. Configure the **Receiving Indexer**:
   - **Hostname or IP**: `192.168.10.10` (Splunk Server's IP).
   - **Port**: `9997` (Default port for Splunk receiving events).
6. Click **"Next"** and then **"Install"**.
7. When the setup flashes orange, click **"Finish"**.

---

### **4. Install Sysmon**
   - Navigate to **Sysmon Downloads** and download Sysmon.
   - Visit **Sysmon's Configuration File** on GitHub and click **"Raw"** to download it.
   - Save the configuration file in your **Downloads** folder.

#### **Extract Sysmon Files**
1. Navigate to the **Downloads** directory.
2. Right-click on the Sysmon archive and select **"Extract All"**.
3. Click **"Extract"** and copy the extracted folder's path.

#### **Install Sysmon via PowerShell**
1. Open **PowerShell** as Administrator.
2. Change directory to the extracted Sysmon folder:
   ```powershell
   cd <Extracted Sysmon Folder Path>
   ```
   Example:
   ```powershell
   cd C:\Users\hacki\Downloads\Sysmon
   ```
3. Run the following command to install Sysmon with the configuration file:
   ```powershell
   .\Sysmon64.exe -i ..\sysmonconfig.xml
   ```
   - The `-i` flag specifies the configuration file.
   - The `..` moves up one directory to locate the configuration file.
4. Agree to install Sysmon when prompted.
5. Verify installation with the message **"Sysmon64 started"**.

---

### **5. Configure Splunk Universal Forwarder**
#### **Edit `inputs.conf`**
1. Navigate to the following directory:
   ```
   C:\Program Files\SplunkUniversalForwarder\etc\system\local
   ```
2. Create a new file named `inputs.conf` in the **local** directory.
3. Open **Notepad** as Administrator and add the following content:
   ```ini
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
   ```
4. Save the file in the **local** directory as `inputs.conf`. Ensure:
   - **File Name**: `inputs.conf`.
   - **Save as type**: **All Files**.

#### **Restart Splunk Universal Forwarder Service**
1. Open **Services** as Administrator:
   - Search for **"Services"** in the Start menu.
2. Locate **"SplunkForwarder"** in the list of services.
3. Double-click **"SplunkForwarder"** and configure:
   - **Log On** tab: Select **"Local System Account"**.
   - Click **"Apply"** and **"OK"**.
4. Restart the service:
   - Right-click **"SplunkForwarder"** and select **"Restart"**.
   - Alternatively, click **"Restart"** in the top-left corner.

---

### **6. Verify Setup**
   - Ensure that both Sysmon and Splunk Universal Forwarder are installed, configured, and running correctly.
   - The updated `inputs.conf` file ensures events related to **Application**, **Security**, **System**, and **Sysmon** logs are sent to the Splunk Server under the **"endpoint"** index.

---
Here is the step-by-step process to finalize the Splunk Server configuration:

---

### **1. Access the Splunk Web Portal**
1. Ensure that the **Splunk VM** is running.
2. On the **target-PC**, open a browser and navigate to:
   ```
   http://192.85.22.7:8000
   ```
3. Log in using the credentials created during the Splunk Server installation.

---

### **2. Create the "endpoint" Index**
1. Once logged in, navigate to **Settings** in the top-right corner.
2. Select **Indexes** under the **Data** section.
3. Click **New Index**.
4. Configure the new index:
   - **Index Name**: `endpoint`
5. Click **Save** to create the index.

---

### **3. Enable Splunk Server to Receive Data**
1. Go to **Settings**.
2. Select **Forwarding and receiving** under the **Data** section.
3. Under **Receive data**, click **Configure receiving**.
4. Click **New Receiving Port** and specify:
   - **Port Number**: `9997` (this is the default port configured during the Universal Forwarder setup).
5. Click **Save**.
6. Verify that the receiving configuration appears in the list.

---

### **4. Verify Data is Being Received**
1. Go to **Search & Reporting** from the Splunk home page.
2. Run the following search query:
   ```
   index=endpoint
   ```
3. Scroll down to the **host** section on the search results page. 
   - You should see **1 host**, which will be the **target-PC**.

---

### **5. Prepare for Additional Hosts**
- Once the Windows Server (or any other host) is configured with the Splunk Universal Forwarder, repeat the verification step.
- You should see an additional host listed under the **host** section of the search results.

---

The following steps outline the detailed configuration of the Windows Server (renamed to **ADDC01**) for Splunk Universal Forwarder and Sysmon, along with verification on the Splunk web portal.

---

### **1. Configure Hostname and Static IP**
1. **Rename the Hostname**:
   - Open the **Start Menu**, search for **"PC"**, and select **Properties**.
   - Click **"Rename this PC"**.
   - Rename it to **ADDC01**.
   - Click **Next**, then **Restart Now**.

2. **Set Static IP**:
   - Open **Command Prompt** and run:
     ```bash
     ipconfig
     ```
     - Note the current IPv4 address.
   - Navigate to the **Network icon** in the bottom-right corner of the screen.
     - Select **Open Network & Internet Settings**.
     - Click **Change adapter options**.
   - Right-click the network adapter (e.g., Ethernet) and select **Properties**.
   - Highlight **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.
   - Select **Use the following IP address** and configure:
     - **IP Address**: `192.85.22.105`
     - **Subnet Mask**: `255.255.255.0`
     - **Default Gateway**: `<provide gateway>`
   - Click **OK** to save changes.

3. Verify the static IP:
   - Run `ipconfig` again in **Command Prompt** to confirm the updated IP.

---

### **2. Install Splunk Universal Forwarder**
1. **Download Universal Forwarder**:
   - On the ADDC01 machine, access the [Splunk Universal Forwarder Download Page](https://www.splunk.com/en_us/download/universal-forwarder.html).
   - Select the compatible OS (e.g., `.msi` for Windows) and download.

2. **Install Universal Forwarder**:
   - Double-click the downloaded `.msi` file.
   - Accept the License Agreement.
   - Select **"An on-premises Splunk Enterprise Instance"**.
   - Configure:
     - **Receiving Indexer**:
       - **Hostname or IP**: `192.168.10.7` (Splunk Server's IP).
       - **Port**: `9997` (default).
   - Complete the installation by clicking **Finish**.

---

### **3. Install Sysmon**
1. **Download Sysmon**:
   - Navigate to the [Sysmon Download Page](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) and download Sysmon.
   - Visit [Sysmon Configuration File on GitHub](https://github.com/SwiftOnSecurity/sysmon-config) and click **"Raw"** to download the configuration file.

2. **Install Sysmon**:
   - Extract the Sysmon file.
   - Open **PowerShell** as Administrator and navigate to the folder:
     ```powershell
     cd <Extracted Sysmon Directory>
     ```
     Example:
     ```powershell
     cd C:\Users\hacki\Downloads\Sysmon
     ```
   - Run the following command to install Sysmon with the configuration file:
     ```powershell
     .\Sysmon64.exe -i ..\sysmonconfig.xml
     ```
   - Agree to install Sysmon when prompted.
   - Verify installation with the message **"Sysmon64 started"**.

---

### **4. Configure Splunk Universal Forwarder**
1. **Edit `inputs.conf`**:
   - Navigate to:
     ```
     C:\Program Files\SplunkUniversalForwarder\etc\system\local
     ```
   - Create a new file named `inputs.conf`.
   - Open the file in **Notepad** as Administrator and add:
     ```ini
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
     ```
   - Save the file as **inputs.conf** in the **local** directory.

2. **Restart Splunk Universal Forwarder Service**:
   - Open **Services** as Administrator.
   - Find **SplunkForwarder** in the list.
   - Double-click **SplunkForwarder** and configure:
     - **Log On** tab: Select **Local System Account**.
     - Click **Apply** and **OK**.
   - Restart the service:
     - Right-click **SplunkForwarder** and select **Restart**.

---

### **5. Verify Data in Splunk Web Portal**
1. Ensure Splunk VM is running.
2. On the **target-PC**, open a browser and navigate to:
   ```
   http://192.168.10.10:8000
   ```
3. Log in with the Splunk credentials.
4. Verify data:
   - Go to **Search & Reporting**.
   - Run:
     ```
     index=endpoint
     ```
   - Scroll down to the **host** section.
     - You should see **2 hosts**:
       - **target-PC**
       - **ADDC01**

---

Here is a detailed step-by-step guide for setting up Active Directory Domain Services (ADDS) on a Windows Server and joining a Windows 10 machine to the newly created domain:

---

### **1. Setup Active Directory Domain Services (ADDS) on Windows Server**
#### **Install ADDS**
1. Open **Server Manager** on the Windows Server.
2. Go to **Manage** > **Add Roles & Features**.
3. Click **Next** on the **Add Roles and Features Wizard**.
4. Select **Role-based or feature-based installation** and click **Next**.
5. Select the server (e.g., **ADDC01**) and click **Next**.
6. Under **Server Roles**, check **Active Directory Domain Services**.
7. When prompted, select **Add Features** and then click **Next**.
8. Click **Next** on the **Features** and **ADDS** information pages.
9. Click **Install**. Once the installation is complete, click **Close**.

#### **Promote the Server to a Domain Controller**
1. Open **Server Manager** and select the **flag icon** in the top-right corner.
2. Select **Promote this server to a domain controller**.
3. Choose **Add a new forest**, as we are creating a new domain.
4. Enter the **Root domain name** (e.g., `yourdomainname.local`). Ensure it includes a top-level domain (e.g., `.local`, `.test`).
5. Set a strong **Directory Services Restore Mode (DSRM)** password and click **Next**.
6. Proceed through the DNS options and click **Next**.
7. Review the **NetBIOS** name and accept the default or modify it if required.
8. Click **Next** through the Paths page, review the configuration, and click **Install**.
9. The server will automatically restart after the installation.

#### **Verify Installation**
1. Log in after the restart using the **domainname\Administrator** credentials.
   - Example: `yourdomainname\Administrator`.
2. This indicates that the server has been successfully promoted to a Domain Controller.

#### **Create Users**
1. Open **Server Manager** and go to **Tools** > **Active Directory Users and Computers**.
2. Navigate to your domain name.
3. Right-click the domain name, select **New**, and choose **User**.
4. Fill in the user's details (e.g., Jenny Smith) and set a password.
5. Complete the wizard to create the user.

---

### **2. Configure Windows 10 Machine to Join the New Domain**
#### **Update the Preferred DNS Server**
1. Log into the Windows 10 machine (e.g., **target-PC**).
2. Navigate to the **Network icon** in the bottom-right corner and select **Open Network & Internet Settings**.
3. Choose **Change adapter options**.
4. Right-click the network adapter (e.g., Ethernet) and select **Properties**.
5. Highlight **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.
6. Select **Use the following IP address** and configure:
   - **IP Address**: Set the machine's IP address.
   - **Subnet Mask**: `255.255.255.0`.
   - **Default Gateway**: Use the IP address of the Domain Controller (e.g., `192.168.10.7`).
   - **Preferred DNS Server**: Set this to the IP address of the Domain Controller (e.g., `192.168.10.7`).
7. Click **OK** to save changes.

8. Confirm the updated DNS server:
   - Open **Command Prompt** and run:
     ```bash
     ipconfig /all
     ```
   - Verify the **Preferred DNS Server** is pointing to the Domain Controller.

#### **Join the Domain**
1. Search for **PC** in the Start Menu and select **Properties**.
2. Click **Advanced system settings** > **Computer Name** > **Change**.
3. Under **Member of**, select **Domain** and enter your domain name (e.g., `yourdomainname.local`).
4. Click **OK**.
5. Enter the **Administrator** credentials of the Domain Controller to authenticate.
6. A pop-up will welcome you to the domain.
7. Restart the computer as prompted.

#### **Log into the Domain**
1. Once restarted, on the logon screen, select **Other User**.
2. Enter the credentials of the newly created user (e.g., Jenny Smith or Maria Rodriguez).
   - Username: `yourdomainname\jenny.smith`.
   - Password: Enter the user's password.

---
### Summary of Accomplishments:

1. **Configured Active Directory Domain Services (ADDS):**
   - Successfully installed and configured ADDS on the Windows Server.
   - Promoted the server to a **Domain Controller**.
   - Created a new domain (`yourdomainname.local`) with proper domain naming conventions.

2. **Created New Users in Active Directory:**
   - Used the **Active Directory Users and Computers** tool to create new users (e.g., Jenny Smith, Maria Rodriguez).
   - Configured user credentials for domain login.

3. **Joined Windows 10 Machine to the New Domain:**
   - Updated the **Preferred DNS Server** to point to the Domain Controller's IP address.
   - Successfully joined the Windows 10 machine (target-PC) to the domain.
   - Verified domain membership by logging in with a newly created domain user account (e.g., Jenny Smith).

4. **Logged in as a Domain User:**
   - Logged into the Windows 10 machine using domain credentials.
   - Verified that the domain environment is functioning as intended.

Congratulations on successfully setting up and verifying your Active Directory environment! 🎉 Let me know if there’s anything else you’d like to configure or explore further.



