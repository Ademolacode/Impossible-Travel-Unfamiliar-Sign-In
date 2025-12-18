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

**[Screenshot 1: Splunk search results showing JChan's login from Singapore IP 188.214.125.138]**

![Initial Login from Singapore](./screenshots/01-splunk-singapore-login.png)

---

## 🔍 Detection Queries Used

### Identify Logins from Non-Baseline Countries
```splunk
index=cloud sourcetype="o365:management:activity" 
Operation="UserLoggedIn" Workload="AzureActiveDirectory"
| iplocation ClientIP
| where Country!="Canada"
| table _time User ClientIP Country City
```

### Detect Email Forwarding Rule Creation
```splunk
index=cloud sourcetype="o365:management:activity"
Operation="Set-Mailbox" OR Operation="UpdateInboxRules" OR Operation="New-InboxRule"
| table _time UserId ClientIP Operation Parameters
| sort _time
```

### Investigation Query - All Activity from Suspicious IP
```splunk
index=cloud ClientIP="188.214.125.138"
| table _time UserId Operation Workload ResultStatus
| sort _time
```

### Identify Accessed Emails
```splunk
index=cloud UserId="JChan@7pd6vr.onmicrosoft.com" 
Operation="MailItemsAccessed" OR Operation="Send"
| table _time Operation Subject AttachmentNames
```

**[Screenshot 2: Splunk log showing malicious email forwarding rule creation with ForwardTo parameter pointing to external address]**

![Email Forwarding Rule Creation](./screenshots/02-email-forwarding-rule.png)

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

### HOW (Investigation Process)
1. **Detection:** Impossible travel alert triggered (Vancouver → Singapore in <2 hours)
2. **Initial Triage:** Verified login via Splunk: `index=cloud user=JChan Operation=UserLoggedIn`
3. **Lateral Movement Check:** Searched for additional compromised accounts from same IP (none found)
4. **Post-Compromise Activity:** Identified forwarding rule via `Operation=Set-Mailbox`
5. **Initial Access Vector:** Likely via Credential Theft (Phishing or Password Spray)
6. **Persistence Mechanism:** Established through an auto-forwarding mailbox rule
7. **Defense Evasion:** Attacker marked forwarded emails as "Read" and moved them to "Deleted Items" to keep the victim unaware of the compromise

**[Screenshot 3: Splunk results displaying MailItemsAccessed and Send operations showing emails viewed and sent by the attacker]**

![Accessed and Sent Emails](./screenshots/03-accessed-sent-emails.png)

---

## 🛠️ MITRE ATT&CK Mapping
* **T1078.004:** Valid Accounts (Cloud Accounts)
* **T1114.003:** Email Forwarding Rule
* **T1566.001:** Phishing (Spearphishing Attachment)
* **T1070.008:** Indicator Removal (Mailbox Modifications)

**[Screenshot 4: Timeline visualization in Splunk showing chronological sequence of attack activities from initial login through email exfiltration]**

![Attack Timeline](./screenshots/04-attack-timeline.png)

---

## 🚨 Recommendations

### Immediate Containment
1.  **Account Lockout:** Disable `JChan` account and revoke all active OAuth refresh tokens.
2.  **Password Reset:** Force a global password reset and re-enroll MFA.
3.  **Rule Removal:** Delete the malicious forwarding rule to `stoicellis@imcourageous[.]com`.
4.  **Email Recall:** Attempt to recall the "URGENT: Client Bank Account" email from internal and external recipients.

### Long-Term Hardening
1.  **Conditional Access:** Implement policies to block authentications from non-operational countries (e.g., Geofencing).
2.  **MFA Enforcement:** Ensure "Number Matching" is enabled for MFA to prevent MFA fatigue attacks.
3.  **Alerting:** Create a Splunk alert for `New-InboxRule` operations where the `ForwardTo` parameter contains an external domain.

### Next Steps if This Were a Real Incident
1. **User Notification:** Contact JChan to inform them of the compromise and provide security awareness training
2. **Threat Intelligence:** Submit the malicious IP and email address to threat intelligence platforms (ThreatConnect, MISP)
3. **Forensic Analysis:** Review JChan's recent email activity to identify if any other financial transactions were compromised
4. **Communication with Finance:** Alert finance team to verify all recent banking detail changes and payment instructions
5. **Executive Reporting:** Prepare incident summary for leadership highlighting financial risk and remediation steps

**[Screenshot 5: Splunk alert configuration showing search query and trigger conditions for detecting impossible travel patterns]**

![Alert Configuration](./screenshots/05-alert-configuration.png)

---

## 🧠 Lessons Learned
* **Visibility is Key:** Without M365 `Workload=Exchange` logs, the forwarding rule creation would have been missed.
* **VPN False Positives:** While Singapore was malicious here, I learned to cross-reference IP reputation with `UserAgent` strings to distinguish between traveling employees and attackers.
* **BEC Stealth:** Attackers don't always change passwords; they prefer "sitting in the middle" via forwarding rules to maintain long-term access.

---
*This investigation was completed as part of the MyDFIR SOC training program.*
