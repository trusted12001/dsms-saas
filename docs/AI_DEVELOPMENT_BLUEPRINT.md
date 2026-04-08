# DSMS AI Development Blueprint

Version: 1.0
Status: Initial Architecture Draft
Last Updated: 28 March 2026
Project: DSMS – Driving School Management System

Note: AI sessions should begin with the Master Boot Prompt stored in `docs/AI_PROMPT_LIBRARY.md`.

---

## Project Overview

DSMS is a multi-tenant SaaS Driving School Management System built with Laravel 12.

It is designed to help driving schools manage their daily operations in real time, including student registration, lesson booking, instructor scheduling, payments, sales, accounting, reporting, and business oversight.

The platform supports both:

- Platform-level management by the Super Admin
- Tenant-level management by each driving school

Each driving school operates as an independent tenant within the same SaaS platform.

---

## Business Goal

The goal of DSMS is to provide a real-time operational and financial management system for driving schools.

It should help school owners and managers:

- track transactions as they happen
- monitor sales and income
- manage students and instructors
- oversee lesson bookings and schedules
- monitor debt, payments, and balances
- generate business reports
- improve accountability across departments

---

## Technology Stack

Backend:
Laravel 12

Frontend:
Blade templates + Bootstrap / Tailwind CSS (to be confirmed)

Database:
MySQL

Authentication:
Laravel authentication

Roles & Permissions:
Spatie Laravel Permission (recommended)

Version Control:
GitHub

Hosting:
To be decided

---

## SaaS / Multi-Tenant Architecture

DSMS is a multi-tenant SaaS application.

Each driving school is a tenant.

Core tenant rule:

- one tenant must never access another tenant’s data

Recommended tenant model:

- `tenants` table
- `tenant_id` column on tenant-owned records
- tenant-aware controllers, models, and queries

All operational records must be tenant-aware.

---

## Workspace Structure

DSMS should have two major workspace layers:

### 1. Super Admin Workspace

This is for the platform owner.

Responsibilities:

- manage all tenants
- manage subscription or platform access
- manage platform-wide settings
- monitor usage and system health
- access support mode
- manage package plans and permissions
- view platform-level reports

### 2. Tenant Workspace

This is for each driving school.

Tenant-side users include:

- Business Owner
- Manager
- Sales
- Accounts

Each tenant user must only operate within their own school.

---

## Tenant Roles

### Super Admin

Platform owner with full control over the SaaS platform.

### Business Owner

Tenant owner of a specific driving school.
Can see overall business performance for that tenant.

### Manager

Runs day-to-day driving school operations.

### Sales

Handles enquiries, registration, student conversion, and booking coordination.

### Accounts

Handles payments, receipts, balances, debt tracking, and financial reporting.

---

## Core Functional Modules

### Platform / SaaS Core

- Authentication
- Tenant management
- User management
- Roles and permissions
- Subscription/package management
- Support mode
- Audit logs
- Global settings

### Tenant Operations

- Dashboard
- Student management
- Instructor management
- Vehicle management
- Lesson scheduling
- Lesson attendance
- Test booking tracking
- Student progress tracking
- Branch management (if needed)
- Document management

### Commercial / Financial

- Sales management
- Payment collection
- Receipts / invoices
- Outstanding balances
- Expense management
- Income reports
- Accounts summary
- Real-time transaction log

### Reporting

- Daily sales report
- Student registration report
- Payment report
- Outstanding debt report
- Instructor activity report
- Lesson report
- Business performance dashboard

---

## Real-Time Transaction Goal

A key architectural goal of DSMS is real-time transaction visibility.

Important requirement:

Whenever money is received, a booking is made, a student is registered, or a lesson is scheduled, the system should update the relevant records immediately so management can see the latest business state without delay.

---

## Development Rules

1. Do not remove working code unless absolutely necessary.
2. Preserve tenant isolation at all times.
3. Make small, safe, incremental changes.
4. Keep code readable and maintainable.
5. Document architecture updates in `ARCHITECTURE_NOTES.md`.
6. Update `HANDOFF.md` and `TASK_BOARD.md` after each major session.
7. Prefer consultant-style AI collaboration over blind code generation.

---

## Recommended Initial Database Thinking

Likely core tables:

- tenants
- users
- roles
- permissions
- students
- instructors
- vehicles
- lessons
- schedules
- payments
- receipts
- expenses
- branches
- transaction_logs
- student_progress
- driving_tests
- tenant_settings

This list may evolve.

---

## Team Workflow

When stopping work:

1. Push code to GitHub
2. Update `HANDOFF.md`
3. Update `TASK_BOARD.md`
4. Record important reasoning in `ARCHITECTURE_NOTES.md` if needed

When resuming work:

1. Pull latest code
2. Read `ReadMeFirst.md`
3. Read `AI_MEMORY.md`
4. Read `HANDOFF.md`
5. Continue from the next verified task

---

## AI Prompt Starter

Use this to begin AI sessions:

"You are the Technical Architect for DSMS, a Laravel 12 multi-tenant SaaS Driving School Management System.

Rules:

- preserve tenant isolation
- do not remove working code unnecessarily
- make minimal safe changes
- work from documented project state
- explain reasoning clearly before major code suggestions

Always read:
docs/AI_MEMORY.md
docs/AI_DEVELOPMENT_BLUEPRINT.md
docs/HANDOFF.md
docs/TASK_BOARD.md
before answering."
