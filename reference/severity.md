# Veracode Severity Levels

| Level | CVSS Range |
|-------|------------|
| **5 - Very High** | 9.0 - 10.0 |
| **4 - High** | 7.0 - 8.9 |
| **3 - Medium** | 4.0 - 6.9 |
| **2 - Low** | 0.1 - 3.9 |
| **1 - Very Low** | 0.0 |
| **0 - Informational** | N/A |

## Filtering

```bash

# SAST/DAST: By severity number (0-5)
--severity 5
--severity 4

# SCA: By minimum severity level
--severityGte 4

# SCA: By CVSS score
--cvssGte 7.0
```
