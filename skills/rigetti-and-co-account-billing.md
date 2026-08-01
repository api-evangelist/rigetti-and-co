---
name: Check Rigetti QCS account balance and billing
description: Read a user's or group's QCS balance and billing invoices before booking reservations or running jobs.
api: openapi/rigetti-and-co-qcs-openapi.yml
operations: [GetUserBalance, GetGroupBalance, ListUserBillingInvoices, ListGroupBillingInvoices, GetUserUpcomingBillingInvoice, ListUserGroups]
---

# Check Rigetti QCS account balance and billing

Confirm funding and review invoices for a QCS user or group before committing to paid QPU time.

## Auth
- OAuth2 (Okta) bearer JWT via `Authorization: Bearer <access_token>`. Scope to the right principal with `x-qcs-account-id` / `x-qcs-account-type`.

## Steps
1. `ListUserGroups` (GET `/v1/users/{userId}/groups`) — find which groups a user belongs to.
2. `GetUserBalance` (GET `/v1/users/{userId}/balance`) or `GetGroupBalance` (GET `/v1/groups/{groupName}/balance`) — read available funds.
3. `ListUserBillingInvoices` (GET `/v1/users/{userId}/billingInvoices`) / `ListGroupBillingInvoices` — review invoices; page with `pageSize` + `pageToken`.
4. `GetUserUpcomingBillingInvoice` (GET `/v1/users/{userId}/billingInvoices:getUpcoming`) — preview the next invoice.

## Rules
- All reads; no idempotency concerns.
- A reservation attempt against an underfunded account returns `insufficient_payment` (402) — check balance here first.
- Error envelope + code vocabulary: `errors/rigetti-and-co-problem-types.yml`.
