---
name: veracode-platform-results
description: Analyze SAST, DAST, SCA vulnerabilities and scan metadata from Veracode platform. Filter by severity, CWE, exploitability, new findings, or policy violations. For remediation guidance, use veracode-flaw-fixing.
---

# Veracode Platform Results

## Path Resolution

The `bin/` directory is co-located with this SKILL.md file. When constructing binary paths, resolve `bin/` relative to the directory containing this SKILL.md — not relative to the workspace root or the current working directory.

Example: if this file was read from `C:\Users\me\skills\veracode-platform-results\SKILL.md`, the Windows binary is at `C:\Users\me\skills\veracode-platform-results\bin\veracode-api-windows-amd64.exe`.

Always use the absolute path when invoking the binary.

## Setup

| Platform | Binary |
|----------|--------|
| Windows (x64) | `bin/veracode-api-windows-amd64.exe` |
| Windows (ARM64) | `bin/veracode-api-windows-arm64.exe` |
| macOS (Intel) | `bin/veracode-api-darwin-amd64` |
| macOS (Apple Silicon) | `bin/veracode-api-darwin-arm64` |
| Linux (x64) | `bin/veracode-api-linux-amd64` |
| Linux (ARM64) | `bin/veracode-api-linux-arm64` |

Use the binary directly or add `bin/` to your `PATH`. On macOS and Linux, make the binary executable first: `chmod +x bin/veracode-api-<os>-<arch>`

```powershell
# Windows (x64)
.\bin\veracode-api-windows-amd64.exe <domain> [flags]
```

```bash
# macOS (Apple Silicon)
./bin/veracode-api-darwin-arm64 <domain> [flags]

# Linux (x64)
./bin/veracode-api-linux-amd64 <domain> [flags]
```

Always provide `--workspace-root` so the binary can locate `.veracode-workspace.json`:

```powershell
# Windows
.\bin\veracode-api-windows-amd64.exe <domain> --workspace-root C:\path\to\workspace [flags]
```

```bash
# macOS / Linux
./bin/veracode-api-darwin-arm64 <domain> --workspace-root /path/to/workspace [flags]
```

## Output format

All commands accept `--format json` (default) or `--format markdown`. Always use `--format markdown` — it is more compact and easier to read in LLM responses.

## Commands

**apps** — list all application profiles
```bash
veracode-api apps [--page 0] [--size 100] --format markdown
```

**appinfo** — application profile details
```bash
veracode-api appinfo --app "AppName" --format markdown
```

**sandboxes** — list sandboxes for an application
```bash
veracode-api sandboxes --app "AppName" --format markdown
```

**static** — SAST findings
```bash
veracode-api static --app "AppName" [--severity 5] [--only-new] [--sandbox "Name"] [--include-mitigations] --format markdown
```

**static flaw detail** — call-stack data paths for a specific SAST finding
```bash
veracode-api static --app "AppName" --flaw-id 12345 --format markdown
```

**dynamic** — DAST findings
```bash
veracode-api dynamic --app "AppName" [--severity 5] [--include-mitigations] --format markdown
```

**dynamic flaw detail** — HTTP request/response details for a specific DAST finding
```bash
veracode-api dynamic --app "AppName" --flaw-id 12345 --format markdown
```

**sca** — SCA component findings
```bash
veracode-api sca --app "AppName" [--severity 5] [--severity-gte 4] [--cvss 7.5] [--cvss-gte 7.0] [--only-exploitable] [--only-new] --format markdown
```

**scaninfo** — scan/build metadata (policy compliance, scan status, analysis units)
```bash
veracode-api scaninfo --app "AppName" [--build-id 12345678] --format markdown
```

If `--app` is omitted, the binary reads the profile name from `.veracode-workspace.json` in the workspace root.

Include the flaw IDs from output when requesting remediation guidance.

## Filters

**Static / Dynamic**: `--severity N`, `--severity-gte N`, `--cvss X.X`, `--cvss-gte X.X`, `--cwe-ids`, `--violates-policy`, `--only-new`, `--include-mitigations`, `--page`, `--size`, `--sandbox` (static only), `--flaw-id`

**SCA**: `--severity N`, `--severity-gte N`, `--cvss X.X`, `--cvss-gte X.X`, `--cwe-ids`, `--violates-policy`, `--only-new`, `--only-exploitable`, `--page`, `--size`

**Scan Info**: `--build-id N` (0 or omit = latest scan)

## Mitigation vs Remediation

**Remediation**: Code changed → vulnerability eliminated → flaw disappears from Veracode.

**Mitigation**: Flaw remains in code → marked as acceptable risk → requires security approval. Use `--include-mitigations` to fetch annotation details. Statuses: `Proposed`, `Approved`, `Rejected`.
