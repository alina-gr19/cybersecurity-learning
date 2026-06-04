# Linux Threat Detection

## What I learned

- One of the most common Initial Access methods on Linux servers is an exposed SSH service.
- Web applications can also be used as an Initial Access vector when vulnerabilities are exploited.
- Process tree analysis is a useful technique for investigating attacker activity and tracing actions back to their origin.
- Analyzing parent-child process relationships can help identify suspicious command execution and malware activity.
- The next step after Initial Access is often Discovery, which can be identified through commands such as `pwd`, `whoami`, `hostname`, `id`, and `ip a`.
- After Discovery, threat actors often execute additional actions based on their objectives (e.g., malware deployment or system exploitation).
- Standalone Linux servers can run for long periods without rebooting and are often left untouched unless something breaks. Some threat actors rely on this behavior and do not immediately establish Persistence.
- A single compromised Linux server can provide access to a wider corporate network and lead to significant impact.

---

## Key concepts

- Initial Access  
  → the stage where an attacker first gains access to a target system

- SSH (Secure Shell)  
  → protocol used for secure remote administration of Linux systems

- Process Tree  
  → visual representation of parent and child processes used to understand process execution flow

- Parent Process  
  → process that starts another process

- Child Process  
  → process launched by another process

- Discovery  
  → phase where attackers gather information about the compromised system (users, network, and environment)

- Persistence  
  → techniques used by attackers to maintain access after reboot or logout

- Reverse Shell  
  → a connection initiated from the victim machine back to the attacker, often used to bypass inbound firewall restrictions

- Cron Jobs  
  → scheduled tasks in Linux, commonly abused for persistence by executing malicious commands at regular intervals

---

## Common attacker objectives

- Cryptomining  
  → using victim CPU/GPU resources to mine cryptocurrency

- Botnet Enrollment  
  → adding the compromised system to a botnet for activities such as DDoS attacks

- Proxying  
  → using the victim machine to route traffic, host malware, or send phishing traffic

---

## Why it matters in SOC

- Detecting Initial Access can stop an attack before it progresses further.
- Process tree analysis helps analysts understand attacker behavior and reconstruct attack timelines.
- Monitoring SSH activity and suspicious process execution can reveal compromised systems.

---

## Commands / tools used

- Analyzed logs for threats related to SSH and web-based Initial Access
- Used `ausearch` to investigate audit logs and system activity
- Investigated process execution chains using process tree analysis

---

## Personal notes

- Process trees make investigations easier by showing how suspicious activity started.
- Looking at a single process is often not enough; understanding its parent process provides important context.
