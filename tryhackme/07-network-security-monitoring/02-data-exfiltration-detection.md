# Data Exfiltration Detection

## What I learned

- Data exfiltration is the unauthorized transfer of data from an organization to an external destination controlled by an attacker.
- Exfiltration can happen through insider threats, malware, or compromised accounts and credentials.
- Common protocols such as DNS, FTP, HTTP, and ICMP can be abused for covert data transfer.

---

## Key concepts

- DNS (Domain Name System)  
  → translates domain names into IP addresses and can be abused for DNS tunneling

- FTP (File Transfer Protocol)  
  → protocol used for transferring files over TCP/IP networks

- HTTP exfiltration  
  → attackers use normal web traffic to move sensitive data outside the network

- ICMP tunneling  
  → data hidden inside ICMP packets, often appearing as normal ping traffic

- DNS tunneling  
  → attackers encode data into DNS queries and responses to bypass security controls

---

## Why it matters in SOC

- Detecting exfiltration activity is critical because attackers often attempt to steal sensitive information after gaining access.
- SOC analysts monitor unusual traffic patterns, abnormal protocol usage, and large outbound transfers to identify possible data theft.

---

## Tools / activities used

- Used Splunk and Wireshark to investigate:
  - DNS tunneling
  - FTP exfiltration
  - HTTP-based exfiltration
  - ICMP tunneling

---

## Personal notes

- Legitimate protocols can easily be abused by attackers to hide malicious activity.
- Detecting tunneling often depends on identifying unusual traffic behavior rather than the protocol itself.
