# DFIR Cedarpelta Analysis

Codex skill for analyzing Cedarpelta Build / Live Response Collection evidence from Windows endpoints during digital forensics and incident response (DFIR) investigations.

This skill is an investigation playbook, not an automated verdict engine. It helps a DFIR analyst review artifact coverage, form and test hypotheses, pivot across Windows evidence, assess confidence, and document follow-up actions.

## Purpose

`dfir-cedarpelta-analysis` provides a structured workflow for investigating Windows endpoint evidence collected by Cedarpelta. It focuses on disciplined analysis of available artifacts and clear separation between observed facts, analytical inferences, unresolved hypotheses, limitations, and recommended next steps.

Use this skill when you need to:

- Review Cedarpelta collection coverage before analysis
- Identify which Windows artifacts are available, missing, empty, or parser-heavy
- Investigate suspicious execution, persistence, account activity, lateral movement, defense evasion, suspicious tools, or data staging
- Cross-check findings across independent artifact groups
- Build evidence-backed pivots from timestamps, users, hosts, processes, paths, registry keys, services, scheduled tasks, network indicators, or file artifacts
- Assess confidence levels for DFIR findings
- Produce follow-up actions for incomplete or ambiguous evidence
- Control closure so an investigation is not marked clean or complete without sufficient evidence

## Repository Layout

```text
dfir-cedarpelta-analysis/
|-- SKILL.md
|-- README.md
|-- CONTRIBUTING.md
|-- references/
|   |-- artifact_groups.md
|   |-- cedarpelta_analysis_sop.md
|   |-- cedarpelta_collection_artifact_map.md
|   |-- cedarpelta_stepwise_report_template.md
|   |-- cross_check_rules.md
|   |-- hunting_scenarios.md
|   |-- hypothesis_lifecycle.md
|   |-- investigator_tool_readiness.md
|   |-- parser_policy.md
|   `-- pivot_workflow.md
`-- schemas/
    |-- cedarpelta_findings.schema.json
    |-- follow_up_action.schema.json
    |-- normalized_event.schema.json
    |-- parser_recommendation_matrix.json
    `-- timeline_event.schema.json
```

## Key Files

| File | Description |
| --- | --- |
| `SKILL.md` | Main Codex skill metadata, trigger guidance, and reference loading rules |
| `references/cedarpelta_analysis_sop.md` | Primary SOP and authority for investigation order |
| `references/cedarpelta_collection_artifact_map.md` | Mapping of Cedarpelta collection output paths to artifact families, parser class, and coverage notes |
| `references/cedarpelta_stepwise_report_template.md` | Working template for recording completed SOP steps |
| `references/artifact_groups.md` | Required artifact groups and coverage notes |
| `references/cross_check_rules.md` | Mandatory independent checks for logon, malware/execution, persistence, and exfiltration scenarios |
| `references/hunting_scenarios.md` | Scenario catalog for logon, credential access, PowerShell/script execution, malware, persistence, lateral movement, exfiltration preparation, evasion, reconnaissance, and suspicious tools |
| `references/hypothesis_lifecycle.md` | Hypothesis states, transition rules, required fields, and closure rules |
| `references/investigator_tool_readiness.md` | Workstation readiness and parser/tool availability guide |
| `references/parser_policy.md` | Parser decision rules, query logging, parsed subset handling, and normalized output expectations |
| `references/pivot_workflow.md` | Pivot seed types, default time windows, and pivot-chain documentation |
| `schemas/*.json` | Output contracts for findings, follow-up actions, normalized events, timeline events, and parser recommendations |

## SOP Authority

The main investigation flow is governed by `references/cedarpelta_analysis_sop.md`.

Use focused references for supporting details, but when guidance conflicts or the correct order is unclear, follow the SOP first.

Core loop:

```text
Evidence -> Hypothesis -> Cross-check -> Pivot -> Finding -> Limitation -> Follow-up -> Closure decision
```

Never conclude that a case is clean only because one artifact source is clean. Missing evidence reduces confidence unless it is explicitly not applicable.

## Installation

Copy the skill directory into your Codex skills directory.

Example:

```powershell
Copy-Item `
  -Recurse `
  -Path .\dfir-cedarpelta-analysis `
  -Destination "$env:USERPROFILE\.codex\skills\dfir-cedarpelta-analysis"
```

Restart Codex after copying the skill so it can be discovered.

## Usage

Invoke the skill when working on Cedarpelta evidence or when coordinating a DFIR case that includes Cedarpelta Build / Live Response Collection output.

Example prompts:

```text
Use dfir-cedarpelta-analysis to review this Cedarpelta collection and identify likely persistence mechanisms.
```

```text
Analyze the Cedarpelta artifacts for suspicious execution and produce evidence-backed findings with confidence levels.
```

```text
Review artifact coverage first, then tell me which hypotheses can and cannot be tested from this collection.
```

## Recommended Workflow

1. Confirm collection scope

   Identify the host, collection time, source path, and available Cedarpelta artifacts.

2. Review evidence availability

   Use the artifact map and artifact groups to determine which investigation questions can be answered from the available data.

3. Establish a host baseline

   Record the baseline host context before interpreting suspicious activity.

4. Check tool readiness

   Confirm required parsers and analysis tools are available. If tooling is missing, document the limitation and choose a scoped fallback.

5. Create hypotheses

   Define testable hypotheses such as suspicious execution, persistence, credential access, lateral movement, defense evasion, or data staging.

6. Select hunting scenarios

   Use the scenario catalog to choose relevant investigation paths instead of reviewing every artifact without a question.

7. Apply parser policy

   Scope parser-heavy processing to the hypothesis and evidence need. Record parser assumptions, query scope, and normalized outputs.

8. Cross-check artifacts

   Validate findings across independent evidence sources where possible. Avoid relying on a single artifact when stronger corroboration is available.

9. Pivot deliberately

   Pivot from users, timestamps, paths, process names, hashes, registry keys, services, scheduled tasks, network destinations, and related host activity.

10. Assess confidence

   Separate high-confidence findings from partial indicators, weak signals, blocked hypotheses, and unresolved leads.

11. Document follow-up

   Record missing artifacts, collection gaps, required enrichment, recommended next actions, and closure blockers.

12. Make a closure decision

   Close only when the SOP closure criteria are satisfied. Otherwise, document why the case remains limited, blocked, or unresolved.

## Output Expectations

Analysis output should clearly separate:

- Evidence observed directly
- Analytical interpretation
- Confidence level
- Supporting artifacts
- Timeline relevance
- Parser scope and assumptions
- Gaps or limitations
- Recommended follow-up
- Closure decision or closure blockers

Avoid presenting a conclusion as confirmed unless the evidence supports it.

## Output Schemas

The `schemas/` directory contains reusable JSON contracts for structured output:

- `cedarpelta_findings.schema.json`
- `follow_up_action.schema.json`
- `normalized_event.schema.json`
- `timeline_event.schema.json`
- `parser_recommendation_matrix.json`

Use these schemas when validating local Cedarpelta outputs or when a case repository does not provide its own schema.

## Scope

This skill is intended for:

- Windows endpoint DFIR
- Cedarpelta Build / Live Response Collection evidence
- Triage and deep-dive endpoint analysis
- Analyst-guided investigation workflows
- Report-ready finding development

This skill is not intended to:

- Replace analyst judgment
- Automatically classify a host as compromised
- Modify original evidence
- Perform malware detonation
- Perform live response actions on production systems

## Evidence Handling Notes

- Work from copies of evidence when possible
- Preserve original timestamps and paths
- Track parser assumptions and normalization steps
- Treat missing artifacts as analysis constraints, not proof of absence
- Document uncertainty explicitly
- Review references for sensitive case data before publishing publicly

## Contributing

Contributions are welcome if they improve investigation quality, artifact coverage, analyst usability, or output consistency.

Useful contribution types include:

- Additional Cedarpelta artifact mappings
- Windows artifact interpretation notes
- New pivot examples
- Investigation checklists
- Confidence assessment guidance
- Report wording templates
- Parser-specific caveats
- Schema improvements

Before submitting a change, make sure the guidance remains evidence-driven and does not encourage unsupported conclusions.

See `CONTRIBUTING.md` for contribution expectations.

## Publishing Checklist

Before uploading to GitHub:

- Add a license file
- Review references for sensitive case names, customer names, hostnames, usernames, IP addresses, hashes, or internal paths
- Confirm sample content is synthetic or approved for public release
- Keep the skill self-contained under `dfir-cedarpelta-analysis/`
- Verify `SKILL.md` metadata is accurate
- Confirm every referenced file exists
- Add repository topics from the list below
- Include a short release note describing the initial public version

## Suggested GitHub Description

```text
Codex skill for DFIR analysis of Cedarpelta Windows live response collections, including artifact coverage review, hypothesis testing, cross-checking, pivot workflows, parser policy, and evidence-backed finding development.
```

## Suggested Topics

```text
dfir, incident-response, digital-forensics, windows-forensics, live-response, cedarpelta, codex-skill, threat-hunting, blue-team
```

## License

Choose a license before publishing. For community reuse, consider one of:

- MIT License for broad reuse
- Apache License 2.0 for broad reuse with explicit patent terms
- Creative Commons Attribution 4.0 for documentation-only distribution

