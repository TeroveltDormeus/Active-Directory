

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

Let me know if you'd like further clarification or assistance with any of the steps!
