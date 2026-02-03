# Email Header Analysis

In the previous lesson, we covered what phishing is and what email header information contains. When you suspect an email is phishing, header analysis helps you validate the sender’s infrastructure and spot inconsistencies that indicate spoofing or impersonation.

## Key questions to answer

- Was the email sent from the correct SMTP infrastructure for the claimed domain?
- Do the `From` and `Return-Path` / `Reply-To` fields match (and if not, is there a legitimate reason)?

## 1) Was the email sent from the correct SMTP server?

Start by reviewing the `Received` headers to understand the route the email took. Each mail server that handles a message typically adds a `Received` line, and these lines are generally listed in reverse chronological order (newest at the top, earliest/origin near the bottom).[1][2]

<img width="1137" height="113" alt="image" src="https://github.com/user-attachments/assets/53234413-8221-45b7-9f94-72c7cc455c73" />

In the sample from this lesson, the email originated from the server with the IP address `101.99.94.116` (as observed in the `Received` chain). Next, compare that sending infrastructure against what the sender domain *should* be using.

A practical way to validate this is to look up the domain’s MX records (the mail exchangers responsible for receiving email for that domain) using a tool such as MXToolbox.

<img width="1230" height="568" alt="image" src="https://github.com/user-attachments/assets/f1eec1e6-84f3-4d92-a94a-e2b9f00a40a6" />

If the domain’s MX records point to a well-known provider (e.g., Google Workspace) but the message route shows an unrelated host/IP (e.g., `emkei.cz` / `101.99.94.116`), that mismatch is a strong indicator the email did not originate from the legitimate mail infrastructure and may be spoofed.

## 2) Are `From` and `Return-Path` / `Reply-To` consistent?

In most normal business email flows, replies go back to the same identity that appears in `From`. If `Reply-To` is different, the user may end up replying to an attacker-controlled inbox even though the visible sender looks legitimate.

<img width="353" height="32" alt="image" src="https://github.com/user-attachments/assets/cb320610-6bfb-488b-a1b5-503d44b08a09" />

To interpret these fields correctly:

- `From` is the visible author/sender shown by mail clients.
- `Reply-To` is set by the sender’s software and tells the mail client where to send human replies when the user clicks “Reply.”
- `Return-Path` (envelope-from) is used for bounces/non-delivery reports and is added/recorded by the receiving mail system during SMTP handling.

### Example phishing pattern (Reply-To abuse)

A common trick is to send an email from a throwaway address (Gmail/Hotmail/etc.) while setting `Reply-To` to a legitimate-looking address (e.g., the real employee at a trusted company). This makes the “reply target” look believable and reduces suspicion when the victim responds, even though the original sender was not legitimate.

<img width="447" height="130" alt="image" src="https://github.com/user-attachments/assets/7487f251-75b8-4b10-b69b-ef5bd4c67524" />

In the next lesson, you can combine header findings with body analysis (URLs, attachment types, language tactics) to confirm whether the email is part of a phishing attempt.

