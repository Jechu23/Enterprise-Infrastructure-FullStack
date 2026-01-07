\# 🖥️ Virtual Machine Specifications \& Optimization



This document details the resource allocation and hardware versions used to balance performance and host stability.



\## 📊 Hardware Resource Mapping



| Virtual Machine | Role | vCPU | RAM | Disk (Thin) | Network Adapter |

| :--- | :--- | :--- | :--- | :--- | :--- |

| \*\*PF-GW-01\*\* | Firewall | 2 | 2 GB | 20 GB | VMXNET3 (x3) |

| \*\*DC-CORP-01\*\* | Domain Controller| 2 | 4 GB | 60 GB | VMXNET3 |

| \*\*RAD-SEC-01\*\* | RADIUS Server | 1 | 2 GB | 40 GB | VMXNET3 |

| \*\*WIN10-CLI\*\* | Workstation | 2 | 4 GB | 50 GB | VMXNET3 |

| \*\*SRV-LNX-01\*\* | Linux Endpoint | 1 | 1 GB | 20 GB | VMXNET3 |







\## ⚙️ Optimization Techniques



\### 1. VMXNET3 Adapters

Instead of the legacy E1000 drivers, all VMs utilize \*\*VMXNET3\*\*. This paravirtualized NIC reduces CPU overhead and supports 10Gbps throughput within the vSwitch fabric.



\### 2. Thin Provisioning

To maximize the 500GB SSD Datastore, all VMDKs are \*\*Thin Provisioned\*\*. This allows the lab to grow dynamically while only consuming physical space for data actually written to disk.



\### 3. VMware Tools

VMware Tools is installed on 100% of the fleet. This is critical for:

\* \*\*Memory Ballooning:\*\* Efficient RAM reclamation.

\* \*\*Graceful Shutdown:\*\* Integration with the ESXi power management.

\* \*\*Clock Sync:\*\* Ensuring Kerberos authentication (Active Directory) does not fail due to time skew.



\### 4. Video Memory (VRAM)

For the Windows 10 Workstation, VRAM was increased to \*\*128MB\*\* with 3D acceleration disabled to provide a smooth RDP experience without taxing the host GPU resources.

