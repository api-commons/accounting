# Accounting

The common accounting vocabulary -- the chart of accounts, the journal entries posted to it, the vendors a business buys from and the bills they send, its expenses and its tax rates -- distilled from the APIs that already exist. Invoices belong to the billing context and payments to the payments context. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Accounting, Bookkeeping, ERP, Accounts Payable, General Ledger, AP Automation, Financial Management in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| ledger_account | 14 | 12 | type 100%, name 92%, number 92%, id 75%, currency 67%, description 67%, status 67%, balance 58% | accounts 9, chart-of-accounts 2, chartofaccounts 2, account 1 |
| journal_entry | 21 | 19 | description 74%, date 68%, id 63% | journals 9, journalentries 7, journal-entries 6, journal_entry 1 |
| vendor | 43 | 39 | id 74%, name 69%, status 62% | vendors 23, suppliers 17, vendor 3, supplier 2 |
| bill | 19 | 17 | due_date 94%, id 88%, description 76%, lines 76%, status 76%, vendor_id 71%, total 65%, currency 59% | bills 11, bill 3, payables 2, purchase_invoices 2 |
| expense | 14 | 12 | id 50% | expenses 12, expense-reports 2, purchases 1, expense 1 |
| tax_rate | 21 | 18 | code 72%, id 67%, rate 67%, name 61% | tax-rates 6, taxes 5, taxcodes 4, tax-codes 3 |

Cohort: 139 providers with harvested OpenAPI, 67 crosswalks. Of the providers exposing a ledger_account: 57% create, 100% list, 64% fetch, 29% update, 21% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A bill agrees almost completely: vendor, due date, lines, total, currency and status are all core. Accounts payable is the most standardised corner of accounting.
- A ledger account agrees on its number, name, type, currency and balance -- the chart of accounts is the same shape in every system that exposes it.
- A journal entry agrees only on id, date and description: the lines that carry the debits and credits are modelled differently everywhere.
- Expenses agree on nothing but an id (14 providers): expense tools, card platforms and ledgers each describe a spend differently.
- The largest accounting platforms are thin or absent: Intuit's harvested spec covers only invoices, customers and payments, and NetSuite, Wave and MYOB publish none the catalog holds.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Invoices belong to billing and payments to payments; accounting does not duplicate them. Its concepts are the ledger side.
- A ledger account is `/ledger-accounts`, though most providers say `/accounts`: that word is a bank account in banking and a login in identity. Bare `/accounts` counts only at a provider that also exposes journal entries.
- Vendors, bills, expenses and tax rates need no journal: inside an accounting cohort their names are unambiguous, and requiring journals cost most of the providers.
- `receipts` is not an expense: reviewed, it was goods received, cash receipts and receipt images as often as spend.
- Journal entries are `/journal-entries` (13 providers across spellings, against 9 for `journals`).
- Duplicates merged: Microsoft Dynamics and Navision into Dynamics 365, Codat, Ottimate.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
