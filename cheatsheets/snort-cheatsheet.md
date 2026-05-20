# Snort Cheatsheet

A comprehensive reference for using Snort — the open-source network intrusion detection and prevention system (IDS/IPS).

---

## Table of Contents

1. [What is Snort?](#what-is-snort)
2. [Operating Modes](#operating-modes)
3. [Installation & Basic Setup](#installation--basic-setup)
4. [Configuration File (`snort.conf`)](#configuration-file-snortconf)
5. [Running Snort](#running-snort)
6. [Rule Anatomy](#rule-anatomy)
7. [Rule Header](#rule-header)
8. [Rule Options](#rule-options)
9. [Common Rule Examples](#common-rule-examples)
10. [Preprocessors](#preprocessors)
11. [Output Plugins](#output-plugins)
12. [Logging & Alerting](#logging--alerting)
13. [Performance Tuning](#performance-tuning)
14. [IPS / Inline Mode](#ips--inline-mode)
15. [Updating Rules](#updating-rules)
16. [Troubleshooting](#troubleshooting)
17. [Key Directories & Files](#key-directories--files)
18. [Quick Reference Card](#quick-reference-card)

---

## What is Snort?

Snort is a free, open-source **Network Intrusion Detection/Prevention System (NIDS/NIPS)** developed by Sourcefire (now Cisco). It performs real-time traffic analysis and packet logging on IP networks to detect a wide variety of attacks and probes.

**Core capabilities:**

- Packet sniffing
- Packet logging
- Network intrusion detection (NIDS)
- Network intrusion prevention (NIPS / inline mode)
- Protocol analysis and content matching

---

## Operating Modes

| Mode              | Flag              | Description                           |
| ----------------- | ----------------- | ------------------------------------- |
| **Sniffer**       | `-v`              | Print packets to stdout               |
| **Packet Logger** | `-l <dir>`        | Log packets to disk                   |
| **NIDS**          | `-c <snort.conf>` | Apply rules and generate alerts       |
| **Inline (IPS)**  | `-Q` + DAQ        | Drop/reject malicious packets in-line |
| **Daemon**        | `-D`              | Run Snort as a background daemon      |

---

## Installation & Basic Setup

```bash
# Ubuntu/Debian
sudo apt install snort

# RHEL/CentOS
sudo yum install snort

# Verify installation
snort --version

# Test configuration file
snort -T -c /etc/snort/snort.conf
```

**Key initial steps:**

1. Set your `HOME_NET` (your protected network range)
2. Set your `EXTERNAL_NET` (usually `!$HOME_NET` or `any`)
3. Point Snort to your rules directory
4. Configure output plugins (alert format, logging)
5. Test with `-T` before going live

---

## Configuration File (`snort.conf`)

The main config file is typically at `/etc/snort/snort.conf`.

### Network Variables

```bash
# Internal network(s)
var HOME_NET
var HOME_NET

# Everything else
var EXTERNAL_NET !$HOME_NET
var EXTERNAL_NET any

# Common server definitions
var DNS_SERVERS $HOME_NET
var SMTP_SERVERS $HOME_NET
var HTTP_SERVERS $HOME_NET
var SQL_SERVERS $HOME_NET
var TELNET_SERVERS $HOME_NET
var FTP_SERVERS $HOME_NET
var SIP_SERVERS $HOME_NET
```

### Port Variables

```bash
var HTTP_PORTS [80,8080,8000,8888,443]
var SHELLCODE_PORTS !80
var ORACLE_PORTS 1521
var SSH_PORTS 22
var FTP_PORTS [21,2100,3535]
var SIP_PORTS [5060,5061,5600]
var FILE_DATA_PORTS [$HTTP_PORTS,110,143]
var GTP_PORTS [2123,2152,3386]
```

### Paths

```bash
var RULE_PATH /etc/snort/rules
var SO_RULE_PATH /etc/snort/so_rules
var PREPROC_RULE_PATH /etc/snort/preproc_rules
var WHITE_LIST_PATH /etc/snort/rules
var BLACK_LIST_PATH /etc/snort/rules
```

### Including Rule Files

```bash
include $RULE_PATH/local.rules
include $RULE_PATH/community.rules
include $RULE_PATH/emerging-threats.rules
```

---

## Running Snort

```bash
# Test configuration only
snort -T -c /etc/snort/snort.conf

# Sniffer mode (verbose)
snort -v -i eth0

# Sniffer mode with full packet dump
snort -vde -i eth0

# Packet logger mode
snort -dev -l /var/log/snort -i eth0

# NIDS mode — alert to console
snort -A console -q -c /etc/snort/snort.conf -i eth0

# NIDS mode — alert to fast log
snort -A fast -c /etc/snort/snort.conf -i eth0

# Read from a pcap file
snort -r capture.pcap -c /etc/snort/snort.conf

# Run as daemon
snort -D -c /etc/snort/snort.conf -i eth0 -l /var/log/snort

# Inline IPS mode (requires DAQ)
snort -Q --daq afpacket --daq-var buffer_size_mb=1024 -c /etc/snort/snort.conf -i eth0:eth1
```

### Common CLI Flags

| Flag            | Meaning                                              |
| --------------- | ---------------------------------------------------- |
| `-i <iface>`    | Specify network interface                            |
| `-c <file>`     | Path to config file                                  |
| `-l <dir>`      | Log directory                                        |
| `-r <file>`     | Read packets from pcap                               |
| `-A <mode>`     | Alert mode: `fast`, `full`, `console`, `none`, `cmg` |
| `-q`            | Quiet mode (suppress banner)                         |
| `-v`            | Verbose (print packet headers)                       |
| `-d`            | Dump application layer                               |
| `-e`            | Show link layer headers                              |
| `-T`            | Test/validate config and exit                        |
| `-D`            | Daemon mode                                          |
| `-Q`            | Inline/IPS mode                                      |
| `-s`            | Log alerts to syslog                                 |
| `-b`            | Log in tcpdump binary format                         |
| `--pcap-filter` | Apply BPF filter to pcap replay                      |
| `-F <bpf>`      | Apply BPF filter on live capture                     |
| `-n <count>`    | Exit after processing N packets                      |
| `-k none`       | Disable TCP checksum verification                    |
| `--pid-path`    | Custom PID file location                             |

---

## Rule Anatomy

A Snort rule consists of two parts:

```
[action] [protocol] [src_ip] [src_port] -> [dst_ip] [dst_port] ([options])
```

**Example:**

```
alert tcp any any -> $HOME_NET 80 (msg:"HTTP traffic detected"; sid:1000001; rev:1;)
```

---

## Rule Header

### Actions

| Action     | Effect                                 |
| ---------- | -------------------------------------- |
| `alert`    | Generate alert, then log packet        |
| `log`      | Log packet only                        |
| `pass`     | Ignore / whitelist packet              |
| `drop`     | Drop packet (IPS mode only) + log      |
| `reject`   | Drop + send TCP RST / ICMP unreachable |
| `sdrop`    | Drop silently (no log)                 |
| `activate` | Alert + enable a dynamic rule          |
| `dynamic`  | Remain idle until activated            |

### Protocols

`tcp`, `udp`, `icmp`, `ip`

### IP Address Notation

```bash
any                        # Any IP
192.168.1.0/24             # CIDR block
[192.168.1.0/24,10.0.0.0/8]  # List
!192.168.1.5               # Negation
!192.168.1.0/24            # Negate entire subnet
$HOME_NET                  # Variable reference
```

### Port Notation

```bash
any          # Any port
80           # Single port
!80          # Not port 80
1:1024       # Port range 1 to 1024
[80,443,8080] # Port list
!6000:6010   # Not range
```

### Direction Operators

| Operator | Meaning                                |
| -------- | -------------------------------------- |
| `->`     | Source to destination (unidirectional) |
| `<>`     | Bidirectional (both directions)        |

---

## Rule Options

### Meta-data Options

| Option      | Description            | Example                             |
| ----------- | ---------------------- | ----------------------------------- |
| `msg`       | Alert message          | `msg:"SQL Injection attempt";`      |
| `sid`       | Snort rule ID (unique) | `sid:1000001;`                      |
| `rev`       | Revision number        | `rev:1;`                            |
| `gid`       | Group ID (default 1)   | `gid:1;`                            |
| `classtype` | Attack classification  | `classtype:web-application-attack;` |
| `priority`  | Alert priority 1–4     | `priority:1;`                       |
| `metadata`  | Key-value tags         | `metadata:service http;`            |
| `reference` | External references    | `reference:cve,2021-44228;`         |

### Detection Options

| Option         | Description                                | Example                   |
| -------------- | ------------------------------------------ | ------------------------- |
| `content`      | Match raw bytes/string                     | `content:"cmd.exe";`      |
| `nocase`       | Case-insensitive match                     | `content:"GET"; nocase;`  |
| `offset`       | Start search at byte N                     | `offset:4;`               |
| `depth`        | Search only N bytes                        | `depth:20;`               |
| `distance`     | Relative offset from last match            | `distance:0;`             |
| `within`       | Match within N bytes of last               | `within:10;`              |
| `rawbytes`     | Search raw (pre-decode) bytes              | `rawbytes;`               |
| `isdataat`     | Check if data exists at offset             | `isdataat:50;`            |
| `uricontent`   | Match in URI (deprecated → use `http_uri`) | `uricontent:"/admin";`    |
| `pcre`         | Perl-compatible regex                      | `pcre:"/select.+from/i";` |
| `byte_test`    | Test byte value at offset                  | `byte_test:4,>,1000,0;`   |
| `byte_jump`    | Jump N bytes for next match                | `byte_jump:4,12;`         |
| `byte_extract` | Extract bytes into variable                | `byte_extract:2,0,len;`   |

### HTTP-specific Modifiers (use after `content`)

| Modifier           | What it matches          |
| ------------------ | ------------------------ |
| `http_method`      | HTTP method (GET, POST…) |
| `http_uri`         | Normalized URI           |
| `http_raw_uri`     | Raw, un-normalized URI   |
| `http_header`      | HTTP header fields       |
| `http_cookie`      | Cookie header            |
| `http_client_body` | Request body             |
| `http_stat_code`   | Response status code     |
| `http_stat_msg`    | Response status message  |
| `http_encode`      | Encoding type in headers |

### Flow Options

```bash
flow:to_server,established;      # Client → server, established TCP
flow:to_client,established;      # Server → client
flow:from_server,established;
flow:stateless;                  # Don't track state
flow:no_stream;                  # Don't inspect reassembled streams
flow:only_stream;                # Only inspect reassembled streams
```

### Threshold & Suppression

```bash
# Alert max once per 60 seconds per source IP
threshold: type limit, track by_src, count 1, seconds 60;

# Alert only after 5 occurrences
threshold: type threshold, track by_src, count 5, seconds 10;

# Alert once, then suppress for 60s
threshold: type both, track by_dst, count 5, seconds 60;
```

Suppression in `threshold.conf`:

```bash
suppress gen_id 1, sig_id 1000001;
suppress gen_id 1, sig_id 1000001, track by_src, ip 192.168.1.100;
```

---

## Common Rule Examples

```bash
# Detect ping (ICMP echo request)
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping detected"; itype:8; sid:1000001; rev:1;)

# Detect Nmap SYN scan
alert tcp any any -> $HOME_NET any (msg:"Possible Nmap SYN scan"; flags:S; threshold:type threshold,track by_src,count 20,seconds 5; sid:1000002; rev:1;)

# Detect FTP brute force
alert tcp any any -> $HOME_NET 21 (msg:"FTP brute force attempt"; content:"530 Login"; flow:to_client,established; threshold:type threshold,track by_src,count 5,seconds 60; sid:1000003; rev:1;)

# Detect SQL Injection
alert tcp any any -> $HOME_NET $HTTP_PORTS (msg:"SQL Injection - UNION SELECT"; flow:to_server,established; content:"UNION"; nocase; content:"SELECT"; nocase; distance:0; within:30; http_uri; sid:1000004; rev:1;)

# Detect directory traversal
alert tcp any any -> $HOME_NET $HTTP_PORTS (msg:"Directory Traversal Attempt"; flow:to_server,established; content:"../"; http_uri; sid:1000005; rev:1;)

# Detect XSS attempt
alert tcp any any -> $HOME_NET $HTTP_PORTS (msg:"XSS Attempt"; flow:to_server,established; content:"<script"; nocase; http_uri; sid:1000006; rev:1;)

# Detect Telnet traffic
alert tcp any any -> $HOME_NET 23 (msg:"Telnet connection attempt"; flow:to_server; flags:S; sid:1000007; rev:1;)

# Detect outbound IRC (possible botnet C2)
alert tcp $HOME_NET any -> any 6667 (msg:"IRC traffic - possible botnet"; flow:to_server,established; sid:1000008; rev:1;)

# DNS amplification attack
alert udp any 53 -> any any (msg:"Possible DNS amplification"; dsize:>512; sid:1000009; rev:1;)

# Detect Log4Shell (CVE-2021-44228)
alert tcp any any -> $HOME_NET any (msg:"Log4Shell Attempt CVE-2021-44228"; content:"${jndi:"; nocase; sid:1000010; rev:1; reference:cve,2021-44228; classtype:web-application-attack;)
```

---

## Preprocessors

Preprocessors run before the detection engine, decoding and normalizing traffic.

```bash
# Stream reassembly (TCP/UDP stateful inspection)
preprocessor stream5_global: track_tcp yes, track_udp yes, track_icmp no, \
    max_tcp 262144, max_udp 131072, max_active_responses 2, min_response_seconds 5
preprocessor stream5_tcp: policy windows, detect_anomalies, require_3whs 180, \
    overlap_limit 10, small_segments 3 bytes 150, timeout 180, \
    ports client [21 22 23 25 110 443 465 995]
preprocessor stream5_udp: timeout 180

# HTTP normalization and inspection
preprocessor http_inspect: global iis_unicode_map unicode.map 1252 \
    compress_depth 65535 decompress_depth 65535
preprocessor http_inspect_server: server default \
    http_methods { GET POST PUT HEAD DELETE TRACE OPTIONS } \
    chunk_length 500000 \
    server_flow_depth 0 client_flow_depth 0 \
    post_depth 65495 \
    oversize_dir_length 500 \
    max_headers 100 max_header_length 0 max_spaces 200 \
    small_chunk_length { 10 20 } \
    ports { 80 8080 8000 }

# FRAG3 — IP defragmentation
preprocessor frag3_global: max_frags 65536
preprocessor frag3_engine: policy windows detect_anomalies overlap_limit 10 \
    min_fragment_length 100 timeout 180

# SMTP decoder
preprocessor smtp: ports { 25 465 587 691 } \
    inspection_type stateful \
    normalize cmds

# FTP/Telnet
preprocessor ftp_telnet: global inspection_type stateful encrypted_traffic no
preprocessor ftp_telnet_protocol: ftp server default \
    def_max_param_len 100 \
    ports { 21 } print_cmds
preprocessor ftp_telnet_protocol: telnet \
    normalize ayt_attack_thresh 20 \
    ports { 23 }

# DNS preprocessor
preprocessor dns: ports { 53 } enable_rdata_overflow
```

---

## Output Plugins

```bash
# Fast alert (one line per alert)
output alert_fast: stdout
output alert_fast: /var/log/snort/alert.fast

# Full alert (verbose)
output alert_full: /var/log/snort/alert.full

# Syslog output
output alert_syslog: LOG_AUTH LOG_ALERT

# CSV output
output alert_csv: /var/log/snort/alert.csv default

# Unified2 (binary — used by Barnyard2/SIEM)
output unified2: filename snort.log, limit 128, nostamp, mpls_event_types, vlan_event_types

# Log pcap
output log_tcpdump: /var/log/snort/snort.pcap

# Database (legacy — Snort 2 only)
output database: log, mysql, user=snort password=pass dbname=snort host=localhost
```

> **Tip:** Use `unified2` + **Barnyard2** to offload alert processing to a SIEM or database without impacting Snort's performance.

---

## Logging & Alerting

### Alert Modes (`-A` flag)

| Mode      | Description                               |
| --------- | ----------------------------------------- |
| `fast`    | Timestamp, message, IPs, ports (one line) |
| `full`    | Full packet header + alert info           |
| `console` | Print to stdout (testing)                 |
| `cmg`     | Content match with hex dump               |
| `none`    | No alerts (logging only)                  |
| `unsock`  | Send over UNIX socket                     |

### Log files location

```
/var/log/snort/
├── alert           # Alert file
├── snort.log.*     # Pcap logs (unified2 or tcpdump)
└── arp-cache       # ARP cache
```

### Reading binary logs

```bash
# Read unified2 with u2spewfoo (Snort built-in)
u2spewfoo /var/log/snort/snort.log.*

# Read with Barnyard2
barnyard2 -c /etc/barnyard2.conf -d /var/log/snort -f snort.log

# Read pcap log with tcpdump
tcpdump -r /var/log/snort/snort.log.* -n
```

---

## Performance Tuning

```bash
# In snort.conf — configure detection engine
config detection: search-method ac-bnfa split-any-any max-pattern-len 20
config detection: no_stream_inserts search-method lowmem

# Limit logged packets
config pkt_count: 10000

# Limit snaplen (default 1514)
config snaplen: 1518

# Use DAQ for better performance
--daq afpacket --daq-var buffer_size_mb=512

# Run multiple instances (one per CPU core) with CPU affinity
taskset -c 0 snort -Q -c /etc/snort/snort.conf -i eth0
taskset -c 1 snort -Q -c /etc/snort/snort.conf -i eth1

# Profile rules (slow-rule analysis)
config profile_rules: print 20, sort avg_ticks
config profile_preprocs: print all, sort avg_ticks
```

**Performance best practices:**

- Use `unified2` output rather than `full` or `database` to minimize I/O overhead
- Enable only the preprocessors you actually need
- Use `stream5` with appropriate session limits for your network size
- Apply BPF filters (`-F`) to ignore irrelevant traffic before Snort processes it
- Place high-traffic pass rules before alert rules

---

## IPS / Inline Mode

Inline mode allows Snort to **drop** malicious packets rather than just alerting.

```bash
# Requires a DAQ (Data Acquisition library)
# afpacket DAQ — most common on Linux
snort -Q --daq afpacket --daq-var buffer_size_mb=1024 \
    -c /etc/snort/snort.conf -i eth0:eth1

# NFQ DAQ (via iptables/nftables)
iptables -I FORWARD -j NFQUEUE --queue-num 1
snort -Q --daq nfq --daq-var queue=1 -c /etc/snort/snort.conf

# ipfw DAQ (FreeBSD/macOS)
snort -Q --daq ipfw -c /etc/snort/snort.conf
```

**Inline rule actions:**

- Change `alert` → `drop` to block matching traffic
- Use `reject` to block AND send a TCP reset or ICMP unreachable
- Use `sdrop` for silent drops (no log)

**Inline config options:**

```bash
# In snort.conf
config policy_mode: inline         # Full inline IPS
config policy_mode: inline_test    # Simulate drops (alert only, no actual drop)
config policy_mode: tap            # Passive tap — default IDS mode
```

---

## Updating Rules

### Oinkmaster (legacy)

```bash
oinkmaster -o /etc/snort/rules -u https://www.snort.org/rules/snortrules-snapshot-XXXXX.tar.gz
```

### PulledPork (recommended)

```bash
# Install
git clone https://github.com/shirkdog/pulledpork3

# Configure /etc/pulledpork/pulledpork.conf
rule_url=https://www.snort.org/rules/|snortrules-snapshot.tar.gz|<oinkcode>
rule_url=https://rules.emergingthreats.net/|emerging.rules.tar.gz|open

# Run
pulledpork.pl -c /etc/pulledpork/pulledpork.conf -l

# Automate via cron
0 3 * * * /usr/local/bin/pulledpork.pl -c /etc/pulledpork/pulledpork.conf -l && snort -T -c /etc/snort/snort.conf && systemctl reload snort
```

### Rule Sources

| Source                 | URL                           | Cost                |
| ---------------------- | ----------------------------- | ------------------- |
| Snort VRT (registered) | snort.org/rules               | Free (30-day delay) |
| Snort VRT (subscriber) | snort.org/rules               | Paid (real-time)    |
| Emerging Threats       | rules.emergingthreats.net     | Free                |
| Community rules        | snort.org/downloads/community | Free                |

---

## Troubleshooting

```bash
# Validate config file
snort -T -c /etc/snort/snort.conf

# Test a single rule file
snort -T -c /etc/snort/snort.conf -R /etc/snort/rules/local.rules

# Check what rules are loaded
snort -c /etc/snort/snort.conf --dump-rule-meta

# Replay a pcap against rules
snort -r /path/to/file.pcap -c /etc/snort/snort.conf -A console -q

# Debug a specific packet (verbose mode)
snort -vd -r suspicious.pcap

# Check interfaces available
snort --list-interfaces

# Show DAQ modules
snort --list-daqs

# Print stats on exit
snort -c /etc/snort/snort.conf -i eth0 --exit-check 1

# Enable debug output
snort -c /etc/snort/snort.conf -i eth0 --conf-error-out
```

**Common issues:**

| Problem                              | Likely Cause                      | Fix                                      |
| ------------------------------------ | --------------------------------- | ---------------------------------------- |
| `ERROR: Can't open rules file`       | Wrong `RULE_PATH` or missing file | Check variable and file permissions      |
| `FATAL: No preprocessors configured` | Missing `preprocessor` directives | Add required preprocessors to snort.conf |
| High CPU usage                       | Too many rules / no BPF filter    | Tune rules, add BPF filter, use AC-BNFA  |
| Dropping packets (kernel)            | Buffer too small                  | Increase `buffer_size_mb` in DAQ options |
| Alerts not appearing                 | Wrong output plugin               | Check `-A` flag or output plugin config  |
| SID conflicts                        | Duplicate rule SIDs               | Use SIDs > 1,000,000 for custom rules    |

---

## Key Directories & Files

| Path                                  | Description                       |
| ------------------------------------- | --------------------------------- |
| `/etc/snort/snort.conf`               | Main configuration file           |
| `/etc/snort/rules/`                   | Rule files directory              |
| `/etc/snort/rules/local.rules`        | Your custom/local rules           |
| `/etc/snort/rules/white_list.rules`   | IP whitelist (pass rules)         |
| `/etc/snort/rules/black_list.rules`   | IP blacklist (block rules)        |
| `/etc/snort/threshold.conf`           | Alert thresholds and suppressions |
| `/etc/snort/reference.config`         | Alert reference URLs              |
| `/etc/snort/classification.config`    | Alert classification definitions  |
| `/var/log/snort/`                     | Default log/alert directory       |
| `/usr/lib/snort_dynamicrules/`        | Shared object (.so) rules         |
| `/usr/lib/snort_dynamicpreprocessor/` | Dynamic preprocessor libraries    |
| `/usr/lib/snort_dynamicengine/`       | Detection engine library          |

---

## Quick Reference Card

```
SNORT MODES
───────────────────────────────────────────────────────
Sniffer:       snort -v -i eth0
Packet Logger: snort -dev -l /var/log/snort -i eth0
NIDS:          snort -A fast -c /etc/snort/snort.conf -i eth0
IPS Inline:    snort -Q --daq afpacket -c /etc/snort/snort.conf -i eth0:eth1
Test Config:   snort -T -c /etc/snort/snort.conf
Read PCAP:     snort -r file.pcap -c /etc/snort/snort.conf -A console -q

RULE FORMAT
───────────────────────────────────────────────────────
action proto src_ip src_port -> dst_ip dst_port (options)

ALERT ACTIONS:  alert | log | pass | drop | reject | sdrop
PROTOCOLS:      tcp | udp | icmp | ip
DIRECTIONS:     -> (unidirectional)  <>  (bidirectional)

KEY RULE OPTIONS
───────────────────────────────────────────────────────
msg:"text"                  Alert message
content:"string"            Payload match
nocase                      Case-insensitive
offset:N                    Start match at byte N
depth:N                     Search N bytes
distance:N                  Relative to last match
within:N                    Within N bytes
pcre:"/regex/flags"         Regex match
flow:to_server,established  Flow direction
threshold:type X,track Y,count N,seconds S
sid:N                       Rule ID (>1000000 for custom)
rev:N                       Revision number
classtype:X                 Classification

SID RANGES
───────────────────────────────────────────────────────
1      – 3464      Snort VRT built-in rules
100000 – 399999    Emerging Threats
400000 – 999999    Reserved
1000000+           User/local custom rules

COMMON CLASSTYPES
───────────────────────────────────────────────────────
attempted-admin        attempted-dos         attempted-recon
web-application-attack shellcode-detect      trojan-activity
policy-violation       protocol-command-decode network-scan
```

---
