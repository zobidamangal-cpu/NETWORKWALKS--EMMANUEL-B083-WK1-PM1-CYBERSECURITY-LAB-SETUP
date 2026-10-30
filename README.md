# NETWORKWALKS--EMMANUEL-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
VirtualBox and Kali Linux setup 
# NETWORKWALKS-<YOUR-NAME>-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP

## Cybersecurity Lab Environment Setup (Week 1)
Building an isolated virtual lab for penetration testing and ethical hacking practice.

### Environment Details
* **Host Operating System:** Linux Mint (constrained RAM environment)
* **Guest Operating System:** Kali Linux (VirtualBox pre-built image)
* **Virtualization Tool:** Oracle VirtualBox
* **Network Architecture:** Custom `NatNetwork` (`10.0.0.0/24` subnet requirement)

---

## Step-by-Step Implementation Guide

### 1. Host & VirtualBox Prerequisites
* Installed Oracle VirtualBox on the Linux Mint host.
* Imported the Kali Linux VirtualBox template.
* Configured VirtualBox display memory and allocated balanced Base Memory to prevent host/guest instability.

### 2. Custom Network Configuration (`NatNetwork`)
* Set up a VirtualBox `NatNetwork` matching the `10.0.0.0/24` subnet requirement.
* Mapped the virtual interface adapter in Kali Linux to translate through the host's wireless connection as a virtual wired adapter (`Wired connection 1` / `eth0`).

### 3. Static IP Configuration
Assigned the required static IP address to Kali Linux via the terminal:
* **IP Address:** `10.0.0.2`[cite: 8]
* **Subnet Mask / Prefix:** `24` (`255.255.255.0`)
* **Gateway:** `10.0.0.1`

---

## Troubleshooting & Linux Mint Host Specific Solutions

Working on a Linux Mint host presented unique hurdles compared to standard Windows instructions. Here is how we resolved them:

1. **Network Hang & DAD Timeout Issue:**
   * *Problem:* The network configuration hung during activation due to IPv4 Duplicate Address Detection (DAD) timeouts.
   * *Solution:* Ran the following `nmcli` command to bypass the timeout:
     ```bash
     sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
     sudo nmcli connection down "Wired connection 1"
     sudo nmcli connection up "Wired connection 1"
     ```

2. **Browser DNS Resolution Error ("Server Not Found"):**
   * *Problem:* Browsers inside Kali could not resolve URLs after applying the static IP because no DNS server was defined.
   * *Solution:* Explicitly added Google's public DNS to the connection configuration:
     ```bash
     sudo nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8"
     sudo nmcli connection down "Wired connection 1"
     sudo nmcli connection up "Wired connection 1"
     ```

3. **VirtualBox "Aborted" State / Host RAM Constraints:**
   * *Problem:* Launching resource-heavy apps like the web browser triggered VM crashes due to tight RAM limits on the Linux Mint host (~3.55 GB total RAM).
   * *Solution:* Optimized the Base Memory cap and maximized video memory allocation in VirtualBox settings for smooth execution.
