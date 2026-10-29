# LOAN_CATEGORY_API — API Reference

[← Overview](loan-category-api.md) · [Data Model](loan-category-data-model.md) · [Dependencies](loan-category-dependencies.md) · [Error Handling](loan-category-error-handling.md)

**Public declarations:** 14 (11 procedures and 3 functions). This is the initial source-backed documentation pass; the complex court-decision and collection processes require deeper branch-level review.

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

Reads rows with non-null PROC_NAME from SP_LOAN_SUB_CATEGORY_PR and executes dynamically assembled calls to the named routines. The configuration controls which category processes are invoked.

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

Closes the current open category record for the loan/category code, then inserts the new category/subcategory record using the supplied transaction, start date and user context.

**Signature**

```sql
procedure set_new_category(p_doc_no in varchar2, p_cat_code in number, p_sub_cat_code in number, p_tx_fid in number default null, p_start_date in date default glbl.getsysdate, p_user_id in varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | varchar2 input/output for the described business operation. |
| `p_cat_code` | IN | number input/output for the described business operation. |
| `p_sub_cat_code` | IN | number input/output for the described business operation. |
| `p_tx_fid` | IN | number input/output for the described business operation. |
| `p_start_date` | IN | date input/output for the described business operation. |
| `p_user_id` | IN | varchar2 input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-category-orchestration-and-classification)

<a id="set-charge-off-status"></a>
#### set_charge_off_status

**Type:** Procedure

**Description**

Iterates active LOAN_CHARGE_OFF_PARAMETERS rules, excluding parameter code 1, executes their configured selection queries and updates the charge-off/category processing state for matching loans.

**Signature**

```sql
procedure set_charge_off_status(p_param_code in varchar2 default null, p_doc_no in varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_param_code` | IN | varchar2 input/output for the described business operation. |
| `p_doc_no` | IN | varchar2 input/output for the described business operation. |

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

Processes pending court-decision records filtered by contract and transaction ID. It resolves loan/account and currency details, compares declared debt to component amounts, adjusts debt for payments after the decision date from IBS and GNI history, and assembles/account-posts eligible reclassification entries. Its branches depend on decision type, product exceptions, debt components and loan balances.

**Signature**

```sql
procedure apply_court_decision(p_contract_no in varchar2, p_txfid in number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_contract_no` | IN | varchar2 input/output for the described business operation. |
| `p_txfid` | IN | number input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-court-decisions-and-accounting)

<a id="inst-accounting-data"></a>
#### inst_accounting_data

**Type:** Procedure

**Description**

Persists a debit/credit accounting entry proposal for a court-decision transaction in GAIA.LOAN_COURT_DECISION_ACCOUNTING_INFO, including account identifiers, amounts and description.

**Signature**

```sql
procedure inst_accounting_data(p_tx_fid in number, p_dr_acc in number, p_dr_sub in number, p_cr_acc in number, p_cr_sub in number, p_amount in number, p_amount_lc in number default null, p_desc in varchar2);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | number input/output for the described business operation. |
| `p_dr_acc` | IN | number input/output for the described business operation. |
| `p_dr_sub` | IN | number input/output for the described business operation. |
| `p_cr_acc` | IN | number input/output for the described business operation. |
| `p_cr_sub` | IN | number input/output for the described business operation. |
| `p_amount` | IN | number input/output for the described business operation. |
| `p_amount_lc` | IN | number input/output for the described business operation. |
| `p_desc` | IN | varchar2 input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-court-decisions-and-accounting)

<a id="fill-acc-tab"></a>
#### fill_acc_tab

**Type:** Procedure

**Description**

Reads saved court-decision accounting entries by transaction ID, resolves the loan's branch/customer context and returns LOAN_API entry records for downstream posting.

**Signature**

```sql
procedure fill_acc_tab(p_tx_fid in number, p_acc_tab out loan_api.g_entries_list_tab);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_tx_fid` | IN | number input/output for the described business operation. |
| `p_acc_tab` | OUT | loan_api.g_entries_list_tab input/output for the described business operation. |

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

Processes configured loan auto-closure rules, optionally bounded by parameter code, document, branch range and run type. It inspects loan/account balances and executes the configured closure/accounting path for eligible records.

**Signature**

```sql
procedure do_close_operation(p_param_code in varchar2 default null, p_doc_no in varchar2 default null, p_min_id in number default null, p_max_id in number default null, p_run_type in varchar2 default 'ALL');
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_param_code` | IN | varchar2 input/output for the described business operation. |
| `p_doc_no` | IN | varchar2 input/output for the described business operation. |
| `p_min_id` | IN | number input/output for the described business operation. |
| `p_max_id` | IN | number input/output for the described business operation. |
| `p_run_type` | IN | varchar2 input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-charge-off-and-loan-closure)

<a id="do-close-loans-ex"></a>
#### do_close_loans_ex

**Type:** Procedure

**Description**

Runs the extended auto-closure flow using active LOAN_EOD_AUTO_CLOSE_PARAMETERS rules, staged candidate rows and loan/collateral checks; optionally restricts the scope to one document.

**Signature**

```sql
procedure do_close_loans_ex(p_doc_no in varchar2 default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | varchar2 input/output for the described business operation. |

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

Collects payable loan amounts using a staged candidate list, configured payment-plan/account information, available customer accounts and exception rules. Builds posting entries for eligible collections and records processing results.

**Signature**

```sql
procedure collect_payment(p_doc_no in varchar2 default null, p_cust_no in number default null);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | varchar2 input/output for the described business operation. |
| `p_cust_no` | IN | number input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

<a id="is-payed"></a>
#### is_payed

**Type:** Function

**Description**

Calls IBS.GET_CRD_PERC_ACCOUNTING_SUM for the given credit account from the supplied begin date through the configured migration date. Returns TRUE when the amount is nonzero and FALSE otherwise.

**Signature**

```sql
function is_payed(p_acc in number, p_begin_date in date) return boolean;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_acc` | IN | number input/output for the described business operation. |
| `p_begin_date` | IN | date input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

<a id="get-pymt-info-from-ibs"></a>
#### get_pymt_info_from_ibs

**Type:** Procedure

**Description**

Aggregates legacy IBS accounting movements by debt component (MAIN, INT, PEN and legal expense) since the supplied start date for a credit contract and returns the four totals via OUT parameters.

**Signature**

```sql
procedure get_pymt_info_from_ibs(p_cont_no in varchar2, p_start_date in date, p_main_amnt out number, p_int_amnt out number, p_pen_amnt out number, p_legal_amnt out number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_cont_no` | IN | varchar2 input/output for the described business operation. |
| `p_start_date` | IN | date input/output for the described business operation. |
| `p_main_amnt` | OUT | number input/output for the described business operation. |
| `p_int_amnt` | OUT | number input/output for the described business operation. |
| `p_pen_amnt` | OUT | number input/output for the described business operation. |
| `p_legal_amnt` | OUT | number input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-payment-collection-and-legacy-ibs)

<a id="has-payment"></a>
#### has_payment

**Type:** Function

**Description**

Checks legacy IBS payment/accounting movements for a given contract within the historical lookback window (36 months from logical date) and returns a Y/N indicator based on the recorded aggregate.

**Signature**

```sql
function has_payment(p_cont_no in varchar2) return varchar2;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_cont_no` | IN | varchar2 input/output for the described business operation. |

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

Requires a customer number and delegates overdraft closure to LOAN_API.CREATE_OVERDRAFTS with operation CLOSE_LOAN.

**Signature**

```sql
procedure change_ov_status(p_cust_no in number);
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_cust_no` | IN | number input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdraft-and-portfolio-lookup)

<a id="is-guaarantee"></a>
#### is_guaarantee

**Type:** Function

**Description**

Looks up the loan in LOAN_PORTFOLIO by DOC_NO and returns 1 when LOAN_TYPE is QN, or 0 for other types. Missing or duplicate matches follow Oracle SELECT INTO behavior.

**Signature**

```sql
function is_guaarantee(p_doc_no in varchar2) return number;
```

**Parameters**

| Parameter | Mode | Meaning |
|---|---|---|
| `p_doc_no` | IN | varchar2 input/output for the described business operation. |

**Dependencies:** [Dependency register](loan-category-dependencies.md).

**Errors:** [Verified conditions](loan-category-error-handling.md).

[↑ API Index](#api-index) · [↑ Functional area](#domain-overdraft-and-portfolio-lookup)

