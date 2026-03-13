# WebSocket API

EdgeX provides WebSocket APIs for real-time data streaming. Two separate WebSocket connections are available:

## Overview

| Connection | URL | Authentication | Purpose |
|------------|-----|----------------|---------|
| **[Public WebSocket](./public-websocket.md)** | `wss://[domain]/api/v1/public/ws` | Not required | Real-time market data |
| **[Private WebSocket](./private-websocket.md)** | `wss://[domain]/api/v1/private/ws` | Required | Account updates and trading events |

**Note**: WebSocket endpoints still use `/api/v1/` path (not changed to v2).

## [Public WebSocket](./public-websocket.md)

Access real-time market data without authentication.

### Features

- **No Authentication Required** - Connect and subscribe immediately
- **Subscription-Based** - Subscribe to specific channels for data you need
- **Real-Time Updates** - Receive market data as it happens
- **Efficient** - Only receive data for subscribed contracts/channels

### Available Channels

- **Metadata** - Platform configuration and contract information
- **Ticker** - 24-hour market statistics
  - Individual contract: `ticker.{contractId}`
  - All contracts: `ticker.all`
  - Periodic push: `ticker.all.1s`
- **K-Line (Candlestick)** - Historical price data
  - Format: `kline.{priceType}.{contractId}.{interval}`
  - Price types: LAST_PRICE, MARK_PRICE
  - Intervals: 1min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 8hour, 12hour, 1day, 1week, 1month
- **Depth (Order Book)** - Buy and sell orders
  - Format: `depth.{contractId}.{depth}`
  - Depth levels: 15, 200
- **Trades** - Latest executed trades
  - Format: `trades.{contractId}`

### Message Types

- **Snapshot** - Initial full dataset after subscription
- **Changed** - Incremental updates (ticker, trades)
- **SNAPSHOT/CHANGED** - Depth uses special update mechanism

**[→ View Public WebSocket Documentation](./public-websocket.md)**

---

## [Private WebSocket](./private-websocket.md)

Receive real-time updates about your account, orders, and positions (authentication required).

### Features

- **Authentication Required** - Signature-based authentication
- **Auto-Push** - No subscription needed; data pushed automatically
- **Account-Specific** - Only receive data for your authenticated account
- **Trading Events** - Real-time order fills, position changes, liquidations

### Event Types (Auto-Push)

**Account & Asset Events:**
- `Snapshot` - Initial snapshot data after connection
- `ACCOUNT_UPDATE` - Account status changes
- `DEPOSIT_UPDATE` - Deposit status updates
- `WITHDRAW_UPDATE` - Withdrawal status updates
- `TRANSFER_IN_UPDATE` - Transfer in updates
- `TRANSFER_OUT_UPDATE` - Transfer out updates

**Trading Events:**
- `ORDER_UPDATE` - Order status changes (created, filled, cancelled)
- `ORDER_FILL_FEE_INCOME` - Order fill fee income (maker rebates)

**Risk Management Events:**
- `FUNDING_SETTLEMENT` - Funding rate settlement
- `FORCE_WITHDRAW_UPDATE` - Forced withdrawal due to risk
- `FORCE_TRADE_UPDATE` - Forced trade (liquidation partial fill)
- `START_LIQUIDATING` - Liquidation process started
- `FINISH_LIQUIDATING` - Liquidation process completed

### Authentication Methods

**App/API:**
- Use 4 HMAC headers:
  - `X-edgeX-Api-Key`
  - `X-edgeX-Passphrase`
  - `X-edgeX-Signature`
  - `X-edgeX-Timestamp`
- Include `accountId` and `timestamp` in URL query parameters

### Data Structure

Messages contain a `data` object with arrays of updated information:
- `account` - Account status and balance
- `collateral` - Collateral balances by coin
- `collateralTransaction` - Collateral transaction details
- `position` - Open positions by contract
- `positionTransaction` - Position change records
- `order` - Order information
- `orderFillTransaction` - Order fill details
- `deposit` - Deposit records
- `withdraw` - Withdrawal records
- `transferIn` - Transfer in records
- `transferOut` - Transfer out records

**[→ View Private WebSocket Documentation](./private-websocket.md)**

---

## Connection Basics

### Connection URLs

- **Production**: `wss://pro.edgex.exchange`
- **Testnet**: `wss://testnet.edgex.exchange`

### Heartbeat Mechanism

Both public and private WebSockets use a ping/pong heartbeat:

**Server → Client (Required Response):**
```json
Server: {"type":"ping","time":"1693208170000"}
Client: {"type":"pong","time":"1693208170000"}
```
- Client must respond to server pings within 5 intervals or connection will be closed

**Client → Server (Latency Measurement):**
```json
Client: {"type":"ping","time":"1693208170000"}
Server: {"type":"pong","time":"1693208170000"}
```
- Optional client-initiated ping for measuring round-trip time

### Error Handling

Both connections may send error messages:

```json
{
  "type": "error",
  "content": {
    "code": "INVALID_CONTRACT_ID",
    "msg": "invalid contractId: 100000001"
  }
}
```

Common error codes:
- `INVALID_CONTRACT_ID` - Invalid contract ID in subscription
- `INVALID_CHANNEL` - Malformed channel name
- `AUTHENTICATION_FAILED` - Authentication failed (private WebSocket)
- `RATE_LIMIT_EXCEEDED` - Too many subscriptions or messages

### Best Practices

1. **Implement Reconnection** - Automatically reconnect on disconnect with exponential backoff
2. **Handle Heartbeats** - Always respond to server pings promptly
3. **Manage Subscriptions** - Unsubscribe from channels you no longer need (public)
4. **Buffer Messages** - Handle message bursts during high volatility
5. **Validate Data** - Check message structure before processing
6. **Log Errors** - Keep error messages for debugging

### Rate Limits

- **Public WebSocket:**
  - Max 50 subscriptions per connection
  - Max 10 messages per second (subscribe/unsubscribe)
  
- **Private WebSocket:**
  - Max 5 connections per account
  - No message limit (server-initiated push only)

## Quick Start Examples

### Public WebSocket (Subscribe to BTC Ticker)

```javascript
const ws = new WebSocket('wss://testnet.edgex.exchange/api/v1/public/ws');

ws.onopen = () => {
  // Subscribe to BTC ticker
  ws.send(JSON.stringify({
    type: 'subscribe',
    channel: 'ticker.10000001'
  }));
};

ws.onmessage = (event) => {
  const message = JSON.parse(event.data);
  
  if (message.type === 'ping') {
    // Respond to heartbeat
    ws.send(JSON.stringify({type: 'pong', time: message.time}));
  } else if (message.type === 'payload') {
    // Process ticker data
    console.log('Ticker:', message.content.data);
  }
};
```

### Private WebSocket (Receive Order Updates)

```javascript
const WebSocket = require('ws');

const accountId = '724625476626153743';
const timestamp = Date.now().toString();
// Generate HMAC signature according to Authentication Guide
const signature = buildWsSignature({
  apiSecret,
  method: 'GET',
  requestURI: '/api/v1/private/ws',
  queryString: `accountId=${accountId}&timestamp=${timestamp}`,
  timestamp
});

const ws = new WebSocket(
  `wss://testnet.edgex.exchange/api/v1/private/ws?accountId=${accountId}&timestamp=${timestamp}`,
  {
    headers: {
      'X-edgeX-Api-Key': apiKey,
      'X-edgeX-Passphrase': apiPassphrase,
      'X-edgeX-Signature': signature,
      'X-edgeX-Timestamp': timestamp
    }
  }
);

ws.onmessage = (event) => {
  const message = JSON.parse(event.data);
  
  if (message.type === 'ping') {
    ws.send(JSON.stringify({type: 'pong', time: message.time}));
  } else if (message.type === 'trade-event') {
    const {event, data} = message.content;
    
    if (event === 'ORDER_UPDATE') {
      console.log('Order updated:', data.order);
    } else if (event === 'ACCOUNT_UPDATE') {
      console.log('Account updated:', data.account);
    }
  }
};
```

## Navigation

- [← Back to API V2 Documentation](../README.md)
- [Public WebSocket Documentation →](./public-websocket.md)
- [Private WebSocket Documentation →](./private-websocket.md)
- [Authentication Guide →](../authentication.md)

---

**Last Updated**: 2026-03-10  
**API Version**: V2 (WebSocket paths still use v1)
