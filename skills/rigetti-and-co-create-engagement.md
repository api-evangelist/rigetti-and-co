---
name: Open an execution engagement against a Rigetti endpoint
description: Resolve a quantum processor's default endpoint and create an engagement so a client can submit programs for execution.
api: openapi/rigetti-and-co-qcs-openapi.yml
operations: [GetDefaultEndpoint, ListEndpoints, GetEndpoint, CreateEngagement]
---

# Open an execution engagement against a Rigetti endpoint

Establish an execution engagement so the pyQuil / qcs-sdk client can submit compiled programs to a QPU endpoint.

## Auth
- OAuth2 (Okta) bearer JWT via `Authorization: Bearer <access_token>`.

## Steps
1. `GetDefaultEndpoint` (GET `/v1/quantumProcessors/{quantumProcessorId}/endpoints:getDefault`) — resolve the default endpoint for the target QPU. (Or `ListEndpoints` / `GetEndpoint` to select a specific one.)
2. `CreateEngagement` (POST `/v1/engagements`) — open an engagement for that endpoint; the response carries the connection/credential material the execution client uses.
3. Hand the engagement to the execution SDK (`qcs-sdk-python` / `pyquil`) — actual program submission and readout run over the QCS gRPC Controller service (`grpc/rigetti-and-co-controller-service.proto`), not this HTTP API.

## Rules
- `GetDefaultEndpoint` returns `not_found` (404) when no default endpoint is set — fall back to `ListEndpoints`.
- Errors use the custom JSON `Error` envelope with a `requestId`; quote it in support tickets.
- Program build/execution is done via the SDKs and gRPC — this HTTP surface handles discovery, engagement, reservations, and billing.
