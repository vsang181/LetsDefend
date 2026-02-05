# Email Body Analysis

Email body analysis is the process of examining the main content of an email message, including text, rich text formats such as HTML, images, links, and other media types. The purpose of the analysis is to ensure the security of the information in the body and to identify any malicious, deceptive, or suspicious content.

Analysis of the information in the message body is fundamental in email forensics, as the body often contains the social engineering hooks, malicious links, embedded scripts, or encoded payloads used in phishing, malware distribution, and fraud campaigns.

This lesson outlines what you need to know to understand the information in the message body and identify potential threats.

## Content extraction

**What to look for**: All text, images, HTML, and other multimedia elements should be extracted from the body of the email. All of this content should be converted into an accessible and processable format for further analysis.

**Main objective**: To securely isolate all email content and make it available for detailed forensic examination without risking infection or alteration.

**Note**: Email attachments can also be exported during this process and should be handled separately for static and dynamic analysis.

### Using Autopsy for content extraction

<img width="2496" height="1068" alt="image" src="https://github.com/user-attachments/assets/c6ab7b72-bc6c-44e5-a874-15ec52e734ce" />

When analyzing a sample `.eml` file with Autopsy, the following items can be extracted and reviewed:

- **Communication Accounts tab**: Displays email accounts extracted from the message, including sender and recipient addresses.

<img width="2522" height="1114" alt="image" src="https://github.com/user-attachments/assets/baffe6e6-cda5-4829-9b04-c471e37c6aed" />

- **Metadata tab**: Contains metadata information about the `.eml` file, such as file creation/modification timestamps, file size, and embedded properties.

<img width="2516" height="1110" alt="image" src="https://github.com/user-attachments/assets/3b5b323a-b5a8-4787-b96a-19afea8de243" />

- **Keyword Hits tab**: Shows how often specific keywords (e.g., email addresses, domains, suspicious terms) appear in the message.

<img width="2506" height="1064" alt="image" src="https://github.com/user-attachments/assets/b1448bd5-9356-4f3f-b39a-14ef3735c812" />

- **Score feature**: Autopsy's scoring feature allows you to identify and investigate whether the `.eml` file contains "bad" or "suspicious" elements based on indicators of compromise (IOCs), known malicious patterns, or user-defined rules.

<img width="1876" height="1068" alt="image" src="https://github.com/user-attachments/assets/49b2b611-f72e-4f6c-9311-e345dbd149bd" />

The Autopsy tool is a valuable resource for content extraction as part of email forensics, automating much of the parsing and indexing work.

## Text analysis

**What to look for**: The linguistic structure of the email text, the phrases used, keywords, and anomalies in language use should be examined, as they are important in identifying attempted fraud such as social engineering or phishing attacks.

**Objective**: To verify the information given in the text and determine if it contains any misleading, deceptive, or threatening information designed to manipulate the recipient.

### Text analysis steps

#### Linguistic structure analysis

- **Language and tone**: Is the language used formal, professional, and accurate, or is it informal, vague, and inaccurate? The use of informal, imprecise, or overly generic language is a common feature of phishing emails.
- **Urgency**: Does the email request urgent action or create a sense of panic? Phishing emails often use phrases that force the recipient to take immediate action without careful thought (e.g., "Act now or your account will be suspended").

#### Phrases and keywords

- **Threatening phrases**: Does the message contain threatening language, such as "Your account will be suspended" or "Immediate action required"? Such phrases are designed to provoke an emotional response (fear, panic) and cause the recipient to act impulsively without verifying the sender's legitimacy.
- **Common phishing keywords**: Words and phrases such as "verification," "urgent," "suspended," "update your information," "confirm your identity," and "click here immediately" should be monitored as potential red flags.

#### Anomalies in language use

- **Grammatical errors**: Does the email contain grammar mistakes, awkward phrasing, or spelling errors? Phishing emails often do not fully adhere to grammar rules, especially if they are translated or mass-produced by attackers with limited language proficiency.
- **Link analysis**: Does the link text match the actual destination URL? When you hover your mouse over a link, does the displayed URL differ from the visible anchor text? This is a common method used to redirect recipients to malicious or phishing sites while displaying a legitimate-looking link.

#### General assessment

- **Sender's email address**: Does the email come from an expected and recognizable address? Could it be a fraudulent or spoofed address? Check for lookalike domains, typosquatting, or free email providers used for impersonation.
- **Context evaluation**: Is the message consistent with the recipient's status or past activities? For example, a verification request from a service the recipient has never used may raise suspicion. Does the timing or content make sense in the context of the recipient's role or recent interactions?

### Example: analyzing a suspicious email

Consider the following email example:

<img width="1332" height="632" alt="image" src="https://github.com/user-attachments/assets/60a79333-6792-40ad-a05f-5f8225033bb1" />

**Subject**: "Account Need Update"

**Issues**:

- The subject "Account Need Update" is grammatically incorrect. The correct phrase would be "Account Needs Update" or "Account Update Needed." In English, verbs with a singular subject usually take the suffix "-s." Such mistakes can indicate that the message is not from an official source.

**Greeting**: "Dear user"

**Issues**:

- "Dear user" is informal, generic, and impersonal. More formal and credible emails usually use the recipient's name ("Dear [First Name]" or "Dear [Full Name]").

**Body**: "Your account will be temporarily suspended"

**Issues**:

- This is threatening language often used in phishing emails to pressure the recipient into acting without thinking. Formal and trustworthy messages usually use clear, non-threatening language and explain what the recipient needs to do without creating panic.

**Body**: "Due to update in systems"

**Issues**:

- The grammatical structure is inadequate and incorrect. The phrase should be "Due to an update in our systems" or "Due to a system update." The missing article and awkward phrasing suggest the message is not professionally written.

**Link text**: "[Verify Account]"

**Issues**:

- Link text such as "[Verify Account]" does not give the user clear information about where they are being directed. Trustworthy emails typically use clear and comprehensible link text (e.g., "Click here to verify your account on our official website") and display the full URL when hovered over.

This is a good example of how email text should **not** be constructed. Linguistic and structural issues can help recipients and analysts spot fake emails, which are often used in phishing and other scams.

## Link analysis

**What to look for**: Evaluate the security status of all URLs in the email body. Check whether these URLs lead to malicious, fraudulent, or compromised websites. The true destinations of shortened links (e.g., bit.ly, tinyurl) should be clarified by expanding them safely.

**Objective**: To detect potentially malicious links and protect users from links that lead to phishing pages, malware distribution sites, or credential harvesting forms.

### Using Autopsy for link extraction

You can use the **Keyword Search** feature in Autopsy to extract all URLs from an `.eml` file.

<img width="3274" height="906" alt="image" src="https://github.com/user-attachments/assets/6c2359c7-00c7-4af8-923b-080573d23edf" />

Steps:

1. Open the case and navigate to the `.eml` file.
2. Use the **Keyword Search** feature to search for patterns such as `http://`, `https://`, or common URL structures.
3. Review the extracted URLs in the **Keyword Hits** tab.
4. Once URLs are detected, perform analysis on each URL using threat intelligence platforms (VirusTotal, URLhaus, urlscan.io) to obtain Indicators of Compromise (IOCs) and reputation data.

## HTML and embedded code analysis

**What to look for**: Embedded JavaScript, CSS, and other active code in HTML content should be carefully examined, as these pieces of code may contain malicious scripts, be designed to collect user information (e.g., keylogging, form hijacking), or exploit vulnerabilities in the email client or browser.

**Objective**: To determine whether embedded codes and scripts are malicious or not, and to assess whether the email content is safe for users to view or interact with.

### Analyzing encoded email attachments

If you examine an email attachment or inline content, you may find that it is encoded using Base64 or other encoding schemes. Encoded data like this needs to be decoded first to reveal the actual content.

<img width="2598" height="2132" alt="image" src="https://github.com/user-attachments/assets/f6378cb1-cc4e-4f71-b329-c7c1e09c61c2" />

**Using Autopsy to decode and analyze encoded **

1. Click on the appropriate `.eml` file in Autopsy.
2. Navigate to the **Attachments** tab to view embedded files or encoded content.

<img width="2456" height="1168" alt="image" src="https://github.com/user-attachments/assets/c864e77d-23ef-471a-baa2-a7b1c5827110" />

3. Right-click on the attachment and select **View file in directory** or select the `.eml` file under **LogicalFileSet** to list the files it contains.
4. View and export the decoded content as text for further analysis.

<img width="2402" height="1066" alt="image" src="https://github.com/user-attachments/assets/57dd2087-7534-46f6-8915-3087de9a9b48" />

Alternatively, you can use online tools (such as CyberChef, base64decode.org) to manually decode Base64-encoded data. Always decode in a safe environment to avoid executing malicious payloads.

### What to look for in decoded content

Once decoded, examine the content for:

- Malicious scripts (JavaScript, VBScript, PowerShell commands)
- Embedded URLs or redirect chains
- Obfuscated code or suspicious patterns
- Hidden iframes or form elements designed to capture credentials
- Executable file signatures (e.g., MZ header for PE files) or macro-enabled documents

## Conclusion

These procedures provide a detailed understanding of email content and help identify potential threats during forensic analysis. Each phase is important to detect and prevent email attacks effectively. By systematically analyzing text structure, links, HTML content, and encoded data, analysts can uncover phishing attempts, social engineering tactics, and malicious payloads embedded in email bodies.

In the next lesson, we will discuss **Email Attachment Analysis**, which focuses on safely examining files attached to emails for malware, exploits, and other dangerous content.
