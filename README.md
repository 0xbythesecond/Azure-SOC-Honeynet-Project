<!--# Azure-SOC-Honeynet-Project-->
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/KGOvcdw.gif)

## Introduction

In this project, I build a mini HoneyNet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/gBvHJo4.gif)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/oQtbais.gif)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
The below image reflects exposed Microsoft SQL server over a 24 hour period and where the attacks were located
![MSSQL Allowed Access](https://i.imgur.com/UHVHIGM.png) <br />
The below image reflects exposed Linux server over a 24 hour period and where the attacks were located
![Linux Syslog Auth Failures](https://i.imgur.com/8QbjEwL.png) <br />
The below image reflects exposed Windows Virtual Machine over a 24 hour period and where the attacks were located
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ALHFE3u.png) <br />
The below image reflects exposed Network Security Group(NSG) over a 24 hour period that is wide open on all ports while capable of accepting all traffic and where the attacks originated from.
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/W2iCXmv.png)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
<br />
`Start Time:` 2023-05-10T20:19:56 <br/>
`Stop Time:` 2023-05-11T20:19:56

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 49067
| Syslog                   | 3420
| SecurityAlert            | 6
| SecurityIncident         | 287
| AzureNetworkAnalytics_CL | 526

## Attack Maps Before Hardening / Security Controls

  >**Note**: All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
<br />
`Start Time:` 2023-05-11T23:44:13.<br />
`Stop Time:`	2023-05-12T23:44:13.

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 960
| Syslog                   | 23
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
