# Private WebSocket API

The Private WebSocket API provides real-time account and trading updates for the authenticated account.

## Connection Information

**WebSocket URL:** `wss://<ws-domain>/api/v1/private/ws`

Replace `<ws-domain>` with the production WebSocket domain `edgex-quote-prod-v2.edgex.exchange`.

**Authentication:** Required.

Private WebSocket authentication uses the same **HMAC-SHA256** rules as the private HTTP APIs. For credential requirements, header definitions, and signature generation, see the [Authentication Guide](../authentication.md).

## Connection Mode

Private WebSocket uses an **authenticate once, then receive events continuously** model.

- No separate `subscribe` request is required.
- After authentication succeeds, the server first pushes the initial account snapshot.
- After the baseline snapshot is established, the server continues to push incremental business updates.
- The client is expected to handle heartbeat messages during the session.

## Common Usage Flow

1. Build the connection URL with `accountId` and `timestamp`.
2. Sign the request with the same HMAC-SHA256 logic used by private HTTP APIs.
3. Connect to `wss://<ws-domain>/api/v1/private/ws`.
4. Receive `connected`.
5. Receive the initial `Snapshot`.
6. Continue processing incremental business updates.
7. Respond to `ping` with `pong`.
8. Reconnect and rebuild local state from a new `Snapshot` if the connection is closed.

## Common Request and Response Models

### Connect and Authenticate

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

#### Response Sequence

After authentication succeeds, the server returns messages in this order:

1. `connected`
2. initial account snapshot
3. later business updates when account-related data changes

#### Connection Success Example

```json
{
  "sid": "session-id-string",
  "type": "connected",
  "time": 1693208170000
}
```

### Message Envelope

There are three important concepts in private WebSocket messages:

- `type` is the top-level message category, such as `connected`, `trade-event`, `ping`, or `error`.
- `content.event` is the business event inside a push message, such as `Snapshot`, `ORDER_UPDATE`, or `FUNDING_SETTLEMENT`.
- `content.data` is the payload grouped by entity type, such as `account`, `collateral`, `position`, `order`, `deposit`, or `transferOut`.

In practice:

- `type` tells you which message family you received.
- `content.event` tells you which business event happened.
- `content.data` tells you which business entities changed in that event.

#### Top-Level Envelope

| Field | Type | Description |
|------|------|-------------|
| `sid` | string | Session identifier assigned by the server |
| `type` | string | Top-level message type |
| `content` | object | Message-specific payload |
| `time` | integer | Timestamp used by heartbeat messages |

#### Top-Level `type` Values

| `type` | Meaning |
|-------|---------|
| `connected` | Connection established |
| `trade-event` | Private business push |
| `ping` | Server or client heartbeat |
| `pong` | Heartbeat response |
| `error` | Authentication error, invalid client message, or business error |

#### `content.data` Field Rules

- `content.data` includes only the fields relevant to the current event.
- Fields that are not affected by the current event may be omitted or empty.
- The initial `Snapshot` is the baseline payload used to initialize local state.
- Later business updates are incremental messages applied after the snapshot.

## Heartbeat

The private WebSocket connection uses a bidirectional heartbeat mechanism to keep the session alive and measure latency.

### Server Ping

After the connection is established, the server periodically sends a `ping` message.

```json
{
  "type": "ping",
  "time": 1693208170000
}
```

When the client receives this message, it should return a `pong` message with the same `time` value.

```json
{
  "type": "pong",
  "time": 1693208170000
}
```

### Client Ping

The client can also send a `ping` message to measure round-trip latency.

```json
{
  "type": "ping",
  "time": 1693208170000
}
```

The server responds immediately with a `pong` message and echoes the same `time` value.

```json
{
  "type": "pong",
  "time": 1693208170000
}
```

## Initial Snapshot

After a successful connection and authentication, the server first pushes an initial snapshot message. This snapshot provides the current account state and acts as the baseline for all later incremental business updates.

### Message Mapping

- `type`: `trade-event`
- `content.event`: `Snapshot`

### Response Model

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 1000,
    "time": 1693208170000,
    "data": {
      "account": [],
      "collateral": [],
      "position": [],
      "order": []
    }
  }
}
```

### Field Details

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Message type. The initial snapshot is sent as `trade-event`. |
| `content.event` | string | Always `Snapshot` for the initial full-state payload. |
| `content.version` | integer | Snapshot version. Later incremental events continue from this version sequence. |
| `content.time` | integer | Server timestamp in milliseconds. |
| `content.data.account` | array | Current account state. |
| `content.data.collateral` | array | Current collateral balances. |
| `content.data.position` | array | Current open positions. |
| `content.data.order` | array | Current active orders. |

## Event Updates

After the initial snapshot, the server pushes incremental updates grouped by business meaning rather than listing raw event names as flat top-level sections.

This page organizes the business updates into four categories:

- `Account Snapshot`
- `Trade Updates`
- `Asset Updates`
- `Funding Settlement`

Other non-covered event families are intentionally left out of this page body.

### Account Snapshot

This category represents the baseline account-state message that clients use to initialize local state before applying incremental updates.

#### Trigger Scenario

The server pushes this message immediately after a successful authenticated connection. Clients should also use the same snapshot category to rebuild local state after reconnecting.

#### Message Mapping

- `type`: `trade-event`
- `content.event`: `Snapshot`

#### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `account` | Current account summary for the authenticated account. |
| `collateral` | Current collateral balances at snapshot time. |
| `position` | Current open positions. |
| `order` | Current active orders. |

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 1000,
    "time": 1693208170000,
    "data": {
      "account": [
        {
          "id": 724625476626153743,
          "status": "NORMAL"
        }
      ],
      "collateral": [
        {
          "coinId": 1,
          "balance": "1200"
        }
      ],
      "position": [
        {
          "contractId": 1000001,
          "size": "1"
        }
      ],
      "order": [
        {
          "id": 900001,
          "contractId": 1000001,
          "status": "OPEN"
        }
      ]
    }
  }
}
```

### Trade Updates

This category covers trade-driven account changes. In practice, the most important incremental event for client processing is `ORDER_UPDATE`.

#### Trigger Scenario

The server pushes this message when an order is created, updated, partially filled, fully filled, or canceled, and when the resulting trade changes related account or position state.

#### Message Mapping

- `type`: `trade-event`
- `content.event`: `ORDER_UPDATE`

#### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `account` | Updated account summary after the trade-related change. |
| `collateral` | Updated collateral balances if margin or balance changes. |
| `collateralTransaction` | Collateral transaction records related to the order lifecycle. |
| `position` | Updated position state if the order opens, reduces, or closes a position. |
| `positionTransaction` | Position change records created by fills or position transitions. |
| `order` | Updated order records. |
| `orderFillTransaction` | Fill records when the order is matched. |
| `positionTermList` | Updated position term information when applicable. |

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": 1001,
    "time": 1693208175000,
    "data": {
      "order": [
        {
          "id": 900001,
          "contractId": 1000001,
          "status": "FILLED"
        }
      ],
      "orderFillTransaction": [
        {
          "id": 800001,
          "orderId": 900001,
          "fillSize": "1"
        }
      ],
      "position": [
        {
          "contractId": 1000001,
          "size": "1"
        }
      ]
    }
  }
}
```

### Asset Updates

This category groups balance-affecting updates caused by deposits, withdrawals, and internal transfers.

#### Trigger Scenario

The server pushes this category when an asset movement changes account balances or updates the state of the related asset-flow record.

#### Message Mapping

- `type`: `trade-event`
- `content.event`: one of `DEPOSIT_UPDATE`, `WITHDRAW_UPDATE`, `TRANSFER_IN_UPDATE`, or `TRANSFER_OUT_UPDATE`

| `content.event` | Business meaning | Primary record field |
|------|-------------|----------------------|
| `DEPOSIT_UPDATE` | Deposit status or balance change | `deposit` |
| `WITHDRAW_UPDATE` | Withdrawal status or balance change | `withdraw` |
| `TRANSFER_IN_UPDATE` | Internal transfer-in state change | `transferIn` |
| `TRANSFER_OUT_UPDATE` | Internal transfer-out state change | `transferOut` |

#### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `account` | Updated account summary after the asset change. |
| `collateral` | Updated collateral balances after funds arrive or leave. |
| `collateralTransaction` | Balance movement records generated by the asset flow. |
| `deposit` | Deposit records when `content.event` is `DEPOSIT_UPDATE`. |
| `withdraw` | Withdrawal records when `content.event` is `WITHDRAW_UPDATE`. |
| `transferIn` | Transfer-in records when `content.event` is `TRANSFER_IN_UPDATE`. |
| `transferOut` | Transfer-out records when `content.event` is `TRANSFER_OUT_UPDATE`. |

#### Response Example

The same envelope applies to all four asset events. The example below uses `TRANSFER_OUT_UPDATE`.

```json
{
  "type": "trade-event",
  "content": {
    "event": "TRANSFER_OUT_UPDATE",
    "version": 1005,
    "time": 1693208195000,
    "data": {
      "transferOut": [
        {
          "id": 730001,
          "coinId": 1,
          "amount": "20"
        }
      ],
      "collateral": [
        {
          "coinId": 1,
          "balance": "1130"
        }
      ],
      "collateralTransaction": [
        {
          "id": 740001,
          "amount": "-20"
        }
      ]
    }
  }
}
```

### Funding Settlement

This category describes periodic account changes caused by funding fee settlement for open positions.

#### Trigger Scenario

The server pushes this message when the system settles funding for eligible open positions.

#### Message Mapping

- `type`: `trade-event`
- `content.event`: `FUNDING_SETTLEMENT`

#### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `account` | Updated account summary after settlement. |
| `collateral` | Updated collateral balances after funding payment or funding income is applied. |
| `collateralTransaction` | Funding-related balance movement records. |
| `position` | Updated position state when settlement affects position-related values. |
| `positionTermList` | Updated term-related position data when applicable. |

#### Response Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "FUNDING_SETTLEMENT",
    "version": 1006,
    "time": 1693208200000,
    "data": {
      "collateral": [
        {
          "coinId": 1,
          "balance": "1128.5"
        }
      ],
      "collateralTransaction": [
        {
          "id": 740001,
          "amount": "-1.5"
        }
      ],
      "position": [
        {
          "contractId": 1000001,
          "fundingFee": "-1.5"
        }
      ]
    }
  }
}
```

## Error Response

Return an error when authentication fails, the client sends an invalid message, or the server cannot load the initial snapshot.

### Response Model

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Always `error` |
| `content` | object | Error payload |
| `time` | integer | Present when applicable |

The exact fields inside `content` depend on the error source, but they represent a structured error object returned by the gateway.

### Common Error Situations

- Invalid API key, passphrase, signature, or timestamp
- Invalid `accountId` or malformed query parameters
- Client sends an unsupported message type after the connection is established
- Snapshot retrieval fails immediately after connection establishment

### Error Example

```json
{
  "type": "error",
  "content": {
    "code": "GATEWAY_UNRECOGNIZED_MESSAGE",
    "msg": "unsupported private ws message"
  }
}
```

### Connection Behavior Notes

- If the client sends invalid messages repeatedly, the server may close the connection.
- If snapshot retrieval fails immediately after connection establishment, the server may close the connection and require the client to reconnect.
