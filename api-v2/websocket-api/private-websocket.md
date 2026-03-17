# Private WebSocket API

## Overview

The Private WebSocket API provides real-time updates for user account information, including trading events and account state changes. This interface automatically pushes data to connected clients without requiring explicit subscriptions.

**Key Features:**
- **Auto-push mechanism**: Data is automatically pushed after successful connection; no subscription required
- **Authentication required**: Must authenticate before receiving private data
- **Real-time updates**: Instant notifications for account, position, order, and collateral changes
- **Bidirectional heartbeat**: Both server and client can initiate ping-pong for connection health monitoring

## Connection URL

```
wss://[domain]/api/v1/private/ws
```

**Note:** The WebSocket endpoint continues to use the `/api/v1/` path for backward compatibility.

## Authentication

Private WebSocket connections require **HMAC-SHA256 authentication**, consistent with private HTTP APIs.

### Authentication Method 1: App/API (Custom Headers)

This is the primary method used by SDK and server-side clients.

#### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `query.accountId` | string(int64) | true | Account ID in WebSocket URL query string |
| `query.timestamp` | string(int64) | true | Millisecond timestamp in WebSocket URL query string |

#### Request Headers

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `X-edgeX-Api-Key` | string | true | API Key |
| `X-edgeX-Passphrase` | string | true | API Passphrase |
| `X-edgeX-Api-Signature` | string | true | HMAC-SHA256 signature |
| `X-edgeX-Api-Timestamp` | string(int64) | true | Millisecond timestamp in request header (should match `query.timestamp`) |

#### Signature Notes

- WebSocket handshake is `GET`
- Request URI for signature: `/api/v1/private/ws`
- Request body for signature: sorted query string (for example: `accountId=xxx&timestamp=yyy`)
- Signature algorithm: same HMAC flow as [Authentication Guide](../authentication.md)

#### Request Example

```http
GET /api/v1/private/ws?accountId=724625476626153743&timestamp=1705720068228
X-edgeX-Api-Key: your-api-key
X-edgeX-Passphrase: your-api-passphrase
X-edgeX-Api-Signature: your-hmac-signature
X-edgeX-Api-Timestamp: 1705720068228
```

**Important Notes:**
- WebSocket is a GET request; there is no request body to sign
- Authentication uses HMAC credentials (`API Key`, `Passphrase`, `API Secret`), not L2 signer key signatures
- `query.timestamp` and `X-edgeX-Api-Timestamp` should be current and consistent

## Heartbeat Mechanism

The Private WebSocket implements a bidirectional ping-pong mechanism for connection health monitoring and latency measurement.

### Server Ping (Heartbeat)

The server sends periodic Ping messages to verify client connectivity.

**Server Ping Message:**
```json
{
  "type": "ping",
  "time": "1693208170000"
}
```

**Client Must Respond with Pong:**
```json
{
  "type": "pong",
  "time": "1693208170000"
}
```

**Timeout Policy:**
- If the server doesn't receive a Pong response after **5 consecutive Pings**, the connection will be terminated

### Client Ping (Latency Measurement)

Clients can also initiate Ping messages to measure round-trip latency.

**Client Ping Message:**
```json
{
  "type": "ping",
  "time": "1693208170000"
}
```

**Server Responds with Pong:**
```json
{
  "type": "pong",
  "time": "1693208170000"
}
```

The `time` field in the Pong response matches the `time` field from the client's Ping, allowing latency calculation.

## Message Format

All messages from the Private WebSocket follow a consistent structure.

### Message Structure

```json
{
  "type": "trade-event",
  "content": {
    "event": "ACCOUNT_UPDATE",
    "version": "1000",
    "data": {
      "account": [],
      "collateral": [],
      "collateralTransaction": [],
      "position": [],
      "positionTransaction": [],
      "deposit": [],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Message Types

| Type | Description |
|------|-------------|
| `trade-event` | Trading-related updates (account, position, order changes) |
| `ping` | Heartbeat message from server or client |
| `pong` | Response to ping message |
| `error` | Error message from server |

### Event Types

The `event` field in `trade-event` messages indicates what triggered the data update:

| Event | Description |
|-------|-------------|
| `Snapshot` | Initial snapshot of account state after connection |
| `ACCOUNT_UPDATE` | Account information updated |
| `DEPOSIT_UPDATE` | Deposit record updated |
| `WITHDRAW_UPDATE` | Withdrawal record updated |
| `TRANSFER_IN_UPDATE` | Transfer-in record updated |
| `TRANSFER_OUT_UPDATE` | Transfer-out record updated |
| `ORDER_UPDATE` | Order status or details updated |
| `FORCE_WITHDRAW_UPDATE` | Forced withdrawal event |
| `FORCE_TRADE_UPDATE` | Forced trade event (liquidation/deleveraging) |
| `FUNDING_SETTLEMENT` | Funding fee settlement |
| `ORDER_FILL_FEE_INCOME` | Order fill fee income event |
| `START_LIQUIDATING` | Account liquidation started |
| `FINISH_LIQUIDATING` | Account liquidation finished |
| `UNRECOGNIZED` | Unrecognized event type |

## Minimal Schema Reference

### PrivateWsEnvelope

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Top-level message type such as `trade-event`, `ping`, `pong`, or `error` |
| `content` | object or null | Event payload for `trade-event` or error payload for `error` |
| `time` | string(int64) or null | Timestamp used by heartbeat messages |

### PrivateWsHeartbeat

| Field | Type | Description |
|------|------|-------------|
| `type` | string | `ping` or `pong` |
| `time` | string(int64) | Heartbeat timestamp echoed between client and server |

### PrivateWsError

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Always `error` |
| `content.code` | string | Machine-readable error code |
| `content.msg` | string | Human-readable error message |
| `content.details` | string or null | Optional additional details |

### TradeEventDataSummary

| Array Field | Item Meaning |
|------------|--------------|
| `account` | Account-level updates |
| `collateral` | Collateral balance updates |
| `collateralTransaction` | Collateral transaction updates |
| `position` | Position snapshots or changes |
| `positionTransaction` | Position transaction updates |
| `deposit` | Deposit record updates |
| `withdraw` | Withdrawal record updates |
| `transferIn` | Transfer-in updates |
| `transferOut` | Transfer-out updates |
| `order` | Order updates |
| `orderFillTransaction` | Fill-level execution updates |

**Event usage note:** `Snapshot` may contain multiple populated arrays, while update events such as `ORDER_UPDATE` or `ACCOUNT_UPDATE` usually populate only the arrays related to the changed entities.

## Data Object Structure

The `data` object contains arrays of various entities. Each array is populated based on the event type. Not all arrays will be present or populated in every message.

### Account Array

Contains account information updates.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Account ID |
| `userId` | string(int64) | User ID - Owner of this account |
| `ethAddress` | string | Ethereum address associated with this account |
| `l2Key` | string | L2 public key for signing transactions |
| `l2KeyYCoordinate` | string | Y-coordinate of L2 public key |
| `status` | string | Account status: `NORMAL`, `LIQUIDATING`, etc. |
| `isLiquidating` | boolean | Whether the account is currently being liquidated |
| `createdTime` | string(int64) | Account creation timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |

### Collateral Array

Contains collateral (margin) information for different coins.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `userId` | string(int64) | User ID - Owner of this collateral |
| `accountId` | string(int64) | Account ID - Parent account |
| `coinId` | string(int64) | Collateral coin ID (e.g., 1000 for USDT) |
| `amount` | string(decimal) | Current collateral amount |
| `legacyAmount` | string(decimal) | Legacy amount (for historical compatibility) |
| `totalAmount` | string(decimal) | Total collateral amount including locked funds |
| `availableAmount` | string(decimal) | Available collateral amount (can be used for trading) |
| `lockedAmount` | string(decimal) | Locked collateral amount (reserved for open orders/positions) |
| `createdTime` | string(int64) | Creation timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |

### Collateral Transaction Array

Contains transaction records that modify collateral balances.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Transaction ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Account ID |
| `coinId` | string(int64) | Collateral coin ID |
| `contractId` | string(int64) | Related contract ID (if applicable) |
| `orderId` | string(int64) | Related order ID (if transaction is order-related) |
| `type` | string | Transaction type: `DEPOSIT`, `WITHDRAW`, `TRANSFER_IN`, `TRANSFER_OUT`, `OPEN_POSITION_FEE`, `CLOSE_POSITION_FEE`, `FUNDING_FEE`, etc. |
| `deltaAmount` | string(decimal) | Change in collateral amount (positive for credit, negative for debit) |
| `fee` | string(decimal) | Transaction fee (if applicable) |
| `censorStatus` | string | Censorship status: `CENSOR_SUCCESS`, `CENSOR_FAILURE` |
| `createdTime` | string(int64) | Transaction timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |

### Position Array

Contains current position information for each contract.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `userId` | string(int64) | User ID - Owner of this position |
| `accountId` | string(int64) | Account ID - Parent account |
| `coinId` | string(int64) | Collateral coin ID |
| `contractId` | string(int64) | Contract ID (e.g., 10000001 for BTC-PERP) |
| `openSize` | string(decimal) | Current position size (positive for long, negative for short) |
| `openValue` | string(decimal) | Current position value (in collateral currency) |
| `openFee` | string(decimal) | Accumulated opening fees |
| `fundingFee` | string(decimal) | Accumulated funding fees |
| `longTermCount` | integer | Number of long position terms |
| `shortTermCount` | integer | Number of short position terms |
| `longOpenSize` | string(decimal) | Total long position size |
| `longOpenValue` | string(decimal) | Total long position value |
| `longOpenFee` | string(decimal) | Total long position opening fees |
| `longFundingFee` | string(decimal) | Total long funding fees |
| `shortOpenSize` | string(decimal) | Total short position size (absolute value) |
| `shortOpenValue` | string(decimal) | Total short position value (absolute value) |
| `shortOpenFee` | string(decimal) | Total short position opening fees |
| `shortFundingFee` | string(decimal) | Total short funding fees |
| `createdTime` | string(int64) | Position creation timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |

### Position Transaction Array

Contains transaction records that modify position states (opens, closes, funding settlements).

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Position transaction ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Account ID |
| `coinId` | string(int64) | Collateral coin ID |
| `contractId` | string(int64) | Contract ID |
| `type` | string | Transaction type: `SELL_POSITION`, `BUY_POSITION`, `SETTLE_FUNDING_FEE`, `LIQUIDATE`, etc. |
| `deltaOpenSize` | string(decimal) | Change in open position size |
| `deltaOpenValue` | string(decimal) | Change in open position value |
| `deltaOpenFee` | string(decimal) | Change in open position fees |
| `deltaFundingFee` | string(decimal) | Change in funding fees |
| `fillCloseSize` | string(decimal) | Size closed by this fill |
| `fillCloseValue` | string(decimal) | Value closed by this fill |
| `fillCloseFee` | string(decimal) | Fee for closing position |
| `fillOpenSize` | string(decimal) | Size opened by this fill |
| `fillOpenValue` | string(decimal) | Value opened by this fill |
| `fillOpenFee` | string(decimal) | Fee for opening position |
| `fillPrice` | string(decimal) | Fill execution price |
| `realizePnl` | string(decimal) | Realized profit/loss from this transaction |
| `isLiquidate` | boolean | Whether this is a liquidation transaction |
| `isDeleverage` | boolean | Whether this is a deleveraging transaction |
| `orderId` | string(int64) | Related order ID |
| `orderFillTransactionId` | string(int64) | Related order fill transaction ID |
| `collateralTransactionId` | string(int64) | Related collateral transaction ID |
| `censorStatus` | string | Censorship status |
| `createdTime` | string(int64) | Transaction timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |

### Deposit Array

Contains deposit records.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Deposit record ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Account ID |
| `coinId` | string(int64) | Deposited coin ID |
| `amount` | string(decimal) | Deposit amount |
| `ethAddress` | string | Ethereum address from which deposit was made |
| `status` | string | Deposit status: `PENDING`, `SUCCESS`, `FAILED`, etc. |
| `l1TxHash` | string | Layer 1 transaction hash |
| `l1ConfirmedTime` | string(int64) | L1 confirmation timestamp |
| `createdTime` | string(int64) | Deposit initiation timestamp |
| `updatedTime` | string(int64) | Last update timestamp |

### Withdraw Array

Contains withdrawal records.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Withdrawal record ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Account ID |
| `coinId` | string(int64) | Withdrawn coin ID |
| `amount` | string(decimal) | Withdrawal amount |
| `ethAddress` | string | Ethereum address to which withdrawal is sent |
| `status` | string | Withdrawal status: `PENDING`, `SUCCESS`, `FAILED`, `PENDING_L2_APPROVING`, `PENDING_L1_CONFIRMING`, etc. |
| `l2Signature` | object | L2 signature information |
| `l1TxHash` | string | Layer 1 transaction hash |
| `l1ConfirmedTime` | string(int64) | L1 confirmation timestamp |
| `createdTime` | string(int64) | Withdrawal initiation timestamp |
| `updatedTime` | string(int64) | Last update timestamp |

### Transfer In Array

Contains incoming transfer records (from other accounts or platforms).

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Transfer record ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Receiving account ID |
| `coinId` | string(int64) | Transferred coin ID |
| `amount` | string(decimal) | Transfer amount |
| `fromAccountId` | string(int64) | Source account ID |
| `status` | string | Transfer status |
| `createdTime` | string(int64) | Transfer timestamp |
| `updatedTime` | string(int64) | Last update timestamp |

### Transfer Out Array

Contains outgoing transfer records (to other accounts or platforms).

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Transfer record ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Sending account ID |
| `coinId` | string(int64) | Transferred coin ID |
| `amount` | string(decimal) | Transfer amount |
| `toAccountId` | string(int64) | Destination account ID |
| `status` | string | Transfer status |
| `createdTime` | string(int64) | Transfer timestamp |
| `updatedTime` | string(int64) | Last update timestamp |

### Order Array

Contains order information and status updates.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Order ID |
| `userId` | string(int64) | User ID - Order owner |
| `accountId` | string(int64) | Account ID - Parent account |
| `coinId` | string(int64) | Collateral coin ID |
| `contractId` | string(int64) | Contract ID for this order |
| `side` | string | Order side: `BUY` or `SELL` |
| `price` | string(decimal) | Order price (limit orders) |
| `size` | string(decimal) | Order size (quantity) |
| `clientOrderId` | string | Client-provided order ID (optional, for idempotency) |
| `type` | string | Order type: `LIMIT`, `MARKET`, `STOP_LIMIT`, `STOP_MARKET`, `TAKE_PROFIT_LIMIT`, `TAKE_PROFIT_MARKET` |
| `timeInForce` | string | Time in force: `GOOD_TIL_CANCEL`, `IMMEDIATE_OR_CANCEL`, `FILL_OR_KILL`, `POST_ONLY` |
| `reduceOnly` | boolean | Whether this order only reduces existing position |
| `triggerPrice` | string(decimal) | Trigger price (for stop/take-profit orders) |
| `triggerPriceType` | string | Trigger price type: `LAST_PRICE`, `INDEX_PRICE`, `MARK_PRICE` |
| `status` | string | Order status: `PENDING`, `OPEN`, `PARTIALLY_FILLED`, `FILLED`, `CANCELLED`, `REJECTED`, `EXPIRED` |
| `filledSize` | string(decimal) | Cumulative filled size |
| `filledValue` | string(decimal) | Cumulative filled value |
| `filledFee` | string(decimal) | Cumulative trading fees |
| `averageFilledPrice` | string(decimal) | Average fill price |
| `isLiquidate` | boolean | Whether this is a liquidation order |
| `isDeleverage` | boolean | Whether this is a deleveraging order |
| `isPositionTpsl` | boolean | Whether this is a position TP/SL order |
| `censorStatus` | string | Censorship status: `CENSOR_SUCCESS`, `CENSOR_FAILURE` |
| `createdTime` | string(int64) | Order creation timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |
| `expireTime` | string(int64) | Order expiration timestamp |

### Order Fill Transaction Array

Contains individual trade execution records (fills).

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string(int64) | Fill transaction ID |
| `userId` | string(int64) | User ID |
| `accountId` | string(int64) | Account ID |
| `coinId` | string(int64) | Collateral coin ID |
| `contractId` | string(int64) | Contract ID |
| `orderId` | string(int64) | Parent order ID |
| `side` | string | Trade side: `BUY` or `SELL` |
| `price` | string(decimal) | Execution price |
| `size` | string(decimal) | Executed size |
| `fee` | string(decimal) | Trading fee for this fill |
| `feeRate` | string(decimal) | Fee rate applied |
| `isMaker` | boolean | Whether this fill was a maker trade (provides liquidity) |
| `isTaker` | boolean | Whether this fill was a taker trade (takes liquidity) |
| `positionTransactionId` | string(int64) | Related position transaction ID |
| `collateralTransactionId` | string(int64) | Related collateral transaction ID |
| `createdTime` | string(int64) | Fill execution timestamp (milliseconds) |
| `updatedTime` | string(int64) | Last update timestamp (milliseconds) |

## Example Messages

### Connection Established - Initial Snapshot

When you first connect and authenticate, you receive a `Snapshot` event with the current state of your account.

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": "1000",
    "data": {
      "account": [
        {
          "id": "543429922991899150",
          "userId": "543429922866069763",
          "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
          "status": "NORMAL",
          "isLiquidating": false,
          "createdTime": "1730204434094",
          "updatedTime": "1733993378059"
        }
      ],
      "collateral": [
        {
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "amount": "100.500000",
          "totalAmount": "100.500000",
          "availableAmount": "95.000000",
          "lockedAmount": "5.500000"
        }
      ],
      "position": [
        {
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "openSize": "0.005",
          "openValue": "484.066000",
          "openFee": "-0.242033",
          "fundingFee": "0.000000"
        }
      ],
      "order": [
        {
          "id": "564814927928230158",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "side": "BUY",
          "price": "97393.8",
          "size": "0.001",
          "type": "LIMIT",
          "timeInForce": "GOOD_TIL_CANCEL",
          "status": "OPEN",
          "filledSize": "0.000",
          "createdTime": "1734662372560",
          "updatedTime": "1734662372575"
        }
      ],
      "collateralTransaction": [],
      "positionTransaction": [],
      "deposit": [],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "orderFillTransaction": []
    }
  }
}
```

### Account Update Event

Triggered when account information changes (e.g., status change, configuration update).

```json
{
  "type": "trade-event",
  "content": {
    "event": "ACCOUNT_UPDATE",
    "version": "1001",
    "data": {
      "account": [
        {
          "id": "543429922991899150",
          "userId": "543429922866069763",
          "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
          "status": "NORMAL",
          "isLiquidating": false,
          "createdTime": "1730204434094",
          "updatedTime": "1734662500123"
        }
      ],
      "collateral": [],
      "collateralTransaction": [],
      "position": [],
      "positionTransaction": [],
      "deposit": [],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Order Update Event

Triggered when an order is created, updated, filled, cancelled, or rejected.

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": "1002",
    "data": {
      "account": [],
      "collateral": [],
      "collateralTransaction": [],
      "position": [],
      "positionTransaction": [],
      "deposit": [],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "order": [
        {
          "id": "564814927928230158",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "side": "BUY",
          "price": "97393.8",
          "size": "0.001",
          "clientOrderId": "21163368294502694",
          "type": "LIMIT",
          "timeInForce": "GOOD_TIL_CANCEL",
          "reduceOnly": false,
          "triggerPrice": "",
          "triggerPriceType": "LAST_PRICE",
          "status": "FILLED",
          "filledSize": "0.001",
          "filledValue": "97.3938",
          "filledFee": "0.048697",
          "averageFilledPrice": "97393.8",
          "isLiquidate": false,
          "isDeleverage": false,
          "isPositionTpsl": false,
          "censorStatus": "CENSOR_SUCCESS",
          "createdTime": "1734662372560",
          "updatedTime": "1734662380125",
          "expireTime": "1736476772359"
        }
      ],
      "orderFillTransaction": [
        {
          "id": "564815001234567890",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "orderId": "564814927928230158",
          "side": "BUY",
          "price": "97393.8",
          "size": "0.001",
          "fee": "0.048697",
          "feeRate": "0.0005",
          "isMaker": true,
          "isTaker": false,
          "createdTime": "1734662380120",
          "updatedTime": "1734662380125"
        }
      ]
    }
  }
}
```

### Position Update with Collateral Changes

When an order fills and affects both position and collateral.

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": "1003",
    "data": {
      "account": [],
      "collateral": [
        {
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "amount": "100.451303",
          "totalAmount": "100.451303",
          "availableAmount": "95.000000",
          "lockedAmount": "5.451303"
        }
      ],
      "collateralTransaction": [
        {
          "id": "564815100000000000",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "orderId": "564814927928230158",
          "type": "OPEN_POSITION_FEE",
          "deltaAmount": "-0.048697",
          "fee": "0.048697",
          "censorStatus": "CENSOR_SUCCESS",
          "createdTime": "1734662380125",
          "updatedTime": "1734662380125"
        }
      ],
      "position": [
        {
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "openSize": "0.006",
          "openValue": "581.459800",
          "openFee": "-0.290730",
          "fundingFee": "0.000000",
          "updatedTime": "1734662380125"
        }
      ],
      "positionTransaction": [
        {
          "id": "564815200000000000",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "type": "BUY_POSITION",
          "deltaOpenSize": "0.001",
          "deltaOpenValue": "97.3938",
          "deltaOpenFee": "-0.048697",
          "fillOpenSize": "0.001",
          "fillOpenValue": "97.3938",
          "fillOpenFee": "-0.048697",
          "fillPrice": "97393.8",
          "realizePnl": "0.000000",
          "isLiquidate": false,
          "isDeleverage": false,
          "orderId": "564814927928230158",
          "orderFillTransactionId": "564815001234567890",
          "collateralTransactionId": "564815100000000000",
          "censorStatus": "CENSOR_SUCCESS",
          "createdTime": "1734662380125",
          "updatedTime": "1734662380125"
        }
      ],
      "deposit": [],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Deposit Update Event

Triggered when a deposit is initiated or its status changes.

```json
{
  "type": "trade-event",
  "content": {
    "event": "DEPOSIT_UPDATE",
    "version": "1004",
    "data": {
      "account": [],
      "collateral": [],
      "collateralTransaction": [],
      "position": [],
      "positionTransaction": [],
      "deposit": [
        {
          "id": "564820000000000000",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "amount": "50.000000",
          "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
          "status": "SUCCESS",
          "l1TxHash": "0xabc123...",
          "l1ConfirmedTime": "1734663000000",
          "createdTime": "1734662800000",
          "updatedTime": "1734663000000"
        }
      ],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Withdraw Update Event

Triggered when a withdrawal is initiated or its status changes.

```json
{
  "type": "trade-event",
  "content": {
    "event": "WITHDRAW_UPDATE",
    "version": "1005",
    "data": {
      "account": [],
      "collateral": [],
      "collateralTransaction": [],
      "position": [],
      "positionTransaction": [],
      "deposit": [],
      "withdraw": [
        {
          "id": "564825000000000000",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "amount": "25.000000",
          "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
          "status": "PENDING_L2_APPROVING",
          "createdTime": "1734663500000",
          "updatedTime": "1734663500000"
        }
      ],
      "transferIn": [],
      "transferOut": [],
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

### Funding Settlement Event

Triggered during funding fee settlement periods.

```json
{
  "type": "trade-event",
  "content": {
    "event": "FUNDING_SETTLEMENT",
    "version": "1006",
    "data": {
      "account": [],
      "collateral": [
        {
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "amount": "100.400000",
          "totalAmount": "100.400000",
          "availableAmount": "94.900000",
          "lockedAmount": "5.500000"
        }
      ],
      "collateralTransaction": [
        {
          "id": "564830000000000000",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "type": "FUNDING_FEE",
          "deltaAmount": "-0.051303",
          "fee": "0.051303",
          "censorStatus": "CENSOR_SUCCESS",
          "createdTime": "1734664800000",
          "updatedTime": "1734664800000"
        }
      ],
      "position": [
        {
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "openSize": "0.006",
          "openValue": "581.459800",
          "openFee": "-0.290730",
          "fundingFee": "-0.051303",
          "updatedTime": "1734664800000"
        }
      ],
      "positionTransaction": [
        {
          "id": "564830100000000000",
          "userId": "543429922866069763",
          "accountId": "543429922991899150",
          "coinId": "1000",
          "contractId": "10000001",
          "type": "SETTLE_FUNDING_FEE",
          "deltaFundingFee": "-0.051303",
          "orderId": "0",
          "censorStatus": "CENSOR_SUCCESS",
          "createdTime": "1734664800000",
          "updatedTime": "1734664800000"
        }
      ],
      "deposit": [],
      "withdraw": [],
      "transferIn": [],
      "transferOut": [],
      "order": [],
      "orderFillTransaction": []
    }
  }
}
```

## Error Handling

When an error occurs, the server sends an error message with the following structure:

```json
{
  "type": "error",
  "content": {
    "code": "AUTHENTICATION_FAILED",
    "msg": "Invalid signature or expired timestamp"
  }
}
```

### Common Error Codes

| Error Code | Description | Action |
|------------|-------------|--------|
| `AUTHENTICATION_FAILED` | Authentication signature is invalid or timestamp is expired | Regenerate signature with current timestamp and reconnect |
| `INVALID_MESSAGE_FORMAT` | Message sent by client has invalid format | Check message JSON structure |
| `CONNECTION_TIMEOUT` | Client failed to respond to ping messages | Ensure client is responding to ping with pong |
| `RATE_LIMIT_EXCEEDED` | Too many connection attempts or messages | Reduce connection/message frequency |
| `ACCOUNT_NOT_FOUND` | Account ID in authentication is invalid | Verify account ID and authentication parameters |
| `INTERNAL_SERVER_ERROR` | Unexpected server error | Retry connection after brief delay |

## Best Practices

### Connection Management

1. **Implement Reconnection Logic**: WebSocket connections can drop due to network issues. Implement exponential backoff reconnection logic.
2. **Handle Pings Promptly**: Respond to server pings immediately to avoid disconnection.
3. **Monitor Connection Health**: Use client-initiated pings to measure latency and detect connection issues.

### Data Handling

1. **Use Version Numbers**: The `version` field increments with each update. Use it to detect missed messages or ordering issues.
2. **Maintain Local State**: Build a local state representation from snapshot and incremental updates.
3. **Handle Empty Arrays**: Not all data arrays will be present in every message. Check for array existence and length before processing.

### Authentication

1. **Refresh Timestamps**: Authentication timestamps should be recent (within a few minutes of current time).
2. **Secure Credential Storage**: Store API Key/Passphrase/API Secret securely; never expose API Secret in browser code.
3. **Signature Validation**: Ensure signatures are generated correctly using HMAC-SHA256 and sorted query parameters.

### Error Recovery

1. **Log All Errors**: Log error messages for debugging and monitoring.
2. **Graceful Degradation**: If WebSocket fails, fall back to REST API polling for critical data.
3. **Alert on Critical Events**: Monitor for liquidation events (`START_LIQUIDATING`, `FINISH_LIQUIDATING`) and alert users immediately.

## Rate Limits

- **Connection Limit**: Maximum 5 concurrent WebSocket connections per account
- **Message Rate**: No explicit message rate limit, but excessive client pings may trigger rate limiting
- **Reconnection**: Implement exponential backoff (start at 1 second, max 60 seconds) to avoid connection rate limits

## See Also

- [Authentication Guide](../authentication.md) - Detailed HMAC authentication and signature generation
- [Public WebSocket API](./public-websocket.md) - Market data WebSocket
- [Account API](../private-api/account-api.md) - REST API for account information
- [Order API](../private-api/order-api.md) - REST API for order management
