# Information Gathering

## Email spoofing

Email protocols (SMTP in particular) do not inherently authenticate the sender's identity, which allows attackers to forge the "From" field and impersonate trusted entities. This technique is called **spoofing**, and it's one of the primary reasons phishing emails appear legitimate at first glance.

To combat spoofing, several email authentication protocols have been developed:

- **SPF (Sender Policy Framework)**: Allows domain owners to publish a DNS TXT record listing authorized IP addresses/servers that can send email on behalf of the domain. Receiving servers query this record and compare it to the sending server's IP.
- **DKIM (DomainKeys Identified Mail)**: Uses cryptographic signatures added to email headers. The sending server signs outgoing messages with a private key, and the receiving server verifies the signature using the public key published in DNS. This proves the message wasn't altered in transit and came from an authorized source.
- **DMARC (Domain-based Message Authentication, Reporting & Conformance)**: Builds on SPF and DKIM by instructing receiving servers what to do if authentication fails (quarantine, reject, or allow) and provides a reporting mechanism back to the domain owner.

Some mail clients and gateways automatically check these records and flag or reject unauthenticated messages. However, adoption is not universal, and misconfigurations or legitimate forwarding scenarios can sometimes cause false positives, which is why these protocols remain optional in many environments.

### Manual spoofing verification

To manually check whether an email is spoofed:

1. **Extract the SMTP sender IP** from the email headers (look for `Received:` headers; the first external IP in the chain is typically the originating server).
2. **Query DNS records** for the sender's domain using tools like [MXToolbox](https://mxtoolbox.com) to retrieve SPF, DKIM, DMARC, and MX records.
3. **Compare the sending IP** against the domain's SPF policy. If the IP is not listed, SPF fails.
4. **Check DKIM signatures** in the email headers. A missing or invalid signature may indicate tampering or unauthorized sending.
5. **Perform a WHOIS lookup** on the SMTP IP address to determine the owner. Large organizations typically use dedicated mail infrastructure (on-premises or cloud providers like Microsoft 365 or Google Workspace), so the IP owner should align with the claimed sender domain.

<img width="1024" height="291" alt="image" src="https://github.com/user-attachments/assets/44295058-b463-4be2-907d-9d097179fc1e" />

**Important caveat**: Even if SPF/DKIM/DMARC pass, the email is not guaranteed to be safe. Legitimate accounts can be compromised (credential stuffing, password reuse, session hijacking), allowing attackers to send malicious emails from authenticated addresses. Always evaluate content and behavior alongside authentication checks.

## Email traffic analysis

When investigating a phishing campaign, analysts typically search the email gateway or SIEM for patterns using several key indicators:

| Parameter | Purpose | Example |
|---|---|---|
| Sender address | Identify the exact "From" field used | `info@letsdefend.io` |
| SMTP IP address | Track the actual sending server | `127.0.0.1` (example; real campaigns use external IPs) |
| Domain | Broaden the search to catch domain-based campaigns | `@letsdefend.io` |
| Keyword/substring | Detect variations or typosquatting | `letsdefend` (catches `letsdefend@gmail.com`, `letsdefend@hotmail.com`, etc.) |
| Subject line | Useful when sender/IP rotate frequently | "Your account has been suspended" |

In addition to counting affected messages, you should capture:

- **Recipient email addresses**: Helps identify targeted users or departments. If the same users receive repeated phishing attempts, their addresses may be exposed (compromised database, public scraping, paste sites like Pastebin).
- **Timestamps**: Reveals campaign timing and possible attacker time zone. Emails sent during off-hours (relative to the organization's geography) may indicate attackers operating from a different region.

### Reconnaissance and email harvesting

Attackers often collect target email addresses using reconnaissance tools such as:

- **theHarvester** (Kali Linux tool): Scrapes search engines, public repositories (GitHub, LinkedIn), and DNS records to enumerate email addresses and subdomains associated with a target domain.
- **Hunter.io, Phonebook.cz**: Web-based OSINT tools for email discovery.
- **Data breaches and paste sites**: Leaked credentials or email lists from third-party breaches.

To reduce exposure, organizations should avoid publishing employee email addresses on public websites, use contact forms instead of direct mailto links, and monitor paste sites and breach databases for leaked credentials.

### Campaign profiling

By correlating the indicators above, you can begin to profile the attack:

- **Scale**: Number of messages, unique recipients, geographic or departmental targeting.
- **Sophistication**: Use of compromised accounts vs. spoofed addresses, quality of social engineering, payload complexity.
- **Infrastructure**: Distribution of sending IPs (single server, botnet, legitimate but compromised mail servers).
- **Timing**: Time zone analysis, alignment with business hours, coordination with external events (tax season, company announcements).

This information feeds into threat intelligence, incident response prioritization, and long-term defensive improvements (user training, blocklists, policy tuning).
