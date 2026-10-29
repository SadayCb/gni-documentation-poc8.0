# LOAN_TRANCHE_API — Error Handling

[← Overview](loan-tranche-api.md)

The implementation uses `R(...)` validation calls and selected application errors. Error messages here summarize code-observed conditions; they are not a stable documented error code registry.

| Area | Code-observed problem |
|---|---|
| `get_ovd_trs_oper_type` | Negative balance or overdue amount is rejected |
| Overdue queue helper | Only `MAIN` or `INT` plan type is accepted |
| `do_create_tranche` | Contract/type/amount missing, loan not found, tranche flag `N`, or invalid CREATE/DELETE type |
| `create_tranche` / `do_merge_tranche` | Base schedule or tranche configuration absent; incompatible/multiple rows |
| `do_payment_plan_conversion` | Missing or multiple pending product conversion rows; missing/ambiguous schedule |
| `calculate_date` | Period code missing or unsupported configuration option |
| `create_tranche_history` | Tranche identifier absent or transaction type not I/U/D |
| Interest and penalty calculations | Incorrect loan flags, unsupported loan state, negative amount or product maximum exceeded |
| `table_log_operation` | Invalid operation type; missing transaction id |
| `do_tranche_operation` | Unsupported ALLOCATE/PAYMENT mode or inconsistent amount |
| `get_plan_error` | Numbered validation details, returned as string |
| `do_plan_control` | Raises diagnostic string produced by `get_plan_error` |

## Plan checker behavior

`get_plan_error` compares payment-plan and tranche state with expected schedule and interest tolerance, using loan metadata, interest rate/stop intervals and configured `MAX_INT_DIFF_CORR_AMOUNT`. It assembles multiple diagnostic messages. `do_plan_control` converts a non-empty message into an application exception.

## Review cautions

- An error handler that includes `SQLERRM` may change the exact customer-facing message. Avoid using message text as an API contract.
- The source alone does not prove automatic rollback of journal entries and transaction logs. Test rollback behavior with the actual caller and deployed packages.
- The supplied `do_create_tranche` body appears to include a duplicated local variable declaration; validate against compiling deployed source before publication.