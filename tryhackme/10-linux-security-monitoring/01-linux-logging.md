# Linux Logging for SOC

## What I learned

- Linux stores most logs in plain text files.
- One of the most important log files for SOC investigations is:
  - `/var/log/auth.log`

- Although its name suggests it only contains authentication events, it can also include:
  - login attempts
  - SSH activity
  - sudo command execution
  - user management events

- Linux does not natively log many runtime events such as:
  - process creation
  - file modifications
  - network connections

- More logs do not always mean better detection. Logging should focus on collecting useful and relevant security data.

---

## Key concepts

- System call
  → request made by a program to the operating system for services such as creating processes, opening files, or accessing hardware resources

- Auditd (Audit Daemon)
  → built-in Linux auditing framework commonly used for runtime monitoring and security investigations

- Runtime events
  → events that occur while a system is running, such as process execution, file modifications, registry changes (where applicable), and network activity

- `/var/log/auth.log`
  → log file commonly used to investigate authentication activity, privilege escalation, SSH access, and user account actions

---

## Why it matters in SOC

- Linux logs provide visibility into user activity, authentication events, and privilege escalation attempts.
- Auditd helps fill visibility gaps by recording important runtime events that are not logged by default.
- SOC analysts use Linux logs to investigate suspicious behavior, detect compromises, and support incident response.

---

## Commands / tools used

- Analyzed Linux log files
- Used Auditd to monitor and investigate runtime events

---

## Personal notes

- The authentication log contains much more than login events and is often one of the first places to check during an investigation.
- Auditd significantly improves visibility on Linux systems by recording events that are not logged by default.
