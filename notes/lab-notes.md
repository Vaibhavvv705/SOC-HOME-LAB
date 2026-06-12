# SOC Home Lab Notes

## Environment
- Host: MacBook Air M4
- Attacker VM: Kali Linux
- Manager VM: Ubuntu Server (Wazuh)

## Commands Learned
- ip a
- hostname -I
- ping
- free -h
- nproc
- git init

# Lab Day 1

## Kali Linux

IP Address:
10.0.2.15

RAM:
4 GB

CPU:
4 Cores

Tools Installed:
- Git
- Wireshark

Commands Practiced:
- ip a
- hostname -I
- free -h
- nproc
- git init
- git add
- git commit
- git push

## Wireshark Investigation 1

Filter Used:
icmp

Observed:
- Echo Request packets
- Echo Reply packets
- Successful communication with google.com

Skill Learned:
Basic packet capture and analysis
 
#ICMP Analysis

Filter used: icmp

Observed:
- Echo Requests sent to google.com IP
- Echo Replies received successfully

Learning:
ICMP is commonly used for network troubleshooting and host discovery.

#DNS Analysis

Filter used: dns

Observed:
- DNS query for google.com
- DNS server returned IPv4 and IPv6 addresses
- DNS uses UDP port 53

Learning:
DNS translates domain names into IP addresses before connections are made.

#HTTPS/TLS Analysis

Filter used: tls

Observed:
- TLS 1.3 Client Hello packets
- SNI fields showed github.com and neverssl.com

Learning:
HTTPS encrypts web traffic but metadata such as SNI and destination IP can still be analyzed.
 ## Nmap Reconnaissance

### Basic Scan
Command:
nmap 10.0.2.15

Purpose:
Identify open ports.

### Service Detection
Command:
nmap -sV 10.0.2.15

Purpose:
Identify service versions.

### OS Detection
Command:
sudo nmap -O 10.0.2.15

Purpose:
Identify operating system.

# Phase 3 - Host Discovery & Enumeration

Target: Ubuntu Server
IP Address: 10.0.2.15

Tools Used:
- ping
- Nmap

Commands:

ping 10.0.2.15

nmap -Pn 10.0.2.15

nmap -sV -Pn 10.0.2.15

Objective:
Discover live hosts and identify running services.

# Phase 3 - Nmap Enumeration

Target: Ubuntu Server
IP: 10.0.2.15

## Host Discovery

ping 10.0.2.15

Result:
Host reachable

## Service Enumeration

nmap -Pn -p 22 10.0.2.15

nmap -sV -Pn -p 22 10.0.2.15

Result:
Port 22 open
SSH service detected

## Learning

Ping uses ICMP.
Nmap identifies open ports and services.
SSH allows secure remote administration.

# SOC Home Lab - Phase 4: SSH Remote Access

## Objective
Establish secure remote access from Kali Linux to Ubuntu Server using SSH.

## Lab Environment

| Machine | Role | IP Address |
|----------|----------|----------|
| Kali Linux | Analyst/Client | 192.168.1.7 |
| Ubuntu Server | Target/Server | 192.168.1.2 |

## Tools Used

- Kali Linux
- Ubuntu Server
- OpenSSH Server
- VirtualBox

## Commands Executed

### Verify Ubuntu IP

```bash
ip a
```

### Verify SSH Service

```bash
sudo systemctl status ssh
```

### Verify Port 22

```bash
sudo ss -tuln | grep 22
```

### Test Connectivity

```bash
ping 192.168.1.2
```

### SSH Login

```bash
ssh vaibhav@192.168.1.2
```

## Findings

- SSH service was running successfully.
- Port 22 was listening.
- Kali Linux successfully connected to Ubuntu Server.
- Encrypted remote session established.

## Challenges

- Initial SSH connection returned "Connection Refused".
- VM networking was misconfigured.
- Resolved by placing both VMs on the same network and confirming SSH service status.

## Security Concepts Learned

- Secure Shell (SSH)
- Remote Administration
- Port 22 Enumeration
- Client-Server Communication
- Network Troubleshooting

## SOC Relevance

SSH is commonly used by SOC Analysts to:

- Access Linux servers remotely
- Investigate incidents
- Collect logs
- Perform system administration
- Conduct forensic analysis

## Outcome

✅ Successfully established SSH connectivity between Kali Linux and Ubuntu Server.

## Screenshots

1. Ubuntu IP Address
2. SSH Service Running
3. Port 22 Listening
4. Successful SSH Login

---

PHASE 5 – WEB SERVER ENUMERATION & MONITORING

Objective:
Install Apache web server on Ubuntu and monitor traffic from Kali.

Commands Used:

Ubuntu:
sudo apt update
sudo apt install apache2 -y
sudo systemctl status apache2
hostname -I

Kali:
nmap 192.168.1.2
nmap -sV 192.168.1.2

Wireshark Filter:
http

Observations:
- Apache web server installed successfully.
- Ubuntu hosted a webpage.
- Kali accessed the webpage.
- Nmap detected HTTP service.
- Wireshark captured HTTP GET requests.
- Server responded with HTTP 200 OK.

Learning:
- Web servers communicate using HTTP.
- Nmap can identify running services.
- Wireshark can capture web traffic.
- SOC analysts monitor HTTP activity to detect suspicious behavior.

PHASE 6 – AUTHENTICATION LOG MONITORING

Objective:
Monitor SSH authentication events in Ubuntu.

Commands:

sudo journalctl -f

ssh vaibhav@192.168.1.2

Observations:

- SSH login generated log entries.
- Source IP address was recorded.
- Username was recorded.
- Successful login displayed:
  "Accepted password for vaibhav"

Learning:

SOC analysts monitor authentication logs to detect:
- Successful logins
- Failed logins
- Unauthorized access attempts

PHASE 7 – WEB SERVER LOG MONITORING

Objective:
Monitor Apache web server access logs.

Commands:

sudo tail -f /var/log/apache2/access.log

Traffic Generation:

From Kali:
http://192.168.1.2

Observed Events:

GET / HTTP/1.1 200
GET /icons/ubuntu-logo.png 200
GET /favicon.ico 404

Learning:

200 = Request Successful
404 = File Not Found

SOC analysts monitor web logs to identify:

- User activity
- Suspicious requests
- Web attacks
- Unauthorized access attempts

Source IP observed:
192.168.1.7 (Kali Linux)

PHASE 8 – SSH FAILED LOGIN DETECTION

Objective:
Detect failed SSH login attempts.

Monitoring Command:

sudo journalctl -f

Attack Simulation:

From Kali:

ssh vaibhav@192.168.1.2

Entered incorrect password three times.

Observed Events:

Failed password for vaibhav
Failed password for vaibhav
Failed password for vaibhav
Connection closed by authenticating user

Source IP:
192.168.1.7

Target:
192.168.1.2

Learning:

SOC analysts monitor authentication logs to detect:

- Brute force attacks
- Password guessing attempts
- Unauthorized access attempts
- Insider threats

Result:
Successfully detected SSH authentication failures.

# Phase 9 – Wazuh SIEM Fundamentals

## Objective

Understand the architecture and workflow of the Wazuh Security Information and Event Management (SIEM) platform.

## Components Studied

### Wazuh Agent
Installed on endpoints to collect:

- Authentication logs
- System logs
- Security events
- File integrity data

### Wazuh Manager

Responsible for:

- Receiving agent data
- Processing events
- Correlating security alerts
- Applying detection rules

### Wazuh Dashboard

Used to:

- Visualize alerts
- Investigate incidents
- Monitor endpoints
- Review security events

## SIEM Workflow

Endpoint Activity
↓
Wazuh Agent
↓
Wazuh Manager
↓
Wazuh Dashboard
↓
SOC Analyst Investigation

## Events Identified for Monitoring

- Successful SSH logins
- Failed SSH logins
- Brute-force attempts
- Apache web requests
- System authentication events
- Network reconnaissance activity

## Learning Outcomes

- SIEM architecture
- Agent-to-manager communication
- Centralized log management
- Threat detection concepts
- Security event monitoring
- SOC analyst workflow

## SOC Relevance

Wazuh enables analysts to:

- Detect suspicious activity
- Monitor authentication events
- Investigate incidents
- Correlate security logs
- Improve security visibility

## Outcome

Successfully studied Wazuh architecture and its role within a Security Operations Center (SOC) environment.
