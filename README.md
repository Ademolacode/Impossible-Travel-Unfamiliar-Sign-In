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
├── README.md                          # You are here (overview for recruiters)
│
├── investigations/
│   └── 001-impossible-travel-unfamiliar-signin/
│       ├── README.md                  # Full investigation report
│       ├── timeline.md                # Event-by-event timeline (UTC)
│       ├── iocs.md                    # Extracted indicators of compromise
│       ├── splunk-queries.md           # Splunk queries used in the investigation
│       └── screenshots/               # Supporting visual evidence
│           ├── 01-unfamiliar-login.png
│           ├── 02-ip-geolocation.png
│           ├── 03-mailbox-rule.png
│           ├── 04-email-activity.png
│           └── 05-activity-timeline.png
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

| Screenshot                   | Description                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------ |
| **01-unfamiliar-login.png**  | Splunk results showing successful login for the user from a Singapore-based IP |
| **02-ip-geolocation.png**    | OSINT enrichment confirming the IP geolocation                                 |
| **03-mailbox-rule.png**      | Audit log evidence of inbox forwarding rule creation                           |
| **04-email-activity.png**    | Logs showing emails accessed and a banking-themed email sent                   |
| **05-activity-timeline.png** | Chronological view of login and mailbox actions                                |

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

## 🤝 Notes

This investigation was completed as part of hands-on training within the **MyDFIR SOC Community** and reflects SOC investigation and documentation standards.


