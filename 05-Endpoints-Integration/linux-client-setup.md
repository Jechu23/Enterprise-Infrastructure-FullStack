\# 🐧 Linux Client Integration \& Audit



This document describes the setup of a Debian-based endpoint used for network auditing and cross-platform service validation.



\## 🌐 Network Configuration

The Linux client is located in the \*\*OPT1 (172.16.20.0/24)\*\* segment.

\- \*\*DHCP Client:\*\* Successfully leased an IP from the Windows DHCP Server.

\- \*\*DNS Resolution:\*\* Configured via `/etc/resolv.conf` to use the Domain Controller.



\## 🧪 Identity \& Connectivity Tools

We use the Linux endpoint to verify the "Identity Bridge" rules in pfSense:

```bash

\# Verify DNS resolution of the Domain Controller

nslookup dc-corp-01.corp.thomasbytes.ca



\# Test SMB connectivity for file sharing

smbclient -L //172.16.10.10 -U CORP/test\_user

