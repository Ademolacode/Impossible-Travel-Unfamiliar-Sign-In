### Investigation Queries

**1. Identify Foreign Logins:**
`index=cloud Operation="UserLoggedIn" | stats count by UserId, ClientIP, Location, _time`

**2. Detect Rule Creation:**
`index=cloud Operation="New-InboxRule" | table _time, UserId, Parameters`

**3. Monitor Email Activity:**
`index=cloud UserId="JChan@7pd6vr.onmicrosoft.com" (Operation="MailItemsAccessed" OR Operation="Send") | table _time, Operation, Subject, Attachments`
