# Detecting Web Attacks

## What I learned

- Server-side attacks are generally easier for SOC teams to detect than client-side attacks because they leave more visible traces in logs and network traffic.
- Web attacks can target either the user’s device or the web application/server infrastructure.

---

## Key concepts

- Client-side attacks  
  → attacks that exploit weaknesses in user behavior, browsers, or the user’s device

Examples:

- malicious JavaScript
- browser exploitation
- phishing pages
- drive-by downloads

---

- Server-side attacks  
  → attacks that target vulnerabilities in the web server, backend systems, or application code

Examples:

- SQL injection
- command injection
- authentication bypass
- file inclusion vulnerabilities

---

## Why it matters in SOC

- Detecting web attacks helps analysts identify compromise attempts against applications and users.
- Server-side attacks often generate logs and alerts that can be investigated through SIEMs and network monitoring tools.

---

## Tools / activities used

- Used log-based detection to investigate suspicious web activity
- Used network-based detection to analyze traffic related to server-side attacks

---

## Personal notes

- Server-side attacks are usually easier to investigate because they often generate more visible indicators in logs and traffic.
- Detecting client-side attacks may require endpoint visibility and browser-level monitoring.
