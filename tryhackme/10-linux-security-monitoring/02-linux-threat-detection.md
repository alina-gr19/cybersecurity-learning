# Linux Threat Detection

## What I learned

- One of the most common Initial Access methods on Linux servers is an exposed SSH service.
- Web applications can also be used as an Initial Access vector when vulnerabilities are exploited.
- Process tree analysis is a useful technique for investigating attacker activity and tracing actions back to their origin.
- Analyzing parent-child process relationships can help identify suspicious command execution and malware activity.

---

## Key concepts

- Initial Access
  → the stage where an attacker first gains access to a target system

- SSH (Secure Shell)
  → protocol used for secure remote administration of Linux systems

- Process Tree
  → visual representation of parent and child processes used to understand how a process was launched

- Parent Process
  → process that starts another process

- Child Process
  → process launched by another process

- Persistence
  → techniques used by attackers to maintain access to a compromised system after a reboot or logout

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

- Process trees make investigations much easier because they show how suspicious activity started.
- Looking at a single process is often not enough; understanding its parent process provides important context.
