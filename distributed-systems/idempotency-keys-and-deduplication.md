# Idempotency Keys and API Deduplication

## Motivation
At-least-once message delivery over lossy networks can duplicate requests (e.g., payment charges during client retry on timeout).

## Implementation Architecture
1. Client generates UUID v4 idempotency key attached as HTTP header (`Idempotency-Key: <uuid>`).
2. Server begins database transaction:
   - Queries `idempotency_keys` table.
   - If key exists with status `COMPLETED`, returns cached response immediately.
   - If key exists with status `PROCESSING`, returns HTTP 409 Conflict.
   - If not found, inserts key with status `PROCESSING`.
3. Server executes business logic.
4. Updates idempotency record with `COMPLETED` and response payload atomically.
