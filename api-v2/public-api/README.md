# Public API

Public APIs provide access to market data and platform information without requiring authentication.

## Available Services

### [Metadata API](./metadata-api.md)

Retrieve platform configuration and reference data:

- **Get Server Time** - Current server timestamp
- **Get Metadata** - Global configuration including:
  - Coin list (supported assets)
  - Contract list (available perpetual contracts)
  - Multi-chain configuration
  - System parameters

### [Quote API](./quote-api.md)

Access real-time and historical market data:

- **Get Depth** - Order book depth (buy/sell orders)
- **Get K-line** - Historical K-line (candlestick) data
- **Get Ticker** - 24-hour market statistics
- **Get Ticker Summary** - Aggregated market overview
- **Get Multi-Contract K-line** - K-line data for multiple contracts
- **Get Accurate Open Interest** - Contract open interest
- **Get Exchange Long/Short Ratio** - Long/short position ratio
- **Get Market Status** - Market open/close status (for stocks)

### [Funding Rate API](./funding-api.md)

Query funding rate information:

- **Get Latest Funding Rate** - Current funding rate by contract
- **Get Funding Rate Page** - Historical funding rates (paginated)

## Key Features

- **No Authentication Required** - All public endpoints are accessible without API keys
- **Rate Limits Apply** - Fair usage policies ensure service availability for all users
- **Real-Time Data** - Market data is updated in real-time
- **Historical Data** - Access to historical market information

## Usage Example

```bash
# Get server time
curl -X GET "https://api.edgex.exchange/api/v2/public/meta/getServerTime"

# Get market depth for BTC contract
curl -X GET "https://api.edgex.exchange/api/v2/public/quote/getDepth?contractId=10000001&level=15"

# Get latest funding rate
curl -X GET "https://api.edgex.exchange/api/v2/public/funding/getLatestFundingRate?contractId=10000001"
```

## Response Format

All public API responses follow the standard format:

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

## Base URL

- **Production**: `https://pro.edgex.exchange`
- **Testnet**: `https://testnet.edgex.exchange`

## Best Practices

1. **Cache Metadata** - Metadata changes infrequently; cache locally to reduce requests
2. **Use Appropriate Depth Levels** - Request only the depth level you need (15 or 200)
3. **Respect Rate Limits** - Implement exponential backoff for retries
4. **Handle Errors Gracefully** - Check `code` field in responses

## Navigation

- [← Back to API V2 Documentation](../README.md)
- [Metadata API →](./metadata-api.md)
- [Quote API →](./quote-api.md)
- [Funding Rate API →](./funding-api.md)
