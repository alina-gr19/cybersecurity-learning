# Recon & DNS Cheatsheet

## WHOIS

| Purpose              | Command               |
| -------------------- | --------------------- |
| Look up WHOIS record | `whois tryhackme.com` |

## DNS — `nslookup` (Legacy)

| Purpose                                        | Command                                   |
| ---------------------------------------------- | ----------------------------------------- |
| Look up DNS A records                          | `nslookup -type=A tryhackme.com`          |
| Look up MX records using a specific DNS server | `nslookup -type=MX tryhackme.com 1.1.1.1` |
| Look up TXT records                            | `nslookup -type=TXT tryhackme.com`        |

## DNS — `dig` (Recommended)

| Purpose                                        | Command                         |
| ---------------------------------------------- | ------------------------------- |
| Look up DNS A records                          | `dig tryhackme.com A`           |
| Look up MX records using a specific DNS server | `dig @1.1.1.1 tryhackme.com MX` |
| Look up TXT records                            | `dig tryhackme.com TXT`         |

### Common DNS Record Types

- **A** — Maps a domain to an IPv4 address.
- **AAAA** — Maps a domain to an IPv6 address.
- **MX** — Specifies mail servers for a domain.
- **TXT** — Stores text information, often used for SPF, DKIM, and domain verification.
- **CNAME** — Creates an alias pointing to another domain.
- **NS** — Identifies the authoritative name servers for a domain.

## Passive Subdomain Discovery

### Certificate Transparency

Use [crt.sh](https://crt.sh) to search Certificate Transparency logs for certificates associated with a domain.

Search:

```text
%.tryhackme.com
```

This can reveal subdomains such as:

```text
www.tryhackme.com
mail.tryhackme.com
dev.tryhackme.com
api.tryhackme.com
```

> Certificate Transparency logs are useful because certificates can expose subdomains that aren't obvious from the main website.

## Tips

### Privacy

Use privacy-focused DNS resolvers such as:

```text
1.1.1.1
```

Cloudflare also provides encrypted DNS through **DoH (DNS over HTTPS)** and **DoT (DNS over TLS)**.

### Defender Perspective

Monitor your organization's external footprint:

- Set up **Shodan/Censys alerts**.
- Monitor **Certificate Transparency (CT) logs** for newly issued certificates.
- Track **DNS changes**.
- Watch for unexpected subdomains that could create **subdomain takeover risks**.

### Authorization

Passive reconnaissance generally does not directly interact with the target, but your **overall reconnaissance and security testing must still be authorized and within scope**.

### Results Change

Reconnaissance results are not permanent:

- DNS records change.
- Subdomains can appear or disappear.
- Cloud infrastructure may rotate IP addresses.
- Privacy protections and WHOIS redactions can change.
- Certificate Transparency logs continue to grow.

**Always verify important findings before relying on them.**
