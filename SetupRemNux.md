# 🛠️ Malware Analysis Lab with REMnux and Windows VM

## 📦 Overview
This guide walks through the process of setting up a malware analysis environment using two virtual machines:

- **Windows VM (Malware Lab):** For executing and analyzing malware samples.
- **REMnux VM:** Acts as a proxy/NAT gateway for logging and optionally manipulating malware traffic.

Both VMs are configured to allow internet access in a controlled and secure way, with logging and isolation.

---

## 🖥️ Windows VM Setup (Malware Lab)

### ✅ Base Configuration
- Use Windows 10/11
- Install with minimal features
- No automatic updates or Defender

### 🔧 Software via Chocolatey
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Install tools:
```powershell
choco install wireshark processhacker procexp autoruns sysinternals -y
choco install pe-bear dnspy -y
```

Disable features:
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
Set-MpPreference -DisableIOAVProtection $true
```

Disable networking features (optional):
- Disable file sharing
- Disable network discovery
- Disable RDP

---

## 🧪 REMnux VM Setup

### 🔧 Configure as NAT Proxy Gateway

#### Network Configuration (netplan)
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
    enp0s8:
      addresses:
        - 192.168.56.1/24
```
Apply with:
```bash
sudo netplan apply
```

#### Enable IP Forwarding
```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo sysctl -w net.ipv4.ip_forward=1
```
To make it persistent:
```bash
sudo nano /etc/sysctl.conf
# Add:
net.ipv4.ip_forward=1
```

#### Setup iptables for NAT
```bash
sudo iptables -t nat -A POSTROUTING -s 192.168.56.0/24 -o enp0s3 -j MASQUERADE
sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
```

Make persistent (optional):
```bash
sudo apt install iptables-persistent
```

---

## 🔐 Isolation & Security Practices

- Use **host-only adapter** for malware VM
- Use **two NICs** on REMnux: one NAT (internet), one host-only
- Disable all shared folders, clipboard sync, drag-and-drop
- Keep malware VM **snapshot ready**
- Treat REMnux as untrusted — do not open malware samples on it

---

## 📡 Logging Malware Traffic

### 1. `tcpdump` (PCAP Capture)
```bash
sudo tcpdump -i enp0s8 -w malware_traffic.pcap
```
Optional: Filter by IP
```bash
sudo tcpdump -i enp0s8 host 192.168.56.101 -w filtered.pcap
```

### 2. Real-Time Monitoring
```bash
sudo tcpdump -i enp0s8 -nn -v
sudo tcpdump -i enp0s8 -A port 80  # ASCII payloads
```

### 3. Analyze in Wireshark
```bash
wireshark malware_traffic.pcap
```

### 4. Zeek for Structured Logs
```bash
sudo zeek -i enp0s8
```
Generates logs:
- `conn.log`, `http.log`, `dns.log`, etc.

---

## ❓ Can Malware Infect REMnux?

### Theoretical Risk: ✅ Yes  
If:
- You run malware on REMnux
- Malware exploits Linux services (very rare)

### Mitigation:
- Don't open samples on REMnux
- Use snapshots
- Disable unnecessary services
- Monitor REMnux like a sacrificial VM

---

## 🧠 FAQs

### ❓ Why allow internet access to malware?
To observe real-world C2 behavior, DNS lookups, payload fetching, etc.

### ❓ Isn’t it unsafe?
Not if:
- Malware VM is isolated
- REMnux only forwards traffic
- Proper logging is in place

### ❓ What blocks the malware?
By default: **nothing** — REMnux only logs. Use tools like:
- `iptables` for blocking
- `INetSim` to simulate fake internet
- `Suricata` for IDS

---

**Maintainer:** Your Name  
**Last Updated:** 2025-07-29
