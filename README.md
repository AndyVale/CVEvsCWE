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

### Goals

**Only cvelistV5:**
*   **Coverage:** How many CVEs have a CWE associated?
*   **Cardinality:** How many CWEs are associated with a CVE on average? (and standard deviation).
*   **Popularity:** Are there CWEs that occur much more frequently in assignments?
*   **Co-occurrence:** Are there CWEs that appear more frequently together in the same CVE record?
*   **Temporal Trends:** Is there a trend over time regarding the "density" of assignments (e.g., are modern CVEs being assigned more specific CWEs than older ones)?
*   **Provider Consistency:** How often do `cna` and `adp` containers provide different CWEs for the same CVE?
*   **Completeness:** What percentage of CVE records use "placeholder" CWEs (like `CWE-noinfo` or `CWE-Other`) vs. actual weakness types?

**cwec required (CWE Hierarchy/Metadata):**
*   **Abstraction Distribution:** Are some levels of abstraction (Pillar, Class, Base, Variant) more present in assignments than others?
*   **Hierarchical Specificity:** What is the average "depth" of the assigned CWEs within the CWE tree? (i.e., do assignments favor generic top-level roots or specific leaf nodes?)
*   **Category Affinity:** Which high-level CWE "Pillars" (e.g., CWE-699 Software Development, CWE-1000 Research View) dominate the CVE mapping landscape?
*   **View Compliance:** How many assignments fall within the "NVD Slice" (CWE-1003) versus the full CWE dictionary?


### Future additions

* Add analysis on the KEV
* CWEC can be used to map a CWE id to its description/name/abstraction