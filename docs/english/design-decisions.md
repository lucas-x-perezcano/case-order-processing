# Design Decisions — Simplifying After Understanding the Work

[Case study](../../README.md) · [English](design-decisions.md) · [Español (México)](../spanish/design-decisions.md)

## Where the reasoning comes from

Lucas's account establishes the opportunity identification, employee collaboration, process mapping, use of Claude Code, and decision to move away from an initially AI-oriented approach. The private evidence audit establishes what the inspected implementation does and what the retained validation demonstrates.

The distinctions below matter: personal reasoning is attributed to Lucas; operational consequences inferred from the design are identified as analysis. Neither is presented as a measured business benefit.

## Clarify the procedure with the person doing it

**Lucas's confirmed account.** After the invoice automation, I continued interviewing employees about repetitive tasks. I worked directly with the employee responsible for preparing orders, reviewed the existing Excel procedures, mapped the broader process and the redesigned preparation workflow, and translated those procedures into requirements. We iterated with the employee's feedback.

The employee supplied knowledge of the existing operation. My contribution was to connect that knowledge to a repeatable preparation workflow and develop the application using Claude Code. Collaboration does not establish formal approval of every business rule; the audit still identifies rule assumptions requiring process-owner confirmation.

The internal diagrams remain private. The public illustrations explain the broader boundary without reproducing the original maps.

## Move from an AI-oriented idea to deterministic processing

**Lucas's confirmed reasoning.** I initially explored an approach similar to the AI-assisted invoice workflow. Through iteration, I recognized that these inputs were structured and the recurring transformations largely followed explicit rules. I simplified the solution into conventional code.

**Verified implementation.** The preparation workflow uses named input fields, catalog matching, calculations, grouping, and fixed output structures. No recurring language-model inference is part of processing an order. AI assistance during application development is a separate activity.

**Engineering interpretation.** Deterministic transformations provide a reproducible way to express the known rules and compare results against references. That suitability does not imply that all inputs, catalogs, or business assumptions are correct. Removing recurring inference also does not make maintenance, infrastructure, or operating work free.

## Keep Excel as a working review interface

**Verified implementation.** The application creates a review workbook and exports from its saved state. People provide delivery and confirmation information there. This keeps the intervention point close to the spreadsheet-based procedure reviewed with the employee.

**Engineering interpretation.** A familiar review surface can support the handoff between generated data and operational knowledge. Actual usability, adoption, or training benefit has not been measured. Editable sheets also create a responsibility to coordinate changes: export refreshes only selected fields and does not rebuild all prepared tables.

## Make exceptions visible while continuing preparation

**Documented implementation policy.** An incompatible report structure stops preparation. Other defined issues generally produce exception records while the batch continues. This includes missing information and selected catalog or consistency issues.

**Engineering interpretation.** Continuing preparation keeps the remaining information available for review, but visible warnings do not guarantee resolution. Error severity does not enforce a business approval gate. The application can prepare output that still needs an operational decision, and export does not rerun all checks.

Some missing dates can receive a flagged estimate from available batch information. An estimate is not a confirmed delivery appointment. In the usual initial preparation step, no appointments have yet been supplied by the person.

## Treat master data as part of the process

**Verified evidence.** Re-enrichment of the retained sample exposed incomplete required catalog metadata. A historical manual quantity adjustment also differed from the automated rule. Existing selected regression comparisons exclude those gaps.

**Engineering interpretation.** Reliable transformations need usable reference information and a way to preserve legitimate exceptions. A successful lookup can still return an incomplete record, and a reproducible formula can still disagree with a justified manual adjustment. The public case does not infer that the override was incorrect or that missing fields were automatically resolved.

The business-specific mappings and conversion rules remain private. [Validation](validation.md) explains the scope of the observed discrepancies and comparison exclusions.

## Keep file preparation distinct from organizational adoption

**Verified boundary.** The application generates the intended import representation, and a separate loading component exists. The audit does not establish successful SAP imports, routine use, or production readiness.

**Lucas's confirmed status.** Organizational deployment and live ERP adoption remain pending. The case uses neutral status language and does not disclose internal organizational circumstances.

**Engineering interpretation.** Implementing transformations, validating known inputs, and integrating a tool into day-to-day operations are separate accomplishments. Approval responsibility, reconciliation, master-data ownership, and accepted ERP outcomes remain important evidence boundaries.

## What this design demonstrates

The project demonstrates process analysis, collaboration with the person doing the work, translation of procedures into explicit rules, and a willingness to simplify the technology. It does not establish measured efficiency gains. Any stronger reliability or adoption claim must be supported by evidence beyond local component tests and the retained reference sample.

[Architecture](architecture.md) · [Validation](validation.md) · [Back to case study](../../README.md)
