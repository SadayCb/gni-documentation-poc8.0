# LOAN_TRANCHE_API — API Reference

[← Overview](loan-tranche-api.md) · [Workflows](loan-tranche-workflows.md) · [Data Model](loan-tranche-data-model.md) · [Dependencies](loan-tranche-dependencies.md) · [Error Handling](loan-tranche-error-handling.md)

**Public API:** 27 declarations (18 procedures and 9 functions). Method signatures are retained from the supplied specification. Only public members are listed.

<a id="api-index"></a>
## API Index

### [Tranche lifecycle](#domain-tranche-lifecycle)

Creates, executes, and consolidates loan tranches.

**Procedures**
- [do_create_tranche](#do-create-tranche)
- [create_tranche](#create-tranche)
- [do_merge_tranche](#do-merge-tranche)

### [Payment schedules](#domain-payment-schedules)

Creates, converts, and evaluates tranche-level repayment plans.

**Procedures**
- [do_payment_plan_creation](#do-payment-plan-creation)
- [do_payment_plan_conversion](#do-payment-plan-conversion)

**Functions**
- [get_plan_payment_amount](#get-plan-payment-amount)
- [get_Annuity_Amount](#get-annuity-amount)

### [Payments and allocation](#domain-payments-and-allocation)

Allocates incoming funds and executes tranche-specific payments.

**Procedures**
- [distribute_payments_between_tranches](#distribute-payments-between-tranches)
- [generate_allocated_payment_list](#generate-allocated-payment-list)
- [do_tranche_operation](#do-tranche-operation)

### [Interest and penalties](#domain-interest-and-penalties)

Calculates interest and derives penalty calculation inputs.

**Procedures**
- [do_int_calculation](#do-int-calculation)
- [do_int_calculation_by_range](#do-int-calculation-by-range)
- [get_pen_info_for_calc](#get-pen-info-for-calc)

### [Overdue and transit](#domain-overdue-and-transit)

Classifies overdue positions and applies overdue/transit corrections.

**Procedures**
- [do_overdue_transit_operation](#do-overdue-transit-operation)
- [ovd_amount_correction](#ovd-amount-correction)

**Functions**
- [get_ovd_trs_oper_type](#get-ovd-trs-oper-type)

### [Dates and loan lookup](#domain-dates-and-loan-lookup)

Retrieves loans and calculates processing, payment, and overdue dates.

**Functions**
- [get_loan_info](#get-loan-info)
- [calculate_date](#calculate-date)
- [get_ovd_date](#get-ovd-date)
- [get_Pym_Startdate](#get-pym-startdate)

### [History and reversal](#domain-history-and-reversal)

Records tranche movements and restores transaction-related state.

**Procedures**
- [create_tranche_history](#create-tranche-history)
- [table_log_operation](#table-log-operation)
- [delete_tranche](#delete-tranche)
- [do_delete_operation](#do-delete-operation)

### [Integration and validation](#domain-integration-and-validation)

Exposes APEX data and validates tranche repayment-plan consistency.

**Procedures**
- [do_plan_control](#do-plan-control)

**Functions**
- [apex_krd2204_2_view](#apex-krd2204-2-view)
- [get_plan_error](#get-plan-error)

---

<a id="domain-tranche-lifecycle"></a>
## Tranche lifecycle

Creates, executes, and consolidates loan tranches.

<a id="do-create-tranche"></a>
#### do_create_tranche

**Type:** Procedure

**Description**

In CREATE mode, looks up `KRD_TEKLIF_DETAY` by `MUKAVELE_NO`, requires tranche enablement and mandatory inputs, and resolves a missing interest commission via `LOAN_API.get_amount_by_range` for `BY_RANGE` loans. Stages a `TRANCHE` operation through `APEX_KRD2204_0_API_P`, calls KRD2204 validation, then execution. DELETE mode retrieves `TRN_ID` from `GLBL_TRANSACTION` by transaction FID and deletes the staged operation before running KRD2204 execution.

**Signature**

```sql
procedure do_create_tranche(p_contract_no    varchar2 default null,
                              p_tranche_type   varchar2 default null,
                              p_tracnhe_term   varchar2 default null,
                              p_tranche_amount number default null,
                              p_int_com_amount number default null,
                              p_trn_type       in varchar2,
                              p_tx_fid         in out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_contract_no` | IN | Loan contract number used to locate the loan. |
| `p_tranche_type` | IN | Tranche type supplied for creation. |
| `p_tracnhe_term` | IN | Tranche term supplied for creation (spelling follows package specification). |
| `p_tranche_amount` | IN | Requested tranche amount. |
| `p_int_com_amount` | IN | Interest commission amount; may be derived for BY_RANGE loans. |
| `p_trn_type` | IN | History change mode (I/U/D) or requested operation mode. |
| `p_tx_fid` | IN OUT | Transaction identifier supplied by the caller or generated and returned to it. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 967–1057.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-tranche-lifecycle)

<a id="create-tranche"></a>
#### create_tranche

**Type:** Procedure

**Description**

Creates tranche and its repayment records. Loads KRD_ODEME_PLAN, tranche parameters and tranche type/term; checks configuration, creates base tranche values, generates schedule through loan_api.create_schedule and writes tranche master and payment-plan rows.

**Signature**

```sql
procedure create_tranche(p_doc_no         in varchar2,
                           p_tranche_type   in varchar2,
                           p_tracnhe_term   in varchar2,
                           p_tranche_amount in number,
                           p_int_amount     in number,
                           p_tx_fid         in number,
                           p_date           in date);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_type` | IN | Tranche type supplied for creation. |
| `p_tracnhe_term` | IN | Tranche term supplied for creation (spelling follows package specification). |
| `p_tranche_amount` | IN | Requested tranche amount. |
| `p_int_amount` | IN | Signed change to regular interest balance. |
| `p_tx_fid` | IN | Transaction identifier used to associate the operation with its transaction history. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 3421–3973.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-tranche-lifecycle)

<a id="do-merge-tranche"></a>
#### do_merge_tranche

**Type:** Procedure

**Description**

Selects active loan records and unmerged tranche rows (`MAIN_TRANCHE_NO IS NULL`) whose `MERGE_DATE` is on or before the requested date and whose combined principal, transit, interest, overdue, accrued and penalty balances are positive. For each selected row it allocates a transaction identifier, retrieves the base repayment schedule in `KRD_ODEME_PLAN`, and looks for the existing main tranche (`TRANCHE_NO = MAIN_TRANCHE_NO`); multiple matches are rejected. It then processes the merge against the tranche's principal and plan information, recording the required changes. It is eligibility-driven rather than a blanket merge of all tranches for a loan.

**Signature**

```sql
procedure do_merge_tranche(p_doc_no varchar2 default null,
                             p_date   date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 3124–3421.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-tranche-lifecycle)

---

<a id="domain-payment-schedules"></a>
## Payment schedules

Creates, converts, and evaluates tranche-level repayment plans.

<a id="do-payment-plan-creation"></a>
#### do_payment_plan_creation

**Type:** Procedure

**Description**

Creates payment plans for eligible tranches. Selects tranche-enabled loans in specified branch/date/document scope and iterates tranche info. Uses configured schedule creation type and temporary schedule rows to produce payment-plan entries, transaction metadata and history.

**Signature**

```sql
procedure do_payment_plan_creation(p_date   date default glbl.getsysdate,
                                     p_min_id number default 0,
                                     p_max_id number default 9999,
                                     p_doc_no varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective processing date (default: logical system date where declared). |
| `p_min_id` | IN | Lower branch/processing range boundary. |
| `p_max_id` | IN | Upper branch/processing range boundary. |
| `p_doc_no` | IN | Loan document number identifying the target loan. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 3982–4215.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-schedules)

<a id="do-payment-plan-conversion"></a>
#### do_payment_plan_conversion

**Type:** Procedure

**Description**

Processes active, tranche-enabled loans marked for schedule conversion where `SCHEDULE_CONVERSION_DATE` is no later than the computed next-working-day boundary. It joins the loan master to its base repayment plan and product settings, registers an EOD `PLAN_CONVERSION` transaction, and requires a unique `PENDING` conversion mapping in `PKRD_ODETIP_PR`. It retrieves the target `LOAN_SCHEDULE_TYPES` record and applies the converted schedule and corresponding tranche plan changes. Missing or duplicate mappings are reported explicitly.

**Signature**

```sql
procedure do_payment_plan_conversion(p_date   date default glbl.getsysdate,
                                       p_doc_no varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective processing date (default: logical system date where declared). |
| `p_doc_no` | IN | Loan document number identifying the target loan. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 4215–4472.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-schedules)

<a id="get-plan-payment-amount"></a>
#### get_plan_payment_amount

**Type:** Function

**Description**

Returns the outstanding due amount for one loan or tranche as of the requested date. The query joins `LOAN_TRANCHE_PAYMENT_PLAN` to `LOAN_ACC_TYPES_PR` and includes only account types configured with `HAS_PAYMENT_PLAN_YN = 'Y'`, open plan rows (`IS_CLOSED = 'N'`) and `PLAN_DATE <= p_date`. Tranche and account-type inputs are optional filters. It sums `NVL(PAYMENT_AMOUNT,0) - NVL(PAID_AMOUNT,0)` and returns `GREATEST(0, amount)`, so a negative aggregate is returned as zero.

**Signature**

```sql
function get_plan_payment_amount(p_doc_no     in varchar2,
                                   p_tranche_no In number,
                                   p_date       in date default glbl.getsysdate,
                                   p_acc_type   in varchar2 default null)
    return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |
| `p_acc_type` | IN | Account type filter or target account classification. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 4472–4506.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-schedules)

<a id="get-annuity-amount"></a>
#### get_Annuity_Amount

**Type:** Function

**Description**

Finds the first **future** payment-plan date for the requested loan/tranche, considering only open schedule entries whose configured account group is MAIN or INT and which have payment-plan support enabled. With `NO_ZERO` (the default), the date search requires a positive scheduled amount; `WITH_ZERO` also allows zero amounts. It then sums the MAIN and INT `PAYMENT_AMOUNT` values on that selected date and returns zero if no matching entries exist. This is the first qualifying installment, not an average across all installments.

**Signature**

```sql
Function get_Annuity_Amount(p_Doc_No      In Varchar2,
                              p_tranche_no  In number,
                              p_return_type In varchar2 default 'NO_ZERO')
    Return Number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_Doc_No` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_return_type` | IN | Controls annuity result selection (default NO_ZERO). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 2832–2865.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-schedules)

---

<a id="domain-payments-and-allocation"></a>
## Payments and allocation

Allocates incoming funds and executes tranche-specific payments.

<a id="distribute-payments-between-tranches"></a>
#### distribute_payments_between_tranches

**Type:** Procedure

**Description**

Combines unpaid payable payment-plan rows with tranche balance components and account-type settings. It orders candidate amounts by configured allocation order, due date and tranche/payment priorities, then assigns positive requested payments subject to each candidate's remaining amount. The output associates allocations with tranche numbers and marks future MAIN payments for schedule updates.

**Signature**

```sql
procedure distribute_payments_between_tranches(p_doc_no       in varchar2,
                                                 p_loan_status  in varchar2,
                                                 p_payment_list in loan_api.g_loan_accounts_tab,
                                                 p_final_list   out loan_api.g_loan_accounts_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_loan_status` | IN | Loan status governing processing rules. |
| `p_payment_list` | IN | Payment account collection input. |
| `p_final_list` | OUT | Output collection of tranche-assigned payment rows. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 4506–4711.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payments-and-allocation)

<a id="generate-allocated-payment-list"></a>
#### generate_allocated_payment_list

**Type:** Procedure

**Description**

Produces allocation records for one tranche. Uses requested amount, operation, loan status and tranche account list; delegates shared allocation ordering to loan_api.generate_allocated_payment_list and builds tranche-specific resulting account collection.

**Signature**

```sql
procedure generate_allocated_payment_list(p_doc_no        in varchar2,
                                            p_tranche_no    in number,
                                            p_oper_type     in varchar2,
                                            p_loan_status   in varchar2,
                                            p_pym_amount    in number,
                                            p_loan_accounts in loan_api.g_loan_accounts_tab,
                                            p_payment_list  out loan_api.g_loan_accounts_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_oper_type` | IN | Operation category or operation identifier. |
| `p_loan_status` | IN | Loan status governing processing rules. |
| `p_pym_amount` | IN | Payment amount to allocate. |
| `p_loan_accounts` | IN | Loan-account records used or updated by processing. |
| `p_payment_list` | OUT | Payment account collection input. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 4711–4811.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payments-and-allocation)

<a id="do-tranche-operation"></a>
#### do_tranche_operation

**Type:** Procedure

**Description**

Orchestrates allocate/payment operation on tranche. Accepts IN OUT account and tranche collections, dispatches ALLOCATE or PAYMENT processing, synchronizes tranche amounts and optional schedule-update parameters, reporting which schedules require update; validates operation type and total allocation.

**Signature**

```sql
procedure do_tranche_operation(p_oper_type           in varchar2,
                                 p_tx_fid              in number,
                                 p_doc_no              in varchar2,
                                 p_tranche_no          in number,
                                 p_trn_amount          in number,
                                 p_schedule_upd_type   in out number,
                                 p_has_schedule_update out varchar2,
                                 p_loan_accounts       in out loan_api.g_loan_accounts_tab,
                                 p_loan_tranche        in out g_loan_tranche_tab,
                                 p_process             in varchar2 default 'ALLOCATE',
                                 p_schedule_parametrs  create_schedule_parametrs%rowtype default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_oper_type` | IN | Operation category or operation identifier. |
| `p_tx_fid` | IN | Transaction identifier used to associate the operation with its transaction history. |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_trn_amount` | IN | Operation amount. |
| `p_schedule_upd_type` | IN OUT | Schedule update type, which may be modified. |
| `p_has_schedule_update` | OUT | Outputs whether a schedule update is required. |
| `p_loan_accounts` | IN OUT | Loan-account records used or updated by processing. |
| `p_loan_tranche` | IN OUT | Tranche collection updated by the operation. |
| `p_process` | IN | Processing stage (default ALLOCATE). |
| `p_schedule_parametrs` | IN | Schedule generation settings passed to the tranche operation; optional CREATE_SCHEDULE_PARAMETRS row. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 5335–5565.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-payments-and-allocation)

---

<a id="domain-interest-and-penalties"></a>
## Interest and penalties

Calculates interest and derives penalty calculation inputs.

<a id="do-int-calculation"></a>
#### do_int_calculation

**Type:** Procedure

**Description**

Calculates interest and penalty at tranche level. Filters eligible loans by date, EOD/SOD, branch, document and tranche; evaluates balances, payment dates, calculation flags, product rates, overdue statuses, stop intervals and penalties. Posts generated amounts and stores calculation/history results.

**Signature**

```sql
procedure do_int_calculation(p_min_id     number default 0,
                               p_max_id     number default 999999999,
                               p_doc_no     in varchar2 default null,
                               p_tranche_no in number default null,
                               p_eod_sod    in varchar2,
                               p_date       in date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Lower branch/processing range boundary. |
| `p_max_id` | IN | Upper branch/processing range boundary. |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_eod_sod` | IN | End/start-of-day processing indicator. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 1777–2658.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-interest-and-penalties)

<a id="do-int-calculation-by-range"></a>
#### do_int_calculation_by_range

**Type:** Procedure

**Description**

Its loan cursor joins `KRD_TEKLIF_DETAY` and `KRD_TESIS`, selects active tranche-enabled loans with `INT_CALC_METHOD = 'BY_RANGE'`, excludes rows already processed for the effective date, applies document/branch filters, and requires nonzero MAIN/OVD_MAIN exposure in `CS_ACCOUNT` joined to `LOAN_ACC_TYPES_PR`. It then iterates tranche calculation records and produces the corresponding interest/accounting movements.

**Signature**

```sql
Procedure do_int_calculation_by_range(p_min_id number default null,
                                        p_max_id number default null,
                                        p_doc_no in varchar2 default null,
                                        p_date   in date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Lower branch/processing range boundary. |
| `p_max_id` | IN | Upper branch/processing range boundary. |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 1057–1299.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-interest-and-penalties)

<a id="get-pen-info-for-calc"></a>
#### get_pen_info_for_calc

**Type:** Procedure

**Description**

Returns overdue principal and penalty days. Derives overdue principal amount and overdue-day count from tranche plan records at processing date, returning both via OUT parameters and checking for negative results.

**Signature**

```sql
procedure get_pen_info_for_calc(p_date                   in date default glbl.getsysdate,
                                  p_doc_no                 in varchar2,
                                  p_tranche_no             in number,
                                  p_ovd_amount             out number,
                                  p_calculated_pen_day_cnt out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective processing date (default: logical system date where declared). |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_ovd_amount` | OUT | Returns overdue amount used for penalty calculation. |
| `p_calculated_pen_day_cnt` | OUT | Returns calculated penalty day count. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 1742–1777.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-interest-and-penalties)

---

<a id="domain-overdue-and-transit"></a>
## Overdue and transit

Classifies overdue positions and applies overdue/transit corrections.

<a id="get-ovd-trs-oper-type"></a>
#### get_ovd_trs_oper_type

**Type:** Function

**Description**

First rejects negative current and overdue balances. For `WOFF`, it removes the threshold and uses the payment day as the period. At or after maturity the threshold becomes zero; an existing overdue position also replaces the configured period with the payment day. `calculate_date` determines the effective date. The subsequent CASE returns `OVERDUE` only for eligible non-overdue account groups whose due date has passed and amount exceeds the threshold; otherwise qualifying account groups can return `TRANSIT` according to the grace-in-transit flag, calculated date, and balance threshold; it may return NULL.

**Signature**

```sql
function get_ovd_trs_oper_type(p_date               in date,
                                 p_acc_type_group     in varchar2,
                                 p_plan_date          in date,
                                 p_balance            in number,
                                 p_overdue_min_amount in number,
                                 p_ovd_balance        in number,
                                 p_loan_end_date      in date,
                                 p_grace_end_date     in date,
                                 p_pymt_day           in number,
                                 p_overdue_type       in varchar2,
                                 p_loan_status        in varchar2,
                                 p_is_pen_calc_on_int in varchar2, 
                                 p_is_grace_applied_in_trs_acc in varchar2)
    return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective processing date (default: logical system date where declared). |
| `p_acc_type_group` | IN | Logical account group for overdue/transit classification. |
| `p_plan_date` | IN | Payment plan due date. |
| `p_balance` | IN | Current amount being classified. |
| `p_overdue_min_amount` | IN | Minimum overdue threshold. |
| `p_ovd_balance` | IN | Existing overdue amount. |
| `p_loan_end_date` | IN | Loan maturity date. |
| `p_grace_end_date` | IN | End of grace period. |
| `p_pymt_day` | IN | Configured payment day. |
| `p_overdue_type` | IN | Configured overdue date period type. |
| `p_loan_status` | IN | Loan status governing processing rules. |
| `p_is_pen_calc_on_int` | IN | Penalty-on-interest configuration flag. |
| `p_is_grace_applied_in_trs_acc` | IN | Grace-in-transit-account configuration flag. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 12–104.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdue-and-transit)

<a id="do-overdue-transit-operation"></a>
#### do_overdue_transit_operation

**Type:** Procedure

**Description**

Applies overdue and transit transitions. Prepares MAIN and INT movement queues for selected branch/document/tranche. Reads transition decisions, account mappings and balances; adjusts tranche plan entries and creates journal/history movements for eligible items.

**Signature**

```sql
Procedure do_overdue_transit_operation(p_min_id     number default null,
                                         p_max_id     number default null,
                                         p_doc_no     in varchar2 default null,
                                         p_tranche_no in number default null,
                                         p_date       in date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_min_id` | IN | Lower branch/processing range boundary. |
| `p_max_id` | IN | Upper branch/processing range boundary. |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 1299–1742.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdue-and-transit)

<a id="ovd-amount-correction"></a>
#### ovd_amount_correction

**Type:** Procedure

**Description**

Processes active tranche-enabled loans with `IS_PEN_CALC_ON_INT = 'Y'` and maturity on or after the calculation date. For enabled conversions it resolves a unique `PENDING` product schedule, creates schedule output, and derives the overdue principal cap. It compares overdue principal against overdue interest/cap, logs pre-change tranche and plan state, moves amounts between MAIN and OVD_MAIN, allocates overdue plan amounts by outstanding overdue-interest dates, posts debit/credit entries via `LOAN_API`, records history, then calls `do_plan_control`.

**Signature**

```sql
procedure ovd_amount_correction(p_date          in date,
                                  p_doc_no        in varchar2,
                                  p_caller_tx_fid number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_date` | IN | Effective processing date (default: logical system date where declared). |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_caller_tx_fid` | IN | Originating transaction identifier for audit linkage. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 311–703.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdue-and-transit)

---

<a id="domain-dates-and-loan-lookup"></a>
## Dates and loan lookup

Retrieves loans and calculates processing, payment, and overdue dates.

<a id="get-loan-info"></a>
#### get_loan_info

**Type:** Function

**Description**

Reads loan master details. Finds matching KRD_TEKLIF_DETAY row for document and returns %ROWTYPE; explicitly raises application error on lookup failure.

**Signature**

```sql
function get_loan_info(p_doc_no in varchar2)
    return krd_teklif_detay%rowtype;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 2658–2676.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-dates-and-loan-lookup)

<a id="calculate-date"></a>
#### calculate_date

**Type:** Function

**Description**

Computes configured overdue or payment date. Reads LOAN_TRANCHE_PERIODS rules; derives date using processing/plan date, grace period, loan maturity and payment day. p_configure_date determines final payment-day adjustment; unsupported types cause errors.

**Signature**

```sql
function calculate_date(p_period_type    varchar2,
                          p_trn_date       date,
                          p_grace_end_date date,
                          p_loan_end_date  date,
                          p_pymt_day       number,
                          p_configure_date varchar2 default 'PYMT_DATE')
    return date;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_period_type` | IN | Configured rule for calculating the date. |
| `p_trn_date` | IN | Operation transaction date. |
| `p_grace_end_date` | IN | End of grace period. |
| `p_loan_end_date` | IN | Loan maturity date. |
| `p_pymt_day` | IN | Configured payment day. |
| `p_configure_date` | IN | Date normalization mode (default PYMT_DATE). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 2865–2969.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-dates-and-loan-lookup)

<a id="get-ovd-date"></a>
#### get_ovd_date

**Type:** Function

**Description**

Looks up the loan's maturity date and product-level overdue minimum amounts in `KRD_TEKLIF_DETAY` and `PKRD_TURKOD_PR`. It chooses the principal or interest threshold from `p_acc_type`, then examines open, nonzero overdue payment-plan amounts joined to tranche configuration and account groups `OVD_MAIN`/`OVD_INT` through the requested date. Each plan date is transformed using `calculate_date` with the tranche overdue-period settings. The query uses a cumulative window sum to identify the applicable earliest overdue date relative to the threshold, rather than merely returning the minimum raw payment-plan date.

**Signature**

```sql
Function get_ovd_date(p_doc_no   varchar2,
                        p_acc_type varchar2,
                        p_date     date default glbl.getsysdate) return date;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_acc_type` | IN | Account type filter or target account classification. |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 5626–5711.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-dates-and-loan-lookup)

<a id="get-pym-startdate"></a>
#### get_Pym_Startdate

**Type:** Function

**Description**

Finds first or next repayment date for tranche. Uses plan end/current dates, payment day/frequency, account group, loan maturity and optional closed-plan check to determine the payment start date.

**Signature**

```sql
Function get_Pym_Startdate(p_Doc_No          In Varchar2,
                             p_tranche_no      In number,
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
| `p_Doc_No` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_pymt_date` | IN | Reference payment date used to calculate the next payment start date. |
| `p_pymt_day` | IN | Configured payment day. |
| `p_pymt_period` | IN | Payment periodicity used for the next repayment date. |
| `p_acc_type` | IN | Account type filter or target account classification. |
| `p_loan_end_Date` | IN | Loan maturity date. |
| `p_check_is_closed` | IN | Whether closed payment-plan rows are considered (default Y). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 5711–5815.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-dates-and-loan-lookup)

---

<a id="domain-history-and-reversal"></a>
## History and reversal

Records tranche movements and restores transaction-related state.

<a id="create-tranche-history"></a>
#### create_tranche_history

**Type:** Procedure

**Description**

For `I`/`U`, inserts a row in `LOAN_TRANCHE_OPERATION_HISTORY`; for `D`, deletes matching transaction/tranche history. It then applies signed amount changes to the related `LOAN_TRANCHE_INFO` balance buckets (including MAIN, transit, INT, overdue and penalty buckets). If a principal base changes it opens/closes corresponding interest-difference tracking through `LOAN_API.loan_int_diff_operation`. Missing tranche number and unsupported transaction modes are rejected.

**Signature**

```sql
procedure create_tranche_history(p_trn_date             in date,
                                   p_tx_fid               in number,
                                   p_oper_type            in varchar2,
                                   p_doc_no               in varchar2,
                                   p_tranche_no           in number,
                                   p_trn_type             in varchar2,
                                   p_eod_sod              in varchar2 default null,
                                   p_main_amount          in number default 0,
                                   p_main_trs_amount      in number default 0,
                                   p_int_amount           in number default 0,
                                   p_int_trs_amount       in number default 0,
                                   p_ovd_main_amount      in number default 0,
                                   p_ovd_int_amount       in number default 0,
                                   p_ovd_calc_int_amount  in number default 0,
                                   p_trs_calc_int_amount  in number default 0,
                                   p_pen_main_amount      in number default 0,
                                   p_int_virt_amount      in number default 0,
                                   p_LP_MAIN_AMOUNT       in number default 0,
                                   p_pen_main_amount_real in number default 0,
                                   p_null_calc_access     in varchar2 default 'N');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_trn_date` | IN | Operation transaction date. |
| `p_tx_fid` | IN | Transaction identifier used to associate the operation with its transaction history. |
| `p_oper_type` | IN | Operation category or operation identifier. |
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_trn_type` | IN | History change mode (I/U/D) or requested operation mode. |
| `p_eod_sod` | IN | End/start-of-day processing indicator. |
| `p_main_amount` | IN | Signed change to regular principal balance. |
| `p_main_trs_amount` | IN | Signed change to principal in transit. |
| `p_int_amount` | IN | Signed change to regular interest balance. |
| `p_int_trs_amount` | IN | Signed change to interest in transit. |
| `p_ovd_main_amount` | IN | Signed change to overdue principal. |
| `p_ovd_int_amount` | IN | Signed change to overdue interest. |
| `p_ovd_calc_int_amount` | IN | Signed change to accrued overdue interest. |
| `p_trs_calc_int_amount` | IN | Signed change to accrued transit interest. |
| `p_pen_main_amount` | IN | Signed change to principal penalty balance. |
| `p_int_virt_amount` | IN | Signed change to virtual interest amount. |
| `p_LP_MAIN_AMOUNT` | IN | Legacy LP principal amount argument; not written to the shown history INSERT or tranche balance UPDATE. |
| `p_pen_main_amount_real` | IN | Actual penalty amount recorded in operation history; not added to the shown tranche balance UPDATE. |
| `p_null_calc_access` | IN | If Y, shifts a non-null last calculation access date to the previous logical system day. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 703–967.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-history-and-reversal)

<a id="table-log-operation"></a>
#### table_log_operation

**Type:** Procedure

**Description**

Snapshots or restores tranche records. LOGGING saves tranche master or payment-plan rows into associated _LOG tables linked to transaction. RESTORING reconstructs prior data from logs for document/tranche/table; validates operation mode and transaction ID.

**Signature**

```sql
procedure table_log_operation(p_doc_no     in varchar2,
                                p_tranche_no in varchar2,
                                p_table_name in varchar2,
                                p_tx_fid     in number,
                                p_oper_type  in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_tranche_no` | IN | Tranche identifier to select or update. |
| `p_table_name` | IN | Tranche table selected for logging or restoring. |
| `p_tx_fid` | IN | Transaction identifier used to associate the operation with its transaction history. |
| `p_oper_type` | IN | Operation category or operation identifier. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 2969–3124.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-history-and-reversal)

<a id="delete-tranche"></a>
#### delete_tranche

**Type:** Procedure

**Description**

Deletes transaction-associated tranche details. Short delete helper used within the recovery path; evaluate together with do_delete_operation and logging for full reversal effects.

**Signature**

```sql
procedure delete_tranche(p_tx_fid number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier used to associate the operation with its transaction history. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 3973–3982.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-history-and-reversal)

<a id="do-delete-operation"></a>
#### do_delete_operation

**Type:** Procedure

**Description**

Reverses tranche operation by transaction ID. Uses tranche master/plan snapshot logs and history rows to restore previous state and reverse transaction-linked movements; delegates deletion where appropriate.

**Signature**

```sql
procedure do_delete_operation(p_tx_fid number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier used to associate the operation with its transaction history. |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 5565–5626.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-history-and-reversal)

---

<a id="domain-integration-and-validation"></a>
## Integration and validation

Exposes APEX data and validates tranche repayment-plan consistency.

<a id="apex-krd2204-2-view"></a>
#### apex_krd2204_2_view

**Type:** Function

**Description**

Provides pipelined account/balance rows for APEX. Projects tranche balances into loan_api.g_loan_accounts_tab records, mapping account types by status; output is pipelined so SQL clients can consume table rows.

**Signature**

```sql
FUNCTION apex_krd2204_2_view RETURN loan_api.g_loan_accounts_tab
    PIPELINED;
```

**Parameters**

No parameters.

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 2693–2765.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-integration-and-validation)

<a id="get-plan-error"></a>
#### get_plan_error

**Type:** Function

**Description**

Examines tranche plan consistency and accumulates diagnostic text, rather than changing the schedule. It reads loan and schedule details, compares tranche amounts with plan balances, uses configurable `MAX_INT_DIFF_CORR_AMOUNT` and stop/flow rate intervals for interest comparisons, and returns the combined errors when checks fail. Its local `add_error` logic stops appending after reaching its error-count threshold.

**Signature**

```sql
function get_plan_error(p_doc_no     varchar2,
                          p_check_type varchar2 default 'ALL',
                          p_date       date default glbl.getsysdate)
    return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number identifying the target loan. |
| `p_check_type` | IN | Controls the scope of repayment-plan validation (default ALL). |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 5815–6481.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-integration-and-validation)

<a id="do-plan-control"></a>
#### do_plan_control

**Type:** Procedure

**Description**

Validates a tranche-enabled loan's repayment-plan consistency by calling `get_plan_error(p_document_no, p_check_type, p_date)`. If that function returns diagnostic text, it raises an application error via `R`; otherwise it exits without altering the schedule. The tranche version does **not** perform the `LOAN_EOD_CONTROL_EXP` exclusion lookup used in the separate `LOAN_API.do_plan_control` implementation.

**Signature**

```sql
procedure do_plan_control(p_document_no in varchar2,
                            p_check_type  varchar2 default 'ALL',
                            p_date        date default glbl.getsysdate);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_document_no` | IN | Loan document identifier to validate. |
| `p_check_type` | IN | Controls the scope of repayment-plan validation (default ALL). |
| `p_date` | IN | Effective processing date (default: logical system date where declared). |

**Dependencies:** [Dependency register](loan-tranche-dependencies.md).

**Errors:** [Verified business/error conditions](loan-tranche-error-handling.md).

*Source: supplied package body, lines 6481–6494.*

[↑ API Index](#api-index) · [↑ Functional area](#domain-integration-and-validation)

