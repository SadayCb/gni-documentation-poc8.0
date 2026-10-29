# Revision report — second audit fixes

Target branch: `preview/documentation-revision-20261027`. Original `main` and backup preserved.

## Applied
- Corrected penalty negative-balance error to `get_amount_for_pen_calc` overload 1.
- Improved verified business rules for transaction caller mapping and clarified selected source-supported parameters.
- Distinguished `do_loan_control` overloads by parameter count.
- Replaced conceptual workflow diagrams with traced limit-evaluation and plan-interest-posting sequences.
- Reorganized the Data Model and Dependencies as non-exhaustive method-linked registers.
- Replaced obsolete documentation prompt with the current shorter standard.

## Limitations
- Public API still contains generic business-rule text for many routines requiring independent semantic review. No claim of full method-by-method validation.
- Shared `R(...)` error helper semantics, external package internals and database DDL remain unavailable.
- Static link checks do not certify semantic correctness.
