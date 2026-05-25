# Intro to Logs

## What I learned

- Almost every interaction with a computer system leaves a digital footprint in the form of logs.
- Logs can record events such as:
  - authentication attempts
  - file access
  - network connections
  - authorization changes
  - system errors

- Logs help answer important investigation questions:
  - What happened?
  - When did it happen?
  - Where did it happen?
  - Who performed the action?
  - Was the action successful?
  - What was the result?

---

## Log formats

### Semi-structured logs

Logs containing both structured and free-form data.

Examples:

- Syslog message format
- Windows Event Log (EVTX)

---

### Structured logs

Logs following a strict and standardized format that is easier to parse and analyze.

Examples:

- CSV (Comma-Separated Values)
- TSV (Tab-Separated Values)
- JSON
- W3C Extended Log Format (ELF)
- XML

---

### Unstructured logs

Logs without a strict structure.

Examples:

- NCSA Common Log Format (CLF)
- NCSA Combined Log Format

---

## Key concepts

- Digital log  
  → historical record of activity and events within a system

- Log standard  
  → set of rules and specifications defining how logs are generated, transmitted, and stored

- Syslog  
  → standard protocol used for collecting and managing logs across systems and network devices

---

## Why it matters in SOC

- Logs are one of the primary sources of evidence during investigations.
- SOC analysts rely on logs to detect suspicious activity, investigate incidents, and reconstruct attacker actions.

---

## Commands / tools used

- Practiced log collection using `rsyslog`
- Practiced log management using `logrotate`

---

## Personal notes

- Logs provide visibility into what is happening across systems and networks.
- Proper logging and log management are essential for effective threat detection and incident response.
