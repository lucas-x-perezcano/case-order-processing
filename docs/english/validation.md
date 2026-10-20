# Validation — Evidence, Exclusions, and Current Status

[Case study](../../README.md) · [English](validation.md) · [Español (México)](../spanish/validation.md)

This is a sanitized account of the private evidence audit dated **October 18, 2026**. Results below are historical audit results; preparing this case study did not rerun the application or establish a new test result. Lucas subsequently confirmed the project origin, collaborative development, technology decision, and pending organizational deployment.

## Evidence categories

| Category | How it is used here |
| --- | --- |
| Verified fact | Source inspection, executed tests, or calculations recorded in the audit. |
| Developer statement | Lucas's account of the project and historical statements in development notes. These do not substitute for accepted ERP results. |
| Reasonable inference | An explicitly identified interpretation of the design or its operational implications. |
| Unknown | A result or condition not established by the available evidence. |
| Proposed improvement | A possible future measure, not an implemented capability or current result. |

## One retained reference dataset

The retained reference sample supported checks of order/line keys and parent relationships. Operational sample sizes and derived measurements are excluded from this public account.

The retained Excel workbooks were **already enriched**. Their corresponding report sheets contain equal values, so they represent **one distinct dataset**, not multiple independent batches. Developer notes describe the records as operational data; their historical use and ERP acceptance were not independently confirmed. They are treated as confidential regardless of that provenance limitation.

There was no separate untouched raw-report fixture in the retained sample. The enrichment regression constructs a raw-like input by selecting original report columns from an enriched workbook and restoring historical delivery/confirmation information from that same reference. This supports selected reconstruction checks, not a full independent test from original download through ERP acceptance.

## Executed tests and the environment limit

The audited version defined **154 test methods**. A deliberately selected subset of **106 tests ran and passed**, with **zero failures or errors**. The other **48 methods were not executed because PyYAML was unavailable** in the audit environment. They were excluded from the runner, not reported as skipped or passing tests.

| Area | Executed and passed | Not executed |
| --- | ---: | ---: |
| Input reading and schema integration | 0 | 13 |
| Enrichment | 45 | 15 |
| Transformation | 25 | 0 |
| Retained-sample transformation regression | 0 | 13 |
| Validation checks | 27 | 7 |
| Loading-map and reconciliation-template structure | 9 | 0 |
| **Total** | **106** | **48** |

The environment used Python 3.12.14, pandas 2.2.3, openpyxl 3.1.5, and the standard-library unittest runner on macOS. pytest was also unavailable. No dependencies were installed during the audit.

Executed tests covered selected transformations and positive/negative cases, including grouping, quantity selection, payment matching, date fallback, unknown catalog matches, selected consistency/type checks, missing metadata warnings, and static loading-template relationships. The template tests did not execute the loading tool or SAP. Input/schema test definitions were inspected, but their 13 methods were not run.

This is a count of executed checks, not code-coverage measurement, an accuracy percentage, or proof that all 154 methods passed. Historical notes mention 135 passing tests; that matches the method count on an earlier local main branch, not a verified full run of the audited extension version.

## Separate reference-comparison diagnostics

Additional audit diagnostics called the actual enrichment, transformation, and validation functions in memory. Because the normal YAML dependency was missing, configuration was read using an installed alternative parser and catalog lookup dictionaries were reconstructed independently from the retained master workbook.

Those diagnostics **did not exercise the application's configuration/master-loading path, current workbook writer, text exporter, desktop interface, or ERP loader**. They were separate from the 106 executed tests and must not be counted as those unexecuted regression methods having passed.

| Diagnostic | Observed result | Scope |
| --- | --- | --- |
| Transform the already enriched input | Prepared header and line tables with missing-date fallback warnings. | Existing enriched values and historical delivery information were present. |
| Compare with reference tables | Reproduced normalized reference header and line tables. | Row-aligned, in-memory comparison, not file-byte equality. |
| Run selected post-transformation checks on that result | The result remained subject to review. | Transformation warnings remained unresolved. |

Comparison normalized blanks, formatted dates consistently, and normalized numbers with two-decimal rounding. It did not establish exact floating-point equality, byte-for-byte exported files, independent correctness of the reference, or acceptance by SAP. The strongest result is reproducibility of the normalized reference representation from the already enriched sample.

## Re-enrichment exposed important gaps

| Finding | What the audit observed | What it does not establish |
| --- | --- | --- |
| Incomplete catalog metadata | Re-enrichment matched product codes but left required unit, fiscal-product, and packaging-capacity metadata missing. | A recognized product is not necessarily complete enough for downstream use. The gaps were not automatically corrected. |
| Historical manual quantity adjustment | Re-enrichment did not reproduce a historical manual quantity adjustment. | The adjustment's business rationale is unknown; the audit did not decide whether it or the automated value should prevail. |
| Selected comparison exclusions | Existing quantity regression omits the override. Metadata comparisons use populated results and omit missing results for the fields concerned. | Passing those selected comparisons is not complete equivalence across every line and field. |

With historical appointments restored, re-enrichment followed by transformation/checks produced missing-date fallback and metadata warnings. Without appointments, matching the initial desktop preparation behavior, missing-date errors and metadata warnings accompanied the prepared tables. These outcomes expose information requiring review; they do not show automatic resolution or operational approval.

The audit also found an inconsistency between historical workbook line quantities and the expected text reference. That discrepancy supports scrutiny of the preparation artifacts, but does not prove that incorrect quantities entered the ERP. No operational error rate is derived from it.

## Review and export limits

The application surfaces selected exceptions but does **not enforce a mandatory approval gate**. Error-level records can accompany output. A reminder to save Excel does not establish that business decisions were approved.

Export reads the saved sheets and refreshes selected header fields. It does **not rerun all validation**, rebuild every prepared table, or synchronize all related edits. The exception list can become stale after changes. Quantity, product, and delivery edits may need coordinated updates across the workbook.

No direct current writer test, byte-level final text regression, desktop interaction test, or Windows loader execution test was found in the audit. Those stages were inspected in code rather than exercised in the full application path. The reference comparison cannot be extended into a raw-input-to-ERP correctness claim.

## Maturity and unverified outcomes

The evidence supports a developed local implementation with selected component testing and reference-based diagnostics. Lucas confirms that organizational deployment and live ERP adoption remain pending. The case does not claim production readiness.

Development notes describe a test load in a development environment, but retained accepted/rejected results and post-load reconciliation were unavailable. The replacement loader was explicitly documented as not yet run on Windows. These statements do not establish successful ERP imports or routine employee use.

There is no measured manual baseline, labor saving, error reduction, ROI, production throughput, or adoption metric. Remote branch freshness, CI execution, and release results were not verified by the audit. Results apply to the inspected local version and retained sample, not all future inputs or catalogs.

## What a stronger claim would require

Accepted-import claims would require retained ERP outcomes tied to the specific prepared files. Adoption claims would require evidence of actual organizational use. Efficiency claims would require a defined manual baseline and comparable measurements. These are evidence requirements for future claims, not completed validation or a new implementation plan.

The current case can accurately describe process redesign, collaborative requirements work, deterministic preparation, selected successful tests, and the limits found during validation.

[Architecture](architecture.md) · [Design decisions](design-decisions.md) · [Back to case study](../../README.md)
