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
- After authentication succeeds, the server automatically pushes all account-related events for the authenticated account.
- The first business message is the initial `Snapshot`.
- After the initial snapshot, the server continues to push incremental updates.
- The client is expected to handle heartbeat messages during the session.

## Common Usage Flow

All private account-event streams follow the same connection lifecycle:

1. Build the connection URL with `accountId` and `timestamp`.
2. Sign the request with the same HMAC-SHA256 logic used by private HTTP APIs.
3. Connect to `wss://<ws-domain>/api/v1/private/ws`.
4. Receive `connected`.
5. Receive the initial `Snapshot`.
6. Continue processing incremental updates.
7. Respond to `ping` with `pong`.
8. Reconnect and rebuild local state from a new `Snapshot` if the connection is closed.

## How to Read a Private WebSocket Message

There are three important concepts in private WebSocket messages:

- `type` is the top-level message category, such as `connected`, `trade-event`, `assets-event`, `ping`, or `error`.
- `content.event` is the business event inside a push message, such as `Snapshot`, `ORDER_UPDATE`, or `FUNDING_SETTLEMENT`.
- `content.data` is the payload grouped by entity type, such as `account`, `position`, `order`, `deposit`, or `transferOut`.

In practice:

- `type` tells you which message family you received.
- `content.event` tells you which business event happened.
- `content.data` tells you which business entities changed in that event.

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

#### Response Sequence

After authentication succeeds, the server returns messages in this order:

1. `connected`
2. initial business snapshot
3. later incremental updates when account-related data changes

#### Connection Success Example

```json
{
  sid: session-id-string,
  type: connected,
  time: 1693208170000
}
```

### Message Envelope

#### Top-Level Envelope

| Field | Type | Description |
|------|------|-------------|
| `sid` | string | Session identifier assigned by the server |
| `type` | string | Top-level message type |
| `content` | object | Message-specific payload |
| `time` | string | Timestamp used by heartbeat messages |

#### Top-level `type` Values

| `type` | Meaning |
|-------|---------|
| `connected` | Connection established |
| `trade-event` | Trade-related and account-state push |
| `assets-event` | Asset-related push |
| `ping` | Server heartbeat |
| `pong` | Heartbeat response |
| `error` | Authentication error, invalid client message, or business error |

#### `content.data` Field Rules

- `content.data` includes only the fields relevant to the current event.
- Fields that are not affected by the current event may be omitted or empty.
- The initial `Snapshot` is the baseline payload used to initialize local state.
- Later event updates are incremental messages applied after the snapshot.

## Heartbeat

The private WebSocket connection uses a bidirectional heartbeat mechanism to keep the session alive and measure latency.

### Server Ping

After the connection is established, the server periodically sends a `ping` message.

```json
{
  type: ping,
  time: 1693208170000
}
```

When the client receives this message, it should return a `pong` message with the same `time` value.

```json
{
  type: pong,
  time: 1693208170000
}
```

### Client Ping

The client can also send a `ping` message to measure round-trip latency.

```json
{
  type: ping,
  time: 1693208170000
}
```

The server responds immediately with a `pong` message and echoes the same `time` value.

```json
{
  type: pong,
  time: 1693208170000
}
```

## Initial Snapshot

After a successful connection and authentication, the server first pushes an initial snapshot message. This snapshot provides the current account state and acts as the baseline for all later incremental updates.

### Message Mapping

- `type`: `trade-event`
- `content.event`: `Snapshot`

### Response Model

```json
{
  type: trade-event,
  content: {
    event: Snapshot,
    version: 1000,
    time: 1693208170000,
    accountId: 724625476626153743,
    data: {
      account: [],
      collateral: [],
      position: [],
      order: []
    }
  }
}
```

### Field Details

| Field | Type | Description |
| --- | --- | --- |
| `type` | string | Message type. The initial snapshot is sent as `trade-event`. |
| `content.event` | string | Always `Snapshot` for the initial full-state payload. |
| `content.version` | integer | Snapshot version. Later incremental events continue from this version sequence. |
| `content.time` | integer | Server timestamp in milliseconds. |
| `content.accountId` | integer | EdgeX account identifier. |
| `content.data.account` | array | Current account state. |
| `content.data.collateral` | array | Current collateral balances. |
| `content.data.position` | array | Current open positions. |
| `content.data.order` | array | Current active orders. |

### Relationship to Later Updates

The initial snapshot is a full-state payload. Subsequent event updates are incremental messages that describe state changes after this baseline.

## Event Updates

After the initial snapshot, the server pushes incremental updates grouped by business purpose. This guide organizes them into four user-facing categories: `Account Snapshot`, `Trade Updates`, `Asset Updates`, and `Funding Settlement`.

### Account Snapshot

This category refers to the initial full account state delivered immediately after connection.

- `type`: `trade-event`
- `content.event`: `Snapshot`
- purpose: establish the baseline account, collateral, position, and active order state before incremental processing begins

### Trade Updates

Trade updates describe order lifecycle changes and other trade-related account changes caused by trading activity.

#### Order Update

##### Trigger Scenario

This event is pushed when an order is created, updated, partially filled, fully filled, or canceled.

##### Message Mapping

- `type`: `trade-event`
- `content.event`: `ORDER_UPDATE`

##### Common `content.data` Fields

| Field | Description |
| --- | --- |
| `account` | Updated account summary after the order change. |
| `collateral` | Updated collateral balances if margin or balance changes. |
| `collateralTransaction` | Collateral transaction records related to the order lifecycle. |
| `position` | Updated position state if the order opens, reduces, or closes a position. |
| `positionTransaction` | Position change records created by fills or position transitions. |
| `order` | Updated order records. |
| `orderFillTransaction` | Fill records when the order is matched. |
| `positionTermList` | Updated position term information when applicable. |

##### Response Example

```json
{
  type: trade-event,
  content: {
    event: ORDER_UPDATE,
    version: 1001,
    time: 1693208175000,
    accountId: 724625476626153743,
    data: {
      order: [
        {
          id: 900001,
          contractId: 1000001,
          status: FILLED
        }
      ],
      orderFillTransaction: [
        {
          id: 800001,
          orderId: 900001
        }
      ],
      position: [
        {
          contractId: 1000001,
          size: 1
        }
      ]
    }
  }
}
```

### Asset Updates

Asset updates describe balance-related changes caused by deposit, withdrawal, and transfer flows.

#### Deposit Update

##### Trigger Scenario

This event is pushed when a deposit-related state changes and the account balance view needs to be updated.

##### Message Mapping

- `type`: `trade-event`
- `content.event`: `DEPOSIT_UPDATE`

##### Common `content.data` Fields

| Field | Description |
| --- | --- |
| `account` | Updated account summary after the deposit change. |
| `collateral` | Updated collateral balances after the deposit is reflected. |
| `deposit` | Deposit records related to the change. |
| `collateralTransaction` | Balance movement records generated by the deposit flow. |

##### Response Example

```json
{
  type: trade-event,
  content: {
    event: DEPOSIT_UPDATE,
    version: 1002,
    time: 1693208180000,
    accountId: 724625476626153743,
    data: {
      deposit: [
        {
          id: 700001,
          coinId: 1,
          status: SUCCESS_CENSOR_SUCCESS
        }
      ],
      collateral: [
        {
          coinId: 1,
          balance: 1200
        }
      ]
    }
  }
}
```

#### Withdraw Update

##### Trigger Scenario

This event is pushed when a withdrawal state changes and the account balance view needs to be updated.

##### Message Mapping

- `type`: `trade-event`
- `content.event`: `WITHDRAW_UPDATE`

##### Common `content.data` Fields

| Field | Description |
| --- | --- |
| `account` | Updated account summary after the withdrawal change. |
| `collateral` | Updated collateral balances after the withdrawal is reflected. |
| `withdraw` | Withdrawal records related to the change. |
| `collateralTransaction` | Balance movement records generated by the withdrawal flow. |

##### Response Example

```json
{
  type: trade-event,
  content: {
    event: WITHDRAW_UPDATE,
    version: 1003,
    time: 1693208185000,
    accountId: 724625476626153743,
    data: {
      withdraw: [
        {
          id: 710001,
          coinId: 1,
          status: PENDING
        }
      ],
      collateral: [
        {
          coinId: 1,
          balance: 1100
        }
      ]
    }
  }
}
```

#### Transfer In Update

##### Trigger Scenario

This event is pushed when an internal transfer-in changes the target account balance or account state.

##### Message Mapping

- `type`: `trade-event`
- `content.event`: `TRANSFER_IN_UPDATE`

##### Common `content.data` Fields

| Field | Description |
| --- | --- |
| `account` | Updated account summary after the transfer-in change. |
| `collateral` | Updated collateral balances after funds arrive. |
| `transferIn` | Transfer-in records related to the change. |
| `collateralTransaction` | Balance movement records generated by the transfer. |

##### Response Example

```json
{
  type: trade-event,
  content: {
    event: TRANSFER_IN_UPDATE,
    version: 1004,
    time: 1693208190000,
    accountId: 724625476626153743,
    data: {
      transferIn: [
        {
          id: 720001,
          coinId: 1,
          amount: 50
        }
      ],
      collateral: [
        {
          coinId: 1,
          balance: 1150
        }
      ]
    }
  }
}
```

#### Transfer Out Update

##### Trigger Scenario

This event is pushed when an internal transfer-out changes the source account balance or account state.

##### Message Mapping

- `type`: `trade-event`
- `content.event`: `TRANSFER_OUT_UPDATE`

##### Common `content.data` Fields

| Field | Description |
| --- | --- |
| `account` | Updated account summary after the transfer-out change. |
| `collateral` | Updated collateral balances after funds leave. |
| `transferOut` | Transfer-out records related to the change. |
| `collateralTransaction` | Balance movement records generated by the transfer. |

##### Response Example

```json
{
  type: trade-event,
  content: {
    event: TRANSFER_OUT_UPDATE,
    version: 1005,
    time: 1693208195000,
    accountId: 724625476626153743,
    data: {
      transferOut: [
        {
          id: 730001,
          coinId: 1,
          amount: 20
        }
      ],
      collateral: [
        {
          coinId: 1,
          balance: 1130
        }
      ]
    }
  }
}
```

### Funding Settlement

Funding settlement describes periodic account changes caused by funding fee settlement for open positions.

#### Trigger Scenario

This event is pushed when the system settles funding for eligible open positions.

#### Message Mapping

- `type`: `trade-event`
- `content.event`: `FUNDING_SETTLEMENT`

#### Common `content.data` Fields

| Field | Description |
| --- | --- |
| `account` | Updated account summary after settlement. |
| `collateral` | Updated collateral balances after funding payment or funding income is applied. |
| `collateralTransaction` | Funding-related balance movement records. |
| `position` | Updated position state when settlement affects position-related values. |
| `positionTermList` | Updated term-related position data when applicable. |

#### Response Example

```json
{
  type: trade-event,
  content: {
    event: FUNDING_SETTLEMENT,
    version: 1006,
    time: 1693208200000,
    accountId: 724625476626153743,
    data: {
      collateral: [
        {
          coinId: 1,
          balance: 1128.5
        }
      ],
      collateralTransaction: [
        {
          id: 740001,
          amount: -1.5
        }
      ]
    }
  }
}
```

## Error Response

Return an error when authentication fails, the client sends an invalid message, or the server encounters a business error while processing the session.

### Response Model

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Always `error` |
| `content` | object | Error payload |
| `time` | string | Present when applicable |

The exact fields inside `content` depend on the error source, but they represent a structured error object returned by the gateway.

### Common Error Situations

- Invalid API key, passphrase, signature, or timestamp.
- Invalid `accountId` or malformed query parameters.
- Client sends an unsupported message type after the connection is established.
- Snapshot retrieval fails after connection establishment.

### Error Example

```json
{
  type: error,
  content: {
    code: GATEWAY_UNRECOGNIZED_MESSAGE,
    msg: unsupported private ws message
  }
}
```

### Connection Behavior Notes

- If the client sends invalid messages repeatedly, the server may close the connection.
- If snapshot retrieval fails immediately after connection establishment, the server may close the connection and require the client to reconnect.
EOF
sed -n  1,260p api-v2/websocket-api/private-websocket.md
