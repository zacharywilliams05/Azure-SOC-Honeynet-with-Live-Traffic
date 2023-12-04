# Building a SOC + Honeynet in Azure (Live Traffic)

![Vulnerable](https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/715fc3f6-198a-4a27-ba64-60ffc6b0c2fe)


## Introduction

In this project I would like to show how basic controls placed on a network can have a drastic reduction in risk compared to a network that is exposed. It may seem a forgone conclusion that a secure network is at less risk than one with a few controls, however the magnitude and the ease of instituting these controls may not be apparent. Many home networks as well as small and medium sized businesses may be at unnecessary risk.

We will build a small network with a windows virtual machine running a SQL database, a linux virtual machine, an Azure Key Vault, Azure storage account, and Microsoft Entry ID components. We will leave this network completely exposed to the internet for 24 hours. During this time log files tracking brute force password attempts, system changes, virus and malware, as well as changes to critical Azure subscription services will be tracked and plotted on maps using the Azure SIEM (Security Information and Event Management) Microsoft Sentinel. The log files we will track are:

- SecurityEvent (Windows Event Logs)</br>
- Syslog (Linux Event Logs)</br>
- SecurityAlert (Log Analytics Alerts Triggered)</br>
- SecurityIncident (Incidents created by Sentinel)</br>
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)</br>
## Architecture Before Hardening / Security Controls
![Vulnerable 2](https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/dcec216e-aca3-4b2b-89fd-522c4085a130)


## Architecture After Hardening / Security Controls
![Vulnerable 3](https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/bc12779e-97d7-4a82-b0cc-f355496f6991)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use of Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
<img width="800" alt="image" src="https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/a5f96e5e-d42d-486d-833a-7cff1c7f643f"></br>

<img width="800" alt="image" src="https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/1b2f80e5-ffbe-403a-8f08-e6c3950502a2"></br>

<img width="800" alt="image" src="https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/006dfd98-0498-49e4-b33c-f10723e05bec"></br>

<img width="800" alt="image" src="https://github.com/zacharywilliams05/Azure-SOC-Honeynet-with-Live-Traffic/assets/82168122/2f8e18dc-29ee-49dc-9920-f07c6e34584c">

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:</br>
Start Time 11/28/2023, 7:36:17</br>
Stop Time 11/29/2023, 7:36:17</br>


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12673
| Syslog                   | 3985
| SecurityAlert            | 6
| SecurityIncident         | 52
| AzureNetworkAnalytics_CL | 68674

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Analysis and Impact

The controls used to harden our environment were as follows:

windows-vm and linux-vm: Hardened the network security group to only allow my public IP address. This completely elimiinated the malicious traffic that was targetting the virtual machines.
SQL-server: The SQL server was secured by hardneing the virtual machines network security group and adding a network security group to the subnet itself. This completely eliminated the malicous traffic targetting the virutal machines.
Azure Key Vault: Enabled diagnostic settings and logging to trigger incidents when someone accesses, views, copies, changes secret key entries in the Azure Key Vault. Will alert and make auditing and investigating possible. 
Azure Storage Account: Enabled diagnostic settings and logging to trigger incidents when someone uploads, makes changes to, deletes files in storage account. Will alert and make auditing and investigating possible.
Azure Entra ID: Enabled diagnostic settings and logging to trigger incidents when someone makes changes at the tenent level. Will alert and make auditing and investigating possible.

These controls eliminated the alerts we saw before hardneing the environment. The controls where basic and novel in concept and implementation.


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness. The results of these security controls illistrates the important of adding simple network controls, but also that these controls need to be tweaked and configured for each system. Availabilty is an important part of the CIA triad and must be considred when implementing security controls. 

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
