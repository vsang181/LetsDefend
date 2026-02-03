# Static Analysis

Many users find plain text emails boring, so email clients support HTML to make messages more visually engaging. The downside is that attackers can abuse HTML to disguise malicious URLs behind buttons or “safe-looking” text.

<img width="1024" height="242" alt="image" src="https://github.com/user-attachments/assets/aa5dec01-49d7-4951-9894-ded1185ab4e7" />

A common example is when the text you see in an email differs from the actual destination URL embedded in the link. In many clients, hovering over a link reveals the real destination, which is why mismatches between displayed text and the underlying URL are a strong phishing indicator.

## Link and domain checks

In many phishing campaigns, attackers register (or compromise) domains and use them for a short window before they are blocked or burned. That’s why a **newly registered domain** in an email is often treated as higher risk, and domain age is commonly used as a useful signal in phishing detection.

If an email claims to be from a major brand (e.g., Twitter/X, LinkedIn, Facebook) but the links point to unrelated or newly registered domains, that inconsistency should be treated as suspicious.

## VirusTotal (URLs) and “old results”

VirusTotal is commonly used to check URLs found in emails to see whether security engines or community detections flag them as malicious. If a URL has been analyzed before, VirusTotal may show the previous results instead of performing a fresh analysis immediately, which can be helpful for speed but risky if you assume it reflects the current state of the site.

<img width="1024" height="156" alt="image" src="https://github.com/user-attachments/assets/b7084526-893f-4db3-984e-08315d5a43a8" />

Attackers can take advantage of this by ensuring a domain looks harmless at one point in time, then later hosting phishing content on the same domain. If the previous scan is old, request a **reanalysis/rescan** so you’re not relying on stale results.

## Static vs dynamic analysis (attachments)

Static analysis means evaluating a suspicious file **without executing it**, such as reviewing metadata, structure, strings, and indicators that hint at capability. This can help you understand what a file might do, but it can be time-consuming and may miss behavior that only appears at runtime.

Dynamic analysis focuses on observing behavior by executing the sample in a controlled environment (e.g., sandboxing) to capture actions like process creation, persistence attempts, network callbacks, and dropped files. Dynamic analysis is often faster for confirming what a sample actually does, and it can reveal behaviors that static techniques don’t easily expose.

## IP reputation checks (SMTP source)

For phishing investigations, checking the SMTP sending IP’s reputation can add context about whether the message likely came from known bad infrastructure, a botnet, or a compromised server.

Common reputation sources include:

- **Cisco Talos Reputation Center** for IP/domain reputation lookups and categorization.
- **AbuseIPDB** to see whether an IP has been reported for abusive activity and to review historical reports.
- **VirusTotal** to check whether an IP/URL has been associated with malicious activity in prior submissions and detections.

<img width="1024" height="453" alt="image" src="https://github.com/user-attachments/assets/b0eb924c-1523-4ad1-923c-07593b97f548" />

If the SMTP IP is consistently reported/blacklisted across multiple sources, treat it as a strong supporting indicator that the email is malicious or originated from compromised infrastructure (but still correlate with the full email context: headers, content, URLs, and attachments).
