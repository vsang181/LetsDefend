# What is an Email Header and How to Read It?

In this lesson we’ll break down what an email header is, how to access it, and how to interpret the most important fields. Header analysis is a core skill for phishing investigations because it helps you validate sender authenticity, trace routing, and spot manipulation attempts.

## What is an email header?

An email header is a block of metadata added to a message as it travels through mail servers (MTAs) and finally to your mailbox. It contains standard fields like `From`, `To`, `Date`, and `Subject`, plus technical fields such as `Return-Path`, `Reply-To`, and multiple `Received` lines that document the delivery path.

<img width="1420" height="881" alt="image" src="https://github.com/user-attachments/assets/9b70f151-6b1e-4a1f-9d8a-0a37601a6083" />

Practically, the header is where you look to answer questions like:

- Who does this email *claim* to be from?
- Which infrastructure actually sent it?
- Did SPF/DKIM/DMARC authenticate successfully?
- Did the message take a suspicious route?

## What does the email header do?

- Identify sender and recipient details (for example via `From`, `To`, `Cc`, `Bcc`).
- Support anti-spam/anti-phishing decisions (mail gateways use header signals + content + reputation).
- Track the route the email took (via `Received` lines and related authentication headers).
- Provide forensic artifacts for incident response (timestamps, sending hosts, IDs, auth results).

<img width="416" height="70" alt="image" src="https://github.com/user-attachments/assets/6c07e8b0-6074-44b6-9ad0-4393c6ba42c3" />

## Important fields (and what to look for)

| Field | What it is | What to check during analysis |
|---|---|---|
| `From` | Display name + *claimed* sender address shown to the user. | This can be spoofed; don’t treat it as proof of origin. Compare with `Reply-To`, `Return-Path`, and authentication results. |
| `To`, `Cc`, `Bcc` | Intended recipients. (`Bcc` is not visible to other recipients.) | Identify whether the email was broadly sprayed or narrowly targeted (single user, specific department). |
| `Date` | Timestamp set by the sender’s client/MTA. | This can be manipulated; correlate with `Received` timestamps for a reliable timeline. |
| `Subject` | Email topic line. | Useful for campaign scoping because senders and SMTP IPs may rotate while subjects stay similar (or vice versa). |
| `Reply-To` | Where replies should be sent (if present). | A common phishing trick is a legitimate-looking `From` but a malicious `Reply-To` for credential harvesting or invoice fraud. |
| `Return-Path` | Envelope sender used for bounces (set by MTAs during delivery). | **Not the same as** `Reply-To`. Mismatches between `From` and `Return-Path` can be normal (mailing lists) or suspicious (spoofing). |
| `Message-ID` | Unique identifier assigned by the sending system. | The domain portion can hint at the sending platform; odd formats or mismatched domains can be a weak signal of forgery. |
| `MIME-Version` | Indicates MIME formatting is used. | Normal for modern mail; MIME enables attachments and non-ASCII content. |
| `Content-Type` / `Content-Transfer-Encoding` | How the body/attachments are encoded and represented. | Look for suspicious attachment types, HTML-heavy content, and unusual encodings (base64/quoted-printable) that may hide indicators. |
| `Received` | One line per hop showing which mail server received the message from which host. | Read **bottom to top** to approximate origin. Identify the first “external/untrusted” hop and the source IP/host. |
| `Authentication-Results` | Results evaluated by the receiving domain/gateway. | Look for `spf=pass/fail`, `dkim=pass/fail`, `dmarc=pass/fail` plus the domain evaluated (alignment matters). |
| `DKIM-Signature` | Cryptographic signature header added by the sending domain. | Presence alone isn’t enough—check that verification is `pass` in `Authentication-Results`. |
| `Received-SPF` (or similar) | SPF evaluation details (implementation varies). | Helpful when `Authentication-Results` is missing or when you want more detail on why SPF passed/failed. |
| `X-Spam-Status` / `X-Spam-Score` | Spam scoring (often from tools like SpamAssassin; names vary). | Useful triage signal; treat as supporting evidence, not a final verdict. |
| `ARC-Seal` / `ARC-Authentication-Results` | Auth chain for forwarded mail (Authenticated Received Chain). | Helps explain why SPF/DKIM may fail after forwarding while still being legitimate. Not always present. |

## How to read the `Received` chain (route tracking)

`Received` headers are typically listed in reverse order of processing:

- The **top** `Received` line is usually the last hop before it reached the mailbox provider or gateway.
- The **bottom** `Received` line is closest to where the message **originated** (though it can be partially forged; forged `Received` lines are harder but not impossible in some scenarios).

A practical workflow:

1. Start at the **bottom-most** `Received` line and move upward.
2. Identify the first hop where the message enters the recipient’s trusted mail infrastructure (this boundary often separates “external” from “internal”).
3. Extract the source IP/hostname from the earliest external hop and validate it (WHOIS, ASN/org ownership, known cloud sender ranges).
4. Correlate routing with authentication:
   - SPF checks the connecting IP against the sender domain’s SPF policy.
   - DKIM verifies the message signature.
   - DMARC evaluates alignment between the visible `From` domain and SPF/DKIM results.
5. Cross-check timestamps: large gaps between hops may indicate queueing, relays, or suspicious rerouting.

## How to access an email header

### Gmail

1. Open the email in question.
2. Click the three dots menu in the top-right (`...`).
3. Click **Download message**.
4. Open the downloaded `.eml` file in a text editor (or any email client that can display “raw” headers).

<img width="1229" height="777" alt="image" src="https://github.com/user-attachments/assets/660db6e7-5250-4da0-891e-85ab1ba19c93" />

> Tip: Some Gmail views also provide “Show original,” which displays raw headers + authentication results in the browser.

### Outlook (desktop)

1. Open the email in question.
2. Go to **File → Info → Properties**.

<img width="1016" height="734" alt="image" src="https://github.com/user-attachments/assets/d80cf4a9-2b23-4cc5-84b9-4d008ab14a98" />

3. Copy from **Internet headers**.

<img width="1019" height="732" alt="image" src="https://github.com/user-attachments/assets/2be82f86-3748-4813-a23b-827dc24677e1" />

If you are using Outlook on the web or a different client, the location may differ, but the goal is always the same: view the full “raw” headers (not just the simplified `From/To/Subject` UI).
