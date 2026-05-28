# 🔬 Phishing Analysis Methodology

A structured, repeatable process for investigating suspicious emails in a SOC environment. Following this methodology ensures consistent, thorough analysis and reduces the chance of missing critical indicators.

---

## 🔁 The 6-Stage Analysis Process

```
Stage 1: RECEIVE & TRIAGE
         ↓
Stage 2: EMAIL HEADER ANALYSIS
         ↓
Stage 3: URL & LINK ANALYSIS
         ↓
Stage 4: ATTACHMENT ANALYSIS
         ↓
Stage 5: VERDICT & SEVERITY
         ↓
Stage 6: DOCUMENT & RESPOND
```

---

## Stage 1 — Receive & Triage

**Goal:** Quickly determine if this warrants full investigation or can be closed immediately.

**Questions to answer:**
- Who reported it — a user, an automated alert, or a mail gateway?
- Who is the recipient? Executive, finance, IT admin, or standard user?
- What is the subject line? Does it use urgency, fear, or reward language?
- Is this a single targeted email or mass distribution?
- Has this sender or domain appeared in previous alerts?

**Triage decision:**
| Signal | Action |
|--------|--------|
| Executive targeted | Immediate full investigation |
| Finance team targeted | Immediate full investigation |
| Mass distribution | Full investigation + check for clicks |
| Single standard user, no links/attachments | Quick review, likely low risk |
| Duplicate of known phishing campaign | Close with reference to prior case |

---

## Stage 2 — Email Header Analysis

**Goal:** Determine the true origin of the email and check authentication results.

**Never trust the display name or From field** — these are trivially spoofed.

**Key header fields to check:**

| Field | What to Look For |
|-------|-----------------|
| `Received` | Trace the full routing path — does it match the claimed sender? |
| `Return-Path` | Should match the From address — mismatch = spoofing indicator |
| `X-Originating-IP` | The true sending IP — check on AbuseIPDB and VirusTotal |
| `SPF` | PASS = authorised sender / FAIL = IP not authorised to send |
| `DKIM` | PASS = email intact / FAIL = email modified in transit |
| `DMARC` | PASS = domain aligned / FAIL = likely spoofed domain |
| `Reply-To` | Different from From address? Attacker wants replies elsewhere |
| `X-Mailer` | Unusual mail client? Bulk mailer? |
| `Message-ID` | Malformed or missing Message-ID = automated/bulk sender |

**Authentication summary:**
```
All three PASS  = Legitimate mail infrastructure (but not a guarantee of safety)
SPF FAIL        = Sending IP not authorised by domain owner
DKIM FAIL       = Email body was modified after sending
DMARC FAIL      = Domain alignment failure — strong spoofing indicator
All three FAIL  = Almost certainly malicious or spoofed
```

**Tools:** MXToolbox Header Analyser, Google Admin Toolbox

---

## Stage 3 — URL & Link Analysis

**Goal:** Determine if any links in the email lead to malicious destinations.

**Rules:**
- ❌ NEVER click a link directly on a production machine
- ✅ ALWAYS use sandboxed tools to inspect URLs

**Step-by-step:**
1. Hover over the link — does the display text match the actual URL?
2. Copy the URL (without clicking)
3. Submit to **VirusTotal** — check detection ratio across 90+ engines
4. Submit to **URLScan.io** — get a visual screenshot and DNS/IP details
5. Check domain registration date — under 30 days = high suspicion
6. Check if domain spoofs a known brand (e.g. `paypa1.com`, `micros0ft-login.com`)
7. Look up the hosting IP on **AbuseIPDB**
8. Check if the page requests credentials, downloads a file, or redirects

**Red flags:**
- Domain registered within last 30 days
- URL uses HTTP instead of HTTPS
- Domain closely resembles a legitimate brand with typos
- URL shortener used to hide destination
- Multiple redirects before reaching final page
- Final page mimics a login portal (credential harvest)

---

## Stage 4 — Attachment Analysis

**Goal:** Determine if any attachments contain malicious code or payloads.

**Rules:**
- ❌ NEVER open an attachment on a production machine
- ✅ ALWAYS use a sandbox for dynamic analysis

**Step-by-step:**
1. Note the file name and extension — does it match? (`invoice.pdf.exe` = masquerading)
2. Calculate or obtain the file hash (MD5 / SHA256)
3. Search the hash on **VirusTotal** — any detections?
4. Submit the file to **Any.run** for interactive sandbox analysis
5. Submit to **Hybrid Analysis** for behavioural report
6. Look for:
   - Macros in Office documents (`.docm`, `.xlsm`)
   - Embedded scripts (`.js`, `.vbs`, `.bat`, `.ps1`)
   - Executables disguised as documents
   - Archive files containing malicious payloads (`.zip`, `.rar`, `.7z`)

**Sandbox — what to look for:**
```
Network activity:   Does it make outbound connections?
Process creation:   Does it spawn PowerShell, cmd, or new processes?
File system:        Does it drop files in Temp or AppData?
Registry changes:   Does it create persistence keys?
C2 communication:   Does it beacon to an external IP?
```

---

## Stage 5 — Verdict & Severity

**Goal:** Make a clear, evidence-based determination.

| Verdict | Definition |
|---------|-----------|
| **True Positive** | Confirmed phishing — malicious intent verified |
| **False Positive** | Legitimate email incorrectly flagged |
| **Benign Positive** | Suspicious but no malicious payload confirmed |

**Severity assignment:**

| Severity | Criteria |
|----------|---------|
| Critical | Executive targeted + credentials entered / malware executed |
| High | Malicious link or attachment confirmed / credentials at risk |
| Medium | Phishing confirmed but no user interaction |
| Low | Suspicious indicators but no confirmed malicious payload |

---

## Stage 6 — Document & Respond

**Goal:** Record all findings clearly and take appropriate containment actions.

**Documentation must include:**
- Sender, recipient, subject, timestamp
- Header analysis summary (SPF/DKIM/DMARC results)
- All IOCs identified (URLs, IPs, domains, hashes)
- Tools used and findings from each
- Verdict and severity
- Actions taken
- Escalation details if applicable

**Response actions by severity:**

| Action | When |
|--------|------|
| Quarantine email from all mailboxes | Always for confirmed phish |
| Block sender domain at mail gateway | Confirmed malicious sender |
| Block malicious URL at proxy | Confirmed malicious link |
| Reset user credentials | User entered credentials |
| Isolate endpoint | Malware executed |
| Notify and educate user | Always |
| Escalate to Tier 2 | Malware executed or credentials compromised |

---

## 📚 References

- [MITRE ATT&CK T1566 — Phishing](https://attack.mitre.org/techniques/T1566/)
- [Google Phishing Quiz](https://phishingquiz.withgoogle.com)
- [PhishTool Documentation](https://www.phishtool.com)
