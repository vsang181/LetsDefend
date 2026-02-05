# Email Forensics Tools

Email forensics tools are used to obtain information that can serve as evidence in legal proceedings by analyzing email traffic and content. These tools can examine email headers, bodies, attachments, and metadata such as timestamps, IP addresses, routing information, and authentication artifacts. Below are some popular tools commonly used in email forensic investigations.

## Wireshark

**Overview**: Wireshark is a powerful, industry-recognized open-source tool for capturing and analyzing network traffic in real time or from saved packet capture files (PCAP). It supports deep inspection of email protocols such as SMTP, IMAP, and POP3, and can examine the data passing over these protocols in detail at the packet level.

**Email forensics use**: Wireshark can be used to observe how email is sent and received across the network, reconstruct email sessions, detect protocol errors, identify cleartext transmission of credentials or message content, and uncover security vulnerabilities such as lack of encryption or suspicious relay behavior.

### Using Wireshark for SMTP analysis

To filter and examine only network traffic belonging to the SMTP protocol:

1. Enter `smtp` in the display filter bar and press Enter. This will show only SMTP packets.

<img width="2518" height="1366" alt="image" src="https://github.com/user-attachments/assets/5e7f7bbf-03cd-42e5-924f-5a09a56d8f24" />

2. Right-click on any SMTP packet and select **Follow → TCP Stream** to see the entire SMTP conversation from start to finish, including commands (`EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`) and responses (status codes).

<img width="2370" height="1744" alt="image" src="https://github.com/user-attachments/assets/c9b03cc8-4e7e-4362-adf4-8533dd7b19da" />

This reconstructed stream allows you to read the full email transaction, including message headers, body content (if transmitted in cleartext), and any SMTP authentication attempts.

## NetworkMiner

**Overview**: NetworkMiner is a passive network monitoring and forensic analysis tool designed for Windows and Linux. It can analyze network traffic captured in PCAP files and extract artifacts such as files, email messages, credentials, DNS queries, and session metadata without active probing.

**Email forensics use**: NetworkMiner can be used to examine email communications and file transfers from network traffic, extract attachments transmitted via email protocols, and identify hosts involved in email transmission.

### Using NetworkMiner for email analysis

1. Open a PCAP file containing network traffic.
2. Navigate to the **Sessions** tab to view TCP/UDP sessions. Filter or identify sessions using the SMTP, IMAP, or POP3 protocols to locate email-related traffic.

<img width="1446" height="578" alt="image" src="https://github.com/user-attachments/assets/d4719aad-2a3d-4ab7-8771-645b79e12a4e" />

3. Use the **Keywords** tab to search for specific terms (e.g., sender addresses, recipient domains, keywords in email bodies) across extracted content and session data.

<img width="1766" height="516" alt="image" src="https://github.com/user-attachments/assets/8e829bdf-1ce4-4cfa-af86-688ed4ba1b25" />

4. View the **Hosts** tab to see detailed information about communicating hosts, including IP addresses, operating systems, open ports, and extracted files.

<img width="1770" height="816" alt="image" src="https://github.com/user-attachments/assets/ad1f593a-0c4b-4608-aa64-f30ec25c8efd" />

NetworkMiner automatically reconstructs files and credentials transmitted over the network, which can include email attachments and authentication credentials sent in cleartext.

## The Sleuth Kit (with Autopsy)

**Overview**: The Sleuth Kit is a collection of command-line digital forensic tools, and Autopsy is an open-source graphical interface built on top of it. Together, they provide a comprehensive platform for digital forensic investigations, including disk image analysis, file system examination, timeline generation, and artifact extraction.

**Email forensics use**: Autopsy can be used to extract email data from disk images, local mailbox files, and email archives, and to examine and report on email communications. Autopsy's **Email Parser** module recognizes common email formats (MBOX, EML, PST) based on file signatures, extracts individual messages, and creates searchable artifacts for each message and attachment.

### Performing email forensics with Autopsy

To analyze email files using Autopsy:

1. Launch Autopsy and create a new case.
2. Click **Add Data Source** and choose the appropriate source type:
   - For individual `.eml` files or mailbox files, select **Logical Files**.
   - For disk images containing email data, select **Disk Image or VM File**.
3. When prompted, select **Generate new hostname based on data source name** and continue.
4. Browse and add the `.eml` files (or MBOX/PST files) to the case using the **Add** button, then click **Next**.
5. In the **Configure Ingest** step, ensure that the **Email Parser** module is selected. This module will extract email messages, parse headers, and create artifacts for each message and attachment.
6. Click **Finish** to start the analysis process.

<img width="1942" height="1286" alt="image" src="https://github.com/user-attachments/assets/3ab1b16e-3e51-4b5f-99cc-9c9dd360c7df" />

Once processing is complete, you can navigate the analysis results in the Autopsy interface and examine extracted email artifacts.

## Key email forensics artifacts in Autopsy

When reviewing the analysis results, prioritize the following artifact categories directly related to email forensics:

### Data Artifacts

**Forensic analysis perspective**: Every finding and piece of data should be considered as potential evidence. Information extracted from email accounts may contain metadata that can be linked to a crime, verify user identity, or establish timelines.

**How to analyze**: Analyze each relevant piece of data in the context of the people the user was interacting with, the timeline of events, and the content of the email. Look for patterns, anomalies, and correlations with other evidence sources.

### Communication Accounts

**Forensic analysis perspective**: Communication accounts can reveal links to individuals or groups that may be relevant to criminal activity. They can also reveal the use of multiple accounts, aliases, or suspicious activity patterns.

**How to analyze**: Analyze contact accounts for patterns of use, associated email addresses, and contact frequency. Anomalous activity (e.g., newly created accounts, accounts with single-use patterns, or accounts linked to known threat actors) may require further investigation.

### Email Messages

**Forensic analysis perspective**: All emails found and processed on the system may reveal information about a crime, malicious activity, or insider threats. Email messages extracted from the investigated system play a key role in understanding the chronology of the incident and the nature of the crime.

**How to analyze**: Analyze each message in detail, focusing on:

- Sender and recipient information (spoofing, impersonation)
- Subject lines and message content (social engineering, threats, deceptive language)
- Embedded links (phishing, malicious redirects)
- Attachments (malware, suspicious file types)
- Technical details such as email headers (routing, authentication results, timestamps)

### Metadata

**Forensic analysis perspective**: Metadata contains information such as file creation and modification dates, access times, and ownership, which is essential for placing file accesses and email activity on a timeline.

**How to analyze**: Compare file creation and modification times, file ownership, and access times to the crime timeline. Correlate metadata from email files with system logs, network traffic, and other forensic artifacts to establish a coherent sequence of events.

### Keyword Hits

**Forensic analysis perspective**: Keyword scans help quickly identify important information through specific terms or phrases (e.g., names, email addresses, financial terms, project code names).

**How to analyze**: Evaluate matches found in keyword scans in terms of the context and significance of the relevant content. Keyword hits should be reviewed manually to confirm relevance and, if appropriate, included as potential evidence.

### Analysis Results and Reports

**Forensic analysis perspective**: The analysis results provide an overview of the case under investigation and highlight important data. Autopsy generates comprehensive reports that can include file characteristics, keywords found, emails detected, web history, and other forensic findings.

**How to analyze**: Examine the results of the analysis modules and interpret the findings in the context of the investigation. Use Autopsy's user interface to filter, sort, and export relevant data for reporting or further analysis.

## Conclusion

All the analysis points covered above address different aspects of email forensics, providing critical information for the investigator to gain a comprehensive view of the case. By combining network traffic analysis (Wireshark, NetworkMiner) with disk-based artifact extraction (Autopsy), investigators can reconstruct email communications, identify malicious activity, and collect evidence that is admissible in legal proceedings.
