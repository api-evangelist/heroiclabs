# Heroic Labs (Nakama) GraphQL Schema

## Overview

This document describes a conceptual GraphQL schema for the Heroic Labs Nakama game backend platform. Nakama exposes its functionality natively through REST, WebSocket, and gRPC interfaces. This GraphQL schema is a conceptual mapping of those capabilities into a typed GraphQL surface, covering the full breadth of Nakama's domain: authentication, accounts, users, social graph, real-time presence, matchmaking, leaderboards, tournaments, chat, storage, wallet, purchases, notifications, and server-side RPC.

The schema file is located at `graphql/heroiclabs-schema.graphql`.

## Schema Source

- **Provider**: Heroic Labs
- **Product**: Nakama (open-source game backend)
- **Native APIs**: REST (HTTP/1.1), WebSocket (real-time), gRPC
- **Reference**: https://heroiclabs.com/docs/nakama/client-libraries/rest-api/
- **GitHub**: https://github.com/heroiclabs/nakama
- **Schema Type**: Conceptual GraphQL mapping

## Domain Coverage

### Authentication and Sessions

Nakama supports multiple authentication strategies. The schema models `Session`, `Token`, `AccessToken`, and `RefreshToken` to represent the stateless JWT-based session Nakama issues after authentication. Social providers (Google, Facebook, Apple, Steam, GameCenter), device ID, email/password, and custom token authentication flows all yield a `Session`.

### Accounts and Users

`Account` is the top-level owner entity in Nakama, wrapping a `User` with wallet balance, device list, and linked social identifiers. `AccountDetails` extends this with metadata fields. `Profile` and `ProfileDetails` model the public-facing display name, avatar URL, and language tag that other players see. `UserStatus` tracks whether a user is online.

### Social Graph (Friends and Groups)

`Friend` and `FriendDetails` represent a directional relationship between two users, with `FriendStatus` encoding the four states Nakama uses: mutual friend, invited by self, invited by other, and blocked. `Group` and `GroupDetails` model team or clan constructs. `GroupMember` and `GroupMemberState` track membership with roles (superadmin, admin, member). `GroupJoinRequest` represents pending open-group applications.

### Notifications

`Notification` and `NotificationDetails` model server-pushed events delivered to a user's inbox. `NotificationCode` enumerates the built-in codes Nakama reserves for system notifications (friend requests, group events, tournament end, etc.) and the custom code range available for game logic.

### Chat and Messaging

`ChatChannel`, `ChatRoom`, and `DirectMessage` map to Nakama's three channel types: named rooms, groups, and direct message pairs. `Message` is the shared content envelope used across all channel types.

### Real-Time Multiplayer

`Match` and `MatchDetails` represent relayed or authoritative multiplayer sessions. `MatchState` captures the current lifecycle phase. `MatchData` models the binary or JSON payloads exchanged between clients during a match.

### Presence

`Presence` and `PresenceEvent` model the real-time online/offline and join/leave signals that Nakama broadcasts over WebSocket. They are shared across matches, chat channels, and status streams.

### Leaderboards and Tournaments

`Leaderboard` and `LeaderboardDetails` model the scored ranking constructs, with `Record` and `Score` capturing individual submissions. `Rank` represents a user's position. `Tournament` and `TournamentDetails` extend leaderboards with time windows, join requirements, and reward tiers. `TournamentRecord` tracks per-user tournament entries.

### Storage

`StorageObject` and `StorageDetails` model Nakama's key/value object store, which supports per-user and public namespaced collections with read/write permission controls.

### Wallet and Purchases

`Wallet` and `WalletDetails` represent a user's virtual currency balances. `WalletTransaction` records ledger entries. `Purchase`, `PurchaseDetails`, and `PurchaseType` model in-app purchase validation receipts from Apple App Store, Google Play, and Huawei AppGallery.

### Status

`Status` and `StatusDetails` represent a user's self-broadcast presence string, viewable by their friends.

### RPC

`RPC` and `RPCResult` model Nakama's server-side function invocation mechanism, which allows clients to call Go, TypeScript, or Lua functions deployed on the server.

### Webhooks and API Keys

`Webhook` and `WebhookDetails` represent HTTP callback registrations for server events. `APIKey` models server API keys used for server-to-server authentication.

### Console

`Console`, `ConsoleUser`, and `ConsoleDetails` map to Nakama's built-in web console for server administration, user management, and runtime inspection.

## Types Summary

| Category | Types |
|---|---|
| Auth / Session | Session, Token, AccessToken, RefreshToken |
| Account / User | Account, AccountDetails, Profile, ProfileDetails, User, UserDetails, UserStatus |
| Social | Friend, FriendDetails, FriendStatus, Group, GroupDetails, GroupMember, GroupMemberState, GroupJoinRequest |
| Notifications | Notification, NotificationDetails, NotificationCode |
| Chat | Message, Chat, ChatRoom, ChatChannel, DirectMessage |
| Multiplayer | Match, MatchDetails, MatchState, MatchData |
| Presence | Realtime, Presence, PresenceEvent |
| Leaderboards | Leaderboard, LeaderboardDetails, Record, Score, Rank |
| Tournaments | Tournament, TournamentDetails, TournamentRecord |
| Storage | StorageObject, StorageDetails |
| Wallet | Wallet, WalletDetails, WalletTransaction |
| Purchases | Purchase, PurchaseDetails, PurchaseType |
| Status | Status, StatusDetails |
| RPC | RPC, RPCResult |
| Webhooks | Webhook, WebhookDetails |
| API Keys | APIKey |
| Console | Console, ConsoleUser, ConsoleDetails |

## References

- Nakama REST API Reference: https://heroiclabs.com/docs/nakama/client-libraries/rest-api/
- Nakama WebSocket API: https://heroiclabs.com/docs/nakama/client-libraries/
- Nakama GitHub Repository: https://github.com/heroiclabs/nakama
- Heroic Labs Documentation: https://heroiclabs.com/docs/nakama/
- Heroic Cloud: https://heroiclabs.com/heroic-cloud/
