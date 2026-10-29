# LOAN_TRANCHE_API — Error Handling

[← Overview](loan-tranche-api.md) · [API Reference](loan-tranche-api-reference.md) · [Workflows](loan-tranche-workflows.md) · [Dependencies](loan-tranche-dependencies.md)

This is a **curated register of important business validations and processing failures** observed in the supplied package body. It is **not** an exhaustive Oracle error list or a promise of stable message text.

The source invokes `R(...)` for many validation failures. Its exact implementation and resulting Oracle error code are not established by the supplied package. Messages containing live amounts, loan numbers or `SQLERRM` are summarized by cause instead of quoted as partial strings.

## Error categories

### Business validation and input constraints

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [get_ovd_trs_oper_type](loan-tranche-api-reference.md#get-ovd-trs-oper-type) | Current or overdue balance is negative. | Overdue/transit classification is rejected. | Reconcile the input balance and already overdue amount. |
| [do_create_tranche](loan-tranche-api-reference.md#do-create-tranche) | CREATE is requested without contract number, tranche type or amount; or the operation is neither CREATE nor DELETE. | Tranche execution cannot continue. | Supply the required values and supported operation mode. |
| [create_tranche_history](loan-tranche-api-reference.md#create-tranche-history) | History mode is not I/U/D or target tranche is missing. | History posting and tranche balance adjustment fail. | Use I, U or D and verify the tranche identifier. |
| [get_pen_info_for_calc](loan-tranche-api-reference.md#get-pen-info-for-calc) | Calculated overdue principal or penalty-day count is negative. | Penalty-calculation inputs are rejected. | Check overdue position and date calculation inputs. |
| [do_int_calculation_by_range](loan-tranche-api-reference.md#do-int-calculation-by-range) | A selected loan is written off, duplicate tranche records are encountered, or computed interest is negative. | Affected range-interest processing fails. | Review loan status, tranche selection and interest inputs. |
| [do_tranche_operation](loan-tranche-api-reference.md#do-tranche-operation) | Unsupported processing mode or allocation exceeds an available account amount. | Requested tranche allocation/payment is rejected. | Use ALLOCATE or PAYMENT where required and reconcile account amounts. |
| [calculate_date](loan-tranche-api-reference.md#calculate-date) | Unsupported configured date option is passed. | Requested configured date cannot be calculated. | Use a supported date-calculation mode. |
| [table_log_operation](loan-tranche-api-reference.md#table-log-operation) | Operation type is neither LOGGING nor RESTORING, or transaction ID is absent. | Table snapshot or recovery action is rejected. | Supply a transaction ID and valid logging mode. |

### Configuration and data lookup failures

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [do_create_tranche](loan-tranche-api-reference.md#do-create-tranche) | Contract lookup finds no matching loan or the loan is not tranche-enabled. | Creation cannot be staged or executed. | Verify contract number and tranche-enabled flag. |
| [ovd_amount_correction](loan-tranche-api-reference.md#ovd-amount-correction) | PENDING schedule-conversion configuration is missing or duplicated, or target schedule mapping is missing/ambiguous. | Overdue principal correction or required schedule generation cannot proceed. | Correct product conversion mapping and schedule configuration. |
| [do_payment_plan_conversion](loan-tranche-api-reference.md#do-payment-plan-conversion) | PENDING product conversion record or target schedule cannot be resolved uniquely. | Configured repayment-plan conversion cannot proceed. | Resolve missing/overlapping PKRD_ODETIP_PR and LOAN_SCHEDULE_TYPES records. |
| [do_merge_tranche](loan-tranche-api-reference.md#do-merge-tranche) | Required base repayment-plan record is missing, or the expected main tranche selection is ambiguous. | Eligible tranche cannot be merged correctly. | Verify base plan and primary tranche data. |
| [create_tranche](loan-tranche-api-reference.md#create-tranche) | Base plan/tranche requirements cannot be resolved; MANUAL term is missing or invalid. | Tranche creation is stopped. | Verify loan plan, tranche parameters and term format such as M12. |
| [do_int_calculation](loan-tranche-api-reference.md#do-int-calculation) | Loan interest/penalty flags, product configuration, non-working-day option or maximum penalty settings are invalid. | Interest/penalty processing for affected loans is blocked. | Correct loan flags and applicable product/EOD configuration. |
| [get_plan_error](loan-tranche-api-reference.md#get-plan-error) | Required loan or base repayment-plan record is missing. | Plan check produces a diagnostic failure. | Confirm loan and base schedule exist before running plan validation. |

### Operation state and processing restrictions

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [ovd_amount_correction](loan-tranche-api-reference.md#ovd-amount-correction) | After overdue principal is reassigned, the remaining overdue amount cannot be allocated completely across expected plan entries. | Correction is rejected before final plan control succeeds. | Reconcile overdue-interest dates and outstanding plan amounts. |
| [do_int_calculation](loan-tranche-api-reference.md#do-int-calculation) | Interest calculation detects unprocessed periods or inconsistent flow-rate configuration. | Calculation for the affected loan cannot safely continue. | Complete prerequisite calculation and inspect dated flow-rate entries. |
| [get_Pym_Startdate](loan-tranche-api-reference.md#get-pym-startdate) | No valid payment start date is resolved for the processing context. | Date resolution raises a configured application message. | Review payment period, maturity, due date and account-plan state. |
| [do_plan_control](loan-tranche-api-reference.md#do-plan-control) | get_plan_error returns one or more diagnostics. | Repayment-plan consistency gate raises an application error. | Review the diagnostic and reconcile plan/tranche values, then rerun control. |

## Internal helper failures

The following restrictions originate in private helper paths and should not be mistaken for separately published API methods.

| Internal routine | Condition | Public processing context |
|---|---|---|
| `prepare_loans_for_ovd_trs` | Only `MAIN` or `INT` is accepted for the plan-type selector; its queue-building errors may be wrapped with `SQLERRM`. | [do_overdue_transit_operation](loan-tranche-api-reference.md#do-overdue-transit-operation) |
| `pen_main_correction` | Adjusts previously allocated penalty amounts across tranche history and balances; consistency depends on its calling transaction. | Interest/penalty history processing |

## Plan checker behavior

[get_plan_error](loan-tranche-api-reference.md#get-plan-error) evaluates consistency of tranche balances and payment-plan data, and assembles explanatory diagnostics rather than repairing the schedule. [do_plan_control](loan-tranche-api-reference.md#do-plan-control) calls the function with the supplied document number, check type and processing date; when it receives non-null diagnostic text it invokes `R(...)`. **This tranche procedure does not perform the `LOAN_EOD_CONTROL_EXP` bypass used in the separate `LOAN_API` implementation.**

## Notes on diagnostic messages

- A dynamic error message may contain a document number, tranche number, configured amount or original `SQLERRM`; matching its complete text is not a reliable integration contract.
- Generic `WHEN OTHERS` handlers may wrap database exceptions. These are separated conceptually from explicit business checks in the tables above.
- Some routines record or query batch errors through `GNI_UTILITY`. Such diagnostics can affect per-loan batch execution without implying the entire batch has rolled back.
- Business validation listed for a public method may originate in a private routine or delegated service. The exact runtime propagation depends on the called implementation.

## Transaction behavior

Balance adjustments, payment-plan changes, transaction history and journal entries may be executed together or delegated to `LOAN_API` and KRD2204 services. **The source does not establish a blanket automatic rollback rule.** Confirm transaction ownership and rollback semantics at the actual caller / deployment layer; do not assume an `R(...)` failure always undoes earlier external side effects.

## Source mapping and review boundaries

- The entries describe active source-observed checks, not commented-out validation fragments or hypothetical Oracle errors.
- A duplicate/ambiguous schedule result and a missing schedule result are distinct cases, even when both block conversion.
- This document should be read with the [API Reference](loan-tranche-api-reference.md) for each method's inputs and business behavior.
- The duplicated local declaration noted in `do_create_tranche` is a separate **source/compiler review question**, not a business error category; it has not been treated as a runtime exception here.

## Coverage and limitations

- **19** curated public-method/condition associations across **16** distinct public methods, plus private-helper context.
- This is selected coverage, not all possible runtime failures or all messages raised by delegated packages.
- No Oracle production/test runtime status, error-code contract or transaction rollback semantics were independently verified.

[↑ Overview](loan-tranche-api.md)
