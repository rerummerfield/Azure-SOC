 Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://imgur.com/bvhD3QV.png)

## Introduction

Within this project, I constructed a compact honeynet within Azure, capturing log data from diverse resources and consolidating it in a Log Analytics workspace. Microsoft Sentinel leveraged this data to construct attack maps, generate alerts, and establish incidents. Additionally, I measured select security metrics within the insecure environment over a 24-hour period, implemented security controls to fortify the environment, and reevaluated the metrics for another 24 hours. The ensuing results are presented below.

## Objective

The primary aim of this project was to establish deliberately vulnerable virtual machines within the Azure infrastructure with the purpose of attracting and examining cyber attacks. This endeavor enabled me to enhance my comprehension of attackers' tactics and techniques, while also demonstrating my adeptness in promptly and efficiently addressing any identified concerns.

## Technologies, regulations, and components utilized in Azure:

- Azure Virtual Network (VNet)

- Azure Network Security Group (NSG)

- Virtual Machines (2x Windows, 1x Linux)

- Log Analytics Workspace with Kusto Query Language (KQL) Queries

- Azure Key Vault for Secure Secrets Management

- Azure Storage Account for Data Storage

- Microsoft Sentinel for Security Information and Event Management (SIEM)

- Microsoft Defender for Cloud to Protect Cloud Resources

- Windows Remote Desktop for Remote Access

- Command Line Interface (CLI) for System Management

- PowerShell for Automation and Configuration Management

- NIST SP 800-53 Revision 4 for Security Controls

- NIST SP 800-61 Revision 2 for Incident Handling Guidance


## Methodology

- <b>*To initiate the creation of the honeynet*</b>: I proceeded by deploying numerous virtual machines within Azure, specifically designed to be vulnerable. This setup aimed to replicate an environment with known security weaknesses.

- <b>*For monitoring and analysis purposes*</b>: Azure was set up to receive log data from multiple sources, which were then consolidated in a log analytics workspace. Microsoft Sentinel was employed to construct attack maps, generate alerts, and establish incidents based on the accumulated information.

- <b>*To measure security metrics*</b>: I conducted a 24-hour observation of the environment, diligently capturing essential security metrics during its vulnerable state. This data served as a baseline for comparison, enabling assessment of the effectiveness of implemented remediation measures.

- <b>*Following the resolution of incidents and identification of vulnerabilities*</b>:I initiated the incident response and remediation phase. This involved fortifying the environment by implementing security best practices and adhering to Azure-specific recommendations, thereby enhancing its overall security posture.

- <b>*Conducting a post-remediation analysis*<</b>: I performed an additional 24-hour observation of the environment to reassess security metrics. By comparing the obtained results with the initial baseline, I gained insights into the effectiveness of the implemented remediation measures.


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://imgur.com/ikpsTRN.png)

- During the initial stage of the project, referred to as the "BEFORE" stage, all resources were initially deployed with public exposure to the internet deliberately. This insecure configuration was intentionally set up to attract potential cyber attackers and monitor their tactics. The Virtual Machines had both their Network Security Groups (NSGs) and built-in firewalls configured with unrestricted access from any source. Furthermore, other resources like storage accounts and databases were deployed with public endpoints visible to the internet, without leveraging Private Endpoints for enhanced security.

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://imgur.com/5S1hkXt.png)
<b>In the subsequent "AFTER" stage, I implemented a series of measures to harden the environment and enhance its overall security posture. These enhancements encompassed the following improvements:</b>
  
- To fortify the Network Security Groups (NSGs), I implemented stringent measures by blocking all inbound and outbound traffic, except for a single exception - my own public IP address. This approach ensured that only authorized traffic from a trusted source, namely my IP address, was permitted to access the virtual machines, thereby enhancing the security of the network infrastructure.

- To safeguard the resources from unauthorized connections, I configured the built-in firewalls on the virtual machines. This involved fine-tuning the firewall rules for each VM, aligning them with the specific requirements, and effectively restricting access. By carefully managing the firewall configurations, I minimized the potential attack surface and bolstered the overall security of the virtual machines.
  
- To heighten the security of various Azure resources, I substituted public endpoints with Private Endpoints. This strategic change restricted access to sensitive resources, including storage accounts and databases, exclusively within the virtual network, preventing exposure to the public internet. Consequently, these resources were shielded from unauthorized access and potential security breaches, bolstering their overall protection.
  
Through the comparison of security metrics before and after the implementation of these hardening measures and security controls, I successfully showcased the efficacy of each step in enhancing the overall security stance of the Azure environment.


## Attack Maps Before Hardening / Security Controls
> It is important to mention that the attack maps were created by extracting data from a workbook that made use of pre-built KQL .JSON map files. These files provided a structured representation of the attack patterns and their associated data, enabling the creation of visualizations that effectively illustrated the cyber threats and their impact on the system.

 <br />
 <br />
  
- <b>The presented attack map serves as a visual representation of the ramifications resulting from leaving the Network Security Group (NSG) open, which facilitated unrestricted passage of malicious traffic. This visualization effectively emphasizes the significance of implementing adequate security measures, such as restricting NSG rules, to thwart unauthorized access and mitigate potential threats.</b>
  
![NSG Allowed Inbound Malicious Flows](https://imgur.com/3XhOoDO.png)<br>

 <br />
 <br />
  
- <b>The depicted attack map brings attention to the significant number of syslog authentication failures encountered by the deployed Linux server, suggesting unauthorized access attempts originating from external sources. This serves as a poignant reminder regarding the criticality of securing Linux servers with robust authentication mechanisms and diligently monitoring system logs to detect and respond to any signs of intrusion attempts.</b>
  
![Linux Syslog Auth Failures](https://imgur.com/6FrJDsi.png)<br>
  
 <br />
 <br />
  
- <b>The presented attack map highlights a multitude of RDP and SMB failures, effectively illustrating the persistent efforts of potential attackers to exploit these protocols. This visualization underscores the importance of securing remote access and file sharing services to safeguard against unauthorized access and mitigate potential cyber threats.</b>
  
 ![Windows RDP/SMB Auth Failures](https://imgur.com/2neSyoq.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 4/18/2023, 6:36:53.581 PM
  
Stop Time 24/19/2023, 6:36:53.581 PM 

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 30792
| Syslog                   | 1677
| SecurityAlert            | 10
| SecurityIncident         | 552
| AzureNetworkAnalytics_CL | 345

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
  
Start 4/20/2023, 6:51:41.337 PM
  
Stop Time	4/21/2023, 6:51:41.337 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10878
| Syslog                   | 24
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

To summarize, I established a concise yet efficient honeynet using Microsoft Azure's robust cloud infrastructure. Microsoft Sentinel played a pivotal role by triggering alerts and generating incidents based on the ingested logs from the implemented watch lists. Baseline metrics were recorded in the initial unprotected environment prior to implementing any security controls. Subsequently, a series of security measures were implemented to bolster the network's resilience against potential threats.

Upon comparing the pre- and post-implementation metrics, a notable decrease in security events and incidents was observed, demonstrating the effectiveness of the implemented security controls. It is worth noting that if the network's resources were extensively utilized by regular users, it is plausible that a higher number of security events and alerts could have been generated within the 24-hour timeframe following the implementation of security controls.
