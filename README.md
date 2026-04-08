# Network Mapper - Advanced CLI Port Scanner

A comprehensive, feature-rich command-line network port scanner with host discovery, OS fingerprinting, service detection, and detailed reporting capabilities.

**Author:** Aman Gour

---

## Features

- **Host Discovery** – Ping sweep a /24 subnet to identify live hosts
- **OS Fingerprinting** – TTL-based operating system detection
- **MAC / Vendor Lookup** – Retrieve MAC addresses and identify device vendors from ARP table
- **TCP + UDP Scan Modes** – Support for both TCP and UDP port scanning protocols
- **SYN Stealth Probe** – Raw socket-based half-open scanning (requires root/sudo; auto-fallback to standard TCP)
- **Port Profiles** – Pre-defined scanning profiles:
  - `web` – Web services (80, 443, 8000, 8008, 8080, 8443, 8888)
  - `db` – Database services (1433, 1521, 3306, 5432, 6379, 27017, etc.)
  - `remote` – Remote access services (21, 22, 23, 3389, 5900, 10000)
  - `mail` – Mail services (25, 110, 143, 465, 587, 993, 995)
  - `top100` – Top 100 well-known ports
  - `full` – All 65535 ports
  - `custom` – User-defined port ranges
- **Banner Grabbing** – Extract version strings and service information from open ports
- **Risk Tagging** – Color-coded port risk classification:
  - 🔴 **HIGH** – Critical security services
  - 🟡 **MEDIUM** – Standard services with vulnerability potential
  - 🟢 **LOW** – Generally low-risk services
- **ETA Display** – Real-time estimated time remaining for scan completion
- **HTML Report** – Generate styled, browser-friendly scan reports
- **Export Formats** – Save results in CSV, JSON, or Text formats
- **Scan History** – Persistent scan history stored in `scan_history.json`
- **Scan Comparison** – Diff against previous scans to identify changes
- **Interactive Menu** – User-friendly menu-driven interface for all options
- **Multi-threaded** – Concurrent scanning for improved performance
- **Color-coded Output** – Rich ANSI color formatting for terminal output (auto-disabled in pipes)

---

## Requirements

- **Python 3.6+**
- **Supported Platforms:** Linux, macOS, Windows
- **Privileges:** Standard user (elevated privileges required for SYN stealth probes)

### Python Standard Library Only
Network Mapper uses only Python's standard library – no external dependencies required!

---

## Installation

1. Clone or download the project
2. Ensure Python 3.6+ is installed
3. Run directly:
   ```bash
   python3 Network\ Mapper.py
   ```

---

## Usage

### Basic Usage

```bash
# Interactive menu (guided mode)
python3 Network\ Mapper.py

# Command-line mode (target and options)
python3 Network\ Mapper.py <target> [options]
```

### Examples

```bash
# Scan a single host with top 100 ports
python3 Network\ Mapper.py 192.168.1.1 --profile top100

# Scan a subnet for live hosts
python3 Network\ Mapper.py 192.168.1.0/24 --discover

# Scan web services only
python3 Network\ Mapper.py 10.0.0.5 --profile web

# Full port scan with banner grabbing
python3 Network\ Mapper.py example.com --profile full --banners

# Export to JSON
python3 Network\ Mapper.py 192.168.1.100 --output scan_result.json

# UDP scan
python3 Network\ Mapper.py 192.168.1.1 --udp

# Custom port range
python3 Network\ Mapper.py 192.168.1.1 --ports 20-80,443,8080-8090
```

---

## Supported Services

The scanner includes built-in service maps for **70+ common ports**, including:

### Web Services
- HTTP (80), HTTPS (443), HTTP-Dev (8000), HTTP-Proxy (8080), Jupyter (8888)

### Database Services
- MySQL (3306), PostgreSQL (5432), MSSQL (1433), MongoDB (27017), Redis (6379), Elasticsearch (9200)

### Remote Access
- SSH (22), RDP (3389), VNC (5900), Telnet (23)

### Network Services
- DNS (53), DHCP (67-68), SNMP (161), LDAP (389), NTP (123)

### Cloud & Container Services
- Docker (2375), Kubernetes (6443), AWS, GCP services

And many more...

---

## Output Examples

### Terminal Output
```
Port Scanner Report for 192.168.1.100
=====================================

Scanning 100 ports...
[████████████░░░░░░░░░░░░░░░░░░░░░░░░] 35% - ETA: 2m 15s

Open Ports:
  22    SSH         MEDIUM  ✓ OpenSSH_7.4
  80    HTTP        LOW     ✓ Apache/2.4.6
  443   HTTPS       LOW     ✓ nginx/1.14.0
  3306  MySQL       HIGH    Response received

Scan History:
  • Last scan: 2 days ago (8 ports open)
  • Changes: +1 new service detected
```

### HTML Report
Beautiful styled HTML report opens automatically in browser with:
- Service summary
- Risk level visualization
- Port details and banners
- Timeline information
- Scan metadata

### JSON Export
```json
{
  "target": "192.168.1.100",
  "timestamp": "2026-04-03T10:30:45",
  "profile": "top100",
  "open_ports": [
    {
      "port": 22,
      "service": "SSH",
      "risk": "MEDIUM",
      "banner": "OpenSSH_7.4"
    }
  ]
}
```

---

## Risk Levels

### HIGH RISK Ports
FTP (21), Telnet (23), RPCbind (111), NetBIOS (137-139), SMB (445), rsh (514), NFS (2049), Docker (2375), VNC (5900), Redis (6379), Memcached (11211), MongoDB (27017)

### MEDIUM RISK Ports
SSH (22), SMTP (25), DNS (53), HTTP (80), IMAP (143), HTTPS (443), LDAP (389), MSSQL (1433), Oracle (1521), MySQL (3306), PostgreSQL (5432), Elasticsearch (9200)

### LOW RISK Ports
All other common services with limited attack surface

---

## Advanced Features

### Host Discovery Sweep
Performs a network sweep to identify live hosts before scanning:
```bash
python3 Network\ Mapper.py 192.168.1.0/24 --discover
```

### OS Detection
Automatic OS fingerprinting based on TTL values:
- TTL ≤ 64: Linux / Unix / Android
- TTL ≤ 128: Windows
- TTL ≤ 255: Cisco / Network Devices

### Scan History
Each scan is automatically saved to `scan_history.json` with:
- Target and timestamp
- Open ports and services detected
- Banner information
- Historical comparison

### Banner Grabbing
Detects service versions and banners from open ports:
- FTP, SSH, Telnet, DNS, HTTP/HTTPS
- SMTP, POP3, IMAP, and many more

---

## Files

- **Network Mapper.py** – Main application (full-featured interactive scanner)
- **port_scanner.py** – Modular port scanner library
- **scan_history.json** – Persistent scan history and results
- **README.md** – This documentation file

---

## Performance

- **Multi-threaded scanning** for fast port enumeration
- **Configurable thread pool** for resource management
- **Real-time progress display** with ETA calculation
- **Efficient service detection** without external requests

---

## Limitations

- **SYN stealth scanning** requires root/sudo privileges (falls back to TCP connect scan)
- **Some network restrictions** may block ICMP (ping) for host discovery
- **Firewall/IDS** may interfere with scanning results
- **Rate limiting** may be necessary for large networks

---

## Security Considerations

⚠️ **Legal Notice:** Only scan networks and systems you own or have explicit permission to scan. Unauthorized port scanning may be illegal in your jurisdiction.

- Network Mapper should only be used for:
  - Your own infrastructure assessments
  - Authorized security testing
  - Network administration and maintenance
  - Educational purposes in controlled environments

---

## Troubleshooting

### Scan timeout
- Increase timeout values
- Reduce thread count for slower networks
- Check firewall rules

### No ports detected
- Verify target is reachable (ping first)
- Check firewall blocking
- Try with elevated privileges for stealth scanning
- Consider network latency

### High CPU usage
- Reduce thread pool size
- Scan smaller port ranges
- Run during off-peak hours

---

## Future Enhancements

- Service version detection via banner analysis
- Vulnerability database integration
- Geo-location and WHOIS lookup
- Network visualization and mapping
- Scheduled scan automation
- REST API interface

---

## License

Open source – use freely for authorized testing and network administration.

---

## Support & Feedback

For issues, improvements, or feature requests, this tool continues to evolve for network professionals and security researchers.

---

**Last Updated:** April 3, 2026  
**Version:** Full Edition  
**Author:** Aman Gour
