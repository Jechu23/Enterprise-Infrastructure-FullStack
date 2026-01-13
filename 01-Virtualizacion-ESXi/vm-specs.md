\# 🖥️ Virtual Machine Specifications \& Resource Optimization



This document details the granular resource allocation, virtual hardware versions, and optimization strategies implemented to ensure a high-performance environment while maintaining host stability on the physical HP Z800 workstation.



\## 📊 Hardware Resource Mapping



To balance the workload across physical Xeon cores, the following vCPU and RAM profiles were assigned to each functional node:



| Virtual Machine | Role | vCPU | RAM | Storage (Thin) | Network Adapter |

| :--- | :--- | :--- | :--- | :--- | :--- |

| \*\*PF-GW-01\*\* | Perimeter Firewall | 2 | 2 GB | 20 GB | VMXNET3 (x3) |

| \*\*DC-CORP-01\*\* | Domain Controller | 2 | 4 GB | 60 GB | VMXNET3 |

| \*\*RAD-SEC-01\*\* | RADIUS (NPS) Server | 1 | 2 GB | 40 GB | VMXNET3 |

| \*\*SRV-LNX-01\*\* | Debian 12 (Headless) | 1 | 1 GB | 20 GB | VMXNET3 |



---



\## ⚙️ Advanced Optimization Techniques



\### 1. High-Performance VMXNET3 Adapters

Standard legacy drivers (e.g., E1000) were bypassed in favor of \*\*VMXNET3\*\* paravirtualized NICs. This significantly reduces CPU overhead during high-bandwidth VPN encryption tasks and supports 10Gbps line-rate throughput within the internal vSwitch fabric.



\### 2. Intelligent Storage Allocation (Thin Provisioning)

To maximize the efficiency of the 500GB SSD Datastore, all Virtual Machine Disks (\*\*VMDKs\*\*) are \*\*Thin Provisioned\*\*. This allows the infrastructure to scale dynamically, consuming physical storage only as data is written, which prevents over-provisioning and extends the lifecycle of the local SSD.



\### 3. VMware Tools Integration \& Lifecycle Management

VMware Tools is mandated across 100% of the fleet. This integration is critical for enterprise-grade stability:

\* \*\*Memory Ballooning:\*\* Facilitates efficient RAM reclamation by the ESXi hypervisor during contention.

\* \*\*Graceful Shutdown:\*\* Integration with the ESXi power management sub-system for safe maintenance windows.

\* \*\*Time Synchronization:\*\* Crucial for \*\*Kerberos Authentication\*\* (Active Directory) to prevent "Clock Skew" errors that disrupt domain logins.



\### 4. Headless Optimization \& VRAM Management

For the remote management nodes, Video RAM (VRAM) was carefully managed:

\* \*\*Windows Server Nodes:\*\* Assigned \*\*128MB\*\* of VRAM to ensure responsive UI performance during initial RDP/Console configuration.

\* \*\*Linux Nodes:\*\* Optimized for \*\*Headless Mode\*\*, minimizing the memory footprint and focusing all available cycles on networking and security services.



---



\## 🛠️ Performance Analysis

During the OpenVPN/RADIUS validation phase, host CPU utilization remained under 15%, and memory pressure was negligible, confirming that the current allocation strategy provides an optimal balance between density and performance.

