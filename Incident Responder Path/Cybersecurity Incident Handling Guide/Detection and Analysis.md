# Detection and Analysis

The **Detection and Analysis** phase is the step in which a cybersecurity incident is detected and investigated in depth. This phase is often the most challenging and time-consuming part of incident response because it requires distinguishing real threats from false positives, correlating data from multiple sources, and making time-sensitive decisions with incomplete information.

## Detection

Every cybersecurity incident starts with **detection**. The earlier an incident is detected, the less damage it typically causes and the faster the organization can respond.

Cybersecurity events can be transmitted to you through different channels, including:

- **SIEM alerts**: Rules triggered by correlation of log data and security events.
- **Security product alerts**: Detections from EDR, antivirus, IDS/IPS, firewall, email gateway, DLP, or other security tools.
- **IT team reports**: Reports of system slowdowns, unusual behavior, service outages, or configuration changes.
- **User reports**: Notifications sent via helpdesk, email, contact forms, or phone calls about suspicious emails, pop-ups, or system behavior.
- **Threat intelligence feeds**: External notifications of compromised credentials, IP addresses, or domains associated with the organization.
- **Third-party notifications**: Alerts from partners, customers, law enforcement, or security researchers.

Effective detection relies on comprehensive logging, continuous monitoring, well-tuned detection rules, and a security-aware culture where users feel comfortable reporting suspicious activity.

## Verification

Things would be easier if every detection was a confirmed cybersecurity incident, but that is not the case. Instead of diving directly into the incident response process, **the detection must first be verified and evaluated** to determine whether it is indeed a cybersecurity incident or a false positive.

A SOC analyst may triage hundreds of SIEM alerts (detections) in their daily work routine. Can you imagine if each of these was a real cybersecurity incident? The volume would be overwhelming and unsustainable.

As an incident responder, you must first confirm the accuracy of the detection you receive and whether it represents a true cybersecurity incident. For example, suppose you receive a detection from your IT team indicating abnormalities in file extensions on a server. If you assume it is directly caused by ransomware and immediately initiate incident response procedures and isolate the server, you may cause unnecessary service interruption and business disruption. You must first **verify the incoming detection** to understand what actually happened.

### Example verification workflow

1. **Verify the reported anomaly**: Confirm whether the file indeed has an abnormal extension (e.g., `.encrypted`, `.locked`, or random characters).
2. **Investigate the root cause**: Even if the file extension is abnormal, someone with access to the server may have renamed files by mistake or as part of a legitimate administrative task. What you need to do is investigate the reason for the abnormal extension by reviewing logs such as:
   - **Sysmon logs** to understand who modified the file, what process was used, and the parent process chain.
   - **File access logs** to determine when and by whom the file was accessed or modified.
   - **User activity logs** to understand the context of the user's actions.
3. **Correlate with other indicators**: Look for additional signs of compromise, such as suspicious processes, network connections, lateral movement, or other affected systems.
4. **Determine if it's an incident**: After completing your investigation, you may find that a bored or untrained IT staff member renamed files as a test or by accident, or you may confirm that ransomware has encrypted files and further action is required.

As you can see in this example, before starting the incident response processes, it is extremely important to confirm the accuracy of the incoming detection and whether it truly represents a cybersecurity incident.

## Documentation and evidence collection

If you determine there is a real cybersecurity incident, you should start **collecting and recording evidence** immediately. You can use an **issue tracking system** (ticketing system, case management platform, or dedicated incident response platform) to record information, evidence, and actions related to the event in a structured and auditable manner.

The information that should be included in the issue tracking system, as recommended by NIST, includes:

- **The current status of the incident** (new, in progress, forwarded for investigation, contained, eradicated, resolved, etc.)
- **A summary of the incident**: Brief description of what happened, when, and to which systems.
- **Indicators related to the incident**: IOCs such as malicious IPs, domains, file hashes, registry keys, user accounts, etc.
- **Other incidents related to this incident**: Links to related cases, campaigns, or threat actors.
- **Actions taken by all incident handlers**: Timeline of response actions, including who did what and when.
- **Chain of custody**, if applicable: Documentation of evidence handling for legal or forensic purposes.
- **Impact assessments related to the incident**: Business impact, data exposure, systems affected, downtime, etc.
- **Contact information for other involved parties**: System owners, system administrators, business unit leaders, legal, HR, etc.
- **A list of evidence gathered during the incident investigation**: Log files, memory dumps, disk images, network captures, malware samples, etc.
- **Comments from incident handlers**: Notes, observations, hypotheses, and decisions made during the investigation.
- **Next steps to be taken**: Actions such as rebuilding hosts, upgrading applications, patching vulnerabilities, resetting passwords, etc.

Since incident response information contains critical and sensitive data, it is necessary to **keep this information up-to-date at all times** and to **limit access** to only authorized personnel (incident response team, management, legal, etc.). Proper access controls and encryption should be applied to protect case data.

## Prioritization

You may be exposed to more than one cybersecurity incident at the same time. The approach of responding to cybersecurity incidents on a **"first come, first served"** basis is not correct. You should prioritize incidents based on **severity, criticality, impact area, potential damage, data sensitivity, and recovery complexity**, and start handling these incidents according to this prioritization.

NIST recommends using the following categories when prioritizing cybersecurity incidents:

### 1. Functional impact of the incident

This category measures the impact on the organization's ability to provide services or conduct business operations.

| Category | Description |
|---|---|
| None | No effect on the organization's ability to provide services. |
| Low | Minimal effect; the organization can still provide all critical services, but has lost efficiency. |
| Medium | Organization has lost the ability to provide a critical service to a subset of system users. |
| High | Organization is no longer able to provide some critical services to any users. |

### 2. Information impact of the incident

This category measures the impact on the confidentiality, integrity, and availability of information.

| Category | Description |
|---|---|
| None | No information was exfiltrated, changed, deleted, or otherwise compromised. |
| Privacy Breach | Sensitive personally identifiable information (PII) of taxpayers, employees, beneficiaries, etc., was accessed or exfiltrated. |
| Proprietary Breach | Unclassified proprietary information, such as protected critical infrastructure information (PCII), was accessed or exfiltrated. |
| Integrity Loss | Sensitive or proprietary information was changed or deleted. |

### 3. Recoverability from the incident

This category measures the time and resources required to recover from the incident.

| Category | Description |
|---|---|
| Regular | Time to recovery is predictable with existing resources. |
| Supplemented | Time to recovery is predictable with additional resources. |
| Extended | Time to recovery is unpredictable; additional resources and outside help are needed. |
| Not Recoverable | Recovery from the incident is not possible (e.g., sensitive data exfiltrated and posted publicly); launch investigation. |

By using these categories to assess and prioritize incidents, you can allocate resources effectively and focus on the incidents that pose the greatest risk to the organization.

## Notification

After verification of the detection, confirmation of the cybersecurity incident, and prioritization, **the relevant authorities should be informed** according to the organization's escalation and communication plan.

It is extremely important to have **documentation prepared in advance** that describes who to contact, their contact information, and under what circumstances to contact them at the time of cybersecurity incident detection. This documentation helps the incident response team act quickly by knowing the correct points of contact and reaching them within a short period of time.

A sample notification list may include:

| Role/Team | When to notify | Contact method |
|---|---|---|
| Incident Response Team Lead | All confirmed incidents | Secure messaging, phone |
| IT Operations / System Administrators | Incidents affecting specific systems | Ticketing system, email, phone |
| CISO / Security Management | Medium and high severity incidents | Email, phone, secure messaging |
| Legal Department | Incidents involving data breach, regulatory obligations, or potential litigation | Email, phone |
| HR Department | Incidents involving insider threats or employee misconduct | Email, phone |
| Public Relations / Communications | Incidents that may become public or affect customers | Email, phone |
| Executive Management / CEO | High severity incidents with significant business impact | Phone, in-person briefing |
| Law Enforcement | Incidents involving criminal activity (as appropriate and legally required) | Phone, formal reporting channels |
| Regulators / Government Agencies | Incidents requiring mandatory breach notification | Formal reporting channels |
| External Partners / Vendors | Incidents affecting third parties or involving their systems | Email, phone |

The issue of **who will be informed** varies depending on the type of cybersecurity incident, the organization's industry, regulatory requirements, and applicable laws. For example, data breaches involving personally identifiable information (PII) may trigger mandatory notification requirements under regulations such as GDPR, CCPA, HIPAA, or state breach notification laws.

## Analysis

The **analysis step** is where the activities of the attacker are analyzed in depth. In this step, every detail should be analyzed—from the moment of the attacker's first access to their most recent activities on the corporate systems. The goal is to understand the full scope of the compromise, the attacker's tactics and techniques, and the impact on the organization.

### Analyzing the root cause

When responding to cybersecurity incidents, **our priority should be to detect the attacker's initial access method** (the root cause) and stop this access to prevent reinfection.

Let's consider a scenario where an attacker gains access to the system by exploiting a vulnerability in a web application. If you attempt to delete the malicious software installed by the attacker on the servers **before** identifying and remediating the attacker's initial access method, the attacker can simply exploit the vulnerability in the web application again, regain access to the server, and reinstall the malware. This creates a cycle where the attacker repeatedly regains access, wasting time and resources.

For this reason, **first of all, the root cause (initial access vector) should be detected and remediated**. Common initial access vectors include:

- Exploited vulnerabilities (web application flaws, unpatched software, misconfigurations)
- Phishing emails with malicious attachments or links
- Compromised credentials (password reuse, credential stuffing, brute force)
- Supply chain compromise (trusted third-party software or services)
- Physical access or insider threats

After the root cause is identified and addressed, you should proceed to the **Containment, Eradication & Recovery** step to prevent the attacker from compromising the systems again during the incident response efforts. After containment is achieved, you return to the **Analysis** step to continue investigating the attacker's other activities and ensure no additional footholds or persistence mechanisms remain.

### Analyzing other activities

During incident response, after eliminating the possibility of the attacker regaining access through the initial method, **other attacker activities should also be analyzed** to understand the full extent of the compromise. This includes:

- **Lateral movement**: Which other systems, accounts, or network segments did the attacker access?
- **Privilege escalation**: Did the attacker gain administrative or domain-level privileges?
- **Persistence mechanisms**: Did the attacker install backdoors, create new accounts, schedule tasks, or modify system configurations to maintain access?
- **Data exfiltration**: Was sensitive data accessed, copied, or transmitted outside the organization?
- **Impact on data integrity**: Were files modified, deleted, or encrypted (ransomware)?
- **Command and control (C2)**: Which external IPs, domains, or infrastructure did the attacker use for communication?

The **Detection & Analysis** and **Containment, Eradication & Recovery** steps exist in a **continuous cycle**. As new compromised devices and user accounts are discovered during analysis, they should be **isolated from the network quickly** to prevent further spread. After containment actions are taken, analysis resumes to identify any additional affected systems or attacker activities, and the cycle continues until the full scope of the incident is understood and all threats are eradicated.

This iterative approach ensures thorough investigation while minimizing the window of opportunity for the attacker to cause additional harm.
