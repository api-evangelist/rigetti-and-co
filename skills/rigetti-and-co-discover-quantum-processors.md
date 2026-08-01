---
name: Discover Rigetti quantum processors and their capabilities
description: List available Rigetti quantum processors and pull the instruction set architecture (ISA) and qubit accessors for a chosen QPU before compiling a program.
api: openapi/rigetti-and-co-qcs-openapi.yml
operations: [ListQuantumProcessors, GetQuantumProcessor, GetInstructionSetArchitecture, GetQuantumProcessorAccessors, ListInstructionSetArchitectures]
---

# Discover Rigetti quantum processors and their capabilities

Use the Rigetti QCS API (`https://api.qcs.rigetti.com`) to find a quantum processor and learn its topology before you compile.

## Auth
- OAuth2 (Okta) bearer JWT. Download an access token from `https://qcs.rigetti.com/auth/token` (valid 24h) and send `Authorization: Bearer <access_token>`. See `authentication/rigetti-and-co-authentication.yml`.

## Steps
1. `ListQuantumProcessors` (GET `/v1/quantumProcessors`) — enumerate available QPUs. Page with `pageSize` + `pageToken`.
2. `GetQuantumProcessor` (GET `/v1/quantumProcessors/{quantum_processor_id}`) — fetch metadata for the QPU you want.
3. `GetInstructionSetArchitecture` (GET `/v1/quantumProcessors/{quantum_processor_id}/instructionSetArchitecture`) — pull the ISA (qubits, edges, native gates) to drive compilation.
4. `GetQuantumProcessorAccessors` (GET `/v1/quantumProcessors/{quantum_processor_id}/accessors`) — list the execution accessors for the QPU.

## Rules
- Errors use the custom JSON `Error` envelope (`code`, `message`, `requestId`) — not RFC 9457. Quote `requestId` in support tickets. See `errors/rigetti-and-co-problem-types.yml`.
- No idempotency-key header is documented; these are all safe GET reads.
- Prefer the `pyquil` / `qcs-sdk-python` SDKs to compile and execute; this API surface is for discovery, reservations, and account management.
