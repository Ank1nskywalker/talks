# Rubric · Writing the Incident Story

*[Versión en español](rubrica.md)*

Eight checks before you send a report. They are not about style: each one answers one of the readers from the talk (the decision-maker, the SOC, the next hunter and the auditor). Use it to review your own report before sharing it on Discord; we review it with the same list.

Each check is scored **Meets**, **Partial** or **Missing**.

| # | Check | Reader | Meets | Usual miss |
|---|---|---|---|---|
| 1 | The executive summary fits on one page and opens with the verdict | Decision-maker | The first sentence says what happened and how sure you are. Someone who does not read KQL can decide from this page alone | It opens with the methodology, or the verdict is in the third paragraph |
| 2 | Every finding has a verdict, evidence and a confidence level | SOC | Each finding says malicious, benign or inconclusive, points to the query and the event that support it, and uses calibrated language (ICD 203) | "Suspicious activity was observed", with no verdict and no confidence |
| 3 | Facts are kept apart from assessments | Everyone | What the telemetry shows and what you infer from it are written separately and can be told apart at a glance | "csc.exe was injected" written as a fact when the telemetry only shows a connection |
| 4 | Limitations and gaps are written down, not implied | Next hunter, auditor | Sources not ingested, blind spots of the queries and noise you excluded are listed, with their impact on the conclusion | An empty query reported as "nothing found" without saying whether the source existed |
| 5 | Every recommendation has a priority, an owner and a status | Decision-maker | Each action can be assigned tomorrow: what, who, how urgent, and whether it is done | "Improve monitoring" |
| 6 | The queries in the appendix can be re-run as they are | Next hunter | Full queries, with time window and platform, that return the same result when pasted | Screenshots of results, or queries cut to the interesting line |
| 7 | The victim's data is anonymized | Auditor | Users, hostnames, internal IPs and organization names are replaced with placeholders; IOCs of the attacker are kept | A real username or hostname in a screenshot or in the timeline |
| 8 | The TLP is marked on the cover | Everyone | The TLP level is on the cover and matches what the report contains | No TLP, or TLP:CLEAR on a report with unredacted internal data |

**A hunt with no findings still gets reported.** If your queries came back empty, the report says which ones, why you trust that result (positive control) and what you could not see. That is a complete report.
