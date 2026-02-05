# Email Header Analysis

Email header analysis, as the name suggests, is the examination of the metadata and technical fields contained in the header of an email message. Email headers contain information such as where the message originated, how it was routed, which servers were involved in the transmission process, and whether authentication checks passed or failed. Therefore, the information gained from header analysis can be used to verify email authenticity, detect spam and phishing, investigate security breaches, and support forensic investigations.

## What are the components of email headers?

Email headers contain a range of technical parameters and metadata. Understanding each field is essential for effective analysis.

### From

This field shows who sent the email message. It is usually in the form of a name and email address and plays a key role in identifying the sender.

**Significance**: The `From` field is a critical element in authenticating and assessing the trustworthiness of the sender. However, it is often tampered with during spoofing or phishing attacks because SMTP does not natively validate this field.

**Attack vector analysis**: The `From` field is examined to detect any forgery of the sender's identity. Verifying the legitimacy of an email address (by comparing it to authentication results like SPF, DKIM, and DMARC) is a critical step in identifying phishing and spoofing attacks. Always cross-reference the `From` address with `Return-Path`, `Reply-To`, and the domain in the `DKIM-Signature` or `Authentication-Results` headers.

### To

This field shows the recipient(s) to whom the message was sent. There can be more than one recipient, and each recipient is separated by a comma.

**Significance**: Identifies recipients and describes who the message is for. It is also used for group communications or distribution lists.

**Attack vector analysis**: The `To` field specifies the target users or groups. This insight is particularly important for spear-phishing or targeted attacks. If the recipients are unexpected, unrelated, or include high-value targets (executives, finance department), it may raise suspicion and warrant further investigation.

### Subject

A short text that contains the topic of the email and allows recipients to get an idea of the content of the message at first glance.

**Significance**: The subject line is an important factor in attracting the recipient's attention and prioritizing the message. Spam filters and detection rules often scan subject lines to evaluate content and flag suspicious patterns.

**Attack vector analysis**: The subject field often contains deceptive messages designed to provoke urgency, fear, or curiosity (e.g., "Your account will be suspended," "Urgent payment required"). Subject lines used in fraud or social engineering attempts can be easily recognized through examination and correlation with known attack patterns.

### Date

This field contains the date and time at which the message was sent. The format generally follows the RFC 2822 standard (e.g., `Wed, 16 Nov 2021 16:57:23 +0000`).

**Significance**: Timestamps are used to verify that the message arrived on time and to list events chronologically during investigations.

**Attack vector analysis**: The `Date` field provides the time the email was purportedly sent, and this information can be used to detect time-based attack patterns or spam campaigns. Note that the `Date` field can be manipulated by the sender, so always correlate it with the timestamps in the `Received` headers, which are added by mail servers and are more trustworthy.

### Received

This field contains the address of each server through which the email passed and the date and time of processing on those servers. For each hop, there is a separate `Received` line. These lines are typically listed in reverse chronological order (newest at the top, oldest/origin at the bottom).

**Significance**: The `Received` headers are used to trace the routing path of the email and to identify servers that may be suspicious. Forensically, this information is valuable to verify the origin of emails, reconstruct delivery timelines, and investigate security breaches.

**Attack vector analysis**: The `Received` section contains a sequential list of the servers the email has passed through, which is used to determine if the email traversed potentially suspicious or malicious infrastructure. It can also help identify cases where the message is routed in unexpected ways (e.g., relayed through uncommon countries, residential IPs, or known malicious hosts). Always read the `Received` headers from bottom to top to trace the origin.

### MIME-Version

Specifies the version of the Multipurpose Internet Mail Extensions (MIME) standard used by the email message. Most modern email uses MIME version 1.0.

**Significance**: MIME defines how email content is formatted and encoded, ensuring that multi-part messages (text + HTML, attachments) and non-text content are handled correctly.

**Attack vector analysis**: The MIME version specifies the standard used; outdated or incompatible versions can sometimes increase the likelihood of rendering issues or indicate attempts to exploit parsing bugs. This field is particularly important when analyzing complex or malicious email content with unusual encoding or structure.

### Content-Type

Provides information about the content of the email body, such as `text/plain`, `text/html`, or more complex multipart types like `multipart/mixed` or `multipart/alternative`.

**Significance**: The `Content-Type` header instructs the recipient's email client on how to display the message. HTML content, images, and other media types are handled based on this field.

**Attack vector analysis**: The `Content-Type` information is used to evaluate which content is safe and which may be risky. For example, `text/html` might contain malicious HTML scripts, hidden iframes, or obfuscated links. Multipart messages with nested attachments or unusual encoding may be used to evade detection. Always examine the declared content type and compare it to the actual content structure.

### Message-ID

A unique identifier generated for each email that distinguishes the message. It usually contains a complex sequence and the `@` character, referencing the sending domain or system (e.g., `<12345.67890@example.com>`).

**Significance**: The `Message-ID` is used to track email traffic, refer to specific messages, and organize email conversations (threading). It is also utilized by spam detection systems to identify duplicate or mass-mailed messages.

**Attack vector analysis**: Since `Message-ID` provides a unique identifier, if these identifiers do not match the expected format or domain (e.g., the domain does not match the sender's domain), this may indicate forgery, spoofing, or spamming. It is also used to help understand the relationship between email chains and detect reply-chain hijacking attacks.

## Email header analysis process

Email header analysis is an important process for understanding the source and direction of email, identifying security threats, and managing email traffic effectively.

### How to collect email header metadata

Email header metadata is typically accessible through the email client or server. Most email clients (Outlook, Apple Mail, Gmail, etc.) offer users the ability to view full header details.

**Steps to collect header information:**

1. **Using the email client to view header information**: Most email clients provide an option such as "View Headers," "Show Original," or "View Message Source." This option displays all the details in the email header.
2. **Store header information**: Header information can be saved, usually in plain text format (`.eml` or `.txt`). This is useful for later analysis or sharing with security teams.
3. **Automatic collection tools**: In large-scale or enterprise environments, header information can be collected using automated tools, SIEM systems, or email gateway solutions. These systems collect and log the header information of all incoming and outgoing emails systematically for monitoring and forensic purposes.

### Accessing email headers in Microsoft 365 (Outlook Web App)

To access an email's header information in O365:

1. Open the email in question.
2. Click the **three-dot menu** (`...`) at the top of the message or right-click the email.
3. Select **View → View message source** (or **View message details** depending on the interface version).
4. The section that opens will display all the details of the email, including full headers, body content, and attachments in raw format.

You can copy this information and paste it into a text file or header analysis tool for further examination.

## How to analyze email header information

Email header analysis is the process of examining and evaluating the collected data in detail. This process typically includes the following steps:

### Timestamp and path analysis

Each `Received` line needs to be analyzed because these lines contain the servers through which the email has passed and the timestamp details recorded on each server. This analysis gives an idea of the path the email took and the times at which it arrived at each hop.

**What to look for:**

- Read the `Received` headers from **bottom to top** to trace the origin.
- Identify the first external server (the one closest to the sender) and extract its IP address or hostname.
- Check for unusual delays between hops, which may indicate queueing, relay abuse, or suspicious routing.
- Correlate the timestamps with the `Date` header to detect inconsistencies or manipulation.

### Verification of sender and recipient information

Fields such as `From`, `To`, `Reply-To`, `Return-Path`, and `Cc` reveal the parties involved in the communication. These fields can be critical in identifying email attacks such as spoofing or phishing.

**What to look for:**

- Compare the `From` address with the `Return-Path` and `Reply-To` addresses. Mismatches may indicate spoofing or redirection.
- Note that the **display name** shown in the email application may not match the actual email address (a common phishing trick).
- Check the domain in the `From` address against the domain in the `DKIM-Signature` or `Authentication-Results` headers.
- Validate the sender's domain using SPF, DKIM, and DMARC results (found in the `Authentication-Results` or `Received-SPF` headers).

### Examination of technical parameters

Fields such as `MIME-Version`, `Content-Type`, `Content-Transfer-Encoding`, and `X-Mailer` indicate how and in what format the message is being sent. For messages with embedded malware or links, this information is particularly important.

**What to look for:**

- Check the `Content-Type` for unusual or complex multipart structures (e.g., `multipart/related`, `multipart/alternative` with deeply nested parts).
- In emails containing malware, verify that the MIME version and encoding are consistent and not malformed.
- Mismatched or outdated MIME versions can be an indicator of malicious activity or attempts to exploit parsing bugs.
- Spam and scam emails often use complex content types or obfuscated encodings. The `Content-Type` and `Content-Transfer-Encoding` fields can provide clues to help investigate these emails further.
- Look for the `X-Mailer` or `User-Agent` headers, which may reveal the email client or script used to send the message. Unusual or missing values can indicate automated sending or forgery.

## Conclusion

Email header analysis is a crucial part of email security and a valuable skill for information security professionals. It is critical for email incident response, threat detection and prevention, and evidence gathering in forensic investigations. By systematically examining sender authentication, routing paths, timestamps, and technical parameters, analysts can identify spoofing, phishing, and malicious campaigns. In addition, email header analysis helps organizations and individuals strengthen their defenses against attacks via email and supports the collection of forensically sound evidence for legal proceedings.
