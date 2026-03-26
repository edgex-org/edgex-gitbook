# API V2 Documentation

Welcome to the EdgeX API V2 documentation. This section provides comprehensive documentation for all V2 REST API endpoints and WebSocket connections.

## Overview

EdgeX API V2 offers a complete set of endpoints for interacting with the EdgeX perpetual trading platform, including:

> Note: This GitBook documents the V2 platform API surface. SDK coverage may differ from the full platform endpoint set.

- **Public APIs**: Market data, metadata, and funding rates (no authentication required)
- **Private APIs**: Account management, order operations, asset management, and transfers (authentication required)
- **WebSocket APIs**: Real-time market data and account updates

## What's New in V2

- Enhanced parameter validation and error handling
- New leverage management endpoints
- Client order ID cancellation support
- POST method for complex order history queries
- Improved WebSocket message documentation
- Better support for position-based order queries

## API Categories

### [Public API](./public-api/README.md)

Access public market data without authentication:

- **[Metadata API](./public-api/metadata-api.md)** - Server time, coin list, contract list, global configuration
- **[Quote API](./public-api/quote-api.md)** - Market depth, K-line data, 24h ticker, market status
- **[Funding Rate API](./public-api/funding-api.md)** - Current and historical funding rates

### [Private API](./private-api/README.md)

Manage accounts and execute trades (authentication required):

- **[Account API](./private-api/account-api.md)** - Account assets, positions, collateral, leverage settings
- **[Order API](./private-api/order-api.md)** - Create, cancel, and query orders
- **[Asset API](./private-api/asset-api.md)** - Deposits, withdrawals, cross-chain transfers
- **[Transfer API](./private-api/transfer-api.md)** - Inter-account fund transfers

### [WebSocket API](./websocket-api/README.md)

Real-time data streaming:

- **[Public WebSocket](./websocket-api/public-websocket.md)** - Market data subscriptions (ticker, depth, trades, K-line)
- **[Private WebSocket](./websocket-api/private-websocket.md)** - Account updates, order updates, position changes

## Authentication

Private API endpoints and private WebSocket connections require authentication. EdgeX uses a signature-based authentication mechanism.

- **[Authentication Guide](./authentication.md)** - HTTP header authentication for REST APIs
- **[L2 Signature Guide](./sign.md)** - EIP-712 Layer 2 signature requirements for trading operations

## Access Domains

Use the following production access domains when integrating with EdgeX:

- **HTTP REST production domain**: `https://edgex-prod-v2.edgex.exchange`
- **WebSocket production domain**: `wss://edgex-quote-prod-v2.edgex.exchange`

All examples in this documentation use placeholders to keep endpoint examples consistent:

- Replace `<api-domain>` with `edgex-prod-v2.edgex.exchange`
- Replace `<ws-domain>` with `edgex-quote-prod-v2.edgex.exchange`
- Internal, non-production, and test-only domains are not part of the public documentation

## API Conventions

### Request Format

- **HTTP REST base URL**: `https://<api-domain>`
- **WebSocket base URL**: `wss://<ws-domain>`
- **Content-Type**: `application/json`
- **Character Encoding**: UTF-8

### Response Format

All API responses follow this structure:

```json
{
  "code": "SUCCESS",
  "data": { ... },
  "msg": null,
  "errorParam": null,
  "requestTime": "1234567890123",
  "responseTime": "1234567890124",
  "traceId": "abc123..."
}
```

**Response Fields:**
- `code`: Result code (`SUCCESS` or error code)
- `data`: Response payload (structure varies by endpoint)
- `msg`: Error message (null on success)
- `errorParam`: Error parameter details (null on success)
- `requestTime`: Request timestamp (milliseconds)
- `responseTime`: Response timestamp (milliseconds)
- `traceId`: Request trace ID for debugging

### Common HTTP Status Codes

- `200 OK`: Request successful
- `400 Bad Request`: Invalid request parameters
- `401 Unauthorized`: Authentication failed
- `403 Forbidden`: Access denied
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: Server error

### Pagination

Paginated endpoints return data in this format:

```json
{
  "code": "SUCCESS",
  "data": {
    "dataList": [ ... ],
    "nextPageOffsetData": "..."
  }
}
```

- `dataList`: Array of result items
- `nextPageOffsetData`: Offset token for next page (empty string indicates last page)

### Timestamps

- All timestamps are in milliseconds (Unix epoch time)
- Server time can be obtained via `/api/v2/public/meta/getServerTime`

### Number Precision

- All numeric values are returned as strings to preserve precision
- Always use string comparison for numeric fields in critical operations

## Rate Limits

To ensure service stability, API requests are subject to rate limiting:

- Public API: Higher rate limits
- Private API: Rate limits based on API key tier
- WebSocket: Connection and subscription limits apply

Exceeding rate limits will result in HTTP 429 responses.

## Error Handling

### Error Response Structure

```json
{
  "code": "ERROR_CODE",
  "data": null,
  "msg": "Error description",
  "errorParam": {
    "field": "value"
  },
  "requestTime": "1234567890123",
  "responseTime": "1234567890124",
  "traceId": "abc123..."
}
```

### Common Error Codes

- `INVALID_PARAMETER`: Invalid request parameter
- `ACCOUNT_NOT_FOUND`: Account does not exist
- `INSUFFICIENT_BALANCE`: Insufficient account balance
- `ORDER_NOT_FOUND`: Order does not exist
- `SIGNATURE_ERROR`: Signature verification failed
- `RATE_LIMIT_EXCEEDED`: Rate limit exceeded

## Support

For API support and questions:

- Check the specific endpoint documentation for detailed parameter descriptions
- Review error messages and `errorParam` for troubleshooting
- Use `traceId` when reporting issues

## Version History

- **V2** (Current): Enhanced features, improved validation, new endpoints
- **V1** (Legacy): Previous API version (see [V1 Documentation](../api/README.md))

---

**Last Updated**: 2026-03-10  
**API Version**: V2
