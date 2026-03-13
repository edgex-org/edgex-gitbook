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

## Connection Management

### Connection Lifecycle

1. **Connect**: Establish WebSocket connection to `wss://[domain]/api/v1/public/ws`
2. **Connected**: Receive connection confirmation message with session ID
3. **Subscribe**: Send subscription requests for desired channels
4. **Subscribed**: Receive subscription confirmation for each channel
5. **Snapshot**: Receive initial snapshot data (for applicable channels)
6. **Updates**: Receive real-time data updates as they occur
7. **Heartbeat**: Exchange ping/pong messages periodically
8. **Unsubscribe**: Send unsubscribe requests when no longer needed (optional)
9. **Disconnect**: Close connection gracefully or handle unexpected disconnections

### Disconnection and Reconnection

**Disconnection Reasons:**
- Network issues or connection timeout
- Server maintenance or restart
- Missing 5 consecutive heartbeat responses
- Multiple invalid message errors (5+)
- Client-initiated disconnect

**Reconnection Strategy:**
1. Implement exponential backoff (start with 1s, max 60s)
2. Re-establish WebSocket connection
3. Re-subscribe to all previously active channels
4. Resume processing data updates

**Important:** Subscription state is not persisted across reconnections. You must re-subscribe after reconnecting.

## Rate Limits

- **Connection Rate Limit**: Maximum 10 new connections per minute per IP
- **Subscription Limit**: Maximum 100 active channel subscriptions per connection
- **Message Rate Limit**: Maximum 100 messages per second per connection (includes pings)

Exceeding rate limits may result in temporary connection rejection or throttling.

## Best Practices

1. **Heartbeat Management**
   - Respond to server pings immediately to maintain connection
   - Send client pings periodically (every 30-60 seconds) to measure latency
   - Monitor connection health and reconnect if no pings received

2. **Subscription Management**
   - Subscribe to only the channels you need
   - Use `ticker.all.1s` instead of individual ticker subscriptions when monitoring many contracts
   - Unsubscribe from channels when no longer needed to reduce bandwidth

3. **Data Processing**
   - Handle both `Snapshot` and `Changed` data types appropriately
   - For depth updates, maintain a local order book and apply incremental updates correctly
   - Process updates asynchronously to avoid blocking the WebSocket receive loop

4. **Error Handling**
   - Validate all outgoing messages before sending
   - Log and monitor error messages
   - Implement automatic reconnection with backoff
   - Alert on repeated errors

5. **Performance Optimization**
   - Use binary compression if available (e.g., permessage-deflate)
   - Batch multiple subscriptions in quick succession if possible
   - Process high-frequency channels (trades, depth) efficiently to avoid backlog

## Example Implementation

### JavaScript/Node.js Example

```javascript
const WebSocket = require('ws');

class EdgeXPublicWebSocket {
  constructor(url) {
    this.url = url;
    this.ws = null;
    this.pingInterval = null;
  }

  connect() {
    this.ws = new WebSocket(this.url);

    this.ws.on('open', () => {
      console.log('Connected to EdgeX Public WebSocket');
      this.startHeartbeat();
    });

    this.ws.on('message', (data) => {
      const message = JSON.parse(data);
      this.handleMessage(message);
    });

    this.ws.on('close', () => {
      console.log('Disconnected from EdgeX Public WebSocket');
      this.stopHeartbeat();
      // Implement reconnection logic here
    });

    this.ws.on('error', (error) => {
      console.error('WebSocket error:', error);
    });
  }

  handleMessage(message) {
    switch (message.type) {
      case 'connected':
        console.log('Connection confirmed, session ID:', message.sid);
        // Subscribe to channels after connection
        this.subscribe('ticker.10000001');
        this.subscribe('depth.10000001.15');
        break;
      
      case 'ping':
        // Respond to server ping
        this.send({ type: 'pong', time: message.time });
        break;
      
      case 'subscribed':
        console.log('Subscribed to channel:', message.channel);
        break;
      
      case 'payload':
        this.processData(message);
        break;
      
      case 'error':
        console.error('Error from server:', message.content);
        break;
    }
  }

  subscribe(channel) {
    this.send({ type: 'subscribe', channel });
  }

  unsubscribe(channel) {
    this.send({ type: 'unsubscribe', channel });
  }

  send(data) {
    if (this.ws && this.ws.readyState === WebSocket.OPEN) {
      this.ws.send(JSON.stringify(data));
    }
  }

  startHeartbeat() {
    // Send client ping every 30 seconds
    this.pingInterval = setInterval(() => {
      this.send({ type: 'ping', time: Date.now().toString() });
    }, 30000);
  }

  stopHeartbeat() {
    if (this.pingInterval) {
      clearInterval(this.pingInterval);
      this.pingInterval = null;
    }
  }

  processData(message) {
    const { channel, content } = message;
    console.log(`Data from ${channel}:`, content.dataType, content.data);
    
    // Implement your data processing logic here
    if (channel.startsWith('ticker.')) {
      // Process ticker data
    } else if (channel.startsWith('depth.')) {
      // Process depth data
      this.updateOrderBook(content);
    } else if (channel.startsWith('trades.')) {
      // Process trade data
    }
  }

  updateOrderBook(content) {
    if (content.dataType === 'Snapshot') {
      // Replace entire order book
      console.log('Order book snapshot received');
    } else if (content.dataType === 'Changed') {
      // Apply incremental updates
      console.log('Order book update received');
    }
  }

  disconnect() {
    this.stopHeartbeat();
    if (this.ws) {
      this.ws.close();
    }
  }
}

// Usage
const client = new EdgeXPublicWebSocket('wss://api.example.com/api/v1/public/ws');
client.connect();
```

## Summary

The EdgeX Public WebSocket API provides efficient real-time access to market data without authentication. Key features include:

- **No Authentication Required**: Connect and subscribe without API keys
- **Real-time Data**: Receive immediate updates for tickers, depth, trades, and K-lines
- **Efficient Updates**: Snapshot + incremental update pattern minimizes bandwidth
- **Reliable Heartbeat**: Bidirectional ping/pong mechanism ensures connection health
- **Flexible Subscriptions**: Subscribe to individual contracts or aggregate channels

For private account data streams, see the [Private WebSocket API](./private-websocket.md) documentation.
