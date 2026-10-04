# KQL Hunting Queries

Free, readable KQL hunting queries for Microsoft Sentinel and Defender. Each one is short enough to understand at a glance, commented step by step, and comes with a full write-up explaining how it was built and what to do with what it finds.

They're the queries from the [KQL Hunting series](https://alanconlon.com/series/kql-hunting/) on [alanconlon.com](https://alanconlon.com). New ones land here as each post goes live.

## The queries

| Query | Finds | MITRE ATT&CK | Write-up |
|---|---|---|---|
| [Failed sign-ins by user](queries/getting-started/failed-sign-ins-by-user.kql) | Accounts with the most failed sign-ins | – | [KQL primer](https://alanconlon.com/posts/sentinel-kql-primer/) |
| [MFA fatigue burst](queries/mfa-fatigue/mfa-fatigue-burst.kql) | Users hit with a burst of denied MFA prompts | [T1621](https://attack.mitre.org/techniques/T1621/) | [Hunting MFA fatigue](https://alanconlon.com/posts/hunting-mfa-fatigue-kql/) |
| [MFA fatigue then success](queries/mfa-fatigue/mfa-fatigue-then-success.kql) | A prompt burst followed by a successful sign-in | [T1621](https://attack.mitre.org/techniques/T1621/) | [Hunting MFA fatigue](https://alanconlon.com/posts/hunting-mfa-fatigue-kql/) |
| [Password spray by IP](queries/password-spray/password-spray-by-ip.kql) | One source failing against many distinct accounts | [T1110.003](https://attack.mitre.org/techniques/T1110/003/) | [Hunting password spray](https://alanconlon.com/posts/hunting-password-spray-kql/) |
| [Password spray then success](queries/password-spray/password-spray-then-success.kql) | Successful sign-ins from an IP that was spraying | [T1110.003](https://attack.mitre.org/techniques/T1110/003/) | [Hunting password spray](https://alanconlon.com/posts/hunting-password-spray-kql/) |

## Using them

All of them run against the `SigninLogs` table, so you need the Microsoft Entra ID data connector sending sign-in logs to your Sentinel workspace (or any Log Analytics workspace).

1. Open **Logs** in Sentinel (or **Advanced hunting** in Defender, if your Entra sign-in data is there).
2. Paste the query and run it.
3. Tune the `let` values at the top. Every query has its lookback window and threshold pulled out as named variables, so you can adjust them without reading the logic first.

**Thresholds are starting points, not answers.** A threshold that's quiet in a 500-person organisation will be noisy in a 50,000-person one. Run a query over a few weeks of history, see what normal looks like for you, then set the number just above it. The write-ups go into how to do that for each one.

**Hunting first, alerting second.** Run them by hand until you trust what they return. Only then turn one into a scheduled analytics rule, and when you do, the "then success" variants are the ones worth alerting on. The others are for looking around.

## Licence

MIT. Use them, change them, put them in your own rules and repos. A link back is appreciated but not required.

---

More practical security, Azure and data write-ups at [alanconlon.com](https://alanconlon.com), a new one every other Tuesday.
