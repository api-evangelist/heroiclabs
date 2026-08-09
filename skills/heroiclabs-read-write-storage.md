---
name: Read and write player storage objects
description: Persist and retrieve versioned JSON data objects for a player.
api: openapi/heroic-labs-nakama-openapi-original.json
operations: [Nakama_WriteStorageObjects, Nakama_ReadStorageObjects, Nakama_ListStorageObjects, Nakama_DeleteStorageObjects]
---

# Read and write player storage objects

Nakama's storage engine holds versioned JSON documents keyed by
`(collection, key, user_id)`.

## Steps
1. Write. Call `Nakama_WriteStorageObjects` (PUT `/v2/storage`) with
   `Authorization: Bearer <token>` and `objects[]`, each `{ collection, key, value,
   permission_read, permission_write, version }`. Pass the previous `version` to enforce
   optimistic concurrency (write only if unchanged); pass `"*"` to create-only.
2. Read. Call `Nakama_ReadStorageObjects` (POST `/v2/storage`) with
   `object_ids[]` of `{ collection, key, user_id }`.
3. List a collection. Call `Nakama_ListStorageObjects`
   (GET `/v2/storage/{collection}`) with `limit` + `cursor`.
4. Delete. Call `Nakama_DeleteStorageObjects` (PUT `/v2/storage/delete`) with the
   object ids (optionally with `version` for a safe delete).

## Rules
- The `version` field is the idempotency/OCC guard for writes — a mismatch returns a
  conflict rather than overwriting. See conventions/heroic-labs-conventions.yml.
- `permission_read`/`permission_write` (0 = no, 1 = owner, 2 = public) control access.
- Errors are gRPC `rpcStatus`.
