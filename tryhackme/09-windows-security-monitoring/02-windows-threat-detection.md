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
  → Microsoft protocol that allows remote access and administration of systems

- Phishing  
  → social engineering technique used to trick users into opening malicious files, links, or attachments

- File extension spoofing  
  → technique where attackers disguise malicious executables as harmless files by abusing hidden file extensions

---

## Why it matters in SOC

- Detecting initial access activity is critical because it can stop an attack before privilege escalation or lateral movement occurs.
- SOC analysts monitor authentication logs, suspicious logins, phishing indicators, and unusual process execution to identify compromise attempts.

---

## Commands / tools used

- Used Windows Event Viewer to investigate:
  - suspicious RDP logins
  - authentication events
  - phishing-related activity

- Reviewed Windows logs related to initial access attempts

---

## Personal notes

- Simple techniques such as hidden file extensions can still be highly effective in phishing attacks.
- Monitoring RDP activity is important because it is frequently targeted by attackers and ransomware groups.
