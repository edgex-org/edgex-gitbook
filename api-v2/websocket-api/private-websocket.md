# Private WebSocket API

The private WebSocket endpoint is `wss://<ws-domain>/api/v1/private/ws`.

Private WebSocket uses the same authentication rules as the private HTTP APIs. No subscribe message is required. After authentication succeeds, the server sends `connected`, then the initial `Snapshot`, and then later account-scoped business updates.

For credential requirements and signature generation, see the [Authentication Guide](../authentication.md).

## Connect and Authenticate

### WebSocket URL

Use the private WebSocket endpoint:

`wss://<ws-domain>/api/v1/private/ws`

### WebSocket Query Parameters

The following query parameters **must** be included on the private WebSocket connection URL:

- `accountId` (integer, required): EdgeX account identifier
- `timestamp` (integer, required): Current millisecond timestamp

### WebSocket Authentication Headers

The following headers **must** be included in the private WebSocket handshake request:

- `X-edgeX-Api-Key` (string, required): Your API Key obtained from the EdgeX platform
- `X-edgeX-Passphrase` (string, required): Your API Passphrase set during API key creation
- `X-edgeX-Api-Timestamp` (string, required): Request timestamp in milliseconds
- `X-edgeX-Api-Signature` (string, required): HMAC-SHA256 signature of the request

Private WebSocket uses the same HMAC credential set and HMAC-SHA256 signing flow as private REST APIs. This page does not repeat the signing algorithm. Use the [Authentication Guide](../authentication.md) to generate `X-edgeX-Api-Signature`.

### Minimal Connection Request

```http
GET /api/v1/private/ws?accountId=724625476626153743&timestamp=1705720068228
X-edgeX-Api-Key: your-api-key
X-edgeX-Passphrase: your-api-passphrase
X-edgeX-Api-Signature: <signature generated as described in the Authentication Guide>
X-edgeX-Api-Timestamp: 1705720068228
```

## Heartbeat

The connection uses a bidirectional ping-pong heartbeat.

- The server sends `ping`.
- The client should reply with `pong` and echo the same `time`.
- The client can also send `ping` proactively to measure latency.

Server ping example:

```json
{
  "type": "ping",
  "time": "1773833240000"
}
```

Client pong example:

```json
{
  "type": "pong",
  "time": "1773833240000"
}
```

## Initial Snapshot

After authentication succeeds, the server first confirms the session and then pushes the current full account state.

Connection success example from a live session:

```json
{
  "sid": "371619ee-b099-751a-74b1-b46e98606322",
  "type": "connected",
  "time": "1773832863182"
}
```

Trimmed live snapshot excerpt with representative fields:

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 907,
    "time": 1773832863190,
    "accountId": 0,
    "data": {
      "account": [
        {
          "id": "729074578726322703",
          "userId": "600162890833461506",
          "clientAccountId": "main",
          "status": "NORMAL",
          "createdTime": "1773826958034",
          "updatedTime": "1773832101293"
        }
      ],
      "collateral": [
        {
          "coinId": "1000",
          "amount": "83.512690",
          "cumDepositAmount": "160.000000",
          "cumWithdrawAmount": "0",
          "cumTransferInAmount": "0",
          "cumTransferOutAmount": "0",
          "updatedTime": "1773832800161"
        }
      ],
      "position": [
        {
          "contractId": "10000002",
          "openSize": "0.03",
          "openValue": "69.600200",
          "fundingFee": "-0.000289",
          "updatedTime": "1773832690744"
        }
      ],
      "order": [
        {
          "id": "729107150223180559",
          "contractId": "10000002",
          "side": "SELL",
          "type": "STOP_MARKET",
          "status": "UNTRIGGERED",
          "updatedTime": "1773832690765"
        }
      ]
    }
  }
}
```

## Event Updates

All business pushes use the same top-level event envelope.

This page documents the stable envelope and representative business fields only. It does not attempt to enumerate the complete schema of every nested record under `account`, `collateral`, `position`, `order`, `deposit`, `withdraw`, `transferIn`, or `transferOut`.

For complete nested record details, use verified responses together with the corresponding gateway model as the source of truth.

### Message Envelope

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Top-level message family. Business pushes use `trade-event`. |
| `content.event` | string | Business event name such as `Snapshot`, `ORDER_UPDATE`, or `TRANSFER_OUT_UPDATE`. |
| `content.version` | integer | Event version for ordering incremental updates. |
| `content.time` | integer | Server timestamp in milliseconds for the business event. |
| `content.accountId` | integer | Present in live samples. Current sampled value is `0`, so clients should not treat it as the authoritative account identifier. |
| `content.data` | object | Business payload grouped by entity type. |
| `sid` | string | Present on the `connected` message, not on `trade-event` payloads. |
| `time` | string | Present on `connected`, `ping`, and `pong`. |

### Business Category Mapping

| Business Category | `content.event` |
|------|------|
| Account Snapshot | `Snapshot` |
| Trade Updates | `ORDER_UPDATE` |
| Asset Updates | `DEPOSIT_UPDATE`, `WITHDRAW_UPDATE`, `TRANSFER_IN_UPDATE`, `TRANSFER_OUT_UPDATE` |
| Funding Settlement | `FUNDING_SETTLEMENT` |

### Common `content.data` Groups

| Field | Description |
|------|-------------|
| `account` | Account summary records. |
| `collateral` | Current collateral balances. |
| `collateralTransaction` | Balance movement records. |
| `position` | Current position records. |
| `positionTransaction` | Position movement records. |
| `deposit` | Deposit records. |
| `withdraw` | Withdrawal records. |
| `transferIn` | Internal transfer-in records. |
| `transferOut` | Internal transfer-out records. |
| `order` | Order records. |
| `orderFillTransaction` | Fill records generated by matched orders. |

Not every event populates every group. Unaffected groups may be empty.

## Account Snapshot

### Trigger Scenario

The server pushes this message immediately after a successful authenticated connection. Clients should also treat the next `Snapshot` after reconnect as the new local baseline.

### Message Mapping

- `type`: `trade-event`
- `content.event`: `Snapshot`

### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `account` | Current account-level status and account settings. |
| `collateral` | Current collateral balances and cumulative balance counters. |
| `position` | Current open positions. |
| `order` | Current open or waiting orders. |

### Response Example

The example below is a trimmed live snapshot excerpt with representative fields only.

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 907,
    "time": 1773832863190,
    "accountId": 0,
    "data": {
      "account": [
        {
          "id": "729074578726322703",
          "userId": "600162890833461506",
          "clientAccountId": "main",
          "status": "NORMAL",
          "updatedTime": "1773832101293"
        }
      ],
      "collateral": [
        {
          "coinId": "1000",
          "amount": "83.512690",
          "cumDepositAmount": "160.000000",
          "cumTransferOutAmount": "0",
          "updatedTime": "1773832800161"
        }
      ],
      "position": [
        {
          "contractId": "10000002",
          "openSize": "0.03",
          "openValue": "69.600200",
          "fundingFee": "-0.000289",
          "updatedTime": "1773832690744"
        }
      ],
      "order": [
        {
          "id": "729107150223180559",
          "contractId": "10000002",
          "side": "SELL",
          "type": "STOP_MARKET",
          "status": "UNTRIGGERED",
          "updatedTime": "1773832690765"
        }
      ]
    }
  }
}
```

## Trade Updates

This page uses one trimmed live `ORDER_UPDATE` example as the primary trade update example.

### Trigger Scenario

The server pushes this category when an order is created, accepted, triggered, canceled, partially filled, or fully filled, and when that order changes positions or collateral.

### Message Mapping

- `type`: `trade-event`
- `content.event`: `ORDER_UPDATE`

### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `order` | The updated order record. |
| `orderFillTransaction` | Fill records created by matching. |
| `position` | Updated position state after the order changes exposure. |
| `positionTransaction` | Position movement records related to the order result. |
| `collateral` | Updated collateral balances after realized PnL, fees, or margin changes. |
| `collateralTransaction` | Collateral movement records related to the order lifecycle. |
| `account` | Optional account summary updates. |

### Response Example

The example below is a trimmed live `ORDER_UPDATE` excerpt with representative fields only.

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": 916,
    "time": 1773833120269,
    "accountId": 0,
    "data": {
      "order": [
        {
          "id": "729107150223180559",
          "contractId": "10000002",
          "side": "SELL",
          "type": "STOP_MARKET",
          "status": "FILLED",
          "triggerPrice": "2310.00",
          "cumMatchSize": "0.02",
          "cumMatchValue": "46.1952",
          "cumMatchFee": "0.017554",
          "updatedTime": "1773833120256"
        }
      ],
      "orderFillTransaction": [
        {
          "id": "729108951626416911",
          "orderId": "729107150223180559",
          "fillSize": "0.02",
          "fillValue": "46.1952",
          "fillFee": "0.017554",
          "fillPrice": "2309.76",
          "direction": "TAKER",
          "matchTime": "1773833120252"
        }
      ],
      "position": [
        {
          "contractId": "10000002",
          "openSize": "0.01",
          "openValue": "23.200067",
          "fundingFee": "-0.000096",
          "updatedTime": "1773833120269"
        }
      ],
      "collateral": [
        {
          "coinId": "1000",
          "amount": "129.690336",
          "cumPositionSellAmount": "46.1952",
          "cumFillFeeAmount": "-0.046603",
          "updatedTime": "1773833120269"
        }
      ]
    }
  }
}
```

## Asset Updates

This category groups deposit, withdrawal, and internal transfer state changes under one business-facing section.

### Trigger Scenario

The server pushes this category when an asset movement changes account balances or when the related asset-flow record changes status.

### Message Mapping

- `type`: `trade-event`
- `content.event`: one of `DEPOSIT_UPDATE`, `WITHDRAW_UPDATE`, `TRANSFER_IN_UPDATE`, or `TRANSFER_OUT_UPDATE`

| `content.event` | Business Meaning | Primary Record Group |
|------|------|------|
| `DEPOSIT_UPDATE` | Deposit state or balance change | `deposit` |
| `WITHDRAW_UPDATE` | Withdrawal state or balance change | `withdraw` |
| `TRANSFER_IN_UPDATE` | Internal transfer-in state or balance change | `transferIn` |
| `TRANSFER_OUT_UPDATE` | Internal transfer-out state or balance change | `transferOut` |

### Common `content.data` Fields

| Field | Description |
|------|-------------|
| `deposit` | Deposit records when `content.event` is `DEPOSIT_UPDATE`. |
| `withdraw` | Withdrawal records when `content.event` is `WITHDRAW_UPDATE`. |
| `transferIn` | Transfer-in records when `content.event` is `TRANSFER_IN_UPDATE`. |
| `transferOut` | Transfer-out records when `content.event` is `TRANSFER_OUT_UPDATE`. |
| `collateral` | Updated collateral balances after funds are credited or debited. |
| `collateralTransaction` | Balance movement records created by the asset flow. |
| `account` | Optional account summary updates. |

### Response Example

The example below is a trimmed live `TRANSFER_OUT_UPDATE` excerpt with representative fields only.

```json
{
  "type": "trade-event",
  "content": {
    "event": "TRANSFER_OUT_UPDATE",
    "version": 920,
    "time": 1773833140579,
    "accountId": 0,
    "data": {
      "transferOut": [
        {
          "id": "729109036640764687",
          "accountId": "729074578726322703",
          "coinId": "1000",
          "amount": "1.000000",
          "receiverAccountId": "729108352151323326",
          "clientTransferId": "870080323493747",
          "transferReason": "USER_TRANSFER",
          "status": "SUCCESS_CENSOR_SUCCESS",
          "receiverTransferInId": "729109036745622206",
          "collateralTransactionId": "729109036863062799",
          "createdTime": "1773833140526",
          "updatedTime": "1773833140579"
        }
      ],
      "collateral": [
        {
          "coinId": "1000",
          "amount": "128.690336",
          "cumTransferOutAmount": "-1.000000",
          "updatedTime": "1773833140579"
        }
      ],
      "collateralTransaction": [
        {
          "id": "729109036863062799",
          "type": "TRANSFER_OUT",
          "deltaAmount": "-1.000000",
          "beforeAmount": "129.690336",
          "transferOutId": "729109036640764687",
          "censorStatus": "CENSOR_SUCCESS",
          "createdTime": "1773833140575",
          "updatedTime": "1773833140579"
        }
      ]
    }
  }
}
```

## Funding Settlement

Funding settlement is a dedicated business category in the private event set.

### Trigger Scenario

The server pushes this category when the system settles funding fees for eligible open positions.

### Message Mapping

- `type`: `trade-event`
- `content.event`: `FUNDING_SETTLEMENT`

This page does not attempt to enumerate the nested settlement record model here. Use verified responses and the corresponding gateway model when documenting settlement-specific fields.
