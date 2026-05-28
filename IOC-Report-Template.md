# 📋 IOC Report Template

Use this template to document all indicators of compromise identified during a phishing investigation. Fill in every section — even if the result is "None found" or "N/A".

---

## 📌 Case Information

| Field | Details |
|-------|---------|
| **Case ID** | PHI-YYYY-XXX |
| **Date** | YYYY-MM-DD |
| **Analyst** | Your name |
| **Verdict** | True Positive / False Positive / Benign |
| **Severity** | Critical / High / Medium / Low |
| **Malware Family** | e.g. AgentTesla, Emotet, or N/A |
| **Attack Type** | Credential Harvest / Malware Delivery / BEC |

---

## 📧 Email IOCs

| Type | Value | Notes |
|------|-------|-------|
| Sender email | | |
| Sender domain | | |
| Reply-To address | | |
| Subject line | | |
| X-Originating-IP | | |
| SPF result | PASS / FAIL | |
| DKIM result | PASS / FAIL | |
| DMARC result | PASS / FAIL | |

---

## 🔗 URL / Domain IOCs

| Type | Value | VT Score | URLScan | Verdict |
|------|-------|----------|---------|---------|
| URL | | /90 | Link | |
| Domain | | /90 | Link | |
| Hosting IP | | /90 | Link | |
| Domain age | | days old | | |

---

## 📎 File / Attachment IOCs

| Type | Value | Notes |
|------|-------|-------|
| File name | | |
| File type | | |
| MD5 hash | | |
| SHA256 hash | | |
| VT detection | /72 | |
| Malware family | | |
| Sandbox report | Link | Any.run / Hybrid Analysis |

---

## 🌐 Network IOCs

| Type | Value | AbuseIPDB | Purpose |
|------|-------|-----------|---------|
| C2 IP | | % confidence | |
| C2 domain | | | |
| C2 port | | | |
| Exfil destination | | | |

---

## 🖥️ Endpoint IOCs

| Type | Value | Notes |
|------|-------|-------|
| Dropped file path | | |
| Dropped file name | | |
| Registry key | | |
| Scheduled task | | |
| Malicious process | | |

---

## ✅ Response Actions Taken

| Action | Done? | Time |
|--------|-------|------|
| Email quarantined | ☐ | |
| Sender domain blocked | ☐ | |
| Malicious URL blocked | ☐ | |
| C2 IP blocked at firewall | ☐ | |
| User account disabled | ☐ | |
| Endpoint isolated | ☐ | |
| Credentials reset | ☐ | |
| User notified | ☐ | |
| Escalated to Tier 2 | ☐ | |
| Ticket closed | ☐ | |

---

## 📝 Analysis Notes

_Add any additional context, observations, or findings that don't fit the above sections._

---

## 💡 Lessons Learned

1. 
2. 
3.
