\## ⚠️ Troubleshooting: Registration Error

During the AD registration process, the following error occurred:

> \*"The task was not completed. You may not have sufficient privileges..."\*



\*\*Root Cause:\*\* The account used lacked write permissions to the 'RAS and IAS Servers' security group in Active Directory.



\*\*Resolution:\*\*

1\. Logged into the RADIUS server using the `CORP\\Administrator` domain account.

2\. Executed the NPS Console with elevated privileges (Run as Administrator).

3\. Verified the RADIUS server object was successfully added to the \*\*RAS and IAS Servers\*\* group in the Domain Controller.

