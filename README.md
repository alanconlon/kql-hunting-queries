# KQL Hunting Queries

Free, readable KQL hunting queries for Microsoft Sentinel and Defender. Each one is short enough to understand at a glance, commented step by step, and comes with a full write-up explaining how it was built and what to do with what it finds.

They're the queries from the [KQL Hunting series](https://alanconlon.com/series/kql-hunting/) and the [Entra Identity series](https://alanconlon.com/series/entra-identity/) on [alanconlon.com](https://alanconlon.com). New ones land here as each post goes live.

New to hunting? [Start with the guide](https://alanconlon.com/kql-threat-hunting/): what to hunt first, and in what order. Once a query works, the [free KQL Detection Tuning Checklist](https://alanconlon.com/free-checklist/) covers turning it into an alert your team still trusts.

## The queries

| Query | Finds | MITRE ATT&CK | Write-up |
|---|---|---|---|
| [Failed sign-ins by user](queries/getting-started/failed-sign-ins-by-user.kql) | Accounts with the most failed sign-ins | – | [KQL primer](https://alanconlon.com/posts/sentinel-kql-primer/) |
| [MFA fatigue burst](queries/mfa-fatigue/mfa-fatigue-burst.kql) | Users hit with a burst of denied MFA prompts | [T1621](https://attack.mitre.org/techniques/T1621/) | [Hunting MFA fatigue](https://alanconlon.com/posts/hunting-mfa-fatigue-kql/) |
| [MFA fatigue then success](queries/mfa-fatigue/mfa-fatigue-then-success.kql) | A prompt burst followed by a successful sign-in | [T1621](https://attack.mitre.org/techniques/T1621/) | [Hunting MFA fatigue](https://alanconlon.com/posts/hunting-mfa-fatigue-kql/) |
| [Password spray by IP](queries/password-spray/password-spray-by-ip.kql) | One source failing against many distinct accounts | [T1110.003](https://attack.mitre.org/techniques/T1110/003/) | [Hunting password spray](https://alanconlon.com/posts/hunting-password-spray-kql/) |
| [Password spray then success](queries/password-spray/password-spray-then-success.kql) | Successful sign-ins from an IP that was spraying | [T1110.003](https://attack.mitre.org/techniques/T1110/003/) | [Hunting password spray](https://alanconlon.com/posts/hunting-password-spray-kql/) |
| [Break-glass sign-ins](queries/break-glass/break-glass-sign-ins.kql) | Any successful sign-in to an emergency-access account | [T1078.004](https://attack.mitre.org/techniques/T1078/004/) | [Break-glass accounts](https://alanconlon.com/posts/break-glass-accounts/) |

## Using them

All of them run against the `SigninLogs` table. Before you start you need:

- The Microsoft Entra ID data connector sending sign-in logs (`SigninLogs`) to your Sentinel workspace (or any Log Analytics workspace).
- Read access to that workspace, enough to run queries in **Logs**.

1. Open **Logs** in Sentinel or your Log Analytics workspace.
2. Paste the query and run it.
3. Tune the `let` values at the top. Every query has its lookback window and threshold pulled out as named variables, so you can adjust them without reading the logic first.

**Defender Advanced Hunting needs changes.** Advanced hunting keeps Entra sign-ins in the [`EntraIdSignInEvents`](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-entraidsigninevents-table) table, which needs a Microsoft Entra ID P2 licence. It replaces the older [`AADSignInEventsBeta`](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-aadsignineventsbeta-table) table, which Microsoft deprecates on 19 October 2026. Both use different column names and types from `SigninLogs`, so the queries won't run there as written. Check the schema on those pages and adapt the columns before you rely on the results.

**Thresholds are starting points, not answers.** A threshold that's quiet in a 500-person organisation will be noisy in a 50,000-person one. Run a query over a few weeks of history, see what normal looks like for you, then set the number just above it. The write-ups go into how to do that for each one.

**Hunting first, alerting second.** Run them by hand until you trust what they return. Only then turn one into a scheduled analytics rule, and when you do, the "then success" variants are the ones worth alerting on. The others are for looking around.

## Planned hunts

These are next. Each query file lands here when its post goes live, not before.

- **Impossible travel**: sign-ins for one account from places too far apart for the time between them (`SigninLogs`).
- **Privileged role changes**: accounts added to admin roles, especially outside normal change windows (`AuditLogs`).
- **Malicious inbox rules**: rules that forward, delete or hide mail, a common step after a mailbox is taken over (`OfficeActivity`).

## File format

Every query file follows the same layout, so you can read any of them the same way:

1. A one-line title comment, then a header block between dashed lines with:
   - **What it finds**: a short description of the behaviour and why it matters.
   - **Table**: the table it queries.
   - **MITRE ATT&CK**: the technique ID and name, where one fits.
   - **Written up in**: a link to the post that explains it.
2. `let` variables at the top for the lookback window, thresholds and anything you'd change for your own tenant, each with a short comment.
3. `// Step 1:`, `// Step 2:` comments through the logic when a query has more than one stage.

New queries use the same layout.

## Changelog

- **Pending**: Break-glass sign-ins query from the Entra Identity series ([PR #1](https://github.com/alanconlon/kql-hunting-queries/pull/1)).
- **2026-10-10**: README links the start-here guide and the tuning checklist.
- **2026-10-04**: `ResultType` values quoted as strings (`"0"`, `"500121"`), since the column is a string.
- **2026-10-04**: Initial queries from the KQL Hunting series.

## Licence

MIT. Use them, change them, put them in your own rules and repos. A link back is appreciated but not required.

---

More practical security, Azure and data write-ups at [alanconlon.com](https://alanconlon.com), a new one every other Tuesday.
