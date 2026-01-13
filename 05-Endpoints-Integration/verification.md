\## ✅ Post-Configuration Validation



The following tests confirm the successful integration of the Windows 10 Endpoint into the Managed Infrastructure:



1\. \*\*Domain Membership:\*\* Verified via `Control Panel > System`. The device is correctly registered in the `corp.thomasbytes` domain.

2\. \*\*Active Directory Visibility:\*\* Confirmed that the computer object `WIN10-CLIENT` (or your VM name) appears in the `Computers` Container of the Domain Controller.

3\. \*\*Cross-Subnet Authentication:\*\* Logged in using the domain account `CORP\\oswa` from the \*\*OPT (10.10.20.x)\*\* network, confirming that Kerberos/LDAP traffic is flowing correctly through the pfSense firewall rules.

