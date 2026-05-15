# Finding Status Values

## Status Values

**Active:**
- `NEW` - Recently discovered
- `OPEN` - Confirmed, awaiting fix
- `MITIGATED` - Compensating control applied

**Closed:**
- `FIXED` - Code changed
- `CANNOT_REPRODUCE` - Cannot replicate
- `FALSE_POSITIVE` - Incorrect detection
- `ACCEPTED` - Risk accepted

## Filtering

```bash

# Active findings
--status "NEW,OPEN"

# Resolved findings
--status "FIXED,MITIGATED"

# Include mitigation details
--includeDetails true
```
