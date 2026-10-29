# LOAN_API — Error Handling

[← Overview](index.md) · [API Reference](api.md) · [Workflows](workflows.md)

This is a curated register of important source-observed validation and processing conditions, not a complete inventory of Oracle exceptions or runtime errors. Conditions are explained in business terms instead of reproducing incomplete dynamic error prefixes. The called error routine `R(...)` is not defined in the supplied package; its exact Oracle error code and propagation contract are not asserted.

## Error categories

### Business validation and input constraints

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [generate_allocated_payment_list](api.md#generate-allocated-payment-list) | Requested payment amount is negative. | Payment allocation is rejected. | Provide a non-negative payment amount. |
| [generate_allocated_payment_list](api.md#generate-allocated-payment-list) | Allocated amount exceeds the available balance. | Payment allocation is rejected. | Reconcile the payment amount with the eligible account balance. |
| [post_int_amount_to_plan](api.md#post-int-amount-to-plan) | Requested account group is not eligible for posting. | The schedule amount is not posted. | Check configured `ACC_TYPE_GROUP` and supported account type. |
| [post_int_amount_to_plan](api.md#post-int-amount-to-plan) | Tranche selector is neither Y nor N. | The schedule update is rejected. | Pass `Y` for tranche-specific or `N` for standard schedule. |
| [get_amount_for_pen_calc](api.md#get-amount-for-pen-calc-1) | Overdue interest or supplied overdue balance is negative. | Penalty base cannot be calculated on the invalid amounts. | Correct the overdue balance or schedule amounts. |
| [get_interest_calc_dates](api.md#get-interest-calc-dates) | Invalid interest calculation timing. | The calculation date list is not produced. | Use the supported EOD/SOD timing value. |
| [get_interest_calc_dates](api.md#get-interest-calc-dates) | Unsupported processing phase. | The calculation date list is not produced. | Use E or S. |
| [get_interest_calc_dates_ex](api.md#get-interest-calc-dates-ex) | Invalid interest calculation timing. | The calculation date list is not produced. | Use the supported EOD/SOD timing value. |
| [get_interest_calc_dates_ex](api.md#get-interest-calc-dates-ex) | Unsupported processing phase. | The calculation date list is not produced. | Use E or S. |
| [get_voucher_data_from_query](api.md#get-voucher-data-from-query) | Unsupported voucher query output mode. | Voucher result extraction is rejected. | Use SINGLE_ROW or MULTI_ROW. |
| [get_voucher_data_from_query](api.md#get-voucher-data-from-query) | Required voucher grouping key is missing or NULL. | Grouped voucher output cannot be assembled. | Return a non-null GROUP_CODE column. |
| [loan_int_diff_operation](api.md#loan-int-diff-operation) | Unsupported interest-difference operation. | Interest-difference operation is rejected. | Use one of the supported operation types. |
| [get_gl_acc_from_matrix](api.md#get-gl-acc-from-matrix) | Invalid or unsupported account type. | GL account mapping cannot complete. | Verify account type configuration. |
| [get_ovd_date](api.md#get-ovd-date) | Unsupported overdue date calculation mode. | Overdue date evaluation is rejected. | Select FOR_PEN_CALC or DEFAULT. |
| [get_ovd_date_old](api.md#get-ovd-date-old) | Unsupported overdue date calculation mode. | Overdue date evaluation is rejected. | Select FOR_PEN_CALC or DEFAULT. |
| [get_amount_for_pen_calc](api.md#get-amount-for-pen-calc-2) | Special-day flag is invalid. | Penalty calculation is rejected. | Use Y or N. |
| [table_log_operation](api.md#table-log-operation) | Unsupported audit log operation. | Table log processing is rejected. | Use LOGGING or RESTORING. |
| [do_journal_entry](api.md#do-journal-entry) | Unsupported journal transaction mode. | The requested journal operation is rejected. | Use CREATE or POST. |
| [recalculate_int_payment_plan](api.md#recalculate-int-payment-plan) | Invalid interest schedule update mode. | Recalculation is rejected. | Use supported update type 3 or 4. |
| [create_loan_history](api.md#create-loan-history) | Invalid history transaction operation. | History operation is rejected. | Use I, U or D. |
| [create_overdrafts](api.md#create-overdrafts) | Unsupported overdraft batch operation. | The requested overdraft action is rejected. | Use CREATE_OVERDRAFT or CLOSE_LOAN. |
| [do_create_tranche](api.md#do-create-tranche) | Unsupported tranche operation mode. | The tranche action is rejected. | Use CREATE or DELETE. |

### Configuration and data lookup failures

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [get_gl_acc_from_matrix](api.md#get-gl-acc-from-matrix) | No GL mapping matches the supplied attributes. | Required GL account cannot be resolved. | Review product and GL matrix configuration. |
| [get_gl_acc_from_matrix](api.md#get-gl-acc-from-matrix) | Multiple GL mappings match the supplied attributes. | The GL account selection is ambiguous. | Remove or disambiguate overlapping mapping configuration. |
| [get_amount_by_range](api.md#get-amount-by-range) | Required amount-range configuration is absent. | Range resolution fails. | Configure a matching amount range. |
| [get_amount_by_range](api.md#get-amount-by-range) | Multiple amount ranges match the request. | Range resolution is ambiguous. | Correct overlapping ranges. |
| [create_journal_entries](api.md#create-journal-entries) | Loan operation description configuration is absent. | Journal definition cannot be resolved. | Configure the relevant operation entry. |
| [get_trn_caller_id](api.md#get-trn-caller-id) | No transaction caller mapping exists for the requested operation type. | Caller identification fails. | Configure `LOAN_OPERATION_DESC` for the operation. |
| [do_int_calculation](api.md#do-int-calculation) | Product configuration is missing. | Interest processing is blocked for the affected product. | Review product configuration in `PKRD_TURKOD_PR`. |
| [do_overdue_transit_operation](api.md#do-overdue-transit-operation) | Product configuration is missing. | Overdue transition processing cannot resolve product settings. | Configure the loan product. |
| [change_account_status](api.md#change-account-status) | Requested account status is unsupported. | Account status update is rejected. | Use O or C. |

### Operation state and processing restrictions

| Public API | Triggering condition | Business effect | Corrective action |
|---|---|---|---|
| [do_commission_recognition](api.md#do-commission-recognition) | Operation date is not month-end. | Commission recognition is not executed. | Execute on the required last calendar day. |
| [do_int_calculation](api.md#do-int-calculation) | Prerequisite interest processing is incomplete. | Downstream calculation stage is prevented. | Complete the preceding interest calculation stage. |
| [update_loan_history](api.md#update-loan-history) | Missing transaction identifier. | History update cannot identify its transaction. | Supply a valid transaction ID. |

## Notes on diagnostic messages

- Several original messages concatenate document numbers, account types, and other runtime values. The tables above describe the triggering condition instead of listing incomplete message fragments.
- Generic `WHEN OTHERS` wrappers, language syntax and routine logging text are deliberately excluded.
- The public API method reference is the source for overload signatures and broader implementation behavior.

## Transaction behavior

Transaction effects must be reviewed per method. For example, `evaluate_limit_amount` contains a conditional `COMMIT` when both `p_caller_tx_fid` and `p_doc_no` are null. Do not generalize this behavior to the whole package.

## Source mapping and review boundaries

- Overloaded `get_amount_for_pen_calc` signatures must be mapped by parameter list, not method name alone. Negative overdue balance validation belongs to overload 1 (accepting `p_ovd_balance`).
- A public method may call private procedures that raise an error; direct and delegated error sources must be distinguished when extending this register.
- The current selection is **not exhaustive**. Absence from this table does not imply a method cannot fail.

## Coverage and limitations

- **34** method/message associations confirmed in the uncommented supplied Body.
- **1** proposed associations excluded because the literal was not verified within an extracted active method segment.
- This is a curated reference, not a list of all possible Oracle exceptions.

[↑ Overview](index.md)