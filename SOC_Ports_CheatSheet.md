# SOC Analyst Ports Cheat Sheet

This file lists the most important network ports SOC analysts should be aware of, with relevance and common attack vectors.

---

## 🔑 Authentication & Remote Access
| Port | Protocol | Service | SOC Relevance / Common Attacks |
|------|----------|---------|--------------------------------|
| 22   | TCP      | SSH     | Brute-force, tunneling, lateral movement |
| 23   | TCP      | Telnet  | Legacy, clear-text, IoT attacks |
| 3389 | TCP/UDP  | RDP     | Brute-force, ransomware, initial access |
| 5900 | TCP      | VNC     | Remote desktop, weak authentication |

---

## 🌐 Web Services
| Port | Protocol | Service | SOC Relevance / Common Attacks |
|------|----------|---------|--------------------------------|
| 80   | TCP      | HTTP    | Web traffic, vulnerable apps, exploits |
| 443  | TCP      | HTTPS   | Encrypted traffic, C2 hiding |
| 8080 | TCP      | Alt HTTP| Proxies, malware communications |
| 8443 | TCP      | Alt HTTPS| Web servers, admin panels |

---

## 📧 Email
| Port | Protocol | Service | SOC Relevance / Common Attacks |
|------|----------|---------|--------------------------------|
| 25   | TCP      | SMTP    | Spam relays, phishing infra |
| 465  | TCP      | SMTPS   | Encrypted email relay |
| 587  | TCP      | Submission | Legit mail relay (can be abused) |
| 110  | TCP      | POP3    | Legacy retrieval, credential theft |
| 143  | TCP      | IMAP    | Account takeover, mailbox access |
| 993  | TCP      | IMAPS   | Encrypted mail retrieval |
| 995  | TCP      | POP3S   | Encrypted mail retrieval |

---

## 📡 File Transfer & Sharing
| Port     | Protocol | Service | SOC Relevance / Common Attacks |
|----------|----------|---------|--------------------------------|
| 20, 21   | TCP      | FTP     | Clear-text credentials, data theft |
| 69       | UDP      | TFTP    | Simple, used in IoT attacks |
| 137-139  | TCP/UDP  | NetBIOS | Reconnaissance, Windows sharing |
| 445      | TCP      | SMB     | Ransomware lateral movement, EternalBlue |
| 2049     | TCP/UDP  | NFS     | Data exfiltration, Linux/Unix shares |

---

## 🛠 Infrastructure & Databases
| Port | Protocol | Service | SOC Relevance / Common Attacks |
|------|----------|---------|--------------------------------|
| 53   | TCP/UDP  | DNS     | Exfiltration, tunneling, DDoS |
| 389  | TCP      | LDAP    | AD queries, exploitation |
| 636  | TCP      | LDAPS   | Secure AD queries |
| 1433 | TCP      | MS SQL  | DB attacks, credential theft |
| 1521 | TCP      | Oracle DB | SQL injection, brute-force |
| 3306 | TCP      | MySQL   | Exposed DBs, credential theft |
| 5432 | TCP      | PostgreSQL | Exploits, brute-force |
| 27017| TCP      | MongoDB | Often exposed, data leaks |

---

## 🎮 Malware & Exploitation Ports
| Port | Protocol | Service | SOC Relevance / Common Attacks |
|------|----------|---------|--------------------------------|
| 135  | TCP      | MS RPC  | Lateral movement, DCOM abuse |
| 161  | UDP      | SNMP    | Device enumeration |
| 500  | UDP      | IKE     | VPN brute-force, tunneling |
| 1723 | TCP      | PPTP    | Legacy VPN, weak encryption |
| 1194 | UDP      | OpenVPN | VPN tunneling |
| 4444 | TCP      | Metasploit default | Payload listener, C2 |

---

## 📲 Misc & High-Risk
| Port   | Protocol | Service | SOC Relevance / Common Attacks |
|--------|----------|---------|--------------------------------|
| 6667   | TCP      | IRC     | Botnets, legacy C2 |
| 1337   | TCP      | Custom  | Malware backdoor (leet) |
| 31337  | TCP      | Back Orifice | Classic backdoor |
| 65535  | TCP/UDP  | High port | Malware/exfiltration |

---

✅ **Tip for SOC Analysts:**  
- Focus on deviations (e.g., RDP on port 443).  
- Monitor exfiltration over DNS (53), HTTPS (443), or high ports.  
- Combine port awareness with context (process, user, geography).  

