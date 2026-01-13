# 🛡️ Group Policy Objects (GPO) & Infrastructure Hardening

This document outlines the security policies enforced across the `corp.thomasbytes.ca` domain. By leveraging Group Policy, we ensure a consistent security baseline and minimize the attack surface of the internal server farm.

## 1. RDP Access Control Policy (Attack Surface Reduction)

This is one of the most critical security controls in the lab. It ensures that Remote Desktop access is not a "default" privilege but a restricted administrative right.

* **GPO Name:** `SEC_Restrict_RDP_Access`
* **Target OU:** `Servers` / `Domain Controllers`
* **Path:** `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment`
* **Setting:** **Allow log on through Remote Desktop Services**
* **Logic:** We have explicitly removed "Remote Desktop Users" and "Everyone" from this policy. Only the **`Builtin\Administrators`** and a dedicated **`IT_Admins`** security group are permitted.
* **Impact:** This mitigates lateral movement. Even if a standard user account is compromised, the attacker cannot pivot into the servers via RDP.



---

## 2. Password & Account Lockout Policy (Fine-Grained Security)

To defend against brute-force and dictionary attacks, a strict password policy is enforced at the domain root.

* **GPO Name:** `Default Domain Policy` (Modified)
* **Key Settings:**
    * **Minimum Password Length:** 12 Characters.
    * **Password Complexity:** Enabled (Must include uppercase, lowercase, numbers, and symbols).
    * **Account Lockout Threshold:** 5 Invalid attempts.
    * **Lockout Duration:** 30 minutes.
* **Logic:** By increasing the length to 12 characters, we exponentially increase the time required for an offline crack, meeting modern enterprise security standards.

---

## 3. Interactive Logon & Information Obfuscation

To prevent information leakage during the login process, we've implemented policies that hide the internal environment details from unauthorized eyes.

* **GPO Name:** `SEC_Logon_Banner`
* **Settings:**
    * **Interactive logon: Message text for users attempting to log on:** "Private System - Authorized Access Only. All activities are monitored."
    * **Interactive logon: Do not display last user name:** Enabled.
* **Logic:** Hiding the last logged-in username prevents an attacker from knowing a valid account name, forcing them to guess both the username and the password.

---

## 💻 Verification & Policy Audit

To ensure the GPOs are correctly applied to the target machines, the following commands are used for auditing:

```powershell
# 1. Force an immediate policy update
gpupdate /force

# 2. Generate a summary report of applied GPOs
gpresult /r

# 3. Verify the RDP Restricted group membership locally
net localgroup "Remote Desktop Users"