# Azure-SOC
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud SOC Honeynet](https://github.com/CPamb/Azure-SOC/assets/149398071/3a2140e6-9673-4ab0-9704-b5ab44a95531)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/CPamb/Azure-SOC/assets/149398071/db6e23ad-fae7-43c1-8fd4-5a98f0eb5f5b)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/CPamb/Azure-SOC/assets/149398071/807dd578-7c95-40e6-b598-556bb94e6ad9)


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

NSG Allowrd Inbound Malicious Flows
![NSG Allowrd Inbound Malicious Flows](https://github.com/CPamb/Azure-SOC/assets/149398071/a18b2c91-a524-4e1e-8464-a5e3a8d2f387)<br>

Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://github.com/CPamb/Azure-SOC/assets/149398071/854b06af-6d28-4671-8619-1ce37991586f)<br>

Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://github.com/CPamb/Azure-SOC/assets/149398071/cd5c17ae-72d7-4ba9-8344-c975c2a26bb2)<br>

MSSQL Auth Failure
![MSSQL Auth Failure](https://github.com/CPamb/Azure-SOC/assets/149398071/333136be-7155-4b54-af19-75d336e9ecc5)


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
