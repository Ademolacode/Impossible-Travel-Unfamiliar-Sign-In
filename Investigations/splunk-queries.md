# Splunk Queries Used in Investigation

## 1. Initial Triage - Foreign Login Detection
**Purpose:** Identify unfamiliar sign-ins from non-Canada locations
```splunk
index=cloud Operation="UserLoggedIn" 
| stats count by ActorIPAddress, ClientIP,n Operation,UserID, Workload
```

**Result:** Found login for JChan from Singapore (188.214.125[.]138) at 2024-01-11 20:46:39 UTC

---

## 2. User Activity Correlation
**Purpose:** Review all activity from compromised account during incident timeframe
```splunk
index=cloud UserId="JChan@7pd6vr.onmicrosoft.com" 
earliest="2024-01-11T20:46:00" latest="2024-01-11T21:30:00"
| table _time, Operation, ClientIP, Parameters
| sort +_time
```

**Result:** Identified forwarding rule creation, email access, and send operations

---

## 3. Mailbox Rule Detection
**Purpose:** Detect creation of suspicious forwarding rules
```splunk
index=cloud Operation="New-InboxRule" UserId="JChan@7pd6vr.onmicrosoft.com"
| table _time, UserId, Parameters
| rex field=Parameters "ForwardTo=(?<ForwardTo>[^,]+)"
| where ForwardTo="stoicellis@imcourageous.com"
```

**Result:** Confirmed rule created at 20:47:15 UTC forwarding schan emails to attacker domain

---

## 4. Email Access Investigation
**Purpose:** Identify which emails were accessed by the attacker
```splunk
index=cloud UserId="JChan@7pd6vr.onmicrosoft.com" Operation="MailItemsAccessed"
ClientIP="188.214.125.138"
| table _time, Subject, Operation
```

**Result:** Two emails accessed - "RE: First Invoice of the month" and "URGENT: Client Bank Account"

---

## 5. Sent Email Analysis
**Purpose:** Detect fraudulent emails sent from compromised account
```splunk
index=cloud UserId="JChan@7pd6vr.onmicrosoft.com" Operation="Send"
ClientIP="188.214.125.138"
| table _time, Subject, Recipients, Attachments
```

**Result:** Malicious email sent with subject "URGENT: Client Bank Account" containing NEW-BANK-ACCOUNT.pdf

---

## 6. Draft Deletion Detection
**Purpose:** Identify potential counter-forensic activities
```splunk
index=cloud UserId="JChan@7pd6vr.onmicrosoft.com" Operation="SoftDelete"
| search Folder="Drafts"
| table _time, Subject, ClientIP
```

**Result:** Draft email "Re: First Invoice of the month!" deleted from drafts folder

---

## 7. IP Address Correlation
**Purpose:** Check if other accounts were compromised from same IP
```splunk
index=cloud ClientIP="188.214.125.138" Operation="UserLoggedIn"
| stats count by UserId
```

**Result:** No other accounts logged in from this IP address

---

## 8. OSINT - IP Geolocation
**Purpose:** Verify geographic origin of suspicious IP
```splunk
| makeresults 
| eval ip="188.214.125.138"
| iplocation ip
| table ip, Country, City
```

**Result:** Confirmed IP originates from Singapore
