# Email Attachment Analysis

Email Attachment Analysis is the process of detecting security threats and malicious content by examining the attachments of inbound and outbound email messages. This process is particularly important for large organizations and enterprises because email attachments are often used to deliver malware, exploit vulnerabilities, and cause data leaks or breaches.

As a result, the analysis of email attachments is an important part of the email forensics process and plays a crucial role in the detection of malicious content, fraud attempts, and security breaches.

This analysis consists of three main phases: **static analysis**, **dynamic analysis**, and **signature-based detection**.

## Static analysis

Static analysis examines the basic characteristics of email attachments without executing them. To determine whether a file is suspicious or malicious, static details such as file type, size, metadata, and embedded artifacts are evaluated.

### Key static analysis checks

**File types**: Certain file types (e.g., `.exe`, `.scr`, `.bat`, `.vbs`, `.js`, `.hta`, `.rar`, `.zip` with nested executables) are common carriers of malware. These types are automatically flagged for further investigation. Double extensions (e.g., `invoice.pdf.exe`) are also a red flag for masquerading.

**File sizes**: Abnormally large or small file sizes might indicate the presence of hidden content, embedded payloads, or attempts to evade detection through file inflation or deflation.

**Metadata**: Metadata such as file creation and modification dates, author information, software used to create the file, and embedded properties provide important clues about the file's origin, history, and legitimacy. Inconsistencies in metadata (e.g., a document purportedly from a corporate sender but created by an unrelated author) can indicate forgery or reuse of stolen templates.

In previous lessons, we learned how to export email attachments with Autopsy and how to decode them if necessary (e.g., Base64-encoded inline attachments).

### Sample scenario: PDF attachment analysis

Let's say you received a PDF file by email. The source of the file is suspicious because it was received unexpectedly or from an unknown sender.

The analysis is carried out in the following steps:

#### 1. Check the file signature and type

The file's MIME type and file signature (magic bytes) are checked to determine what type of file it really is. This step is necessary because attackers often rename file extensions to disguise malicious executables (e.g., a file ending in `.pdf` may actually be an `.exe` with a spoofed extension).

You can use tools like `file` (Linux/Unix command) or hex editors to inspect the file header and confirm the actual file type.

#### 2. Compare hash values

The hash value (MD5, SHA1, SHA256) of the file is obtained and compared with hash values in various malware databases and threat intelligence feeds (VirusTotal, MalwareBazaar, hybrid-analysis.com).

If the hash matches a known malicious file, the attachment can be immediately flagged and quarantined.

#### 3. Content analysis

With the help of PDF reader and editor software (or specialized tools like `pdfid`, `pdf-parser`, `peepdf`), the text, images, and embedded objects in the file are analyzed.

PDF files often contain malicious JavaScript code, embedded executables, or exploit triggers (e.g., exploiting vulnerabilities in PDF readers). Look for suspicious elements such as `/JavaScript`, `/OpenAction`, `/Launch`, `/AA` (automatic actions), and embedded streams.

#### 4. String analysis

Extract plaintext (character strings) within the file using tools like `strings` (Linux/Unix command) or hex editors, and search for suspicious content such as:

- Malicious commands or PowerShell scripts
- IP addresses or domains (potential C2 infrastructure)
- Obfuscated code or encoded payloads
- URLs that may be used for phishing or payload delivery

### Popular static analysis tools

**VirusTotal**

<img width="3002" height="1640" alt="image" src="https://github.com/user-attachments/assets/f718d55e-4cd3-4728-a6ff-203dec45c7d1" />

VirusTotal is an online service that can be used to analyze files, URLs, domains, and IP addresses. It provides detailed information on file hashes, signatures, and content by checking each uploaded file against multiple antivirus engines and threat intelligence databases. VirusTotal also displays behavioral indicators, metadata, and related IOCs (URLs, domains, IPs) associated with the file.

<img width="2986" height="1302" alt="image" src="https://github.com/user-attachments/assets/2d5d8971-a0a1-4ed3-a616-e75b16a5e44c" />

<img width="2904" height="1916" alt="image" src="https://github.com/user-attachments/assets/f7dc69c6-26f8-4491-92e7-e9f18643db3c" />

**Hybrid Analysis**

Hybrid Analysis, powered by Falcon Sandbox, provides a wide range of dynamic and static analysis capabilities. It performs in-depth analysis of user-uploaded files and is widely used by the security community for threat hunting and malware research.

**Strings (Linux/Unix command)**

`strings` is an offline tool commonly used on Unix-like systems. It analyzes files by extracting printable character sequences, which can reveal embedded commands, URLs, file paths, or other artifacts useful for investigation.

### Example: analyzing a suspicious `.xls` file on VirusTotal

When analyzing a file attached to an email with the hash of an `.xls` file on VirusTotal, you may see the following results:

- **Detection**: The file is reported as "suspicious" or "malicious" by many vendors (antivirus engines).
- **Relationships tab**: You can view "Connected URLs," "Connected Domains," and "Connected IP Addresses" that the file communicates with or references. This helps identify command-and-control (C2) infrastructure or phishing domains.
- **Behavior tab**: Shows actions the file attempted (e.g., process creation, registry modifications, network connections) if dynamic analysis was performed.

These tools allow cybersecurity professionals to investigate threats in more detail and provide the basis for static analysis of files. Tool selection depends on the type of file being analyzed and your requirements (e.g., offline vs. online analysis, depth of inspection, automation needs).

## Dynamic analysis

Dynamic analysis is the real-time execution of email attachments in a sandbox environment. This method aims to detect malicious activity by observing the behavioral characteristics of the file when it runs.

### Key dynamic analysis techniques

**Sandbox environment**: Email attachments are executed in an isolated virtual machine or container. In this way, malware cannot cause damage to real systems, and the analyst can safely observe the full execution chain.

**Behavior monitoring**: The file's access to system resources, communication over the network, file and registry modifications, process creation, and other operating system interactions are monitored. This is vital because malware often attempts to copy itself, steal data, establish persistence, or spread to other systems.

### Sample scenario: `.docx` attachment analysis

A file with the extension `.docx` is received by email. The file content encourages the recipient to "enable macros" to view an important work-related document.

The analysis is carried out in the following steps:

#### 1. Execution in a sandbox environment

Run the file in an isolated virtual machine or sandbox environment so that the behavior of the malware can be observed without causing damage to production systems.

#### 2. Network activity monitoring

Monitor whether the file attempts to access the Internet, communicate with external servers, or download additional payloads. Record all DNS queries, HTTP/HTTPS requests, and connections to suspicious or known malicious IPs.

#### 3. Monitoring file and registry changes

Record the changes made to the file system and Windows registry by the file. This includes file creation, modification, deletion, and registry key additions or modifications (often used for persistence).

#### 4. Behavior monitoring

Detect possible malicious behavior such as:

- Keystroke logger activity
- Suspicious process creation (e.g., spawning PowerShell, cmd.exe, or script interpreters)
- Credential dumping or privilege escalation attempts
- Data exfiltration or encryption (ransomware indicators)

### Popular dynamic analysis tools

**ANY.RUN**

ANY.RUN is an interactive online sandbox service that allows users to run malware in real time in an isolated environment and visually track all processes. The user can see how the malware interacts with the network, changes files, modifies the registry, and performs other actions. The interactive nature allows the analyst to click through prompts, enable macros, or trigger conditional behavior.

**Cuckoo Sandbox**

Cuckoo Sandbox is an open-source sandbox software that allows users to run suspicious files in an isolated environment to analyze how they behave on the system. It reports network traffic, file interactions, registry changes, and API calls in detail. Cuckoo can be deployed on-premises for private analysis and supports multiple operating systems and file types.

**Joe Sandbox**

Joe Sandbox provides support for a wide range of file types and operating systems for comprehensive malware analysis. It performs automated and in-depth analysis, providing detailed information on malware network activity, file modifications, registry entries, and behavioral patterns. Joe Sandbox also includes advanced features like anti-evasion techniques and detection of VM-aware malware.

**VMRay**

VMRay is best known for its advanced threat detection and response capabilities. Its sandbox technology uses advanced analysis techniques (including hypervisor-based monitoring) to detect malicious activity while monitoring the behavior of malware. VMRay is particularly effective at detecting evasive and polymorphic malware.

### Example: analyzing a suspicious `.xlsx` file on ANY.RUN

When you analyze a sample file with the extension `.xlsx` on ANY.RUN, you can view and examine:

**HTTP Requests**: All HTTP/HTTPS requests made by the file or spawned processes, including URLs, user agents, and response codes.

<img width="1370" height="240" alt="image" src="https://github.com/user-attachments/assets/43b7369b-a53e-4d7b-a4cd-da4ab96e221a" />

**Connections**: Network connections established by the file, including destination IPs, ports, and protocols.

<img width="1376" height="400" alt="image" src="https://github.com/user-attachments/assets/e85922ec-fc7d-4e74-a60e-0cf351d50ac1" />

**DNS Queries**: DNS lookups performed by the file or related processes, which can reveal C2 domains or infrastructure.

<img width="1752" height="240" alt="image" src="https://github.com/user-attachments/assets/18a32497-4ef4-4025-b709-b17649779e77" />

**Threats**: If a threat has been detected (e.g., known malware family, malicious behavior, IOC match), you can view it under the "Threats" tab with details and classifications.

<img width="1314" height="194" alt="image" src="https://github.com/user-attachments/assets/91605b58-6bd1-4a3b-a276-10d2fa4117c0" />

**Files**: File activity, including files created, modified, deleted, or dropped by the malware. This section also shows file paths and hashes for further analysis.

<img width="2622" height="498" alt="image" src="https://github.com/user-attachments/assets/3ad7befc-9c63-4197-924e-be1843847cc8" />

These tools make malware detection and analysis much easier for cybersecurity professionals. Each tool offers unique features to address different needs (e.g., interactive vs. automated analysis, on-premises vs. cloud-based, depth of behavioral monitoring). Using these tools is recommended for effective malware analysis, threat hunting, and incident response.

## Signature-based detection

Signature-based detection involves comparing file hashes, byte patterns, or known malicious signatures against threat intelligence databases and antivirus engines. This method is fast and effective for detecting known threats but may miss zero-day malware or polymorphic variants that alter their signatures.

Common signature-based tools include antivirus engines (integrated into VirusTotal, Hybrid Analysis) and YARA rules, which allow analysts to define custom patterns and indicators to match against files.

## Conclusion

The analysis techniques covered in this lesson are essential to the security of email attachments and the prevention of email-based attacks. Each phase of analysis (static, dynamic, signature-based) provides in-depth information in email forensics investigations and plays an important role in eliminating potential threats.

By combining static analysis (metadata, file type verification, string extraction, hash checking) with dynamic analysis (sandbox execution, behavior monitoring, network traffic analysis), analysts can detect both known and unknown threats, validate suspected malicious activity, and gather forensic evidence.

These techniques also provide accurate and reliable evidence in legal and security investigations and play a critical role in shaping institutional and individual security policies, incident response procedures, and threat intelligence programs.
