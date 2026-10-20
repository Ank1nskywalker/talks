# Writing the Incident Story

**Professional Reporting for Malware and Cyber Attacks** · DEATHCon 2026 · November 13–14 · Online + Córdoba

*[Versión en español](README.es.md)*

A self-guided workshop on writing threat hunting reports that someone can act on. You hunt for a ClickFix intrusion in Microsoft Sentinel with seed queries, and what you find becomes the raw material for a report: executive summary first, facts kept apart from assessments, and a confidence level on every finding.

The hunt is the means. **The report is the deliverable.**

---

## What's in this folder

| | |
|---|---|
| [Slides (EN)](slides/Writing%20the%20Incident%20Story%20%28EN%29.pdf) · [PPTX](slides/Writing%20the%20Incident%20Story%20%28EN%29.pptx) | The talk, with speaker notes |
| [Slides (ES)](slides/Writing%20the%20Incident%20Story%20%28ES%29.pdf) · [PPTX](slides/Writing%20the%20Incident%20Story%20%28ES%29.pptx) | La charla en español |
| [queries/](queries/) | The seed queries, one per file, ready to paste into Sentinel |
| [rubric.md](rubric.md) | The checklist your report is reviewed against |
| [Model report and template](https://github.com/Ank1nskywalker/Templates) | What a finished report looks like, in English and Spanish (Word and PDF) |

---

## Before you start

- **Lab access.** Use the Microsoft Sentinel lab access provided by DEATHCon. The queries use `TimeGenerated`; in Defender XDR advanced hunting, use `Timestamp` instead.
- **The template.** Download the model report from the [Templates](https://github.com/Ank1nskywalker/Templates) repo, in [English](https://github.com/Ank1nskywalker/Templates/blob/main/Threat%20Hunting%20Report%20Template%20-%20ClickFix%20RunMRU%20-%20Sentinel%20%28EN%29.docx) or [Spanish](https://github.com/Ank1nskywalker/Templates/blob/main/Plantilla%20Informe%20Threat%20Hunting%20-%20ClickFix%20RunMRU%20-%20Sentinel%20%28ES%29.docx). Keep the structure and replace the content with your own hunt.
- **No KQL experience needed.** The queries are written for you. Your time goes into reading results and writing, not fighting syntax.

---

## The hypothesis

> If someone pasted a command from a ClickFix lure, we will see an interpreter launched by Explorer (Win+R) or by the browser (FileFix) with a download command and, seconds later, the chain that command triggers.

| | |
|---|---|
| **Data source** | EDR process, network, file and registry events across the fleet: the events, not the alerts |
| **Confirmed by** | A host with an interpreter that downloads and executes, followed by the chain it launches |
| **Refuted by** | Zero results, with the queries already validated by the positive control |
| **Known gap** | The clipboard is not logged. RunMRU only sees Win+R; a paste into Terminal (Win+X) is only seen by Script Block Logging |
| **ATT&CK** | T1204.004 → T1059.001 · T1564.003 · T1218.007 · T1055 · T1176.001 |

---

## The exercise

1. **Watch the talk** or read the slides. Pay attention to the writing section: that is what you will be reviewed on.
2. **Run the positive control first** ([Q0a](queries/00a-table-coverage.kql), [Q0b](queries/00b-reporting-hosts.kql)). Confirm the tables have data and see which hosts report. Zero results without a positive control prove nothing.
3. **Run the hunting queries in order.** Each result raises the next question. When a query finds something, note the host and the time and use them to scope the next one.
4. **Classify every result** as malicious, benign or inconclusive, and write down why. The lab is shared with other workshops, so you will see activity that looks malicious and is not. Noise you document is part of the story.
5. **Write the report** with the template. The executive summary goes first and opens with the verdict. Mark the TLP on the cover.
6. **Review it against the [rubric](rubric.md)** and share it in the workshop channel on the DEATHCon Discord. We review it with the same rubric.

You don't need to find everything. An honest report about what you found and what you could not see is worth more than a page of screenshots.

### Seed queries

| # | Question | What it looks for | Table | File |
|---|---|---|---|---|
| 0a | Can I see? | Data in each table the hunt depends on | Device* | [00a](queries/00a-table-coverage.kql) |
| 0b | Which hosts? | Hosts that send Defender telemetry | DeviceInfo | [00b](queries/00b-reporting-hosts.kql) |
| 1 | Did anyone paste a command? | Explorer or the browser launches an interpreter with a download command | DeviceProcessEvents | [01](queries/01-lineage.kql) |
| 1b | Was it the Run dialog? | Explorer writes the command to RunMRU | DeviceRegistryEvents | [01b](queries/01b-runmru.kql) |
| 2 | Why didn't they see it? | A script that hides its own console | Event (4104) | [02](queries/02-hidden-console-4104.kql) |
| 2b | Same, without 4104 | The cmdlets the sensor recorded around the Q1 hit | DeviceEvents | [02b](queries/02b-powershellcommand.kql) |
| 3 | What came next? | msiexec launches a hidden PowerShell | DeviceProcessEvents | [03](queries/03-msiexec-hidden-powershell.kql) |
| 4 | Who did it talk to? | A .NET binary (csc, ilasm…) with a network connection | DeviceNetworkEvents | [04](queries/04-dotnet-outbound.kql) |
| 5 | Did it locate itself? | A non-browser process checks its public IP | DeviceNetworkEvents | [05](queries/05-public-ip-check.kql) |
| 6 | How does it stay? | A non-browser process modifies the browser profile | DeviceFileEvents | [06](queries/06-browser-preferences.kql) |
| 7 | Next hunt | An extension manifest created outside the browser | DeviceFileEvents | [07](queries/07-manifest-json.kql) |

The queries look back 30 days (`ago(30d)`). If you run them later than the event, widen the window.

### Things the lab will teach you

- **Not every source is ingested.** If the PowerShell operational log (4104) is not in the `Event` table, Q2 cannot find anything. Use Q2b and write the gap in *Scope and Limitations*. A gap you declare is a finding; a gap you hide is a false negative.
- **Network events can be aggregated.** Short connections may only appear as `ConnectionSuccessAggregatedReport`, reported minutes later. That is why Q4 does not filter by `ActionType`.
- **Browsers write their profile with a temp file and a rename.** A change to `Preferences` may show up as `FileRenamed`. That is why Q6 does not filter by `ActionType` either.
- **An empty result is a result** only if the positive control worked. Say which one it was.

---

## What a good report looks like

The [model report](https://github.com/Ank1nskywalker/Templates) applies everything in the talk to a real ClickFix case (TLP:CLEAR, victim data anonymized): BLUF executive summary, hypothesis with confirmation and refutation criteria, scope and limitations, findings with a verdict and a confidence level (ICD 203), facts separated from assessments, ACH for competing hypotheses, an action plan with owners, and appendices with IOCs, re-runnable queries, a timeline and evidence of controls.

Use it as a reference, not as a form to fill in: your hunt will find different things, and your report should say so.

---

## References

- Splunk SURGe, [PEAK Threat Hunting Framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html) (2023)
- D. J. Bianco, [The Pyramid of Pain](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html) (2013)
- ODNI, [ICD 203: Analytic Standards](https://www.dni.gov/files/documents/ICD/ICD-203.pdf)
- MITRE ATT&CK, [T1204.004 User Execution: Malicious Copy and Paste](https://attack.mitre.org/techniques/T1204/004/)
- mrd0x, [FileFix: a ClickFix alternative](https://mrd0x.com/filefix-clickfix-alternative/)
- Microsoft, [DeviceRegistryEvents](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-deviceregistryevents-table) · [PowerShell logging on Windows](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging_windows)

---

David Pérez · [@anakinswal](https://x.com/anakinswal) · Shared for educational purposes. If you reuse it, attribution is appreciated.
