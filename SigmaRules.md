# Sigma Rules

**Sigma rules** are used in cybersecurity, specifically in **threat detection** and **SIEM (Security Information and Event Management)** systems.

Provides a standardized way to describe log-based detection rules, similar to how **Snort** or **YARA** rules work but for log data.

---

## 🔹 What is Sigma?

- Sigma is an **open standard** for writing rules that **detect suspicious or malicious activity** in **log files**.
- The goal is to make these rules **platform-agnostic**, so a single Sigma rule can be converted to work with many different SIEM tools like **Splunk**, **Elastic (ELK)**, **Graylog**, **Microsoft Sentinel**, etc.

---

## 🔹 Structure of a Sigma Rule

A typical Sigma rule is written in **YAML** and includes:

1. **Metadata**: title, id, author, description, etc.
2. **Log source**: type of logs (e.g., Windows Security, Sysmon, etc.)
3. **Detection logic**: conditions to match (e.g., process name, command line, file path).
4. **Level/severity**: informational, low, medium, high, critical.
5. **Tags**: MITRE ATT&CK techniques, tactics, etc.

---

## 🔹 Example Sigma Rule

```yaml
title: Suspicious PowerShell Download
id: 1234-5678-9012
description: Detects PowerShell commands downloading files from the internet
author: YourName
date: 2025/08/05
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: powershell.exe
    CommandLine|contains: "Invoke-WebRequest"
  condition: selection
level: high
tags:
  - attack.execution
  - attack.t1059.001

```

## 🔹 Tools Related to Sigma

Here are some popular tools used to work with Sigma rules:

### 1. [sigmac](https://github.com/SigmaHQ/sigmac)
A command-line tool that converts Sigma rules into SIEM-specific query formats like:
- SPL (Splunk)
- KQL (Azure Sentinel)
- EQL (Elastic)
- Others...

GitHub Repo: [https://github.com/SigmaHQ/sigmac](https://github.com/SigmaHQ/sigmac)

---

### 2. [Uncoder.IO](https://uncoder.io/)
A free, web-based GUI tool to:
- Write Sigma rules
- Convert between formats like Sigma, Splunk SPL, Elastic DSL, KQL, and more
- Preview queries in real-time

Website: [https://uncoder.io/](https://uncoder.io/)

---

### 3. [SigmaHQ (Main Project)](https://github.com/SigmaHQ/sigma)
The official Sigma rules repository with hundreds of detection rules contributed by the security community.

GitHub Repo: [https://github.com/SigmaHQ/sigma](https://github.com/SigmaHQ/sigma)

---

### 4. [SIGMA-CLI](https://github.com/SigmaHQ/sigma-cli)
A newer and more powerful CLI tool from the Sigma team for rule conversion, validation, and searching.

GitHub Repo: [https://github.com/SigmaHQ/sigma-cli](https://github.com/SigmaHQ/sigma-cli)

---

### 5. [Sigma Visual Rule Editor (VRE)](https://sigma.vx-underground.org/)
A browser-based Sigma rule builder/editor powered by VX-Underground.

Website: [https://sigma.vx-underground.org/](https://sigma.vx-underground.org/)

---

