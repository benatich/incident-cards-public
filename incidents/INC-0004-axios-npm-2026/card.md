# Axios (2026) — Two malicious releases on npm

> 5 min · Easy

## Summary in 3 lines

Axios is a JavaScript library distributed through the npm registry.\
On March 31, 2026, two malicious versions were published from the lead maintainer’s compromised account.\
External reports preceded their removal from the registry. [1]

## What happened

- **March 31, 00:21 and around 01:00 UTC.** Versions `1.14.1` and `0.30.4` are published with a malicious dependency capable of installing remote access malware. [1][2]
- **Around 01:00 UTC.** External detections and community reports begin. [1]
- **01:38 UTC.** A project collaborator opens a pull request to deprecate the affected versions. They also contact npm. [1]
- **Around 03:15 UTC, according to Axios.** The two versions are removed from the registry. The exact time of the initial compromise remains unknown. [1]

## Attack steps

> ATT&CK mapping proposed by the card author. “N/A” indicates that no technique is assigned to that step.

| # | Documented step | Source | ATT&CK technique | Mapping confidence |
|---|---|---|---|---|
| 1 | The maintainer reports being targeted by a social engineering campaign before the releases. | [1] | N/A | low |
| 2 | The maintainer’s legitimate account is used to publish to npm. | [1] | T1078 | high |
| 3 | A malicious dependency is added to two Axios releases published on npm. | [1][2] | T1195.001 | high |

## Detection leads

### Lead 1 — Unexpected release

- **Signal:** a new registry version appears without a matching release approved by the project.
- **Log:** npm version history and the build pipeline’s release log.
- **Visibility condition:** keep a record of authorized releases and monitor new versions in the registry.
- **Status:** proposed lead. External reports are documented, and the maintainer says no automated alert of this kind was in place. [1]

### Lead 2 — Installation of an affected release

- **Signal:** Axios `1.14.1` or `0.30.4`, or the dependency `plain-crypto-js@4.2.1`, appears in build or installation records.
- **Log:** build and installation logs. Use lockfiles and dependency caches as leads when investigating exposure.
- **Visibility condition:** retain records of the version installed in the affected environment, including transitive dependencies.
- **Status:** proposed lead based on CISA guidance. Installation indicates a potential compromise. Also look for unexpected processes or unusual connections during installation to help determine whether the malware ran. [2]

## Prevention

Restrict publishing to approved workflows. As a consumer, pin dependencies to known-safe versions and review changes in the lockfile, which records their exact versions. [1][2]

## Lessons learned

1. Compare what the project approved with what actually appeared in the registry.
2. If installation is confirmed, investigate the affected environments and restore them to a known-safe state. Revoke or rotate potentially exposed secrets. For a compromised ephemeral CI job, rotate every secret injected into that job. [2]

## Test yourself

1. Where were the malicious releases published?
2. How was the anomaly detected?
3. What can help identify an affected installation?

<details>
<summary>Show answers</summary>

1. In the npm registry.
2. External observers and community members reported the suspicious versions.
3. Lockfiles and caches guide the investigation. Installation or build logs help verify which version was installed in the environment.

</details>

## Sources

- [1] [Axios maintainer’s postmortem](https://github.com/axios/axios/issues/10636) — primary source.
- [2] [CISA alert and recommendations](https://content.govdelivery.com/accounts/USDHSCISA/bulletins/413c7a5) — official advisory.
- MITRE ATT&CK — [T1078](https://attack.mitre.org/techniques/T1078/) and [T1195.001](https://attack.mitre.org/techniques/T1195/001/).

## Card quality

Sources checked on November 12, 2026. Independent review completed and corrections incorporated.
