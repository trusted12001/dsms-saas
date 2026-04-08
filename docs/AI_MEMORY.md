# DSMS AI Memory

This file contains the essential context for AI sessions working on DSMS.

Any AI assistant working on this repository must assume the following project context.

---

## Project Identity

Project:
DSMS – Driving School Management System

Type:
Multi-tenant SaaS platform

Primary Purpose:
To manage the real-time operations and transactions of driving schools.

---

## Business Context

Each driving school is a tenant on the platform.

The system is intended to help schools manage:

- students
- instructors
- lesson bookings
- schedules
- payments
- sales
- accounting
- reports
- business oversight

A major goal is real-time visibility into business activities as they happen.

---

## Technology Context

Framework:
Laravel 12

Database:
MySQL

Frontend:
Blade templates

Architecture style:
Laravel monolith with multi-tenant SaaS structure

Recommended permissions approach:
Spatie Laravel Permission

---

## Workspace Model

DSMS has two main workspaces:

### Super Admin Workspace

Used by the platform owner.

### Tenant Workspace

Used by each driving school.

Tenant-side roles include:

- Business Owner
- Manager
- Sales
- Accounts

---

## Core Architecture Rules

1. Every tenant-owned operational record must belong to a tenant.
2. All queries must respect tenant boundaries.
3. Cross-tenant access must never happen.
4. Super Admin has platform-wide visibility.
5. Tenant users must only access their own tenant data.
6. Financial data must be accurate and traceable.
7. Real-time transaction records must be reliable.

---

## Core Modules Expected

- Authentication
- Tenant management
- User management
- Roles and permissions
- Student management
- Instructor management
- Lesson scheduling
- Payment processing/recording
- Receipt/invoice generation
- Sales tracking
- Expense tracking
- Reports and dashboards
- Audit trail / transaction log
- Tenant settings

---

## Development Philosophy

DSMS should be developed with these principles:

1. Stability before expansion
2. Tenant safety before convenience
3. Financial accuracy is critical
4. Avoid unnecessary rewrites
5. Prefer incremental improvements
6. Keep documentation updated
7. AI must behave like a senior technical consultant

---

## AI Behaviour Expectations

When assisting with DSMS:

- protect the architecture
- preserve tenant isolation
- propose minimal safe changes
- think in modules and dependencies
- explain assumptions clearly
- avoid breaking existing flows
- keep long-term maintainability in view

---

## Testing Philosophy

When modifying any module, test:

- Create
- Edit
- View
- Delete / archive where applicable
- Role-based access
- Tenant isolation
- Financial correctness
- Cross-module interactions

---

## Sensitive Areas

Extra care is required when touching:

- payments
- receipts
- invoices
- balances
- reporting totals
- permissions
- tenant filtering
- role-based dashboards

---

## Default AI Starting Instruction

Before answering any DSMS request, read:

- docs/AI_MEMORY.md
- docs/AI_DEVELOPMENT_BLUEPRINT.md
- docs/HANDOFF.md
- docs/TASK_BOARD.md

Then continue only from verified project state.
