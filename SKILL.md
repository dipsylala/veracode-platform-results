---
name: veracode-platform-results
description: Analyze SAST, DAST, SCA vulnerabilities and scan metadata from Veracode platform. Filter by severity, status, CWE, exploitability, or policy violations. For remediation guidance, use veracode-flaw-fixing.
---

# Veracode Platform Results

## Path Resolution

The `bin/` directory is co-located with this SKILL.md file. When constructing binary paths, resolve `bin/` relative to the directory containing this SKILL.md — not relative to the workspace root or the current working directory.

Example: if this file was read from `C:\Users\me\skills\veracode-platform-results\SKILL.md`, the Windows binary is at `C:\Users\me\skills\veracode-platform-results\bin\veracode-api-windows-amd64.exe`.

Always use the absolute path when invoking the binary.

## Setup

Pre-built binaries for all platforms are included in `bin/` (see [Path Resolution](#path-resolution) above for how to locate them):

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

**scaninfo** — scan/build metadata (policy compliance, scan status, analysis units)
```bash
veracode-api scaninfo --app "AppName" [--build-id 12345678]
```

If `--app` is omitted, the binary reads the profile name from `.veracode-workspace.json` in the workspace root.

Include the flaw IDs from output when requesting remediation guidance.

## Filters

**Static / Dynamic**: `--severity N`, `--status`, `--cwe-ids`, `--violates-policy`, `--exclude-mitigations`, `--page`, `--size`, `--sandbox`, `--flaw-id`

**SCA**: `--severity N`, `--severity-gte N`, `--cvss-gte X.X`, `--status`, `--cwe-ids`, `--violates-policy`, `--only-exploitable`, `--only-new`, `--page`, `--size`

**Scan Info**: `--build-id N` (0 or omit = latest scan)

## Mitigation vs Remediation

**Remediation**: Code changed → vulnerability eliminated → flaw disappears from Veracode. Use `veracode-flaw-fixing` skill.

**Mitigation**: Flaw remains in code → marked as acceptable risk → requires security approval. Statuses: `Proposed`, `Approved`, `Rejected`.

## Reference

[reference/severity.md](reference/severity.md), [reference/status.md](reference/status.md), [reference/cwe-common.md](reference/cwe-common.md)