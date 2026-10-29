# LOAN_CATEGORY_API — Workflows

[← Overview](loan-category-api.md) · [API Reference](loan-category-api-reference.md) · [Data Model](loan-category-data-model.md)

These diagrams summarize the business intent of **initially reviewed paths**. They are not a complete control-flow map of the long court-decision procedure.

## Configured category processing

```mermaid
flowchart TD
 A[run_loan_category] --> B[Read active procedure names]
 B --> C[Build dynamic PL/SQL call]
 C --> D[Execute configured category routine]
 D --> E{More configuration rows?}
 E -- Yes --> C
 E -- No --> F[Done]
```

## Court decision processing

```mermaid
flowchart TD
 A[Pending court decision] --> B[Resolve contract and loan accounts]
 B --> C[Read debt components and currency]
 C --> D{Decision and product conditions}
 D --> E[Compare decision debt with loan balances]
 E --> F[Check IBS and GNI payment history when applicable]
 F --> G[Calculate adjustments]
 G --> H[Prepare accounting operations]
 H --> I[Update processing records and relevant loan state]
```

## Charge-off and closing

```mermaid
flowchart TD
 A[Configured charge-off or closure rule] --> B[Select eligible loans]
 B --> C[Check balances and exceptions]
 C --> D{Eligible to process?}
 D -- No --> E[Skip or record outcome]
 D -- Yes --> F[Apply category, closing or accounting changes]
```

## Payment collection

```mermaid
flowchart TD
 A[Collection candidates] --> B[Check loan and customer account]
 B --> C[Apply payment and exception rules]
 C --> D{Available eligible amount?}
 D -- No --> E[No collection / diagnostic]
 D -- Yes --> F[Prepare accounting entry]
 F --> G[Execute collection and record result]
```
