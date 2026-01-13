\# 💿 pfSense Virtual Appliance Deployment \& Initialization



This document outlines the step-by-step installation and initial bootstrap process for the \*\*pfSense 2.7.x CE\*\* gateway within the VMware ESXi hypervisor.



\## 📥 1. Virtual Machine Provisioning

The VM was created using the following specifications to ensure stability and compatibility with FreeBSD:



\* \*\*Guest OS Family:\*\* Other

\* \*\*Guest OS Version:\*\* FreeBSD 12 or later (64-bit)

\* \*\*Hardware Version:\*\* 13 (or highest supported by ESXi 6.7)

\* \*\*CPU:\*\* 2 vCPUs (AES-NI support enabled)

\* \*\*Memory:\*\* 2 GB RAM (Reserved)

\* \*\*Network Interfaces (3x VMXNET3):\*\*

&nbsp;   \* Adapter 1 -> \*\*WAN-Uplink\*\* Port Group

&nbsp;   \* Adapter 2 -> \*\*LAN-Core\*\* Port Group

&nbsp;   \* Adapter 3 -> \*\*OPT-Isolated\*\* Port Group



---



\## 🛠️ 2. Phase 1: Console-Based Setup

After booting from the ISO, the initial configuration was performed via the ESXi web console:



1\.  \*\*Interface Assignment:\*\* \* Detected MAC addresses to correctly map `vmx0` to WAN, `vmx1` to LAN, and `vmx2` to OPT1.

2\.  \*\*IP Addressing:\*\* \* Assigned the static Management IP: `172.16.10.1/24` to the LAN interface.

3\.  \*\*VLAN Configuration:\*\* \* Initially skipped to focus on base Layer 3 connectivity.







---



\## 🌐 3. Phase 2: WebGUI Post-Installation Wizard

Once the LAN IP was reachable from a management workstation, the Setup Wizard was completed:



\* \*\*Hostname:\*\* `pf-gw-01`

\* \*\*Domain:\*\* `corp.thomasbytes.ca`

\* \*\*DNS Servers:\*\* Configured to `172.16.10.10` (Internal AD DC) with a fallback to `8.8.8.8`.

\* \*\*Timezone:\*\* Synchronized with the host (America/Lima or your local zone).

\* \*\*Admin Password:\*\* Changed from default `pfsense` to a hardened passphrase.



---



\## 🔒 4. Hardening \& Initial Optimization

To prepare the gateway for the production lab, the following "Day 0" tasks were performed:



\* \*\*Secure WebGUI:\*\* Switched protocol from HTTP to \*\*HTTPS\*\* on a non-standard port (optional).

\* \*\*Secure Shell (SSH):\*\* Enabled SSH with \*\*RSA Key-only authentication\*\* for remote command-line management.

\* \*\*Unused Services:\*\* Disabled unnecessary services (like DHCP on WAN) to reduce the attack surface.

\* \*\*Package Management:\*\* Installed `openvpn-client-export` to facilitate the deployment of VPN profiles.



---



\## 📸 Installation Evidence

\* \*\*Boot Sequence:\*\* `screenshots/pfsense-boot.png`

\* \*\*Initial Interface Mapping:\*\* `screenshots/console-interfaces.png`

\* \*\*WebGUI Dashboard (Final):\*\* `screenshots/pfsense-dashboard.png`

