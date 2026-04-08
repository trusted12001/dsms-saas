# DSMS Architecture Notes

Version: 1.3
Status: Active
Last Updated: 8 April 2026

---

## Purpose

This document records key architectural decisions made during the development of DSMS.

It answers:

- Why was this approach chosen?
- What alternatives were considered?
- What are the implications?

This prevents confusion and repeated rework.

---

## Decision Log

---

### 1. Multi-Tenant Approach

Decision:

Use **shared database, shared schema multi-tenancy** with `tenant_id` on tenant-owned records.

Details:

- Single database for the entire platform
- All tenants share the same tables
- Tenant-owned records include a `tenant_id` column
- Data isolation is enforced at the application level

Reason:

- Easier to implement and maintain
- Best fit for Laravel monolith architecture
- Supports real-time reporting and aggregation more easily
- Lower infrastructure complexity and cost for MVP and early growth

Alternatives considered:

- Multi-database per tenant
- Laravel tenancy package / more advanced tenancy models

Why not chosen now:

- Higher operational complexity
- Slower development speed
- Harder platform-wide reporting
- Not necessary at current project stage

Future plan:

- Can evolve into more advanced tenancy patterns if scale demands it

---

### 2. Tenant Table Classification

Decision:

Tables are classified into global tables and tenant-owned tables.

#### Global Tables (no tenant_id)

- tenants
- roles
- permissions

#### Tenant-Owned Tables (with tenant_id)

- users
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

Notes:

- Super Admin users may have `tenant_id = null`
- All tenant-owned records must include `tenant_id`

Implication:

- Tenant isolation must be enforced consistently
- Queries must not assume ownership indirectly where explicit ownership is required

---

### 3. Roles & Permissions System

Decision:

Use **Spatie Laravel Permission**

Reason:

- Mature and widely used
- Supports role + permission structure
- Easy to integrate with Laravel

Current role direction:

- Super Admin
- Business Owner
- Manager
- Sales
- Accounts

Implication:

- Roles and permissions remain globally defined
- Tenant restriction is enforced through `users.tenant_id` and tenant-aware authorization

---

### 4. Tenant Context Resolution Strategy

Decision:

Tenant context is resolved centrally using the authenticated user, with controlled Super Admin override through support mode.

Rules:

1. Every tenant-owned request must resolve an active tenant.
2. For tenant users, tenant context comes from `auth()->user()->tenant_id`.
3. Tenant users cannot switch tenants.
4. Super Admin has no active tenant by default.
5. Super Admin may enter a tenant context only through explicit support mode.
6. Tenant context must be resolved server-side only.
7. Tenant context must not come from request input such as URL, form, or query parameters.
8. No tenant-owned query should execute without a valid resolved tenant context.

Reason:

- Simpler and safer than request-driven or URL-driven tenant selection
- Prevents cross-tenant leakage
- Fits Laravel middleware/controller/policy patterns well
- Supports clean SaaS support workflows

Implication:

- Tenant context logic must be centralized
- Controllers and services must not implement ad-hoc tenant resolution logic

---

### 5. Support Mode Design

Decision:

Super Admin can explicitly enter a tenant workspace using **Support Mode**.

Rules:

- Support mode must be explicitly entered and exited
- The active tenant should be clearly visible in the UI
- Support actions should be auditable
- Support mode should use controlled server-side tenant context, likely session-based

Reason:

- Allows safe tenant troubleshooting and guided support
- Prevents confusion between platform context and tenant context
- Makes Super Admin assistance deliberate and traceable

Implication:

- Support-mode state must be managed carefully
- Sensitive actions may later require extra safeguards

---

### 6. Financial Data Handling

Decision:

All financial transactions must be:

- recorded explicitly
- traceable
- immutable where necessary

Implication:

- introduce `transaction_logs` table
- avoid silent updates to financial data
- use audit trail where possible

---

### 7. Data Ownership Rule

Decision:

Tenant-owned operational records must always store `tenant_id` explicitly.

Rules:

- `tenant_id` must be assigned from resolved tenant context, not user input
- Do not rely only on indirect relationships for tenant ownership
- Always store `tenant_id` explicitly on operational tables where ownership matters

Reason:

- Safer tenant filtering
- Easier reporting
- Clearer auditing
- Simpler joins and better maintainability

Implication:

- Record creation flows must assign tenant ownership automatically from server-side context

---

### 8. Real-Time Transaction Philosophy

Decision:

System should reflect business state immediately.

Implication:

- no delayed processing for core transactions
- dashboards must update based on latest data
- reporting must use live data

---

### 9. Monolith vs Microservices

Decision:

Use **Laravel Monolith**

Reason:

- Faster development
- Easier maintenance for MVP
- Suitable for SaaS startup phase

Future:

- extract services only if necessary

---

### 10. UI Strategy

Status: Pending Decision

Options:

- Bootstrap (fast)
- Tailwind (modern, flexible)

Recommendation:

Start with Bootstrap → upgrade later if needed

---

### 11. Module Isolation Strategy

Decision:

Use modular structure where possible:

- Controllers grouped by domain
- Models clearly scoped
- Services for complex logic

Future Option:

Introduce `Modules/` structure if complexity grows

---

### 12. Workspace & Route Architecture

Decision:

DSMS will use **two clearly separated workspace route structures**:

- Super Admin Workspace → `/super-admin/...`
- Tenant Workspace → `/app/...`

---

### Workspace Structure

#### 1. Super Admin Workspace

Prefix:
`/super-admin`

Used by:

- Platform owner (Super Admin)

Responsibilities:

- Tenant management
- Platform configuration
- Package/subscription management
- Platform-wide reporting
- System monitoring
- Support mode entry/exit

Example Routes:

- `/super-admin/dashboard`
- `/super-admin/tenants`
- `/super-admin/tenants/{tenant}`
- `/super-admin/platform-users`
- `/super-admin/packages`
- `/super-admin/reports`
- `/super-admin/settings`
- `/super-admin/support-mode/enter/{tenant}`
- `/super-admin/support-mode/exit`

---

#### 2. Tenant Workspace

Prefix:
`/app`

Used by:

- Business Owner
- Manager
- Sales
- Accounts
- Super Admin (only in support mode)

Responsibilities:

- Daily driving school operations
- Student and instructor management
- Lesson scheduling
- Payment and financial tracking
- Reports and dashboards

Example Routes:

- `/app/dashboard`
- `/app/students`
- `/app/instructors`
- `/app/vehicles`
- `/app/lessons`
- `/app/schedules`
- `/app/payments`
- `/app/receipts`
- `/app/expenses`
- `/app/reports`
- `/app/settings`
- `/app/users`

---

### Route Design Principles

1. Strict workspace separation
2. No tenant_id in URLs
3. Server-side tenant resolution
4. Support mode uses same `/app/...` routes
5. Flat route structure for MVP
6. UI/layout separation between workspaces

---

### 13. Middleware & Access Control Architecture

Decision:

DSMS will use a **layered middleware and access control architecture**.

---

### Access Control Layers

1. Authentication
2. Workspace access middleware
3. Tenant context enforcement
4. Role/permission authorization

---

### Middleware Families

- `auth`
- `super.admin`
- `tenant.workspace`
- `resolved.tenant`

---

### Route Protection Model

#### Super Admin

- `auth`
- `super.admin`

#### Tenant Workspace

- `auth`
- `tenant.workspace`
- `resolved.tenant`
- - permissions/policies

---

### Core Principle

**Middleware controls access to areas.  
Permissions control actions within areas.**

---

### Super Admin Support Mode Rule

Super Admin in support mode has full access within tenant workspace.

All actions must be auditable.

---

### Record-Level Tenant Protection

Every tenant-owned record must match the effective tenant context.

---

### Enforcement Locations

- Middleware → access entry
- Policies → action control
- Controllers/services → safe data handling

---

### DSMS Access Control Rules

1. All protected routes require authentication
2. Only Super Admin may access `/super-admin/...`
3. Only tenant users or Super Admin in support mode may access `/app/...`
4. Tenant context must always be resolved
5. All queries must be tenant-scoped
6. Tenant ownership must never come from request input

---

## Rules for Updating This File

Update this file when:

- a major architectural decision is made
- a previous decision is changed
- a better approach is adopted

---

## Important Principle

> If it is not documented here, it will be forgotten or debated again.
