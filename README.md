# Splunk-Simplifier

Splunk catches everything but is unreadable if you aren't a security analyst, this lab fixes that.

The goal is a pipeline where a Windows 11 VM sends logs to Splunk, a Python script pulls those alerts on a schedule, and a local LLM (llama.cpp + Phi-3.5 Mini, Q4_K_M) rewrites them in plain English. No cloud, no third-party AI. Everything runs on one Proxmox server.

The plan is to attack the Windows VM from Kali Linux and check if the summary actually makes sense to someone non-technical.

The target pipeline is Kali Linux to attacks to Windows 11 (Sysmon) to Splunk Universal Forwarder to Ubuntu Splunk to llama.cpp to a readable report.

## Status

- The isolated network and the isolation testing are built and working.
- The Splunk pipeline is not working yet. Logs are not flowing and there are no detections yet.
- See Docs/Setup.Md for the current state and what is still in progress.

Stack: Proxmox, Windows 11, Ubuntu Server, Splunk, Sysmon, Python, llama.cpp, Splunk Universal Forwarder
