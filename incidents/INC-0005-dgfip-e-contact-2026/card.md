# DGFiP (2026) — Taxpayer records stolen from E-Contact

> 5 min · Intermediate

## Summary in 3 lines

Stolen staff credentials gave an attacker access to E-Contact, an application France's tax authority (DGFiP) uses to communicate with taxpayers.\
France's national cybersecurity agency (ANSSI) reconstructed two periods of data theft in June and July 2026. Monitoring detected suspicious searches but missed the data transfers.\
In June, a password reset failed to terminate an active session, and the transfer continued. [1, pp. 4–5, 17–19]

## What happened

- **May.** Investigators found suspicious logins and early access to E-Contact. ANSSI assessed that credential-stealing malware on unmanaged devices was a likely source of stolen credentials. [1, pp. 4, 6, 15–16]
- **June 23–25.** Suspicious searches on the public finance portal (PIGP) triggered a security ticket on June 23. The attacker reached the separate ADER portal through the State Interministerial Network (RIE), using access gained through a compromised government network. The June 24 password reset left an ADER session active, and the E-Contact transfer continued until June 25. [1, pp. 4–5, 8, 17–18]
- **July 22–24.** Automated extraction resumed on July 22. The security team flagged suspicious searches the next day, then reset an account and blocked an IP address on July 24. [1, p. 19]
- **August 12–13.** The attacker publicly claimed the theft on August 12. ANSSI links the claim to the June transfer. The tax authority disabled staff access to the portal used for extraction on August 13. [1, pp. 4, 19–20]

## Attack steps

> The ATT&CK codes show how this card classifies the actions described in the sources. “N/A” means no code was assigned to that step. The confidence column applies to the code assignment.

| # | Documented step | Source | ATT&CK technique | Mapping confidence |
|---|---|---|---|---|
| 1 | The attacker signed in using compromised accounts. | [1, pp. 15–17] | T1078 | high |
| 2 | The attacker used automated requests to collect E-Contact records. | [1, pp. 17–19] | T1119 | high |
| 3 | Investigators confirmed outbound data transfers. Details of the extraction are redacted in the public report. | [1, pp. 17–19] | N/A | low |

## Detection leads

### Lead 1 — Suspicious searches

- **Signal:** an account runs unusual searches on the monitored portal.
- **Log:** search activity and alert records from the monitored portal.
- **Visibility condition:** the portal logs searches by account and sends alerts to the security team.
- **Status:** observed during the incident. A PIGP search alert generated a ticket. The security team did not monitor ADER at the time, and the transfer went undetected. [1, pp. 5, 17–18]

### Lead 2 — Unusual access followed by bulk retrieval

- **Signal:** an unusual login followed by a spike in requests or data transferred by the same account or session.
- **Log:** authentication, application access, and network traffic logs.
- **Visibility condition:** the portal serving the data is monitored, and analysts can link authentication and application events by account or session, then compare traffic volumes by time and source.
- **Status:** proposed lead based on the signals ANSSI identified retrospectively. [1, pp. 5, 7–8]

## Prevention

Require strong multifactor authentication and managed devices for internal access. Centralize application logs and set limits on request or data volumes. [1, pp. 10–11]

## Lessons learned

1. When resetting a compromised account's password, revoke active sessions across affected applications. [1, pp. 12, 18]
2. Correlate identity and application activity across portals to establish whether suspicious searches accompanied data theft. [1, pp. 5, 7–8]

## Test yourself

1. What activity generated a security ticket?
2. Why did the June password reset fail to stop the transfer?
3. Which logs would help link a login to bulk retrieval?

<details>
<summary>Show answers</summary>

1. Suspicious searches on a monitored portal.
2. The active session remained valid.
3. Authentication, application access, and network traffic logs.

</details>

## Sources

- [1] [ANSSI incident report, 23 September 2026](https://cyber.gouv.fr/documents/821/20260923_NP_TLPCLEAR_ANSSI_Rapport_incident_DGFIP.pdf) — primary source. References use the report's printed page numbers. The report is also linked from the [ANSSI publication page](https://cyber.gouv.fr/actualites/lanssi-publie-le-rapport-dincident-sur-les-cyberattaques-ayant-touche-la-dgfip/).
- MITRE ATT&CK — [T1078 Valid Accounts](https://attack.mitre.org/techniques/T1078/) and [T1119 Automated Collection](https://attack.mitre.org/techniques/T1119/).

## Card quality

This card covers E-Contact. The ANSSI report describes the land registry incident separately. Redactions in the public report limit how precisely the outbound transfer can be mapped to ATT&CK. [1, pp. 17–20]
