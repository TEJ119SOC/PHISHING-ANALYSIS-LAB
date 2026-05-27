# PHISHING-ANALYSIS-LAB
# 🎣 Phishing Analysis Lab

A hands-on phishing email analysis portfolio documenting real-world investigation techniques, tools, and methodologies used by SOC analysts to detect, analyse, and respond to phishing threats.

Each case study walks through a complete phishing investigation — from the initial suspicious email through header analysis, URL and attachment inspection, verdict, and recommended response actions.

---

## 🎯 Purpose

This lab demonstrates practical skills in:
- **Email header forensics** — tracing email origin and authentication failures
- **URL & domain analysis** — identifying malicious links and spoofed domains
- **Attachment analysis** — safely examining suspicious files using sandboxes
- **IOC extraction** — documenting indicators of compromise for threat intel
- **Investigation documentation** — producing clear, structured analysis reports

---

## 📂 Repository Structure

```
phishing-analysis-lab/
├── README.md                        ← You are here
├── methodology.md                   ← Step-by-step analysis process
├── tools-reference.md               ← Tools used and how to use them
├── sample-analysis/
│   ├── case-001-credential-harvest.md   ← Fake login page phishing
│   └── case-002-malware-attachment.md   ← Malicious attachment phishing
└── ioc-templates/
    └── ioc-report-template.md       ← Blank IOC documentation template
```

---

## 🔍 Case Studies

| Case | Type | Delivery Method | Verdict | Severity |
|------|------|----------------|---------|----------|
| [Case 001](./sample-analysis/case-001-credential-harvest.md) | Credential Harvesting | Malicious URL | True Positive | High |
| [Case 002](./sample-analysis/case-002-malware-attachment.md) | Malware Delivery | Malicious Attachment | True Positive | Critical |

---

## 🧰 Tools Used

| Tool | Purpose |
|------|---------|
| [MXToolbox](https://mxtoolbox.com/EmailHeaders.aspx) | Email header analysis |
| [VirusTotal](https://www.virustotal.com) | URL, domain, and file hash scanning |
| [URLScan.io](https://urlscan.io) | URL visual inspection and DNS info |
| [AbuseIPDB](https://www.abuseipdb.com) | IP reputation checking |
| [Any.run](https://any.run) | Interactive malware sandbox |
| [Hybrid Analysis](https://www.hybrid-analysis.com) | Behavioural file analysis |
| [PhishTool](https://www.phishtool.com) | Phishing email analysis platform |

---

## ⚠️ Disclaimer

All email samples in this lab are **fictional and created for educational purposes only**. No real personal data, real IP addresses, or real organisational information is used. All analysis techniques are documented for **defensive security purposes**.

---

## 👤 Author

**Sai Teja Adapa** — SOC Analyst Level 1
[LinkedIn](https://www.linkedin.com/in/tejaadapa23) | [Email](mailto:tejadapa2309@gmail.com)
