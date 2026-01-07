\## ⚠️ Troubleshooting: Registration Error

During the AD registration process, the following error occurred:

> \*"The task was not completed. You may not have sufficient privileges..."\*



\*\*Root Cause:\*\* The account used lacked write permissions to the 'RAS and IAS Servers' security group in Active Directory.



\*\*Resolution:\*\*

1\. Logged into the RADIUS server using the `CORP\\Administrator` domain account.

2\. Executed the NPS Console with elevated privileges (Run as Administrator).

3\. Verified the RADIUS server object was successfully added to the \*\*RAS and IAS Servers\*\* group in the Domain Controller.

## 👥 Active Directory Group Strategy
To manage access control effectively, specific Security Groups were created in the Domain Controller. These groups are used as conditions within the NPS Network Policies.

### Security Groups Created:
* **VPN_Users:** Standard users authorized to establish VPN connections and network authentication.
* **VPN_Admins:** Elevated users with administrative access to network infrastructure via RADIUS.

**Configuration:**
- **Scope:** Global
- **Type:** Security
- **Access Control:** Managed via the "Control access through NPS Network Policy" setting in user Dial-in properties.

