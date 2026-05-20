# Web Security Essentials

## What I learned

- Web applications are one of the most common entry points for attackers because they are publicly exposed and continuously accessible.
- Web security involves protecting multiple layers:
  - the web application
  - the web server
  - the host machine
- Additional protections such as CDNs and WAFs can improve performance and security.

---

## Key concepts

- Web application  
  → the code, images, styles, and functionality that users interact with through a website

- Web server  
  → hosts the application and handles incoming HTTP/HTTPS requests

- Host machine  
  → the underlying operating system (Linux or Windows) running the web server and application

- Content Delivery Network (CDN)  
  → distributed servers that cache and deliver content closer to users to improve performance and reduce latency

- Web Application Firewall (WAF)  
  → security layer that filters and blocks malicious web traffic before it reaches the application

---

## Why it matters in SOC

- Web applications are heavily targeted by attackers and often serve as initial access points.
- SOC analysts monitor web traffic, server logs, and security alerts to identify attacks against web infrastructure.

---

## Tools / activities used

- Applied security best practices to:
  - the web application
  - the web server
  - the host machine

---

## Personal notes

- Web security requires protecting multiple layers together rather than focusing only on the application itself.
- Even a secure application can become vulnerable if the server or host machine is misconfigured.
