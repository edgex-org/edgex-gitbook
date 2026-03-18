# Private API

Private APIs require authentication and provide access to account management, trading operations, asset operations, and personal account data.

## Authentication Required

All private REST API endpoints require authentication via HTTP headers:

- `X-edgeX-Api-Key`: Your API key
- `X-edgeX-Passphrase`: Your API passphrase
- `X-edgeX-Api-Timestamp`: Request timestamp in milliseconds
- `X-edgeX-Api-Signature`: HMAC-SHA256 request signature

For detailed authentication rules, please refer to the [Authentication Documentation](../authentication.md).

## Scope of This Section

This documentation section describes the EdgeX V2 private API surface. Platform API capabilities may be broader than the current SDK coverage, and not every platform capability is expanded into a dedicated page in this GitBook yet.

## Available Documented Services

### [Account API](./account-api.md)

Manage trading accounts and query account information.

### [Order API](./order-api.md)

Create, cancel, and query orders.

### [Asset API](./asset-api.md)

Manage deposits, withdrawals, and asset-related queries.

### [Transfer API](./transfer-api.md)

Inter-account fund transfers and transfer-out queries.

## Capability Notes

The V2 platform includes private API capabilities beyond the pages currently expanded in this section. When a capability is mentioned in higher-level summaries but does not yet have a dedicated page here, treat this GitBook as a staged documentation set rather than a one-to-one index of every backend endpoint.

## L2 Signature Requirements

Certain private operations require **EIP-712 L2 signatures** in addition to HTTP authentication, including order creation, withdrawals, and transfer-out operations.

For L2 signature implementation details, refer to the [L2 Signature Documentation](../sign.md).

## Usage Example

```bash
curl -X GET "https://<api-domain>/api/v2/private/account/getAccountAsset?accountId=123456" \
  -H "X-edgeX-Api-Key: your_api_key" \
  -H "X-edgeX-Passphrase: your_api_passphrase" \
  -H "X-edgeX-Api-Timestamp: 1234567890123" \
  -H "X-edgeX-Api-Signature: calculated_signature"
```

## Response Format

All private API responses follow the standard format:

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

- **HTTP REST base URL**: `https://<api-domain>`
- Private REST path prefix: `/api/v2/private/...`

Replace `<api-domain>` with the production HTTP API domain provided by EdgeX. Internal or non-production domains are not included in this public documentation.

## Navigation

- [Back to API V2 Documentation](../README.md)
- [Account API](./account-api.md)
- [Order API](./order-api.md)
- [Asset API](./asset-api.md)
- [Transfer API](./transfer-api.md)
