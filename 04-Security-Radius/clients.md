# 🤝 Active Directory & NPS Integration Detail

This document focuses on the synchronization between the Identity Store (Active Directory) and the Policy Engine (NPS), including the critical troubleshooting steps taken to ensure the RADIUS server was properly authorized within the domain.

## ⚠️ Critical Troubleshooting: NPS Registration Error

During the initial deployment of the RADIUS role, a permission-based error prevented the NPS service from reading user dial-in properties from Active Directory.

> **Error Message:** *"The task was not completed. You may not have sufficient privileges to register the service in Active Directory."*

### 🔍 Root Cause Analysis
The Network Policy Server (NPS) must be a member of the **'RAS and IAS Servers'** built-in security group in the domain. Without this membership, the service cannot access the `msRADIUSServiceType` and other sensitive attributes of user objects required for authentication.

### ✅ Resolution Workflow
1. **Administrative Elevation:** Re-authenticated to the RADIUS server (`RAD-SEC-01`) using the **Domain Admin** (`CORP\Administrator`) account to ensure full delegation rights.
2. **Manual Registration:** - Opened the NPS Management Console.
   - Right-clicked **NPS (Local)** and selected **"Register Server in Active Directory"**.
3. **Group Validation:** Verified via *Active Directory Users and Computers (ADUC)* that the computer object for the RADIUS server was successfully added to the `RAS and IAS Servers` group.
4. **Service Restart:** Executed `Restart-Service Ias` via PowerShell to commit the new security token.



---

## 👥 Security Group Strategy & Role-Based Access Control (RBAC)

To enforce the **Principle of Least Privilege (PoLP)**, network access is not granted to individuals, but to specific Security Groups. This allows for scalable management of the VPN population.

### Infrastructure Security Groups:
| Group Name | Scope | Description | Access Level |
| :--- | :--- | :--- | :--- |
| **VPN_Users** | Global | Standard employees/users authorized for remote work. | Basic VPN Tunnel Access. |
| **VPN_Admins** | Global | IT Staff with administrative responsibilities. | Full access to Infrastructure Management. |

### 🛠️ User Object Configuration (Dial-in)
For these groups to function with NPS, the **Dial-in** tab on user objects was managed with the following logic:
* **Network Access Permission:** Set to **"Control access through NPS Network Policy"**.
* **Logic:** This delegates the decision-making entirely to the NPS policy engine, allowing us to manage complex conditions (like time-of-day or device type) rather than using static "Allow/Deny" flags on individual accounts.



---

## 💻 PowerShell Audit
To verify that the Security Groups are correctly populated and the NPS server is authorized, the following commands are used:

```powershell
# Check membership of the VPN_Users group
Get-ADGroupMember -Identity "VPN_Users" | Select-Object Name, SamAccountName

# Verify NPS Server registration status in the domain
Get-ADGroupMember -Identity "RAS and IAS Servers" | Where-Object {$_.Name -like "*RAD*"}