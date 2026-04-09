# WebSocket API

This document describes two WebSocket interfaces:

- Public WebSocket: market data only. No authentication is required. Clients subscribe to the channels they need after the connection is established.
- Private WebSocket: account and trading data. Authentication is required. Data is pushed automatically after authentication succeeds.

## Domain

`wss://quote.edgex.exchange`

## Endpoints

| Path | Description |
| --- | --- |
| `/api/v1/public/ws` | Public market data WebSocket endpoint. |
| `/api/v1/private/ws` | Private account and trading WebSocket endpoint. |

## Common Behavior

### Heartbeat

After a connection is established, the server periodically sends a ping frame in JSON form:

```json
{
  "type": "ping",
  "time": "1693208170000"
}
```

The client should reply with:

```json
{
  "type": "pong",
  "time": "1693208170000"
}
```

The client may also proactively send `ping` messages for latency measurement. The server replies with a matching `pong`.

### Error Message

For invalid requests, the server returns an `error` message. For example:

```json
{
  "type": "error",
  "content": {
    "code": "INVALID_CONTRACT_ID",
    "msg": "invalid contractId:100000001"
  }
}
```

## Public WebSocket

### Overview

The public WebSocket provides real-time market data. It does not require authentication.

You can use the public WebSocket to receive:

- Exchange metadata
- 24-hour ticker updates
- Kline / candlestick updates
- Order book depth updates
- Latest trade updates
- Funding rate updates
- Best bid / best ask updates

### Endpoint

- Domain: `wss://quote.edgex.exchange`
- Path: `/api/v1/public/ws`
- Full URL: `wss://quote.edgex.exchange/api/v1/public/ws`

### Authentication

No authentication is required for public interfaces.

### How It Works

1. Connect to the public endpoint.
2. Send `subscribe` messages for the channels you need.
3. Receive `subscribed` acknowledgements.
4. Receive subsequent `quote-event` pushes.
5. Reply to `ping` messages with `pong` to keep the connection alive.

### Subscribe and Unsubscribe

Public channels require explicit subscription.

Subscribe request:

```json
{
  "type": "subscribe",
  "channel": "ticker.10000001"
}
```

Unsubscribe request:

```json
{
  "type": "unsubscribe",
  "channel": "ticker.10000001"
}
```

Subscribe acknowledgement:

```json
{
  "type": "subscribed",
  "channel": "ticker.10000001"
}
```

### Public Message Envelope

All public market data pushes use `quote-event`.

```json
{
  "type": "quote-event",
  "channel": "ticker.10000001",
  "content": {
    "channel": "ticker.10000001",
    "dataType": "Snapshot",
    "data": []
  }
}
```

Real public push example:

```json
{
  "type": "quote-event",
  "channel": "kline.LAST_PRICE.10000004.MINUTE_30",
  "content": {
    "channel": "kline.LAST_PRICE.10000004.MINUTE_30",
    "dataType": "changed",
    "data": [
      {
        "klineId": "687195055420839258",
        "contractId": "10000004",
        "contractName": "BNBUSD",
        "klineType": "MINUTE_30",
        "klineTime": "1775698200000",
        "priceType": "LAST_PRICE",
        "trades": "2381",
        "size": "1076.55",
        "value": "646586.5584",
        "high": "601.73",
        "low": "598.29",
        "open": "599.14",
        "close": "600.43",
        "makerBuySize": "179.51",
        "makerBuyValue": "107781.6211"
      }
    ]
  }
}
```

`content.dataType` identifies whether the payload is an initial snapshot or a later update. In current production traffic, values may appear with different casing such as `Snapshot`, `Changed`, or `changed`. Clients should handle this field case-insensitively.

### Metadata

#### Channel

| Channel | Description |
| --- | --- |
| `metadata` | Global metadata, including coin and contract information. |

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "metadata"
}
```

#### Snapshot Example

```json
{
  "type": "quote-event",
  "channel": "metadata",
  "content": {
    "channel": "metadata",
    "dataType": "Snapshot",
    "data": [
      {
        "global": {},
        "coinList": [],
        "contractList": [],
        "multiChain": {}
      }
    ]
  }
}
```

### Ticker

#### Channels

| Channel | Description |
| --- | --- |
| `ticker.{contractId}` | 24-hour ticker for one contract. |
| `ticker.all` | 24-hour ticker snapshot for all contracts. |
| `ticker.all.1s` | Periodic ticker updates for all contracts. |

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "ticker.all.1s"
}
```

#### Update Example

```json
{
  "type": "quote-event",
  "channel": "ticker.all.1s",
  "content": {
    "channel": "ticker.all.1s",
    "dataType": "changed",
    "data": [
      {
        "contractId": "10000024",
        "contractName": "UNIUSD",
        "priceChange": "0.000",
        "priceChangePercent": "0.000000",
        "trades": "0",
        "size": "0",
        "value": "0",
        "high": "4.698",
        "low": "4.698",
        "open": "4.698",
        "close": "4.698",
        "highTime": "1769865871385",
        "lowTime": "1769865871385",
        "startTime": "1775612700000",
        "endTime": "1775699100000",
        "lastPrice": "4.698",
        "indexPrice": "3.097950710",
        "oraclePrice": "3.09847989119589328765869140625",
        "markPrice": "3.09847989119589328765869140625",
        "openInterest": "558",
        "fundingRate": "0.00000208",
        "fundingTime": "1775698800000",
        "nextFundingTime": "1775699400000",
        "bestAskPrice": "0",
        "bestBidPrice": "0",
        "marketOpen": true
      }
    ]
  }
}
```

### Kline

#### Channel Format

`kline.{priceType}.{contractId}.{interval}`

#### `priceType`

| Value | Description |
| --- | --- |
| `LAST_PRICE` | Last traded price kline. |
| `INDEX_PRICE` | Index price kline. |
| `ORACLE_PRICE` | Oracle price kline. |
| `MARK_PRICE` | Mark price kline. |

#### `interval`

| Value | Description |
| --- | --- |
| `MINUTE_1` | 1-minute kline |
| `MINUTE_5` | 5-minute kline |
| `MINUTE_15` | 15-minute kline |
| `MINUTE_30` | 30-minute kline |
| `HOUR_1` | 1-hour kline |
| `HOUR_2` | 2-hour kline |
| `HOUR_4` | 4-hour kline |
| `HOUR_6` | 6-hour kline |
| `HOUR_8` | 8-hour kline |
| `HOUR_12` | 12-hour kline |
| `DAY_1` | 1-day kline |
| `WEEK_1` | 1-week kline |
| `MONTH_1` | 1-month kline |

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "kline.LAST_PRICE.10000004.MINUTE_30"
}
```

#### Update Example

```json
{
  "type": "quote-event",
  "channel": "kline.LAST_PRICE.10000004.MINUTE_30",
  "content": {
    "channel": "kline.LAST_PRICE.10000004.MINUTE_30",
    "dataType": "changed",
    "data": [
      {
        "klineId": "687195055420839258",
        "contractId": "10000004",
        "contractName": "BNBUSD",
        "klineType": "MINUTE_30",
        "klineTime": "1775698200000",
        "priceType": "LAST_PRICE",
        "trades": "2381",
        "size": "1076.55",
        "value": "646586.5584",
        "high": "601.73",
        "low": "598.29",
        "open": "599.14",
        "close": "600.43",
        "makerBuySize": "179.51",
        "makerBuyValue": "107781.6211"
      }
    ]
  }
}
```

### Depth

#### Channel Format

`depth.{contractId}.{level}`

#### Supported Levels

| Value | Description |
| --- | --- |
| `15` | 15 levels |
| `200` | 200 levels |

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "depth.10000004.200"
}
```

#### Behavior

After a successful subscription, the server first pushes a full snapshot and then pushes incremental updates.

- `depthType = Snapshot` means a full order book snapshot for the subscribed level.
- `depthType = CHANGED` means an incremental update.

#### Snapshot Example

```json
{
  "type": "quote-event",
  "channel": "depth.10000004.200",
  "content": {
    "channel": "depth.10000004.200",
    "dataType": "Snapshot",
    "data": [
      {
        "startVersion": "90595400",
        "endVersion": "90595447",
        "level": 200,
        "contractId": "10000004",
        "contractName": "BNBUSD",
        "asks": [
          { "price": "601.03", "size": "23.33" },
          { "price": "601.09", "size": "18.68" }
        ],
        "bids": [
          { "price": "600.97", "size": "14.26" },
          { "price": "600.90", "size": "8.41" }
        ],
        "depthType": "Snapshot"
      }
    ]
  }
}
```

#### Incremental Example

```json
{
  "type": "quote-event",
  "channel": "depth.10000004.200",
  "content": {
    "channel": "depth.10000004.200",
    "dataType": "changed",
    "data": [
      {
        "startVersion": "90595448",
        "endVersion": "90595463",
        "level": 200,
        "contractId": "10000004",
        "contractName": "BNBUSD",
        "asks": [
          { "price": "601.03", "size": "23.33" },
          { "price": "601.09", "size": "18.68" },
          { "price": "601.15", "size": "18.57" },
          { "price": "601.25", "size": "19.07" },
          { "price": "601.34", "size": "21.14" },
          { "price": "601.43", "size": "0.40" },
          { "price": "601.51", "size": "19.98" }
        ],
        "bids": [],
        "depthType": "CHANGED"
      }
    ]
  }
}
```

### Latest Trades

#### Channel Format

`trades.{contractId}`

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "trades.10000001"
}
```

#### Payload Structure

```json
{
  "type": "quote-event",
  "channel": "trades.10000001",
  "content": {
    "channel": "trades.10000001",
    "dataType": "changed",
    "data": [
      {
        "ticketId": "1",
        "time": "1688365544504",
        "price": "30065.12",
        "size": "0.01",
        "value": "300.6512",
        "takerOrderId": "10",
        "makerOrderId": "11",
        "takerAccountId": "3001",
        "makerAccountId": "3002",
        "contractId": "10000001",
        "isBestMatch": true,
        "isBuyerMaker": false
      }
    ]
  }
}
```

### Funding Rate

#### Channels

| Channel | Description |
| --- | --- |
| `fundingRate.{contractId}` | Funding rate for one contract. |
| `fundingRate.all` | Funding rates for all contracts. |

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "fundingRate.all"
}
```

#### Payload Structure

```json
{
  "type": "quote-event",
  "channel": "fundingRate.all",
  "content": {
    "channel": "fundingRate.all",
    "dataType": "Snapshot",
    "data": [
      {
        "contractId": "10000001",
        "fundingTime": "0",
        "fundingTimestamp": "0",
        "oraclePrice": "0",
        "markPrice": "0",
        "indexPrice": "0",
        "fundingRate": "0",
        "isSettlement": false,
        "previousFundingRate": "0",
        "previousFundingTimestamp": "0",
        "premiumIndex": "0",
        "avgPremiumIndex": "0",
        "premiumIndexTimestamp": "0",
        "impactMarginNotional": "0",
        "impactAskPrice": "0",
        "impactBidPrice": "0",
        "interestRate": "0",
        "predictedFundingRate": "0",
        "fundingRateIntervalMin": "480",
        "fundingIndex": "0"
      }
    ]
  }
}
```

### Book Ticker

#### Channels

| Channel | Description |
| --- | --- |
| `bookTicker.{contractId}` | Best bid and best ask for one contract. |
| `bookTicker.all` | Best bid and best ask for all contracts. |
| `bookTicker.all.1s` | Periodic best bid and ask updates for all contracts. |

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "bookTicker.all.1s"
}
```

#### Payload Structure

```json
{
  "type": "quote-event",
  "channel": "bookTicker.all.1s",
  "content": {
    "channel": "bookTicker.all.1s",
    "dataType": "changed",
    "data": [
      {
        "contractId": "10000001",
        "contractName": "BTCUSD",
        "bestBidPrice": "30000",
        "bestBidSize": "2.5",
        "bestAskPrice": "30001",
        "bestAskSize": "1.8"
      }
    ]
  }
}
```

### Client Handling Notes

- After subscribing, treat the first pushed payload as the initial state for that channel.
- For `metadata`, `ticker`, `kline`, `fundingRate`, and `bookTicker`, later pushes replace the affected records in the local cache.
- For `depth`, apply updates by price level and use `startVersion` / `endVersion` to maintain ordering.
- Handle `dataType` values case-insensitively.
- If a channel is invalid, fix the request and resubscribe instead of retrying blindly.

## Private WebSocket

### Overview

The private WebSocket provides authenticated account and trading updates.

Clients can use the private WebSocket to receive:

- Initial account snapshot after connection
- Account updates
- Deposit and withdrawal updates
- Transfer updates
- Order updates
- Funding settlement updates
- Liquidation-related updates

### Endpoint

- Domain: `wss://quote.edgex.exchange`
- Path: `/api/v1/private/ws`
- Full URL: `wss://quote.edgex.exchange/api/v1/private/ws?accountId={accountId}`

### Authentication

The following headers must be included in the request to authenticate access to private APIs:

| Name | Location  | Required | Description |
| --- | --- | --- | --- | --- |
| `X-edgeX-Api-Timestamp` | header | must | The timestamp when the request was made. |
| `X-edgeX-Api-Signature` | header | must | The signature generated using the private key and request details. |
| `accountId` | query | must | The account ID included in the WebSocket handshake URL. |

Notes:

- The WebSocket handshake is an HTTP `GET` request.
- There is no request body to sign.
- The signing rule should follow the same rule described in `authentication.md` for private HTTP APIs.
- When generating the signature payload, include the request path and query string used in the actual WebSocket handshake.

Example URL:

```text
wss://quote.edgex.exchange/api/v1/private/ws?accountId=543429922991899150
```

### How It Works

1. Connect to the private endpoint with valid signed request information.
2. Receive a `connected` message.
3. Receive an initial `trade-event` snapshot.
4. Receive later `trade-event` updates when account or order data changes.
5. Reply to `ping` messages with `pong` to keep the connection alive.

### Connected Message

```json
{
  "sid": "b146cb1d-528f-cc8f-2733-5cab080a98e9",
  "type": "connected",
  "time": "1775699229526"
}
```

### Private Event Envelope

All private business data is wrapped in a `trade-event` message.

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 17873,
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
    },
    "time": 1775699229529,
    "accountId": 0
  }
}
```

### Event Types

The `content.event` field identifies the reason for the push.

| Event | Description |
| --- | --- |
| `Snapshot` | Initial full snapshot pushed immediately after a successful connection. |
| `ACCOUNT_UPDATE` | Account information changed. |
| `DEPOSIT_UPDATE` | Deposit information changed. |
| `WITHDRAW_UPDATE` | Withdrawal information changed. |
| `TRANSFER_IN_UPDATE` | Transfer-in information changed. |
| `TRANSFER_OUT_UPDATE` | Transfer-out information changed. |
| `ORDER_UPDATE` | Order information changed. |
| `FORCE_WITHDRAW_UPDATE` | Forced withdrawal information changed. |
| `FORCE_TRADE_UPDATE` | Forced trade information changed. |
| `FUNDING_SETTLEMENT` | Funding settlement information changed. |
| `ORDER_FILL_FEE_INCOME` | Fill fee income information changed. |
| `START_LIQUIDATING` | Liquidation started. |
| `FINISH_LIQUIDATING` | Liquidation finished. |
| `UNRECOGNIZED` | Unknown event type. |

### Snapshot Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "Snapshot",
    "version": 17873,
    "data": {
      "account": [
        {
          "id": "645046721134460943",
          "userId": "600162890833461506",
          "ethAddress": "0x488F6C0445af6bb2AfD902758f64BcCDc1C9CF7a",
          "clientAccountId": "main",
          "status": "NORMAL",
          "isLiquidating": false,
          "createdTime": "1769398842321",
          "updatedTime": "1773932040911"
        }
      ],
      "collateral": [
        {
          "userId": "600162890833461506",
          "accountId": "645046721134460943",
          "coinId": "1000",
          "amount": "105.444919",
          "legacyAmount": "105.444919",
          "cumDepositAmount": "1200.000000",
          "cumWithdrawAmount": "-3.000000",
          "cumTransferInAmount": "1000.000000",
          "cumTransferOutAmount": "-1110.000000",
          "cumPositionBuyAmount": "-4618.3770",
          "cumPositionSellAmount": "4170.092034",
          "cumFillFeeAmount": "-0.418796",
          "cumFundingFeeAmount": "-532.851319",
          "cumFillFeeIncomeAmount": "0",
          "createdTime": "1769398842321",
          "updatedTime": "1775699227822"
        }
      ],
      "position": [],
      "order": []
    },
    "time": 1775699229529,
    "accountId": 0
  }
}
```

### Order Update Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": 17874,
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
          "id": "736936291735699983",
          "userId": "600162890833461506",
          "accountId": "645046721134460943",
          "coinId": "1000",
          "contractId": "10000004",
          "side": "BUY",
          "price": "0.00",
          "size": "0.10",
          "clientOrderId": "38704677398287624",
          "type": "MARKET",
          "timeInForce": "IMMEDIATE_OR_CANCEL",
          "reduceOnly": false,
          "triggerPrice": "0",
          "triggerPriceType": "UNKNOWN_PRICE_TYPE",
          "expireTime": "1777513702729",
          "status": "PENDING",
          "maxLeverage": "10",
          "takerFeeRate": "0.000260",
          "makerFeeRate": "0.000020",
          "marketLimitValue": "60.071000",
          "l2Nonce": "3744737356"
        }
      ],
      "orderFillTransaction": []
    },
    "time": 1775699301177,
    "accountId": 0
  }
}
```

### Funding Settlement Example

```json
{
  "type": "trade-event",
  "content": {
    "event": "FUNDING_SETTLEMENT",
    "version": 17877,
    "data": {
      "account": [],
      "collateral": [],
      "collateralTransaction": [
        {
          "id": "736936948907639311",
          "userId": "600162890833461506",
          "accountId": "645046721134460943",
          "coinId": "1000",
          "type": "POSITION_FUNDING",
          "deltaAmount": "0.000000",
          "beforeAmount": "45.358301",
          "fundingTime": "1775699400000",
          "fundingRate": "0",
          "fundingIndexPrice": "600.537396373",
          "fundingOraclePrice": "600.53739626891911029815673828125",
          "fundingPositionSize": "0.10",
          "positionContractId": "10000004",
          "createdTime": "1775699460199",
          "updatedTime": "1775699460246"
        }
      ],
      "position": [],
      "positionTransaction": [
        {
          "id": "736936948907640335",
          "userId": "600162890833461506",
          "accountId": "645046721134460943",
          "coinId": "1000",
          "contractId": "10000004",
          "type": "SETTLE_FUNDING_FEE",
          "deltaFundingFee": "0.000000",
          "beforeOpenSize": "0.10",
          "fundingTime": "1775699400000",
          "fundingRate": "0"
        }
      ]
    },
    "time": 1775699460246,
    "accountId": 0
  }
}
```

### Client Handling Notes

- Treat the first `Snapshot` as the initial full state.
- Treat subsequent events as incremental updates keyed by `event` and entity ID.
- Empty arrays mean that the event did not update that section.
- Always respond to server `ping` messages to keep the connection alive.
