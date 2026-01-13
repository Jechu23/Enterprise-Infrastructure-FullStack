\# 🛠️ Domain Controller Promotion \& Forest Setup



This document details the step-by-step initialization of the `corp.thomasbytes.ca` forest and the promotion of \*\*DC-CORP-01\*\* as the primary Domain Controller.



\## 1. Role Installation

The deployment began with the installation of the \*\*Active Directory Domain Services (AD DS)\*\* binaries via Server Manager. 

\- \*\*Binary Source:\*\* Windows Server 2022 Standard.

\- \*\*Method:\*\* Role-based installation including Management Tools (RSAT).



\## 2. Forest Configuration (Deployment Configuration)

During the promotion wizard, the following architectural decisions were made:

\- \*\*Deployment Operation:\*\* Add a new forest.

\- \*\*Root Domain Name:\*\* `corp.thomasbytes.ca`

\- \*\*Forest/Domain Functional Level:\*\* Windows Server 2016 (Ensures a balance between modern security features and backward compatibility for the lab).

\- \*\*Additional Options:\*\* - \*\*DNS Server:\*\* Installed and integrated during promotion.

&nbsp;   - \*\*Global Catalog (GC):\*\* Enabled (Required for the first DC in the forest).







\## 3. Post-Promotion Tasks (DCPromo)

After the mandatory reboot, the following "Day 1" configurations were applied to ensure domain health:



\### 🔑 Directory Services Restore Mode (DSRM)

A strong DSRM administrator password was configured and vaulted. This is critical for offline maintenance or forest recovery scenarios.



\### 📁 Database Path Customization

To optimize I/O performance and follow security best practices, the AD database and log files are stored in the default protected paths, but prepared for future migration to dedicated virtual disks:

\- \*\*Database folder:\*\* `C:\\Windows\\NTDS`

\- \*\*Log files folder:\*\* `C:\\Windows\\NTDS`

\- \*\*SYSVOL folder:\*\* `C:\\Windows\\SYSVOL` (Critical for GPO replication).



\## 4. Verification of AD Health

To confirm that the promotion was successful and the database is consistent, the following health checks were performed:



```powershell

\# 1. Check the functional levels

Get-ADDomain | fl Name, DomainMode, ForestMode



\# 2. Verify that the DC is advertising as a Global Catalog

Get-ADDomainController -Identity DC-CORP-01 | Select-Object IsGlobalCatalog, IsPDC



\# 3. Test Netlogon and Sysvol shares

net share

