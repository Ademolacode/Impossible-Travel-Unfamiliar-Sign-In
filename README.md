# 🛡️ Impossible-Travel-Unfamiliar-Sign-In

A hands-on SOC investigation documenting the detection and analysis of an **Impossible Travel / Unfamiliar Sign-In** event in a Microsoft 365 environment using **Splunk SIEM**.

This repository demonstrates how I triage identity-based alerts, analyze logs, document evidence, and produce clear, defensible investigation reports aligned with real SOC workflows.

---

## 🎯 What This Project Shows

* Identity-based alert triage (Impossible Travel)
* Microsoft 365 audit log investigation
* Splunk search and analysis
* IOC extraction and documentation
* Business Email Compromise (BEC) analysis
* Clear, professional SOC-style reporting
* Evidence-backed conclusions (no assumptions)

---

## 📂 Repository Structure

```
impossible-travel-unfamiliar-signin/
├── README.md                                    # Landing page (overview, skills, links)
│
└── Investigations/                              # All investigation materials here
    ├── README.md                                # Full investigation report (5W1H format)
    ├── timeline.md                              # Event-by-event timeline
    ├── iocs.md                                  # Indicators of compromise
    ├── splunk-queries.md                        # Queries with results
    └── screenshots/                             # Visual evidence
        ├── 01-unfamiliar-login.png
        ├── 02-ip-geolocation.png
        ├── 03-mailbox-rule-creation.png
        ├── 04-mailbox-rule-details.png
        ├── 05-emails-accessed.png
        ├── 06-email-sent.png
        ├── 07-draft-deleted.png
        ├── 08-ip-correlation-check.png
        └── 09-activity-timeline.png
```

---

## 🧾 Featured Investigation

### 🔹 Impossible Travel & Unfamiliar Sign-In (Microsoft 365)

**Category:** Identity Security / Business Email Compromise

**SIEM:** Splunk

**Data Source:** Microsoft 365 Audit Logs

**Timezone:** UTC

**Org Context:** Vancouver, BC, Canada

**Summary:**
Investigated an unfamiliar sign-in originating from **Singapore** for a Canada-based user. Post-authentication activity revealed **mailbox forwarding rule abuse** and **fraudulent email activity**, consistent with **Business Email Compromise (BEC)**.

👉 **[View Full Investigation Report](./Investigations/README.md)**

---

## 📸 Evidence Included (Screenshots)

Screenshots are included to demonstrate **tool familiarity** and **support findings**.
All conclusions are based on log evidence, not speculation.

### Screenshot Index (Located in `/screenshots/`)

| Screenshot | Description |
|-----------|-------------|
| 01-unfamiliar-login.png | Login from Singapore IP (188.214.125.138) for JChan at 2024-01-11 20:46:39 UTC |
| 02-ip-geolocation.png | OSINT confirmation: IP geolocates to Singapore |
| 03-mailbox-rule-creation.png | Audit log: Forwarding rule created |
| 04-mailbox-rule-details.png | Full rule config: schan → stoicellis, delete + mark read |
| 05-emails-subject-distribution.png | Two emails subject: "RE: First Invoice" and "URGENT: Client Bank Account" |
| 06-email-sent.png | Malicious email sent with NEW-BANK-ACCOUNT.pdf attachment |
| 07-draft-deleted.png | Email deleted from drafts: "Re: First Invoice of the month!" |
| 08-ip-correlation-check.png | Query confirming no other accounts used this IP |
| 09-activity-timeline.png | Chronological view of all actions from compromised session |

---

## 🔍 Queries Used (Splunk)

All Splunk queries used during the investigation are documented for transparency and reproducibility.

📁 **Location:**

```
./Investigations/splunk-queries.md
```

**Query Coverage Includes:**

* Identifying unfamiliar / foreign logins
* Reviewing all activity for the compromised user
* Detecting inbox forwarding rule creation
* Identifying accessed and sent emails
* Correlating activity by IP address

---

## 📄 Investigation Documentation

Inside the investigation folder you’ll find:

* **README.md** – Full SOC-style investigation report
* **timeline.md** – Ordered timeline of events (UTC)
* **iocs.md** – Extracted indicators (IP, account, email, attachment)
* **splunk-queries.md** – All Splunk searches used
* **screenshots/** – Visual evidence supporting findings

---

## 🧠 Skills Demonstrated

* SIEM log analysis (Splunk)
* Identity security investigations
* Microsoft 365 mailbox auditing
* OSINT-based IP enrichment
* IOC handling and documentation
* Incident response recommendations
* Clear written communication for SOC audiences

---



