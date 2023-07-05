<!--# Azure-SOC-Honeynet-Project-->
# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC Honeynet (1)](https://github.com/0xbythesecond/Azure-SOC-Honeynet-Project/assets/23303634/43177fa9-4746-4f8d-8774-f9aca74b891d)

## Introduction
I present a summary of varying parts to create a HoneyNet via Microsoft Azure. This HoneyNet is to provide a visual representation of real-world cyber attacks from all parts of the world. The HoneyNet is designed to allow me to gather data related to the different bad actors from across the world from differing IP addresses.

## Sub-Intro
In this project, I build a mini HoneyNet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measured metrics for another 24 hours, then show the results below. 


## Azure Resources Deployed, Technologies, and Regulations used:
- [Azure Virtual Network](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) (VNet)
- [Azure Network Security Group](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview) (NSG)
- [Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/overview) (2x Windows 10 Pro, 1x Linux Server)
- [Log Analytics Workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-workspace-overview) with Kusto Query Language (KQL) Queries
- [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/basic-concepts) for Secure Secrets Management
- [Azure Storage Account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview) for Data Storage
- [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview) for Security Information and Event Management (SIEM)
- [Microsoft Defender](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction) for Cloud to Protect Cloud Resources
- [Windows Remote Desktop](https://support.microsoft.com/en-us/windows/how-to-use-remote-desktop-5fe128d5-8fb1-7a23-3b8a-41e636865e8c) for Remote Access
- [Command Line Interface](https://www.w3schools.com/whatis/whatis_cli.asp) (CLI) for System Management
- [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.3) for Automation and Configuration Management
- [NIST SP 800-53 Revision 4](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance

## Course of Action
- ***Establishing the honeynet:*** To start, I created the vulnerable environment with the Virtual Machines. This was done by disabling the firewall inside of the VM as well as allowing all ports and traffic to be received by the Network Security Group (NSG).

- ***Tracking and examination:*** The Azure infrastructure was meticulously configured to seamlessly ingest log sources from a multitude of resources into a dedicated log analytics workspace. Leveraging the advanced capabilities of Microsoft Sentinel, sophisticated attack maps were meticulously constructed, meticulously triggering highly precise alerts and meticulously generating comprehensive incidents, all meticulously derived from the meticulously collected and meticulously analyzed data.

- ***Tracking and evaluating security metrics:*** I monitored the unsecured environment for a full day, noting important security measurements during that time. This served as a starting point for comparison once I applied security improvements.

- ***Addressing and resolving security incidents:*** Following the resolution of incidents and identification of vulnerabilities, I proceeded to fortify the environment by implementing security best practices and incorporating Azure-specific recommendations.

- ***Analysis after implementing remediation measures:*** An additional 24-hour period was dedicated to the meticulous re-observation of the environment, facilitating a comprehensive evaluation of the security metrics. The resulting data was then meticulously juxtaposed with the initial baseline, enabling a rigorous comparative analysis.

The metrics we will show are:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/gBvHJo4.gif)
In the "BEFORE" measurement phase, it was observed that all resources were initially provisioned with direct internet exposure. The Virtual Machines were configured with open Network Security Groups and permissive built-in firewalls, while other resources were deployed with publicly accessible endpoints, thereby rendering the usage of Private Endpoints unnecessary.

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/oQtbais.gif)
In the "AFTER" evaluation stage, the Network Security Groups underwent fortification measures whereby all traffic, with the exception of my administrative workstation, was comprehensively blocked. Additionally, other resources were fortified by leveraging their built-in firewalls alongside the implementation of Private Endpoint functionality.

## Attack Maps Before Hardening / Security Controls
The visual representation presented below provides an overview of the assault endeavors targeted at a publicly accessible Microsoft SQL server throughout a span of 24 hours. The plotted data points on the map delineate the precise origins of these attacks or attempted logins.
![MSSQL Allowed Access](https://i.imgur.com/UHVHIGM.png) <br />

The depicted attack map elucidates the multitude of syslog authentication failures encountered by the Linux server I provisioned, elucidating the presence of unsanctioned endeavors to gain entry from external sources beyond the confines of the local network. This serves as an emphatic reminder underscoring the indispensability of fortifying Linux servers with robust authentication protocols and diligently scrutinizing system logs to detect and thwart potential intrusions.
![Linux Syslog Auth Failures](https://i.imgur.com/8QbjEwL.png) <br />

The exhibited attack map encapsulates a multitude of RDP (Remote Desktop Protocol) and SMB (Server Message Block) failures, vividly exemplifying the unrelenting endeavors of potential assailants to exploit these specific protocols. The visual depiction accentuates the imperative nature of fortifying remote access and file-sharing services as a means to safeguard against illicit entry and mitigate the looming cyber threats that may ensue.
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ALHFE3u.png) <br />

The illustrated attack map serves as a compelling showcase of the ramifications stemming from the act of leaving the Network Security Group (NSG) unrestricted, thereby facilitating the unhindered ingress of malicious network traffic. This visualization effectively emphasizes the criticality of deploying robust security protocols, including the imposition of stringent NSG rules, as a means to thwart unauthorized entry and mitigate the inherent risks posed by potential threats.
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

## Attack Maps After Hardening / Security Controls

  >**Note**: All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
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

## Reflection
This has been both a challenging and rewarding experience creating this lab and how real-world traffic can be analyzed using attack maps as well as KQL data to parse out different metrics. It was a beautiful sight to see everything come together and have the ability to paint a picture of an insecure environment as well as one that is secure and you no longer see the malicious traffic after implementing the various security controls. During the process of leaving the resources vulnerable, I was able to see the differing IP addresses of the bad actors and the user names that they were attempting to access my virtual machines. After the hardening was completed and waiting 24 hours, it was quite a sight to behold when seeing that there were 0 results found that represent any allowed traffic from the bad actors on the public internet.

## Conclusion
This project involved the establishment of a mini honeynet within the Microsoft Azure platform, where diverse log sources were seamlessly integrated into a dedicated Log Analytics workspace. Microsoft Sentinel played a pivotal role in proactively generating alerts and initiating incidents based on the logs ingested. Notably, comprehensive metrics were diligently measured in the vulnerable environment prior to the implementation of security controls, followed by a subsequent assessment after fortifying the infrastructure. The remarkable outcome emerged as a significant reduction in the frequency of security events and incidents, which undeniably attested to the efficacy of the implemented security measures.

It is important to acknowledge that if the network's resources were extensively utilized by regular users, it is conceivable that a greater number of security events and alerts could have been generated within the 24-hour timeframe subsequent to the enforcement of the security controls.
