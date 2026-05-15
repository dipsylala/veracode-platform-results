# CWE Filtering Reference

Filter Veracode findings by CWE ID using `--cweIds` parameter.

## Common CWE Groups

**Injection:** `79,89,78,94,91`  
**Auth/Access:** `287,285,862,863,306`  
**Crypto:** `327,328,329,330,798`  
**Input Validation:** `20,129,190,416,787`  
**Info Exposure:** `200,209,215,532`  
**Session:** `352,384,601,311`

## Usage

```bash

# Single CWE
--cweIds 89

# Multiple CWEs (comma-separated)
--cweIds "79,89,78"

# Combine with severity
--cweIds "79,89" --severity "High,Very High"
```
