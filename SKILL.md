---
name: veracode-platform-results
description: Analyze SAST, DAST, and SCA vulnerabilities from Veracode platform. Filter by severity, status, CWE, exploitability, or policy violations. For remediation guidance, use veracode-flaw-fixing.
---

# Veracode Platform Results

## Setup

The pre-built binary is included in `bin/`. Currently Windows only (`bin/veracode-api.exe`); Mac and Linux versions will be added in a future release.

Use the binary directly or add `bin/` to your `PATH`:

```powershell
# Windows — full path
.\bin\veracode-api.exe <domain> [flags]
```

Always provide `--workspace-root` so the binary can locate `.veracode-workspace.json`:

```powershell
.\bin\veracode-api.exe <domain> --workspace-root C:\path\to\workspace [flags]
```

## Commands

**static** — SAST findings (mitigations included by default)
```bash
veracode-api static --app "AppName" [--severity 5] [--status "NEW,OPEN"] [--sandbox "Name"] [--exclude-mitigations]
```

**static flaw detail** — call-stack data paths for a specific SAST finding
```bash
veracode-api static --app "AppName" --flaw-id 12345
```

**dynamic** — DAST findings (mitigations included by default)
```bash
veracode-api dynamic --app "AppName" [--severity 5] [--sandbox "Name"] [--exclude-mitigations]
```

**dynamic flaw detail** — HTTP request/response details for a specific DAST finding
```bash
veracode-api dynamic --app "AppName" --flaw-id 12345
```

**sca** — SCA component findings
```bash
veracode-api sca --app "AppName" [--severity 5] [--severity-gte 4] [--cvss-gte 7.0] [--status "OPEN"] [--only-exploitable] [--only-new]
```

If `--app` is omitted, the binary reads the profile name from `.veracode-workspace.json` in the workspace root.

Include the flaw IDs from output when requesting remediation guidance.

## Filters

**Static / Dynamic**: `--severity N`, `--status`, `--cwe-ids`, `--violates-policy`, `--exclude-mitigations`, `--page`, `--size`, `--sandbox`, `--flaw-id`

**SCA**: `--severity N`, `--severity-gte N`, `--cvss-gte X.X`, `--status`, `--cwe-ids`, `--violates-policy`, `--only-exploitable`, `--only-new`, `--page`, `--size`

## Mitigation vs Remediation

**Remediation**: Code changed → vulnerability eliminated → flaw disappears from Veracode. Use `veracode-flaw-fixing` skill.

**Mitigation**: Flaw remains in code → marked as acceptable risk → requires security approval. Statuses: `Proposed`, `Approved`, `Rejected`.

## Reference

[reference/severity.md](reference/severity.md), [reference/status.md](reference/status.md), [reference/cwe-common.md](reference/cwe-common.md)