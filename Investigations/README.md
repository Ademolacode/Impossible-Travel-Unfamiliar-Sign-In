# Investigation Report: Business Email Compromise - JChan Account

## Findings

**Time:** 2024-01-11 20:46:39 UTC  
**Compromised Account:** JChan@7pd6vr.onmicrosoft[.]com  
**Source IP:** 188.214.125[.]138  
**Source Location:** Singapore  
**IOC Domain:** imcourageous[.]com  
**IOC Email:** stoicellis@imcourageous[.]com  
**Exfiltrated Attachment:** NEW-BANK-ACCOUNT.pdf  
**Attack Pattern:** Business Email Compromise (BEC)  
**Forwarding Rule Target:** schan@7pd6vr.onmicrosoft[.]com  

---

## Investigation Summary

On 2024-01-11 at 20:46:39 UTC, an unfamiliar sign-in was detected for user JChan@7pd6vr.onmicrosoft[.]com originating from Singapore (IP: 188.214.125[.]138). This login was anomalous as the user is based in Vancouver, BC, Canada. Following successful authentication, the attacker established persistence by creating a mailbox forwarding rule targeting emails from "schan" to be forwarded to stoicellis@imcourageous[.]com, then moved to deleted items and marked as read to avoid detection.

The attacker then accessed two emails: "RE: First Invoice of the month" and "URGENT: Client Bank Account." Shortly after, a fraudulent email with subject "URGENT: Client Bank Account" was sent containing an attachment named "NEW-BANK-ACCOUNT.pdf"—likely an attempt to redirect financial transactions. The attacker also deleted the email "Re: First Invoice of the month!" from the drafts folder, suggesting counter-forensic behavior.

Based on log analysis, no other accounts logged in from this IP address, and no evidence of persistent access mechanisms beyond the forwarding rule was identified. The activity appears to be a targeted Business Email Compromise (BEC) attack focused on financial fraud.

---

## 5W1H Analysis

**WHO:**  
- Compromised account: JChan@7pd6vr.onmicrosoft[.]com  
- Attacker IP: 188.214.125[.]138 (Singapore-based)  
- No other accounts accessed from this IP

**WHAT:**  
- Unauthorized login to JChan's Microsoft 365 mailbox  
- Creation of malicious inbox forwarding rule (schan → stoicellis@imcourageous[.]com)  
- Accessed two emails containing financial information  
- Sent fraudulent email with banking-themed attachment (NEW-BANK-ACCOUNT.pdf)  
- Deleted draft email to cover tracks

**WHEN:**  
- Initial login: 2024-01-11 20:46:39 UTC  
- Activity timeframe: 2024-01-11 20:46:39 UTC to [end time from logs]  
- Current status: Cannot confirm if access is ongoing; forwarding rule likely still active if not removed

**WHERE:**  
- JChan's Microsoft 365 mailbox (corporate M365 tenant)  
- No lateral movement to other accounts detected

**WHY:**  
- Financial fraud via Business Email Compromise (BEC)  
- Intent to intercept financial communications from "schan" and redirect payments to attacker-controlled accounts

**HOW:**  
- Attacker gained unauthorized access to JChan's credentials (method unknown—could be phishing, credential stuffing, or previous breach)  
- Bypassed geographic controls to authenticate from Singapore  
- Created stealthy forwarding rule with auto-deletion to maintain persistence and avoid detection  
- Leveraged compromised mailbox to conduct financial fraud via spoofed banking communication

---

## Recommendations

### 1. **Immediate Actions**
- **Disable the JChan account** and force password reset  
- **Revoke all active MFA sessions** for JChan  
- **Delete the forwarding rule** to stoicellis@imcourageous[.]com  
- **Quarantine or recall** the email "URGENT: Client Bank Account" if possible  
- **Alert schan** that their communications may have been intercepted

### 2. **Short-Term Actions**
- **Hunt for similar forwarding rules** across all mailboxes, especially targeting "imcourageous[.]com"  
- **Search for the IP 188.214.125[.]138** in authentication logs to identify any other successful logins  
- **Review JChan's recent sent/deleted items** for additional malicious activity  
- **Contact recipients** of the fraudulent email to prevent payment redirection

### 3. **Long-Term Actions**
- **Implement Conditional Access Policies** to block logins from non-trusted geographic regions  
- **Enable mailbox auditing alerts** for inbox rule creation events  
- **Deploy MFA with number matching** to prevent MFA fatigue attacks  
- **Conduct security awareness training** focused on credential phishing and BEC tactics  
- **Consider blocking email forwarding to external domains** or requiring approval workflows
