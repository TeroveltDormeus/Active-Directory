
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
To set a static IP address on your Splunk VM, follow the steps below:

1. **Start the Splunk VM**  
   - Run the following command to display the current IP address:  
     ```bash
     ip a
     ```
   - Note the current IP address, as it will be updated to a static IP address (`192.168.10.10/24`).

2. **Edit the Netplan Configuration File**  
   - Run this command to open the current Netplan configuration file:  
     ```bash
     sudo nano /etc/netplan/00-installer-config.yaml
     ```  
     - Use the **Tab** key to auto-complete the file name. If you only have one file, it should open.

3. **Update the Configuration File**  
   Replace the contents of the file with the following configuration:  
   ```yaml
   network:
     ethernets:
       enp0s3:
         dhcp4: no
         addresses: [192.168.10.10/24]
         nameservers:
           addresses: [8.8.8.8]
         routes:
           - to: default
             via: 192.168.10.1
     version: 2
   ```

4. **Save the Configuration**  
   - Press **Ctrl+O** to save the file.  
   - Press **Enter** to confirm the file name.  
   - Press **Ctrl+X** to exit the editor.

5. **Apply the Netplan Configuration**  
   - Run the following command to apply the configuration:  
     ```bash
     sudo netplan apply
     ```
   - Verify the IP address using:  
     ```bash
     ip a
     ```

6. **Reboot and Verify the Connection**  
   - Reboot the VM:  
     ```bash
     sudo reboot
     ```
   - After rebooting, verify that the static IP address persists and is set to `192.168.10.10/24`.  
     ```bash
     ip a
     ```
   - Test the connection to the internet:  
     ```bash
     ping google.com
     ```

### Troubleshooting (If the above steps don't work):
- **Different Configuration File**:  
  If your configuration file differs (e.g., `50-cloud-init.yaml`), the changes might not persist after a reboot. To resolve this:  
  1. Create a new custom file (`00-installer-config.yaml`) in `/etc/netplan/`:  
     ```bash
     sudo nano /etc/netplan/00-installer-config.yaml
     ```
  2. Add the updated configuration as shown earlier.  
  3. Backup and delete the old file:  
     ```bash
     sudo cp /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.bak
     sudo rm /etc/netplan/50-cloud-init.yaml
     ```
  4. Reapply Netplan:  
     ```bash
     sudo netplan apply
     ```
  5. Reboot and confirm the static IP persists.
---

