# LOAN_CATEGORY_API — Error Handling

[← Overview](loan-category-api.md) · [API Reference](loan-category-api-reference.md) · [Workflows](loan-category-workflows.md)

A curated list of source-visible business validation and processing conditions, **not** a catalogue of all Oracle exceptions or stable error codes.

## Error categories

### Business validation and input constraints

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [change_ov_status](loan-category-api-reference.md#change-ov-status) | Customer number is null | Overdraft closure request is rejected | Supply a customer number |
| [apply_court_decision](loan-category-api-reference.md#apply-court-decision) | Declared component amounts do not sum to total debt | Processing diagnostic is written to `SS_KRD22060_0_LOG`; this branch does not directly raise the commented-out application error | Reconcile principal, interest, penalty and legal expense with total debt |
| [run_loan_category](loan-category-api-reference.md#run-loan-category) | Configured procedure name does not resolve to a callable routine | Dynamic execution can fail | Correct the active configuration entry |

### Configuration and data lookup failures

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [apply_court_decision](loan-category-api-reference.md#apply-court-decision) | Counter account mapping or loan/account lookup cannot be resolved | Relevant processing may be logged as an error or raise a lookup exception | Verify loan and `SP_ACCOUNT_PR` account mapping |
| [set_charge_off_status](loan-category-api-reference.md#set-charge-off-status) | Configured parameter SQL or required category data is invalid | Charge-off batch processing may fail | Review active `LOAN_CHARGE_OFF_PARAMETERS` records |
| [do_close_operation](loan-category-api-reference.md#do-close-operation) | Closure parameter query or expected account mapping cannot be resolved | Loan may not complete the configured closing flow | Review `LOAN_EOD_AUTO_CLOSE_PARAMETERS` and balance/account mappings |

## Diagnostic and transaction notes

- Some errors are written to tables rather than thrown directly. In particular, court-decision record status updates must not be interpreted as unconditional termination.
- Generic `WHEN OTHERS` handling and calls to other packages can change the observed error message and transaction outcome.
- The code alone does not establish a universal rollback or commit contract for all entry points.
- Validation and recovery behavior should be confirmed against the deployed environment when this initial reference is finalized.

[↑ Overview](loan-category-api.md)
