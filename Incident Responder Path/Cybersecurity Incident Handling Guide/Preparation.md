# Preparation

The preparation step covers all the preparations made before intervening in cybersecurity incidents. The preparations undertaken in this step ensure that cyber incident response is carried out correctly, efficiently, and smoothly.

The incident handling steps framework prepared by NIST shows that **Preparation** is the first and most important step of the Incident Response process to avoid potential incidents and respond effectively when they occur. Notably, the last step (Post-Incident Activity) feeds directly back into the Preparation step within the Incident Response Life Cycle. This demonstrates that preparation is always a **progressive and iterative process** that should be continuously improved based on experiences and lessons learned after each security incident.

The preparation step covers all activities related to incident response readiness. It encompasses many topics, including:

- Preparation of plans, procedures, and policies for incident response processes
- Establishment and training of the incident response team
- Installing and maintaining security tools (IPS/IDS/EDR, SIEM, forensic tools) properly
- Preparing equipment and resources to be used during incident response
- Training employees on social engineering and security awareness
- Establishing communication channels and escalation procedures
- Creating and maintaining documentation and runbooks

As these examples show, the preparation step has broad scope. Everything that facilitates cybersecurity incident response and everything that helps prevent cybersecurity incidents from occurring is considered part of the Preparation step. Let's examine some important categories in detail.

## Communication

Incident responders are in constant communication with different teams and individuals during incident response. It is critically important to create a list of points of contact and their contact information that may be needed during incident response, as well as documentation that includes clear guidance on who to contact, when, and how.

Since the internal communication network may have been compromised during an incident, communications should be carried out through **separate, out-of-band channels** during incident response. For this reason, dedicated telephones, secure messaging platforms, and alternate communication lines to be used by incident responders should be prepared in advance and tested regularly.

When creating contact lists, organizing them by category (internal technical teams, management, legal, external partners, law enforcement, vendors) will facilitate incident response processes and reduce delays during high-pressure situations.

It is also recommended to establish a **war room** (physical or virtual) to be used during cybersecurity incidents. War rooms are dedicated spaces where the incident response team gathers to coordinate activities, discuss strategies and plans, and make time-sensitive decisions collaboratively.

## Inventory list

Preparing a properly created and constantly updated inventory list will directly facilitate your incident response processes. You should maintain current information for all critical systems, especially critical servers (web servers, FTP servers, Exchange servers, SWIFT servers, domain controllers, database servers, etc.). Inventory lists should include:

- System hostnames and IP addresses
- Operating systems and patch levels
- System owners and business criticality
- Data classification and compliance scope
- Physical and logical network locations
- Dependencies and interconnections

Inventory lists that are not kept updated will critically impact incident response processes, cause waste of time, and lead to wrong decisions during the response. An outdated inventory may cause you to miss critical systems that need to be checked, or misidentify systems entirely. For example, if the IP address `10.75.11.10` is listed as belonging to "Web-Server" in an outdated inventory, but it actually belongs to a database server or has been reassigned, it will cause confusion, waste valuable time, and delay necessary containment actions.

Keeping the inventory list up-to-date is a laborious task, so you should utilize automated discovery tools, Configuration Management Databases (CMDBs), and asset management solutions to make it manageable and accurate.

## Documentation

Documentation describing the incident response processes should be prepared in advance, and the incident response team should be familiar with these documents. This ensures that every team member follows the plans, procedures, and policies correctly and that the incident response process proceeds as smoothly as possible.

A separate plan, policy, and procedure should be prepared for each important activity. For example:

- **Incident Response Plan**: High-level document describing the organization's approach to incident response, roles and responsibilities, and integration with business continuity.
- **Incident Response Procedures**: Step-by-step instructions for handling different types of incidents (malware, data breach, DDoS, insider threat, etc.).
- **Containment Procedures**: Specific actions for isolating affected systems while preserving evidence and maintaining business continuity.
- **Communication Plan**: Guidance on how to communicate with internal teams, management, legal, external partners, customers, regulators, and media during incident response.
- **Escalation Matrix**: Criteria for escalating incidents based on severity, scope, and impact.

It is recommended to create **report templates** to standardize and shorten the report preparation process after incident response. Templates ensure consistency, completeness, and compliance with organizational and regulatory requirements.

Prepared documentation should be reviewed and updated periodically (at least annually, and after each major incident) to reflect lessons learned, organizational changes, and evolving threats.

## Network topologies

Network topologies have critical importance for understanding attacker activities and analyzing the attack successfully during incident response. Accurate network diagrams help responders:

- Identify how an attacker may have moved laterally
- Determine which systems may be affected based on network segmentation
- Plan containment actions that minimize business disruption
- Understand trust relationships and data flows

Network topologies should be kept updated regularly to ensure they reflect the correct network information, diagrams, connections, VLANs, trust zones, and security controls (firewalls, IPS/IDS placement, network monitoring points). Outdated network diagrams can mislead responders and cause critical mistakes during containment and eradication.

## Incident handling software and hardware

You should prepare the hardware and software that will be utilized during incident response in advance and maintain it in a ready state. Essential tools and equipment include:

- **Digital forensics workstations** and/or backup devices with sufficient storage and processing power
- **Laptops** dedicated to incident response (not used for daily operations) with forensic and analysis tools pre-installed
- **Blank removable media and hard disks** for evidence collection and imaging
- **Forensics and incident handling software**, such as:
  - Disk imaging and forensic analysis tools (FTK, EnCase, Autopsy, X-Ways)
  - Memory analysis tools (Volatility, Rekall)
  - Network traffic analysis tools (Wireshark, NetworkMiner, Zeek)
  - Malware analysis tools (IDA Pro, Ghidra, Cuckoo Sandbox, ANY.RUN)
  - Log analysis and SIEM platforms
  - Endpoint detection and response (EDR) consoles
  - Threat intelligence platforms

Hardware and software should be tested regularly to ensure they are functional and up-to-date when needed.

## Preventing incidents

The preparation step also includes proactive measures for the prevention of cybersecurity incidents before they occur. Organizations should make the necessary investments and provide **multi-layered protection** by deploying different security solutions, such as:

- **EDR (Endpoint Detection and Response)**: Advanced endpoint protection that detects and responds to threats on workstations and servers.
- **IPS/IDS (Intrusion Prevention/Detection Systems)**: Network and host-based systems that detect and block malicious activity.
- **Antivirus/Anti-malware**: Signature and behavior-based protection against known and unknown malware.
- **WAF (Web Application Firewall)**: Protection for web applications against common attacks (SQL injection, XSS, etc.).
- **Firewall**: Network segmentation and access control to limit lateral movement and unauthorized access.
- **DLP (Data Loss Prevention)**: Tools to detect and prevent unauthorized data exfiltration.
- **Email security gateways**: Filtering and analysis to block phishing, malware, and spam.
- **SIEM (Security Information and Event Management)**: Centralized logging, correlation, and alerting.

At the same time, the awareness level of employees against social engineering attacks should be increased by performing **social engineering tests** (simulated phishing campaigns, physical penetration tests) regularly and providing ongoing security awareness training.

## Establishing an official incident response capability

Organizations must establish an official incident response capability. According to the documentation shared by NIST, this capability includes the following actions:

1. **Creating an incident response policy and plan**: High-level governance documents that define the organization's commitment, authority, and approach to incident response.
2. **Developing procedures for performing incident handling and reporting**: Detailed, step-by-step guidance for responding to incidents and documenting findings.
3. **Setting guidelines for communicating with outside parties regarding incidents**: Rules for engaging with law enforcement, regulators, customers, media, and other external stakeholders.
4. **Selecting a team structure and staffing model**: Determining whether the team will be centralized, distributed, or hybrid, and whether it will be staffed internally, outsourced, or a combination.
5. **Establishing relationships and lines of communication** between the incident response team and other groups, both internal (legal department, HR, IT operations, business units) and external (law enforcement agencies, ISACs, CERTs, vendors, partners).
6. **Determining what services the incident response team should provide**: Defining the scope of the team's responsibilities (monitoring, triage, containment, forensics, threat intelligence, training, etc.).
7. **Staffing and training the incident response team**: Hiring or designating qualified personnel and providing ongoing training, certifications, and professional development.

Since the preparation step will directly affect the quality, speed, and effectiveness of incident response processes, managers must ensure that these preparations are implemented correctly and maintained continuously. You can test your readiness through **tabletop exercises, simulations, and red team/purple team engagements** and improve your preparation before encountering a real incident. Regular testing reveals gaps in documentation, communication, tools, and skills, allowing the organization to mature its incident response capability proactively.
