# Cybersecurity Threats Overview

## Web application
- **SQL injection** — attacker injects DB queries.  
  *Mitigation:* parameterised queries / input validation / WAF.
- **Cross-site scripting (XSS)** — attacker injects script into webpages.  
  *Mitigation:* output encoding / CSP / input sanitisation.
- **Cross-Site Request Forgery (CSRF)** — unwanted actions via authenticated user.  
  *Mitigation:* anti-CSRF tokens / SameSite cookies.
- **Server-Side Request Forgery (SSRF)** — server forced to fetch attacker URLs.  
  *Mitigation:* allowlist outbound requests / egress filtering.
- **Command injection / RCE** — execute commands on server.  
  *Mitigation:* avoid shell calls / strict input validation.
- **Path traversal** — access files outside intended directory.  
  *Mitigation:* canonicalise paths / denylist traversal sequences.
- **Insecure deserialization** — remote object deserialisation leads to code exec.  
  *Mitigation:* avoid deserializing untrusted data / use safe formats.

## Authentication & access
- **Broken authentication / session hijack** — stolen or weak creds/sessions.  
  *Mitigation:* MFA / secure cookies / session expiry.
- **Credential stuffing / brute force** — automated login attempts.  
  *Mitigation:* rate limits / IP reputation / MFA.
- **Privilege escalation** — gain higher rights.  
  *Mitigation:* least privilege / patching / audit logs.
- **Insecure direct object reference (IDOR)** — object access by manipulating IDs.  
  *Mitigation:* access checks per request.

## Network & transport
- **Man-in-the-Middle (MitM)** — eavesdrop or modify traffic.  
  *Mitigation:* TLS everywhere / certificate pinning.
- **Evil Twin / Rogue AP** — fake Wi-Fi to intercept traffic.  
  *Mitigation:* WPA2/3 + 802.1X / avoid public Wi-Fi / VPN.
- **ARP poisoning (ARP spoofing)** — local network traffic interception.  
  *Mitigation:* static ARP entries / network segmentation / switch port security.
- **DNS poisoning / DNS spoofing** — tamper DNS responses.  
  *Mitigation:* DNSSEC / use trusted resolvers / DNS monitoring.
- **DHCP spoofing** — attacker provides rogue network settings.  
  *Mitigation:* DHCP snooping / port security.

## Malware & endpoint
- **Ransomware** — encrypts files for ransom.  
  *Mitigation:* backups offline / EDR / patching.
- **Trojans / Remote Access Trojans (RATs)** — covert backdoors.  
  *Mitigation:* EDR / application allowlisting / user training.
- **Botnets** — infected devices for C2 and DDoS.  
  *Mitigation:* network monitoring / sinkholing / patching endpoints.
- **Rootkits / bootkits** — persistent kernel/boot compromise.  
  *Mitigation:* secure boot / integrity checks / rebuild systems.
- **Cryptojacking** — unauthorised crypto mining.  
  *Mitigation:* monitor CPU/network anomalies / block mining domains.

## Social engineering
- **Phishing / spear-phishing** — credential theft via email.  
  *Mitigation:* user training / email filtering / MFA.
- **Vishing / smishing** — phone/SMS social engineering.  
  *Mitigation:* verification procedures / user awareness.
- **Business Email Compromise (BEC)** — fraudulent wire/payment requests.  
  *Mitigation:* dual-approval financial controls.

## Supply chain & third party
- **Software supply-chain compromise** — malicious dependency or build.  
  *Mitigation:* provenance checks / signed packages / SBOM.
- **Dependency confusion / typosquatting** — malicious public package impersonates internal one.  
  *Mitigation:* scoped registries / allowlists.

## Cloud & infrastructure
- **Cloud misconfiguration** — open S3 buckets, permissive IAM.  
  *Mitigation:* automated configuration scans / least privilege / IaC reviews.
- **Container escape** — break out of container to host.  
  *Mitigation:* runtime restrictions / namespaces / image scanning.
- **API abuse / exposed keys** — leaked credentials or open APIs.  
  *Mitigation:* rotate keys / IAM roles / API rate limiting.

## Identity & data
- **Insider threat** — malicious or negligent insider leaks data.  
  *Mitigation:* monitoring / data loss prevention / least privilege.
- **Data exfiltration** — covert removal of sensitive data.  
  *Mitigation:* DLP / network egress controls / encryption at rest/in transit.
- **Supply of forged certificates** — fake TLS certs.  
  *Mitigation:* CT logs / cert monitoring / CAs validation.

## Availability & attacks at scale
- **DDoS / amplification attacks** — overwhelm services.  
  *Mitigation:* CDNs / DDoS protection / autoscaling policies.
- **Port scanning / reconnaissance** — map attack surface.  
  *Mitigation:* honeypots / IDS/IPS / reduce exposed services.

## Emerging / niche
- **SEO poisoning (search poisoning)** — malicious pages rank highly to distribute malware.  
  *Mitigation:* user awareness / verify domains.
- **Zero-day exploits** — unpatched unknown vulnerabilities.  
  *Mitigation:* layered defenses / rapid patching when available / virtual patching via WAF.
- **Side-channel attacks** — leak via timing/power.  
  *Mitigation:* constant-time algorithms / hardware controls.
- **IoT insecurity** — default creds, weak updates.  
  *Mitigation:* network segmentation / device inventory / firmware management.

## Detection & general mitigations
- **Detection** — centralised logging, EDR, network IDS, SIEM, anomaly detection.
- **Prevention baseline** — strong patching cadence, MFA, least privilege, secure SDLC, backups, segmentation, user training, threat intel.
