# Impossible Travel & Unfamiliar Sign-In Detection

## 🎯 Project Overview
This investigation demonstrates the detection and analysis of an **Impossible Travel** pattern and **Unfamiliar Sign-in** activity using Splunk SIEM. 

The scenario involves a compromised Microsoft 365 account accessed from Singapore while the organization is based in Vancouver, Canada. This access was followed by the creation of malicious email forwarding rules and unauthorized access to sensitive financial communications—a classic **Business Email Compromise (BEC)** pattern.

**Skills Demonstrated:**
* SIEM Log Analysis (Splunk)
* Identity Threat Detection & Identity Protection
* OSINT (IP Reputation & Geolocation)
* Incident Response Procedures (Containment & Remediation)

**Tools Used:**
* **Splunk Enterprise** (M365 Audit Logs)
* **ipinfo.io** (OSINT for IP enrichment)
* **MITRE ATT&CK Framework** (Technique Mapping)

---

## 🔍 Investigation Findings

### Key Indicators of Compromise (IOCs)
| Indicator | Value |
| :--- | :--- |
| **Compromised Account** | JChan@7pd6vr.onmicrosoft[.]com |
| **Malicious IP Address** | 188.214.125[.]138 |
| **Geographic Location** | Singapore |
| **Timestamp (UTC)** | 2024-01-11 20:46:39 |
| **Forwarding Destination** | stoicellis@imcourageous[.]com |
| **Suspicious Attachment** | NEW-BANK-ACCOUNT.pdf |

### 📊 Executive Summary
On **2024-01-11 at 20:46:39 UTC**, an alert was triggered for an unfamiliar sign-in for user `JChan`. The login originated from Singapore, approximately **7,000+ miles** from the company's headquarters in Vancouver, BC. Given the timeframe of previous logins, this represents a confirmed case of **Impossible Travel**.

Post-compromise analysis revealed the attacker successfully established persistence and conducted reconnaissance on financial invoices, eventually attempting to redirect payments via a fraudulent PDF attachment.

![Initial Login from Singapore](./screenshots/01-splunk-singapore-login.png)

---

## 📋 Detailed Analysis (5W1H)

### WHO
* **Affected User:** JChan@7pd6vr.onmicrosoft[.]com
* **Attacker Source:** 188.214.125[.]138 (Singapore-based VPN/Host)

### WHAT
* Unauthorized M365 authentication.
* Created a mailbox rule to forward all "schan" emails to an external address (`stoicellis@imcourageous[.]com`).
* Accessed sensitive emails regarding "First Invoice" and "Client Bank Account."
* Sent a phishing email with a malicious PDF (`NEW-BANK-ACCOUNT.pdf`).

### WHEN
* **Initial Access:** 2024-01-11 20:46:39 UTC.
* **Duration:** Malicious activity occurred within a 15-minute window, suggesting a scripted or highly efficient attacker.

### WHERE
* **Origin:** Singapore.
* **Environment:** Exchange Online / Microsoft 365.

### WHY
* **Motivation:** Financial fraud (Invoice redirection/BEC). The attacker targeted specific financial threads to intercept payments.

### HOW
* **Initial Access:** Likely via Credential Theft (Phishing or Password Spray).
* **Persistence:** Established through an auto-forwarding mailbox rule.
* **Defense Evasion:** Attacker marked forwarded emails as "Read" and moved them to "Deleted Items" to keep the victim unaware of the compromise.

![Email Forwarding Rule Creation](./screenshots/02-email-forwarding-rule.png)

---

## 🛠️ MITRE ATT&CK Mapping
* **T1078.004:** Valid Accounts (Cloud Accounts)
* **T1114.003:** Email Forwarding Rule
* **T1566.001:** Phishing (Spearphishing Attachment)
* **T1070.008:** Indicator Removal (Mailbox Modifications)

---

## 🚨 Recommendations

### Immediate Containment
1.  **Account Lockout:** Disable `JChan` account and revoke all active Oauth refresh tokens.
2.  **Password Reset:** Force a global password reset and re-enroll MFA.
3.  **Rule Removal:** Delete the malicious forwarding rule to `stoicellis@imcourageous[.]com`.
4.  **Email Recall:** Attempt to recall the "URGENT: Client Bank Account" email from internal and external recipients.

### Long-Term Hardening
1.  **Conditional Access:** Implement policies to block authentications from non-operational countries (e.g., Geofencing).
2.  **MFA Enforcement:** Ensure "Number Matching" is enabled for MFA to prevent MFA fatigue attacks.
3.  **Alerting:** Create a Splunk alert for `New-InboxRule` operations where the `ForwardTo` parameter contains an external domain.

---

## 🧠 Lessons Learned
* **Visibility is Key:** Without M365 `Workload=Exchange` logs, the forwarding rule creation would have been missed.
* **VPN False Positives:** While Singapore was malicious here, I learned to cross-reference IP reputation with `UserAgent` strings to distinguish between traveling employees and attackers.
* **BEC Stealth:** Attackers don't always change passwords; they prefer "sitting in the middle" via forwarding rules to maintain long-term access.

---
*This investigation was completed as part of the MyDFIR SOC training program.*
