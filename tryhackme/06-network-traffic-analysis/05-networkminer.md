# NetworkMiner

## What I learned

- The goal of network forensics is to identify malicious activity, security breaches, and network anomalies through network traffic analysis.
- NetworkMiner has two operating modes:
  - Sniffer mode
  - Packet parsing / processing mode
- NetworkMiner is better suited for quick overview, traffic mapping, and data extraction, while Wireshark is more suitable for in-depth packet analysis.

---

## Key concepts

- Network forensics  
  → process of capturing and analyzing network traffic to investigate suspicious activity

- NetworkMiner  
  → open-source Network Forensic Analysis Tool (NFAT) used for traffic analysis and artifact extraction

- Artifact extraction  
  → recovering files, images, credentials, hosts, and metadata from network traffic

---

## Why it matters in SOC

- NetworkMiner helps analysts quickly identify hosts, transferred files, credentials, and communication patterns from PCAP files.
- It is useful during investigations and incident response because it provides a fast overview of network activity.

---

## Tools / activities used

- Used NetoworkMiner to analyze PCAP files
- Extracted and reviewed network artifacts from captured traffic

---

## Personal notes

- NetworkMiner provides a faster high-level overview of traffic compared to Wireshark.
- Combining NetworkMiner and Wireshark provides both quick visibility and detailed packet analysis.
