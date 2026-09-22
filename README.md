# Phishing-Email-Analysis
A project involved analyzing real emails to identify technical indicators of phishing

# Phishing Email Analysis

## Overview
This project involved analysing real emails to identify technical indicators of
phishing — comparing a legitimately authenticated email against samples of actual
phishing attempts — using header analysis and safe URL inspection. The goal was to
practice email/threat analysis skills relevant to SOC and security analyst roles,
and to build the ability to explain, in plain terms, how to spot a phishing attempt.

## Objectives
- Identify technical indicators of phishing (headers, links, spoofing)
- Practice safe inspection of suspicious emails without executing content
- Compare a legitimate, fully-authenticated email against real phishing samples
- Document findings in a way that helps others recognise similar attempts

## Tools Used
- **MXToolbox Email Header Analyzer** — parsing raw headers and checking SPF/DKIM/DMARC
- **urlscan.io** — safely inspecting suspicious URLs without visiting them
- **TryHackMe** — source of real sample phishing `.eml` files for safe analysis

## Methodology
1. Collected sample emails: one legitimate newsletter email (as an authentication
   baseline) and several real phishing samples sourced from TryHackMe's phishing
   analysis rooms.
2. Extracted the raw headers from each email and ran them through the MXToolbox
   Email Header Analyzer, focusing on SPF, DKIM and DMARC results and alignment.
3. Reviewed the relay/routing path shown for each email to check for unusual hops
   or blacklisted servers.
4. Where emails contained links, inspected the destination safely using urlscan.io
   rather than visiting them directly.
5. Compared the authentication results of the legitimate baseline email against
   each phishing sample to identify clear, explainable differences.

## Case Studies

### Case 1: Legitimate Baseline — Beehiiv Newsletter Email
- **Claimed to be:** A newsletter/marketing email sent via the Beehiiv platform
- **Authentication results:** SPF Authenticated ✓, SPF Alignment ✓, DKIM
  Authenticated ✓, DKIM Alignment ✓, DMARC Compliant ✓ — every check passed
- **Relay path:** Clean, direct path (`mail.beehiiv.com` → Google's mail servers),
  delivered in about 1 second, no blacklist hits
- **Why this matters:** This was used as a baseline to show what a properly
  authenticated, legitimate email looks like, so the phishing samples below can be
  meaningfully contrasted against it
- **Evidence:** *[Insert your MXToolbox screenshot here]*

### Case 2: Phishing Sample — Failed Authentication (TryHackMe)
- **Claimed to be:** *[Fill in what this email pretended to be — e.g. a bank, a
  service provider, an internal company email]*
- **Authentication results:** DMARC Compliant ✗ (No DMARC record found), SPF
  Authenticated ✗, SPF Alignment ✗, DKIM Authenticated ✗, DKIM Alignment ✗ —
  every check failed
- **Red flags found:** The sending domain had no DMARC record at all, and neither
  SPF nor DKIM could be verified — meaning the server that sent this email was
  never authorised to send mail on behalf of the domain it claims to be from
- **How to spot it:** A completely failed authentication result like this is one
  of the clearest technical signs of a spoofed sender; compared to Case 1 above,
  the contrast is stark
- **Evidence:** *[Insert your MXToolbox screenshot here]*

### Case 3: Phishing Sample — "URGENT: ParrotPost Account Update Required" (TryHackMe)
- **Claimed to be:** An account security notification from "ParrotPost", sent from
  `no-reply@postparrot.thm` to Paul Feathers, warning of "unusual activity" and
  demanding immediate action to "verify account information" via an attached
  webpage
- **Authentication results:** DMARC Compliant ✗ (No DMARC record found), SPF
  Authenticated ✗, SPF Alignment ✗, DKIM Authenticated ✗, DKIM Alignment ✗ —
  every check failed
- **Red flags found:**
  - Routed through **emkei.lv**, a free "anonymous mailer" service that lets
    anyone send email with a freely-chosen, spoofed sender address — a strong
    technical red flag on its own, independent of the failed SPF/DKIM/DMARC
  - Sender domain (`postparrot.thm`) is a mismatched, informal variant of the
    brand name it claims to represent ("ParrotPost")
  - Classic urgency language ("URGENT", "immediate action required", "unusual
    activity") designed to pressure quick, unthinking action
  - Instead of a clickable link, the email carries an **attached HTML file**
    ("ParrotPostACTIONREQUIRED...") for the recipient to open — a technique
    often used to dodge link-scanning security tools, since the malicious
    content sits in an attachment rather than a URL
  - Greeting uses an abbreviated, slightly impersonal form of the recipient's
    name ("PFeathers") rather than a natural full name
- **How to spot it:** The combination of a spoofed/unauthenticated sender, a
  mismatched domain, manufactured urgency, and an attachment standing in for
  what would normally be a link are all textbook phishing indicators — any one
  of these alone is suspicious, but together they make this a clear-cut case
- **Evidence:** *[Insert your MXToolbox screenshot and the email screenshot here]*

## What I Learned
*[Once all three cases are filled in with your real findings/evidence, write 3-5
sentences here on what patterns you noticed across the samples, what surprised
you, and how this changes how you'll personally spot phishing attempts going
forward.]*

## Disclaimer
All samples were inspected safely (headers and links only — no attachments opened,
no links clicked directly) using TryHackMe's provided sample files and read-only
analysis tools, for educational purposes.
