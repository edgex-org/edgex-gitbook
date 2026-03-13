# Private API

Private APIs require authentication and provide access to account management, trading operations, and personal data.

## Authentication Required

All private API endpoints require authentication via HTTP headers:

- `X-edgeX-Api-Key`: Your API key
- `X-edgeX-Api-Signature`: Request signature
- `X-edgeX-Api-Timestamp`: Request timestamp (milliseconds)

For detailed authentication methods, please refer to the [Authentication Documentation](../authentication.md).

## Available Services

### [Account API](./account-api.md)

Manage trading accounts and query account information:

- **Get Account By ID** - Retrieve account details
- **Get Account Asset** - Get account assets, positions, and collateral
- **Get Account Page** - List all accounts for a user (paginated)
- **Register Account** - Create a new trading account
- **Update Account Name** - Change account name
- **Update Account Signers** - Modify authorized signers
- **Create/Delete API Signer** - Manage SDK signers
- **Update Leverage Setting** - Configure leverage for trading ⭐ **[V2 New]**
- **Get Position By Contract ID** - Query positions by contract
- **Get Position Orders** - Get historical orders by position term ⭐ **[V2 New]**
- **Get Position Transaction Page** - Position transaction history (paginated)
- **Get Position Transaction By ID** - Batch get position transactions
- **Get Position Term Page** - Position term records (paginated)
- **Get Collateral By Coin ID** - Query collateral by coin
- **Get Collateral Transaction Page** - Collateral transaction history (paginated)
- **Get Collateral Transaction By ID** - Batch get collateral transactions
- **Get Account Asset Snapshot Page** - Historical account snapshots (paginated)
- **Get Account Deleverage Light** - ADL indicator by account
- **Get User Deleverage Light** - ADL indicators for all user accounts

### [Order API](./order-api.md)

Create, cancel, and query orders:

- **Create Order** - Place a new order (requires L2 signature)
- **Cancel Order By ID** - Cancel order by order ID
- **Cancel Order By Client Order ID** - Cancel order by client order ID ⭐ **[V2 New]**
- **Cancel All Orders** - Cancel all active orders
- **Get Max Order Size** - Calculate maximum order size
- **Get Orders By ID** - Batch get orders by order IDs
- **Get Orders By Client Order ID** - Batch get orders by client order IDs
- **Get Active Order Page** - Active orders (paginated)
- **Get History Order Page** - Historical orders (paginated, GET or POST) ⭐ **[V2 Enhanced]**
- **Get History Order By ID** - Batch get historical orders by IDs
- **Get History Order By Client Order ID** - Batch get historical orders by client IDs
- **Get History Order Fill Transaction Page** - Order fill transactions (paginated)
- **Get History Order Fill Transaction By ID** - Batch get fill transactions

### [Asset API](./asset-api.md)

Manage deposits and withdrawals:

- **Get All Orders Page** - Query all deposit/withdrawal orders (paginated)
- **Get Normal Withdraw By ID** - Get normal withdrawal order
- **Get Normal Withdrawable Amount** - Check withdrawable amount
- **Create Normal Withdraw** - Create normal withdrawal (requires L2 signature)
- **Get Cross Withdraw By ID** - Get cross-chain withdrawal order
- **Get Cross Withdraw Sign Info** - Get signature info for cross-chain withdrawal
- **Create Cross Withdraw** - Create cross-chain withdrawal
- **Get Fast Withdraw By ID** - Get fast withdrawal order
- **Get Fast Withdraw Sign Info** - Get signature info for fast withdrawal
- **Create Fast Withdraw** - Create fast withdrawal
- **Get Coin Rate** - Query coin exchange rates
- **Create Internal Transfer** - Create internal transfer between accounts
- **Get CCTP Max Fee** - Get cross-chain CCTP maximum fee limit

### [Transfer API](./transfer-api.md)

Inter-account fund transfers:

- **Get Transfer In By ID** - Get transfer in order by ID
- **Get Active Transfer In** - Active transfer in orders (paginated)
- **Get Transfer Out By ID** - Get transfer out order by ID
- **Get Active Transfer Out** - Active transfer out orders (paginated)
- **Get Transfer Out Available Amount** - Check available transfer out amount
- **Get Batch Transfer Out Available Amount** - Batch query available amounts
- **Create Transfer Out** - Create transfer out order (requires L2 signature)

### [User API](./user-api.md)

User profile and preferences:

- **Get User** - Get user information by ID
- **Get User Preference** - Get user preferences
- **Get User Info** - Get combined user info and preferences
- **Update User** - Update user information
- **Update User Preference** - Update user preferences
- **Check User Nickname Exist** - Check if nickname is available
- **Send Email Verification** - Resend email verification
- **Get Favorites** - Get favorite contracts list
- **Update Favorites Batch** - Batch update favorite contracts
- **Claim Faucet Coin** - Claim testnet tokens
- **Get Faucet Coin Claim Page** - Faucet claim history (paginated)
- **Get Site Message Page** - In-app messages (paginated)
- **Read Site Message** - Mark messages as read
- **Create Push Config** - Create push notification config
- **Get/Set US Stock Trading Guide Read** - US stock guide read status
- **Get User Accounts Asset** - Get assets for all user accounts

### Additional Private APIs

- **[Deposit API](./deposit-api.md)** - Deposit operations
- **[Withdraw API](./withdraw-api.md)** - Withdrawal operations
- **[Vault API](./vault-api.md)** - Vault staking operations
- **[User Password API](./user-password-api.md)** - Password management
- **[Quote Private API](./quote-private-api.md)** - Private quote data with user trades

## L2 Signature Requirements

Certain operations require StarkEx Layer 2 signatures for security:

- Creating orders
- Creating withdrawals
- Creating transfers

For L2 signature implementation, please refer to the [L2 Signature Documentation](../sign.md).

## Usage Example

```bash
# Get account asset (requires authentication)
curl -X GET "https://api.edgex.exchange/api/v2/private/account/getAccountAsset?accountId=123456" \
  -H "X-edgeX-Api-Key: your_api_key" \
  -H "X-edgeX-Api-Signature: calculated_signature" \
  -H "X-edgeX-Api-Timestamp: 1234567890123"

# Create order (requires authentication + L2 signature)
curl -X POST "https://api.edgex.exchange/api/v2/private/order/createOrder" \
  -H "X-edgeX-Api-Key: your_api_key" \
  -H "X-edgeX-Api-Signature: calculated_signature" \
  -H "X-edgeX-Api-Timestamp: 1234567890123" \
  -H "Content-Type: application/json" \
  -d '{
    "accountId": "123456",
    "contractId": "10000001",
    "price": "50000.0",
    "size": "0.1",
    "side": "BUY",
    "type": "LIMIT",
    "signature": {
      "r": "...",
      "s": "..."
    }
  }'
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

- **Production**: `https://pro.edgex.exchange`
- **Testnet**: `https://testnet.edgex.exchange`

## Best Practices

1. **Secure API Keys** - Never expose API keys in client-side code or public repositories
2. **Implement Signature Verification** - Always verify request signatures to prevent tampering
3. **Handle Rate Limits** - Implement exponential backoff for retries
4. **Use Account ID Validation** - Ensure you have permission to access the account
5. **Check L2 Signature Requirements** - Some operations require L2 signatures for StarkEx settlement

## Error Handling

Common private API error codes:

- `SIGNATURE_ERROR`: Invalid signature
- `ACCOUNT_NOT_FOUND`: Account does not exist
- `INSUFFICIENT_BALANCE`: Insufficient account balance
- `ORDER_NOT_FOUND`: Order does not exist
- `INVALID_PARAMETER`: Invalid request parameter
- `PERMISSION_DENIED`: No permission to access resource

## Navigation

- [← Back to API V2 Documentation](../README.md)
- [Account API →](./account-api.md)
- [Order API →](./order-api.md)
- [Asset API →](./asset-api.md)
- [Transfer API →](./transfer-api.md)
