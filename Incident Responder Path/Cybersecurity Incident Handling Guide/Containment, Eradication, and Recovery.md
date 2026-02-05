# Containment, Eradication, and Recovery

The **Containment, Eradication, and Recovery** step includes isolating the threat, cleaning the indicators and persistence methods used in the attack, and restoring affected systems to normal operation. This phase is critical because it stops the attacker's access, removes malicious artifacts, and returns the organization to a secure operational state.

## Containment

The **containment step** is where the attacker is isolated so that they cannot cause any further damage or continue their malicious activities. Since delaying the containment step is risky and allows the attacker more time to expand their foothold, exfiltrate data, or cause destruction, it should be carried out **as soon as the event is detected and confirmed as a cybersecurity incident**.

Containment is typically divided into two phases:

- **Short-term containment**: Immediate actions taken to limit the spread and impact of the incident while preserving evidence and maintaining some level of business continuity.
- **Long-term containment**: Sustainable measures that allow the organization to continue operating while preparing for eradication and recovery (e.g., applying temporary patches, implementing compensating controls, segmenting networks).

### Containment methods

Containment methods may vary depending on the type of cybersecurity incident, the criticality of affected systems, and the organization's operational requirements. Some common containment methods include:

- **Placing the device in an isolated network segment**: Move the compromised system to a quarantine VLAN or isolated network zone where it cannot communicate with production systems but can still be analyzed.
- **Stopping the affected services**: Shut down specific services or processes that are being exploited or abused by the attacker (e.g., stopping a vulnerable web service, disabling SMB, halting a compromised application).
- **Turning off the device**: Power down the system to immediately halt attacker activity. Note that this results in loss of volatile data (memory), so consider capturing memory before shutdown if forensic analysis is needed.
- **Disconnecting the device from the network**: Physically unplug the network cable or disable network interfaces to prevent further communication. This is a quick containment action but also results in loss of network-based monitoring.
- **Disabling the user's account on the corporate network**: Disable or reset the password for compromised user accounts to prevent the attacker from using stolen credentials for lateral movement or persistence.
- **Blocking malicious IPs, domains, or URLs**: Update firewall, proxy, and DNS filtering rules to block command-and-control (C2) infrastructure and prevent further communication.
- **Revoking certificates or tokens**: Invalidate compromised credentials, API keys, certificates, or session tokens.

### EDR containment features

Modern **Endpoint Detection and Response (EDR)** products come with built-in containment features. With this functionality, the device on which the agent is installed is **separated from the network** and communicates only with the EDR central management server. In this way, while performing live forensic analysis and collecting evidence on the device, the isolation of the device is also ensured, preventing the attacker from using it as a pivot point or exfiltrating data through it.

### Containment strategy

You should create a **complete containment strategy** during the **Preparation** phase. This strategy should define:

- Criteria for triggering containment actions
- Approved containment methods for different types of incidents and systems
- Roles and responsibilities for executing containment
- Communication and approval workflows (especially for critical systems)
- Procedures for preserving evidence during containment

Since you cannot isolate critical servers directly from the network without causing significant business disruption, it is **highly recommended to prepare a separate containment procedure for critical applications and servers**. This procedure may include:

- Implementing compensating controls (e.g., enhanced monitoring, firewall rules, access restrictions) instead of full isolation
- Coordinating with business units to plan maintenance windows for containment actions
- Preparing backup systems or failover mechanisms to maintain service availability during containment

## Eradication

The **eradication step** includes activities such as cleaning the malicious software left by the attacker, disabling compromised user accounts, deleting accounts created by the attacker, and removing any persistence mechanisms or backdoors installed during the attack.

### Preserving evidence before eradication

The most important point that should not be forgotten in this step is that **indicators belonging to the attack must be recorded as evidence before they are deleted**. This is critical for forensic analysis, understanding the full scope of the attack, attribution, and potential legal or regulatory requirements.

For example, before deleting malicious software used in the attack from a server, you should:

1. **Take a screenshot** of the folder where the malware is located, showing the file name, path, and timestamps.
2. **Save the hash information** of the malware (MD5, SHA1, SHA256) to uniquely identify the file and search for it on other systems or in threat intelligence databases.
3. **Record a copy of the malware** in an isolated environment (secure storage, forensic workstation, or malware repository) for further analysis and reference.
4. **Document the context**: Where was the file located? What process created or executed it? What user or account was involved?

Proper evidence preservation ensures that if the attacker returns or if legal action is needed, you have the necessary artifacts to support your response and investigation.

### Activities in the eradication step

Activities to be performed during eradication can be listed as:

- **Cleaning files uploaded by the attacker**: Remove malicious executables, scripts, web shells, and other files dropped by the attacker.
- **Disabling or deleting compromised user accounts**: Disable accounts whose credentials were stolen or misused, and delete accounts created by the attacker for persistence.
- **Deleting users created by the attacker**: Remove unauthorized accounts, service accounts, or backdoor accounts added to Active Directory, local systems, or applications.
- **Mitigating identified vulnerabilities**: Apply patches, update software, fix misconfigurations, and close security gaps that were exploited during the attack.
- **Terminating actively running malicious processes**: Kill processes associated with malware, backdoors, or attacker tools.
- **Removing persistence mechanisms**: Delete scheduled tasks, startup registry keys, services, WMI event subscriptions, or other mechanisms used to maintain access.
- **Cleaning registry modifications**: Restore or remove malicious registry keys or values added by the attacker.
- **Revoking and rotating credentials**: Reset passwords for compromised accounts, rotate API keys, and revoke certificates or tokens that may have been compromised.

## Recovery

The **recovery phase** includes restoring the affected servers, devices, and services to their previous secure operating state and confirming their proper functioning. The goal is to return to normal business operations while ensuring that the threat has been fully eradicated and that systems are hardened against reinfection.

### Recovery strategy

The recovery phase varies from organization to organization based on system architecture, backup strategies, compliance requirements, and risk tolerance. Make sure your organization has a **proper recovery strategy** documented and tested during the Preparation phase.

### Recovery strategies and activities

Strategies and activities that can be applied in the recovery phase include:

- **Returning to pre-compromise backup**: Restore systems from clean backups taken before the compromise occurred. Verify the integrity and cleanliness of backups before restoration, and ensure that the restoration process does not reintroduce the threat.
- **Rebuilding systems from scratch**: Reinstall operating systems and applications from trusted media or gold images. This is the most thorough method and ensures no residual malicious code remains, but it is also the most time-consuming.
- **Replacing compromised files with clean versions**: Restore individual files or configurations from known-good sources (verified backups, original installation media, trusted repositories).
- **Installing patches and updates**: Apply security patches, firmware updates, and configuration changes to remediate vulnerabilities exploited during the attack and reduce the attack surface.
- **Changing passwords**: Reset passwords for all affected accounts (user accounts, service accounts, administrative accounts) and enforce strong password policies. Consider implementing multi-factor authentication (MFA) if not already in place.
- **Tightening network perimeter security**: Update firewall rules, intrusion prevention signatures, web application firewall (WAF) policies, and access control lists (ACLs) to block known malicious infrastructure and restrict unnecessary access.
- **Enhancing monitoring and detection**: Deploy additional logging, monitoring, and detection capabilities on recovered systems to quickly identify any signs of reinfection or residual attacker presence.
- **Validating system integrity**: Perform integrity checks (file integrity monitoring, hash verification, antivirus scans) to confirm that systems are clean and functioning correctly.
- **Testing functionality**: Verify that applications, services, and business processes are operating normally and that no unintended side effects resulted from recovery actions.
- **Gradual reintroduction to production**: Bring systems back online incrementally with enhanced monitoring, rather than all at once, to detect any issues early.

### Post-recovery validation

After recovery is complete, the organization should:

- Monitor recovered systems closely for signs of reinfection or anomalous behavior.
- Conduct follow-up security assessments (vulnerability scans, penetration tests) to validate that weaknesses have been addressed.
- Document the recovery process, lessons learned, and any outstanding issues or risks.
- Communicate with stakeholders to confirm that normal operations have resumed and to provide updates on the incident status.

The recovery phase marks the transition from active incident response back to normal operations, but it should be followed by thorough post-incident activities to ensure that the root causes have been addressed and that the organization is better prepared for future incidents.
