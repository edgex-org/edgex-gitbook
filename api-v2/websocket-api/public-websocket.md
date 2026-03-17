# Public WebSocket API

The Public WebSocket API provides real-time market data streams without requiring authentication. This includes ticker data, order book depth, K-line (candlestick) data, trade information, and metadata updates.

## Connection Information

**WebSocket URL:** `wss://[domain]/api/v1/public/ws`

**Note:** The WebSocket endpoint continues to use the `/api/v1/` path for backward compatibility, even though this is the V2 API documentation.

**Authentication:** Not required for public WebSocket connections.

## Connection Establishment

Upon successful connection, the server sends a connection confirmation message:

```json
{
  "sid": "session-id-string",
  "type": "connected",
  "channel": null,
  "request": null,
  "content": null,
  "time": null
}
```

**Response Fields:**

| Name | Type | Description |
|------|------|-------------|
| `sid` | string | Unique session identifier assigned by the server |
| `type` | string | Message type, always `connected` for connection confirmation |
| `channel` | string or null | Not applicable for connection messages |
| `request` | string or null | Not applicable for connection messages |
| `content` | any or null | Not applicable for connection messages |
| `time` | string or null | Not applicable for connection messages |

## Heartbeat Mechanism (Ping/Pong)

The WebSocket connection requires periodic heartbeat messages to maintain the connection and measure latency.

### Server-Initiated Ping (Heartbeat)

The server sends periodic ping messages to verify the connection is alive.

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

**Important:** The client must respond with a pong message containing the same `time` value received in the ping. If the server doesn't receive a pong response after 5 consecutive pings, the connection will be terminated.

**Response Schema:**

| Name | Type | Description |
|------|------|-------------|
| `type` | string | Always `ping` |
| `time` | string(int64) | Server timestamp in milliseconds when the ping was sent |

### Client-Initiated Ping (Latency Measurement)

Clients can also initiate ping messages to measure round-trip latency.

**Client Ping Message:**
```json
{
  "type": "ping",
  "time": "1693208170000"
}
```

**Server Response:**
```json
{
  "type": "pong",
  "time": "1693208170000"
}
```

**Request Schema:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | true | Must be `ping` |
| `time` | string(int64) | true | Client timestamp in milliseconds |

**Response Schema:**

| Name | Type | Description |
|------|------|-------------|
| `type` | string | Always `pong` |
| `time` | string(int64) | Echoes the request `time` for latency calculation |

## Subscription Mechanism

### Subscribe to a Channel

**Request Format:**
```json
{
  "type": "subscribe",
  "channel": "ticker.10000001"
}
```

**Request Fields:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | true | Must be `subscribe` |
| `channel` | string | true | Channel identifier to subscribe to (see channel list below) |

**Success Response:**
```json
{
  "type": "subscribed",
  "channel": "ticker.10000001"
}
```

**Response Fields:**

| Name | Type | Description |
|------|------|-------------|
| `type` | string | Always `subscribed` on successful subscription |
| `channel` | string | Channel that was successfully subscribed to |

**Error Response:**
```json
{
  "type": "error",
  "content": {
    "code": "INVALID_CONTRACT_ID",
    "msg": "invalid contractId:100000001"
  }
}
```

**Error Response Fields:**

| Name | Type | Description |
|------|------|-------------|
| `type` | string | Always `error` |
| `content.code` | string | Error code identifying the type of error |
| `content.msg` | string | Human-readable error message |

### Unsubscribe from a Channel

**Request Format:**
```json
{
  "type": "unsubscribe",
  "channel": "ticker.10000001"
}
```

**Request Fields:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | string | true | Must be `unsubscribe` |
| `channel` | string | true | Channel identifier to unsubscribe from |

**Success Response:**
```json
{
  "type": "unsubscribed",
  "channel": "ticker.10000001"
}
```

**Response Fields:**

| Name | Type | Description |
|------|------|-------------|
| `type` | string | Always `unsubscribed` on successful unsubscription |
| `channel` | string | Channel that was successfully unsubscribed from |

## Data Push Format

After subscribing to a channel, the server will push data updates in the following format:

```json
{
  "type": "payload",
  "channel": "ticker.10000001",
  "content": {
    "dataType": "Snapshot",
    "channel": "ticker.10000001",
    "data": [...]
  }
}
```

**Message Fields:**

| Name | Type | Description |
|------|------|-------------|
| `type` | string | Always `payload` for data push messages |
| `channel` | string | Channel this data belongs to |
| `content.dataType` | string | Data update type: `Snapshot` or `Changed` |
| `content.channel` | string | Redundant channel identifier (same as outer `channel`) |
| `content.data` | array | Data objects specific to the subscribed channel |

### Data Types

- **Snapshot**: Full dataset pushed once immediately after subscription or when a complete state is needed
- **Changed**: Incremental updates pushed when data changes occur

## Available Channels

### 1. Metadata Channel

Subscribe to global metadata including coin and contract information.

**Channel:** `metadata`

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "metadata"
}
```

#### Subscribe Response

```json
{
  "type": "subscribed",
  "channel": "metadata"
}
```

#### Data Payload

After subscription, a snapshot of metadata is pushed:

```json
{
  "type": "payload",
  "channel": "metadata",
  "content": {
    "dataType": "Snapshot",
    "channel": "metadata",
    "data": [
      {
        "global": {
          "timeZone": "UTC",
          "serverTime": "1693208170000"
        },
        "coinList": [
          {
            "coinId": "1",
            "coinName": "USDT",
            "coinFullName": "Tether USD",
            "displayName": "USDT",
            "displayPrecision": "2"
          }
        ],
        "contractList": [
          {
            "contractId": "10000001",
            "contractName": "BTCUSDT",
            "baseCoinId": "1",
            "quoteCoinId": "2",
            "pricePrecision": "1",
            "sizePrecision": "3",
            "contractType": "PERPETUAL",
            "contractStatus": "TRADING"
          }
        ],
        "multiChain": {
          "chains": []
        }
      }
    ]
  }
}
```

**Data Fields:**
- `global` (object): Global system information
  - `timeZone` (string): Server timezone setting
  - `serverTime` (string): Current server timestamp in milliseconds
- `coinList` (array): List of all available coins/currencies
  - `coinId` (string): Unique identifier for the coin
  - `coinName` (string): Short coin name (e.g., "USDT", "BTC")
  - `coinFullName` (string): Full coin name
  - `displayName` (string): Display name for UI
  - `displayPrecision` (string): Number of decimal places for display
- `contractList` (array): List of all trading contracts
  - `contractId` (string): Unique identifier for the contract
  - `contractName` (string): Contract symbol (e.g., "BTCUSDT")
  - `baseCoinId` (string): ID of the base coin in the trading pair
  - `quoteCoinId` (string): ID of the quote coin in the trading pair
  - `pricePrecision` (string): Number of decimal places for price
  - `sizePrecision` (string): Number of decimal places for quantity
  - `contractType` (string): Contract type (e.g., "PERPETUAL")
  - `contractStatus` (string): Current trading status
- `multiChain` (object): Multi-chain withdrawal configuration
  - `chains` (array): List of supported blockchain networks

### 2. Ticker Channels

Subscribe to 24-hour rolling ticker statistics for contracts.

**Available Channels:**
- `ticker.{contractId}` - Subscribe to ticker for a specific contract (e.g., `ticker.10000001`)
- `ticker.all` - Subscribe to ticker updates for all contracts (event-driven)
- `ticker.all.1s` - Subscribe to ticker for all contracts with periodic 1-second updates

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "ticker.10000001"
}
```

#### Subscribe Response

```json
{
  "type": "subscribed",
  "channel": "ticker.10000001"
}
```

#### Data Payload

```json
{
  "type": "payload",
  "channel": "ticker.10000001",
  "content": {
    "dataType": "Snapshot",
    "channel": "ticker.10000001",
    "data": [
      {
        "contractId": "10000001",
        "contractName": "BTCUSDT",
        "priceChange": "150.5",
        "priceChangePercent": "0.0051",
        "trades": "15234",
        "size": "1523.456",
        "value": "45678901.23",
        "high": "30250.00",
        "low": "29800.00",
        "open": "30000.00",
        "close": "30150.50",
        "highTime": "1693208100000",
        "lowTime": "1693205500000",
        "startTime": "1693121770000",
        "endTime": "1693208170000",
        "lastPrice": "30150.50",
        "indexPrice": "30148.75",
        "oraclePrice": "30149.25",
        "markPrice": "30149.25",
        "openInterest": "12345.678",
        "fundingRate": "0.0001",
        "fundingTime": "1693206000000",
        "nextFundingTime": "1693234800000",
        "bestAskPrice": "30151.00",
        "bestBidPrice": "30150.00",
        "marketOpen": true
      }
    ]
  }
}
```

**Data Fields:**
- `contractId` (string): Unique identifier for the contract
- `contractName` (string): Contract symbol name (e.g., "BTCUSDT")
- `priceChange` (string): Absolute price change in the last 24 hours (current price - open price)
- `priceChangePercent` (string): Percentage price change in the last 24 hours (decimal format, e.g., "0.0051" = 0.51%)
- `trades` (string): Total number of trades executed in the last 24 hours
- `size` (string): Total trading volume (in base currency) in the last 24 hours
- `value` (string): Total trading value (in quote currency) in the last 24 hours
- `high` (string): Highest price in the last 24 hours
- `low` (string): Lowest price in the last 24 hours
- `open` (string): Opening price at the start of the 24-hour window
- `close` (string): Closing price (most recent price) at the end of the 24-hour window
- `highTime` (string): Timestamp in milliseconds when the highest price occurred
- `lowTime` (string): Timestamp in milliseconds when the lowest price occurred
- `startTime` (string): Start timestamp of the 24-hour window in milliseconds
- `endTime` (string): End timestamp of the 24-hour window in milliseconds
- `lastPrice` (string): Most recent trade price
- `indexPrice` (string): Current index price calculated from spot exchanges
- `oraclePrice` (string): Current oracle price from price feed
- `markPrice` (string): Current mark price used for liquidations and PnL calculation
- `openInterest` (string): Total open interest (sum of all open positions)
- `fundingRate` (string): Current settled funding rate (decimal format)
- `fundingTime` (string): Timestamp in milliseconds of the last funding settlement
- `nextFundingTime` (string): Timestamp in milliseconds of the next funding settlement
- `bestAskPrice` (string): Best (lowest) ask price in the order book
- `bestBidPrice` (string): Best (highest) bid price in the order book
- `marketOpen` (boolean): Whether the market is currently open for trading

**Notes:**
- Initial subscription receives `"dataType": "Snapshot"` with current data
- Subsequent updates are `"dataType": "Changed"` and sent when ticker values change
- For `ticker.all.1s`, updates are pushed every 1 second regardless of changes

### 3. K-line (Candlestick) Channels

Subscribe to K-line (candlestick) data for technical analysis.

**Channel Format:** `kline.{priceType}.{contractId}.{interval}`

**Price Types:**
- `LAST_PRICE` - K-line based on actual trade prices
- `MARK_PRICE` - K-line based on mark prices

**Intervals:**
- `MINUTE_1` - 1-minute candlesticks
- `MINUTE_5` - 5-minute candlesticks
- `MINUTE_15` - 15-minute candlesticks
- `MINUTE_30` - 30-minute candlesticks
- `HOUR_1` - 1-hour candlesticks
- `HOUR_2` - 2-hour candlesticks
- `HOUR_4` - 4-hour candlesticks
- `HOUR_6` - 6-hour candlesticks
- `HOUR_8` - 8-hour candlesticks
- `HOUR_12` - 12-hour candlesticks
- `DAY_1` - Daily candlesticks
- `WEEK_1` - Weekly candlesticks
- `MONTH_1` - Monthly candlesticks

**Example Channel:** `kline.LAST_PRICE.10000001.MINUTE_1`

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "kline.LAST_PRICE.10000001.MINUTE_1"
}
```

#### Subscribe Response

```json
{
  "type": "subscribed",
  "channel": "kline.LAST_PRICE.10000001.MINUTE_1"
}
```

#### Data Payload

```json
{
  "type": "payload",
  "channel": "kline.LAST_PRICE.10000001.MINUTE_1",
  "content": {
    "dataType": "Changed",
    "channel": "kline.LAST_PRICE.10000001.MINUTE_1",
    "data": [
      {
        "klineId": "1693208160000",
        "contractId": "10000001",
        "contractName": "BTCUSDT",
        "klineType": "MINUTE_1",
        "klineTime": "1693208160000",
        "priceType": "LAST_PRICE",
        "trades": "45",
        "size": "12.345",
        "value": "371234.56",
        "high": "30120.00",
        "low": "30100.00",
        "open": "30105.00",
        "close": "30115.00",
        "makerBuySize": "6.789",
        "makerBuyValue": "204123.45"
      }
    ]
  }
}
```

**Data Fields:**
- `klineId` (string): Unique identifier for this K-line bar (typically the opening timestamp)
- `contractId` (string): Contract identifier this K-line belongs to
- `contractName` (string): Contract symbol name
- `klineType` (string): K-line interval type (e.g., "MINUTE_1", "HOUR_1", "DAY_1")
- `klineTime` (string): Opening timestamp of this K-line bar in milliseconds
- `priceType` (string): Price type used for this K-line ("LAST_PRICE" or "MARK_PRICE")
- `trades` (string): Number of trades that occurred during this K-line period
- `size` (string): Total trading volume (in base currency) during this period
- `value` (string): Total trading value (in quote currency) during this period
- `high` (string): Highest price during this K-line period
- `low` (string): Lowest price during this K-line period
- `open` (string): Opening price at the start of this K-line period
- `close` (string): Closing price at the end of this K-line period (or current price if bar is incomplete)
- `makerBuySize` (string): Volume of maker buy orders (in base currency) - indicates buying pressure
- `makerBuyValue` (string): Value of maker buy orders (in quote currency) - indicates buying pressure

**Notes:**
- Initial subscription receives a snapshot of the most recent completed K-line
- Updates are sent when the current K-line bar changes (new trades occur)
- When a K-line period completes, the final update has the closing values
- A new K-line bar starts immediately after the previous period ends

### 4. Order Book Depth Channels

Subscribe to order book depth updates for real-time market depth analysis.

**Channel Format:** `depth.{contractId}.{depth}`

**Depth Levels:**
- `15` - Top 15 price levels on each side (bids and asks)
- `200` - Top 200 price levels on each side (bids and asks)

**Example Channel:** `depth.10000001.15`

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "depth.10000001.15"
}
```

#### Subscribe Response

```json
{
  "type": "subscribed",
  "channel": "depth.10000001.15"
}
```

#### Snapshot Payload (Initial)

After subscription, a full order book snapshot is pushed once:

```json
{
  "type": "payload",
  "channel": "depth.10000001.15",
  "content": {
    "dataType": "Snapshot",
    "channel": "depth.10000001.15",
    "data": [
      {
        "startVersion": "12345678",
        "endVersion": "12345678",
        "level": 15,
        "contractId": "10000001",
        "contractName": "BTCUSDT",
        "depthType": "SNAPSHOT",
        "bids": [
          ["30150.00", "5.234"],
          ["30149.50", "3.456"],
          ["30149.00", "8.901"]
        ],
        "asks": [
          ["30150.50", "4.567"],
          ["30151.00", "6.789"],
          ["30151.50", "2.345"]
        ]
      }
    ]
  }
}
```

#### Incremental Update Payload

After the initial snapshot, incremental updates are pushed:

```json
{
  "type": "payload",
  "channel": "depth.10000001.15",
  "content": {
    "dataType": "Changed",
    "channel": "depth.10000001.15",
    "data": [
      {
        "startVersion": "12345679",
        "endVersion": "12345680",
        "level": 15,
        "contractId": "10000001",
        "contractName": "BTCUSDT",
        "depthType": "CHANGED",
        "bids": [
          ["30150.00", "0"],
          ["30149.75", "2.500"]
        ],
        "asks": [
          ["30150.50", "7.123"]
        ]
      }
    ]
  }
}
```

**Data Fields:**
- `startVersion` (string): Starting order book version number for this update
- `endVersion` (string): Ending order book version number after this update
- `level` (integer): Maximum number of price levels included (15 or 200)
- `contractId` (string): Contract identifier for this order book
- `contractName` (string): Contract symbol name
- `depthType` (string): Update type - `"SNAPSHOT"` for full data, `"CHANGED"` for incremental updates
- `bids` (array of [price, size]): Buy orders sorted by price descending (highest first)
  - `[0]` (string): Price level
  - `[1]` (string): Total size at this price level
- `asks` (array of [price, size]): Sell orders sorted by price ascending (lowest first)
  - `[0]` (string): Price level
  - `[1]` (string): Total size at this price level

**Important Notes on Incremental Updates:**
- **Snapshot (`depthType: "SNAPSHOT"`)**: Provides the complete order book state. Replace your entire local order book with this data.
- **Incremental (`depthType: "CHANGED"`)**: Contains only changed price levels. Apply these rules:
  - If `size = "0"`: Remove this price level from your local order book
  - If `size > "0"` and price exists: Update the size at this price level
  - If `size > "0"` and price doesn't exist: Add this new price level
- Version numbers (`startVersion`, `endVersion`) can be used to detect missed updates
- Maintain sorted order: bids descending by price, asks ascending by price
- Only the top N levels (15 or 200) are maintained and pushed

### 5. Recent Trades Channel

Subscribe to real-time trade execution data.

**Channel Format:** `trades.{contractId}`

**Example Channel:** `trades.10000001`

#### Subscribe Request

```json
{
  "type": "subscribe",
  "channel": "trades.10000001"
}
```

#### Subscribe Response

```json
{
  "type": "subscribed",
  "channel": "trades.10000001"
}
```

#### Data Payload

```json
{
  "type": "payload",
  "channel": "trades.10000001",
  "content": {
    "dataType": "Changed",
    "channel": "trades.10000001",
    "data": [
      {
        "ticketId": "1234567890",
        "time": "1693208170123",
        "price": "30150.50",
        "size": "0.125",
        "value": "3768.8125",
        "takerOrderId": "9876543210",
        "makerOrderId": "9876543211",
        "takerAccountId": "100234",
        "makerAccountId": "100456",
        "contractId": "10000001",
        "contractName": "BTCUSDT",
        "isBestMatch": true,
        "isBuyerMaker": false
      }
    ]
  }
}
```

**Data Fields:**
- `ticketId` (string): Unique identifier for this trade execution
- `time` (string): Timestamp in milliseconds when the trade was executed
- `price` (string): Execution price of the trade
- `size` (string): Quantity traded (in base currency)
- `value` (string): Total value of the trade (price × size, in quote currency)
- `takerOrderId` (string): Order ID of the taker (aggressor) side
- `makerOrderId` (string): Order ID of the maker (passive) side
- `takerAccountId` (string): Account ID of the taker (may be masked for privacy)
- `makerAccountId` (string): Account ID of the maker (may be masked for privacy)
- `contractId` (string): Contract identifier for this trade
- `contractName` (string): Contract symbol name
- `isBestMatch` (boolean): Whether this trade matched at the best available price
- `isBuyerMaker` (boolean): `true` if the maker was buying (taker was selling), `false` if maker was selling (taker was buying). This indicates market direction: `false` means an aggressive buy (bullish), `true` means an aggressive sell (bearish)

**Notes:**
- Trades are pushed in real-time as they occur
- `dataType` is always `"Changed"` (trades are new events, not snapshots)
- Multiple trades may be pushed in a single message if they occur simultaneously
- Account IDs may be anonymized/masked for privacy depending on configuration

## Minimal Schema Reference

The examples above describe the WebSocket behavior channel by channel. The following schema reference summarizes the reusable message structures for public WebSocket consumers.

### PublicWsEnvelope

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Top-level message type such as `connected`, `subscribed`, `unsubscribed`, `payload`, `ping`, `pong`, or `error` |
| `channel` | string or null | Channel identifier for subscription, payload, and some error messages |
| `request` | string or null | Original request payload for error messages when available |
| `content` | object or null | Message-specific body |
| `time` | string(int64) or null | Timestamp used by heartbeat messages |
| `sid` | string or null | Session identifier, mainly present in the initial `connected` message |

### PublicWsPayload

| Field | Type | Description |
|------|------|-------------|
| `type` | string | Always `payload` |
| `channel` | string | Channel identifier such as `ticker.10000001` |
| `content.dataType` | string | Usually `Snapshot` or `Changed` |
| `content.channel` | string | Same logical channel as the top-level `channel` |
| `content.data` | array | Array of channel-specific payload items |

### MetadataPayloadItem

| Field | Type | Description |
|------|------|-------------|
| `global` | object | Global platform settings such as timezone and server time |
| `coinList` | array | Coin metadata entries |
| `contractList` | array | Contract metadata entries |
| `multiChain` | object | Multi-chain withdrawal and chain configuration |

### TickerPayloadItem

| Field | Type | Description |
|------|------|-------------|
| `contractId` | string(int64) | Contract identifier |
| `contractName` | string | Contract symbol |
| `lastPrice` | string(decimal) | Latest traded price |
| `markPrice` | string(decimal) | Current mark price |
| `indexPrice` | string(decimal) | Current index price |
| `openInterest` | string(decimal) | Current open interest |
| `fundingRate` | string(decimal) | Current funding rate |
| `bestAskPrice` | string(decimal) | Best ask |
| `bestBidPrice` | string(decimal) | Best bid |
| `marketOpen` | boolean | Whether the market is currently open |

### KlinePayloadItem

| Field | Type | Description |
|------|------|-------------|
| `klineId` | string(int64) | Unique identifier for the bar |
| `contractId` | string(int64) | Contract identifier |
| `contractName` | string | Contract symbol |
| `klineType` | string | Interval such as `MINUTE_1` or `HOUR_1` |
| `klineTime` | string(int64) | Opening timestamp of the bar |
| `priceType` | string | `LAST_PRICE` or `MARK_PRICE` |
| `open` | string(decimal) | Open price |
| `high` | string(decimal) | High price |
| `low` | string(decimal) | Low price |
| `close` | string(decimal) | Close price |
| `size` | string(decimal) | Traded size during the interval |
| `value` | string(decimal) | Traded value during the interval |

### DepthSnapshotItem

| Field | Type | Description |
|------|------|-------------|
| `startVersion` | string(int64) | Starting order-book version |
| `endVersion` | string(int64) | Ending order-book version |
| `level` | integer | Book depth level such as `15` or `200` |
| `contractId` | string(int64) | Contract identifier |
| `contractName` | string | Contract symbol |
| `depthType` | string | Always `SNAPSHOT` for a full book snapshot |
| `bids` | array<[price, size]> | Bid levels sorted descending by price |
| `asks` | array<[price, size]> | Ask levels sorted ascending by price |

### DepthChangedItem

| Field | Type | Description |
|------|------|-------------|
| `startVersion` | string(int64) | Starting order-book version |
| `endVersion` | string(int64) | Ending order-book version |
| `level` | integer | Book depth level such as `15` or `200` |
| `contractId` | string(int64) | Contract identifier |
| `contractName` | string | Contract symbol |
| `depthType` | string | Always `CHANGED` for incremental updates |
| `bids` | array<[price, size]> | Changed bid levels only |
| `asks` | array<[price, size]> | Changed ask levels only |

### TradePayloadItem

| Field | Type | Description |
|------|------|-------------|
| `ticketId` | string(int64) | Unique trade identifier |
| `time` | string(int64) | Execution timestamp |
| `price` | string(decimal) | Trade price |
| `size` | string(decimal) | Trade size |
| `value` | string(decimal) | Trade notional value |
| `takerOrderId` | string(int64) | Taker order id |
| `makerOrderId` | string(int64) | Maker order id |
| `contractId` | string(int64) | Contract identifier |
| `contractName` | string | Contract symbol |
| `isBestMatch` | boolean | Whether the fill matched at the best available price |
| `isBuyerMaker` | boolean | Market-side indicator |

## Error Handling

When an error occurs (e.g., invalid channel, malformed message), the server responds with an error message:

```json
{
  "type": "error",
  "channel": null,
  "request": "{\"type\":\"subscribe\",\"channel\":\"invalid.channel\"}",
  "content": {
    "code": "INVALID_CONTRACT_ID",
    "msg": "invalid contractId:100000001",
    "details": null
  }
}
```

**Error Response Fields:**
- `type` (string): Always `"error"` for error messages
- `channel` (string|null): The channel that caused the error, if applicable
- `request` (string): The original request message that caused the error
- `content.code` (string): Machine-readable error code for programmatic handling
- `content.msg` (string): Human-readable error description
- `content.details` (string|null): Additional error details, if available

**Common Error Codes:**
- `INVALID_CONTRACT_ID` - The specified contract ID does not exist
- `INVALID_MESSAGE_TYPE` - The message type is not recognized
- `UNRECOGNIZED_MESSAGE` - The message format is invalid or cannot be parsed
- `GATEWAY_ERROR` - Generic gateway error

**Error Handling Best Practices:**
- If you receive 5+ error responses, the server may close the connection
- Validate channel names and parameters before subscribing
- Implement exponential backoff for reconnection attempts
- Log error messages for debugging

## Minimal Client Example

Use a lightweight client that can:
- connect to `wss://[domain]/api/v1/public/ws`
- send `subscribe` / `unsubscribe` messages
- respond to server `ping` messages with `pong`
- route `payload` messages by `channel`

A minimal subscription request looks like this:

```json
{
  "type": "subscribe",
  "channel": "ticker.10000001"
}
```

A minimal heartbeat response looks like this:

```json
{
  "type": "pong",
  "time": "1693208170000"
}
```

## Connection Management

1. Connect to `wss://[domain]/api/v1/public/ws`
2. Wait for the `connected` message
3. Subscribe to the channels you need
4. Handle `payload` messages by channel and `dataType`
5. Reply to every server `ping` with a matching `pong`
6. Reconnect with backoff if the socket closes unexpectedly

