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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
