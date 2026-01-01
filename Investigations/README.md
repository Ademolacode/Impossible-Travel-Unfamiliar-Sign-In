# Investigation Report: Account Takeover via Singapore IP

### Executive Summary
Confirmed account takeover of JChan@7pd6vr.onmicrosoft[.]com. The attacker bypassed geographic restrictions to establish a malicious forwarding rule and exfiltrate financial data.

### 5W1H Analysis
* **Who:** JChan account via IP 188.214.125.138 (Singapore).
* **What:** Persistence via `New-InboxRule`; Data exfiltration of `NEW-BANK-ACCOUNT.pdf`.
* **When:** 2024-01-11 20:46:39 UTC.
* **Where:** Corporate M365 Mailbox.
* **Why:** Financial fraud (Business Email Compromise).
* **How:** Unauthorized authentication followed by mailbox manipulation.

### Recommendations
1. **Immediate:** Disable account, revoke MFA sessions, and delete the forwarding rule to `stoicellis@imcourageous[.]com`.
2. **Short-term:** Scan all mailboxes for similar rules referencing "imcourageous[.]com".
3. **Long-term:** Implement Conditional Access to block non-domestic logins.
