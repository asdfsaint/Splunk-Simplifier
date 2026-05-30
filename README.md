# Splunk-Simplifier
Splunk catches everything but is unreadble if you aren't a security analyst, this lab fixes that.

Windows 11 VM sends logs to Splunk. A Python script pulls those alerts on a schedule and sends them to a local LLM (llama.cpp  + Phi-3.5 Mini,Q4_K_M) that rewrites them in plain English. No cloud, no third-party AI  everything runs on one Proxmox server.

Tested by attacking the Windows VM from Kali Linux and checking if the summary actually makes sense to someone non-technical. 

The pipeline is Windows → Splunk → Python → llama.cpp → file output.

Stack: Proxmox · Windows 11 · Ubuntu Server · Splunk · Python · LLama.cpp . Splunk Universal Forwarder
