# Additional Techniques

Attackers frequently abuse legitimate SaaS and well-known domains to increase trust and bypass simplistic filtering.

## Cloud storage links (Google/Microsoft)

Attackers may use Google Drive, OneDrive, or SharePoint links that look harmless to trick users into downloading malware (or opening a file that triggers a second-stage download).  
When reviewing these links, focus on the actual file type and delivery flow (preview vs direct download), the final resolved URL after redirects, and whether the content matches the email’s story (invoice, HR doc, “secure message”, etc.).

## Free subdomain and site-builder abuse

Services that allow free subdomains (or “hosted app” domains) are commonly abused to make phishing pages look more credible (e.g., a URL ending with a recognizable parent domain).  
A key pitfall is that WHOIS typically applies to the registered/apex domain, not the attacker-controlled subdomain, so “WHOIS shows Microsoft/WordPress/Wix” does not prove legitimacy.

Practical analysis tips:

- Treat the parent domain reputation as a weak signal; validate the *subdomain* and the page behavior.
- Check the full redirect chain and landing page domain(s).
- Look for brand impersonation patterns: mismatched login branding, unusual paths, or credential collection forms on unrelated hosted pages.

## Form applications (Google Forms, Microsoft Forms)

Attackers may use form services to collect credentials, MFA tokens, or personal data without hosting their own phishing infrastructure.  
Because the domain belongs to a reputable provider, these links may evade basic blocking and can mislead analysts who only validate the parent domain.

What to check:

- What data is being requested (passwords, recovery email/phone, OTPs, payment details).
- Whether the form is used as a first step to “confirm identity” and then redirects to a phishing site.
- Any embedded prefilled parameters in the URL (e.g., an email field), which can validate a target just by clicking the link.
