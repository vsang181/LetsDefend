# Identifying and Analyzing Email Threats

This lesson explains the most common email-based attack vectors with examples and how they should be considered in the email forensics process.

## Phishing

Phishing is the practice of sending fraudulent emails to users to redirect them to websites that appear legitimate but are in fact malicious. The purpose is usually to steal the user's personal information, banking details, or login credentials.

**Example**: An email that appears to be from a bank asks users to click on a link to update their account information.

**Analysis points**: The legitimacy of the sender's address, the language and tone of the text, linguistic quality, and the security of embedded links should all be checked. Verify SPF, DKIM, and DMARC authentication results in the email headers.

### Example header analysis

In the example below, `Received-SPF` and `Authentication-Results` show that SPF and DMARC validation failed, indicating that the email did not come from the legitimate-looking domain `wellknownbank.com`. The support address in the `From` header impersonates a legitimate bank, but the email was actually sent from a spoofed source on the `mail.illegitimateserver.com` domain, and the `Message-ID` header points to that server.

<img width="2344" height="926" alt="image" src="https://github.com/user-attachments/assets/dafe33b7-6e81-45c6-86c8-ba4204d98497" />

These discrepancies are important clues for identifying a phishing attempt.

## Spear phishing

Spear phishing is a personalized phishing attack that targets a specific individual or organization. The aim is to gain the victim's trust by using information that has been previously gathered about the target (reconnaissance via social media, LinkedIn, company websites, or data breaches).

**Example**: An email pretending to be from the CEO of a company instructs the finance department to make an urgent payment to a new vendor or account.

**Analysis points**: Any targeting of personal data, custom content, and context-specific details should be carefully considered. Verify the sender's identity through out-of-band communication (phone call, in-person confirmation) before acting on unusual requests.

## Spam

Spam refers to large amounts of unsolicited or junk email, often containing advertising and sometimes malware or phishing links.

**Example**: Emails offering free products, pharmaceutical promotions, fake competitions, or lottery winnings.

**Analysis points**: Look out for unsolicited content, signs of bulk sending (generic greetings, mass recipient lists), and repetitive message patterns. Analyze email headers and send times to detect mass campaigns or botnet activity.

## Malware

Malware-laden emails contain malicious software delivered via email attachments or links. This category includes viruses, Trojans, ransomware, spyware, and other malicious programs.

**Example**: An employee opens an email attachment that appears to be a work-related invoice or HR document, and their computer gets infected with ransomware that encrypts files and demands payment.

**Analysis points**: Watch out for malicious attachments (executables, Office documents with macros, PDFs with embedded scripts), suspicious file extensions, and links to payload delivery sites. Use static and dynamic analysis techniques to monitor attachment behavior and detect malicious payloads before they execute on production systems.

## Business Email Compromise (BEC)

BEC is a sophisticated form of financial fraud that involves the impersonation or compromise of executive or administrator accounts. Attackers use compromised or spoofed accounts to authorize fraudulent wire transfers, payroll changes, or sensitive data requests.

**Example**: Fraudulent payment instructions are sent to suppliers or the finance department after the email account of a company's CFO or CEO is compromised.

**Analysis points**: Authorization requests and urgent, unusual financial requests in the content are red flags. Check the identity of the sender (verify email address, authentication results), consistency of communication style with earlier emails, and validate unusual requests through secondary channels (phone call, in-person confirmation). Look for subtle changes in email addresses or display names.

### Example header analysis

In the example below, the `From` and `Reply-To` headers show the email address of the company's CEO, but the `Received` and `Message-ID` headers reveal that the email was actually sent from a different server (`externalmailserver.com` and IP `203.0.113.5`).

<img width="2528" height="900" alt="image" src="https://github.com/user-attachments/assets/7707f685-a282-44ab-ba81-ec86e599a04e" />

The `Received-SPF` and `Authentication-Results` headers confirm that the sender has permission to send messages from the domain `yourcompanydomain.com`. However, this could be a sign that the attacker is manipulating SPF records and other authentication protocols, or that the legitimate account has been compromised and is being used from an external location.

## Email spoofing

Spoofing is the practice of deceiving the recipient by forging the sender's email address or other header information to make the email appear to come from a trusted source.

**Example**: An attacker can request sensitive information from employees by sending an email impersonating a company executive, IT department, or trusted vendor.

**Analysis points**: Check the `From`, `Return-Path`, and `Reply-To` headers, as well as the IP address and domain details in the `Received` headers, to identify spoofing indicators. Validate SPF, DKIM, and DMARC results.

### Example header analysis

In this example, the email address of a trusted company appears in the `From` and `Reply-To` fields. However, the `Received` field shows that the email came from a different server (`mailserver.example.com` and IP `192.0.2.1`). This is an indication that the true sender address is being masked and that the email is potentially being used for spoofing purposes.

<img width="1444" height="588" alt="image" src="https://github.com/user-attachments/assets/e5036fe8-ce85-4f7d-a56f-358b07ea1e4b" />

In reality, the `From` and `Reply-To` addresses are likely spoofed. The real sender address or infrastructure can be revealed by analyzing the IP address or server name in the `Received` field and comparing it to the domain's legitimate mail infrastructure (MX records, SPF policy).

## Domain impersonation

Domain impersonation is the act of imitating a domain name that is real and trusted. Attackers can mislead recipients by slightly altering the domain name (typosquatting), using a similar-sounding name, or registering lookalike domains with different TLDs.

**Example**: Attackers can mislead users by using `examp1e.com` (with the number "1" instead of the letter "l") instead of `example.com`, or `example-secure.com` instead of `example.com`.

**Analysis points**: URLs and sender domains that look similar but contain different characters (homograph attacks, character substitution, extra hyphens, different TLDs) should be carefully examined to spot subtle changes. Use DNS queries and WHOIS information to verify domain registration dates, ownership, and legitimacy. Newly registered lookalike domains are a strong indicator of impersonation.

## Credential harvesting

Credential harvesting involves creating emails or landing pages designed to steal usernames, passwords, and other authentication credentials from victims.

**Example**: A phishing email that tricks a user into entering login details via a fake login form that mimics a legitimate service (Microsoft 365, Google Workspace, banking portal).

**Analysis points**: Forms and login page links should be checked carefully. Verify the security status and authenticity of websites targeted by emailed URLs (certificate validity, domain ownership, URLscan/VirusTotal results). Look for URL redirects, obfuscation, or shortened links that hide the true destination.

## Whaling

Whaling is a type of spear-phishing that specifically targets senior executives, board members, or other high-value individuals. The goal is to commit large-scale fraud, steal sensitive data, or gain access to privileged accounts.

**Example**: A CEO receives an email claiming to be from the company's legal department or external counsel. The email asks for sensitive case information, board meeting minutes, or approval for a confidential transaction.

**Analysis points**: Tailored and detailed content aimed at senior executives should be monitored closely. Emails with unusual sender information, urgent requests, and appeals to authority or confidentiality should be highlighted and verified through out-of-band communication.

## Extortion

Extortion emails contain ransom demands or threats. Attackers may threaten users to disclose sensitive information, publish compromising data, or lock down systems with ransomware unless payment is made.

**Example**: A user receives an email containing a threat to disclose their personal information, browsing history, or alleged compromising material in exchange for money (often in cryptocurrency).

**Analysis points**: Threatening content and ransom demands should be detected and flagged. Requests that include personal user information (often obtained from breaches or OSINT) and are made under time pressure should be analyzed. Verify whether any actual breach or compromise occurred, or if the threat is based on bluffing or publicly available breach data.

## Brand impersonation

Brand impersonation is a form of forgery that uses the name, logo, and visual style of a well-known brand. Such emails are often designed to look like the brand's official communications to exploit user trust.

**Example**: An email pretending to be from a popular technology company (Apple, Microsoft, Amazon) may ask users to verify their account, update payment information, or claim a prize.

**Analysis points**: Brand logos, color schemes, communication styles, and sender domains may be mimicked. Visuals and content should be checked against the brand's official sources (legitimate domains, official communication templates) and verified. Look for subtle differences in logos, fonts, or domain names.

## Data exfiltration

Data exfiltration is the unauthorized transfer of sensitive data outside the organization. Leaking confidential information via email (intentionally or due to compromise) falls into this category.

**Example**: An employee emails confidential company project details, customer data, or intellectual property to a competitor, personal account, or external recipient.

**Analysis points**: Check email attachments and text content for unauthorized disclosure of confidential data. Monitor for data leaving the organization through email channels, especially to external or personal email addresses. Use Data Loss Prevention (DLP) tools and email gateway filtering to detect and prevent exfiltration. Investigate the context (user behavior, account compromise, insider threat indicators).
