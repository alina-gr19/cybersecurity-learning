# Detecting Web DDoS

## What I learned

- DoS and DDoS attacks attempt to overwhelm applications or infrastructure to make services unavailable to legitimate users.
- Web-based DDoS attacks often target application resources rather than only network bandwidth.
- Access logs can reveal abnormal request patterns associated with DDoS activity.

---

## DoS attack types

- Slowloris  
  → sending many partial HTTP requests to exhaust server connections and resources

- HTTP Flood  
  → overwhelming the server with a massive number of HTTP requests

- Cache Bypass  
  → bypassing CDN cache systems and forcing requests directly to the origin server

- Oversized Query  
  → sending resource-intensive requests that consume excessive server resources

- Login/Form Abuse  
  → overloading authentication systems using repeated login attempts or password reset requests

- Faulty Input Validation Abuse  
  → abusing poorly validated input fields to increase server workload or trigger unexpected behavior

---

## Application-level defenses

- Secure development practices  
  → validating user input and protecting forms/search fields from abuse

- Challenges (CAPTCHA)  
  → reducing automated bot activity

- Large-scale mitigation  
  → using cloud-based mitigation and traffic filtering services

---

## Network and infrastructure defenses

- Content Delivery Network (CDN)  
  → distributes and caches traffic to reduce load on origin servers

- Web Application Firewall (WAF)  
  → filters malicious web traffic and blocks suspicious requests

---

## Key concepts

- Denial-of-Service (DoS) attack  
  → attack designed to overwhelm a website, application, or service and make it unavailable to legitimate users

- Distributed Denial-of-Service (DDoS) attack  
  → DoS attack performed using multiple systems or botnets simultaneously

---

## Why it matters in SOC

- DDoS attacks can disrupt business operations and impact availability of critical services.
- SOC analysts monitor traffic spikes, abnormal request patterns, and server behavior to identify ongoing attacks.

---

## Commands / tools used

- Investigated `access.log` files for indicators of DDoS activity
- Used Splunk to analyze suspicious web traffic and request patterns

---

## Personal notes

- Even simple HTTP requests can become dangerous when performed at very large scale.
- Application-layer DDoS attacks can be difficult to detect because they often resemble legitimate traffic.
