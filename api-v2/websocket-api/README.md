# WebSocket API

EdgeX provides two WebSocket connections for real-time data streaming.

## Production Access Domains

- **HTTP REST production domain**: `https://edgex-prod-v2.edgex.exchange`
- **WebSocket production domain**: `wss://edgex-quote-prod-v2.edgex.exchange`

Use placeholders in examples for consistency:

- `<api-domain>` -> `edgex-prod-v2.edgex.exchange`
- `<ws-domain>` -> `edgex-quote-prod-v2.edgex.exchange`

Internal or non-production domains are not included in this public documentation.

## Choose the Right Connection

| Connection | URL | Authentication | Best For |
|------------|-----|----------------|----------|
| [Public WebSocket](./public-websocket.md) | `wss://<ws-domain>/api/v1/public/ws` | Not required | Market data such as ticker, depth, trades, metadata, and funding rate |
| [Private WebSocket](./private-websocket.md) | `wss://<ws-domain>/api/v1/private/ws` | Required | Account state, order lifecycle, transfers, funding settlements, and risk events |

## Public WebSocket at a Glance

Use the public connection when your application needs public market data.

Common lifecycle:

1. Connect
2. Receive `connected`
3. Send `subscribe`
4. Receive `subscribed`
5. Receive `Snapshot`
6. Continue processing `Changed` updates
7. Respond to `ping` with `pong`

Common business streams:

- `metadata`
- `ticker.all.1s`
- `ticker.{contractId}`
- `depth.{contractId}.15` / `depth.{contractId}.200`
- `trades.{contractId}`
- `fundingRate.{contractId}` / `fundingRate.all`

See [Public WebSocket Documentation](./public-websocket.md).

## Private WebSocket at a Glance

Use the private connection when your application needs authenticated account, order, or risk events.

Common lifecycle:

1. Build the connection URL with `accountId` and `timestamp`
2. Sign the request with the same HMAC-SHA256 logic used by private HTTP APIs
3. Connect
4. Receive the initial `Snapshot`
5. Process incremental `trade-event` messages
6. Respond to `ping` with `pong`

Common business streams:

- Initial account snapshot
- Account state updates
- Order lifecycle updates
- Risk and forced events

See [Private WebSocket Documentation](./private-websocket.md).

## Authentication Reminder

Private WebSocket authentication uses the same HMAC-SHA256 rules as private HTTP APIs. See the [Authentication Guide](../authentication.md) for the full signing rules.

## Navigation

- [Public WebSocket Documentation](./public-websocket.md)
- [Private WebSocket Documentation](./private-websocket.md)
- [Authentication Guide](../authentication.md)
- [Back to API V2 Documentation](../README.md)
