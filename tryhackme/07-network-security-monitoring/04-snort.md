# Snort

## What I learned

- IDS (Intrusion Detection System) can detect threats but requires user action to respond.
- IPS (Intrusion Prevention System) can detect and automatically block threats in real time.
- Snort requires superuser (root) privileges to sniff network traffic.
- By default, Snort runs in passive (IDS) mode.
- To enable prevention capabilities, Snort must be run in inline (IPS) mode.

---

## Key concepts

- Snort  
  → open-source, rule-based Network Intrusion Detection and Prevention System (NIDS/NIPS)

- IDS (Intrusion Detection System)  
  → passive monitoring system that detects suspicious activity and generates alerts

- IPS (Intrusion Prevention System)  
  → active security system that blocks or prevents malicious activity in real time

---

## Why it matters in SOC

- Snort is widely used in SOC environments for network-based detection of attacks.
- It helps analysts identify malicious traffic patterns and respond to threats faster.

---

## Commands / tools used

### Sniffer Mode

- `snort -V` → check version
- `snort -c /etc/snort/snort.conf -T` → test configuration file
- `sudo snort -v` → run in verbose mode
- `sudo snort -d` → dump packet data
- `sudo snort -d -e` → include link-layer headers

---

### Logger Mode

- `sudo snort -dev -l` → start packet logging mode
- `sudo snort -dev -K ASCII` → log in readable ASCII format
- `sudo snort -r logname.log` → read logs
- `sudo snort -r logname.log tcp` → filter TCP logs
- `snort -dvr logname.log -n 10` → process first 10 packets
- `snort -r snort.log 'tcp and port 80'` → filter HTTP traffic

---

### IDS/IPS Mode

- `sudo snort -c /etc/snort/snort.conf -T` → test configuration
- `sudo snort -c /etc/snort/snort.conf -N` → run without packet logging
- `sudo snort -c /etc/snort/snort.conf -D` → run in daemon mode
- `sudo snort -c /etc/snort/snort.conf -A console` → output alerts to console
- `sudo snort -c /etc/snort/snort.conf -A cmg` → detailed packet + rule output

---

## Personal notes
