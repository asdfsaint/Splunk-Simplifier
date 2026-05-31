# Setup Notes

## Network Topology
I made an isolated Proxmox network topology with vmbr0, vmbr1, and vmbr2. vmbr0 is my home network. vmbr1 is the isolated attack lab network (Windows 11 + Ubuntu Splunk server). vmbr2 is reserved for Kali Linux.

## Ubuntu Splunk Server
Updated Ubuntu 24.04 LTS, configured network, installed Splunk. UFW configured to deny all inbound/outbound except port 8000 and 22 from inside the home network only. Complex 20-character password set, HTTPS enabled on Splunk web UI. Splunk must be started as root.

- IP: 192.168.10.10 (ens18)
- Username: splunk
- Splunk version: 10.4.0
- Start command: sudo /opt/splunk/bin/splunk start

## Windows 11 VM (2026-05-30)
- OS: Windows 11 25H2
- Network: vmbr1 (isolated)
- VirtIO drivers installed
- Local account (no Microsoft account)
- Sysmon downloaded, pending install
- Splunk Universal Forwarder pending install → 192.168.10.10:9997

## Network Summary
- vmbr0 = Home network
- vmbr1 = Windows 11 VM + Ubuntu Splunk server (isolated)
- vmbr2 = Kali Linux VM

## Pending
- Start Splunk: sudo /opt/splunk/bin/splunk start
- Enable receiver: splunk enable listen 9997
- Install Sysmon on Windows 11
- Install and configure Splunk Universal Forwarder pointing to 192.168.10.10:9997
- Verify Sysmon events appear in Splunk at 192.168.10.10:8000
