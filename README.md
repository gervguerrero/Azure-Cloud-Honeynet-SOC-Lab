# Azure-Cloud-SOC-Lab + Honeynet 

![final map](https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/cd438962-f370-40f5-9a5d-2a8034c579de)


## Introduction/Goals

This project involves creating an exposed Honeynet in the Microsoft Azure Cloud to observe how real cyber threat actors attempt to break into vulnerable networks on a continuous basis. Upon observation of the attacks, we will practice the incident response process with NIST 800-61 and implement security controls with NIST 800-53 by using the security tools available in Microsoft Azure. The end goal is to effectively emulate a security incident response action plan from a SOC perspective in a cloud environment.  


## Observation Methods

The log analytics workspace imports a wide variety of security logs from our assets in  Azure. Any anomalies or security events are logged from our endpoints and imported into our log analytical workspace for querying.

The Azure Honeynet was unprotected for 24 hours to observe the cyber attacks in certain metrics with the logs ingested. Security controls were then implemented to harden the environment, and after another 24 hours the logs were measured again. The metrics measured by logs are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

Microsoft Sentinel will utilize these logs ingested in the analytical workspace to construct a heat map from the geographical sources of the attacks, create alerts, and advertise incidents to our defenders.


## Tools & Technologies Used 

**Virtual Machines**
- Windows 10 (Attack VM) 
- Windows 10 with SQL Database (Victim VM) 
- Ubuntu 20.04.6 (Victim VM)

**Key Vaults**

**Storage Accounts**

**Azure Active Directory**

**Log Analytics Workspace**

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet) 




**Microsoft Sentinel**

**Microsoft Defender for Cloud**

**Virtual Networks**

**Network Security Groups**

**Network Watcher**

**Azure Firewalls**

**Route Tables**

## Architecture Before Hardening & Security Controls
![Unsecured Network](https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/768fa3df-0c0e-4062-8369-3223d4336e0b)

To allow attacks into the network for observation, the azure virtual network was left unprotected at multiple levels.  All Azure assets were public facing to the internet and were available directly to cyber threat actors. 

Our Assets include:
- Windows 10 Server hosting SQL Database
- Ubuntu  20.04.6 Server
- Azure Key Vault
- Azure Blob Storage (Storage Account)

No Azure firewall was configured for the network, a Network Service Group (NSG) covering the network with our assets was left open to any traffic, both the Windows & Ubuntu virtual machines had NSGs open to any traffic, and lastly the respective firewalls for each asset were disabled. The operating system firewalls for Windows and Ubuntu were not configured, and the network rule settings for our Blob Storage and Key Vault were available to be viewed from any public space, without the security tool Azure Private Endpoints configured.

**Threat:**  Malicious Cyber Actors Online

**Vulnerability:**  No Firewalls or Networks Service Groups configured 

**Risk:**  Remote Login available to threat actors brute forcing into publicly exposed Virtual Machines, and public access/viewing of our Blob Storage and Key Vault.  

To see a detailed process on following the Incident Response Process using Microsoft Sentinel covering NIST 800-61 (Incident Management Lifecycle) for this project, Click Here (PLACEHOLDER)

## Architecture After Hardening / Security Controls
![Secured Network Asset Firewall](https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/80e529bd-5cad-45c4-80c6-2d5eb3002fef)


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

![147203-dark-simple-background-texture-digital-art-blue-green-abstract](https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/c140ff7f-c7e9-472f-b78c-4c69aef191de)
