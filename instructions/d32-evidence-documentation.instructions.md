---
description: 'Agent instructions for collecting and validating NATO D32 compliance evidence for Azure platform implementations'
applyTo: '**/Platform/Compliance-Evidence/NATO-D32-Evidence/**/*.md, **/.d32context/**'
---

# NATO D32 Evidence Collection Agent

## Purpose

Assist in collecting, validating, and documenting compliance evidence for NATO D32 Security Measures (SM) against Azure platform implementations. Focus on structured evidence gathering with traceability to implementation documentation and Infrastructure as Code.

## MCP Servers

### Required Servers

| Server | Purpose | Key Operations |
|--------|---------|----------------|
| **Azure DevOps** | Work item search, project queries | `search_work_items`, `get_work_item`, `search_code` |
| **File System** | Read/write files, folder traversal | `read_file`, `write_file`, `list_directory` |

### Server Configuration

#### Azure DevOps

- **Project:** CaelumPlatform
- **Area Path:** CaelumPlatform\Azure
- **Common searches:** Control IDs (SM-LOG-03), keywords (Sentinel, SIEM, logging)

#### File System

- **Validation:** `.d32context/exportedARMTemplates/`
- **Evidence:** `Platform/Platform-Implementation/`
- **Documents:** `Platform/Compliance-Evidence/NATO-D32-Evidence/`

## Workflow

### 1. Document Structure Validation

Compare target SM document against leading template. Required sections:

```markdown
# SM-xxx-xx — Title

[[_TOC_]]

## Metadata
| **Status** | <span style="background:GREY;padding: 0px 5px;text-align:center;color:white;">**DRAFT**</span> |
| --- | --- |
| **Scope** | NATO UNCLASSIFIED (NATO Unrestricted) |
| **SCF Control** | |
| **SMCF Control** | |
| **Owner** | [REPLACE: Team/Role] |
| **Last review** | [REPLACE: yyyy-mm-dd] |
| **Next review** | [REPLACE: yyyy-mm-dd] |

## Requirement (Directive)
## Control sub-objectives
## Shared responsibility
## Evidence checklist
### Azure
#### Compliance Statement
#### Evidence of Compliance
#### Action List — Missing Evidence
### Identity and Access Management
### Devices
```

### 2. Scope Management

- Focus on **Azure platform only** initially
- Leave IAM and Devices sections as placeholders
- Remove action items belonging to IAM or Devices scope

### 3. Evidence Collection

#### Priority Order

1. **Implementation documentation** — Primary evidence for auditors
2. **ARM templates** — Validation only, not cited as evidence
3. **ALZ Policies** — Reference policy name, scope, effect
4. **Azure DevOps work items** — Link with status

#### Evidence Table Format

```markdown
| NATO D32 control identifier | NATO D32 description | SCF control identifier | SCF description | Evidence |
| --- | --- | --- | --- | --- |
| CSC-xxx-xx-01 | Description | None | None | - Evidence item with specific setting (value)<br>- Second evidence item ([Source](/path/to/doc.md)) |
```

#### Evidence Requirements

- Use bullet points (`-`) per evidence item
- Include specific configuration values (e.g., `retention: 365 days`, `SKU: PerGB2018`)
- Reference implementation docs, not ARM templates
- Link to ALZ Policies when applicable

### 4. Evidence Validation

Cross-reference evidence against:

- ARM templates in `.d32context/exportedARMTemplates/`
- Implementation documentation
- ALZ Policy assignments

Mark as verified only when backed by real data.

### 5. Action List Format

```markdown
| # | Security Measure | Sub Control Objective | Description | Artefact | D32 Rationale | Microsoft Docs | Azure DevOps Work Item |
|---|---|---|---|---|---|---|---|
| 1 | SM-LOG-03 | CSC-LOG-03-04 | Full description with <u>underlined relevant requirement</u> | Sentinel workspace configuration | SIEM must support parsing, correlation, UEBA | [Onboard Sentinel](https://learn.microsoft.com/...) | [#1478](https://dev.azure.com/fm-stare/CaelumPlatform/_workitems/edit/1478) — Title (New) |
```

## Formatting Rules

### General

- No emoticons — use `[VERIFIED]`, `[TODO]`, `To Do`
- Use Markdown tables with proper alignment
- Use `<u>...</u>` for underlined text in descriptions
- Use `<br>` for line breaks within table cells

### Links

- Wiki pages: `/Platform/Platform-Implementation/...`
- ALZ Policies: `https://dev.azure.com/fm-stare/CaelumPlatform/_wiki/wikis/Platform-Wiki/1614/ALZ-Policies`
- Work items: `https://dev.azure.com/fm-stare/CaelumPlatform/_workitems/edit/{id}`
- Microsoft Docs: `https://learn.microsoft.com/en-us/azure/...`

## Key File Locations

| Purpose | Path |
|---------|------|
| SM evidence documents | `Platform/Compliance-Evidence/NATO-D32-Evidence/CD{n}-{name}/SM-*.md` |
| Implementation docs | `Platform/Platform-Implementation/{domain}/` |
| ARM templates | `.d32context/exportedARMTemplates/` |
| D32 control list | `.d32context/d32controllist.csv` |
| Templates | `.d32context/templates/` |

## Control Domain Mapping

| CD# | Domain | Implementation Path |
|-----|--------|---------------------|
| CD1 | Audit and Assurance | TBD |
| CD10 | Identity and Access Management | `Platform-Implementation/IAM/` |
| CD13 | Logging and Monitoring | `Platform-Implementation/Logging-and-Monitoring/` |
| CD16 | Threats and Vulnerabilities | `Platform-Implementation/Security-Posture-Management/` |

## Common Tasks

### Compare Document Structure

1. Read target SM document
2. Read leading template (e.g., SM-LOG-02)
3. Identify missing sections
4. Add missing sections with placeholder content

### Collect Evidence

1. Read implementation documentation for the control domain
2. Extract specific settings and configurations
3. Validate against ARM templates in `.d32context`
4. Populate evidence table with verified items
5. Create action list for missing evidence

### Validate ALZ Coverage

1. Read `ALZ-Policies.md` from Platform-Wiki
2. Match policy names to control requirements
3. Add policy references to evidence table
4. Remove covered items from action list

### Search Related Work Items

```
Project: CaelumPlatform
Area: CaelumPlatform\Azure
Keywords: {control-id}, {technology}, {feature}
```

## Anti-Patterns to Avoid

- Citing ARM templates as evidence (use for validation only)
- Using emoticons in status indicators
- Including IAM/Device items in Azure scope
- Referencing evidence without specific configuration values
- Creating action items without D32 rationale
- Omitting Microsoft Docs guidance links
- Not validating claimed evidence against source data

## Example Evidence Entry

```markdown
| CSC-LOG-03-08 | Retention complies with applicable policies/directives. | None | None | - Log Analytics workspace `log-monitoring-prod-001`: retention 365 days, SKU PerGB2018, location Sweden Central ([Azure-Logging-Monitoring-Implementation.md](/Platform/Platform-Implementation/Logging-and-Monitoring/Azure-Logging-Monitoring-Implementation.md))<br>- Archive Storage Account: StorageV2, Standard_GRS, 7-year immutable retention, TLS 1.2 minimum ([Azure-Logging-Monitoring-Implementation.md](/Platform/Platform-Implementation/Logging-and-Monitoring/Azure-Logging-Monitoring-Implementation.md))<br>- Lifecycle policy: auto-archive after 365 days to Cool tier |
```

## Example Action List Entry

```markdown
| 4 | SM-LOG-03 | CSC-LOG-03-04 | Relevant logs are ingested in a Security Information and Event Manager (SIEM), either located in the same cloud environment or external. The SIEM shall support as minimum the following features: <u>Parsing and formatting of logs</u>; Automated correlation of events; UEBA capabilities; Automatic alert system; Automatic Response capabilities. | Microsoft Sentinel workspace configuration | SIEM must support log parsing and formatting | [Onboard Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/quickstart-onboard) | [#3821](https://dev.azure.com/fm-stare/CaelumPlatform/_workitems/edit/3821) — Deploy LAW in Security Subscription (New) |
```
