---
name: Reserve execution time on a Rigetti QPU
description: Check a quantum processor's calendar, find an available slot, and book (or cancel) a reservation on Quantum Cloud Services.
api: openapi/rigetti-and-co-qcs-openapi.yml
operations: [GetQuantumProcessorCalendar, FindAvailableReservations, CreateReservation, ListReservations, GetReservation, DeleteReservation]
---

# Reserve execution time on a Rigetti QPU

Book dedicated time on a Rigetti quantum processor through the QCS API (`https://api.qcs.rigetti.com`).

## Auth
- OAuth2 (Okta) bearer JWT via `Authorization: Bearer <access_token>`. Reservations may be scoped to a group/user via the `x-qcs-account-id` / `x-qcs-account-type` headers.

## Steps
1. `GetQuantumProcessorCalendar` (GET `/v1/calendars/{quantumProcessorId}`) — review the QPU's availability calendar.
2. `FindAvailableReservations` (GET `/v1/reservations:findAvailable`) — find open slots matching your `duration` and `startTimeFrom`.
3. `CreateReservation` (POST `/v1/reservations`) — book a slot.
4. `ListReservations` (GET `/v1/reservations`) / `GetReservation` (GET `/v1/reservations/{reservationId}`) — confirm the booking.
5. `DeleteReservation` (DELETE `/v1/reservations/{reservationId}`) — cancel if plans change.

## Rules
- A booking can fail with `insufficient_payment` (402) if funds are unavailable, or `reservation_unavailable` (409) if the slot is taken — check the group/user balance first (`GetGroupBalance` / `GetUserBalance`).
- No idempotency key is supported; do not blind-retry `CreateReservation` on a timeout — re-list and reconcile first.
- Error envelope + codes: `errors/rigetti-and-co-problem-types.yml`.
