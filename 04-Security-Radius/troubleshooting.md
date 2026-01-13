# 🛠️ Troubleshooting & Issue Resolution Log (Post-Mortem)

This log documents the critical challenges encountered during the **pfSense-to-NPS** integration and the systematic approach used to resolve them. documenting these "lessons learned" ensures infrastructure reliability and knowledge retention.

## 📋 Systematic Issue Tracking

| Issue | Root Cause Analysis | Resolution |
| :--- | :--- | :--- |
| **Silent Failures (No Logs)** | **IP Mismatch:** pfSense was targeting an incorrect host IP for the RADIUS server. | Verified IP using `ipconfig` on the DC and updated pfSense Authentication settings to `172.16.10.11`. |
| **NPS Reason Code 66** | **Protocol Mismatch:** pfSense was sending credentials via PAP while NPS strictly required MS-CHAPv2. | Temporarily enabled **PAP** in NPS Constraints for diagnostic testing, then aligned both to **MS-CHAPv2** for production. |
| **Shared Secret Mismatch** | **Service Cache:** Even after updating the secret, NPS retained the old hash in memory. | Executed `Restart-Service Ias` in PowerShell to force-reload the policy engine and clear the cache. |
| **NAS Identifier Rejection** | **Identity Mismatch:** pfSense used the WAN IP (`192.168.4.x`) as source, but NPS expected the LAN IP. | Configured **NAS IP Address** binding in pfSense to explicitly use `172.16.10.1`. |
| **UDP Timeout** | **Host Firewall:** The Windows Advanced Firewall was dropping unsolicited UDP 1812/1813 traffic. | Provisioned a scoped inbound rule via PowerShell to allow RADIUS traffic from the pfSense Gateway. |

---

## 💡 Essential Diagnostic Toolkit

To maintain the AAA environment, the following commands are utilized for real-time monitoring and verification:

### 1. Network Reachability (pfSense Shell)
Before testing authentication, we verify that the UDP port is open and reachable through the virtual switches:
```bash
# Verify UDP 1812 reachability to the NPS Server
nc -u -z -v 172.16.10.11 1812
