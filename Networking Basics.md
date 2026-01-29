---
tags:
  - main
---
# Networking Fundamentals 

---

## 📖 Table of Contents
1. [[#1. How the Web Works (HTTP Basics)]]
2. [[#2. Protocols: HTTP vs. HTTPS]]
3. [[#3. The Address Book: DNS]]
4. [[#4. Network Barriers: VPNs, Firewalls & Proxies]]
5. [[#5. Common Issues & Troubleshooting]]
6. [[#6. Python Networking Best Practices]]

---

## 1. How the Web Works (HTTP Basics)

### The Request/Response Cycle
When you visit a URL like `https://openai.com`, or make a request via Python, a specific sequence of events occurs.

1.  **DNS Lookup:** The computer converts the domain (`openai.com`) into an IP address (`104.x.x.x`).
2.  **TCP Connection:** A reliable connection ("handshake") is established with that IP.
3.  **HTTP Request:** The client sends a message to the server.
```http
GET / HTTP/1.1
	Host: openai.com
```
4.  **Server Response:** The server sends back data (HTML, JSON, etc.).
```http
 HTTP/1.1 200 OK
    Content-Type: text/html
```
4.  **Rendering/Processing:** The browser displays the page, or your Python script parses the text.

### Python Example
Python performs these exact same internal steps when using the `requests` library.

```python
import requests

# This single line performs DNS -> TCP -> Request -> Response
r = requests.get("[https://openai.com](https://openai.com)")

print(r.status_code) # 200
print(r.text)        # HTML content
````

> [!NOTE] Key Concept
> 
> HTTP is a **stateless request/response protocol**. Every interaction starts with a client request and ends with a server response.

---

## 2. Protocols: HTTP vs. HTTPS

### Comparison

|**Feature**|**HTTP**|**HTTPS**|
|---|---|---|
|**Full Name**|Hypertext Transfer Protocol|HTTP Secure|
|**Security**|Plain Text|Encrypted (TLS/SSL)|
|**Visibility**|Anyone on the network can read/modify|Only endpoints can read data|
|**Port**|80|443|

### Why HTTPS Matters

Without encryption (TLS), malicious actors on the network (like in a coffee shop or compromised router) can:

- Steal passwords/API keys.
- Alter data in transit (Man-in-the-Middle attacks).
- Impersonate servers.

> **Rule:** Always use HTTPS in production. All modern APIs require it.

---

## 3. The Address Book: DNS

**DNS (Domain Name System)** acts as the phonebook of the internet, translating human-readable names into machine-readable IP addresses.

**Example:** `google.com` → `142.250.x.x`

### The Lookup Flow

1. **Local Cache:** Browser/OS checks if it already knows the address.
2. **OS Resolver:** Asks the Operating System.
3. **DNS Server:** Queries external providers (ISP, Google, Cloudflare).
### Common DNS Providers

- **Google:** `8.8.8.8` / `8.8.4.4`
- **Cloudflare:** `1.1.1.1`

### Symptoms of Failure

- "Could not resolve host"
- "Name or service not known"
- `socket.gaierror` in Python.

---

## 4. Network Barriers: VPNs, Firewalls & Proxies

Modern networks are rarely direct lines from Client to Server. Traffic passes through various intermediaries.

### A. VPNs (Virtual Private Networks)

Creates an encrypted tunnel to another server, changing your IP and encrypting traffic.

- **Impact:** Often overrides local DNS settings.
- **Issues:** Can cause broken DNS, blocked APIs, or certificate errors if the VPN endpoint is flagged.

### B. Firewalls

Software or hardware that filters traffic based on rules.

- **Types:** Local (Windows Defender, macOS) or Network (Corporate Gateways).
- **Blocking Targets:** Ports (80, 443), specific domains, or specific applications (e.g., blocking `python.exe` from accessing the web).
### C. Corporate Security (Zscaler, Netskope, etc.)

Large organizations often route all traffic through security proxies.

- **MITM Inspection:** These proxies decrypt your HTTPS traffic to scan for viruses/leaks, then re-encrypt it using a corporate certificate.
- **The Problem:** Python tools (pip, requests) often do not trust the corporate certificate authority (CA) by default, leading to SSL errors.

> [!TIP] Student Advice
> 
> If you are on a strict corporate network and facing constant blocks:
> 
> 1. Try a **mobile hotspot**.
>     
> 2. **Disable VPN** temporarily to test.
>     
> 3. Use a **personal device** if policy permits.
>     

---

## 5. Common Issues & Troubleshooting

### 🛑 Issue: SSL Certificate Errors

**Error:** `SSL: CERTIFICATE_VERIFY_FAILED`

**Cause:** Corporate MITM proxies, missing root certs, or outdated local certificates.

**Fixes:**

1. **Update Certifi:**
```bash
uv pip install --upgrade certifi
```
1. **Trust Corporate CA:** Install your company's Zscaler/Root certificate into your OS trust store.
2. **Dangerous Bypass (Debugging only):**
```Python
requests.get(url, verify=False) # ⚠️ NOT for production
```
### 🛑 Issue: DNS Failures

**Symptoms:** Python can't find the host, but some sites work in the browser.

**Fixes:**

- **Flush DNS:** Restart router or computer.
- **Change DNS Server:** Manually set DNS to `8.8.8.8` or `1.1.1.1`.
- **Disable VPN:** VPNs often handle DNS poorly.

### 🔬 Debugging Tools

If Python fails, test outside of Python first.

- **Curl:** `curl -v https://api.openai.com`
- **Ping:** `ping google.com`
- **Nslookup:** `nslookup openai.com`

> If `curl` or `ping` fails, your Python code will fail too. The issue is the network, not the code.

---

## 6. Python Networking Best Practices

### 1. Always Use Timeouts

Never let a request hang indefinitely.

Python

```
# Raises an error if no response within 10 seconds
requests.get("[https://api.openai.com](https://api.openai.com)", timeout=10)
```

### 2. Catch Exceptions

Handle network failures gracefully.

Python

```
try:
    response = requests.get(url, timeout=5)
    response.raise_for_status() # Check for HTTP errors (4xx/5xx)
except requests.exceptions.RequestException as e:
    print(f"Network error occurred: {e}")
```

### 3. Golden Rule

> **Never suppress SSL verification (`verify=False`) permanently.** It defeats the purpose of HTTPS and leaves you vulnerable to attacks. Use it _only_ for local debugging.
