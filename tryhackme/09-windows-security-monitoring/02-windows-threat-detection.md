# Windows Threat Detection

## What I learned

- Regardless of the attacker’s final objective, the first stage of an attack is usually Initial Access.
- Remote Desktop Protocol (RDP) is commonly abused by attackers and ransomware operators to gain remote access to systems.
- Windows hides known file extensions by default, which attackers can abuse by disguising malicious executables as legitimate files such as:
  - `invoice.pdf.exe`
  - `photo.png.com`

- Attackers may also change file icons to make malicious files appear trustworthy.

---

## Key concepts

- Initial Access
  → the stage where an attacker first gains access to a target system or environment

- RDP (Remote Desktop Protocol)
  → Microsoft protocol that allows remote administration of systems

- Phishing
  → social engineering technique used to trick users into executing malicious files or clicking malicious links

- File extension spoofing
  → technique where attackers disguise malicious executables by abusing hidden file extensions

- Command and Control (C2)
  → method attackers use to send commands to and maintain control over a compromised system
  - In some cases, C2 is not needed. For example, attackers may directly execute commands through an RDP session after gaining access.

---

### Post-compromise activity

- Discovery
  → stage where attackers explore the compromised environment and identify valuable targets

- Collection
  → gathering useful or sensitive data from the compromised system

- Credential Access
  → techniques used to steal usernames, passwords, or authentication tokens

- Exfiltration
  → transferring stolen data out of the target environment

---

## Why it matters in SOC

- Detecting initial access early can stop attacks before privilege escalation or lateral movement occurs.
- SOC analysts monitor authentication logs, process execution, and suspicious access patterns to identify compromise attempts.

---

## Commands / tools used

- Used Windows Event Viewer to investigate:
  - suspicious RDP logins
  - authentication events
  - phishing-related activity

- Reviewed Windows logs related to initial access attempts

---

## Personal notes

- Simple tricks like hiding file extensions are still effective in phishing campaigns.
- Monitoring RDP activity is important because it is frequently abused in real-world attacks.
