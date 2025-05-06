### Summary of Brute Force Attack Simulation Guide:

---

### **1. Configure Network on Kali Linux**
- Log onto the Kali Linux machine using default credentials.
- Set a static IP address (`192.168.10.250`) by:
  - Editing **Network Connections** and saving the configuration.
  - Testing connectivity using:
    ```bash
    ip a
    ping google.com
    ping 192.168.10.10
    ```
- Update and upgrade repositories:
  ```bash
  sudo apt-get update && sudo apt-get upgrade -y
  ```

---

### **2. Setup for the Attack**
- **Create a Directory**:  
  Run the following command to organize files:
  ```bash
  mkdir AD-Project
  ```
- **Enable Remote Desktop Protocol (RDP)** on the Windows 10 target-PC:
  1. Go to **PC Properties** > **Advanced System Settings** > **Remote Tab**.
  2. Enable **Allow remote connections**.
  3. Add users (e.g., **Jenny Smith**) with the password **cutti4l**.

---

### **3. Launch the Attack**
- **Prepare Attack Files**:
  1. Create two files in the **AD-Project** directory:
     - `user.txt`: Contains usernames.
     - `pass.txt`: Contains passwords.
  2. Use the Hydra tool to brute force RDP on the target-PC:
     ```bash
     hydra -L user.txt -P pass.txt rdp://192.85.22.111 -V
     ```

---

### **4. Analyze Telemetry in Splunk**
- Access Splunk on the target-PC:
  1. Open a browser and navigate to `http://192.85.22.7:8000`.
  2. Use **Search & Reporting** to analyze the telemetry.
  3. Search for:
     ```spl
     index=endpoint cutti4l
     ```
  4. Identify **Event Code 5379**, which indicates failed RDP login attempts.
  5. Correlate events based on timestamps to confirm the attack.

---

### **5. Create a Splunk Dashboard**
- Save the search results as a new dashboard:
  1. Go to the **Dashboard** section in Splunk.
  2. Add a chart using the SPL query:
     ```spl
     index=endpoint cutti4l EventCode=5379
     ```
  3. Visualize the brute force activity.

---

### **Outcome**
This guide successfully demonstrates:
1. Configuring and simulating a brute force attack using the Hydra tool on Kali Linux.
2. Monitoring and analyzing the attack using Splunk Server.
3. Creating a Splunk dashboard for visualizing RDP attack telemetry.

---

### **Acknowledgments**
- **MyDFIR on YouTube** for the tutorial.
- Tools used:
  - **Sysmon**
  - **Splunk Universal Forwarder**
  - **Hydra**
  - **Splunk Documentation**

Congratulations on completing the project and gaining hands-on experience in simulating and monitoring a brute force attack! 🎉
