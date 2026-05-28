# 🧰 Tools Reference Guide

A complete reference for every tool used in phishing email analysis — what it does, when to use it, and how to get the most out of it.

---

## 📧 Email Header Analysis

### MXToolbox Email Header Analyser
**URL:** https://mxtoolbox.com/EmailHeaders.aspx
**What it does:** Parses raw email headers into a readable format, highlights SPF/DKIM/DMARC results, flags suspicious routing hops, and shows the originating IP with geolocation.
**When to use:** First tool to open for every phishing investigation — paste the full raw headers.
**How to get headers:**
- Outlook: Open email → File → Properties → Internet headers
- Gmail: Open email → three dots → Show original
- Office 365: Message trace in Security & Compliance Center

---

### Google Admin Toolbox — Messageheader
**URL:** https://toolbox.googleapps.com/apps/messageheader/
**What it does:** Analyses email routing delays between hops — useful for detecting anomalous routing paths that suggest spoofing or relay abuse.
**When to use:** When MXToolbox results are unclear or you need to verify routing path timing.

---

## 🔗 URL & Domain Analysis

### VirusTotal
**URL:** https://www.virustotal.com
**What it does:** Scans URLs, domains, IPs, and file hashes against 90+ antivirus and threat intelligence engines simultaneously.
**When to use:** Every URL, domain, IP, and file hash encountered during analysis.
**Tips:**
- Check the **Community** tab for analyst comments
- Check **Relations** tab to see connected domains and IPs
- A low detection ratio (1-2 engines) may still be malicious — check other tools
- Search by hash first before uploading a file (preserves OpSec)

---

### URLScan.io
**URL:** https://urlscan.io
**What it does:** Visits the URL in a sandboxed browser and returns a screenshot, DOM content, DNS records, IP info, and all network requests made by the page.
**When to use:** When you need to see what a URL actually looks like without visiting it — especially useful for credential harvesting pages.
**Tips:**
- The screenshot instantly shows if it's a fake login page
- Check **Redirects** — malicious URLs often redirect multiple times
- Check **Certificates** — a brand-new TLS cert is suspicious
- Set scan to **Unlisted** if you don't want it publicly visible

---

### AbuseIPDB
**URL:** https://www.abuseipdb.com
**What it does:** Community-reported database of malicious IP addresses. Shows abuse confidence score, report history, ISP, and country.
**When to use:** Look up every IP found in email headers (X-Originating-IP) and every IP associated with malicious URLs.
**Tips:**
- Confidence score > 50% = treat as malicious
- Check the **Reports** tab for details on what the IP was reported for
- Cross-reference with VirusTotal for confirmation

---

### Whois / DomainTools
**URL:** https://whois.domaintools.com
**What it does:** Shows domain registration date, registrar, registrant info (if not privacy-protected), and DNS records.
**When to use:** Check every suspicious domain for registration age and ownership.
**Tips:**
- Domain registered < 30 days ago = high suspicion
- Privacy-protected registrant + new domain + suspicious name = almost certainly malicious
- Check if nameservers match what you'd expect for the claimed organisation

---

## 📎 File & Attachment Analysis

### Any.run
**URL:** https://any.run
**What it does:** Interactive online sandbox — you can watch malware execute in real time in a Windows VM. Shows all processes, network connections, file system changes, and registry modifications live.
**When to use:** When you need to see exactly what a suspicious file does when opened.
**Tips:**
- Use **Free** tier for most analysis — sufficient for most phishing attachments
- Watch the **Process graph** — it visually shows parent-child process relationships
- Check **Network activity** for any C2 connections
- Download the full **IOC report** at the end for documentation

---

### Hybrid Analysis
**URL:** https://www.hybrid-analysis.com
**What it does:** Automated sandbox that runs files in multiple environments (Windows 7, Windows 10, Linux) and produces a detailed behavioural report including MITRE ATT&CK mapping.
**When to use:** For a comprehensive automated report — especially useful when you need MITRE ATT&CK technique mapping for your incident report.
**Tips:**
- Check the **Indicators** tab for extracted IOCs
- The **MITRE ATT&CK** section maps techniques automatically — great for reports
- Search by hash first — the file may already have been analysed

---

### VirusTotal (File Analysis)
**URL:** https://www.virustotal.com
**What it does:** Scans file against 70+ AV engines and shows detailed static analysis, strings, imports, and behaviour sandbox results.
**When to use:** First check for any file hash — search hash before uploading the file.
**Tips:**
- Always search the hash first — uploading shares the file publicly
- Check **Behaviour** tab for sandbox results
- Check **Details** tab for file metadata (compile time, imphash, sections)

---

## 🎣 Phishing-Specific Platforms

### PhishTool
**URL:** https://www.phishtool.com
**What it does:** Purpose-built phishing analysis platform — paste an email and it automatically extracts headers, URLs, attachments, and threat intelligence in one interface.
**When to use:** For rapid triage of multiple phishing emails — faster than running each tool separately.
**Tips:**
- Free community edition available
- Generates a shareable analysis report
- Integrates with VirusTotal and other threat intel feeds automatically

---

### Phishtank
**URL:** https://phishtank.org
**What it does:** Community database of known phishing URLs. Check if a URL has already been reported and verified as phishing.
**When to use:** Quick check on suspicious URLs before deeper analysis.

---

## 🌐 Threat Intelligence

### Shodan
**URL:** https://www.shodan.io
**What it does:** Search engine for internet-connected devices. Shows what services are running on an IP, open ports, SSL certificates, and associated domains.
**When to use:** Look up suspicious IPs to understand what infrastructure the attacker is using.
**Tips:**
- Free searches limited — use for confirmed malicious IPs
- Check **SSL certificates** — they often reveal other domains hosted on the same IP
- Hosting provider and ASN can help attribute the infrastructure

---

### ThreatFox (abuse.ch)
**URL:** https://threatfox.abuse.ch
**What it does:** Community IOC sharing platform — search IPs, domains, URLs, and hashes against known malware IOCs.
**When to use:** Cross-reference IOCs found during investigation against known malware campaigns.

---

## 📋 Quick Reference — Which Tool for What

| What You Have | Tool to Use |
|--------------|------------|
| Raw email headers | MXToolbox |
| Suspicious URL | VirusTotal + URLScan.io |
| Suspicious IP | AbuseIPDB + VirusTotal |
| Suspicious domain | VirusTotal + Whois |
| File hash | VirusTotal |
| Suspicious file | Any.run + Hybrid Analysis |
| Full phishing email | PhishTool |
| Unknown IP infrastructure | Shodan |
| IOC cross-reference | ThreatFox |
