# salefisher-ops

Operational scripts, data pipelines, SQL queries and analytics models for SaleFisher.
Owned and maintained by Dennis — Data Engineer and Ops Sheet Master.

---

## What is this repo?

SaleFisher is a human-operated ecommerce middleware platform that lists products on Amazon and eBay on behalf of retailers and manages order relay, commission invoicing, and retailer compliance.

This repo contains everything on the data and operations layer. It does not contain the main application backend (Next.js) or frontend (React). Those live in separate repos.

---

## Repo Structure

- apps-script/Code.gs — All 5 Google Apps Script functions (J-2.9)
- dbt/salefisher_analytics/models/ — dbt SQL transformation models (Phase 4)
- queries/ — Ad-hoc SQL queries for analytics
- docs/column-mapping.md — DB field to Google Sheets column mapping reference
- README.md — This file

---

## apps-script/Code.gs

The single Apps Script file that runs inside the SaleFisher-OPS-MASTER Google Sheet.
All 5 functions live in one file. Do not split them into separate files — they share the CONFIG block.

### Functions

| Function | Trigger | What it does |
|---|---|---|
| CONFIG | none | Settings block. Edit your email, sheet names, column positions here. |
| onStrikeAdded() | On change | Counts strikes per retailer. Auto-suspends at 3. Emails ops alert. |
| onRetailerStatusChange() | On edit | Logs every status change via email. |
| generateInvoiceAlert() | Weekly Monday 8am | Scans FINANCE tab. Emails list of unpaid overdue invoices. |
| dailyHealthCheck() | Daily 8am | Pings the SaleFisher API /health endpoint. Emails alert if down. |
| calculateMonthlyCommission() | Monthly 1st midnight | BLOCKED — awaiting J-0.1 business rules decision. |

### How to install

1. Open the SaleFisher-OPS-MASTER Google Sheet
2. Click Extensions then Apps Script
3. Delete the default empty function in the editor
4. Copy the full contents of apps-script/Code.gs and paste it in
5. Edit the CONFIG block at the top — set your email address and API health URL
6. Save with Ctrl+S and name the project SaleFisher-Ops-Scripts
7. Set triggers per function — instructions are in the comments inside each function

### Permissions required

Google will ask for two permissions when setting triggers. Both are required.

- Gmail — needed to send alert emails
- Google Sheets — needed to read and write the ops spreadsheet

The unverified app warning is normal for personal scripts. Select all then click Allow.

---

## Google Sheets OPS-MASTER Tab Structure

| Tab | Purpose |
|---|---|
| RETAILERS | All retailers, status, strike count, channel connection status |
| PRODUCTS | All products listed across Amazon and eBay |
| ORDERS | All incoming orders, channel, relay status, commission amount |
| FINANCE | Monthly commission invoices and payment status |
| STRIKES | Retailer violation log |

---

## Tech Stack Data Layer

| Tool | Purpose | Cost |
|---|---|---|
| Google Sheets | Human-readable ops database | Free |
| Google Apps Script | Sheet automation and email alerts | Free |
| Neon PostgreSQL | Production database read access for analytics | Free up to 0.5 GB |
| pgAdmin 4 | Visual database browser and SQL query tool | Free |
| dbt Core | SQL transformation models | Free |
| Metabase | Ops analytics dashboards | Free cloud up to 5 users |
| Looker Studio | Retailer-facing performance reports | Free |

---

## Related Repos

| Repo | Owner | Contents |
|---|---|---|
| salefisher-api | Other Dev | Next.js 14 backend, Prisma ORM, BullMQ jobs |
| salefisher-web | Other Dev | React frontend, ops dashboard, retailer portal |
| salefisher-docs | Dennis | Blueprint HTML documents |
| salefisher-ops | Dennis | This repo |

---

## Current Job Status

| Job | Status | Notes |
|---|---|---|
| J-1.7 Google Sheets OPS-MASTER | In progress | Building 5-tab structure |
| J-2.9 Apps Script 4 of 5 functions | Ready to deploy | calculateMonthlyCommission blocked on J-0.1 |
| J-0.1 Business Rules | Pending | Commission rate, basis, refund clawback |
| dbt models | Not started | Phase 4 after real data exists |
| Metabase dashboards | Not started | Phase 4 after real data exists |

---

## Commit Convention

Start every commit with the job ID from SF-EMD-v0.5.

- [J-2.9] add onStrikeAdded function
- [J-2.9] fix CONFIG column positions for FINANCE tab
- [J-4.x] add monthly_commission dbt model
- [docs] update column mapping reference

---

SaleFisher · salefisher-ops · Dennis · Data Engineering
