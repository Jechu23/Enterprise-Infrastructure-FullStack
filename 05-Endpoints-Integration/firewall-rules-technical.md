\# 🛡️ Firewall Implementation: OPT1 to LAN (Identity Traffic)



To maintain a \*\*Zero Trust\*\* posture, communication between the isolated \*\*OPT1 (172.16.20.0/24)\*\* segment and the \*\*Identity Core\*\* in the LAN segment is strictly limited. Only the protocols necessary for domain operations and authentication are permitted, while all other inter-VLAN traffic is implicitly denied.



\## 🏷️ Network Aliases (pfSense Management)



Aliases were implemented in pfSense to simplify administration and ensure consistency. Instead of managing individual IPs and ports across multiple rules, we use these logical abstractions:



\### 1. Host Aliases

| Alias Name | IP Address | Description |

| :--- | :--- | :--- |

| `DC\_Server` | `172.16.10.10` | Primary Domain Controller (DC-CORP-01) |



\### 2. Port Aliases (`AD\_Services`)

This alias groups the critical ports required for Active Directory, DNS, and RADIUS handshakes:



| Protocol | Ports | Service |

| :--- | :--- | :--- |

| \*\*DNS\*\* | 53 (UDP/TCP) | Name Resolution |

| \*\*Kerberos\*\* | 88, 464 (TCP/UDP) | Ticket-based Authentication |

| \*\*RPC\*\* | 135 (TCP) | RPC Endpoint Mapper |

| \*\*LDAP/S\*\* | 389, 636 (TCP/UDP) | Directory Access |

| \*\*SMB/CIFS\*\* | 445 (TCP) | GPO Synchronization \& Netlogon |

| \*\*Global Catalog\*\* | 3268, 3269 (TCP) | Multi-domain Forest Search |

| \*\*RADIUS\*\* | 1812, 1813 (UDP) | Authentication \& Accounting |



---



\## 🚦 Rule Logic: "The Identity Bridge"



The following rules were applied to the \*\*OPT1 Interface\*\* in pfSense. Traffic is evaluated top-down; if a packet does not match these specific requirements, it is dropped by the default "Deny All" policy.



| Action | Protocol | Source | Destination | Port | Description |

| :--- | :--- | :--- | :--- | :--- | :--- |

| \*\*PASS\*\* | TCP/UDP | `OPT1\_Net` | `DC\_Server` | `AD\_Services` | Allow Workstations to bind to AD/DNS/NPS. |

| \*\*PASS\*\* | ICMP | `OPT1\_Net` | `DC\_Server` | \* | Allow Echo Request for diagnostics. |

| \*\*REJECT\*\* | \* | `OPT1\_Net` | `LAN\_Net` | \* | Block all other access to the Management LAN. |







---



\## 🔍 Implementation Impact: GPO \& Authentication

By using this granular approach:

1\.  \*\*GPO Synchronization:\*\* The client workstation can reach the `SYSVOL` share via SMB (445) to pull security policies.

2\.  \*\*Authentication Handshake:\*\* The client can reach the KDC (Kerberos) on port 88 for ticket granting.

3\.  \*\*Lateral Movement Prevention:\*\* If an endpoint in OPT1 is compromised, the attacker cannot scan or attack other hosts in the LAN (172.16.10.x) because the firewall only permits communication with the specific `DC\_Server` on defined ports.



---



\## ✅ Technical Verification

To confirm the rules are functioning as intended from a \*\*Win10-CLI\*\* endpoint:



```powershell

\# Test connectivity to AD DS ports

Test-NetConnection 172.16.10.10 -Port 445  # SMB (GPO)

Test-NetConnection 172.16.10.10 -Port 389  # LDAP



\# Expected Result: TcpTestSucceeded : True

