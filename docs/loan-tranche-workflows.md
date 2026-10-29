# LOAN_TRANCHE_API — Workflows

[← Overview](loan-tranche-api.md)

## Tranche creation

```mermaid
flowchart TD
 A[do_create_tranche: CREATE] --> B{Required parameters?}
 B -- No --> X[Raise validation error]
 B -- Yes --> C[Find loan by contract]
 C --> D{Tranche enabled?}
 D -- No --> X
 D -- Yes --> E[Compute optional BY_RANGE commission]
 E --> F[Stage KRD2204 TRANCHE action]
 F --> G[Validate and execute via KRD2204]
 G --> H[Tranche master and plan creation]
```

`create_tranche` separately uses base loan schedule and configured tranche terms. `do_merge_tranche` merges existing tranche state and adjusts related plan records when eligible.

## Allocate payments

```mermaid
flowchart TD
 A[Incoming payment account list] --> B[distribute_payments_between_tranches]
 B --> C[Select due plan amounts and unscheduled tranche balances]
 C --> D[Filter payable and account types]
 D --> E[Sort by date, tranche and payment priority]
 E --> F[Allocate up to remaining eligible amount]
 F --> G[Mark future MAIN schedule updates]
 G --> H[Return tranche-numbered account list]
 H --> I[do_tranche_operation: PAYMENT/ALLOCATE]
```

The selection combines scheduled payment plan rows with an UNPIVOT view of tranche balances and orders by dates plus configured tranche priorities; it is more than an equal-proportion split.

## Overdue / transit and interest

```mermaid
flowchart TD
 A[Selected date and loan scope] --> B[Prepare MAIN and INT overdue queue]
 B --> C[get_ovd_trs_oper_type]
 C --> D{Overdue, transit or neither}
 D --> E[do_overdue_transit_operation]
 E --> F[Adjust plans, balances and accounting]
 A --> G[do_int_calculation or BY_RANGE path]
 G --> H[Evaluate stop intervals, rate and penalty configuration]
 H --> I[Post amounts and record history]
```

## Reversal and validation

```mermaid
flowchart TD
 A[Transaction] --> B[table_log_operation: LOGGING]
 B --> C[Modify tranche and payment plan]
 C --> D[create_tranche_history]
 D --> E[do_plan_control]
 F[do_delete_operation] --> G[Read _LOG snapshots and history]
 G --> H[Restore or reverse affected tranche state]
```

These diagrams express code-observed relationships rather than a guaranteed global sequence or transaction boundary.