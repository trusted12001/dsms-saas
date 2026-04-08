# DSMS Architecture Notes

Version: 1.0
Status: Active
Last Updated: 28 March 2026

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

Status: Pending Decision

Options:

A. Single Database (tenant_id column)
B. Multi-database per tenant
C. Laravel tenancy package (e.g., tenancyforlaravel)

Current Thinking:

- Start with **single database + tenant_id**
- Easier to implement
- Easier to maintain
- Suitable for MVP

Future Plan:

- Can evolve into advanced tenancy if scaling requires

---

### 2. Roles & Permissions System

Decision:

Use **Spatie Laravel Permission**

Reason:

- Mature and widely used
- Supports role + permission structure
- Easy to integrate with Laravel

---

### 3. Financial Data Handling

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

### 4. Real-Time Transaction Philosophy

Decision:

System should reflect business state immediately.

Implication:

- no delayed processing for core transactions
- dashboards must update based on latest data
- reporting must use live data

---

### 5. Monolith vs Microservices

Decision:

Use **Laravel Monolith**

Reason:

- Faster development
- Easier maintenance for MVP
- Suitable for SaaS startup phase

Future:

- extract services only if necessary

---

### 6. UI Strategy

Status: Pending Decision

Options:

- Bootstrap (fast)
- Tailwind (modern, flexible)

Recommendation:

Start with Bootstrap → upgrade later if needed

---

### 7. Module Isolation Strategy

Decision:

Use modular structure where possible:

- Controllers grouped by domain
- Models clearly scoped
- Services for complex logic

Future Option:

Introduce `Modules/` structure if complexity grows

---

## Rules for Updating This File

Update this file when:

- a major architectural decision is made
- a previous decision is changed
- a better approach is adopted

Always include:

- what changed
- why it changed
- impact on system

---

## Important Principle

> If it is not documented here, it will be forgotten or debated again.

This file protects your long-term vision.
