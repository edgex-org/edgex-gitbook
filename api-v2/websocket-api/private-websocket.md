# Private WebSocket API

The Private WebSocket API provides real-time account and trading updates for the authenticated account.

## Connection Information

**WebSocket URL:** `wss://<ws-domain>/api/v1/private/ws`

Replace `<ws-domain>` with the production WebSocket domain `edgex-quote-prod-v2.edgex.exchange`.

**Authentication:** Required.

Private WebSocket authentication uses the same **HMAC-SHA256** rules as the private HTTP APIs. For credential requirements, header definitions, and signature generation, see the [Authentication Guide](../authentication.md).

## Common Usage Flow

All private account-event streams follow the same connection lifecycle:

1. Build the connection URL with `accountId` and `timestamp`
2. Sign the request with the same HMAC-SHA256 logic used by private HTTP APIs
3. Connect to `wss://<ws-domain>/api/v1/private/ws`
4. Receive the initial `Snapshot` after authentication succeeds
5. Continue processing incremental `trade-event` messages
6. Respond to `ping` with `pong`
7. Keep a local cache by applying updates in event order

## How to Read a Private WebSocket Message

There are three important concepts in private WebSocket messages:

- `type` - the top-level message category, such as `connected`, `trade-event`, or `ping`
- `content.event` - the business event inside a `trade-event`, such as `Snapshot`, `ACCOUNT_UPDATE`, or `ORDER_UPDATE`
- `content.data` - the event payload grouped by entity type, such as `account`, `position`, `order`, or `transferOut`

In practice:

- `type` tells you what kind of envelope this is
- `content.event` tells you which account or order event happened
- `content.data` tells you which business entities changed in that event

## Common Request and Response Models

### Connect and Authenticate

#### Description

Establish an authenticated private WebSocket connection.

#### Request Parameters

##### Query Parameters

| Field | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | integer | yes | EdgeX account identifier |
| `timestamp` | integer | yes | Current millisecond timestamp |

##### Headers

| Field | Type | Required | Description |
|------|------|----------|-------------|
| `X-edgeX-Api-Key` | string | yes | API key |
| `X-edgeX-Passphrase` | string | yes | API passphrase |
| `X-edgeX-Api-Signature` | string | yes | HMAC-SHA256 signature |
| `X-edgeX-Api-Timestamp` | string | yes | Timestamp used for signing |

#### Request Example

```http
GET /api/v1/private/ws?accountId=724625476626153743&timestamp=1705720068228
X-edgeX-Api-Key: your-api-key
X-edgeX-Passphrase: your-api-passphrase
X-edgeX-Api-Signature: <signature generated as described in the Authentication Guide>
X-edgeX-Api-Timestamp: 1705720068228
```

#### Response Model

After authentication succeeds, the server starts pushing private events. The first business message is usually the initial `Snapshot`.

### Trade Event

#### Description

Receive account and trading updates for the authenticated account.

#### Response Model

##### Top-Level Envelope

| Field | Type | Description |
|------|------|-------------|
| `sid` | string | Session identifier assigned by the server |
| `type` | string | Top-level message type. For account pushes, this is `trade-event` |
| `content` | object | Message-specific payload |
| `time` | string | Timestamp used by heartbeat messages |

##### Top-level `type` Values

| `type` | Meaning |
|-------|---------|
| `connected` | Connection established |
| `trade-event` | Account or order event push |
| `ping` | Server heartbeat |
| `error` | Authentication or business error |

##### `content.event` Values

| `content.event` | Meaning |
|----------------|---------|
| `Snapshot` | Initial full account state after connection |
| `ACCOUNT_UPDATE` | Account-level balances, margin, or state changed |
| `DEPOSIT_UPDATE` | Deposit record updated |
| `WITHDRAW_UPDATE` | Withdrawal record updated |
| `TRANSFER_IN_UPDATE` | Transfer-in record updated |
| `TRANSFER_OUT_UPDATE` | Transfer-out record updated |
| `ORDER_UPDATE` | Order lifecycle update |
| `FORCE_WITHDRAW_UPDATE` | Forced withdrawal update |
| `FORCE_TRADE_UPDATE` | Forced trade update |
| `FUNDING_SETTLEMENT` | Funding settlement update |
| `ORDER_FILL_FEE_INCOME` | Fill fee income or rebate update |
| `START_LIQUIDATING` | Liquidation started |
| `FINISH_LIQUIDATING` | Liquidation finished |

##### `trade-event` Payload

| Field | Type | Description |
|------|------|-------------|
| `content.event` | string | Business event type |
| `content.version` | integer | Event version used for ordered processing |
| `content.time` | integer | Event creation time |
| `content.accountId` | integer | EdgeX account identifier |
| `content.data` | object | Business payload grouped by entity type |

##### `content.data` Fields

| Field | Type | Description |
|------|------|-------------|
| `account` | array | Account-level updates |
| `collateral` | array | Collateral balance updates |
| `collateralTransaction` | array | Collateral transaction updates |
| `position` | array | Position snapshots or changes |
| `positionTransaction` | array | Position transaction updates |
| `deposit` | array | Deposit record updates |
| `withdraw` | array | Withdrawal record updates |
| `transferIn` | array | Transfer-in updates |
| `transferOut` | array | Transfer-out updates |
| `order` | array | Order updates |
| `orderFillTransaction` | array | Fill-level execution updates |
| `forceWithdrawList` | array | Forced withdrawal updates |
| `forceTradeList` | array | Forced trade updates |
| `oraclePriceList` | array | Oracle price related updates |
| `positionTermList` | array | Position term updates |

#### Response Examples

##### Initial Snapshot

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 1000,
    "time": 1693208170000,
    "accountId": 724625476626153743,
    "data": {
      "account": [],
      "collateral": [],
      "position": [],
      "order": []
    }
  }
}
```

##### Order Update

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": 1001,
    "time": 1693208171000,
    "accountId": 724625476626153743,
    "data": {
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Heartbeat

#### Description

Maintain the connection by responding to server heartbeat messages.

#### Server Message Example

```json
{
  "type": "ping",
  "time": "1693208170000"
}
```

#### Client Response Example

```json
{
  "type": "pong",
  "time": "1693208170000"
}
```

## Business Streams

### Initial Account Snapshot

#### Use Case

Use the first `Snapshot` message to initialize the local account cache immediately after the private connection is established.

#### Trigger

This message is pushed automatically after authentication succeeds.

#### Response Model

The server returns a `trade-event` message where:

- `content.event = Snapshot`
- `content.version` is the initial event version for the session
- `content.data` contains the initial account state grouped by entity type

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 1000,
    "time": 1693208170000,
    "accountId": 724625476626153743,
    "data": {
      "account": [],
      "collateral": [],
      "position": [],
      "order": []
    }
  }
}
```

### Account State Updates

#### Use Case

Use account-state events to refresh balances, margin, collateral, positions, and related account state.

#### Trigger

These updates are pushed automatically when account-level data changes.

#### Event Types

- `ACCOUNT_UPDATE`
- `DEPOSIT_UPDATE`
- `WITHDRAW_UPDATE`
- `TRANSFER_IN_UPDATE`
- `TRANSFER_OUT_UPDATE`
- `FUNDING_SETTLEMENT`

#### Response Model

The server returns a `trade-event` message where:

- `content.event` is one of the account-state event types listed above
- `content.version` increases with each update
- `content.data` usually populates arrays such as `account`, `collateral`, `position`, `deposit`, `withdraw`, `transferIn`, or `transferOut`

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "ACCOUNT_UPDATE",
    "version": 1002,
    "time": 1693208172000,
    "accountId": 724625476626153743,
    "data": {
      "account": [],
      "collateral": [],
      "position": []
    }
  }
}
```

### Order Lifecycle Updates

#### Use Case

Use order-lifecycle events to track order creation, cancellation, partial fill, full fill, and fee-related changes.

#### Trigger

These updates are pushed automatically after order state changes.

#### Event Types

- `ORDER_UPDATE`
- `ORDER_FILL_FEE_INCOME`

#### Response Model

The server returns a `trade-event` message where:

- `content.event` is `ORDER_UPDATE` or `ORDER_FILL_FEE_INCOME`
- `content.data.order` contains order-level updates
- `content.data.orderFillTransaction` contains fill-level execution updates when applicable

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": 1003,
    "time": 1693208173000,
    "accountId": 724625476626153743,
    "data": {
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Risk and Forced Events

#### Use Case

Use these events to detect forced withdrawals, forced trades, and liquidation state changes.

#### Trigger

These updates are pushed automatically during risk-control workflows.

#### Event Types

- `FORCE_WITHDRAW_UPDATE`
- `FORCE_TRADE_UPDATE`
- `START_LIQUIDATING`
- `FINISH_LIQUIDATING`

#### Response Model

The server returns a `trade-event` message where:

- `content.event` is one of the risk-related event types listed above
- `content.data.forceWithdrawList` or `content.data.forceTradeList` is populated for forced-event updates
- other account arrays may also be present when the event affects balances or positions

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "FORCE_TRADE_UPDATE",
    "version": 1004,
    "time": 1693208174000,
    "accountId": 724625476626153743,
    "data": {
      "forceTradeList": []
    }
  }
}
```

## Integration Notes

- Reuse the private HTTP authentication code instead of implementing a second signing flow for WebSocket
- Treat `Snapshot` as your initial cache and later `trade-event` messages as incremental updates
- Route messages by `type`, then route business events by `content.event`
- Keep heartbeat handling independent from business event handling
- Expect different event types to populate different arrays under `content.data`
