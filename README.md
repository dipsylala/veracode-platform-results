# veracode-platform-results

A reusable agent skill + scripts pattern for querying and interpreting [Veracode platform](https://docs.veracode.com/) SAST, DAST, and SCA results via the Findings API.

Works with GitHub Copilot, Cursor, Claude Code, and any agent that supports the `SKILL.md` convention.

## What it does

- Fetches **SAST** findings from platform static analysis scans
- Fetches **DAST** findings from dynamic analysis scans
- Fetches **SCA** findings (open-source component vulnerabilities)
- Retrieves detailed flaw data including data-flow paths for SAST and HTTP traces for DAST
- Supports filtering by severity, status, CWE, policy violations, sandbox, and more

## Usage

### GitHub Copilot, Cursor, and other AI IDEs

Copy or clone this folder into your project (or home directory for personal use):

| Location | Scope |
| ---------- | ------- |
| `.github/skills/veracode-platform-results/` | Project — GitHub Copilot |
| `.agents/skills/veracode-platform-results/` | Project — other agents |
| `.claude/skills/veracode-platform-results/` | Project — Claude/Cursor |

The agent will automatically load the skill when relevant, or you can invoke it directly:

> "Show me all High and Very High SAST findings for MyApp"

> "Give me SCA findings for MyApp that violate policy"

> "Get details on flaw 12345 in MyApp"

## Requirements

- Python 3.8+
- Veracode API credentials (`~/.veracode/veracode.yml` or environment variables `VERACODE_API_KEY_ID` / `VERACODE_API_KEY_SECRET`)

Dependencies are installed automatically on first use.

## Repository contents

| Path | Description |
|------|-------------|
| `SKILL.md` | Skill definition and agent instructions |
| `scripts/get-static.py` | Fetch SAST findings with filters |
| `scripts/get-dynamic.py` | Fetch DAST findings with filters |
| `scripts/get-flaw-details.py` | Detailed flaw data (data-flow / HTTP trace) |
| `scripts/get-sca.py` | Fetch SCA component vulnerability findings |
| `scripts/get-sca-summary.py` | SCA risk overview per application |
| `scripts/requirements.txt` | Python dependencies |
| `veracode_lib/` | Shared Veracode API client library |
| `reference/severity.md` | Severity levels and CVSS ranges |
| `reference/status.md` | Finding status values |
| `reference/cwe-common.md` | Common CWE IDs for filtering |

## Remediation priority

1. **Very High** — fix immediately; likely exploitable
2. **High** — fix in current sprint; exploitability is neutral or likely
3. **Medium / Low** — schedule for remediation; lower exploitability risk