This project explores a simulated CTF environment where the core component is a misconfigured DNS service. Unlike conventional exploitation of services at the application or OS level, this challenge leverages the DNS protocol as a vector for information retrieval.
The target machine (10.10.161.245) acts as an authoritative DNS server for the fictional domain givemetheflag.com. The task was to emulate a proper recursive query and analyze the returned DNS records.
Initial analysis involved verifying the local DNS resolution environment, followed by direct, targeted queries to the DNS daemon running on the remote machine. Tools used included nslookup and dig — both successfully returned TXT records containing the hidden flag.
Additionally, an external resolution test (e.g., querying br.dk) confirmed that the server handles only its authoritative zone, highlighting the sandboxed nature and intentionally constrained vulnerability scope of the environment.

Technologies Used
Kali Linux – Attack environment
TryHackMe – Virtual CTF lab platform
DNS Utilities – dig, nslookup, ping
Protocol – DNS over UDP (Port 53)
Methodology – Passive Recon → Manual DNS Interrogation

 Commands & Results
1. Verifying DNS client functionality:
dig
Response from root name servers confirms working client environment.

2. Primary DNS query using nslookup:
nslookup givemetheflag.com 10.10.161.245
givemetheflag.com text = "flag{0767ccd06e79853318f25aeb08ff83e2}"

3. Alternative query using dig with filtered output:
dig @10.10.161.245 givemetheflag.com TXT +short
"flag{0767ccd06e79853318f25aeb08ff83e2}"

4. External domain resolution test (expected failure):
dig @10.10.161.245 br.dk
Output:
pgsql
ANSWER: 0 (server does not handle external zones)
Extracted Flag
flag{0767ccd06e79853318f25aeb08ff83e2}

Conclusion
This project demonstrates how DNS records can be used as a covert channel for transmitting data in CTF or Red Team scenarios. Direct interaction with DNS infrastructure and the ability to interpret its responses are critical skills in practical offensive security.
The use of tools like dig and nslookup can be extended with automation for large-scale DNS reconnaissance via utilities such as dnsenum, dnsrecon, and amass.
This approach is adaptable for internal penetration tests targeting corporate DNS servers, where similar misconfigurations may lead to data leaks or detection evasion.
