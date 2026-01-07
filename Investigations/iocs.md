# Indicators of Compromise (IOCs)

## Network Indicators
| Type | Value | Context |
|------|-------|---------|
| IP Address | `188.214.125[.]138` | Attacker source IP (Singapore) |
| Domain | `imcourageous[.]com` | Attacker-controlled email domain |

## Email Indicators
| Type | Value | Context |
|------|-------|---------|
| Email Address | `stoicellis@imcourageous[.]com` | Forwarding rule destination |
| Subject Line | `URGENT: Client Bank Account` | Fraudulent email subject |

## File Indicators
| Type | Value | Context |
|------|-------|---------|
| Filename | `NEW-BANK-ACCOUNT.pdf` | Malicious attachment (banking fraud) |


## Account Indicators
| Type | Value | Context |
|------|-------|---------|
| Compromised Account | `JChan@7pd6vr.onmicrosoft[.]com` | Victim account |
| Targeted Account | `schan@7pd6vr.onmicrosoft[.]com` | Forwarding rule target |
