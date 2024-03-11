# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/Julian-1001/Azure-SOC/assets/162458677/2f9a9152-299f-4cbd-939d-baed5bb2e638)

## Introduction

In this project, I developed a small-scale honeynet within Azure and integrated log data from a variety of sources into a Log Analytics workspace. This setup enables Microsoft Sentinel to generate attack maps, activate alerts, and establish incidents. I monitored specific security metrics in a vulnerable environment for 24 hours, implemented several security enhancements to fortify the environment, and conducted another 24-hour metric assessment to compare the outcomes. Below, are the findings:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/Julian-1001/Azure-SOC/assets/162458677/78aac68f-d22d-437a-8992-9a6abf9cbca6)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/Julian-1001/Azure-SOC/assets/162458677/32557f4f-8cc4-47ca-aa36-bd9ad4665f0d)

The architecture of the small honeynet in Azure consists of the following:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the initial "BEFORE" assessment, resources were deployed with full exposure to the internet. Virtual Machines operated under fully open Network Security Groups and built-in firewalls, and all additional resources were set up with public endpoints, essentially bypassing the use of Private Endpoints.

In the "AFTER" evaluation, security was significantly tightened. Network Security Groups were configured to block all incoming and outgoing traffic, with the sole exception being connections from my administrative workstation. Furthermore, all resources were safeguarded through the implementation of their integrated firewalls alongside the deployment of Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/Julian-1001/Azure-SOC/assets/162458677/86e9b681-a19c-4948-91fc-960ad43a7abd)<br>
![Linux Syslog Auth Failures](https://github.com/Julian-1001/Azure-SOC/assets/162458677/dadb2321-2842-4420-9a47-17d2dd884124)<br>
![Windows RDP/SMB Auth Failures](https://github.com/Julian-1001/Azure-SOC/assets/162458677/02fed795-dce1-4e8e-a87f-94d3819c81b1)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-17 19:51:33
Stop Time 2024-02-18 19:51:33

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 48585
| Syslog                   | 2345
| SecurityAlert            | 5
| SecurityIncident         | 246
| AzureNetworkAnalytics_CL | 4432

## Attack Maps Before Hardening / Security Controls

```No instances of malicious activity were detected within the 24-hour period following the hardening process, resulting in all map queries yielding no results.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-02-29 02:49:36
Stop Time	2024-02-29 02:49:36

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9368
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

A small honeynet was set up within Microsoft Azure, and various log sources were incorporated into a Log Analytics workspace. Using Microsoft Sentinel, the system was configured to initiate alerts and record incidents based on the processed logs. An evaluation of security metrics took place in the initially unsecured environment, followed by a subsequent assessment after the introduction of security enhancements. A significant observation was the marked decrease in the number of security events and incidents post-implementation of these security measures, underscoring their efficacy.

It's important to highlight that if the network's resources experience normal use by regular users, it's likely that an increased volume of security events and alerts would have been observed within the 24-hour timeframe after applying the security controls.
