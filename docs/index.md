# GNI Software | Loan Packages

**Oracle PL/SQL · Loan module · Technical documentation**

> A centralized, source-backed technical reference for the loan service packages. This is a reviewable documentation proof of concept; package-level business behavior must be confirmed against the implementation.

## Overview

The loan module exposes reusable PL/SQL services to support banking loan processes. This documentation is organized by **package**, so developers can quickly find the relevant public interfaces, data model, dependencies, error handling, and workflows without mixing the responsibilities of different packages.

## Package directory

| Package | Purpose | Documentation |
|---|---|---|
| `GAIA.LOAN_API` | Shared loan business services, including operations, payments, schedules, interest, limits, accounting, and history. | **[Open LOAN_API →](loan-api.md)** |
| `LOAN_TRANCHE_API` | Tranche-related services; exact public interfaces and business rules will be documented from the package source. | **[Open LOAN_TRANCHE_API →](loan-tranche-api.md)** |
| `LOAN_CATEGORY_API` | Category-related services; exact public interfaces and business rules will be documented from the package source. | **[Open LOAN_CATEGORY_API →](loan-category-api.md)** |

## Documentation approach

Each package is documented individually using a consistent structure: **Overview, API Reference, Data Model, Dependencies, Error Handling, and Workflows**. SQL queries, cursor behavior, configuration lookups, and database effects are described according to the supplied code rather than inferred from method names.

**GNI Software — Loan Packages Documentation**
