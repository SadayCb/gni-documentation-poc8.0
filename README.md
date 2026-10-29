# GNI Software — Technical Documentation

**Banking platform · Oracle PL/SQL · Module and package reference**

A central documentation portal for GNI Software's banking platform. Explore a business module, select a documented package, and navigate its public API, business workflows, data model, dependencies and error-handling reference.

## Modules

| Module | Description | Documentation |
|---|---|---|
| **System Administration** | Platform configuration and shared services | [Open module](docs/modules/system-administration.md) |
| **Customer Onboarding** | Customer registration and onboarding | [Open module](docs/modules/customer-onboarding.md) |
| **Loan Management** | Loans, tranches, categories and payments | [Open module](docs/modules/loan-management.md) |
| **Anti-Money Laundering (AML)** | Compliance and financial crime monitoring | [Open module](docs/modules/anti-money-laundering.md) |
| **Credit Scoring** | Credit risk assessment and scoring | [Open module](docs/modules/credit-scoring.md) |
| **Fraud Detection & Prevention** | Fraud controls and detection | [Open module](docs/modules/fraud-detection-prevention.md) |
| **Accounting & General Ledger** | Accounting and ledger processing | [Open module](docs/modules/accounting-general-ledger.md) |
| **Non-Performing Loans (NPL)** | Problem loan management | [Open module](docs/modules/non-performing-loans.md) |
| **Enterprise Resource Planning (ERP)** | Enterprise resources and operations | [Open module](docs/modules/enterprise-resource-planning.md) |
| **Payment Hub** | Payment operations and integrations | [Open module](docs/modules/payment-hub.md) |
| **Treasury Management** | Treasury and financial operations | [Open module](docs/modules/treasury-management.md) |
| **Fixed Assets Management** | Fixed asset lifecycle | [Open module](docs/modules/fixed-assets-management.md) |
| **Securities Management** | Securities processing | [Open module](docs/modules/securities-management.md) |
| **End-of-Day & Start-of-Day Processing** | Daily opening and closing operations | [Open module](docs/modules/eod-sod-processing.md) |

## Documentation structure

```text
GNI Software Documentation
└── Business / technical module
    └── Package
        ├── Overview
        ├── API Reference
        ├── Workflows
        ├── Data Model
        ├── Dependencies
        └── Error Handling
```

The **[Loan Management](docs/modules/loan-management.md)** module currently contains three documented packages: `GAIA.LOAN_API`, `GAIA.LOAN_TRANCHE_API`, and `GAIA.LOAN_CATEGORY_API`. Other module pages are prepared for future packages; their content is not presented as already documented.

[Documentation home](docs/index.md) · [Loan Management](docs/modules/loan-management.md)

---

*GNI Software · Source-backed technical documentation*
