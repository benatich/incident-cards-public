# Uber (2022): MFA fatigue

> ⏱️ ~5 min · 🟢 Easy

## Summary in 3 lines
In September 2022, an attacker reached Uber's internal systems using a contractor's password, likely bought on the dark web.
The attacker triggered repeated multifactor authentication (MFA) prompts until the contractor accepted one.
In its September 19 update, Uber said it had not seen access to production systems or user databases, with the investigation still ongoing.

## What happened
- **September 15, 2022**: Uber posts a first notice about a cybersecurity incident and says law enforcement has been contacted.
- **September 16**: Uber says its services are operational and that it has no evidence of access to sensitive user data.
- **September 19**: Uber details the incident. According to Uber, after the successful login the attacker accessed several employee accounts, gained elevated permissions on several tools (including G-Suite and Slack), posted a message in a company-wide Slack channel, and reconfigured OpenDNS to show an image on some internal sites.
- Uber also says the attacker downloaded some internal Slack messages, accessed or downloaded information from an internal invoicing tool, and accessed the HackerOne dashboard (the reports accessed having been remediated).
- Uber cautiously attributes the attack ("we believe") to an individual affiliated with the Lapsus$ group.
- **Uber's response**: Uber blocked or reset accounts, disabled internal tools, rotated keys, locked the codebase, and required employees to sign in again. Uber says its existing monitoring helped it identify the issue quickly.

Note: secondary sources also report that the attacker contacted the contractor directly by messaging, which Uber's update does not mention. This card therefore leaves it out of the steps table.

## Attack steps
> The ATT&CK codes show how this card classifies the actions described in the sources. “N/A” means no code was assigned to that step.

| # | Observed step | Source | ATT&CK technique | Mapping confidence |
|---|---|---|---|---|
| 1 | The contractor's corporate password was likely bought on the dark web after malware infected their personal device | Uber (primary) | T1589.001 | medium |
| 2 | The attacker repeatedly tries to log in; the contractor receives MFA approval requests and eventually accepts one | Uber (primary) | T1621 | high |
| 3 | The login succeeds with valid credentials | Uber (primary) | T1078 | high |
| 4 | The attacker accesses other employee accounts and gains elevated permissions (including G-Suite and Slack); Uber does not specify the method | Uber (primary) | N/A | low |
| 5 | Message in a company-wide Slack channel and OpenDNS change to show an image on internal sites | Uber (primary) | T1491.001 | medium |

## Detection leads
### Lead 1: burst of MFA requests followed by an approval
- **Signal:** many denied or ignored MFA approval requests for one account in a short time, then an approval.
- **Log:** authentication logs from the identity provider or MFA service.
- **Visibility condition:** request, denial and approval events must be logged with timestamp and account identifier.
- **Status:** proposed lead

### Lead 2: unusual sign-in followed by sensitive access
- **Signal:** a successful sign-in from an unfamiliar device or location is followed by access to a sensitive internal tool.
- **Log:** identity provider sign-in logs and application audit logs.
- **Visibility condition:** both logs record an account identifier and timestamp, and the security team knows which tools are sensitive.
- **Status:** proposed lead. Uber reported subsequent access to internal tools, while its public update gives no details on the alert that led to detection.

## Lessons learned
1. Prefer phishing-resistant MFA. CISA recommends number matching as an interim measure when an organization must use push-based MFA.
2. Include contractor accounts and devices in access reviews. Uber assessed that malware on a personal device exposed the contractor's credentials.
3. Prepare for containment: Uber blocked accounts, rotated keys, locked the codebase and required re-authentication. Being able to do this quickly saves time.

## Sources
- [Uber: Security update](https://www.uber.com/us/en/newsroom/security-update/): primary (company statement, September 19, 2022 update)
- [NHI Management Group: Uber Breach 2022](https://nhimg.org/uber-breach): secondary (third-party analysis)
- [CISA: Implementing number matching in MFA applications](https://www.cisa.gov/sites/default/files/publications/fact-sheet-implement-number-matching-in-mfa-applications-508c.pdf): defensive guidance

## Card quality
Uber's September 19 update reflects an investigation that was still underway. It describes the account compromise and the response without identifying the monitoring signal.
