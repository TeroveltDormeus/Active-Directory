Here is a step-by-step guide to install Splunk on your VM:

### 1. **Download Splunk**
   - Navigate to the [Splunk Free Trial](https://www.splunk.com/en_us/download/splunk-enterprise.html) page.
   - Select **Linux** as the operating system.
   - Choose the **".deb"** file format.
   - Save the file in your preferred directory.

### 2. **Prepare Your Splunk VM**
   - Run the following commands to install VirtualBox Guest Additions:
     ```bash
     sudo apt-get install virtualbox
     sudo apt-get install virtualbox-guest-additions-iso
     ```
   - When prompted, type **"y"** and press **Enter**.
   - After installation, clear the terminal:
     ```bash
     clear
     ```
   - Navigate to **Devices > Shared Folders > Shared Folders Settings** in VirtualBox.
     - Add a folder:
       - **Folder Path**: Select the path where the Splunk installer was saved.
       - **Folder Name**: Leave the default name (e.g., `AD-Project`).
       - Select the options **Read-only**, **Auto-mount**, and **Make Permanent**.
     - Click **"Ok"**.

   - Reboot the Splunk VM:
     ```bash
     sudo reboot
     ```

### 3. **Add User to Shared Folder Group**
   - Log back into the VM and add the user to the `vboxsf` group:
     ```bash
     sudo adduser username vboxsf
     ```
     Replace `username` with your actual username.
   - If you receive the error **"group vboxsf does not exist"**, install additional guest utilities:
     ```bash
     sudo apt-get install virtualbox
     sudo apt-get install virtualbox-guest-utils
     sudo reboot
     sudo adduser username vboxsf
     ```

### 4. **Create and Mount the Shared Directory**
   - Create a new directory called `Share`:
     ```bash
     mkdir Share
     ls
     ```
     Confirm that the directory `Share` is listed.
   - Mount the shared folder onto the directory:
     ```bash
     sudo mount -t vboxsf -o uid=1000,gid=1000 <directory name where .deb file is located> share/
     ```
     Example:
     ```bash
     sudo mount -t vboxsf -o uid=1000,gid=1000 AD-Project share/
     ```
   - If you forget the shared folder name, check it in **Devices > Shared Folders > Shared Folders Settings**.
   - If you encounter an error, log out and redo the steps:
     ```bash
     exit
     ```
   - Verify that the share is mounted:
     ```bash
     ls -la
     ```

### 5. **Access the Shared Directory**
   - Change directories into the shared folder:
     ```bash
     cd share
     clear
     ls -la
     ```
   - Ensure the Splunk installer file is listed in this directory.

### 6. **Install Splunk**
   - Run the following command to install Splunk:
     ```bash
     sudo dpkg -i splunk
     ```
     - Hit **Tab** to auto-complete the file name.
   - Once the installation is complete, change into the Splunk directory:
     ```bash
     cd /opt/splunk
     ls -la
     ```
   - Verify that all users and groups belong to Splunk.

### 7. **Switch to the Splunk User**
   - Change into the Splunk user:
     ```bash
     sudo -u splunk bash
     ```
   - Navigate to the `bin` directory:
     ```bash
     cd bin
     ls -la
     ```
   - Start the Splunk installer:
     ```bash
     ./splunk start
     ```
     - Press **"q"** to scroll through the license terms.
     - Type **"y"** to accept.
     - Set an administrator username and password.

### 8. **Enable Splunk at Boot**
   - Exit the Splunk user:
     ```bash
     exit
     ```
   - Enable Splunk to start automatically on VM boot:
     ```bash
     sudo ./splunk enable boot-start -user splunk
     ```

With these steps, Splunk will now be installed and configured to run with user `splunk` on every VM reboot. Let me know if you encounter any issues!
