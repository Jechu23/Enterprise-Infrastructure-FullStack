\# 🖥️ Windows Domain Join \& User Provisioning



This document details the process of onboarding the Windows 10 Workstation into the `corp.thomasbytes.ca` forest.



\## ⚙️ Pre-Requisites Check

Before the join, the following network parameters were validated on the client:

\- \*\*DNS Server:\*\* Pointing to `172.16.10.10` (Crucial for SRV record lookup).

\- \*\*IP Assignment:\*\* Obtained via DHCP from the Domain Controller.

\- \*\*Connectivity:\*\* Successful ping to `corp.thomasbytes.ca`.



\## 🛠️ Step-by-Step Join

1\. \*\*System Properties:\*\* Navigated to \*Computer Name / Domain Changes\*.

2\. \*\*Domain:\*\* Entered `corp.thomasbytes.ca`.

3\. \*\*Credentials:\*\* Authenticated using the `CORP\\Administrator` account.

4\. \*\*Result:\*\* "Welcome to the corp.thomasbytes.ca domain."



\## 🛡️ GPO Verification

After the mandatory reboot, we verified that the security policies from \*\*Module 03\*\* were applied:

\- \*\*Login Banner:\*\* The legal warning message is displayed before login.

\- \*\*RDP Restrictions:\*\* Only authorized IT Admins can access the machine remotely.

\- \*\*Password Policy:\*\* Users are prompted to meet the 12-character complexity requirement.

