# Public WebSocket API

The Public WebSocket API provides real-time market data without authentication.

## Connection Information

**WebSocket URL:** `wss://<ws-domain>/api/v1/public/ws`

Replace `<ws-domain>` with the production WebSocket domain `edgex-quote-prod-v2.edgex.exchange`.

**Authentication:** Not required.

**Note:** The endpoint continues to use the `/api/v1/` path for backward compatibility.

## Common Usage Flow

All public market-data streams follow the same connection lifecycle:

1. Connect to `wss://<ws-domain>/api/v1/public/ws`
2. Receive a `connected` message from the server
3. Send a `subscribe` request for the channel you need
4. Receive a `subscribed` confirmation
5. Receive an initial `Snapshot` for that channel
6. Continue processing incremental `Changed` updates
7. Respond to `ping` with `pong`
8. Send `unsubscribe` when the channel is no longer needed

## How to Read a Public WebSocket Message

There are three important concepts in public WebSocket messages:

- `type` - the top-level message category, such as `connected`, `subscribed`, `quote-event`, or `ping`
- `channel` - the market data topic, such as `ticker.all.1s`, `depth.{contractId}.200`, or `fundingRate.all`
- `content.dataType` - the update stage inside a `quote-event`, typically `Snapshot` or `Changed`

In practice:

- `type` tells you what kind of message this is
- `channel` tells you which market data stream this message belongs to
- `content.dataType` tells you whether the payload is the initial snapshot or a later incremental update

## Common Request and Response Models

### Connect

#### Description

Establish a public WebSocket connection.

#### Request

Open the following WebSocket URL:

```text
wss://<ws-domain>/api/v1/public/ws
```

#### Response Model

| Field | Type | Description |
|------|------|-------------|
| `sid` | string | Session identifier assigned by the server |
| `type` | string | Always `connected` for the connection confirmation message |

#### Response Example

```json
{
  "sid": "session-id-string",
  "type": "connected"
}
```

### Subscribe

#### Description

Subscribe to a public market-data channel.

#### Request Parameters

| Field | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | yes | Must be `subscribe` |
| `channel` | string | yes | Channel to subscribe to |

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "depth.10000001.200"
}
```

#### Response Model

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Always `subscribed` |
| `channel` | string | Subscribed channel name |

#### Response Example

```json
{
  "type": "subscribed",
  "channel": "depth.10000001.200"
}
```

### Unsubscribe

#### Description

Unsubscribe from a public market-data channel.

#### Request Parameters

| Field | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | yes | Must be `unsubscribe` |
| `channel` | string | yes | Channel to unsubscribe from |

#### Request Example

```json
{
  "type": "unsubscribe",
  "channel": "depth.10000001.200"
}
```

#### Response Model

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Always `unsubscribed` |
| `channel` | string | Unsubscribed channel name |

#### Response Example

```json
{
  "type": "unsubscribed",
  "channel": "depth.10000001.200"
}
```

### Quote Event

#### Description

Receive market-data updates for a subscribed channel.

#### Response Model

##### Top-Level Envelope

| Field | Type | Description |
|------|------|-------------|
| `sid` | string | Session identifier assigned by the server |
| `type` | string | Top-level message type. For market data pushes, this is `quote-event` |
| `channel` | string | Channel name for this market data stream |
| `request` | string | Original request payload or request context when applicable |
| `content` | object | Message-specific payload |
| `time` | string | Timestamp used by heartbeat messages |

##### Top-level `type` Values

| `type` | Meaning |
|-------|---------|
| `connected` | Connection established |
| `subscribed` | Subscription accepted |
| `unsubscribed` | Unsubscription accepted |
| `quote-event` | Market data push |
| `ping` | Server heartbeat |
| `error` | Request or channel error |

##### `quote-event` Payload

| Field | Type | Description |
|------|------|-------------|
| `content.channel` | string | Subscribed channel name |
| `content.dataType` | string | Event data type: `Snapshot` or `Changed` |
| `content.data` | array | Event payload list for the subscribed channel |

#### Response Examples

##### Initial Snapshot

```json
{
  "type": "quote-event",
  "channel": "depth.10000001.200",
  "content": {
    "dataType": "Snapshot",
    "channel": "depth.10000001.200",
    "data": []
  }
}
```

##### Incremental Update

```json
{
  "type": "quote-event",
  "channel": "depth.10000001.200",
  "content": {
    "dataType": "Changed",
    "channel": "depth.10000001.200",
    "data": []
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

### Metadata

#### Use Case

Use this channel to bootstrap coin, token, and contract definitions before subscribing to market-specific streams.

#### Channel

`metadata`

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "metadata"
}
```

#### Response Model

The server returns a `quote-event` message where:

- `channel = metadata`
- `content.dataType` is `Snapshot` or `Changed`
- `content.data` contains metadata objects for market initialization

#### Response Example

```json
{
  "type": "quote-event",
  "channel": "metadata",
  "content": {
    "dataType": "Snapshot",
    "channel": "metadata",
    "data": []
  }
}
```

### Market Overview Ticker

#### Use Case

Use this stream for market overview pages and contract lists.

#### Channel

`ticker.all.1s`

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "ticker.all.1s"
}
```

#### Response Model

`content.data` is an array of ticker objects.

| Field | Type | Description |
|------|------|-------------|
| `contractId` | integer | Contract identifier |
| `price` | string | Latest traded price |
| `oraclePrice` | string | Oracle price |
| `indexPrice` | string | Index price |
| `markPrice` | string | Mark price |
| `open24h` | string | 24h open price |
| `high24h` | string | 24h high price |
| `low24h` | string | 24h low price |
| `volume24h` | string | 24h volume |
| `turnover24h` | string | 24h turnover |
| `openInterest` | string | Open interest |
| `fundingRate` | string | Current funding rate |
| `predictedFundingRate` | string | Predicted funding rate |
| `marketOpen` | boolean | Whether the market is open |
| `timestamp` | integer | Event timestamp |

#### Response Example

```json
{
  "type": "quote-event",
  "channel": "ticker.all.1s",
  "content": {
    "dataType": "Snapshot",
    "channel": "ticker.all.1s",
    "data": [
      {
        "contractId": 10000001,
        "price": "43000.5",
        "oraclePrice": "42998.2",
        "indexPrice": "43001.0",
        "markPrice": "43000.1",
        "open24h": "42000.0",
        "high24h": "43500.0",
        "low24h": "41800.0",
        "volume24h": "12345.67",
        "turnover24h": "530000000",
        "openInterest": "98765.43",
        "fundingRate": "0.0001",
        "predictedFundingRate": "0.00012",
        "marketOpen": true,
        "timestamp": 1693208170000
      }
    ]
  }
}
```

### Single-Market Ticker

#### Use Case

Use this stream for the latest ticker of a specific trading market.

#### Channel

`ticker.{contractId}`

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "ticker.10000001"
}
```

#### Response Model

The response structure is the same as `ticker.all.1s`, but `content.data` contains ticker objects for the target market.

#### Response Example

```json
{
  "type": "quote-event",
  "channel": "ticker.10000001",
  "content": {
    "dataType": "Snapshot",
    "channel": "ticker.10000001",
    "data": []
  }
}
```

### Order Book

#### Use Case

Use this stream for the trading page order book.

#### Channel

`depth.{contractId}.15` or `depth.{contractId}.200`

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "depth.10000001.200"
}
```

#### Response Model

`content.data` is an array of depth objects.

| Field | Type | Description |
|------|------|-------------|
| `contractId` | integer | Contract identifier |
| `level` | integer | Depth level, currently `15` or `200` |
| `timestamp` | integer | Event timestamp |
| `bids` | array | Bid levels |
| `asks` | array | Ask levels |

Each entry in `bids` and `asks` is a price level tuple:

| Element | Type | Description |
|--------|------|-------------|
| `[0]` | string | Price |
| `[1]` | string | Size |

#### Response Example

```json
{
  "type": "quote-event",
  "channel": "depth.10000001.200",
  "content": {
    "dataType": "Snapshot",
    "channel": "depth.10000001.200",
    "data": [
      {
        "contractId": 10000001,
        "level": 200,
        "timestamp": 1693208170000,
        "bids": [["43000.0", "12.5"]],
        "asks": [["43001.0", "8.3"]]
      }
    ]
  }
}
```

#### Notes

- The gateway currently supports only depth levels `15` and `200`
- Treat `Snapshot` as the full initial book state
- Treat `Changed` as incremental updates after the initial snapshot

### Recent Trades

#### Use Case

Use this stream for the recent trades panel of a trading page.

#### Channel

`trades.{contractId}`

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "trades.10000001"
}
```

#### Response Model

`content.data` is an array of trade objects.

| Field | Type | Description |
|------|------|-------------|
| `contractId` | integer | Contract identifier |
| `tradeId` | integer | Trade identifier |
| `price` | string | Executed price |
| `size` | string | Executed size |
| `side` | string | Aggressor side |
| `timestamp` | integer | Trade timestamp |

#### Response Example

```json
{
  "type": "quote-event",
  "channel": "trades.10000001",
  "content": {
    "dataType": "Snapshot",
    "channel": "trades.10000001",
    "data": [
      {
        "contractId": 10000001,
        "tradeId": 987654321,
        "price": "43000.5",
        "size": "0.25",
        "side": "BUY",
        "timestamp": 1693208170000
      }
    ]
  }
}
```

### Funding Rate

#### Use Case

Use this stream to display funding information for one market or all markets.

#### Channel

`fundingRate.{contractId}` or `fundingRate.all`

#### Request Example

```json
{
  "type": "subscribe",
  "channel": "fundingRate.10000001"
}
```

#### Response Model

`content.data` is an array of funding rate objects.

| Field | Type | Description |
|------|------|-------------|
| `contractId` | integer | Contract identifier |
| `fundingRate` | string | Current funding rate |
| `nextFundingTime` | integer | Next funding settlement time |
| `timestamp` | integer | Event timestamp |

#### Response Example

```json
{
  "type": "quote-event",
  "channel": "fundingRate.10000001",
  "content": {
    "dataType": "Snapshot",
    "channel": "fundingRate.10000001",
    "data": [
      {
        "contractId": 10000001,
        "fundingRate": "0.0001",
        "nextFundingTime": 1693219200000,
        "timestamp": 1693208170000
      }
    ]
  }
}
```

