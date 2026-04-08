# DSMS Workflow Test Prompt

Version: 1.0
Status: Active

---

## Purpose

This file ensures every feature is properly tested before being considered complete.

It provides a standard testing approach for DSMS.

---

## Universal Test Prompt

Use this after implementing any feature:

---

Help me test this DSMS module:

[module name]

Context:

- Laravel 12
- multi-tenant SaaS
- role-based access

Provide a full test checklist covering:

1. Create
2. Read / View
3. Update
4. Delete / Archive
5. Role-based access
6. Tenant isolation
7. Cross-module interactions
8. Edge cases
9. Data integrity
10. Financial correctness (if applicable)

---

## Manual Testing Checklist

For every module, verify:

---

### 1. CRUD Operations

- Create works correctly
- Edit updates correctly
- View displays correct data
- Delete behaves safely (soft delete if needed)

---

### 2. Tenant Isolation

- Tenant A cannot see Tenant B data
- Queries are properly filtered
- No leakage across tenants

---

### 3. Role-Based Access

Check for:

- Business Owner
- Manager
- Sales
- Accounts

Ensure:

- correct access
- restricted actions
- UI reflects permissions

---

### 4. Data Integrity

- no duplicate records where not expected
- relationships are valid
- foreign keys are respected

---

### 5. Financial Validation (Critical)

For payment-related modules:

- totals are correct
- balances update correctly
- no silent changes
- transaction history is accurate

---

### 6. Workflow Validation

Simulate real use:

Example:

- register student
- book lesson
- record payment
- generate report

Ensure full flow works.

---

### 7. Edge Cases

Test:

- empty inputs
- invalid data
- large data
- simultaneous actions

---

### 8. UI Behavior

- forms submit correctly
- validation messages show
- no broken layouts
- dashboards reflect changes

---

## Automation (Future)

Future improvement:

- PHPUnit tests
- Feature tests
- API tests (if API introduced)

---

## Important Rule

A feature is NOT complete until:

- it works
- it is tested
- it respects tenant isolation
- it passes workflow validation

---

## Final Principle

> If it is not tested, it is broken — even if it works once.
