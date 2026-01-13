# 🌐 Networking Deep Dive: Virtual Fabric Configuration

This document explains the Layer 2 architecture implemented within VMware ESXi to ensure strict traffic isolation between the enterprise zones.

## 🏗️ Virtual Switch Architecture

I implemented a dual-vSwitch design to separate external traffic from internal east-west traffic.

### vSwitch0: The Edge Gateway
- **Type:** Standard vSwitch
- **Uplink:** `vmnic0` (Physical NIC)
- **Security:** Standard settings.
- **Port Groups:** - **WAN-Uplink:** Provides the pfSense WAN interface with direct access to the external gateway/ISP.

### vSwitch1: The Private Backplane
- **Type:** Standard vSwitch
- **Uplink:** **None (Isolated)**. 
- **Purpose:** This switch acts as a "Virtual Wire" that never leaves the physical host. All inter-VM traffic for the LAN and OPT segments stays within the hypervisor RAM, ensuring maximum speed and security.
- **Port Groups:**
    - **LAN-Core:** Dedicated to servers (DC, RADIUS).
    - **OPT-Isolated:** Dedicated to client workstations.



## 🛡️ Security Policies (Layer 2)
To prevent common network attacks within the virtual environment, the following security overrides were applied to all Port Groups:

* **Promiscuous Mode:** `Reject` (Prevents VMs from sniffing traffic on the same vSwitch).
* **MAC Address Changes:** `Reject` (Prevents MAC spoofing).
* **Forged Transmits:** `Reject` (Prevents a VM from sending traffic with a fake source MAC).

## 🚦 Traffic Flow Logic
1. A packet from a **Client (OPT)** destined for the **Internet (WAN)** must enter the pfSense `vmx2` interface.
2. pfSense inspects the packet against Firewall Rules.
3. If allowed, pfSense routes it out through the `vmx0` interface to **vSwitch0**.
4. **Conclusion:** No bypass is possible because the Client Port Group has no physical uplink.