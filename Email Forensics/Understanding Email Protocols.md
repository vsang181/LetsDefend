# Understanding Email Protocols

Understanding email protocols is essential in email forensics, criminal investigations, and security breach response. In this lesson, we will discuss in detail the basic email protocols, how they work, and how they can be used in email forensic investigations.

Email is an integral part of our daily communication, and email protocols such as SMTP, POP3, and IMAP are the foundation of that communication. To meet users' messaging needs securely and effectively, these protocols define how email is sent, received, and managed. Each protocol presents different security challenges and solutions while addressing different needs.

## SMTP (Simple Mail Transfer Protocol)

SMTP (Simple Mail Transfer Protocol) is the fundamental protocol for sending email over the Internet. SMTP rules govern how an email message is transmitted and delivered between mail servers. This protocol is often referred to as a "push" protocol because messages are sent directly from the sender's email server to the recipient's email server.

<img width="1200" height="628" alt="image" src="https://github.com/user-attachments/assets/d32e76e5-428a-4335-b196-a2c97ce5881a" />

### SMTP operation process

When a user sends an email, their email client (such as Outlook or Gmail) uses SMTP to transmit the message to the outbound email server. The sender's email server then connects to the recipient's email server and forwards the message using SMTP commands and responses, ultimately delivering it to the recipient's mailbox.

<img width="647" height="600" alt="image" src="https://github.com/user-attachments/assets/e34ebaf7-c7f1-4ad5-ac9d-36460cb4bf4c" />

### Security and vulnerabilities

One of the disadvantages of SMTP from a security perspective is that it does not encrypt the contents of messages by default, which leaves the message body and headers vulnerable to interception or modification during transit. However, with additional security measures such as TLS (Transport Layer Security) or STARTTLS (opportunistic encryption upgrade), these weaknesses can be minimized. Modern mail infrastructure increasingly enforces encrypted transport to protect against eavesdropping and man-in-the-middle attacks.

### Email flow example: Bob to Alice

Let's follow the flow of an email as it travels from Bob to Alice:

**Step 1**: Bob opens his email application, enters Alice's email address (`alice@yahoo.com`), types his message, and sends it.

**Step 2**: The email application communicates with Bob's email server (e.g., Gmail's SMTP server) and forwards Bob's message to the server, where it is queued for delivery to `alice@yahoo.com`.

**Step 3**: Bob's email server sees a message waiting to be sent to `alice@yahoo.com`. To deliver this message, it contacts the `yahoo.com` mail server. This is where SMTP comes into play. SMTP is the protocol in charge of server-to-server communication. In this scenario, Bob's email server acts as an SMTP client, and Alice's email server acts as an SMTP server.

**Step 4**: After the initial SMTP handshake between the Gmail and Yahoo mail servers, the SMTP client transmits Bob's message to Alice's mail server.

**Step 5**: Alice's mail server receives the message and stores it in Alice's mailbox so that Alice can retrieve it later using an email client.

**Step 6**: Alice uses her email client (e.g., Microsoft Outlook) to retrieve messages from her mailbox and reads the message that Bob sent.

### SMTP commands

SMTP is a text-based protocol consisting of commands sent by the client and numeric response codes returned by the server. Some common SMTP commands include:

| Command | Description |
|---|---|
| `EHLO` | Used by the SMTP client to identify itself during communication with the server and to obtain a list of supported extensions (Enhanced SMTP). |
| `MAIL FROM` | Specifies the sender's email address (envelope sender). |
| `RCPT TO` | Specifies the recipient's email address (envelope recipient). |
| `DATA` | Indicates the start of the message header and body. The client sends the full message content after this command. |
| `QUIT` | Ends the SMTP session and closes the connection. |

### Differences between SMTP port 25 and port 587

The ports used for SMTP may vary depending on the purpose and security requirements.

<img width="1600" height="910" alt="image" src="https://github.com/user-attachments/assets/53f2a316-bb64-41f3-a282-264ae19db467" />


**Port 25**

- **Purpose**: Port 25 is the default and oldest SMTP port, traditionally used for server-to-server email relay (MTA-to-MTA communication).
- **Security**: Many Internet Service Providers (ISPs) restrict or block outbound connections on port 25 to reduce spam and abuse. Port 25 generally supports unencrypted communication by default, which presents security risks. Some servers support STARTTLS on port 25, but enforcement is inconsistent.

**Port 587**

- **Purpose**: Port 587 is specifically designed for email submission by clients (MSA, Mail Submission Agent). It allows users to send email from their email client to their outbound mail server for delivery.
- **Security**: Port 587 typically requires authentication (username/password) and encourages or enforces the use of encryption (STARTTLS or TLS). This provides an extra layer of security for user credentials and data integrity, making it the preferred port for modern email sending.

**Comparison**

| Aspect | Port 25 | Port 587 |
|---|---|---|
| **Primary use** | Server-to-server relay (MTA to MTA) | Client-to-server submission (user to MSA) |
| **Authentication** | Often not required for relay between trusted servers | Typically required |
| **Encryption** | Optional, inconsistently enforced | Strongly encouraged or required (STARTTLS/TLS) |
| **ISP blocking** | Commonly blocked for residential/consumer connections | Usually allowed |

If a user or organization wants to send email securely from an email client, using port 587 with STARTTLS is more appropriate and secure. Port 25 is generally used for server-to-server communication in large and controlled networks.

## IMAP and POP3

IMAP (Internet Message Access Protocol) and POP3 (Post Office Protocol version 3) are two different protocols for retrieving email from email servers. Both protocols offer different methods of managing email, depending on the user's needs and workflow.

### IMAP

**Features**: IMAP manages emails on the server and allows users to access and synchronize email across multiple devices. When users read, delete, or organize emails, these changes are reflected across all devices that access the same mailbox. IMAP supports folder hierarchies, server-side search, and partial message downloads (e.g., headers only), making it ideal for modern multi-device workflows.

**Security**: IMAP requires a persistent or repeated connection to the server and is usually encrypted with SSL/TLS. However, strong authentication mechanisms (e.g., OAuth2, App Passwords) and encryption are essential, as credentials and message content can be exposed if transmitted in cleartext.

**IMAP ports**

- **Port 143**: Standard unencrypted IMAP port. Modern usage often upgrades this connection using STARTTLS.
- **Port 993**: Secure IMAP over TLS/SSL (IMAPS). This port provides encrypted communication from the start and is generally preferred by modern email services.

### POP3

**Features**: POP3 downloads emails from the server to the local device and typically deletes them from the server (though some configurations leave copies on the server). This allows offline access to emails but limits synchronization across multiple devices. Messages are only viewable on the device where they were downloaded unless server copies are retained.

**Security**: POP3 can be encrypted with SSL/TLS, but it is less flexible than IMAP in terms of message management and multi-device access. If no local backups are made and server copies are deleted, the risk of data loss increases.

**POP3 ports**

- **Port 110**: Standard unencrypted POP3 port. Can be upgraded to encrypted communication using STARTTLS.
- **Port 995**: Secure POP3 over TLS/SSL (POP3S). This port provides encrypted communication and is recommended for secure email retrieval.

### Comparison and usage scenarios

| Protocol | Best for | Multi-device sync | Server storage | Offline access |
|---|---|---|---|---|
| IMAP | Multi-device workflows, modern email management | Yes | Messages remain on server | Limited (depends on client caching) |
| POP3 | Single-device use, simple local storage | No | Messages typically deleted after download | Yes (full local copy) |

**IMAP** is more suitable for situations requiring constant synchronization and email access on multiple devices (laptop, phone, tablet).

**POP3** is ideal for users with simple email needs who usually manage email on a single device and prefer local storage.

### Receiving email: Alice's workflow

In our earlier scenario, Bob used his email application to send a message via SMTP to his email server. Alice, however, does not want to send email; she wants to receive and read messages already stored in her Yahoo mailbox.

Alice's email application can use either **POP3** or **IMAP** to retrieve messages from her Yahoo server:

- If Alice uses **POP3**, her client downloads the message to her local device, and the server copy is typically deleted.
- If Alice uses **IMAP**, her client synchronizes with the server, allowing her to view and manage the message on multiple devices without losing the server copy.

### Web-based email access

Apart from these protocols, many users access email through web browsers (e.g., visiting `gmail.com` or `yahoo.com`). In this case, protocols such as SMTP, POP3, or IMAP do not directly come into play on the user side because browsers send and receive HTTP/HTTPS messages to interact with the mail server's web interface. However, behind the scenes, SMTP is still used for server-to-server communication (e.g., between Gmail's mail server and Yahoo's mail server) when messages are relayed.

## Email protocols in forensic investigations

Understanding how these protocols work and their potential security issues is critical in email forensics investigations. Detailed analysis of email logs, SMTP transaction records, and authentication mechanisms is essential for evidence collection and evaluation in forensic investigations. For example:

- SMTP logs can reveal the path a message took, including intermediate servers, timestamps, and authentication results.
- IMAP/POP3 access logs can show when and from which IP addresses a mailbox was accessed, aiding in attribution and timeline reconstruction.
- Protocol-level artifacts (e.g., unencrypted transmission, authentication failures, unusual relay patterns) can indicate compromise, abuse, or policy violations.

## Conclusion

The email protocols SMTP, IMAP, and POP3 are the cornerstones of today's digital communications infrastructure. They play a critical role in sending, receiving, and managing email. SMTP is used to reliably deliver messages between servers, while IMAP and POP3 offer different benefits depending on how users retrieve and manage messages. A thorough knowledge of these protocols is essential to identify and address potential security breaches, conduct forensic analysis, and implement defensive measures. By understanding the security gaps in these protocols and promoting encryption, authentication, and awareness, we can create more secure digital communication environments. Professionals equipped with this knowledge can effectively carry out critical tasks such as evidence collection, timeline reconstruction, and threat analysis in forensic IT processes.
