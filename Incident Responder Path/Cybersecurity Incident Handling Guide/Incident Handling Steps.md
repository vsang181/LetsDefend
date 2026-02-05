# Incident Handling Steps

According to the recommendations published by NIST in the "Computer Security Incident Handling Guide" (NIST SP 800-61 Revision 2), cyber incident response processes should be handled in **4 main steps**. These steps form a cyclical framework that guides organizations through the entire incident lifecycle, from readiness through recovery and continuous improvement.

<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/84a15be5-5f0c-4e76-b80e-991113795f30" />

The four steps are:

## 1. Preparation

**Preparation** is the foundational phase that occurs before any incident takes place. This step involves establishing and maintaining the capability to respond to incidents effectively. Key activities include:

- Forming and training an Incident Response Team (IRT) with clearly defined roles and responsibilities
- Developing incident response policies, procedures, playbooks, and runbooks
- Deploying detection and monitoring tools (SIEM, IDS/IPS, EDR, log management)
- Establishing communication plans and escalation procedures
- Acquiring and maintaining forensic tools, hardware, and software
- Creating baseline configurations and documentation of systems and networks
- Conducting tabletop exercises and simulations to test readiness
- Building relationships with external partners (law enforcement, ISACs, vendors, legal counsel)

Effective preparation ensures that when an incident occurs, the team can respond quickly and confidently without wasting time determining roles, locating tools, or improvising procedures.

## 2. Detection & Analysis

**Detection & Analysis** is the phase where potential security incidents are identified, investigated, and validated. This step involves:

- Monitoring security alerts and events from various sources (SIEM, IDS/IPS, antivirus, firewalls, user reports)
- Triaging and prioritizing alerts based on severity, scope, and potential impact
- Analyzing indicators of compromise (IOCs) and anomalous behavior
- Determining whether an event is a true incident or a false positive
- Documenting initial findings, timeline, affected systems, and attack vectors
- Assessing the scope and severity of the incident
- Notifying stakeholders and escalating as appropriate

Detection and analysis is often the most challenging phase because it requires distinguishing real threats from noise, correlating disparate data sources, and making time-sensitive decisions with incomplete information.

## 3. Containment, Eradication & Recovery

**Containment, Eradication & Recovery** comprises three closely related sub-phases aimed at stopping the incident's spread, removing the threat, and restoring normal operations:

### Containment

- Isolating affected systems to prevent further damage or lateral movement
- Implementing short-term containment (immediate actions to limit impact) and long-term containment (sustainable measures while planning eradication)
- Preserving evidence for forensic analysis and potential legal proceedings
- Maintaining business continuity where possible

### Eradication

- Removing malware, backdoors, unauthorized accounts, and other malicious artifacts
- Closing exploited vulnerabilities and misconfigurations
- Validating that the threat has been fully removed from the environment

### Recovery

- Restoring affected systems and services to normal operation
- Verifying system integrity and functionality
- Monitoring for signs of attacker return or residual malicious activity
- Gradually returning systems to production with enhanced monitoring

This phase requires coordination between technical teams, management, and business units to balance the need for rapid recovery with the need for thorough remediation and evidence preservation.

## 4. Post-Incident Activity

**Post-Incident Activity** (also called "Lessons Learned") is the phase where the organization reviews the incident, evaluates the response, and implements improvements. Key activities include:

- Conducting a post-incident review meeting with all stakeholders
- Documenting what happened, how it was detected, and how the response was conducted
- Identifying what worked well and what could be improved
- Updating policies, procedures, playbooks, and technical controls based on lessons learned
- Sharing threat intelligence and IOCs with relevant communities
- Calculating the financial and operational impact of the incident
- Implementing corrective actions to prevent recurrence

Post-incident activity transforms reactive response into proactive improvement, ensuring that each incident makes the organization more resilient and better prepared for future threats.

***

These four steps form a continuous cycle. Lessons learned from post-incident activity feed back into preparation, creating a process of continuous improvement that strengthens the organization's incident response capability over time.
