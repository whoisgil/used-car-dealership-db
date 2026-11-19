# Used Car Dealership Database

A buy-here-pay-here (BHPH) used car dealership database, designed in IT 408: Data Design & Development (Walsh College, Summer 2026).

## What it models

- Customers and vehicles (VIN as the natural key)
- Sales, with down payment, trade-in allowance, and tax
- Financing as a supertype/subtype: each financed sale has one financing account, either in-house or through an external lender
- In-house payments, with delinquency tracking

## Schema

Eight tables in the `BHPH_DEALERSHIP` database: CUSTOMER, VEHICLE, SALE, FINANCING_ACCOUNT, INHOUSE_ACCOUNT, EXTERNAL_ACCOUNT, EXTERNAL_LENDER, PAYMENT. Keys, foreign keys, unique and check constraints are in `sql/deliverable-3.sql`, which also loads sample data.

## Queries

`docs/deliverable-4-query-alias-reference.md` has seven analytical queries: customer purchase totals, vehicle sales history, financing path per sale, financing subtype validation, lender funding totals, in-house payment summary, and a check that payments are in-house only.

## Running it

Run `sql/deliverable-3.sql` in MySQL to create the database, tables, and sample data.

## Files

- `sql/` — schema and sample data (Deliverable 3)
- `docs/` — Deliverables 1, 2, and 4 (design documentation and the query reference)
