# LOAN_TRANCHE_API — API Reference

[← Overview](loan-tranche-api.md) · [All packages](index.md)

**Inventory:** 27 public members (18 procedures, 9 functions), identified from supplied package specification. Body-only helpers are excluded. Each entry reproduces the public signature and summarizes its implementation.

## API index

Methods are grouped by their primary responsibility. Select a group or public method to jump directly to its documentation.

### Tranche lifecycle

Creation, execution and consolidation of tranche records.

- [`do_create_tranche`](#do-create-tranche)
- [`create_tranche`](#create-tranche)
- [`do_merge_tranche`](#do-merge-tranche)

### Payment schedules

Generation, conversion and calculation of tranche repayment schedules.

- [`do_payment_plan_creation`](#do-payment-plan-creation)
- [`do_payment_plan_conversion`](#do-payment-plan-conversion)
- [`get_plan_payment_amount`](#get-plan-payment-amount)
- [`get_Annuity_Amount`](#get-annuity-amount)

### Payments and allocation

Allocation of loan-account payments among tranches and operation execution.

- [`distribute_payments_between_tranches`](#distribute-payments-between-tranches)
- [`generate_allocated_payment_list`](#generate-allocated-payment-list)
- [`do_tranche_operation`](#do-tranche-operation)

### Interest and penalties

Interest calculations and overdue penalty calculation inputs.

- [`do_int_calculation`](#do-int-calculation)
- [`do_int_calculation_by_range`](#do-int-calculation-by-range)
- [`get_pen_info_for_calc`](#get-pen-info-for-calc)

### Overdue and transit

Overdue/transit classification, movement processing and corrective postings.

- [`get_ovd_trs_oper_type`](#get-ovd-trs-oper-type)
- [`do_overdue_transit_operation`](#do-overdue-transit-operation)
- [`ovd_amount_correction`](#ovd-amount-correction)

### Dates and loan lookup

Loan record retrieval and configurable payment/overdue dates.

- [`get_loan_info`](#get-loan-info)
- [`calculate_date`](#calculate-date)
- [`get_ovd_date`](#get-ovd-date)
- [`get_Pym_Startdate`](#get-pym-startdate)

### History and reversal

Tranche ledger history, logging, deletion and restoration.

- [`create_tranche_history`](#create-tranche-history)
- [`table_log_operation`](#table-log-operation)
- [`delete_tranche`](#delete-tranche)
- [`do_delete_operation`](#do-delete-operation)

### Integration and validation

APEX view integration and tranche plan consistency checks.

- [`apex_krd2204_2_view`](#apex-krd2204-2-view)
- [`get_plan_error`](#get-plan-error)
- [`do_plan_control`](#do-plan-control)

---

## Public API details

## Tranche lifecycle

Creation, execution and consolidation of tranche records.

<a id="do-create-tranche"></a>

### do_create_tranche**Procedure · Lifecycle.** Executes CREATE/DELETE tranche operation through KRD2204.

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

**Business logic.** CREATE checks contract/type/amount, resolves loan by contract, requires tranche flag, optionally derives interest commission using loan_api.get_amount_by_range for BY_RANGE loans. Stages APEX KRD2204 operation then validates and executes. DELETE locates transaction operation ID and removes staged operation before execution. Transaction ID is IN OUT.

**Data access.** `KRD_TEKLIF_DETAY`, `GLBL_TRANSACTION`

**Important calls.** `loan_api.get_amount_by_range`, `krd2204_0_api.justdoit`

**Validation and outcomes.** In Creation this parameteres cant be p_contract_no or p_tranche_type or p_tranche_amount null!; Does not find loan in [krd_teklif_detay] with contract_no [; Loan [with_tranche_info_yn] is [N], you can not do this operation!; Error encountered in search of tx_fid in glbl_transaction

**Source location:** supplied package body, lines 967–1057.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="create-tranche"></a>

### create_tranche**Procedure · Lifecycle.** Creates tranche and its repayment records.

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

**Business logic.** Loads KRD_ODEME_PLAN, tranche parameters and tranche type/term; checks configuration, creates base tranche values, generates schedule through loan_api.create_schedule and writes tranche master and payment-plan rows.

**Data access.** `KRD_ODEME_PLAN`, `LOAN_TRANCHE_PARAMETERS`, `LOAN_TRANCHE_INFO`, `LOAN_TRANCHE_PAYMENT_PLAN`, `ODEME_PLAN_TMP`, `LOAN_TRANCHE_OPERATION_HISTORY`

**Important calls.** `loan_api.get_acc_type_by_status`, `loan_api.create_schedule`, `loan_api.get_stop_ratio_info`

**Validation and outcomes.** Can not find info in [krd_odeme_plan] with doc_no [; Cannot find requirments in [loan_tranche_pr] with Combination no [; More than one tranche found for loan document number [; Unexpected error retrieving tranche for loan document number [

**Source location:** supplied package body, lines 3421–3973.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-merge-tranche"></a>

### do_merge_tranche**Procedure · Lifecycle.** Combines tranche balances and schedules.

**Signature**

```sql
procedure do_merge_tranche(p_doc_no varchar2 default null,
                             p_date   date default glbl.getsysdate);
```

**Business logic.** Uses loan master schedule and existing tranche data to validate merge scenario, consolidate amounts and adjust repayment schedules, including generated temporary plan and history; guards against ambiguous tranche states.

**Data access.** `LOAN_TRANCHE_INFO`, `KRD_ODEME_PLAN`, `LOAN_TRANCHE_PAYMENT_PLAN`, `ODEME_PLAN_TMP`

**Important calls.** `loan_api.get_acc_type_by_status`, `loan_api.create_schedule`, `loan_api.get_stop_ratio_info`, `loan_api.do_plan_control`, `loan_api.create_loan_history`, `loan_api.add_info_tab`, `loan_api.add_info_rec`

**Validation and outcomes.** Can not find info in [krd_odeme_plan] with doc_no [; More than one tranche found for loan document number [; Unexpected error retrieving tranche for loan document number [

**Source location:** supplied package body, lines 3124–3421.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## Payment schedules

Generation, conversion and calculation of tranche repayment schedules.

<a id="do-payment-plan-creation"></a>

### do_payment_plan_creation**Procedure · Payment schedules.** Creates payment plans for eligible tranches.

**Signature**

```sql
procedure do_payment_plan_creation(p_date   date default glbl.getsysdate,
                                     p_min_id number default 0,
                                     p_max_id number default 9999,
                                     p_doc_no varchar2 default null);
```

**Business logic.** Selects tranche-enabled loans in specified branch/date/document scope and iterates tranche info. Uses configured schedule creation type and temporary schedule rows to produce payment-plan entries, transaction metadata and history.

**Data access.** `KRD_TEKLIF_DETAY`, `LOAN_TRANCHE_INFO`, `LOAN_TRANCHE_PAYMENT_PLAN`, `ODEME_PLAN_TMP`

**Important calls.** `glbl_tarih.sonraki_isgunu`, `loan_api.get_trn_caller_id`, `trn_broker.register_eod`, `loan_tranche_api.calculate_date`, `loan_api.create_schedule`, `loan_api.get_stop_ratio_info`, `loan_api.get_acc_type_by_status`, `loan_api.finalize_addtional_process`, `loan_api.create_loan_history`, `loan_api.add_info_tab`

**Validation and outcomes.** schedule_creation_type [; Error Doc_no [

**Source location:** supplied package body, lines 3982–4215.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-payment-plan-conversion"></a>

### do_payment_plan_conversion**Procedure · Payment schedules.** Converts payment schedules to a configured type.

**Signature**

```sql
procedure do_payment_plan_conversion(p_date   date default glbl.getsysdate,
                                       p_doc_no varchar2 default null);
```

**Business logic.** Locates active loans requiring conversion, resolves pending product schedule mapping from PKRD_ODETIP_PR and LOAN_SCHEDULE_TYPES, regenerates schedule and updates tranche plan amounts/dates with logging and transaction context.

**Data access.** `KRD_TEKLIF_DETAY`, `PKRD_ODETIP_PR`, `LOAN_SCHEDULE_TYPES`, `LOAN_TRANCHE_INFO`, `LOAN_TRANCHE_PAYMENT_PLAN`, `ODEME_PLAN_TMP`, `LOAN_ACC_TYPES_PR`, `SET`

**Important calls.** `glbl_tarih.sonraki_isgunu`, `loan_api.get_trn_caller_id`, `trn_broker.register_eod`, `loan_api.add_column_update`, `loan_api.create_schedule`, `loan_api.get_stop_ratio_info`, `glbl_faiz.day_count_basis_new`, `loan_api.get_acc_type_by_status`, `loan_api.finalize_addtional_process`, `loan_api.create_loan_history`

**Validation and outcomes.** No schedule conversion data found for product code ; Multiple schedule conversion records found for product code ; An unexpected error occurred while fetching schedule conversion data for product code ; No loan schedule found for schedule number 

**Source location:** supplied package body, lines 4215–4472.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="get-plan-payment-amount"></a>

### get_plan_payment_amount**Function · Payments.** Returns outstanding scheduled amount for a loan tranche.

**Signature**

```sql
function get_plan_payment_amount(p_doc_no     in varchar2,
                                   p_tranche_no In number,
                                   p_date       in date default glbl.getsysdate,
                                   p_acc_type   in varchar2 default null)
    return number;
```

**Business logic.** Aggregates unpaid plan balance by document and tranche, with optional account type and date filtering; uses tranche payment-plan rows and account-type configuration.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN`, `LOAN_ACC_TYPES_PR`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 4472–4506.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="get-annuity-amount"></a>

### get_Annuity_Amount**Function · Payments.** Returns planned annuity for loan tranche.

**Signature**

```sql
Function get_Annuity_Amount(p_Doc_No      In Varchar2,
                              p_tranche_no  In number,
                              p_return_type In varchar2 default 'NO_ZERO')
    Return Number;
```

**Business logic.** Queries tranche payment-plan amounts and applies p_return_type selection (default NO_ZERO) when deriving numeric annuity.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 2832–2865.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## Payments and allocation

Allocation of loan-account payments among tranches and operation execution.

<a id="distribute-payments-between-tranches"></a>

### distribute_payments_between_tranches**Procedure · APEX/allocation.** Splits payment requests across tranche-specific accounts.

**Signature**

```sql
procedure distribute_payments_between_tranches(p_doc_no       in varchar2,
                                                 p_loan_status  in varchar2,
                                                 p_payment_list in loan_api.g_loan_accounts_tab,
                                                 p_final_list   out loan_api.g_loan_accounts_tab);
```

**Business logic.** For positive incoming payment lines combines unpaid plan entries with unpivoted tranche balances, filters payable account groups, then sorts by allocation order, plan date, tranche priority, tranche date and number. Caps allocations to planned remainder, sets future MAIN schedule-update flag and returns tranche-numbered rows.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN`, `GAIA.LOAN_TRANCHE_PAYMENT_PLAN`, `LOAN_TRANCHE_INFO`

**Important calls.** `loan_api.g_loan_accounts_tab`, `loan_api.get_acc_type_by_status`, `loan_api.g_loan_accounts_rec_type`

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 4506–4711.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="generate-allocated-payment-list"></a>

### generate_allocated_payment_list**Procedure · APEX/allocation.** Produces allocation records for one tranche.

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

**Business logic.** Uses requested amount, operation, loan status and tranche account list; delegates shared allocation ordering to loan_api.generate_allocated_payment_list and builds tranche-specific resulting account collection.

**Data access.** No direct table access confirmed in this method.

**Important calls.** `loan_api.generate_allocated_payment_list`, `loan_api.g_loan_accounts_tab`, `loan_api.g_loan_accounts_rec_type`

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 4711–4811.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-tranche-operation"></a>

### do_tranche_operation**Procedure · APEX/allocation.** Orchestrates allocate/payment operation on tranche.

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

**Business logic.** Accepts IN OUT account and tranche collections, dispatches ALLOCATE or PAYMENT processing, synchronizes tranche amounts and optional schedule-update parameters, reporting which schedules require update; validates operation type and total allocation.

**Data access.** `APEX_KRD2204_2_VIEW`, `LOAN_SCHEDULE_UPDATE_TYPES`, `LOAN_TRANCHE_INFO`

**Important calls.** `krd2204_0_api.init_global_variables`, `loan_api.get_acc_type_by_status`

**Validation and outcomes.** alocated trn amount for [; [p_opet_type] is wrong! Should be [ALLOCATE] [PAYMENT]!

**Source location:** supplied package body, lines 5335–5565.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## Interest and penalties

Interest calculations and overdue penalty calculation inputs.

<a id="do-int-calculation"></a>

### do_int_calculation**Procedure · Interest/penalties.** Calculates interest and penalty at tranche level.

**Signature**

```sql
procedure do_int_calculation(p_min_id     number default 0,
                               p_max_id     number default 999999999,
                               p_doc_no     in varchar2 default null,
                               p_tranche_no in number default null,
                               p_eod_sod    in varchar2,
                               p_date       in date default glbl.getsysdate);
```

**Business logic.** Filters eligible loans by date, EOD/SOD, branch, document and tranche; evaluates balances, payment dates, calculation flags, product rates, overdue statuses, stop intervals and penalties. Posts generated amounts and stores calculation/history results.

**Data access.** `LOAN_PAYMENT_DATES_LIST`, `KRD_TEKLIF_DETAY`, `LOAN_TRANCHE_PAYMENT_PLAN`, `LOAN_ACC_TYPES_PR`, `EOD_PROGRAMS_EXCEPTION`, `IBS.ONLINE_CRD_BLOCK`, `LOAN_TRANCHE_INFO`, `LOAN_STOP_RATIO_INFO`, `LOAN_TRANCHE_OPERATION_HISTORY`, `CS_ACCOUNT`

**Important calls.** `glbl_tarih.sonraki_isgunu`, `loan_api.g_entries_list_tab`, `glbl_tarih.onceki_isgunu`, `loan_api.get_trn_caller_id`, `loan_api.g_date_tab_type`, `loan_api.get_gl_value`, `loan_api.get_interest_calc_dates`, `loan_api.get_interest_calc_info`, `glbl_faiz.days_between2`, `loan_tranche_api.calculate_date`

**Validation and outcomes.** is_int_calc or is_pen_calc columns in krd_teklif_detay table is null! Please check...; The maximum penalty amount listed for this product ; Non calculated days of loan {; Too many rows found in loan_flow_ratio_info table, for date: 

**Source location:** supplied package body, lines 1777–2658.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-int-calculation-by-range"></a>

### do_int_calculation_by_range**Procedure · Interest/penalties.** Runs range-based interest calculation for eligible tranche loans.

**Signature**

```sql
Procedure do_int_calculation_by_range(p_min_id number default null,
                                        p_max_id number default null,
                                        p_doc_no in varchar2 default null,
                                        p_date   in date default glbl.getsysdate);
```

**Business logic.** Selects active tranche-enabled BY_RANGE loans not processed on date, matching document/branch range and having nonzero main exposure. Iterates tranche balances, accounts for stop periods, computes applicable interest, posts entries and updates history/calculation state.

**Data access.** `KRD_TEKLIF_DETAY`, `CS_ACCOUNT`, `LOAN_TRANCHE_INFO`, `LOAN_STOP_RATIO_INFO`, `LOAN_TRANCHE_PAYMENT_PLAN`

**Important calls.** `loan_api.g_entries_list_tab`, `loan_api.get_trn_caller_id`, `glbl_tarih.sonraki_isgunu`, `loan_api.get_gl_acc_from_matrix`, `loan_tranche_api.calculate_date`, `loan_api.get_amount_by_range`, `loan_tranche_api.create_tranche_history`, `loan_report_api.get_loan_message`, `loan_api.do_journal_entry`, `loan_api.add_column_update`

**Validation and outcomes.** Status of loan is WOFF, its not allowed operation, Doc_no{; Int amount cant be negative! {; Error encountered in loan_tranche_api.do_int_calculation_by_range operation for loan, doc_no: 

**Source location:** supplied package body, lines 1057–1299.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="get-pen-info-for-calc"></a>

### get_pen_info_for_calc**Procedure · Interest/penalties.** Returns overdue principal and penalty days.

**Signature**

```sql
procedure get_pen_info_for_calc(p_date                   in date default glbl.getsysdate,
                                  p_doc_no                 in varchar2,
                                  p_tranche_no             in number,
                                  p_ovd_amount             out number,
                                  p_calculated_pen_day_cnt out number);
```

**Business logic.** Derives overdue principal amount and overdue-day count from tranche plan records at processing date, returning both via OUT parameters and checking for negative results.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Overdue Main amount for penalty calculation or overdue days count is negative!

**Source location:** supplied package body, lines 1742–1777.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## Overdue and transit

Overdue/transit classification, movement processing and corrective postings.

<a id="get-ovd-trs-oper-type"></a>

### get_ovd_trs_oper_type**Function · Overdue/transit.** Classifies eligible payment balances as OVERDUE, TRANSIT or no movement.

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

**Business logic.** Rejects negative balances; adjusts grace threshold when written off or mature. Existing overdue can override configured grace period. Calls calculate_date to decide eligibility using plan date, maturity, account group, overdue amount, penalty-on-interest and grace-in-transit flags.

**Data access.** No direct table access confirmed in this method.

**Important calls.** `loan_tranche_api.calculate_date`

**Validation and outcomes.** Overdue amount {; Balance amount {

**Source location:** supplied package body, lines 12–104.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-overdue-transit-operation"></a>

### do_overdue_transit_operation**Procedure · Overdue/transit.** Applies overdue and transit transitions.

**Signature**

```sql
Procedure do_overdue_transit_operation(p_min_id     number default null,
                                         p_max_id     number default null,
                                         p_doc_no     in varchar2 default null,
                                         p_tranche_no in number default null,
                                         p_date       in date default glbl.getsysdate);
```

**Business logic.** Prepares MAIN and INT movement queues for selected branch/document/tranche. Reads transition decisions, account mappings and balances; adjusts tranche plan entries and creates journal/history movements for eligible items.

**Data access.** `LOAN_TRANCHE_OVERDUE_TRANSIT_QUEUE`, `CS_ACCOUNT`, `LOAN_OVERDUE_MAPPING`, `LOAN_TRANCHE_PAYMENT_PLAN`, `KRD_TEKLIF_DETAY`

**Important calls.** `loan_api.g_loan_accounts_tab`, `loan_api.g_entries_list_tab`, `loan_tranche_api.table_log_operation`, `loan_api.create_account`, `loan_api.get_acc_type_by_status`, `loan_api.get_trn_caller_id`, `loan_api.do_journal_entry`, `loan_api.finalize_addtional_process`, `loan_api.create_loan_history`, `loan_api.add_info_tab`

**Validation and outcomes.** Loan product code not found in pkrd_turkod_pr table!

**Source location:** supplied package body, lines 1299–1742.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="ovd-amount-correction"></a>

### ovd_amount_correction**Procedure · Overdue/transit.** Corrects regular versus overdue principal allocation.

**Signature**

```sql
procedure ovd_amount_correction(p_date          in date,
                                  p_doc_no        in varchar2,
                                  p_caller_tx_fid number);
```

**Business logic.** Filters active tranche loans with interest penalty enabled and not past maturity. Resolves pending schedule conversion if enabled, checks tranche cardinality, computes correction against overdue interest and temporary schedule amount, adjusts payment-plan rows, writes tranche operation history, generates debit/credit accounting entries and calls do_plan_control.

**Data access.** `KRD_TEKLIF_DETAY`, `PKRD_ODETIP_PR`, `LOAN_SCHEDULE_TYPES`, `LOAN_TRANCHE_INFO`, `LOAN_TRANCHE_PAYMENT_PLAN`, `ODEME_PLAN_TMP`, `CS_ACCOUNT`

**Important calls.** `loan_api.g_entries_list_tab`, `loan_api.get_trn_caller_id`, `loan_api.create_schedule`, `loan_api.get_stop_ratio_info`, `loan_tranche_api.create_tranche_history`, `loan_api.get_acc_type_by_status`, `loan_api.do_journal_entry`, `loan_api.create_loan_history`, `loan_api.add_info_tab`, `loan_api.add_info_rec`

**Validation and outcomes.** No schedule conversion data found for product code ; Multiple schedule conversion records found for product code ; An unexpected error occurred while fetching schedule conversion data for product code ; No loan schedule found for schedule number 

**Source location:** supplied package body, lines 311–703.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## Dates and loan lookup

Loan record retrieval and configurable payment/overdue dates.

<a id="get-loan-info"></a>

### get_loan_info**Function · Lookup/dates.** Reads loan master details.

**Signature**

```sql
function get_loan_info(p_doc_no in varchar2)
    return krd_teklif_detay%rowtype;
```

**Business logic.** Finds matching KRD_TEKLIF_DETAY row for document and returns %ROWTYPE; explicitly raises application error on lookup failure.

**Data access.** `KRD_TEKLIF_DETAY`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 2658–2676.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="calculate-date"></a>

### calculate_date**Function · Lookup/dates.** Computes configured overdue or payment date.

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

**Business logic.** Reads LOAN_TRANCHE_PERIODS rules; derives date using processing/plan date, grace period, loan maturity and payment day. p_configure_date determines final payment-day adjustment; unsupported types cause errors.

**Data access.** `IN_LIST`, `LOAN_TRANCHE_PERIODS`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** [p_configure_date]{; Can not find period type [; Something went wrong when execute [calculate_date] - {

**Source location:** supplied package body, lines 2865–2969.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="get-ovd-date"></a>

### get_ovd_date**Function · Lookup/dates.** Finds effective overdue date for account type.

**Signature**

```sql
Function get_ovd_date(p_doc_no   varchar2,
                        p_acc_type varchar2,
                        p_date     date default glbl.getsysdate) return date;
```

**Business logic.** Evaluates loan maturity, unpaid tranche plan dates and account balances for requested document/account type/date; returns applicable due/overdue boundary.

**Data access.** `KRD_TEKLIF_DETAY`, `LOAN_TRANCHE_PAYMENT_PLAN`, `CS_ACCOUNT`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 5626–5711.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="get-pym-startdate"></a>

### get_Pym_Startdate**Function · Lookup/dates.** Finds first or next repayment date for tranche.

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

**Business logic.** Uses plan end/current dates, payment day/frequency, account group, loan maturity and optional closed-plan check to determine the payment start date.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN`

**Important calls.** `glbl_faiz.days_between2`

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 5711–5815.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## History and reversal

Tranche ledger history, logging, deletion and restoration.

<a id="create-tranche-history"></a>

### create_tranche_history**Procedure · Audit/reversal.** Records tranche movement and applies its amount deltas.

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

**Business logic.** For I/U inserts operation history; D removes matching transaction/tranche history. Updates tranche balance buckets with the passed signed deltas; errors if tranche not found. Calls loan_api.loan_int_diff_operation for changed principal, transit principal and overdue principal calculation bases.

**Data access.** `GLBL_TRANSACTION`, `LOAN_TRANCHE_OPERATION_HISTORY`, `LOAN_TRANCHE_INFO`

**Important calls.** `loan_api.loan_int_diff_operation`

**Validation and outcomes.** Transaction with tx_fid: ; error encountered in search of tx_fid in glbl_transaction table!; Tranche_no: ; p_trn_type must be I-insert, U-update, D-delete!

**Source location:** supplied package body, lines 703–967.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="table-log-operation"></a>

### table_log_operation**Procedure · Audit/reversal.** Snapshots or restores tranche records.

**Signature**

```sql
procedure table_log_operation(p_doc_no     in varchar2,
                                p_tranche_no in varchar2,
                                p_table_name in varchar2,
                                p_tx_fid     in number,
                                p_oper_type  in varchar2);
```

**Business logic.** LOGGING saves tranche master or payment-plan rows into associated _LOG tables linked to transaction. RESTORING reconstructs prior data from logs for document/tranche/table; validates operation mode and transaction ID.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN_LOG`, `LOAN_TRANCHE_PAYMENT_PLAN`, `LOAN_TRANCHE_INFO_LOG`, `LOAN_TRANCHE_INFO`

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** when use [table_log_operation] , [p_tx_fid](; Oper type should be LOGGING-for logging or RESTORING-for restoring

**Source location:** supplied package body, lines 2969–3124.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="delete-tranche"></a>

### delete_tranche**Procedure · Audit/reversal.** Deletes transaction-associated tranche details.

**Signature**

```sql
procedure delete_tranche(p_tx_fid number);
```

**Business logic.** Short delete helper used within the recovery path; evaluate together with do_delete_operation and logging for full reversal effects.

**Data access.** No direct table access confirmed in this method.

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 3973–3982.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-delete-operation"></a>

### do_delete_operation**Procedure · Audit/reversal.** Reverses tranche operation by transaction ID.

**Signature**

```sql
procedure do_delete_operation(p_tx_fid number);
```

**Business logic.** Uses tranche master/plan snapshot logs and history rows to restore previous state and reverse transaction-linked movements; delegates deletion where appropriate.

**Data access.** `LOAN_TRANCHE_PAYMENT_PLAN_LOG`, `LOAN_TRANCHE_INFO_LOG`, `LOAN_TRANCHE_OPERATION_HISTORY`

**Important calls.** `loan_tranche_api.create_tranche_history`, `loan_tranche_api.delete_tranche`

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 5565–5626.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

## Integration and validation

APEX view integration and tranche plan consistency checks.

<a id="apex-krd2204-2-view"></a>

### apex_krd2204_2_view**Function · APEX/allocation.** Provides pipelined account/balance rows for APEX.

**Signature**

```sql
FUNCTION apex_krd2204_2_view RETURN loan_api.g_loan_accounts_tab
    PIPELINED;
```

**Business logic.** Projects tranche balances into loan_api.g_loan_accounts_tab records, mapping account types by status; output is pipelined so SQL clients can consume table rows.

**Data access.** `LOAN_TRANCHE_INFO`

**Important calls.** `loan_api.get_acc_type_by_status`, `loan_api.g_loan_accounts_rec_type`

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 2693–2765.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="get-plan-error"></a>

### get_plan_error**Function · Validation.** Returns payment-plan validation diagnostics.

**Signature**

```sql
function get_plan_error(p_doc_no     varchar2,
                          p_check_type varchar2 default 'ALL',
                          p_date       date default glbl.getsysdate)
    return varchar2;
```

**Business logic.** Checks loan, base schedule, tranche balances and scheduled amounts/dates, with interest differences and stop/flow rate intervals. Uses MAX_INT_DIFF_CORR_AMOUNT configuration and collects numbered messages instead of raising them directly.

**Data access.** No direct table access confirmed in this method.

**Important calls.** `loan_api.get_gl_value`

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 5815–5842.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

<a id="do-plan-control"></a>

### do_plan_control**Procedure · Validation.** Enforces payment-plan validity.

**Signature**

```sql
procedure do_plan_control(p_document_no in varchar2,
                            p_check_type  varchar2 default 'ALL',
                            p_date        date default glbl.getsysdate);
```

**Business logic.** Calls get_plan_error for the specified loan/check type/date and raises returned error text when nonempty.

**Data access.** No direct table access confirmed in this method.

**Important calls.** No cross-package call extracted.

**Validation and outcomes.** Refer to the implementation and calling transaction for failure handling; no explicit direct R-message extracted.

**Source location:** supplied package body, lines 6481–6494.

[↑ API index](#api-index) · [← Overview](loan-tranche-api.md)

