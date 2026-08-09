---
name: Submit and read a leaderboard score
description: Write a player's score to a leaderboard and read back the standings.
api: openapi/heroic-labs-nakama-openapi-original.json
operations: [Nakama_WriteLeaderboardRecord, Nakama_ListLeaderboardRecords, Nakama_ListLeaderboardRecordsAroundOwner]
---

# Submit and read a leaderboard score

## Steps
1. Write the score. Call `Nakama_WriteLeaderboardRecord`
   (POST `/v2/leaderboard/{leaderboardId}`) with `Authorization: Bearer <token>` and a
   body containing `score` (and optional `subscore`, `metadata`). The leaderboard must
   already exist (created server-side).
2. Read the top of the board. Call `Nakama_ListLeaderboardRecords`
   (GET `/v2/leaderboard/{leaderboardId}`) with `limit` and an optional `cursor`.
3. Read around the player. Call `Nakama_ListLeaderboardRecordsAroundOwner`
   (GET `/v2/leaderboard/{leaderboardId}/owner/{ownerId}`) to fetch records centered on
   a specific user's rank.

## Rules
- Pagination is opaque cursor-based: pass the returned `cursor` to page forward.
- The leaderboard operator (server config) decides the sort order and score aggregation
  (best / set / increment / decrement).
- Errors are gRPC `rpcStatus`; `5` = NOT_FOUND if the leaderboard id is wrong.
