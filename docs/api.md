# LOAN_API — API Reference

[← Back to Overview](index.md) · [Workflows](workflows.md) · [Data Model](data-model.md) · [Dependencies](dependencies.md) · [Error Handling](error-handling.md)

<a id="api-index"></a>
## API Index

### [Loan operations and controls](#domain-loan-operations-and-controls)

Coordinates loan operations, validation, and operational control checks.

**Procedures**
- [do_delete_operation](#do-delete-operation)
- [do_plan_control](#do-plan-control)
- [do_loan_control](#do-loan-control-1) — 2 parameters
- [do_loan_control](#do-loan-control-2) — 4 parameters
- [do_loan_status_control](#do-loan-status-control)
- [init_global_variables](#init-global-variables)

**Functions**
- [get_amount_by_range](#get-amount-by-range)
- [get_loan_amount](#get-loan-amount)
- [get_offer_id](#get-offer-id)

### [Accounts, guarantees and closure](#domain-accounts-guarantees-and-closure)

Manages loan-linked accounts, guarantees, collateral release, and closure activities.

**Procedures**
- [create_account](#create-account)
- [do_loan_collateral_out](#do-loan-collateral-out)
- [do_guarantee_out](#do-guarantee-out)
- [do_Loan_Close_With_Collateral_Out](#do-loan-close-with-collateral-out)
- [do_non_cash_loan_close](#do-non-cash-loan-close)
- [change_account_status](#change-account-status)

**Functions**
- [get_gl_acc_active_passive](#get-gl-acc-active-passive)
- [get_gl_acc_from_matrix](#get-gl-acc-from-matrix)
- [get_account_feature](#get-account-feature)
- [get_new_acc](#get-new-acc)
- [get_acc_type_by_status](#get-acc-type-by-status)

### [Payments and balances](#domain-payments-and-balances)

Allocates and evaluates loan payments, plan amounts, and outstanding amounts.

**Procedures**
- [post_int_amount_to_plan](#post-int-amount-to-plan)
- [generate_allocated_payment_list](#generate-allocated-payment-list)
- [add_payment_details](#add-payment-details)
- [do_Auto_Payment](#do-auto-payment)

**Functions**
- [get_overdue_min_amount](#get-overdue-min-amount)
- [get_overdue_grace_days](#get-overdue-grace-days)
- [get_next_Plan_Pym](#get-next-plan-pym)
- [get_plan_payment_amount](#get-plan-payment-amount)

### [Payment schedules and dates](#domain-payment-schedules-and-dates)

Produces and adjusts repayment schedules, installment dates, and grace-period information.

**Procedures**
- [recalculate_int_payment_plan](#recalculate-int-payment-plan)
- [do_schedule_allignment](#do-schedule-allignment)
- [do_com_schedule_allignment](#do-com-schedule-allignment)
- [get_special_schedule](#get-special-schedule)
- [get_created_schedule](#get-created-schedule)
- [create_schedule](#create-schedule)
- [UPDATE_PAYMENT_PLAN](#update-payment-plan)

**Functions**
- [get_formatted_grace_from_list](#get-formatted-grace-from-list)
- [get_grace_info](#get-grace-info)
- [get_commission_schedule](#get-commission-schedule)
- [add_months_for_loan](#add-months-for-loan)
- [get_Plan_Month_Count](#get-plan-month-count)
- [get_plan_paid_month_count](#get-plan-paid-month-count)
- [get_plan_error](#get-plan-error)
- [get_Annuity_Amount](#get-annuity-amount)
- [get_Pym_Startdate](#get-pym-startdate)
- [is_Payment_Date](#is-payment-date)
- [get_schedule_from_memo](#get-schedule-from-memo)
- [get_standart_schedule](#get-standart-schedule)
- [get_annuity_schedule](#get-annuity-schedule)

### [Interest, penalties and rates](#domain-interest-penalties-and-rates)

Calculates or retrieves interest, penalty bases, rates, and recognition information.

**Procedures**
- [get_interest_calc_info](#get-interest-calc-info)
- [loan_int_diff_operation](#loan-int-diff-operation)
- [do_int_calculation](#do-int-calculation)
- [do_commission_recognition](#do-commission-recognition)

**Functions**
- [get_amount_for_pen_calc](#get-amount-for-pen-calc-1) — date, varchar2
- [get_interest_calc_dates](#get-interest-calc-dates)
- [get_interest_calc_dates_ex](#get-interest-calc-dates-ex)
- [get_basis_year_days_cnt](#get-basis-year-days-cnt)
- [Get_Delay_Int_Rate](#get-delay-int-rate)
- [get_amount_for_pen_calc](#get-amount-for-pen-calc-2) — Varchar2, Date
- [get_flow_ratio_info](#get-flow-ratio-info)
- [get_stop_ratio_info](#get-stop-ratio-info)
- [get_incorrect_days](#get-incorrect-days)
- [get_incorrect_days_count](#get-incorrect-days-count)

### [Delinquency and status](#domain-delinquency-and-status)

Determines overdue transitions, overdue dates, and related overdue processing.

**Procedures**
- [do_overdue_transit_operation](#do-overdue-transit-operation)

**Functions**
- [get_ovd_trs_oper_type](#get-ovd-trs-oper-type)
- [get_ovd_date](#get-ovd-date)
- [get_ovd_date_old](#get-ovd-date-old)

### [Limits, tranches and overdrafts](#domain-limits-tranches-and-overdrafts)

Evaluates credit limits, creates tranches, and processes overdraft contracts.

**Procedures**
- [evaluate_limit_amount](#evaluate-limit-amount)
- [do_create_tranche](#do-create-tranche)
- [create_overdrafts](#create-overdrafts)

**Functions**
- [get_new_ov_cont_no](#get-new-ov-cont-no)

### [Accounting and audit](#domain-accounting-and-audit)

Creates accounting postings and retrieves accounting mappings and journal information.

**Procedures**
- [table_log_operation](#table-log-operation)
- [do_journal_entry](#do-journal-entry)
- [create_journal_entries](#create-journal-entries)
- [do_journal_control](#do-journal-control)

**Functions**
- [get_trn_caller_id](#get-trn-caller-id)
- [get_voucher_data_from_query](#get-voucher-data-from-query)
- [get_gl_value](#get-gl-value)

### [History, metadata and reversal](#domain-history-metadata-and-reversal)

Captures transaction metadata and loan operation history, and supports reversal of recorded changes.

**Procedures**
- [clear_addtional_data](#clear-addtional-data)
- [add_additional_group](#add-additional-group)
- [add_additional_info](#add-additional-info-1) — varchar2, varchar2
- [add_additional_info](#add-additional-info-2) — varchar2, date
- [add_column_update](#add-column-update-1) — varchar2, varchar2
- [add_column_update](#add-column-update-2) — varchar2, date
- [finalize_addtional_process](#finalize-addtional-process)
- [reverse_update](#reverse-update)
- [create_loan_history](#create-loan-history)
- [update_loan_history](#update-loan-history)

**Functions**
- [get_additional_info](#get-additional-info)
- [is_online_offer](#is-online-offer)

<a id="domain-loan-operations-and-controls"></a>
## Loan operations and controls

**Responsibility:** Coordinates loan operations, validation, and operational control checks.

[↑ API Index](#api-index)

### Procedures

<a id="do-delete-operation"></a>
#### do_delete_operation

**Type:** Procedure

**Description**

Reverses a previously recorded loan operation identified by its transaction ID. The procedure retrieves the related operation history and accounting journal information, checks the requested date and payment-plan controls, and delegates reversal of the affected loan and accounting records. The reversal is tied to the original transaction rather than a new loan request.


**Signature**

```sql
procedure do_delete_operation(p_tx_fid       number,
                                p_date_control varchar2 default 'Y',
                                p_plan_control varchar2 default 'Y');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier |
| `p_date_control` | IN | Supplied `p_date_control` input (varchar2) |
| `p_plan_control` | IN | Supplied `p_plan_control` input (varchar2) |

**Dependencies:** `LOAN_TRANCHE_API`, `MM_ACCOUNTING#`, `KRD_EOD` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="do-plan-control"></a>
#### do_plan_control

**Type:** Procedure

**Description**

Validates the repayment schedule of the specified loan for the requested processing date. Before running plan validation, the procedure checks `LOAN_EOD_CONTROL_EXP` for an exception matching the loan document number. If a matching exception exists, it exits without evaluating the plan.

For loans not excluded from control, it calls `get_plan_error` to evaluate the repayment schedule as of the selected date. A nonempty error result is raised as an application error; otherwise the procedure finishes without updating the plan. This is a validation gate, not a schedule-generation routine.

**Signature**

```sql
procedure do_plan_control(p_document_no in varchar2,
                            p_date        date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_document_no` | IN | Supplied `p_document_no` input (varchar2) |
| `p_date` | IN | Effective/processing date |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="do-loan-control-1"></a>
#### do_loan_control

**Type:** Procedure

**Description**

Initiates the broader loan-control process for an operation type and processing date. It uses the loan control records and loan details available for that processing context, while delegating supporting orchestration to banking utilities. The operation type can be omitted, allowing the procedure to apply the processing scope established by its implementation.


**Signature**

```sql
procedure do_loan_control(p_oper_type varchar2 default null,
                            p_date      date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation type selector |
| `p_date` | IN | Effective/processing date |

**Dependencies:** `GNI_UTILITY` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="do-loan-control-2"></a>
#### do_loan_control

**Type:** Procedure

**Description**

Processes a selected range of loan-control records using the supplied row ID boundaries, operation type and processing date. The procedure reads loan details, control exceptions and operation history, and writes the results to `LOAN_EOD_CONTROL`. This overload is designed for bounded or parallel control execution and contains explicit transaction commits.


**Signature**

```sql
procedure do_loan_control(p_oper_type varchar2 default null,
                            p_date      date default glbl.getsysdate,
                            p_min_rowid rowid,
                            p_max_rowid rowid);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation type selector |
| `p_date` | IN | Effective/processing date |
| `p_min_rowid` | IN | Supplied `p_min_rowid` input (rowid) |
| `p_max_rowid` | IN | Supplied `p_max_rowid` input (rowid) |

**Dependencies:** `GLBL` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="do-loan-status-control"></a>
#### do_loan_status_control

**Type:** Procedure

**Description**

Evaluates the loan status for the account identified by branch, customer and loan account number. The procedure uses loan portfolio information to determine the relevant loan record and performs status-control processing, including related messages when applicable. Its scope is limited to the specified account rather than the entire loan portfolio.


**Signature**

```sql
procedure do_loan_status_control(p_branch_no   in number,
                                   p_customer_no in number,
                                   p_loan_acc    in number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_no` | IN | Branch identifier |
| `p_customer_no` | IN | Customer identifier |
| `p_loan_acc` | IN | Supplied `p_loan_acc` input (number) |

**Dependencies:** `GNI_MESSAGE` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="init-global-variables"></a>
#### init_global_variables

**Type:** Procedure

**Description**

Loads and optionally resets shared package-level loan data for the specified document number. It retrieves the associated offer, loan detail, facility and repayment-plan records from `KRD_TEKLIF`, `KRD_TEKLIF_DETAY`, `KRD_TESIS` and `KRD_ODEME_PLAN`. Missing or invalid required records are reported instead of silently initializing incomplete loan state.


**Signature**

```sql
Procedure init_global_variables(p_doc_no varchar2,
                                  p_reset  boolean default false);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_reset` | IN | Supplied `p_reset` input (boolean) |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

### Functions

<a id="get-amount-by-range"></a>
#### get_amount_by_range

**Type:** Function

**Description**

Determines a configured amount from `LOAN_AMOUNT_RANGE_INFO` for the requested loan operation, account type and transaction amount. The selection matches `OPER_TYPE` and `ACC_TYPE`, requires the amount to fall within the configured `MIN_AMOUNT` and `MAX_AMOUNT` bounds, and considers the tranche type where the configuration either permits a NULL value or matches the supplied type.

When exactly one configuration matches, the function returns its `RETURN_AMOUNT`, treating a NULL result as zero. No matching range or overlapping matching ranges are treated as configuration errors. The function resolves a configured value; it does not post a payment.

**Signature**

```sql
function get_amount_by_range(p_oper_type    in varchar2,
                               p_acc_type     in varchar2,
                               p_tranche_type in varchar2 default null,
                               p_amount       in number) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation type selector |
| `p_acc_type` | IN | Loan account type |
| `p_tranche_type` | IN | Supplied `p_tranche_type` input (varchar2) |
| `p_amount` | IN | Amount supplied for processing |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="get-loan-amount"></a>
#### get_loan_amount

**Type:** Function

**Description**

Calculates a loan amount for the selected document, date and amount category (`TOTAL`, `DEBT` or `DUE`). It joins the loan's linked `CS_ACCOUNT` records with account-type and payment-allocation configuration and evaluates accounts with a nonzero balance. When an operation type is supplied, the matching allocation configuration controls which account types participate.

For `TOTAL`, it sums eligible account balances. For `DEBT`, it restricts the sum to the configured payable accounts. For `DUE`, it calculates the amount to collect using the account's allocation method: balance-based entries use the current eligible balance, whereas schedule-based entries use the relevant payment-plan amount for the requested date. The function therefore distinguishes the loan's total amount, collectible debt and current scheduled amount rather than returning the same balance for every request.

**Signature**

```sql
function get_loan_amount(p_doc_no      in varchar2,
                           p_amount_type in varchar2,
                           p_date        in date default glbl.getsysdate,
                           p_oper_type   in varchar2 default null)
    return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_amount_type` | IN | Supplied `p_amount_type` input (varchar2) |
| `p_date` | IN | Effective/processing date |
| `p_oper_type` | IN | Operation type selector |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `P`, `O` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="get-offer-id"></a>
#### get_offer_id

**Type:** Function

**Description**

Generates or retrieves a loan offer identifier for the requested branch and date. It updates the offer-number tracking data in `KRD_TEKLIF_NO`, ensuring the returned identifier follows the configured numbering process. The method includes an explicit commit and therefore affects transaction handling even though it is declared as a function.


**Signature**

```sql
Function get_offer_id(p_branch_id In Number Default Glbl.Getsube,
                        P_date      In Date Default Glbl.Getsysdate)
    Return Varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_id` | IN | Supplied `p_branch_id` input (Number) |
| `P_date` | IN | Effective/processing date |

**Returns:** `Varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-loan-operations-and-controls)

---

<a id="domain-accounts-guarantees-and-closure"></a>
## Accounts, guarantees and closure

**Responsibility:** Manages loan-linked accounts, guarantees, collateral release, and closure activities.

[↑ API Index](#api-index)

### Procedures

<a id="create-account"></a>
#### create_account

**Type:** Procedure

**Description**

Creates or links the loan-related bank accounts required for the specified document and operation type. The procedure retrieves the loan's offer and facility details together with account-type mappings, product settings and account creation parameters to identify the required accounts.

The account-selection cursors evaluate configuration applicable to the requested operation and loan characteristics; conditional creation rules determine whether a particular account should be generated. Existing loan account references and relevant account settings are checked before new accounts are requested from the supporting account service. The procedure applies generated account identifiers to the corresponding loan or facility records as appropriate and returns the created-account collection through `P_CREATED_ACC`. Not every configured account type necessarily results in a new account for every operation.

**Signature**

```sql
procedure create_account(p_doc_no      in varchar2,
                           p_oper_type   in varchar2,
                           p_acc_type    in varchar2 default null,
                           p_tx_fid      in number,
                           p_created_acc out g_loan_accounts_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_oper_type` | IN | Operation type selector |
| `p_acc_type` | IN | Loan account type |
| `p_tx_fid` | IN | Transaction identifier |
| `p_created_acc` | OUT | g_loan_accounts_tab value populated by the operation |

**Dependencies:** `GLBL_TARIH`, `GLBL_NAME`, `GNI_MESSAGE` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="do-loan-collateral-out"></a>
#### do_loan_collateral_out

**Type:** Procedure

**Description**

Processes the release of collateral associated with the specified loan. It identifies linked customer accounts and applicable collateral types, including real estate, vehicles and other pledged asset records. The procedure updates the relevant collateral records to reflect the release process and uses the loan's account relationships to select the affected guarantees or assets.


**Signature**

```sql
procedure do_loan_collateral_out(p_doc_no in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |

**Dependencies:** `GLBL_EOD`, `TRN_BROKER`, `GLBL_CCY`, `PA_API#`, `MM_ACCOUNTING#` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="do-guarantee-out"></a>
#### do_guarantee_out

**Type:** Procedure

**Description**

Releases or processes guarantee obligations associated with a selected branch, customer or loan account. The procedure uses facility information and account balances to identify the eligible guarantee relationships, then delegates the necessary guarantee and transaction operations to supporting banking modules. The optional input parameters determine the scope of loans considered.


**Signature**

```sql
Procedure do_guarantee_out(p_branch_id   In Number Default Null,
                             p_customer_no In Number Default Null,
                             p_loan_acc    In Number Default Null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_id` | IN | Supplied `p_branch_id` input (Number) |
| `p_customer_no` | IN | Customer identifier |
| `p_loan_acc` | IN | Supplied `p_loan_acc` input (Number) |

**Dependencies:** `GLBL_BAKIYE`, `APEX_KRD2220_0_API_P`, `TRN_BROKER`, `KRD2220_0_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="do-loan-close-with-collateral-out"></a>
#### do_Loan_Close_With_Collateral_Out

**Type:** Procedure

**Description**

Coordinates loan closure together with collateral-release processing. It reads the related loan portfolio, automatic-close records and limit-change history, then updates loan status and relevant closure records. The procedure combines financial account closure with collateral-related handling, so its effects extend beyond a simple status flag update.


**Signature**

```sql
procedure do_Loan_Close_With_Collateral_Out(p_doc_no in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |

**Dependencies:** `GLBL_EOD`, `GLBL_HESAP`, `TRN_BROKER` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="do-non-cash-loan-close"></a>
#### do_non_cash_loan_close

**Type:** Procedure

**Description**

Processes closure of a non-cash loan using the specified loan document and processing date. It retrieves the related facility and account balances and delegates accounting or guarantee-related closure steps to the corresponding banking services. The procedure operates on the matched contract rather than performing a general batch closure.


**Signature**

```sql
Procedure do_non_cash_loan_close(p_doc_no in varchar2,
                                   p_Date   In Date Default Glbl.Getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_Date` | IN | Effective/processing date |

**Dependencies:** `GLBL_BAKIYE`, `APEX_KRD2220_0_API_P`, `TRN_BROKER`, `KRD2220_0_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="change-account-status"></a>
#### change_account_status

**Type:** Procedure

**Description**

Changes the account status associated with the specified loan document. It identifies the linked loan and account records from `KRD_TEKLIF_DETAY` and `CS_ACCOUNT`, then applies the requested open (`O`) or close (`C`) action through account services. It also interacts with loan-category handling where required by the operation.


**Signature**

```sql
procedure change_account_status(p_doc_no in varchar2,
                                  p_status in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_status` | IN | Supplied `p_status` input (varchar2) |

**Dependencies:** `GLBL_HESAP`, `LOAN_CATEGORY_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

### Functions

<a id="get-gl-acc-active-passive"></a>
#### get_gl_acc_active_passive

**Type:** Function

**Description**

Determines whether a GL account is configured as active or passive. The function looks up the specified account number in `GL_ACCOUNT_DEFINE` and returns the configured classification. A missing or ambiguous matching definition is reported rather than selecting an arbitrary account record.


**Signature**

```sql
function get_gl_acc_active_passive(p_acc_no in varchar2) return varchar2 result_cache;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_acc_no` | IN | Supplied `p_acc_no` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="get-gl-acc-from-matrix"></a>
#### get_gl_acc_from_matrix

**Type:** Function

**Description**

Resolves a general-ledger account from configured mapping records using the requested base type and loan account type. Where supplied, product code, customer type, term/period and currency are used to narrow or qualify the matching configuration; a loan document can provide additional context for the selection.

The lookup is intended to return a single applicable account rather than choose arbitrarily among possible configurations. It validates account-type eligibility and reports missing or multiple matching mapping rows. The returned GL account is subsequently used by account-creation or accounting operations; this function itself does not post journal entries.

**Signature**

```sql
function get_gl_acc_from_matrix(p_product_code in number default null,
                                  p_cust_type    in number default null,
                                  p_period_id    in number default null,
                                  p_currency_id  in number default null,
                                  p_base_type    in varchar2,
                                  p_loan_doc_no  in varchar2 default null,
                                  p_acc_type     in varchar2) return varchar2 result_cache;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_product_code` | IN | Supplied `p_product_code` input (number) |
| `p_cust_type` | IN | Supplied `p_cust_type` input (number) |
| `p_period_id` | IN | Supplied `p_period_id` input (number) |
| `p_currency_id` | IN | Supplied `p_currency_id` input (number) |
| `p_base_type` | IN | Supplied `p_base_type` input (varchar2) |
| `p_loan_doc_no` | IN | Supplied `p_loan_doc_no` input (varchar2) |
| `p_acc_type` | IN | Loan account type |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_HESAP` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="get-account-feature"></a>
#### get_account_feature

**Type:** Function

**Description**

Returns the stored feature or classification of a banking account. The lookup uses the account number and subaccount, together with the supplied branch where applicable, to select the matching `CS_ACCOUNT` record. This function reads account metadata and does not create or modify an account.


**Signature**

```sql
function get_account_feature(p_branch_no   number,
                               p_account_no  number,
                               p_account_sub number)
    return cs_account.account_feature%type;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_no` | IN | Branch identifier |
| `p_account_no` | IN | Supplied `p_account_no` input (number) |
| `p_account_sub` | IN | Supplied `p_account_sub` input (number) |

**Returns:** `cs_account.account_feature%type` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_TARIH` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="get-new-acc"></a>
#### get_new_acc

**Type:** Function

**Description**

Creates a new bank account for the supplied branch, customer, account configuration and loan. The function passes currency, account type, interest rates, product and loan-document details to account services, and returns the new account identifier. The optional parameters influence how the created account is configured and linked.


**Signature**

```sql
function get_new_acc(p_branch_no       in number,
                       p_customer_no     in number,
                       p_balance_acc     in varchar2,
                       p_detail_code     in varchar2,
                       p_ccy_id          in number,
                       p_main_acc        in number default null,
                       p_cr_int_rate     in number default null,
                       p_dr_int_rate     in number default null,
                       p_com_int_rate    in number default null,
                       p_product_code    in number default null,
                       p_period_id       in number default null,
                       p_acc_description in varchar2,
                       p_loan_doc_no     in varchar2,
                       p_loan_acc_type   in varchar2,
                       p_tx_fid          in number) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_no` | IN | Branch identifier |
| `p_customer_no` | IN | Customer identifier |
| `p_balance_acc` | IN | Supplied `p_balance_acc` input (varchar2) |
| `p_detail_code` | IN | Supplied `p_detail_code` input (varchar2) |
| `p_ccy_id` | IN | Supplied `p_ccy_id` input (number) |
| `p_main_acc` | IN | Supplied `p_main_acc` input (number) |
| `p_cr_int_rate` | IN | Supplied `p_cr_int_rate` input (number) |
| `p_dr_int_rate` | IN | Supplied `p_dr_int_rate` input (number) |
| `p_com_int_rate` | IN | Supplied `p_com_int_rate` input (number) |
| `p_product_code` | IN | Supplied `p_product_code` input (number) |
| `p_period_id` | IN | Supplied `p_period_id` input (number) |
| `p_acc_description` | IN | Supplied `p_acc_description` input (varchar2) |
| `p_loan_doc_no` | IN | Supplied `p_loan_doc_no` input (varchar2) |
| `p_loan_acc_type` | IN | Supplied `p_loan_acc_type` input (varchar2) |
| `p_tx_fid` | IN | Transaction identifier |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GNI_MESSAGE`, `GLBL_HESAP` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="get-acc-type-by-status"></a>
#### get_acc_type_by_status

**Type:** Function

**Description**

Resolves the destination account type when a loan changes status. It looks up `LOAN_ACC_TYPE_MAPPING` using the original account type together with the source and target loan statuses, then returns the mapped account type. Missing or inconsistent mappings are reported because the transition cannot be resolved reliably.


**Signature**

```sql
function get_acc_type_by_status(p_acc_type         varchar2,
                                  p_to_loan_status   varchar2,
                                  p_from_loan_status varchar2 default 'NORM')
    return varchar2 result_cache;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_acc_type` | IN | Loan account type |
| `p_to_loan_status` | IN | Supplied `p_to_loan_status` input (varchar2) |
| `p_from_loan_status` | IN | Supplied `p_from_loan_status` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounts-guarantees-and-closure)

---

<a id="domain-payments-and-balances"></a>
## Payments and balances

**Responsibility:** Allocates and evaluates loan payments, plan amounts, and outstanding amounts.

[↑ API Index](#api-index)

### Procedures

<a id="post-int-amount-to-plan"></a>
#### post_int_amount_to_plan

**Type:** Procedure

**Description**

Updates the interest installment amount for a loan and payment date in either the standard or tranche-specific schedule. It first resolves `P_ACC_TYPE` from `LOAN_ACC_TYPES_PR`, requiring payment-plan eligibility and the interest account group `INT`. It then finds the corresponding principal schedule using the loan document, date and mapped principal account type; the tranche query also matches the supplied tranche number.

If that principal row has schedule-update type `3` and no remaining payment amount, the routine returns without changing interest. Otherwise `P_WITH_TRANCHE_INFO_YN` selects `LOAN_PAYMENT_PLAN` (`N`) or `LOAN_TRANCHE_PAYMENT_PLAN` (`Y`). Before modification, the appropriate plan-log routine is called. The updated installment amount takes the supplied interest, already paid amount and unpaid amounts of earlier open installments into account, while grace-payment entries retain their existing scheduled amount. Invalid selectors or the absence of an updated plan row produce application errors.

**Signature**

```sql
procedure post_int_amount_to_plan(p_doc_no               in varchar2,
                                    p_tranche_no           in number,
                                    p_tx_fid               in number,
                                    p_acc_type             in varchar2,
                                    p_amount               in number,
                                    p_plan_date            in date,
                                    p_with_tranche_info_yn in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_tranche_no` | IN | Tranche identifier |
| `p_tx_fid` | IN | Transaction identifier |
| `p_acc_type` | IN | Loan account type |
| `p_amount` | IN | Amount supplied for processing |
| `p_plan_date` | IN | Supplied `p_plan_date` input (date) |
| `p_with_tranche_info_yn` | IN | Supplied `p_with_tranche_info_yn` input (varchar2) |

**Dependencies:** `LOAN_TRANCHE_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="generate-allocated-payment-list"></a>
#### generate_allocated_payment_list

**Type:** Procedure

**Description**

Builds a payment allocation list for the specified loan, operation type and requested payment amount. It identifies the eligible loan accounts, their balances and payment-plan information, then distributes the amount according to account and allocation configuration. Negative requested payments or amounts exceeding the available eligible balance are rejected; the outputs include the populated account allocation list and any applicable schedule-update indicator.


**Signature**

```sql
procedure generate_allocated_payment_list(p_doc_no                in varchar2,
                                            p_oper_type             in varchar2,
                                            p_pym_amount            in number,
                                            p_has_schedule_upd_type out varchar2,
                                            p_payment_list          out loan_api.g_loan_accounts_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_oper_type` | IN | Operation type selector |
| `p_pym_amount` | IN | Supplied `p_pym_amount` input (number) |
| `p_has_schedule_upd_type` | OUT | varchar2 value populated by the operation |
| `p_payment_list` | OUT | loan_api.g_loan_accounts_tab value populated by the operation |

**Dependencies:** `P`, `O` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="add-payment-details"></a>
#### add_payment_details

**Type:** Procedure

**Description**

Completes an existing payment allocation list with the information needed to execute the payment. For the specified loan it resolves payment destination accounts, account types, balances and related schedule details, then updates the supplied collection in place. The procedure also returns whether payment-plan schedule-update behavior applies to the assembled allocation.


**Signature**

```sql
procedure add_payment_details(p_doc_no                in varchar2,
                                p_has_schedule_upd_type out varchar2,
                                p_payment_list          in out loan_api.g_loan_accounts_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_has_schedule_upd_type` | OUT | varchar2 value populated by the operation |
| `p_payment_list` | IN OUT | loan_api.g_loan_accounts_tab value populated by the operation |

**Dependencies:** `O` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="do-auto-payment"></a>
#### do_Auto_Payment

**Type:** Procedure

**Description**

Executes automatic loan collections within the selected processing range, optionally restricting the batch to one loan document. Processing covers the `LP_PAYMENT` and `PAYMENT` operation paths and can use different payment passes depending on automatic-payment configuration and the optional test parameter.

The loan-selection cursors consider active contracts with automatic payment enabled and a linked payment account with available funds. They exclude configured EOD exceptions, legal-proceeding cases, product `50001` and payment accounts marked with the `NB` account feature. Branch limits, the document filter and applicable payment-operation settings further narrow the eligible records.

For each selected loan, the procedure calculates the collectible amount through `get_loan_amount`, using the scheduled `DUE` or outstanding `DEBT` amount as appropriate. Written-off loans can be processed using `WOFF_PAYMENT`. The candidates are handled using overdue-day, amount and next-payment-date ordering. Before an individual payment is applied, the linked payment account's balance is refreshed because multiple loan contracts may draw from the same account; the procedure then performs the appropriate allocation and payment steps.

**Signature**

```sql
Procedure do_Auto_Payment(p_min_id     number default null,
                            p_max_id     number default null,
                            p_doc_no     in varchar2 default null,
                            p_eod_sod    in varchar2 default 'E',
                            p_test_param in number default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Supplied `p_min_id` input (number) |
| `p_max_id` | IN | Supplied `p_max_id` input (number) |
| `p_doc_no` | IN | Loan document identifier |
| `p_eod_sod` | IN | EOD/SOD processing selector |
| `p_test_param` | IN | Supplied `p_test_param` input (number) |

**Dependencies:** `GNI_UTILITY`, `GLBL_BAKIYE`, `GLBL_CCY`, `LOAN_TRANCHE_API`, `APEX_KRD2204_0_API_P` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

### Functions

<a id="get-overdue-min-amount"></a>
#### get_overdue_min_amount

**Type:** Function

**Description**

Retrieves the minimum overdue threshold configured for the selected loan product in `PKRD_TURKOD_PR`. It matches the supplied product code to `URUN_KOD` and chooses the threshold according to the account category: `OVERDUE_MIN_AMOUNT_MAIN` for principal (`MAIN`) and `OVERDUE_MIN_AMOUNT_INT` for interest (`INT`).

A NULL threshold or an unsupported account-type selector is interpreted as zero. When the product configuration cannot be found, the lookup reports an application error. The value is used as a business threshold by subsequent overdue and penalty decisions, not as an amount to be posted.

**Signature**

```sql
function get_overdue_min_amount(p_product_code in number,
                                  p_acc_type     in varchar2) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_product_code` | IN | Supplied `p_product_code` input (number) |
| `p_acc_type` | IN | Loan account type |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="get-overdue-grace-days"></a>
#### get_overdue_grace_days

**Type:** Function

**Description**

Retrieves the overdue grace period configured for the specified loan product and account type. It reads the appropriate principal or interest grace-day setting from `PKRD_TURKOD_PR` after matching the product code. The returned day count is used when determining whether scheduled payments qualify for overdue processing.


**Signature**

```sql
function get_overdue_grace_days(p_product_code in number,
                                  p_acc_type     in varchar2) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_product_code` | IN | Supplied `p_product_code` input (number) |
| `p_acc_type` | IN | Loan account type |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="get-next-plan-pym"></a>
#### get_next_Plan_Pym

**Type:** Function

**Description**

Returns the next applicable payment-plan amount for a specified loan, account type and reference date. It reads scheduled installments from `LOAN_PAYMENT_PLAN` and uses the supplied date and account-type selection to identify the relevant future or outstanding plan payment. The function is a plan lookup rather than a payment execution routine.


**Signature**

```sql
Function get_next_Plan_Pym(p_doc_no   In Varchar2,
                             P_date     In Date,
                             p_acc_type in varchar2) Return Number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `P_date` | IN | Effective/processing date |
| `p_acc_type` | IN | Loan account type |

**Returns:** `Number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="get-plan-payment-amount"></a>
#### get_plan_payment_amount

**Type:** Function

**Description**

Retrieves the planned payment amount for the specified loan, date and optional account type. It determines whether the loan uses tranche-specific schedules and selects the applicable plan information accordingly. The result represents a scheduled amount for the selected context rather than a general account balance.


**Signature**

```sql
function get_plan_payment_amount(p_doc_no   in varchar2,
                                   p_date     in date default glbl.getsysdate,
                                   p_acc_type in varchar2 default null)
    return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_date` | IN | Effective/processing date |
| `p_acc_type` | IN | Loan account type |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `LOAN_TRANCHE_API` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payments-and-balances)

---

<a id="domain-payment-schedules-and-dates"></a>
## Payment schedules and dates

**Responsibility:** Produces and adjusts repayment schedules, installment dates, and grace-period information.

[↑ API Index](#api-index)

### Procedures

<a id="recalculate-int-payment-plan"></a>
#### recalculate_int_payment_plan

**Type:** Procedure

**Description**

Recalculates interest installments for a specified loan and account type using the principal amount, interest ratio and interest calculation convention. It updates corresponding plan rows under the requested schedule update mode, which must be `3` or `4`. The procedure adjusts the interest payment schedule rather than generating a new loan contract.


**Signature**

```sql
procedure recalculate_int_payment_plan(p_doc_no               in varchar2,
                                         p_acc_type             in varchar2,
                                         p_main_amount          in number,
                                         p_INT_RATIO            in number,
                                         p_int_calc_type        in varchar2,
                                         p_schedule_update_type in number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_acc_type` | IN | Loan account type |
| `p_main_amount` | IN | Supplied `p_main_amount` input (number) |
| `p_INT_RATIO` | IN | Supplied `p_INT_RATIO` input (number) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_schedule_update_type` | IN | Supplied `p_schedule_update_type` input (number) |

**Dependencies:** `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="do-schedule-allignment"></a>
#### do_schedule_allignment

**Type:** Procedure

**Description**

Aligns the repayment plan for a loan with a supplied payment amount, account type and date. It reads the relevant entries from `LOAN_PAYMENT_PLAN` and updates plan amounts or states to reflect the requested alignment. Only plan rows matching the loan and applicable payment context are affected.


**Signature**

```sql
Procedure do_schedule_allignment(p_doc_no         In Varchar2,
                                   p_payment_amount In Number,
                                   p_acc_type       In Varchar2,
                                   p_date           In Date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_payment_amount` | IN | Supplied `p_payment_amount` input (Number) |
| `p_acc_type` | IN | Loan account type |
| `p_date` | IN | Effective/processing date |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="do-com-schedule-allignment"></a>
#### do_com_schedule_allignment

**Type:** Procedure

**Description**

Adjusts a loan's commission repayment schedule for the requested option, amount and plan type. The procedure reads facility and commission-plan information, then updates `KRD_KOM_PLAN_DETAY` and related accounting records where the selected commission operation requires it. The migration indicator and option code affect the chosen processing path.


**Signature**

```sql
Procedure do_com_schedule_allignment(p_doc_no      In Varchar2,
                                       p_option_code In Varchar2,
                                       p_amount      In Number,
                                       p_plan_type   In Varchar2,
                                       p_mig_status  In Number Default 0);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_option_code` | IN | Supplied `p_option_code` input (Varchar2) |
| `p_amount` | IN | Amount supplied for processing |
| `p_plan_type` | IN | Supplied `p_plan_type` input (Varchar2) |
| `p_mig_status` | IN | Supplied `p_mig_status` input (Number) |

**Dependencies:** `GLOG` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-special-schedule"></a>
#### get_special_schedule

**Type:** Procedure

**Description**

Builds or updates a special repayment schedule for a loan using the requested principal amount, interest rate, calculation convention and optional annuity or remaining-interest values. It uses temporary schedule data in `ODEME_PLAN_TMP2` while preparing the result. The procedure supports a loan-specific schedule rather than returning a generic preconfigured plan.


**Signature**

```sql
Procedure get_special_schedule(p_doc_no             In Varchar2,
                                 p_begin_date         in Date Default Glbl.Getsysdate,
                                 p_offer_amount       In Number,
                                 p_int_ratio          In Number,
                                 p_int_calc_type      in varchar2,
                                 p_calc_int_int_grace in varchar2,
                                 p_annuity_amount     in Number Default Null,
                                 p_remain_int_amount  in number default 0);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_begin_date` | IN | Supplied `p_begin_date` input (Date) |
| `p_offer_amount` | IN | Supplied `p_offer_amount` input (Number) |
| `p_int_ratio` | IN | Supplied `p_int_ratio` input (Number) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_calc_int_int_grace` | IN | Supplied `p_calc_int_int_grace` input (varchar2) |
| `p_annuity_amount` | IN | Supplied `p_annuity_amount` input (Number) |
| `p_remain_int_amount` | IN | Supplied `p_remain_int_amount` input (number) |

**Dependencies:** `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-created-schedule"></a>
#### get_created_schedule

**Type:** Procedure

**Description**

Retrieves a previously generated schedule using its identifier. It selects the matching schedule information from the temporary or generated schedule data and prepares it for subsequent use. If no record exists for the supplied identifier, the procedure reports the missing schedule.


**Signature**

```sql
Procedure get_created_schedule(p_fid number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_fid` | IN | Supplied `p_fid` input (number) |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="create-schedule"></a>
#### create_schedule

**Type:** Procedure

**Description**

Generates a repayment schedule from the supplied loan amount, dates, interest settings, payment periods, grace rules and schedule options. It supports both standard and annuity-related configurations, including optional flow-rate, stop-rate and special-date inputs. The method routes the requested schedule generation into the appropriate calculations and stores or assembles the resulting repayment entries for the chosen target.


**Signature**

```sql
procedure create_schedule(p_schedule_no          number,
                            p_coll_name            varchar2 default null,
                            p_fk_id                number default null,
                            p_doc_no               varchar2 default 'XXX',
                            p_memorandum_id        varchar2 default null,
                            p_customer_no          number default null,
                            p_tx_fid               number default null,
                            p_begin_date           date default glbl.getsysdate,
                            p_last_int_Calc_Date   date default glbl.getsysdate - 1,
                            p_main_pymt_date       date,
                            p_int_pymt_date        date,
                            p_pymt_day             number default null,
                            p_end_date             date,
                            p_offer_amount         number,
                            p_ovd_main_amount      number default 0,
                            p_int_ratio            number,
                            p_int_calc_type        varchar2,
                            p_annuity_amount       number default null,
                            p_max_annuity_amount   number default null,
                            p_min_main_pay_amount  number default null,
                            p_remain_int_amount    number default 0,
                            p_main_grace_period    varchar2 default null,
                            p_int_grace_period     varchar2 default null,
                            p_main_pymt_period     number default 1,
                            p_int_pymt_period      number default 1,
                            p_product_code         number default null,
                            p_same_annuity         varchar2 default 'N',
                            p_calc_int_in_grace    varchar2 default 'Y',
                            p_return_type          varchar2 default 'NO_ZERO',
                            p_rest_main_pay_amount number default null,
                            p_rest_main_pay_months number default null,
                            p_loan_flow_rate_tab   LOAN_FLOW_RATE_TAB default null,
                            p_loan_stop_rate_tab   LOAN_STOP_RATE_TAB default null,
                            p_special_dates        loan_special_dates_tab default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_schedule_no` | IN | Supplied `p_schedule_no` input (number) |
| `p_coll_name` | IN | Supplied `p_coll_name` input (varchar2) |
| `p_fk_id` | IN | Supplied `p_fk_id` input (number) |
| `p_doc_no` | IN | Loan document identifier |
| `p_memorandum_id` | IN | Supplied `p_memorandum_id` input (varchar2) |
| `p_customer_no` | IN | Customer identifier |
| `p_tx_fid` | IN | Transaction identifier |
| `p_begin_date` | IN | Supplied `p_begin_date` input (date) |
| `p_last_int_Calc_Date` | IN | Supplied `p_last_int_Calc_Date` input (date) |
| `p_main_pymt_date` | IN | Supplied `p_main_pymt_date` input (date) |
| `p_int_pymt_date` | IN | Supplied `p_int_pymt_date` input (date) |
| `p_pymt_day` | IN | Supplied `p_pymt_day` input (number) |
| `p_end_date` | IN | Supplied `p_end_date` input (date) |
| `p_offer_amount` | IN | Supplied `p_offer_amount` input (number) |
| `p_ovd_main_amount` | IN | Supplied `p_ovd_main_amount` input (number) |
| `p_int_ratio` | IN | Supplied `p_int_ratio` input (number) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_annuity_amount` | IN | Supplied `p_annuity_amount` input (number) |
| `p_max_annuity_amount` | IN | Supplied `p_max_annuity_amount` input (number) |
| `p_min_main_pay_amount` | IN | Supplied `p_min_main_pay_amount` input (number) |
| `p_remain_int_amount` | IN | Supplied `p_remain_int_amount` input (number) |
| `p_main_grace_period` | IN | Supplied `p_main_grace_period` input (varchar2) |
| `p_int_grace_period` | IN | Supplied `p_int_grace_period` input (varchar2) |
| `p_main_pymt_period` | IN | Supplied `p_main_pymt_period` input (number) |
| `p_int_pymt_period` | IN | Supplied `p_int_pymt_period` input (number) |
| `p_product_code` | IN | Supplied `p_product_code` input (number) |
| `p_same_annuity` | IN | Supplied `p_same_annuity` input (varchar2) |
| `p_calc_int_in_grace` | IN | Supplied `p_calc_int_in_grace` input (varchar2) |
| `p_return_type` | IN | Supplied `p_return_type` input (varchar2) |
| `p_rest_main_pay_amount` | IN | Supplied `p_rest_main_pay_amount` input (number) |
| `p_rest_main_pay_months` | IN | Supplied `p_rest_main_pay_months` input (number) |
| `p_loan_flow_rate_tab` | IN | Supplied `p_loan_flow_rate_tab` input (LOAN_FLOW_RATE_TAB) |
| `p_loan_stop_rate_tab` | IN | Supplied `p_loan_stop_rate_tab` input (LOAN_STOP_RATE_TAB) |
| `p_special_dates` | IN | Supplied `p_special_dates` input (loan_special_dates_tab) |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="update-payment-plan"></a>
#### UPDATE_PAYMENT_PLAN

**Type:** Procedure

**Description**

Updates repayment-plan records associated with the specified loan document. It initializes the relevant loan context and applies the schedule changes required by the current contract configuration, including checks for tranche-related handling. The procedure modifies payment-plan state and should be reviewed alongside the loan's active schedule setup.


**Signature**

```sql
PROCEDURE UPDATE_PAYMENT_PLAN(
                              P_DOC_NO             varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `P_DOC_NO` | IN | Loan document identifier |

**Dependencies:** `API_CALENDAR`, `TRN_BROKER`, `GAIA` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

### Functions

<a id="get-formatted-grace-from-list"></a>
#### get_formatted_grace_from_list

**Type:** Function

**Description**

Converts a supplied grace-period list into the formatted representation used by repayment-plan processing. The function interprets the period-list values and assembles the corresponding readable or normalized sequence. It performs format conversion rather than querying an existing loan account.


**Signature**

```sql
function get_formatted_grace_from_list(p_period_list in varchar2)
    return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_period_list` | IN | Supplied `p_period_list` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-grace-info"></a>
#### get_grace_info

**Type:** Function

**Description**

Retrieves grace-period information for a specified loan, account type and period-end date. The requested information type controls which grace details are returned. The function uses the loan's repayment and grace configuration to report the portion relevant to the selected period.


**Signature**

```sql
function get_grace_info(p_Doc_No          In Varchar2,
                          p_period_end_date in date,
                          p_acc_type        In Varchar2 default 'A',
                          p_info_type       in varchar2) Return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Doc_No` | IN | Loan document identifier |
| `p_period_end_date` | IN | Supplied `p_period_end_date` input (date) |
| `p_acc_type` | IN | Loan account type |
| `p_info_type` | IN | Supplied `p_info_type` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-commission-schedule"></a>
#### get_commission_schedule

**Type:** Function

**Description**

Produces a commission payment schedule over the supplied date range for the specified amount and interest-calculation convention. The pipelined function calculates the corresponding commission installments and returns the generated schedule rows. Its output can be consumed as a table rather than written to a loan record by the function itself.


**Signature**

```sql
Function get_commission_schedule(p_begin_date    In Date,
                                   p_end_date      In Date,
                                   p_amount        In Number,
                                   p_int_calc_type in varchar2)
    Return Krd_Kom_Plan_Tt
    Pipelined;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_begin_date` | IN | Supplied `p_begin_date` input (Date) |
| `p_end_date` | IN | Supplied `p_end_date` input (Date) |
| `p_amount` | IN | Amount supplied for processing |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |

**Returns:** `Krd_Kom_Plan_Tt` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_FAIZ`, `GNI_MESSAGE`, `GLBL_TARIH` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="add-months-for-loan"></a>
#### add_months_for_loan

**Type:** Function

**Description**

Calculates a loan-related date by moving the supplied date forward or backward by the requested number of months. When a preferred payment day is supplied, the function tries to align the resulting date with that day, accounting for months in which the day does not exist. It returns the computed date without changing loan data.


**Signature**

```sql
function add_months_for_loan(p_date   date,
                               p_months number,
                               p_day    number default null) return date;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective/processing date |
| `p_months` | IN | Supplied `p_months` input (number) |
| `p_day` | IN | Supplied `p_day` input (number) |

**Returns:** `date` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-plan-month-count"></a>
#### get_Plan_Month_Count

**Type:** Function

**Description**

Calculates the number of repayment months between the supplied start and end dates under the chosen interest day-count convention. It also considers whether working-day adjustments and a preferred payment day apply. The returned count is used for schedule-related calculations rather than reflecting the number of payments already made.


**Signature**

```sql
Function get_Plan_Month_Count(p_Start_Date    In Date,
                                p_End_Date      In Date,
                                p_int_calc_type varchar2 default '30/360 AZ',
                                p_work_days     varchar2 default 'Y',
                                p_pymt_day      number default null)
    Return Number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Start_Date` | IN | Supplied `p_Start_Date` input (Date) |
| `p_End_Date` | IN | Supplied `p_End_Date` input (Date) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_work_days` | IN | Supplied `p_work_days` input (varchar2) |
| `p_pymt_day` | IN | Supplied `p_pymt_day` input (number) |

**Returns:** `Number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-plan-paid-month-count"></a>
#### get_plan_paid_month_count

**Type:** Function

**Description**

Determines the number of paid repayment periods for a loan up to the selected date. The calculation uses the loan document, contract term and payment-plan history to assess paid months. It returns a count, not the total monetary amount repaid.


**Signature**

```sql
function get_plan_paid_month_count(p_doc_no in varchar2,
                                     p_term   in number,
                                     p_date   in date) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_term` | IN | Supplied `p_term` input (number) |
| `p_date` | IN | Effective/processing date |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-plan-error"></a>
#### get_plan_error

**Type:** Function

**Description**

Evaluates a loan's repayment plan for control issues as of the supplied date. It examines relevant plan data and returns an error or control message when an inconsistency is identified. The function is used for validation and reporting rather than modifying the repayment schedule.


**Signature**

```sql
Function get_plan_error(p_doc_no varchar2,
                          p_date   date default glbl.getsysdate)
    return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_date` | IN | Effective/processing date |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-annuity-amount"></a>
#### get_Annuity_Amount

**Type:** Function

**Description**

Obtains the annuity installment amount associated with the specified loan. It uses the loan's scheduled repayment information and applies the requested return mode, including the `NO_ZERO` option. The function returns an amount and does not post an installment.


**Signature**

```sql
Function get_Annuity_Amount(p_Doc_No      In Varchar2,
                              p_return_type In varchar2 default 'NO_ZERO')
    Return Number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Doc_No` | IN | Loan document identifier |
| `p_return_type` | IN | Supplied `p_return_type` input (varchar2) |

**Returns:** `Number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-pym-startdate"></a>
#### get_Pym_Startdate

**Type:** Function

**Description**

Determines the first applicable repayment date from the supplied payment date, preferred payment day, installment interval, account type and loan maturity. The optional closed-plan check influences whether already closed installments are considered. It returns the selected start date for schedule processing.


**Signature**

```sql
Function get_Pym_Startdate(p_Doc_No          In Varchar2,
                             p_pymt_date       In Date,
                             p_pymt_day        In Number,
                             p_pymt_period     In Number,
                             p_acc_type        In varchar2,
                             p_loan_end_Date   in date,
                             p_check_is_closed in varchar2 default 'Y')
    Return Date;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Doc_No` | IN | Loan document identifier |
| `p_pymt_date` | IN | Supplied `p_pymt_date` input (Date) |
| `p_pymt_day` | IN | Supplied `p_pymt_day` input (Number) |
| `p_pymt_period` | IN | Supplied `p_pymt_period` input (Number) |
| `p_acc_type` | IN | Loan account type |
| `p_loan_end_Date` | IN | Supplied `p_loan_end_Date` input (date) |
| `p_check_is_closed` | IN | Supplied `p_check_is_closed` input (varchar2) |

**Returns:** `Date` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_TARIH`, `GNI_MESSAGE`, `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="is-payment-date"></a>
#### is_Payment_Date

**Type:** Function

**Description**

Checks whether the specified date is an applicable payment date for the supplied loan. The function evaluates the loan's repayment schedule or date rules and returns its payment-date indicator. It is a validation lookup and does not record a payment.


**Signature**

```sql
Function is_Payment_Date(p_Document_No In Varchar2, p_Date Date)
    Return Varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Document_No` | IN | Supplied `p_Document_No` input (Varchar2) |
| `p_Date` | IN | Effective/processing date |

**Returns:** `Varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_TARIH` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-schedule-from-memo"></a>
#### get_schedule_from_memo

**Type:** Function

**Description**

Generates a repayment schedule from the selected memorandum using the supplied interest ratio, calculation type and interest amount. The pipelined function reads the memorandum-related terms and returns calculated schedule rows. It does not represent a payment execution or loan account posting.


**Signature**

```sql
function get_schedule_from_memo(p_memorandum_id varchar2,
                                  p_int_ratio     number,
                                  p_int_calc_type varchar2,
                                  p_int_amount    number) return SCHEDULE_TT
    pipelined;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_memorandum_id` | IN | Supplied `p_memorandum_id` input (varchar2) |
| `p_int_ratio` | IN | Supplied `p_int_ratio` input (number) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_int_amount` | IN | Supplied `p_int_amount` input (number) |

**Returns:** `SCHEDULE_TT` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-standart-schedule"></a>
#### get_standart_schedule

**Type:** Function

**Description**

Calculates a standard loan repayment schedule between the supplied start and maturity dates. It considers principal and interest payment dates, payment day, amount, overdue principal, grace periods, day-count convention, working-day handling and optional varying or stopped rates. The pipelined result contains the computed repayment rows without directly committing them as a loan payment plan.


**Signature**

```sql
function get_standart_schedule(p_begin_date          date default glbl.getsysdate,
                                 p_main_pymt_date      date,
                                 p_int_pymt_date       date,
                                 p_pymt_day            number default null,
                                 p_end_date            date,
                                 p_offer_amount        number,
                                 p_ovd_main_amount     number default 0,
                                 p_int_calc_type       varchar2,
                                 p_main_grace_period   varchar2 default null,
                                 p_int_grace_period    varchar2 default null,
                                 p_remain_int_amount   number default 0,
                                 p_min_main_pay_amount number default null,
                                 p_calc_int_in_grace   varchar2 default 'Y',
                                 p_work_days           varchar2 default 'Y',
                                 p_loan_flow_rate_tab LOAN_FLOW_RATE_TAB default null,
                                 p_loan_stop_rate_tab LOAN_STOP_RATE_TAB default null,
                                 p_special_dates      loan_special_dates_tab default null)
    return SCHEDULE_TT
    pipelined;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_begin_date` | IN | Supplied `p_begin_date` input (date) |
| `p_main_pymt_date` | IN | Supplied `p_main_pymt_date` input (date) |
| `p_int_pymt_date` | IN | Supplied `p_int_pymt_date` input (date) |
| `p_pymt_day` | IN | Supplied `p_pymt_day` input (number) |
| `p_end_date` | IN | Supplied `p_end_date` input (date) |
| `p_offer_amount` | IN | Supplied `p_offer_amount` input (number) |
| `p_ovd_main_amount` | IN | Supplied `p_ovd_main_amount` input (number) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_main_grace_period` | IN | Supplied `p_main_grace_period` input (varchar2) |
| `p_int_grace_period` | IN | Supplied `p_int_grace_period` input (varchar2) |
| `p_remain_int_amount` | IN | Supplied `p_remain_int_amount` input (number) |
| `p_min_main_pay_amount` | IN | Supplied `p_min_main_pay_amount` input (number) |
| `p_calc_int_in_grace` | IN | Supplied `p_calc_int_in_grace` input (varchar2) |
| `p_work_days` | IN | Supplied `p_work_days` input (varchar2) |
| `p_loan_flow_rate_tab` | IN | Supplied `p_loan_flow_rate_tab` input (LOAN_FLOW_RATE_TAB) |
| `p_loan_stop_rate_tab` | IN | Supplied `p_loan_stop_rate_tab` input (LOAN_STOP_RATE_TAB) |
| `p_special_dates` | IN | Supplied `p_special_dates` input (loan_special_dates_tab) |

**Returns:** `SCHEDULE_TT` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="get-annuity-schedule"></a>
#### get_annuity_schedule

**Type:** Function

**Description**

Calculates an annuity repayment schedule using the supplied loan dates, principal, interest settings and optional annuity constraints. It accounts for payment-day rules, grace periods, remaining amounts, working-day adjustments and optional mortgage, special-date or changing-rate inputs. The function returns schedule rows through a pipelined result rather than executing the resulting installments.


**Signature**

```sql
function get_annuity_schedule(p_begin_date      date,
                                p_main_pymt_date  date,
                                p_int_pymt_date   date,
                                p_pymt_day        number default null,
                                p_end_date        date,
                                p_offer_amount    number,
                                p_ovd_main_amount number default 0,
                                p_int_calc_type   varchar2,
                                p_main_grace_period varchar2 default null,
                                p_int_grace_period  varchar2 default null,
                                p_annuity_amount     number default null,
                                p_max_annuity_amount number default null,
                                p_remain_int_amount  number default 0,
                                p_work_days         varchar2 default 'Y',
                                p_own_annuity       varchar2 default 'N',
                                p_calc_int_in_grace varchar2 default 'Y',
                                p_is_mortgage       varchar2 default 'N',
                                p_max_iteration number default null,
                                p_rest_main_pay_amount number default null,
                                p_rest_main_pay_months number default null,
                                p_loan_flow_rate_tab LOAN_FLOW_RATE_TAB default null,
                                p_loan_stop_rate_tab LOAN_STOP_RATE_TAB default null,
                                p_special_dates      loan_special_dates_tab default null)
    return SCHEDULE_TT
    pipelined;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_begin_date` | IN | Supplied `p_begin_date` input (date) |
| `p_main_pymt_date` | IN | Supplied `p_main_pymt_date` input (date) |
| `p_int_pymt_date` | IN | Supplied `p_int_pymt_date` input (date) |
| `p_pymt_day` | IN | Supplied `p_pymt_day` input (number) |
| `p_end_date` | IN | Supplied `p_end_date` input (date) |
| `p_offer_amount` | IN | Supplied `p_offer_amount` input (number) |
| `p_ovd_main_amount` | IN | Supplied `p_ovd_main_amount` input (number) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_main_grace_period` | IN | Supplied `p_main_grace_period` input (varchar2) |
| `p_int_grace_period` | IN | Supplied `p_int_grace_period` input (varchar2) |
| `p_annuity_amount` | IN | Supplied `p_annuity_amount` input (number) |
| `p_max_annuity_amount` | IN | Supplied `p_max_annuity_amount` input (number) |
| `p_remain_int_amount` | IN | Supplied `p_remain_int_amount` input (number) |
| `p_work_days` | IN | Supplied `p_work_days` input (varchar2) |
| `p_own_annuity` | IN | Supplied `p_own_annuity` input (varchar2) |
| `p_calc_int_in_grace` | IN | Supplied `p_calc_int_in_grace` input (varchar2) |
| `p_is_mortgage` | IN | Supplied `p_is_mortgage` input (varchar2) |
| `p_max_iteration` | IN | Supplied `p_max_iteration` input (number) |
| `p_rest_main_pay_amount` | IN | Supplied `p_rest_main_pay_amount` input (number) |
| `p_rest_main_pay_months` | IN | Supplied `p_rest_main_pay_months` input (number) |
| `p_loan_flow_rate_tab` | IN | Supplied `p_loan_flow_rate_tab` input (LOAN_FLOW_RATE_TAB) |
| `p_loan_stop_rate_tab` | IN | Supplied `p_loan_stop_rate_tab` input (LOAN_STOP_RATE_TAB) |
| `p_special_dates` | IN | Supplied `p_special_dates` input (loan_special_dates_tab) |

**Returns:** `SCHEDULE_TT` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-payment-schedules-and-dates)

---

<a id="domain-interest-penalties-and-rates"></a>
## Interest, penalties and rates

**Responsibility:** Calculates or retrieves interest, penalty bases, rates, and recognition information.

[↑ API Index](#api-index)

### Procedures

<a id="get-interest-calc-info"></a>
#### get_interest_calc_info

**Type:** Procedure

**Description**

Determines the day counts used for loan interest calculation. It evaluates the previous calculation date, transaction date, payment day, interest convention, rate type and periodicity, and returns both the calculation-day count and the applicable year-day basis. These outputs support subsequent interest calculations and do not themselves post interest.


**Signature**

```sql
procedure get_interest_calc_info(p_last_calc_date       in date,
                                   p_pymt_day             in number,
                                   p_trn_date             in date,
                                   p_int_calc_type        in varchar2,
                                   p_rate_type            in varchar2,
                                   p_int_rate_periodicity in varchar2,
                                   p_calc_day_count       out number,
                                   p_year_day_count       out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_last_calc_date` | IN | Supplied `p_last_calc_date` input (date) |
| `p_pymt_day` | IN | Supplied `p_pymt_day` input (number) |
| `p_trn_date` | IN | Supplied `p_trn_date` input (date) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_rate_type` | IN | Supplied `p_rate_type` input (varchar2) |
| `p_int_rate_periodicity` | IN | Supplied `p_int_rate_periodicity` input (varchar2) |
| `p_calc_day_count` | OUT | number value populated by the operation |
| `p_year_day_count` | OUT | number value populated by the operation |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="loan-int-diff-operation"></a>
#### loan_int_diff_operation

**Type:** Procedure

**Description**

Processes the difference between a loan's calculated interest and the related planned or recorded interest amounts. The operation uses the loan, account type, source account, transaction date, EOD/SOD context and supplied interest values to determine the adjustment. It returns the calculated difference through `P_CALC_AMOUNT` and may apply related interest-difference processing according to the transaction mode.


**Signature**

```sql
procedure loan_int_diff_operation(p_trn_date        in date,
                                    p_oper_type       in varchar2,
                                    p_trn_type        in varchar2,
                                    p_doc_no          in varchar2,
                                    p_acc_type        in varchar2,
                                    p_source_acc_type in varchar2,
                                    p_tx_fid          number,
                                    p_original_amount in number,
                                    p_eod_sod         in varchar2,
                                    p_payment_date    in date,
                                    p_plan_int_amount in number,
                                    p_int_amount      in number,
                                    p_calc_amount     out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_trn_date` | IN | Supplied `p_trn_date` input (date) |
| `p_oper_type` | IN | Operation type selector |
| `p_trn_type` | IN | Transaction direction/type selector |
| `p_doc_no` | IN | Loan document identifier |
| `p_acc_type` | IN | Loan account type |
| `p_source_acc_type` | IN | Supplied `p_source_acc_type` input (varchar2) |
| `p_tx_fid` | IN | Transaction identifier |
| `p_original_amount` | IN | Supplied `p_original_amount` input (number) |
| `p_eod_sod` | IN | EOD/SOD processing selector |
| `p_payment_date` | IN | Supplied `p_payment_date` input (date) |
| `p_plan_int_amount` | IN | Supplied `p_plan_int_amount` input (number) |
| `p_int_amount` | IN | Supplied `p_int_amount` input (number) |
| `p_calc_amount` | OUT | number value populated by the operation |

**Dependencies:** `GLBL` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="do-int-calculation"></a>
#### do_int_calculation

**Type:** Procedure

**Description**

Runs interest accrual and related loan interest processing for eligible loans in the supplied branch range or for a specific document, using the requested EOD/SOD phase and calculation date. Its selection queries combine loan details, product settings, account configuration and interest-processing controls to determine which contracts are eligible at that stage.

The procedure works with the last interest calculation date, applicable interest rate and day-count configuration, payment dates, and account balances when preparing calculation periods. Depending on the loan's configuration and processing phase, it calculates the interest amount, handles relevant interest differences or adjustments, and delegates account or plan postings to supporting loan operations. Configured exclusions and checks for missing product settings or incomplete prerequisite processing can prevent a loan from progressing through the next stage. This is a batch operation, not a simple function of principal multiplied by a rate.

**Signature**

```sql
procedure do_int_calculation(p_min_id  number default 0,
                               p_max_id  number default 999999999,
                               p_doc_no  in varchar2 default null,
                               p_eod_sod in varchar2 default 'E',
                               p_date    in date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Supplied `p_min_id` input (number) |
| `p_max_id` | IN | Supplied `p_max_id` input (number) |
| `p_doc_no` | IN | Loan document identifier |
| `p_eod_sod` | IN | EOD/SOD processing selector |
| `p_date` | IN | Effective/processing date |

**Dependencies:** `GLBL_TARIH`, `PR`, `GNI_UTILITY`, `LA`, `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="do-commission-recognition"></a>
#### do_commission_recognition

**Type:** Procedure

**Description**

Processes recognition of loan commissions for the selected contract or eligible set of loans. It uses the supplied date and related commission scheduling information to determine the amounts due for recognition. This batch operation is restricted to the last day of the month, reflecting its month-end accounting purpose.


**Signature**

```sql
Procedure do_commission_recognition(p_doc_no varchar2 default null,
                                      Pdate    In Date Default Glbl.Getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `Pdate` | IN | Supplied `Pdate` input (Date) |

**Dependencies:** `GLBL_TARIH`, `GLBL_BAKIYE`, `KRD_EOD` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

### Functions

<a id="get-amount-for-pen-calc-1"></a>
#### get_amount_for_pen_calc

**Type:** Function

**Description**

Calculates the monetary base for penalty calculation for a specified loan, combining the supplied overdue balance with selected unpaid overdue interest and applying optional guarantee deductions.

When `P_IS_PEN_CALC_ON_INT = 'Y'`, the function sums outstanding interest amounts from open `LOAN_PAYMENT_PLAN` rows in the `OVD_INT` account group. These rows must correspond to principal grace-payment entries of the same loan and payment date; the corresponding principal entry is identified through `LOAN_ACC_TYPES_PR`. A negative supplied overdue balance or calculated interest component is rejected.

If `P_IS_EXCLUDE_COLL_AMOUNT = 'Y'`, it sums nonzero committed guarantees linked to the loan through the guarantee tables, restricted to fund type `1` and guarantee subtype `C3`. It then compares the combined overdue amount against the applicable minimum threshold plus eligible guarantee coverage. Under the specified interest-on-penalty or maturity condition, an amount at or below that combined limit produces a zero base; otherwise eligible guarantees are deducted. The final result cannot be negative.

**Signature**

```sql
function get_amount_for_pen_calc(p_date                    in date,
                                   p_doc_no                  in varchar2,
                                   p_ovd_balance             in number,
                                   p_is_pen_calc_on_int      in varchar2,
                                   p_is_exclude_coll_amount  in varchar2,
                                   p_overdue_min_amount_main in number,
                                   p_loan_end_date           in date)
    return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective/processing date |
| `p_doc_no` | IN | Loan document identifier |
| `p_ovd_balance` | IN | Overdue balance included in the penalty base |
| `p_is_pen_calc_on_int` | IN | Supplied `p_is_pen_calc_on_int` input (varchar2) |
| `p_is_exclude_coll_amount` | IN | Supplied `p_is_exclude_coll_amount` input (varchar2) |
| `p_overdue_min_amount_main` | IN | Supplied `p_overdue_min_amount_main` input (number) |
| `p_loan_end_date` | IN | Contractual loan end date |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-interest-calc-dates"></a>
#### get_interest_calc_dates

**Type:** Function

**Description**

Returns the set of dates on which interest calculation should occur for the supplied last-calculation date and EOD/SOD context. The function uses the configured interest calculation phase to determine eligible processing dates. Unsupported phase or timing parameters are rejected instead of producing an arbitrary date sequence.


**Signature**

```sql
function get_interest_calc_dates(p_last_calc_access_date in date,
                                   p_eod_sod               in varchar2,
                                   p_int_calc_at           in varchar2)
    return loan_api.g_date_tab_type;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_last_calc_access_date` | IN | Supplied `p_last_calc_access_date` input (date) |
| `p_eod_sod` | IN | EOD/SOD processing selector |
| `p_int_calc_at` | IN | Supplied `p_int_calc_at` input (varchar2) |

**Returns:** `loan_api.g_date_tab_type` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_TARIH` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-interest-calc-dates-ex"></a>
#### get_interest_calc_dates_ex

**Type:** Function

**Description**

Determines interest calculation dates using the supplied reference date, EOD/SOD phase and interest-calculation timing. The function applies the corresponding phase rules and returns the calculated date collection. Invalid EOD/SOD or calculation-timing values are treated as parameter errors.


**Signature**

```sql
function get_interest_calc_dates_ex(p_date        in date,
                                      p_eod_sod     in varchar2,
                                      p_int_calc_at in varchar2)
    return g_date_tab_type;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective/processing date |
| `p_eod_sod` | IN | EOD/SOD processing selector |
| `p_int_calc_at` | IN | Supplied `p_int_calc_at` input (varchar2) |

**Returns:** `g_date_tab_type` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_TARIH` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-basis-year-days-cnt"></a>
#### get_basis_year_days_cnt

**Type:** Function

**Description**

Returns the annual day-count denominator required for the requested interest calculation convention and date. It accounts for the selected basis and calendar context where applicable. The result supports interest-rate prorating rather than changing the underlying loan rate.


**Signature**

```sql
function get_basis_year_days_cnt(p_int_calc_type in varchar2,
                                   p_date          in date) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |
| `p_date` | IN | Effective/processing date |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-delay-int-rate"></a>
#### Get_Delay_Int_Rate

**Type:** Function

**Description**

Retrieves the applicable delayed-interest rate for a specified loan and delay category. It reads loan settings from `KRD_TEKLIF_DETAY` and selects the rate associated with the requested delay type. The return value is a rate, not a calculated overdue amount.


**Signature**

```sql
Function Get_Delay_Int_Rate(p_Loan_Id    In Varchar2,
                              p_Delay_Type In Varchar2) Return Number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Loan_Id` | IN | Supplied `p_Loan_Id` input (Varchar2) |
| `p_Delay_Type` | IN | Supplied `p_Delay_Type` input (Varchar2) |

**Returns:** `Number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-amount-for-pen-calc-2"></a>
#### get_amount_for_pen_calc

**Type:** Function

**Description**

Calculates overdue amounts relevant to penalty assessment for the specified loan and date, with optional special-day treatment. The function considers outstanding overdue principal and interest and applies the requested interest calculation convention when day-weighted processing is needed. The `P_FOR_SPEC_DAYS` flag controls how special-day periods are handled and must be `Y` or `N`; this overload does not accept the guarantee-offset parameters of overload 1.


**Signature**

```sql
Function get_amount_for_pen_calc(p_doc_no        In Varchar2,
                                   P_date          In Date,
                                   p_for_spec_days in varchar2 default 'N',
                                   p_int_calc_type in varchar2 default null)
    Return Number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `P_date` | IN | Effective/processing date |
| `p_for_spec_days` | IN | Supplied `p_for_spec_days` input (varchar2) |
| `p_int_calc_type` | IN | Supplied `p_int_calc_type` input (varchar2) |

**Returns:** `Number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `GLBL_FAIZ` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-flow-ratio-info"></a>
#### get_flow_ratio_info

**Type:** Function

**Description**

Retrieves time-dependent interest flow-rate information for the specified loan as of the requested date. The function reads matching records from `LOAN_FLOW_RATIO_INFO`, taking their effective periods into account. The full-information flag controls the level of rate detail returned in the `LOAN_FLOW_RATE_TAB` collection.


**Signature**

```sql
FUNCTION get_flow_ratio_info(p_doc_no    VARCHAR2,
                               p_date      DATE DEFAULT glbl.getsysdate,
                               p_full_info varchar2 default 'N')
    RETURN LOAN_FLOW_RATE_TAB;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_date` | IN | Effective/processing date |
| `p_full_info` | IN | Supplied `p_full_info` input (varchar2) |

**Returns:** `LOAN_FLOW_RATE_TAB` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-stop-ratio-info"></a>
#### get_stop_ratio_info

**Type:** Function

**Description**

Retrieves loan rate-stoppage or suspension information applicable to the specified contract and date. It reads relevant intervals from `LOAN_STOP_RATIO_INFO`, with information type and full-detail parameters controlling the result. The function returns the selected stop-rate records as a `LOAN_STOP_RATE_TAB` collection.


**Signature**

```sql
FUNCTION get_stop_ratio_info(p_doc_no    VARCHAR2,
                               p_date      DATE DEFAULT glbl.getsysdate,
                               p_full_info varchar2 default 'N',
                               p_info_type varchar2 default 'INT')
    RETURN LOAN_STOP_RATE_TAB;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_date` | IN | Effective/processing date |
| `p_full_info` | IN | Supplied `p_full_info` input (varchar2) |
| `p_info_type` | IN | Supplied `p_info_type` input (varchar2) |

**Returns:** `LOAN_STOP_RATE_TAB` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-incorrect-days"></a>
#### get_incorrect_days

**Type:** Function

**Description**

Returns the dates or day identifiers treated as incorrect under the package's calendar rules for the supplied date. The pipelined function produces a row set that can be consumed by SQL. It is used for date/calendar evaluation and does not modify the loan account.


**Signature**

```sql
function get_incorrect_days(p_date date) return NUMBER_TT
    pipelined;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective/processing date |

**Returns:** `NUMBER_TT` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="get-incorrect-days-count"></a>
#### get_incorrect_days_count

**Type:** Function

**Description**

Counts the dates identified as incorrect by the package's calendar/day-validation logic for the supplied date. The function returns a numeric count and does not modify loan or payment-plan data.


**Signature**

```sql
function get_incorrect_days_count(p_date date) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective/processing date |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-interest-penalties-and-rates)

---

<a id="domain-delinquency-and-status"></a>
## Delinquency and status

**Responsibility:** Determines overdue transitions, overdue dates, and related overdue processing.

[↑ API Index](#api-index)

### Procedures

<a id="do-overdue-transit-operation"></a>
#### do_overdue_transit_operation

**Type:** Procedure

**Description**

Processes overdue transitions for loans matching the requested branch range, optional document number and processing date. It identifies eligible loan installments and overdue balances using repayment-plan records, overdue grace periods and product settings, excluding contracts outside the processing scope. The procedure determines the required overdue movement and updates the related accounts or loan records through the appropriate processing services.


**Signature**

```sql
Procedure do_overdue_transit_operation(p_min_id number default null,
                                         p_max_id number default null,
                                         p_doc_no in varchar2 default null,
                                         p_date   in date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Supplied `p_min_id` input (number) |
| `p_max_id` | IN | Supplied `p_max_id` input (number) |
| `p_doc_no` | IN | Loan document identifier |
| `p_date` | IN | Effective/processing date |

**Dependencies:** `GNI_UTILITY`, `CL`, `CR`, `LOAN_REPORT_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-delinquency-and-status)

---

### Functions

<a id="get-ovd-trs-oper-type"></a>
#### get_ovd_trs_oper_type

**Type:** Function

**Description**

Determines whether an installment requires an overdue-related operation, using the supplied account group, due date, grace days, current and overdue balances, maturity date and loan status. The decision logic evaluates whether the installment has passed the applicable grace threshold and whether the remaining amount exceeds the configured minimum.

The grace-day interpretation is influenced by `P_OVERDUE_GRACE_DAYS_TYPE` and `P_PLAN_TYPE`, so principal and interest may follow different transition criteria. The function returns the operation type required for the given installment state, or no operation when the conditions for a transition are not met. It only decides the transition; the caller performs the actual account movement.

**Signature**

```sql
function get_ovd_trs_oper_type(p_date               in date,
                                 p_acc_type_group     in varchar2,
                                 p_plan_date          in date,
                                 p_overdue_grace_days in number,
                                 p_balance            in number,
                                 p_overdue_min_amount in number,
                                 p_ovd_balance        in number,
                                 p_loan_end_date      in date,
                                 p_loan_status        in varchar2,
                                 P_OVERDUE_GRACE_DAYS_TYPE IN VARCHAR2,
                                 P_PLAN_TYPE               IN VARCHAR2,
                                 p_doc_no                  IN VARCHAR2 DEFAULT NULL)
    return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective/processing date |
| `p_acc_type_group` | IN | Supplied `p_acc_type_group` input (varchar2) |
| `p_plan_date` | IN | Supplied `p_plan_date` input (date) |
| `p_overdue_grace_days` | IN | Supplied `p_overdue_grace_days` input (number) |
| `p_balance` | IN | Supplied `p_balance` input (number) |
| `p_overdue_min_amount` | IN | Supplied `p_overdue_min_amount` input (number) |
| `p_ovd_balance` | IN | Supplied `p_ovd_balance` input (number) |
| `p_loan_end_date` | IN | Supplied `p_loan_end_date` input (date) |
| `p_loan_status` | IN | Supplied `p_loan_status` input (varchar2) |
| `P_OVERDUE_GRACE_DAYS_TYPE` | IN | Supplied `P_OVERDUE_GRACE_DAYS_TYPE` input (VARCHAR2) |
| `P_PLAN_TYPE` | IN | Supplied `P_PLAN_TYPE` input (VARCHAR2) |
| `p_doc_no` | IN | Loan document identifier |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-delinquency-and-status)

---

<a id="get-ovd-date"></a>
#### get_ovd_date

**Type:** Function

**Description**

Determines the overdue date associated with a loan and account type as of the supplied reference date. It examines loan, account and repayment-plan data, including tranche processing where applicable. The calculation mode selects normal reporting behavior or the date interpretation used for penalty calculation.


**Signature**

```sql
Function get_ovd_date(p_doc_no   varchar2,
                        p_acc_type varchar2,
                        p_date     date default glbl.getsysdate, 
                        p_calc_mode varchar2 default 'DEFAULT'
                        ) return date;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_acc_type` | IN | Loan account type |
| `p_date` | IN | Effective/processing date |
| `p_calc_mode` | IN | Supplied `p_calc_mode` input (varchar2) |

**Returns:** `date` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `LOAN_TRANCHE_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-delinquency-and-status)

---

<a id="get-ovd-date-old"></a>
#### get_ovd_date_old

**Type:** Function

**Description**

Calculates an overdue date using the earlier implementation of the loan overdue-date rules. It selects matching loan, account and payment-plan records and applies the requested reporting or penalty-calculation mode. This legacy function is retained separately from `get_ovd_date` and should not be assumed to apply identical logic.


**Signature**

```sql
Function get_ovd_date_old(p_doc_no   varchar2,
                        p_acc_type varchar2,
                        p_date     date default glbl.getsysdate,
                        p_calc_mode varchar2 default 'DEFAULT') return date;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_acc_type` | IN | Loan account type |
| `p_date` | IN | Effective/processing date |
| `p_calc_mode` | IN | Supplied `p_calc_mode` input (varchar2) |

**Returns:** `date` result; interpretation follows the documented purpose and source processing.

**Dependencies:** `LOAN_TRANCHE_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-delinquency-and-status)

---

<a id="domain-limits-tranches-and-overdrafts"></a>
## Limits, tranches and overdrafts

**Responsibility:** Evaluates credit limits, creates tranches, and processes overdraft contracts.

[↑ API Index](#api-index)

### Procedures

<a id="evaluate-limit-amount"></a>
#### evaluate_limit_amount

**Type:** Procedure

**Description**

Reconciles recorded credit-line limits with the amount available under each eligible contract. It selects active, non-deleted line/card loan records in the branch range and optional document scope, then compares the recorded limit-account balance with the offered limit adjusted for increases, decreases and outstanding principal. Principal is derived from tranche history for non-revolving lines or account balances for other lines. An active loan category `667` or passed maturity date makes the calculated available limit zero. For a difference, the procedure determines debit and credit directions, posts journal entries and records limit-evaluation history.


**Signature**

```sql
procedure evaluate_limit_amount(p_min_id        number default 0,
                                  p_max_id        number default 999999999,
                                  p_doc_no        in varchar2,
                                  p_caller_tx_fid in number default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Lower branch-number filter bound |
| `p_max_id` | IN | Upper branch-number filter bound |
| `p_doc_no` | IN | Loan document identifier |
| `p_caller_tx_fid` | IN | Optional originating transaction identifier |

**Dependencies:** `LOAN_REPORT_API`, `GLBL_HESAP` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-limits-tranches-and-overdrafts)

---

<a id="do-create-tranche"></a>
#### do_create_tranche

**Type:** Procedure

**Description**

Creates or removes a credit tranche for the specified loan contract according to `P_TRN_TYPE`. It finds the matching loan details in `KRD_TEKLIF_DETAY` and uses tranche-related services and transaction information to process the amount, tranche type and term. The transaction identifier is an `IN OUT` parameter, allowing the caller to receive or reuse the related transaction context.


**Signature**

```sql
procedure do_create_tranche(p_contract_no    varchar2,
                              p_tranche_type   varchar2 default null,
                              p_tracnhe_term   varchar2 default null,
                              p_tranche_amount number,
                              p_int_com_amount number default null,
                              p_trn_type       in varchar2,
                              p_tx_fid         in out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_contract_no` | IN | Supplied `p_contract_no` input (varchar2) |
| `p_tranche_type` | IN | Supplied `p_tranche_type` input (varchar2) |
| `p_tracnhe_term` | IN | Supplied `p_tracnhe_term` input (varchar2) |
| `p_tranche_amount` | IN | Supplied `p_tranche_amount` input (number) |
| `p_int_com_amount` | IN | Supplied `p_int_com_amount` input (number) |
| `p_trn_type` | IN | Transaction direction/type selector |
| `p_tx_fid` | IN OUT | Transaction identifier |

**Dependencies:** `LOAN_TRANCHE_API`, `APEX_KRD2204_0_API_P`, `KRD2204_0_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-limits-tranches-and-overdrafts)

---

<a id="create-overdrafts"></a>
#### create_overdrafts

**Type:** Procedure

**Description**

Processes overdraft creation or closure for selected customers and branch ranges. It retrieves the relevant customer, loan and card/account information within the supplied scope and performs the requested `CREATE_OVERDRAFT` or `CLOSE_LOAN` action. The procedure may create or update overdraft-related records and linked accounts as part of the selected operation.


**Signature**

```sql
procedure create_overdrafts(p_customer in number default null,
                              p_min_id   in number default null,
                              p_max_id   in number default null, 
                              p_oper_type in varchar2 default 'CREATE_OVERDRAFT');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_customer` | IN | Supplied `p_customer` input (number) |
| `p_min_id` | IN | Supplied `p_min_id` input (number) |
| `p_max_id` | IN | Supplied `p_max_id` input (number) |
| `p_oper_type` | IN | Operation type selector |

**Dependencies:** `GLBL_HESAP`, `API_VISTA` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-limits-tranches-and-overdrafts)

---

### Functions

<a id="get-new-ov-cont-no"></a>
#### get_new_ov_cont_no

**Type:** Function

**Description**

Obtains a new overdraft contract identifier for the specified branch, customer and account. It verifies matching customer and account information in the banking account and loan records before constructing or retrieving the identifier. Missing or inconsistent customer/account information is reported as an application error.


**Signature**

```sql
function get_new_ov_cont_no(p_branch_no in number,
                              p_cust_no   in number,
                              p_acc       in number) return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_no` | IN | Branch identifier |
| `p_cust_no` | IN | Supplied `p_cust_no` input (number) |
| `p_acc` | IN | Supplied `p_acc` input (number) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-limits-tranches-and-overdrafts)

---

<a id="domain-accounting-and-audit"></a>
## Accounting and audit

**Responsibility:** Creates accounting postings and retrieves accounting mappings and journal information.

[↑ API Index](#api-index)

### Procedures

<a id="table-log-operation"></a>
#### table_log_operation

**Type:** Procedure

**Description**

Creates or restores transaction-specific snapshots for supported loan tables. In `LOGGING` mode it records the relevant table data against the loan document and transaction identifier; in `RESTORING` mode it applies stored log information to restore data. Only the explicitly supported tables and the `LOGGING`/`RESTORING` modes are accepted.


**Signature**

```sql
procedure table_log_operation(p_doc_no     in varchar2,
                                p_table_name in varchar2,
                                p_tx_fid     in number,
                                p_oper_type  in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_table_name` | IN | Supplied `p_table_name` input (varchar2) |
| `p_tx_fid` | IN | Transaction identifier |
| `p_oper_type` | IN | Operation type selector |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

<a id="do-journal-entry"></a>
#### do_journal_entry

**Type:** Procedure

**Description**

Creates or posts debit and credit journal entries for the supplied transaction. It uses the provided entry collection, transaction caller and `CREATE` or `POST` mode to select the required accounting processing path. The transaction identifier is an `IN OUT` parameter, allowing the operation to associate generated or existing transaction data with the journal posting.


**Signature**

```sql
Procedure do_journal_entry(p_entries_list  g_entries_list_tab,
                             p_trn_caller_id in number,
                             p_tx_fid        in out number,
                             p_trn_type      in varchar2,
                             p_is_eod        in varchar2 default 'Y');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_entries_list` | IN | Supplied `p_entries_list` input (g_entries_list_tab) |
| `p_trn_caller_id` | IN | Supplied `p_trn_caller_id` input (number) |
| `p_tx_fid` | IN OUT | Transaction identifier |
| `p_trn_type` | IN | Transaction direction/type selector |
| `p_is_eod` | IN | Supplied `p_is_eod` input (varchar2) |

**Dependencies:** `TRN_BROKER`, `GLBL_FUNCTIONS`, `GLBL_HESAP`, `PA_API#`, `MM_ACCOUNTING#` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

<a id="create-journal-entries"></a>
#### create_journal_entries

**Type:** Procedure

**Description**

Constructs journal-entry data for a requested loan operation and transaction. It first resolves the operation definition from `LOAN_OPERATION_DESC`, then selects the associated voucher configuration and query rules from `LOAN_OPERATION_VOUCHER`. These settings determine the accounting fields, grouping and value expressions needed for the selected operation.

The voucher query results are collected and transformed into the debit/credit entry structure used by journal processing, including grouping where the configured result contains multiple rows. The procedure checks the required configuration rather than silently creating partial entries when a mapping is missing or ambiguous. The transaction type determines how the resulting journal operation is handled.

**Signature**

```sql
procedure create_journal_entries(p_oper_type in varchar2,
                                   p_tx_fid    in number,
                                   p_trn_type  in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation type selector |
| `p_tx_fid` | IN | Transaction identifier |
| `p_trn_type` | IN | Transaction direction/type selector |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

<a id="do-journal-control"></a>
#### do_journal_control

**Type:** Procedure

**Description**

Checks journal or account totals for the specified branch, customer and subaccount. It retrieves relevant account information from `CS_ACCOUNT` and validates the monetary components used in the journal-control calculation, including principal and interest totals. Negative amounts in the controlled components are reported instead of being treated as valid posting values.


**Signature**

```sql
procedure do_journal_control(p_branch_id   IN Number,
                               p_customer_no IN Number,
                               p_account_sub IN Number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_branch_id` | IN | Supplied `p_branch_id` input (Number) |
| `p_customer_no` | IN | Customer identifier |
| `p_account_sub` | IN | Supplied `p_account_sub` input (Number) |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

### Functions

<a id="get-trn-caller-id"></a>
#### get_trn_caller_id

**Type:** Function

**Description**

Retrieves the transaction caller identifier associated with the specified loan operation type from `LOAN_OPERATION_DESC`. The lookup matches the requested operation type and returns its configured caller value. An application error is reported if the operation has no matching configuration.


**Signature**

```sql
function get_trn_caller_id(p_oper_type in varchar2) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation type selector |

**Returns:** `number` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

<a id="get-voucher-data-from-query"></a>
#### get_voucher_data_from_query

**Type:** Function

**Description**

Executes the supplied SQL query and converts its result into structured voucher column data. The function supports `SINGLE_ROW` and `MULTI_ROW` result modes and, where grouping is required, expects a non-null `GROUP_CODE` column. The returned `G_COL_VALUE_TAB_MAP` preserves voucher column names and values for downstream journal processing.


**Signature**

```sql
function get_voucher_data_from_query(p_query       in varchar2,
                                       p_result_type in varchar2)
    return g_col_value_tab_map;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_query` | IN | Supplied `p_query` input (varchar2) |
| `p_result_type` | IN | Supplied `p_result_type` input (varchar2) |

**Returns:** `g_col_value_tab_map` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

<a id="get-gl-value"></a>
#### get_gl_value

**Type:** Function

**Description**

Returns the configured value for a named global loan parameter. The function looks up the requested parameter in `LOAN_GLOBAL_PARAMETERS_PR` and retrieves the matching stored setting. A missing or ambiguous parameter configuration is reported instead of returning an unrelated value.


**Signature**

```sql
function get_gl_value(p_parameter_name in varchar2) return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_parameter_name` | IN | Supplied `p_parameter_name` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-accounting-and-audit)

---

<a id="domain-history-metadata-and-reversal"></a>
## History, metadata and reversal

**Responsibility:** Captures transaction metadata and loan operation history, and supports reversal of recorded changes.

[↑ API Index](#api-index)

### Procedures

<a id="clear-addtional-data"></a>
#### clear_addtional_data

**Type:** Procedure

**Description**

Resets or changes the state of the package-level collection used for additional loan transaction data. The optional state parameter controls how the pending information is cleared or prepared for a subsequent operation. This routine manages in-memory update metadata rather than directly updating a repayment schedule.


**Signature**

```sql
procedure clear_addtional_data(p_state varchar2 default 'CLEAR');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_state` | IN | Supplied `p_state` input (varchar2) |

**Dependencies:** `RUHA3H` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="add-additional-group"></a>
#### add_additional_group

**Type:** Procedure

**Description**

Starts a named group within the additional-information structure used for loan transaction history. The optional tag identifies the group under which subsequent values are collected. It prepares metadata for later history or mapped-update processing rather than posting a financial transaction.


**Signature**

```sql
procedure add_additional_group(p_tag_name varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tag_name` | IN | Supplied `p_tag_name` input (varchar2) |

**Dependencies:** `RUHA3H` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="add-additional-info-1"></a>
#### add_additional_info

**Type:** Procedure

**Description**

Adds a string-valued tag to the pending transaction additional-information collection. The tag name and text value are retained for subsequent serialization, history recording or related mapped-update processing. This overload accepts `VARCHAR2` data and does not itself perform loan payment allocation.


**Signature**

```sql
procedure add_additional_info(p_tag_name varchar2, p_value varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tag_name` | IN | Supplied `p_tag_name` input (varchar2) |
| `p_value` | IN | Supplied `p_value` input (varchar2) |

**Dependencies:** `RUHA3H` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="add-additional-info-2"></a>
#### add_additional_info

**Type:** Procedure

**Description**

Adds a date-valued tag to the pending transaction additional-information collection. It records the tag and date using the package's date representation so they can be included with subsequent transaction history. This overload differs from the string-valued variant in the type and formatting of its value.


**Signature**

```sql
procedure add_additional_info(p_tag_name varchar2, p_value date);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tag_name` | IN | Supplied `p_tag_name` input (varchar2) |
| `p_value` | IN | Supplied `p_value` input (date) |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="add-column-update-1"></a>
#### add_column_update

**Type:** Procedure

**Description**

Registers a proposed change to a mapped loan field with its old and new string values. The routine resolves the tag against `LOAN_HISTORY_MAPPING` and, where applicable, tranche information, so the change can later be applied and reversed. It prepares update metadata rather than independently executing the full mapped-update transaction.


**Signature**

```sql
procedure add_column_update(p_tag_name   varchar2,
                              p_old_value  varchar2,
                              p_new_value  varchar2,
                              p_tranche_no number default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tag_name` | IN | Supplied `p_tag_name` input (varchar2) |
| `p_old_value` | IN | Supplied `p_old_value` input (varchar2) |
| `p_new_value` | IN | Supplied `p_new_value` input (varchar2) |
| `p_tranche_no` | IN | Tranche identifier |

**Dependencies:** `RUHA3H` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="add-column-update-2"></a>
#### add_column_update

**Type:** Procedure

**Description**

Registers an old and a new date value for a mapped loan field. It uses the same additional-update workflow as the string overload while preserving date semantics for later application and reversal. The optional tranche number identifies tranche-specific changes where relevant.


**Signature**

```sql
procedure add_column_update(p_tag_name   varchar2,
                              p_old_value  date,
                              p_new_value  date,
                              p_tranche_no number default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tag_name` | IN | Supplied `p_tag_name` input (varchar2) |
| `p_old_value` | IN | Supplied `p_old_value` input (date) |
| `p_new_value` | IN | Supplied `p_new_value` input (date) |
| `p_tranche_no` | IN | Tranche identifier |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="finalize-addtional-process"></a>
#### finalize_addtional_process

**Type:** Procedure

**Description**

Applies the pending mapped field changes collected for a specified loan document and offer identifier. The procedure resolves target fields from the stored mapping metadata and constructs the required updates, including tranche handling where applicable. The resulting transaction metadata is retained for history and possible reversal; dynamic updates require careful authorization and transaction review.


**Signature**

```sql
procedure finalize_addtional_process(p_doc_no   varchar2,
                                       p_offer_id varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document identifier |
| `p_offer_id` | IN | Supplied `p_offer_id` input (varchar2) |

**Dependencies:** `LOAN_TRANCHE_API` · [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="reverse-update"></a>
#### reverse_update

**Type:** Procedure

**Description**

Reverses mapped loan-field changes recorded under a specified transaction identifier. It loads the transaction's additional history information, resolves the corresponding table and column mappings and reconstructs previous values. The procedure applies those prior values through the mapped-update mechanism rather than assuming every change can be reversed by simply deleting a history row.


**Signature**

```sql
procedure reverse_update(p_tx_fid number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier |

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="create-loan-history"></a>
#### create_loan_history

**Type:** Procedure

**Description**

Records a loan business operation in transaction history with its document number, operation type, date, transaction ID and optional additional information. The procedure resolves the operation configuration and applies the requested history mode (`I`, `U` or `D`). It links the operation to its transaction and preserves related metadata for subsequent reporting or reversal.


**Signature**

```sql
procedure create_loan_history(p_oper_type VARCHAR2 default null,
                                p_doc_no    VARCHAR2 default null,
                                p_trn_date  DATE default glbl.getsysdate,
                                p_tx_fid    NUMBER,
                                p_trn_type  varchar2,
                                p_add_info  add_info_tab default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation type selector |
| `p_doc_no` | IN | Loan document identifier |
| `p_trn_date` | IN | Supplied `p_trn_date` input (DATE) |
| `p_tx_fid` | IN | Transaction identifier |
| `p_trn_type` | IN | Transaction direction/type selector |
| `p_add_info` | IN | Supplied `p_add_info` input (add_info_tab) |

**Dependencies:** `TRN_BROKER` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="update-loan-history"></a>
#### update_loan_history

**Type:** Procedure

**Description**

Updates an existing loan-history record using the supplied transaction identifier. It retrieves the corresponding operation history, accounting journal information, account details and operation description to refresh relevant recorded attributes. The transaction must exist and identify an unambiguous history record before the update can complete.


**Signature**

```sql
procedure update_loan_history(p_tx_fid number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier |

**Dependencies:** `LOAN_TRANCHE_API` · [Dependency register](dependencies.md).

**Errors:** [Verified business/error conditions](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

### Functions

<a id="get-additional-info"></a>
#### get_additional_info

**Type:** Function

**Description**

Retrieves a stored additional-information value from the loan operation history for a specified transaction and tag. It reads the transaction's recorded metadata and extracts the value associated with the requested tag name. The function returns the stored text value without changing the history record.


**Signature**

```sql
function get_additional_info(p_Tx_fid number, p_tag_name varchar2)
    return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Tx_fid` | IN | Transaction identifier |
| `p_tag_name` | IN | Supplied `p_tag_name` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

<a id="is-online-offer"></a>
#### is_online_offer

**Type:** Function

**Description**

Determines whether the specified memorandum corresponds to an online loan offer. It looks up the memorandum-related record in `KRD_SCOR_MAIN` and evaluates the stored offer classification. The result is an indicator used by callers to distinguish online offers from other offer types.


**Signature**

```sql
function is_online_offer(p_memorandum_id varchar2) return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_memorandum_id` | IN | Supplied `p_memorandum_id` input (varchar2) |

**Returns:** `varchar2` result; interpretation follows the documented purpose and source processing.

**Dependencies:** [Dependency register](dependencies.md).

**Errors:** No explicit business error identified; see [Error Handling](error-handling.md).

[↑ API Index](#api-index) · [↑ Domain](#domain-history-metadata-and-reversal)

---

