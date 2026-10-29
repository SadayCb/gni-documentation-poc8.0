# LOAN_CATEGORY_API — API Reference

[← Overview](loan-category-api.md) · [Data Model](loan-category-data-model.md) · [Dependencies](loan-category-dependencies.md) · [Error Handling](loan-category-error-handling.md)

**Public declarations:** 14 (11 procedures and 3 functions). Descriptions reflect the supplied package body. The long court-decision routine is summarized by its major business branches; it is not a line-by-line accounting specification.

<a id="api-index"></a>
## API Index

### [Category orchestration and classification](#domain-category-orchestration-and-classification)

**Procedures**
- [run_loan_category](#run-loan-category)
- [set_new_category](#set-new-category)
- [set_charge_off_status](#set-charge-off-status)

### [Court decisions and accounting](#domain-court-decisions-and-accounting)

**Procedures**
- [apply_court_decision](#apply-court-decision)
- [inst_accounting_data](#inst-accounting-data)
- [fill_acc_tab](#fill-acc-tab)

### [Charge-off and loan closure](#domain-charge-off-and-loan-closure)

**Procedures**
- [do_close_operation](#do-close-operation)
- [do_close_loans_ex](#do-close-loans-ex)

### [Payment collection and legacy IBS](#domain-payment-collection-and-legacy-ibs)

**Procedures**
- [collect_payment](#collect-payment)
- [get_pymt_info_from_ibs](#get-pymt-info-from-ibs)

**Functions**
- [is_payed](#is-payed)
- [has_payment](#has-payment)

### [Overdraft and portfolio lookup](#domain-overdraft-and-portfolio-lookup)

**Procedures**
- [change_ov_status](#change-ov-status)

**Functions**
- [is_guaarantee](#is-guaarantee)

---

<a id="domain-category-orchestration-and-classification"></a>
## Category orchestration and classification

<a id="run-loan-category"></a>
#### run_loan_category

**Type:** Procedure

**Description**

Reads every row of `SP_LOAN_SUB_CATEGORY_PR` with a non-null `PROC_NAME`, constructs an anonymous PL/SQL block invoking `LOAN_CATEGORY_API.<configured name>`, and executes it with `EXECUTE IMMEDIATE`. Thus the enabled category jobs and their order come from configuration rather than a fixed list in this procedure. A malformed or unavailable configured routine can interrupt execution.

**Signature**

```sql
procedure run_loan_category;
```

**Parameters**

None.

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-category-orchestration-and-classification)

<a id="set-new-category"></a>
#### set_new_category

**Type:** Procedure

**Description**

Records a new category/subcategory for the specified loan in `LOAN_CATEGORY`, while end-dating the preceding category state using the supplied effective date (or logical system date). Transaction and user identifiers are carried into the category history when provided. This changes category history; it does not itself execute a payment or close the loan.

**Signature**

```sql
procedure set_new_category(p_doc_no in varchar2, p_cat_code in number, p_sub_cat_code in number, p_tx_fid in number default null, p_start_date in date default glbl.getsysdate, p_user_id in varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number; optionally narrows processing when defaulted to NULL. |
| `p_cat_code` | IN | New loan category code. |
| `p_sub_cat_code` | IN | New loan subcategory code. |
| `p_tx_fid` | IN | Transaction identifier used for history or accounting linkage. |
| `p_start_date` | IN | Effective category date or historical payment search start date, depending on the routine. |
| `p_user_id` | IN | User identifier associated with category change. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-category-orchestration-and-classification)

<a id="set-charge-off-status"></a>
#### set_charge_off_status

**Type:** Procedure

**Description**

Reads active rules in `LOAN_CHARGE_OFF_PARAMETERS`, optionally filtered by `p_param_code`, and excludes parameter code `1`. It executes each rule's configured selection SQL to identify eligible loans, optionally scoped to `p_doc_no`, then uses the charge-off worklist and loan/category state to apply configured status changes. Eligibility therefore depends on current configuration and the selected loans, not a single hard-coded status condition.

**Signature**

```sql
procedure set_charge_off_status(p_param_code in varchar2 default null, p_doc_no in varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_param_code` | IN | Configured rule/parameter code; NULL selects all applicable rules. |
| `p_doc_no` | IN | Loan document number; optionally narrows processing when defaulted to NULL. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-category-orchestration-and-classification)

---

<a id="domain-court-decisions-and-accounting"></a>
## Court decisions and accounting

<a id="apply-court-decision"></a>
#### apply_court_decision

**Type:** Procedure

**Description**

Processes `LOAN_COURT_DECISION_LIST` entries whose `PROCESS_STATUS = 'N'` and `TX_FID = p_txfid`, optionally restricted by contract. Each entry resolves `KRD_TEKLIF_DETAY`, `LOAN_ACCOUNTS`, payment-account data and currency conversion. The procedure compares principal, interest, penalty and legal-expense components with the declared total and records inconsistencies in `SS_KRD22060_0_LOG`. For the applicable decision branch it deducts payments made since the decision date using both legacy IBS totals and GNI `LOAN_OPERATION_HISTORY`, with the result floored at zero. It evaluates product-specific exceptions and current balance components, constructs debit/credit reclassification entries for eligible amounts, and uses shared loan/accounting services to apply the decision and record its outcome. The detailed posting path depends on decision type, product, loan status and the component being adjusted; it is not a single universal write-off rule.

**Signature**

```sql
procedure apply_court_decision(p_contract_no in varchar2, p_txfid in number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_contract_no` | IN | Court-decision contract number; optional filter of pending records. |
| `p_txfid` | IN | Transaction batch identifier matching pending court-decision rows. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-court-decisions-and-accounting)

<a id="inst-accounting-data"></a>
#### inst_accounting_data

**Type:** Procedure

**Description**

Inserts a debit/credit entry record into `LOAN_COURT_DECISION_ACCOUNTING_INFO`, keyed by the court-decision transaction and recording debit/credit account and subaccount, transaction amount, local-currency amount and description. This stages accounting details for subsequent retrieval; it does not itself post a GL journal.

**Signature**

```sql
procedure inst_accounting_data(p_tx_fid in number, p_dr_acc in number, p_dr_sub in number, p_cr_acc in number, p_cr_sub in number, p_amount in number, p_amount_lc in number default null, p_desc in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier used for history or accounting linkage. |
| `p_dr_acc` | IN | Debit account number. |
| `p_dr_sub` | IN | Debit subaccount number. |
| `p_cr_acc` | IN | Credit account number. |
| `p_cr_sub` | IN | Credit subaccount number. |
| `p_amount` | IN | Transaction-currency accounting amount. |
| `p_amount_lc` | IN | Local-currency accounting amount, if supplied. |
| `p_desc` | IN | Accounting-entry description. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-court-decisions-and-accounting)

<a id="fill-acc-tab"></a>
#### fill_acc_tab

**Type:** Procedure

**Description**

Selects saved court-decision accounting records by `p_tx_fid`, resolves loan/customer context as required, and maps each row into `LOAN_API.G_ENTRIES_LIST_REC`. Returns the assembled `LOAN_API.G_ENTRIES_LIST_TAB` via the OUT parameter for later journal posting.

**Signature**

```sql
procedure fill_acc_tab(p_tx_fid in number, p_acc_tab out loan_api.g_entries_list_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | Transaction identifier used for history or accounting linkage. |
| `p_acc_tab` | OUT | OUT: collection of loan journal entry records. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-court-decisions-and-accounting)

---

<a id="domain-charge-off-and-loan-closure"></a>
## Charge-off and loan closure

<a id="do-close-operation"></a>
#### do_close_operation

**Type:** Procedure

**Description**

Uses configured auto-closure parameters and candidate records in `LOAN_EOD_AUTO_CLOSE` to find loans eligible for closure. Inputs can restrict rule, document, branch range and execution mode. It reads loan and account balances, checks the rule-dependent conditions, builds required accounting entries and invokes shared loan-closing services. Eligibility and posting depend on the selected configured rule.

**Signature**

```sql
procedure do_close_operation(p_param_code in varchar2 default null, p_doc_no in varchar2 default null, p_min_id in number default null, p_max_id in number default null, p_run_type in varchar2 default 'ALL');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_param_code` | IN | Configured rule/parameter code; NULL selects all applicable rules. |
| `p_doc_no` | IN | Loan document number; optionally narrows processing when defaulted to NULL. |
| `p_min_id` | IN | Lower branch/range boundary. |
| `p_max_id` | IN | Upper branch/range boundary. |
| `p_run_type` | IN | Controls the auto-close run scope (default ALL). |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-charge-off-and-loan-closure)

<a id="do-close-loans-ex"></a>
#### do_close_loans_ex

**Type:** Procedure

**Description**

Processes the extended auto-close candidates using `LOAN_EOD_AUTO_CLOSE_PARAMETERS` and `LOAN_EOD_AUTO_CLOSE`, optionally for one loan. It checks loan attributes and collateral relationships (including `KRD_SCOR_TEMINAT_ALAKA_C`) before preparing closure entries and invoking shared loan operations. This is an additional closure path, not a simple alias of `do_close_operation`.

**Signature**

```sql
procedure do_close_loans_ex(p_doc_no in varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number; optionally narrows processing when defaulted to NULL. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-charge-off-and-loan-closure)

---

<a id="domain-payment-collection-and-legacy-ibs"></a>
## Payment collection and legacy IBS

<a id="collect-payment"></a>
#### collect_payment

**Type:** Procedure

**Description**

Builds payment-collection candidates from `LOAN_PAYMENT_COLLECT_TEMP_LIST`, loan schedules and loan/customer account data, optionally filtered by document and customer. It checks collection exceptions, guarantee relationships, account balances and applicable product/payment rules. For eligible amounts it prepares debit/credit posting entries and records collection processing through the relevant banking routines and tracking tables. An eligible loan does not necessarily produce a collection if no usable payer balance is available.

**Signature**

```sql
procedure collect_payment(p_doc_no in varchar2 default null, p_cust_no in number default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number; optionally narrows processing when defaulted to NULL. |
| `p_cust_no` | IN | Customer number to filter collection or close overdraft loans. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

<a id="is-payed"></a>
#### is_payed

**Type:** Function

**Description**

Calls `IBS.GET_CRD_PERC_ACCOUNTING_SUM` for the specified credit account and period from `p_begin_date` to the package's migration cutoff. Returns a PL/SQL Boolean according to whether the amount is nonzero. Because its result is `BOOLEAN`, callers must use PL/SQL rather than ordinary SQL projection.

**Signature**

```sql
function is_payed(p_acc in number, p_begin_date in date) return boolean;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_acc` | IN | IBS account identifier checked for payment. |
| `p_begin_date` | IN | Beginning of the IBS payment lookup interval. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

<a id="get-pymt-info-from-ibs"></a>
#### get_pymt_info_from_ibs

**Type:** Procedure

**Description**

Reads legacy IBS / DWMAIN accounting transactions for the contract from `p_start_date`, classifies relevant movements into principal, interest, penalty and legal-expense components, and returns four aggregated amounts through OUT parameters. Contract/account and historical-transaction mapping influence which entries are included.

**Signature**

```sql
procedure get_pymt_info_from_ibs(p_cont_no in varchar2, p_start_date in date, p_main_amnt out number, p_int_amnt out number, p_pen_amnt out number, p_legal_amnt out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_cont_no` | IN | Legacy credit contract number. |
| `p_start_date` | IN | Effective category date or historical payment search start date, depending on the routine. |
| `p_main_amnt` | OUT | OUT: total principal payments found in IBS. |
| `p_int_amnt` | OUT | OUT: total interest payments found in IBS. |
| `p_pen_amnt` | OUT | OUT: total penalty payments found in IBS. |
| `p_legal_amnt` | OUT | OUT: total legal-expense payments found in IBS. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

<a id="has-payment"></a>
#### has_payment

**Type:** Function

**Description**

Aggregates qualifying legacy IBS/DWMAIN payment history for the supplied contract over a lookback window beginning 36 months before the package's logical processing date. Returns a character indicator (`Y` when matching paid amount is found, otherwise `N`), rather than an amount.

**Signature**

```sql
function has_payment(p_cont_no in varchar2) return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_cont_no` | IN | Legacy credit contract number. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

---

<a id="domain-overdraft-and-portfolio-lookup"></a>
## Overdraft and portfolio lookup

<a id="change-ov-status"></a>
#### change_ov_status

**Type:** Procedure

**Description**

Rejects a null customer number with an application error; otherwise delegates the customer's overdraft-loan closing action to `GAIA.LOAN_API.CREATE_OVERDRAFTS` using `p_oper_type => 'CLOSE_LOAN'`. It does not directly implement the overdraft account update.

**Signature**

```sql
procedure change_ov_status(p_cust_no in number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_cust_no` | IN | Customer number to filter collection or close overdraft loans. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdraft-and-portfolio-lookup)

<a id="is-guaarantee"></a>
#### is_guaarantee

**Type:** Function

**Description**

Queries `LOAN_PORTFOLIO` by loan document number and maps `LOAN_TYPE = 'QN'` to `1`; all other matched types return `0`. The function uses a single-row lookup, so an absent or non-unique matching row follows the corresponding Oracle exception behavior.

**Signature**

```sql
function is_guaarantee(p_doc_no in varchar2) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | Loan document number; optionally narrows processing when defaulted to NULL. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdraft-and-portfolio-lookup)

