# ☁️ Azure-Cloud-SOC-Lab + Honeynet 🍯 

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



## Architecture Before Hardening / Security Controls
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

**To see a detailed process on following the Incident Response Process using Microsoft Sentinel covering NIST 800-61 (Incident Management Lifecycle) for this project, click here:** 

[Azure-Cloud-Honeynet-SOC-Lab-Incident-Response-Investigation](https://github.com/gervguerrero/Azure-Cloud-Honeynet-SOC-Lab-Incident-Response-Investigation)

</br>


## Architecture After Hardening / Security Controls
![Secured Network Azure firewall](https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/c9fea765-11bc-48af-a9ac-36a0e9c6b6d7)

During the Containment, Eradication and Recovery Phase of the incident lifecycle, I hardened the network by implementing security controls focused on NIST 800 53 R5 SC-7 Boundary Protection. 

- **Azure Firewall** - I configured and set an Azure Firewall to set only authorized IP addresses to remote into the Virtual Machines and only configured certain website traffic to be allowed. All other traffic is denied by default with Azure firewalls.  A default route and separate Azure subnet was created to route all internal traffic directly through the firewall, and all external traffic must be filtered through the firewall before going internal to the assets.

- **Virtual Network Network Service Group (NSG)** - I configured a Network Service Group spanning the entire virtual network to only accept inbound traffic from authorized IP addresses. 

- **Virtual Machine Network Service Group (NSG)** - I configured a Network Service Group specific to each Virtual Machine (Windows & Ubuntu) to only accept inbound traffic from authorized IP addresses. 

- **Operating System Firewalls** - I configured each VM’s respective Operating System Firewall to only accept inbound remote logins from authorized IP addresses.

   Windows 10: Windows Firewall 

   Ubuntu: UFW Firewall 

- **Private Endpoints** - I configured public access to disabled for Azure Key Vault and Blob Storage. Private endpoint access was configured to only allow access to these resources from within the virtual network, and deny access from public IP space.

**To see a detailed process on how I secured the network using 5 layers of boundary protection, click here:** 

[Azure-Cloud-SOC-Lab-Incident-Response-Investigation](https://github.com/gervguerrero/Azure-Cloud-Honeynet-SOC-Lab-Incident-Response-Investigation/edit/main/README.md)


</br>
</br>

## Attack Maps Before Hardening / Security Controls

<img width="720" alt="windowsrdp before" src="https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/0567b69e-8f38-4811-9815-07afa9a5e823">

This map above shows the attempts to login to the Windows 10 virtual machine through either RDP, SMB, or other native Windows login methods and where they originate from. (Measured by Windows Security Event Logs)



![image](https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/cd9e7f99-a016-48c7-9b1e-a1cb64a9f764)

This map above shows the attempts to login to the Ubuntu virtual machine through SSH and where they originate from. (Measured by Syslog Logs)



<img width="732" alt="SQL before" src="https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/0f82527b-f978-45b1-89ad-394dbca90c57">

This map above shows the attempts to login to the SQL database application in the Windows 10 virtual machine and where they originate from. (Measured by Failed logins to SQL in Application logs in Windows event logs.) 



<img width="727" alt="NSGmaliciousbefore" src="https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/cfc53ae3-ef64-4b4e-b00f-137043665f27">

This map above shows the malicious network traffic flow passing our Open Network Security Group Rules (NSG)  and where they originate from. (Measured by AzureNetworkAnalytics_CL logs.)

</br>

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```


</br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 2023-07-19 19:39:01 Z

Stop Time 2023-07-20 19:39:01 Z

| Metric                                                | Count
| ------------------------------------------------------| -----
| SecurityEvent (Windows VM)                            | 36291
| Syslog (Ubuntu VM)                                    | 16973
| SecurityAlert (Microsoft Defender for Cloud)          | 12
| SecurityIncident (Microsoft Sentinel Incidents)       | 365
| AzureNetworkAnalytics_CL (NSG Malicious Inbound Flow) | 2444


</br>


## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 2023-07-23 02:21:53 Z

Stop Time	2023-07-23 02:21:53 Z

| Metric                                                | Count
| ------------------------------------------------------| -----
| SecurityEvent (Windows VM)                            | 8019
| Syslog (Ubuntu VM)                                    | 8
| SecurityAlert (Microsoft Defender for Cloud)          | 0
| SecurityIncident (Microsoft Sentinel Incidents)       | 0
| AzureNetworkAnalytics_CL (NSG Malicious Inbound Flow) | 0


</br>

## Reduction of Overall Security Logs After Hardening / Security Controls 

| Metric                                                | Percentage Reduced
| ------------------------------------------------------| ------------------
| SecurityEvent (Windows VM)                            | -77.90%
| Syslog (Ubuntu VM)                                    | -99.95%
| SecurityAlert (Microsoft Defender for Cloud)          | -100.00%
| SecurityIncident (Microsoft Sentinel Incidents)       | -100.00%
| AzureNetworkAnalytics_CL (NSG Malicious Inbound Flow) | -100.00%

</br>


## Azure Secure Score Before Hardening / Security Controls
<img width="334" alt="securescore2" src="https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/205fac34-ad94-468e-a84e-c3a1ac5d4c80">

</br>

## Azure Secure Score After Hardening / Security Controls

<img width="346" alt="securescore1" src="https://github.com/gervguerrero/Azure-Cloud-SOC-Lab/assets/140366635/84637ed2-ce4f-410c-be29-0f29a96f0f0c">


</br>

## Conclusion

While learning how Microsoft's Azure Cloud models operate, I built a small Honey Net and SOC environment to capture the logs that were generated and integrated into a Log Analytics workspace. Using Microsoft Sentinel, incidents were triaged and investigated, and controls were implemented based on NIST 800-53 R5 SC-7. Log metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures.

Implementing security controls solely based on Boundary Protection (SC-7 of NIST 800-53) improved the overall network security posture by 27% according to Microsoft Defender for Cloud’s Secure score (54% increased to 81%). Our tables that show before and after security events with drastic reductions demonstrate how effective these network controls are in securing an environment.

It’s important to state that in a real life enterprise network environment with much more factors to consider, it’s likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

