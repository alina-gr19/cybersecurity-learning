# Man-in-the-Middle Detection

## What I learned

- MITM attacks generally involve two main phases:
  - interception of communication
  - manipulation or decryption of traffic

- Common types of MITM attacks include:
  - packet sniffing
  - session hijacking
  - SSL stripping
  - DNS spoofing
  - IP spoofing
  - rogue Wi-Fi access points

---

## Key concepts

- Man-in-the-Middle (MITM) attack  
  → attack where an adversary secretly intercepts and potentially alters communication between two parties without their knowledge

- Gratuitous ARP responses  
  → unsolicited ARP replies that may indicate ARP spoofing or MITM activity, especially when sent repeatedly to multiple hosts

- SSL stripping  
  → attack technique where encrypted HTTPS traffic is downgraded to unencrypted HTTP traffic

- Rogue Wi-Fi access point  
  → fake wireless access point created to capture or manipulate victim traffic

---

## Why it matters in SOC

- MITM attacks can lead to credential theft, data interception, and session hijacking.
- Detecting abnormal ARP behavior, spoofing activity, or encryption downgrades helps analysts identify ongoing attacks.

---

## Tools / activities used

- Used Wireshark to analyze traffic and detect indicators of MITM attacks

---

## Personal notes

- Many MITM attacks rely on manipulating trusted network communication rather than directly exploiting systems.
- Unusual ARP traffic and unexpected HTTP communication can be strong indicators of MITM activity.
