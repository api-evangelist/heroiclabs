---
name: Authenticate a player and read their account
description: Obtain a Nakama session for a player and load their account profile.
api: openapi/heroic-labs-nakama-openapi-original.json
operations: [Nakama_AuthenticateEmail, Nakama_AuthenticateDevice, Nakama_GetAccount, Nakama_SessionRefresh]
---

# Authenticate a player and read their account

Nakama sessions are JWTs obtained from an authenticate endpoint, then sent as a
Bearer token on subsequent calls.

## Steps
1. Authenticate. Call `Nakama_AuthenticateEmail` (POST `/v2/account/authenticate/email`)
   with `{ email, password }`, or `Nakama_AuthenticateDevice` (POST
   `/v2/account/authenticate/device`) with a device id for guest login. The request is
   authorized with the server key via HTTP Basic (username = server key, empty password).
   Set `create=true` to auto-create the account on first login.
2. Store the returned `token` (and `refresh_token`) from the `apiSession`.
3. Read the profile. Call `Nakama_GetAccount` (GET `/v2/account`) with
   `Authorization: Bearer <token>`.
4. Refresh when expired. Call `Nakama_SessionRefresh` (POST
   `/v2/account/session/refresh`) with the refresh token rather than re-authenticating.

## Rules
- Errors come back as a gRPC `rpcStatus` envelope (`code`/`message`); `16` = UNAUTHENTICATED,
  `3` = INVALID_ARGUMENT. See errors/heroic-labs-problem-types.yml.
- There is no idempotency-key header; authenticate calls are keyed by identity so repeats
  are safe. See conventions/heroic-labs-conventions.yml.
