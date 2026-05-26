# Windows Logging for SOC

## What I learned

- Windows logs are stored in binary format inside:
  - `C:\Windows\System32\winevt\Logs`

- The Security log is one of the most valuable sources for SOC investigations.
- Sysmon extends default Windows logging and provides advanced monitoring capabilities.
- Sysmon logs are located in:
  - Event Viewer → Applications & Services Logs → Microsoft → Windows → Sysmon → Operational

- PowerShell history is stored in:
  - `C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

---

## Key concepts

### Authentication events

| Event ID | Purpose          | Notes                                                  |
| -------- | ---------------- | ------------------------------------------------------ |
| 4624     | Successful logon | Useful for detecting suspicious RDP or network logins  |
| 4625     | Failed logon     | Useful for detecting brute force and password spraying |

---

### User management events

| Event ID           | Description                                 | Possible malicious usage                           |
| ------------------ | ------------------------------------------- | -------------------------------------------------- |
| 4720 / 4722 / 4738 | User account created / enabled / changed    | Attackers may create or enable backdoor accounts   |
| 4725 / 4726        | User account disabled / deleted             | Attackers may disable accounts to disrupt response |
| 4723 / 4724        | Password changed / reset                    | Attackers may reset passwords to gain access       |
| 4732 / 4733        | User added to / removed from security group | Attackers may add accounts to privileged groups    |

---

### Process creation events

| Event ID   | Purpose                         | Notes                                               |
| ---------- | ------------------------------- | --------------------------------------------------- |
| 4688       | Process creation logging        | Disabled by default                                 |
| 1 (Sysmon) | Advanced process creation event | Includes hashes, signatures, parent process details |

---

### File, registry, and network events

| Event ID         | Purpose                                 |
| ---------------- | --------------------------------------- |
| 11 / 13 (Sysmon) | File creation and registry modification |
| 3 / 22 (Sysmon)  | Network connections and DNS queries     |

---

## Why it matters in SOC

- Windows logs provide visibility into authentication, process execution, account management, and network activity.
- SOC analysts use these logs to detect suspicious behavior, investigate incidents, and identify attacker activity.

---

## Commands / tools used

- Used Windows Event Viewer to investigate:
  - authentication logs
  - user management events
  - process creation events
  - file and registry activity
  - network activity

- Used Sysmon for advanced monitoring and logging
- Investigated PowerShell history files

---

## Personal notes

- Windows event logs can reveal attacker actions across multiple stages of an intrusion.
- Sysmon provides much more detailed visibility compared to default Windows logging.
