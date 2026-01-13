# 🌐 Enterprise DNS Infrastructure & Name Resolution

The Domain Name System (DNS) is the foundational service for the `corp.thomasbytes.ca` forest. It is Active Directory-Integrated, ensuring that all service location (SRV) records, host mappings, and reverse lookups are secure, replicated, and dynamic.

## ⚙️ External Forwarders & Recursion
To maintain internal resolution for the domain while allowing endpoints to access the internet, the DNS server is configured with upstream forwarders. This prevents the DC from performing root hints recursion, improving performance and security.

* **Primary Forwarder:** `8.8.8.8` (Google Public DNS)
* **Secondary Forwarder:** `1.1.1.1` (Cloudflare DNS)
* **Policy:** Requests for non-authoritative zones are forwarded only if the internal cache cannot satisfy the query.

---

## 🔄 Reverse Lookup Zones (L3-to-L7 Mapping)
A critical component of this lab was the manual provisioning of the **Reverse Lookup Zone**. This ensures that security tools and VPN clients can perform "Pointer" (PTR) lookups to verify server identities.

* **Zone Name:** `10.16.172.in-addr.arpa`
* **Network ID:** `172.16.10.x`
* **Status:** Active / AD-Integrated.
* **Technical Note:** The creation of this zone resolved an initial `NXDOMAIN` error during VPN client validation, allowing the command `nslookup 172.16.10.10` to successfully return the hostname `dc.corp.thomasbytes.ca`.



---

## 🛡️ DNS Security & Integrity
- **Dynamic Updates:** Set to **Secure Only**. This ensures that only domain-joined machines can register or update their DNS records, preventing DNS hijacking or spoofing within the LAN.
- **Scavenging/Aging:** Enabled to automatically remove stale resource records from VPN clients that no longer have an active lease, keeping the database clean.

---

## ✅ Health & SRV Verification
Active Directory relies on **SRV Records** to locate domain services (LDAP, Kerberos, etc.). The following PowerShell and CMD commands are used to verify the integrity of these records from the core and remote endpoints:

```powershell
# 1. Verify Authoritative Domain Resolution
nslookup corp.thomasbytes.ca

# 2. Verify LDAP SRV Records (Critical for AD Auth)
nslookup -type=srv _ldap._tcp.dc._msdcs.corp.thomasbytes.ca

# 3. Verify Reverse Lookup (PTR)
nslookup 172.16.10.10
