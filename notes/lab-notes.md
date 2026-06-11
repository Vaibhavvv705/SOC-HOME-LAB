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
