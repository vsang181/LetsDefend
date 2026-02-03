# Dynamic Analysis

URLs and attachments in an email should be treated as untrusted until proven otherwise. To avoid exposing your host to drive-by downloads, credential harvesting, or browser/plugin exploitation, perform analysis in an isolated environment and observe what the URL/file actually does at runtime.

## Why use a sandbox?

Dynamic analysis (sandboxing) lets you execute or open suspicious content in a controlled environment and monitor behavior such as process creation, filesystem changes, registry modifications, and network connections (e.g., callbacks to external servers).
This reduces the risk of infecting your own workstation while still allowing you to capture indicators of compromise (IOCs) and understand the attack chain.

<img width="1024" height="498" alt="image" src="https://github.com/user-attachments/assets/6006f8c6-38f6-433b-aba3-8560c6d62535" />

## Safe URL checking (browser isolation)

If you need a quick look at a suspicious link, using an online/isolated browser can help you avoid visiting the page directly from your endpoint.  
The trade-off is visibility and execution: some environments won’t allow you to download and detonate files, which can limit analysis if the phishing flow includes a payload dropper.

<img width="1024" height="242" alt="image" src="https://github.com/user-attachments/assets/192f5293-67f4-4fcd-8bc3-2f30c2581e13" />

Before opening a URL, inspect it for embedded information (query parameters, path fragments, tracking IDs). If a URL contains user-identifying values (e.g., `email=user@company.com`), simply visiting the link may confirm to the attacker that the address is valid, even if the user never enters a password.

<img width="633" height="157" alt="image" src="https://github.com/user-attachments/assets/4d881a4f-7a6a-42b9-a14f-5bb7f7a0eb0b" />

**Good practice:** replace sensitive parameters (email, username, tokens) with dummy values before visiting the link in a sandboxed browser to avoid “valid user” confirmation.

## Common sandbox services

You can choose one or more sandbox products depending on what you’re analyzing (URLs vs. files, Windows vs. Office docs, detonation depth, reporting needs). Some commonly used sandboxes include:

- VMRay
- Joe Sandbox
- ANY.RUN
- Hybrid Analysis (Falcon Sandbox)
- urlscan.io (useful for dynamic URL behavior, redirects, and page artifacts)

## Timing and evasion considerations

Malware and phishing payloads may delay execution (sleep loops, staged downloads, user-interaction requirements) to evade quick scans. Allow enough runtime and review behavioral timelines before concluding that a file or URL is benign.

## No links/attachments doesn’t mean safe

An email can still be malicious without obvious URLs or attachments. Attackers may embed content in ways that bypass simplistic detection (for example, sending a malicious payload as an “image” or using HTML/QR-based redirection), so always correlate header findings, body content, and observed behavior during dynamic analysis.
