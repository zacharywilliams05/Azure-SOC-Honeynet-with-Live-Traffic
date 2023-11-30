# Building a SOC + Honeynet in Azure (Live Traffic)

![Vulnerable](https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/715fc3f6-198a-4a27-ba64-60ffc6b0c2fe)


## Introduction

In this project I would like to show how basic controls placed on a network can have a drastic reduction in risk compared to a network that is exposed. It may seem a forgone conclusion that a secure network is at less risk than one with a few controls, however the magnitude and the ease of instituting these controls may not be apparent. Many home networks as well as small and medium sized businesses may be at unnecessary risk.

We will build a small network with a windows virtual machine running a SQL database, a linux virtual machine, an Azure Key Vault, Azure storage account, and Microsoft Entry ID components. We will leave this network completely exposed to the internet for 24 hours. During this time log files tracking brute force password attempts, system changes, virus and malware, as well as changes to critical Azure subscription services will be tracked and plotted on maps using the Azure SIEM (Security Information and Event Management) Microsoft Sentinel. The log files we will track are:

SecurityEvent (Windows Event Logs)</br>
Syslog (Linux Event Logs)</br>
SecurityAlert (Log Analytics Alerts Triggered)</br>
SecurityIncident (Incidents created by Sentinel)</br>
AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)</br>
## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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
