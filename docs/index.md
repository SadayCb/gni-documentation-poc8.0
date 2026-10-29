# GNI Software | LOAN_API Technical Documentation

**Enterprise loan services · Oracle PL/SQL · Source-based technical reference**

> A structured reference for developers who maintain, integrate, troubleshoot and extend the shared `GAIA.LOAN_API` package. This documentation is a **reviewable proof of concept**, not an approved operational runbook.

---

## Overview

`GAIA.LOAN_API` is a shared banking service package that exposes procedures and functions for loan operations, accounts, payment allocation, repayment plans, interest and penalty calculations, overdue handling, credit limits, accounting journals, and transaction history.

It is not a single-screen controller. Multiple banking operations and supporting processes can use its public interfaces. The **Package Specification** defines the public contract; the **Package Body** is the primary source for verified business behavior and database effects.

### Package at a glance

| Item | Description |
|---|---|
| Package | `GAIA.LOAN_API` |
| Technology | Oracle Database / PL/SQL |
| Public API | **89 declarations:** 45 procedures and 44 functions (overloads counted separately) |
| Scope | Shared loan business operations and supporting services |
| Source of truth | Supplied Package Specification and Package Body |
| Audience | GNI Software developers, reviewers, and customer technical teams |
| Documentation status | Draft / Proof of Concept — further business-rule review required |

## Documentation guide

| Document | Why open it |
|---|---|
| [Workflows](docs/workflows.md) | Understand the package's major capabilities through high-level visual diagrams |
| [Data Model](docs/data-model.md) | Review important loan tables, verified access types, and method relationships |
| [Dependencies](docs/dependencies.md) | Find relevant packages, integration calls and configuration dependencies |
| [API Reference](docs/api.md) | Find public methods, exact signatures, parameters, business logic, rules and errors |
| [Error Handling](docs/error-handling.md) | Understand selected application validation, configuration and processing failures |

## High-level architecture

```mermaid
flowchart TD
  A[APEX modules and other callers] --> API[GAIA.LOAN_API]
  B[EOD / SOD processing] --> API
  API --> C[Loan operations and accounts]
  API --> D[Payments and schedules]
  API --> E[Interest and overdue processing]
  API --> F[Accounting and history]
  C --> DB[(Loan and banking data)]
  D --> DB
  E --> DB
  F --> DB
```

*Conceptual capability map only — not a literal order of execution or a complete call graph.*

The [Workflows page](docs/workflows.md) shows the earlier broad, cross-cutting diagrams for account creation, EOD/SOD processing and history/reversal.


## Functional map

The API is organized into **nine navigation domains**. These categories organize the reference; they are not separate independent modules.

| Functional domain | Business responsibility |
|---|---|
| [Loan operations and controls](docs/api.md#domain-loan-operations-and-controls) | Coordinates loan operations, validation and control activities |
| [Accounts, guarantees and closure](docs/api.md#domain-accounts-guarantees-and-closure) | Supports loan-linked accounts, collateral or guarantee activities and closure |
| [Payments and balances](docs/api.md#domain-payments-and-balances) | Handles payment allocation, due amounts and account balances |
| [Payment schedules and dates](docs/api.md#domain-payment-schedules-and-dates) | Generates or adjusts schedules, payment dates and grace-period information |
| [Interest, penalties and rates](docs/api.md#domain-interest-penalties-and-rates) | Supplies rate information and interest/penalty calculations or operations |
| [Delinquency and status](docs/api.md#domain-delinquency-and-status) | Evaluates overdue dates and supports delinquency transitions |
| [Limits, tranches and overdrafts](docs/api.md#domain-limits-tranches-and-overdrafts) | Evaluates credit limits and handles tranche and overdraft operations |
| [Accounting and audit](docs/api.md#domain-accounting-and-audit) | Resolves accounting mappings and creates or checks journal postings |
| [History, metadata and reversal](docs/api.md#domain-history-metadata-and-reversal) | Records operation metadata and history and supports reversing mapped changes |


## Scope and review notes

- Public API inventory is derived from active declarations in the supplied specification; commented-out declarations are excluded.
- Business explanations are based on the provided implementation, but **not every method has completed manual semantic review**.
- Workflow diagrams are intentionally general and should not be mistaken for scheduling requirements.
- Before customer publication, validate business rules, sensitive implementation details and transaction boundaries with the responsible maintainers.

---

**GNI Software — LOAN_API Documentation** · [Browse the full API](docs/api.md) · [Return to documentation home](docs/index.md)
