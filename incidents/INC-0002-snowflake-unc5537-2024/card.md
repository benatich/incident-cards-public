# Snowflake / UNC5537 (2024): stolen credentials, no MFA

> ⏱️ ~5 min · 🟡 Intermediate

## Summary in 3 lines
In 2024, a financially motivated actor that Mandiant tracks as UNC5537 accessed customers' Snowflake instances using credentials stolen by infostealer malware, then exfiltrated data and tried to extort victims.
Mandiant found no evidence that Snowflake's own environment was breached: every incident it investigated traced back to compromised customer credentials.
Mandiant and Snowflake notified about 165 potentially exposed organizations.

## What happened
- **April 2024**: Mandiant receives intelligence about records originating from a victim's Snowflake instance. The investigation shows stolen infostealer credentials were used on an account without multi-factor authentication.
- **May 22, 2024**: new intelligence reveals a broader campaign; Mandiant contacts Snowflake and notifies potential victims.
- **May 30, 2024**: Snowflake publishes detection and hardening guidance.
- **June 10, 2024**: Mandiant publishes its report; a June 17 update adds a threat hunting guide.
- According to Mandiant, three main factors explain the compromises: accounts without MFA, stolen credentials that remained valid (sometimes for years), and no network allow lists.
- According to Mandiant and Snowflake, at least 79.7% of the accounts used had prior credential exposure; the earliest infostealer infection observed dates to November 2020.
- Mandiant also notes that contractor computers, sometimes used for personal activities, may have been the source of the infections.

## Attack steps
> The ATT&CK codes show how this card classifies the actions described in the sources. “N/A” means no code was assigned to that step.

| # | Observed step | Source | ATT&CK technique | Mapping confidence |
|---|---|---|---|---|
| 1 | Snowflake credentials exposed by infostealers (some since 2020) are gathered by the attacker | Mandiant (primary) | T1589.001 | medium |
| 2 | Login with valid credentials on accounts without MFA, rotation or network allow lists | Mandiant (primary) | T1078.004 | high |
| 3 | The actor enumerated users in affected Snowflake instances. Mandiant also observed queries about roles and other session details. | Mandiant (primary) | T1087.004 | high |
| 4 | The actor prepared and compressed exported data in temporary Snowflake storage. | Mandiant (primary) | T1074.002 | medium |
| 5 | The actor downloaded the prepared files to local directories. Mandiant also describes sales and extortion attempts. | Mandiant (primary) | N/A | low |

## Detection leads
### Lead 1: unusual client applications in sessions
- **Signal:** unexpected client application IDs or client environment (for example a database client the organization does not use, or an application name specific to the attacker).
- **Log:** Snowflake session and login log (Mandiant's examples show "Sessions" type entries).
- **Visibility condition:** history must be accessible; according to Mandiant, default retention of the relevant views allows hunting across the past 365 days.
- **Status:** observed during the incident. Mandiant published examples of client information recorded in affected sessions.

### Lead 2: unusual preparation and download of data
- **Signal:** an account prepares data for export and then downloads it in a session with an unfamiliar source IP or client.
- **Log:** Snowflake query, session, and sign-in history.
- **Visibility condition:** retain enough history to join the events by account or session and compare them with typical activity.
- **Status:** proposed lead

## Lessons learned
1. Enforce multi-factor authentication on all accounts: it is the first factor Mandiant cites.
2. Rotate credentials and monitor their exposure: some stolen credentials remained valid for years.
3. Restrict network access to trusted locations (allow lists).

## Sources
- [Mandiant: UNC5537 Targets Snowflake Customer Instances for Data Theft and Extortion](https://cloud.google.com/blog/topics/threat-intelligence/unc5537-snowflake-data-theft-extortion): primary (June 10, 2024 report, updated June 17)
- [Snowflake: detecting and preventing unauthorized access](https://community.snowflake.com/s/question/0D5VI00000Emyl00AB/detecting-and-preventing-unauthorized-user-access): additional vendor guidance cited by Mandiant

## Card quality
Mandiant's findings describe the customer environments it investigated. The report documents downloads to local directories. The table leaves the transfer unmapped because the evidence cited does not establish an ATT&CK technique for that step.
