# SOC Analyst Windows Event ID Cheat Sheet

This file lists critical Windows Security Event IDs that SOC analysts should be aware of, including relevance and common attack scenarios.

---

## 👤 Logon / Authentication Events
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 4624 | Successful logon | Track valid logons, brute force success |
| 4625 | Failed logon attempt | Brute force, password spraying |
| 4634 | Logoff | Track session termination |
| 4647 | User initiated logoff | Normal user activity |
| 4672 | Special privileges assigned | Admin / SYSTEM logon, privilege escalation |
| 4648 | Logon with explicit credentials | Pass-the-Hash, credential theft |
| 4768 | Kerberos TGT requested | Detect Kerberoasting attempts |
| 4769 | Kerberos service ticket requested | Abnormal volume = Kerberoasting |
| 4771 | Kerberos pre-auth failed | Password spraying, brute-force |
| 4776 | NTLM authentication | NTLM relay, brute-force |

---

## 🔑 Account Management
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 4720 | User account created | Malicious account creation |
| 4722 | User account enabled | Re-activation of disabled accounts |
| 4723 | Password change attempt | Unauthorized changes |
| 4724 | Password reset attempt | Privilege escalation |
| 4725 | User account disabled | Account tampering |
| 4726 | User account deleted | Covering tracks |
| 4738 | User account changed | Privilege modifications |
| 4740 | Account locked out | Brute force / password spray |
| 4756 | User added to security-enabled group | Privilege escalation |
| 4728 | User added to global group | Admin membership abuse |

---

## 🛡 Policy & Privilege Changes
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 4719 | System audit policy changed | Indicator of log tampering |
| 4732 | User added to local group | Local privilege escalation |
| 4735 | Local group changed | Privilege abuse |
| 4737 | Domain group changed | AD group escalation |
| 4907 | Audit policy change | Attempts to evade detection |

---

## 📂 Object Access / File Events
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 4663 | File or folder accessed | Sensitive file access |
| 4656 | Handle to object requested | Attempts to open restricted files |
| 4660 | Object deleted | Data destruction |
| 4670 | Permissions on object changed | Privilege escalation |

---

## 🖥 System & Service Events
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 4697 | Service installed | Persistence (malware services) |
| 7045 | New service installed (System) | Common malware persistence |
| 6005 | Event log service started | System reboot tracking |
| 6006 | Event log service stopped | Shutdown, possible tampering |
| 1102 | Audit log cleared | Attacker covering tracks |

---

## 🌐 Network & Firewall
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 5156 | Allowed connection | Baseline network flows |
| 5158 | Filtering platform permit | Firewall allowed traffic |
| 5152 | Blocked packet | Firewall blocking suspicious traffic |

---

## 🏰 Active Directory Specific
| Event ID | Description | SOC Relevance / Common Attacks |
|----------|-------------|--------------------------------|
| 4765 | SID history added | Golden ticket / DC persistence |
| 4766 | SID history failure | Attempted AD tampering |
| 4750 | Computer account created | Rogue device addition |
| 4741 | Computer account created | Domain persistence |

---

✅ **Tip for SOC Analysts:**  
- Focus on **4624, 4625, 4672, 4720, 4728, 4740, 4768–4769, 4697, 7045, 1102**.  
- Correlate logon types (in 4624) to distinguish local vs remote vs RDP logons.  
- Look for anomalies in **service creation, account changes, and log clearance**.  

