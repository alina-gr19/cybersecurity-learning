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
  → Microsoft protocol that allows remote remote administration of systems

- Phishing  
  → social engineering technique used to trick users into executing malicious files or clicking malicious links

- File extension spoofing  
  → technique where attackers disguise malicious executables by abusing hidden file extensions

- Command and Control
  - how threat actors send the commands and keep control of the victim's host
  - In some cases, C2 is not needed at all. For example, threat actors can type their commands directly in the RDP session after an RDP breach

---

### Post-compromise activity

- Discovery  
  → the stage after initial access where attackers explore the system and environment

- Collection  
  → gathering useful data from the compromised system

- Credential Access  
  → techniques used to steal usernames, passwords, or authentication tokens

- Exfiltration  
  → transferring stolen data out of the target environment

---

## Why it matters in SOC

- Detecting initial access early can stop an attack before escalation or lateral movement occurs.
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
- RDP monitoring is important because it is frequently abused in real-world attacks.
