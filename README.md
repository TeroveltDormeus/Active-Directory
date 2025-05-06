# Active Directory Project

## Project Overview

This lab provides a hands-on experience in setting up a virtualized environment for cybersecurity testing and exploration. Participants will create and configure multiple virtual machines (VMs) running Windows 10, Kali Linux, Windows Server, and Ubuntu Server. The lab focuses on essential skills such as network configuration, security tool installation (e.g., Splunk, Sysmon), endpoint monitoring, and conducting security tests (e.g., brute force attacks with Crowbar). 

### Key Objectives:
- Joining Windows machines to an Active Directory domain.
- Enabling Remote Desktop.
- Automating tasks using PowerShell scripting.
- Configuring Splunk for log monitoring and analysis.
- Simulating cybersecurity attacks and analyzing telemetry data.



## Tools and Technologies

The project uses the following tools and technologies:
- **Oracle VM VirtualBox Manager**: For creating and managing virtual machines (VMs).
- **Active Directory (AD)**: For centralized domain management and user authentication.
- **PowerShell**: For scripting and task automation.
- **Splunk Server**: For log analysis, monitoring, and security information/event management (SIEM).
- **Splunk Universal Forwarder**: For collecting and forwarding log data to Splunk.
- **Sysmon**: For endpoint monitoring and logging system activity on Windows.
- **Operating Systems**:
  - Windows Server 2022: For Active Directory Domain Services setup.
  - Ubuntu Server: As a Splunk server.
  - Microsoft Windows 10: As the target machine.
  - Kali Linux: As the attacker machine (using tools like Hydra).

---

## Project Steps

### Phase 1: Environment Setup
1. **Diagram the Project**: Use [draw.io](https://app.diagrams.net/) to visually represent the architecture.
2. **Install Oracle VM VirtualBox Manager**: [Download here](https://www.virtualbox.org/).
3. **Create and Configure Virtual Machines**:
   - **Windows 10**: [Download ISO](https://www.microsoft.com/software-download/windows10).
   - **Kali Linux**: [Download Pre-Built Virtual Machine](https://www.kali.org/get-kali/).
   - **Windows Server 2022**: [Download ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022).
   - **Ubuntu Server**: [Download ISO](https://ubuntu.com/download/server).

   Follow the detailed instructions in [docs/VM-Setup.md](docs/VM-Setup.md).

---

### Phase 2: Network Configuration
1. Set up a NAT Network in VirtualBox.
2. Assign static IPs to each VM:
   - Splunk VM: `192.168.10.10/24`
   - Target-PC: `192.85.22.111`
   - ADDC01: `192.85.22.105`
   - Kali Linux: `192.168.10.250`

Details in [docs/Network-Config.md](docs/Network-Config.md).

---

### Phase 3: Active Directory Setup
1. **Install Active Directory Domain Services (ADDS)** on Windows Server.
2. **Create a new domain**: e.g., `yourdomainname.local`.
3. **Join Windows 10 machine to the domain**.
4. **Create users**: e.g., Jenny Smith, Maria Rodriguez.

Details in [docs/AD-Setup.md](docs/AD-Setup.md).

---

### Phase 4: Splunk Configuration
1. Install Splunk Server and Universal Forwarder.
2. Configure `inputs.conf` to send data to Splunk Server.
3. Create an `endpoint` index in Splunk.
4. Verify data ingestion and set up dashboards.

Details in [docs/Splunk-Config.md](docs/Splunk-Config.md).

---

### Phase 5: Brute Force Attack Simulation
1. Enable RDP on the Windows 10 target machine.
2. Use `Hydra` on the Kali Linux machine to simulate an RDP brute force attack.
3. Analyze telemetry in Splunk and create dashboards.

Details in [docs/Brute-Force.md](docs/Brute-Force.md).



---

## References
- **Active Directory Project by MyDFIR**: [YouTube Tutorial](https://www.youtube.com/c/MyDFIR)
- **Splunk Documentation**: [Splunk Docs](https://docs.splunk.com/)
- **Sysmon Configuration**: [Sysmon GitHub](https://github.com/SwiftOnSecurity/sysmon-config)

---

