# LOAN_TRANCHE_API — Dependencies

[← Overview](loan-tranche-api.md)

| Service | Verified role |
|---|---|
| `GAIA.LOAN_API` | Account collection types, general payment allocation, schedule generation, journal entries, loan history, account status mapping, stop-rate helpers, interest difference operations |
| `KRD2204_0_API` | Tranche execution and validation, operation context and payment-plan alignment |
| `APEX_KRD2204_0_API_P`, `APEX_KRD2204_0_COLL_P` | Staging and deletion of tranche execution operations |
| `LOAN_REPORT_API` | Schedule lookup used by internal payment-plan update |
| `TRN_BROKER` | Transaction identifiers and registration |
| `GLBL`, `GLBL_TARIH`, `GLBL_FAIZ` | Processing date, next working day, day-count basis |

## Configuration inputs

- `PKRD_TURKOD_PR` — product minimum overdue thresholds and grace days.
- `LOAN_ACC_TYPES_PR` — normal/status account types, payment groups, payable and plan flags.
- `LOAN_TRANCHE_TYPES` — tranche order and payment sequence.
- `LOAN_TRANCHE_PERIODS` — period-code mapping for `calculate_date`.
- `LOAN_TRANCHE_PARAMETERS` — tranche creation constraints.
- `PKRD_ODETIP_PR` and `LOAN_SCHEDULE_TYPES` — conversion to a replacement payment schedule.
- `EOD_PROGRAMS_EXCEPTION` — skips exceptional accounts from certain batch jobs.

The public specification directly exposes `loan_api.g_loan_accounts_tab`. Therefore changes to shared account-record layout and account-type mappings can affect callers of both packages. This does not establish ownership of commits, rollbacks or journaling across integrated packages.