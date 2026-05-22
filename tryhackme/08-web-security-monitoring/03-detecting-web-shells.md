# Detecting Web Shells

## What I learned

- For an attacker to upload and execute a web shell, a file upload vulnerability, server misconfiguration, or prior system access is usually required.
- Web shells abuse web server functionality, making web server logs an important source for investigations.
- URL encoding can be used by attackers to obfuscate malicious requests and bypass basic filtering.

---

## Key concepts

- Web shell  
  → malicious script or program uploaded to a web server that allows attackers to execute commands remotely

- Auditd  
  → native Linux auditing utility used to monitor and record system events and activity

- URL encoding  
  → encoding special characters in URLs using percent notation (`%20` for space), sometimes abused to hide malicious payloads

---

## Why it matters in SOC

- Detecting web shells is critical because they can provide attackers with persistent remote access to compromised servers.
- SOC analysts monitor web logs and system activity to identify suspicious uploads, command execution, and abnormal requests.

---

## Commands / tools used

- Practiced interacting with a web shell on a vulnerable web application
- Used URL encoding during testing and investigation
- Investigated web server access logs to identify web shell activity and suspicious requests

---

## Personal notes

- Web server logs can reveal a large amount of attacker activity when investigating web shell incidents.
- Even simple upload vulnerabilities can lead to serious server compromise if not properly secured.
