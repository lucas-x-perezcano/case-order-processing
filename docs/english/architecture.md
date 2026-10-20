# Architecture and Workflow Boundary

[Case study](../../README.md) · [English](architecture.md) · [Español (México)](../spanish/architecture.md)

## The business boundary

The application prepares customer purchase orders for an ERP import process. Its scope begins with a person selecting a downloaded structured report and ends with files prepared for a separate loading step. Order acquisition, operational approval, ERP acceptance, fulfillment, invoicing, and collection are outside this preparation boundary.

This description reflects inspected implementation. It does not mean the complete desktop and ERP sequence was executed during the audit. The evidence and its limits are detailed in [Validation](validation.md).

![Conceptual solution architecture with a separate downstream ERP loading boundary.](../../assets/solution-architecture.en.svg)

## Inputs and processing stages

| Stage | Business role | Implemented behavior and boundary |
| --- | --- | --- |
| Structured order report | Supply the customer's purchase-order lines. | A person obtains the Excel file. Named-column checks identify missing or unexpected columns; this is not complete business validation. |
| Catalog and configuration | Supply master-data references and rule settings. | Private lookup tables provide product, customer-related, payment, and delivery attributes. Configuration supplies schemas and selected mappings. |
| Deterministic preparation | Apply repeatable procedures. | Normalize selected identifiers, match catalog records, enrich lines, apply transformations, and group order references. |
| Header and line construction | Represent the orders in an importable relationship. | Create order headers and detail lines linked by internal keys. These keys are not ERP-assigned order numbers. Grouping does not remove duplicate source lines or establish uniqueness across runs. |
| Selected validation | Make defined inconsistencies visible. | Check selected codes, types, quantities, header consistency, and required metadata. Flagged records can remain in the output. |

Catalog matching depends on the retained master data. Matching a product does not prove that its unit, fiscal, or packaging information is complete. Selection among duplicate catalog entries also does not independently establish that the chosen record is commercially correct.

## The review workbook is an operational interface

The implemented writer defines an enriched report, order headers, order lines, an order index, and an exceptions sheet. These views connect the source information to the intended import representation and provide places for review and edits. The current writer was inspected but not exercised in the audit; historical workbook fixtures do not independently prove its current output.

Delivery dates and confirmations remain human inputs. The desktop interface asks the person to save the workbook before export, because export reads the saved file. It does not obtain delivery information from another system, verify all review decisions, or enforce approval before proceeding.

The exception list is generated during preparation. It is a snapshot, not a continuously refreshed assessment of subsequent Excel edits.

## Export is a separate step

The exporter reads the saved header and line sheets, refreshes selected header text and delivery-related fields from the enriched report, formats values, and writes a tab-delimited header file and line file. Both use the intended DTW header layout and date representation.

Export does not rebuild every field from the enriched report or rerun all validation. A quantity change in the report does not automatically regenerate the line sheet. Editing a delivery date in one location does not synchronize every related field. Coordinated review remains necessary.

The pair is written sequentially rather than as one atomic operation, and repeated output names can replace earlier files. The preparation application lacks a persistent structured run record linking input/catalog versions to final accepted outcomes. These limits matter for recovery and traceability; the audit did not test final file bytes or the export path.

## SAP and DTW remain a separate integration boundary

The desktop application prepares files intended for SAP Business One Data Transfer Workbench. A separate Windows loading component exists in the private implementation. It defines file-structure and relationship checks, an exact-file replay guard, credential separation, and preparation of reconciliation queries.

Those mechanisms do not establish successful imports. The audit did not execute the loader or connect to SAP. An internal description of a development-environment load does not supply accepted/rejected results, and the replacement loader was documented as not yet executed on Windows.

The replay guard concerns identical file contents, not every possible business duplicate. Reconciliation queries require a person to run and interpret them. A successful process exit alone is not proof that every ERP record was accepted. Organizational deployment and live ERP adoption remain pending.

## Technical components in context

Python and pandas handle table preparation, openpyxl handles Excel access, and PyYAML loads configuration. Tkinter provides the desktop interface. PyInstaller packaging and build/test workflows are defined for Windows delivery. PowerShell, XML, and SQL support the separate DTW boundary.

The catalog remains an external operational input. In packaged execution, configuration is bundled with the application; updating an external configuration file is not necessarily equivalent to changing the packaged behavior. Build definitions demonstrate packaging intent, not a verified release or installation.

This case contains a sanitized architectural explanation only. Private implementation files, operating configuration, loading credentials, and business-specific rules are not included.

[Design decisions](design-decisions.md) · [Validation](validation.md) · [Back to case study](../../README.md)
