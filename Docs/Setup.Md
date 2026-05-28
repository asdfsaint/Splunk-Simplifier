#Setup notes
I made an isolated proxmox network topology with vmbr0,vmbr1 and 2 with 0 being my network and vmbr1 and 2 being my isolated networks where I will be adding my Ubuntu 24.04 lts server for splunk and Windows 11 Virtual machine with sysmon.
Splunk and the windows 11 machine only talk to eachother on the Vmbr1 linux bridge and vmbr2 will be used for my kali linux attack machine which is on my personal laptop and windows to talk to eachother only.
I've updated my ubuntu machine and configured my network configurations and installed splunk. Ive configured UFW to deny everything coming in and out besides port 8000 and 22 from only inside the home network and with a complex 20 character password and enabling HTTPS on the splunk web UI.
Splunk starts on boot, I will be adding an LLM thru llama.cpp most likely a 3-4b model because of hardware limitations

-- Network --
Vmbr0 = Home network
vmbr1 = Windows 11 and Ubuntu VM
vmbr2 = Kali linux vm and Windows 11 vm
Splunk IP = 192.168.68.53 port 8000
Splunk version: 10.4.0
 
