# Network Discovery Detection

## What I learned

- Both attackers and defenders perform discovery actions:
  - attackers → to identify targets and weaknesses
  - defenders → to understand assets and reduce the attack surface

- Discovery and scanning activity can be identified through network and firewall logs.

---

## Key concepts

- External scanning activity  
  → scanning originating from outside the organization’s network  
  → associated with the Reconnaissance phase in the MITRE ATT&CK lifecycle.

- Internal scanning activity  
  → scanning performed from inside the organization’s network  
  → associated with the Discovery phase in the MITRE ATT&CK lifecycle.

- Horizontal scanning  
  → scanning the same port across multiple destination IP addresses

- Vertical scanning  
  → scanning multiple ports on a single host

- Ping sweep  
  → identifies active hosts by sending ICMP packets across the network

- TCP SYN scan  
  → initiates a TCP connection using SYN packets without completing the full handshake

---

## Why it matters in SOC

- Discovery activity is often one of the earliest signs of attacker reconnaissance.
- Detecting scans early helps SOC analysts identify suspicious behavior before exploitation occurs.

---

## Tools / activities used

- Analyzed log files for:
  - external scanning activity
  - internal scanning activity
  - horizontal scans
  - vertical scans

- Used Kibana to investigate firewall logs and identify scan patterns

---

## Personal notes

- Scan patterns become easier to identify once the difference between horizontal and vertical scanning is understood.
- Early detection of reconnaissance activity can help prevent later stages of an attack.
