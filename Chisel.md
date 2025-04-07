# Chisel

**GitHub:** [chisel-GitHub-Repository.](https://github.com/jpillora/chisel)

Chisel is a Go-based tool for encrypted tunneling and network pivoting, enabling secure, authenticated connections to bypass firewalls and NAT using reverse TCP tunnels.

---

## 📦 Available Builds

- Linux AMD64  
- Linux 386  
- Windows AMD64  
- Windows 386  

---

## 🛠️ Installation

### Install Chisel using APT

```bash
apt install chisel
```

### Verify Installation

```bash
chisel -v
```

### Display Help

```bash
chisel -h
```

---

## 📁 Setup

### Create Directory

```bash
mkdir /opt/chisel
```

### Copy Files

```bash
cp -v chisel 1.9.1_* /opt/chisel/
cd /opt/chisel/
```

### Unzip Files

```bash
gunzip chisel 1.9.1_*
```

---

## 🌐 Server Setup

### Help and Manual

```bash
chisel -h
man chisel
chisel server -h
```

### Start Server (Reverse Mode)

```bash
chisel server -p 8000 --reverse
```

Alternative:

```bash
chisel server --port 8000 --reverse
```

---

## 👨‍💻 Client Setup

### Download and Prepare Client

```bash
wget http://192.168.1.6/chisel
gunzip chisel 1.7.6_linux_amd64.gz
mv chisel 1.7.6_linux_amd64.gz chisel
chmod +x chisel
```

### Start Simple HTTP Server (Optional)

```bash
python2 -m SimpleHTTPServer 80
```

---

## 🔁 Port Forwarding with Chisel

### 🔒 Local Port Forwarding

Forward local port 1337 to remote 127.0.0.1:1337:

```bash
chisel client 10.10.14.6:8000 R:1337:127.0.0.1:1337
```

- `10.10.14.6` – Chisel server IP  
- `8000` – Chisel server port  
- `R:1337:127.0.0.1:1337` – Route config  

### 📡 Remote Port Forwarding

Expose a service from client to Chisel server:

```bash
./chisel client 192.168.1.7:80 R:8080:127.0.0.1:8080
```

Other examples:

```bash
./chisel client 192.168.1.6:8000 R:22:127.0.0.1:22
chisel client 192.168.1.6:8000 R:80:10.0.0.100:80
```

#### Windows Example

```bash
chisel.exe client 192.168.1.7:21 R:80:127.0.0.1:80
```

---

## 🔄 Dynamic Port Forwarding

Set up a SOCKS proxy:

```bash
chisel client 192.168.1.6:8000 R:socks
```

- `192.168.1.6` – Chisel server IP  
- `8000` – Chisel server port  
- `R:socks` – Enables SOCKS proxy (default port 1080)

---

**🚨 Use Chisel responsibly and only in authorized environments.**
