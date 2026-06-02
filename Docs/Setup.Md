# Setup Notes

## Status

- The network and isolation are built and tested. They work.
- The Splunk pipeline is not working yet. No logs are flowing and no detections exist.
- Anything marked in progress below is not done. I have not faked any results.

## Network Topology

I built an isolated lab on my Proxmox server (ramrez-core) using two bridges.

- vmbr0 is my home network, 192.168.68.0/22.
- vmbr1 is the isolated lab network, 192.168.10.0/24, with no gateway. The Windows 11 victim, the Ubuntu Splunk server, and the Kali attack box all live on vmbr1.

I deleted vmbr2. My old notes had Kali on vmbr2, but an attacker has to be on the same subnet as the target to reach it, so Kali is on vmbr1 with the victim now.

This is the Proxmox host with the lab VMs.

![Proxmox node summary](../images/proxmox-node-summary.png)

These are the bridges. vmbr0 has the home IP and gateway. vmbr1 has no gateway.

![Proxmox network bridges](../images/proxmox-network-bridges.png)

## Ubuntu Splunk Server

Ubuntu 24.04 LTS running Splunk 10.4.0. The server is dual homed. One leg is on the lab and one is on my home network, so I can open the dashboard from my desktop while the lab stays sealed off.

- Lab IP: 192.168.10.10 (ens18)
- Home IP: 192.168.68.157 (ens19)
- Username: splunk
- Splunk web UI: http://192.168.68.157:8000 (home side only)
- Start command: sudo /opt/splunk/bin/splunk start (it has to run as root)
- 20 character password set, HTTPS enabled on the web UI

UFW is set to deny all inbound and outbound by default. I only open what the lab actually needs.

- ens18 (lab side): only port 9997 inbound, the forwarder receiver. Nothing else.
- ens19 (home side): ports 8000, 22, and 8089 inbound, from my home network only.
- Outbound: only DNS (53) and NTP (123).

I also set net.ipv4.ip_forward to 0 so a compromised host cannot route through the server into my home network.

Here are both NICs in Proxmox. net0 is on vmbr1 (lab) and net1 is on vmbr0 (home).

![Ubuntu server VM hardware](../images/ubuntu-vm-hardware.png)

Here is ip a, ip_forward set to 0, and the UFW rules on the server.

![Ubuntu ip a, ip_forward, and ufw status](../images/ubuntu-ip-and-ufw.png)

## Windows 11 VM

- OS: Windows 11 25H2
- Network: vmbr1, 192.168.10.20, no gateway (isolated)
- VirtIO drivers installed
- Local account, no Microsoft account
- Sysmon and the Universal Forwarder were set up earlier, but I have not verified they are running or forwarding yet, so I am treating them as not confirmed. The forwarder should point to 192.168.10.10:9997.

Here is the VM in Proxmox with its single NIC on vmbr1.

![Windows 11 VM hardware](../images/windows-vm-hardware.png)

## Kali Linux VM

- OS: Kali 2026.1
- Network: vmbr1, 192.168.10.30, static, no gateway
- I verified the ISO SHA256 before installing
- This is my attack box, on the same subnet as the victim

Here is ip a on Kali showing 192.168.10.30.

![Kali ip a](../images/kali-ip-a.png)

## Isolation Testing

I tested the isolation from both sides.

From the Windows victim: it can reach the Splunk server at 192.168.10.10 but cannot reach my home network. Pinging 192.168.68.157 and the home gateway both fail with transmit failed, no route out.

![Windows ping showing transmit failed](../images/windows-ping-isolation.png)

From Kali: I ran a full nmap of the Splunk server. Ports 22, 8000, and 8089 came back filtered, so the firewall is dropping them. Out of all 65535 ports, only 9997 was reachable. From the attacker side the only thing on the dirty network is the log receiver.

I also confirmed Kali can see the victim at layer 2, since the victim MAC showed up in the ARP table. Ping to the victim fails only because Windows Firewall blocks ICMP by default, which is normal.

Note: I do not have screenshots saved for the Kali nmap scan or the Kali ARP table yet. I will add them when I re-run the scan after Splunk is started.

## Pending

These are not done. They are in progress.

- Start Splunk. It is not running yet, which is why 9997 showed closed instead of open: sudo /opt/splunk/bin/splunk start
- Enable the receiver, then re-scan and 9997 should show open: splunk enable listen 9997
- Confirm or install Sysmon on Windows 11 with the SwiftOnSecurity config
- Confirm or install the Universal Forwarder, set inputs to the Sysmon log and outputs to 192.168.10.10:9997
- Create a sysmon index and install the Splunk Add-on for Sysmon on the server
- Verify Sysmon events show up in Splunk at http://192.168.68.157:8000
- Build the detection layer (MITRE, Sigma) and the LLM reporting tool later
