\# 🌐 DNS Configuration



The DNS service is integrated with Active Directory to manage name resolution for the `corp.thomasbytes` domain.



\## ⚙️ Forwarders Configuration

To ensure the Domain Controller can resolve external internet addresses, Google's public DNS servers were configured as Forwarders:

\* \*\*Primary:\*\* `8.8.8.8`

\* \*\*Secondary:\*\* `8.8.4.4`



\## 🔄 Reverse Lookup Zone

A reverse lookup zone was created to allow IP-to-Hostname resolution:

\* \*\*Network ID:\*\* `10.10.10.x`

\* \*\*Status:\*\* Active



\## ✅ Verification Commands

To verify the health of the DNS, run the following in PowerShell:

```powershell

\# Verify domain resolution

nslookup corp.thomasbytes



\# Verify SRV records (Critical for AD)

nslookup -type=srv \_ldap.\_tcp.dc.\_msdcs.corp.thomasbytes

