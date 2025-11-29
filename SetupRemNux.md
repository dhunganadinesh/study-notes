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


# 🛡️ Secure Malware Lab Network with Internet Access

This guide sets up two VMs using **VMware Workstation/Player**:

- **REMnux VM**: Acts as a secure gateway (with internet)
- **Malware VM**: Routes internet through REMnux for safe monitoring

---

## 🖥️ Architecture Overview

```
            +------------------+
            |   Host Machine   |
            |  (Isolated)      |
            +--------+---------+
                     |
            [ VMnet1 - Host-Only ]
                     |
        +------------+------------+
        |                         |
+---------------+        +----------------+
|   Malware VM  |        |   REMnux VM    |
| 192.168.56.101| <----> |192.168.56.1    |
| Gateway: .1   |        | NAT to Internet|
+---------------+        +----------------+
```

---

## 🧱 Step 1: Configure VMware Network

### ✅ REMnux VM

| Adapter # | Type      | Purpose            |
|-----------|-----------|--------------------|
| 1         | NAT       | Internet access    |
| 2         | Host-Only | Malware VM access  |

### ✅ Malware VM

| Adapter # | Type      | Purpose              |
|-----------|-----------|----------------------|
| 1         | Host-Only | Route via REMnux     |

---

## 🛠️ Step 2: Update REMnux Network Configuration

Edit the file:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Update it like this:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:   # NAT interface
      dhcp4: true
      dhcp6: false

    enp0s8:   # Host-only interface (check with `ip a`)
      dhcp4: no
      dhcp6: no
      addresses:
        - 192.168.56.1/24
```

Apply config:

```bash
sudo netplan apply
```

Verify:

```bash
ip a
```

---

## 🔄 Step 3: Enable Routing on REMnux

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

## 🔀 Step 4: Enable NAT with iptables

```bash
sudo iptables -t nat -A POSTROUTING -s 192.168.56.0/24 -o enp0s3 -j MASQUERADE
sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
```

Make rules persistent:

```bash
sudo apt install iptables-persistent
```

---

## 🪟 Step 5: Configure Malware Windows VM

Manually assign static IP:

| Field      | Value              |
|------------|--------------------|
| IP         | 192.168.56.101     |
| Subnet     | 255.255.255.0      |
| Gateway    | 192.168.56.1       |
| DNS        | 192.168.56.1 or 1.1.1.1 |

Disable all other adapters and test:

```cmd
ping 192.168.56.1
```

Try browsing (optionally simulate internet with INetSim on REMnux).

---

## 🔐 Security Best Practices

| Rule                            | Why                                      |
|---------------------------------|------------------------------------------|
| ❌ No shared folders             | Prevent malware escape                   |
| ❌ No clipboard/drag-drop        | Avoid accidental file transfer           |
| ❌ No bridge or NAT on malware VM| Prevent direct host/Internet access      |
| ✅ Use snapshots                 | Easy rollback after analysis             |
| ✅ Monitor traffic on REMnux     | Full visibility of malware behavior      |

---

## 📊 Optional Traffic Monitoring on REMnux

| Tool         | Purpose                      |
|--------------|------------------------------|
| `tcpdump`    | Packet capture                |
| `Wireshark`  | Visual traffic inspection     |
| `INetSim`    | Fake internet services        |
| `mitmproxy`  | Intercept HTTPS               |
| `Zeek`       | High-level traffic analysis   |

---

## ✅ Final Checklist

| Item                    | Status  |
|-------------------------|---------|
| REMnux NAT setup        | ✅       |
| Host-Only bridge active | ✅       |
| IP forwarding enabled   | ✅       |
| Malware VM routed via REMnux | ✅   |
| Host isolated           | ✅       |
| Logging active (optional) | ✅     |

---

## 🧯 To Reset or Stop Routing

```bash
sudo iptables -F
sudo iptables -t nat -F
```

---

## 🧪 Sample Test

```powershell
# From Malware VM PowerShell
Invoke-WebRequest http://example.com
```

Check REMnux with:

```bash
sudo tcpdump -i enp0s8
```

---


