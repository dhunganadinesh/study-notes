# 🧪 REMnux as Proxy Server for Malware Analysis

This guide configures **REMnux** to act as a fake internet proxy for malware running in a Windows VM. You'll capture and simulate network traffic using tools like **INetSim** and **mitmproxy**.

---

## 📐 Network Setup Overview

```
+------------------------+         +------------------------+
|  Windows VM (malware)  | <--->   |     REMnux VM (proxy)  |
|  IP: 192.168.56.101    |         |  IP: 192.168.56.1       |
|  GW/DNS: 192.168.56.1  |         |  Running INetSim       |
+------------------------+         +------------------------+
             ↑                              ↑
           Host-Only Network (no internet access)
```

---

## 🛠️ Requirements

- REMnux installed in a VM
- Windows malware analysis VM
- VMware with Host-Only network (e.g., vmnet1)
- Administrator/root access to both VMs

---

## 📶 Step 1: Configure REMnux Network (Static IP)

1. Open terminal in REMnux.

2. Edit network config:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

3. Example config (update `ens33` if your interface differs):

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.56.1/24]
```

4. Apply changes:

```bash
sudo netplan apply
```

5. Confirm:

```bash
ip a
```

---

## 🔄 Step 2: Launch INetSim

INetSim is pre-installed on REMnux and simulates services like DNS, HTTP, FTP, SMTP, etc.

```bash
sudo systemctl start inetsim
```

Check status:

```bash
sudo systemctl status inetsim
```

Log files are stored in:

```bash
/var/log/inetsim/
```

To monitor:

```bash
sudo tail -f /var/log/inetsim/service.log
```

---

## 🪟 Step 3: Configure Windows Malware VM

1. Set **manual IP** in Windows:
   - IP: `192.168.56.101`
   - Subnet: `255.255.255.0`
   - Gateway: `192.168.56.1`
   - DNS: `192.168.56.1`

2. Disable all other network adapters.

3. Test network (ping REMnux):

```cmd
ping 192.168.56.1
```

---

## 🔎 Step 4 (Optional): Intercept HTTPS with mitmproxy

1. Run mitmproxy:

```bash
mitmproxy -p 8080
```

2. In Windows malware VM, set proxy manually:
   - HTTP/HTTPS proxy: `192.168.56.1:8080`

3. (Optional) Install mitmproxy cert to trust HTTPS interception.

---

## 📊 Tools Available in REMnux

| Tool        | Description                               |
|-------------|-------------------------------------------|
| **INetSim** | Fake internet services (HTTP, FTP, DNS...)|
| **mitmproxy** | HTTPS proxy with inspection             |
| **tcpdump**  | Raw packet capture                       |
| **Fakenet-NG** | Simulates services and captures traffic |

---

## 🧪 Sample Output You Can Capture

- DNS queries (to fake domains)
- HTTP/HTTPS URLs accessed
- Malware trying to send email
- Beaconing behavior
- File download/upload attempts

---

## 🧼 Stop Services / Reset

To stop INetSim:

```bash
sudo systemctl stop inetsim
```

To reset networking:

```bash
sudo netplan apply
```

---

## 🧷 Security Tips

- Keep malware VM isolated (no internet)
- Disable clipboard & shared folders
- Revert snapshots after each test
- Monitor traffic in real-time

---

## ✅ Summary

| Step               | Description                        |
|--------------------|------------------------------------|
| Set static IP      | REMnux = 192.168.56.1              |
| Start INetSim      | Fake internet services             |
| Point malware VM   | GW/DNS = 192.168.56.1              |
| Observe traffic    | INetSim logs, mitmproxy, tcpdump   |

---

🔁 You can combine this setup with tools like **Wireshark**, **Zeek**, or **Arkime** to build full network visibility in your malware lab.
