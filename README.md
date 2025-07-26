# Building a SOC + Honeynet in Azure (Live Traffic)

<img width="683" height="410" alt="Screenshot 2025-07-26 at 1 07 09 AM" src="https://github.com/user-attachments/assets/7b4789ec-2e6b-49e9-9f38-fe5a0532f989" />

## Introduction

For this project, I built a mini honeynet in Azure and connected log sources from different resources into a Log Analytics Workspace. From there, Microsoft Sentinel helped me generate attack maps, set up alerts, and track incidents.

To test the effectiveness of security controls, I first left the environment unsecured and collected metrics for 24 hours. After that, I hardened the environment with basic security measures and monitored it for another 24 hours.

Here are the metrics I gathered before and after applying those security controls:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- NTANetAnalytics  (Malicious Flows allowed into our honeynet)

## Technologies and Regulations Used

- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (Windows and Linux)
- Log Analytics Workplace
- Kusto Query Language (Queries)
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Manangement
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) for Security Controls
- [NIST SP 800-61 Revision 2 ](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guide


## Architecture Before Hardening / Security Controls
<img width="690" height="435" alt="Screenshot 2025-07-26 at 1 33 31 AM" src="https://github.com/user-attachments/assets/35074486-43c4-46a9-808f-a9430d7c3b59" />

In the initial stage of the project, I set up a virtual environment and intentionally left it exposed to the internet to see how attackers would interact with it. The goal was to attract malicious traffic and study their behavior. I deployed a Windows VM running a SQL database and a Linux server, both with wide-open NSGs (Allow All). To make the environment even more tempting, I added a storage account and key vault with public endpoints visible to anyone online. Microsoft Sentinel was used to monitor the environment, pulling in logs through the Log Analytics workspace.

## Architecture After Hardening / Security Controls
<img width="795" height="484" alt="Screenshot 2025-07-26 at 1 50 54 AM" src="https://github.com/user-attachments/assets/115c9f3e-564f-4cd9-a388-0e6c4c88c4a7" />


During the post-deployment phase of the project, the environment was hardened and key security controls were implemented to comply with NIST SP 800-53 Rev. 4 SC-7(3) – Access Points. The following measures were taken to strengthen the infrastructure:
- Network Security Groups (NSGs): NSGs were configured to block all inbound and outbound traffic by default, allowing exceptions only for specific public IP addresses that required access to the virtual machines. This approach ensured that only authorized and trusted sources could communicate with the environment.
- Built-in Firewalls: Azure’s native firewalls were enabled and fine-tuned on each virtual machine. Firewall rules were customized according to each VM’s role and responsibilities, significantly reducing the attack surface and minimizing the risk of unauthorized access.
- Private Endpoints: Public endpoints for Azure Key Vault and Storage Accounts were replaced with private endpoints. This restricted access to these critical resources exclusively to the internal virtual network, eliminating exposure to the public internet and enhancing data security.

## Attack Maps Before Hardening / Security Controls
<img width="1217" height="663" alt="BF NSG LOGS" src="https://github.com/user-attachments/assets/afc551c9-2aff-4059-836e-033726eada2f" />
<img width="1174" height="641" alt="BF LINUX LOGS" src="https://github.com/user-attachments/assets/3ad34b3e-5fcd-4bfb-8bfe-adc8ad0ee1c6" />
<img width="1063" height="643" alt="BF WINDOWS LOGS" src="https://github.com/user-attachments/assets/d4c3a943-f849-490f-8b1a-38a7058ac1b3" />


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2025-07-11 16:06:22
Stop Time 2025-07-12 16:06:22

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 22014
| Syslog                   | 20022
| SecurityAlert            | 16
| SecurityIncident         | 176
| NTANetAnalytics          | 392

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2025-07-13 16:15:28
Stop Time	2025-07-14 16:15:28

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9583
| Syslog                   | 184
| SecurityAlert            | 0
| SecurityIncident         | 0
| NTANetAnalytics          | 0

## Conclusion

In this project, I built a mini honeynet in Microsoft Azure and connected various log sources to a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and generate incidents based on the incoming data. I collected metrics from the environment while it was unsecured, then again after applying key security controls. The results showed a significant drop in security events and incidents once those controls were in place, proving their impact.

It’s also worth mentioning that if this environment had real user activity, the number of events and alerts after securing it might have been higher, simply due to normal usage triggering more security checks.
