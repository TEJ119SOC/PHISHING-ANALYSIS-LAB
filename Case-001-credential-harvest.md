# 🔍 Case 001 — Credential Harvesting Phishing Email

**Case ID:**        PHI-2026-001
**Date Reported:**  2026-01-14
**Reported By:**    User — Sarah Mitchell (Finance Department)
**Analyst:**        Sai Teja Adapa
**Verdict:**        ✅ True Positive
**Severity:**       🟠 High
**Status:**         Closed

---

## 📧 Email Summary

| Field | Details |
|-------|---------|
| **From (Display)** | Microsoft Account Team \<security@microsoft.com\> |
| **From (Actual)** | security@micros0ft-verify-login.com |
| **Reply-To** | support@micros0ft-verify-login.com |
| **To** | sarah.mitchell@examplecorp.com |
| **Subject** | ⚠️ Urgent: Your Microsoft Account Has Been Compromised |
| **Date** | 2026-01-14 02:47:33 UTC |
| **Attachments** | None |
| **Links Present** | Yes — 1 URL |

---

## 🚩 Initial Triage Observations

Upon first review, several immediate red flags were identified before any technical analysis:

- **Urgency language** in subject line — "Urgent" and "Compromised" designed to panic the recipient
- **Off-hours send time** — 02:47 UTC, outside normal business hours
- **Finance department recipient** — high-value target for credential theft
- **Sender domain mismatch** — display name shows `microsoft.com` but actual sender is `micros0ft-verify-login.com` (note the zero replacing the letter O)
- **Reply-To differs from From** — replies would go to attacker-controlled address

**Triage decision:** Full investigation required — executive/finance target with domain spoofing indicators.

---

## 🔬 Stage 2 — Email Header Analysis

**Tool used:** MXToolbox Email Header Analyser

**Raw header findings:**

```
Received: from mail.micros0ft-verify-login.com (185.234.219.47)
Return-Path: security@micros0ft-verify-login.com
X-Originating-IP: 185.234.219.47
Authentication-Results:
  spf=FAIL (micros0ft-verify-login.com is not authorised to send as microsoft.com)
  dkim=FAIL (signature verification failed)
  dmarc=FAIL (policy=reject; domain alignment failure)
```

**Authentication results:**

| Check | Result | Meaning |
|-------|--------|---------|
| SPF | ❌ FAIL | Sending IP not authorised by microsoft.com |
| DKIM | ❌ FAIL | Email signature invalid — possible tampering |
| DMARC | ❌ FAIL | Domain alignment failed — confirmed spoofing |

**IP Analysis — 185.234.219.47:**
- **AbuseIPDB:** Confidence score 94% — reported 47 times for phishing and spam
- **VirusTotal:** Flagged by 12 engines — associated with phishing campaigns
- **Geolocation:** Bucharest, Romania — no legitimate Microsoft infrastructure in this location
- **ISP:** Hosting provider — not a Microsoft-owned IP range

**Conclusion:** Email is definitively spoofed. All three authentication checks failed. Sending IP is a known malicious host.

---

## 🔗 Stage 3 — URL Analysis

**URL found in email body:**
```
http://micros0ft-verify-login.com/account/verify?token=a7f3k9p2&user=sarah.mitchell
```

**Immediate red flags:**
- HTTP (not HTTPS) — no encryption
- Domain uses zero (`0`) instead of letter O in `microsoft`
- URL contains victim's username — personalised lure
- `/verify` path — classic credential harvesting pattern

**VirusTotal results:**
- **Detection ratio:** 18/90 engines flagged as phishing
- **Categories:** Phishing, Malicious Site
- **Associated malware:** Generic credential stealer campaign

**URLScan.io results:**
- **Screenshot:** Fake Microsoft login page — pixel-perfect replica of Microsoft sign-in portal
- **Domain registered:** 2026-01-10 — 4 days before this email was sent
- **Hosting IP:** 185.234.219.47 — same IP as sending server (attacker controls both)
- **SSL Certificate:** Self-signed, issued same day as domain registration
- **Page behaviour:** Form submits credentials to `POST /collect.php` on same domain

**Whois lookup:**
- **Registered:** 2026-01-10 (4 days old)
- **Registrar:** Namecheap (commonly abused for phishing domains)
- **Registrant:** Privacy protected
- **Nameservers:** Cloudflare (used to hide origin IP)

**Conclusion:** Confirmed credential harvesting page. Domain is 4 days old, designed to mimic Microsoft login, collecting credentials via PHP script.

---

## 📎 Stage 4 — Attachment Analysis

**No attachments present in this email.**

---

## 👤 User Interaction Check

Contacted the reporting user (Sarah Mitchell) directly:

- ✅ User reported the email without clicking the link
- ✅ No credentials entered
- ✅ No other users in the organisation received this email (single targeted delivery)
- ✅ No endpoint alerts on Sarah's machine

**Conclusion:** No compromise occurred. User responded correctly by reporting without interacting.

---

## 🏁 Stage 5 — Verdict & Severity

| Field | Assessment |
|-------|-----------|
| **Verdict** | True Positive — confirmed phishing |
| **Type** | Credential Harvesting |
| **Technique** | Spearphishing Link (T1566.002) |
| **Severity** | High |
| **User Impact** | None — not clicked |
| **Business Impact** | None — contained |

**Severity rationale:** High (not Critical) because although the email was a confirmed phishing attempt targeting a finance user with a convincing fake login page, no credentials were entered and no compromise occurred.

---

## 🚧 Response Actions Taken

| Action | Status | Time |
|--------|--------|------|
| Email quarantined from all mailboxes | ✅ Done | 09:14 |
| Sender domain blocked at mail gateway | ✅ Done | 09:17 |
| Malicious URL blocked at web proxy | ✅ Done | 09:19 |
| Sending IP blocked at firewall | ✅ Done | 09:21 |
| User notified and educated | ✅ Done | 09:35 |
| IOCs submitted to threat intel platform | ✅ Done | 09:40 |
| Ticket closed | ✅ Done | 09:45 |

---

## 🧾 IOCs Extracted

| Type | Value | Verdict |
|------|-------|---------|
| Domain | micros0ft-verify-login.com | Malicious — phishing domain |
| IP Address | 185.234.219.47 | Malicious — phishing infrastructure |
| URL | http://micros0ft-verify-login.com/account/verify | Malicious — credential harvesting page |
| Email | security@micros0ft-verify-login.com | Malicious — phishing sender |

---

## 💡 Lessons Learned

1. **User awareness works** — the finance user correctly identified the suspicious email and reported it before clicking. Security awareness training is effective.
2. **Domain typosquatting** — attackers replaced the letter O with zero in `microsoft`. This is a common technique that bypasses simple domain blocklists.
3. **Detection gap identified** — SPF/DKIM/DMARC failures should trigger automatic quarantine. Recommend reviewing mail gateway rules to auto-quarantine all three failures.
4. **Short-lived infrastructure** — domain was only 4 days old. Consider implementing a rule to flag or quarantine emails from domains less than 30 days old.

---

## 📚 References

- [MITRE ATT&CK T1566.002 — Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)
- [VirusTotal Report — micros0ft-verify-login.com](#)
- [URLScan.io Report — Case 001](#)
