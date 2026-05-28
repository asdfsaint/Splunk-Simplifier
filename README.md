# Splunk-Simplifier
Splunk catches everything but is unreadble if you aren't a security analyst, this lab fixes that.

Windows 11 VM sends logs to Splunk. A Python script pulls those alerts on a schedule and sends them to a local LLM (llama.cpp  + TBD) that rewrites them in plain English. No cloud, no third-party AI  everything runs on one Proxmox server.

Tested by attacking the Windows VM from Kali Linux and checking if the summary actually makes sense to someone non-technical.

Stack: Proxmox · Windows 11 · Ubuntu Server · Splunk · Python · LLama.cpp
