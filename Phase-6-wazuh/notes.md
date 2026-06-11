# Phase 6 - Wazuh Preparation

## Objective
Prepare Ubuntu Server for Wazuh Agent installation.

## Commands

sudo apt update
sudo apt upgrade -y
sudo apt install curl -y
hostnamectl
ip a

## Learning

- Wazuh is an open-source SIEM.
- Agents collect logs from endpoints.
- The manager receives and analyzes security events.
- Ubuntu will act as a monitored endpoint.
