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
- **Evidence:** **<img width="1522" height="657" alt="image" src="https://github.com/user-attachments/assets/975be726-74ba-4e68-af80-72d1ac787e4a" />


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
- **Evidence:** <img width="604" height="263" alt="image" src="https://github.com/user-attachments/assets/f60132b4-bc15-4763-a6bc-78acfe2f8e19" />


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
- **Evidence:** <img width="1535" height="757" alt="image" src="https://github.com/user-attachments/assets/3ad90604-5f8f-4451-8c1e-ef90f435b7ea" />
<img width="837" height="622" alt="image" src="https://github.com/user-attachments/assets/9b4e5500-9558-4644-8df0-c71dcdc8b0fc" />

## What I Learned

This project showed me how much technical evidence sits behind a phishing email that most users never see. Comparing a fully-authenticated legitimate email against the phishing samples made the contrast obvious — a clean SPF/DKIM/DMARC pass versus a complete authentication failure is one of the clearest, most reliable signals available, far more dependable than just "does this look suspicious" at a glance. I also learned that sender authentication is only part of the picture: Case 3 showed how an anonymous mailer service like emkei.lv can be used to spoof a sender address entirely, and how attackers use attachments instead of links specifically to dodge basic link-scanning tools. Going forward, I'll treat urgency language and mismatched sender domains as a prompt to check headers directly, rather than relying on instinct alone.

## Disclaimer
All samples were inspected safely (headers and links only — no attachments opened,
no links clicked directly) using TryHackMe's provided sample files and read-only
analysis tools, for educational purposes.
