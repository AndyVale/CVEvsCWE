# CVEvsCWE

## CVE JSON record

This repository analyzes CVE records and maps them to CWE categories.

Source schema:
- https://cveproject.github.io/cve-schema/schema/docs/

### CVE record structure

1. `dataType`
   - type of information in the JSON instance
   - expected value: `CVE_RECORD`

2. `dataVersion`
   - CVE schema version
   - regex: `^5\.(0|[1-9][0-9]*)(\.(0|[1-9][0-9]*))?$`

3. `cveMetadata`
   - core CVE metadata
   - key fields:
     - `cveId`
     - `assignerOrgId` (correct spelling; avoid `assignerOrdId`)
     - `state` (`PUBLISHED` or `REJECTED`)

4. `containers` (main analysis payload)
   - required: `cna`
     - `providerMetadata` (CNA provider info)
     - `descriptions` (natural-language descriptions, typically English)
     - `affected` (affected products)
     - `problemTypes` (optional)
       - each item may include `descriptions` referencing CWEs, OWASP categories, or free text
   - optional: `adp`
     - `providerMetadata` (ADP provider info)
     - `problemTypes` (optional)
       - each item may include `descriptions` referencing CWEs, OWASP categories, or free text

### Analysis note

For CVE‑to‑CWE relationship extraction, this project considers `containers.*.problemTypes` in both `cna` and `adp` sections when present.

