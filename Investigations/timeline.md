# Investigation Timeline

**Incident Date:** 2024-01-11  
**Timezone:** UTC  
**Compromised Account:** JChan@7pd6vr.onmicrosoft[.]com  
**Attacker IP:** 188.214.125[.]138 (Singapore)

---

| Time (UTC) | Event Type | Description | Evidence Source | Severity |
|------------|-----------|-------------|-----------------|----------|
| 20:46:39 | **Initial Access** | Successful login from Singapore (IP: 188.214.125[.]138) | Operation=UserLoggedIn | 🔴 Critical |
| 20:47:15 | **Persistence** | Created inbox forwarding rule: schan → stoicellis@imcourageous[.]com (auto-delete + mark read) | Operation=New-InboxRule | 🔴 Critical |
| 20:50:10 | **Collection** | Accessed email: "RE: First Invoice of the month" | Operation=MailItemsAccessed | 🟠 High |
| 20:51:33 | **Collection** | Accessed email: "URGENT: Client Bank Account" | Operation=MailItemsAccessed | 🟠 High |
| 20:53:22 | **Exfiltration** | Sent fraudulent email with subject "URGENT: Client Bank Account" containing NEW-BANK-ACCOUNT.pdf | Operation=Send | 🔴 Critical |
| 20:54:45 | **Defense Evasion** | Deleted draft email: "Re: First Invoice of the month!" | Operation=SoftDelete | 🟠 High |

---

## MITRE ATT&CK Mapping

| Tactic | Technique | Evidence |
|--------|-----------|----------|
| Initial Access | T1078 - Valid Accounts | Login with stolen credentials |
| Persistence | T1114.003 - Email Forwarding Rule | Forwarding rule to attacker email |
| Collection | T1114.002 - Email Collection | Accessed financial emails |
| Exfiltration | T1048.003 - Exfiltration Over Alternative Protocol | Email with malicious attachment |
| Defense Evasion | T1070.004 - File Deletion | Deleted draft to cover tracks |
