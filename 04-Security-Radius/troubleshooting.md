\# 🛠️ Troubleshooting \& Issue Resolution Log



During the integration of pfSense and Windows NPS, several challenges were identified and resolved.



| Issue | Root Cause | Resolution |

| :--- | :--- | :--- |

| \*\*No Logs in Event Viewer\*\* | IP Mismatch (pfSense pointing to `.11` instead of `.20`) | Corrected Host IP in pfSense Authentication Server settings. |

| \*\*Reason Code 66\*\* | Protocol Mismatch (pfSense sent PAP, NPS only allowed MS-CHAPv2) | Enabled \*\*PAP\*\* in the NPS Network Policy Constraints. |

| \*\*Authentication Failed (New Pass)\*\* | Service Cache (NPS kept the old secret in memory) | Executed `Restart-Service Ias` in PowerShell to force-reload the configuration. |

| \*\*NAS Identity Error\*\* | Source IP Mismatch (pfSense used WAN IP `192.168.4.41`) | Bound pfSense RADIUS traffic to the \*\*LAN interface\*\* (10.10.10.1). |

| \*\*Port Blockage\*\* | Host Firewall | Applied PowerShell rule: `New-NetFirewallRule -DisplayName "RADIUS" -Protocol UDP -LocalPort 1812`. |



\## 💡 Key Commands Used

\- `gpupdate /force`: To refresh Group Policy objects.

\- `Restart-Service Ias`: To restart the RADIUS engine without a reboot.

\- `nc -u -z -v 10.10.10.20 1812`: To verify UDP port reachability from pfSense.

