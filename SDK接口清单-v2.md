# EdgeX Golang SDK V2 API Interface List

## Overview

This document provides a comprehensive list of all V2 API endpoints implemented in the EdgeX Golang SDK, with real request/response examples from actual test executions.

**SDK Version**: V2  
**Last Updated**: 2026-03-12  
**Test Environment**: EdgeX Testnet

---

## I. Public API (No Authentication Required)

### 1. Metadata Service

#### 1.1 Get Server Time

**SDK Method**: `GetServerTime(ctx context.Context) (*metadata.ResultGetServerTime, error)`

**API Path**: `GET /api/v2/public/meta/getServerTime`

**Description**: Get current server timestamp

**Request Parameters**: None

**Response Example**:
```json
{
  "code": "SUCCESS",
  "data": {
    "serverTime": "1773302046875"
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302046874",
  "responseTime": "1773302046875",
  "traceId": "b683336aa3441f230d89806eaaca79a6"
}
```

#### 1.2 Get Metadata

**SDK Method**: `GetMetaData(ctx context.Context) (*metadata.ResultMetaData, error)`

**API Path**: `GET /api/v2/public/meta/getMetaData`

**Description**: Get global metadata including coin list, contract list, and multi-chain configuration

**Request Parameters**: None

**Response Fields**:
- `coinList`: List of supported collateral coins
  - `coinId`, `coinName`, `resolution`, `stepSize`, `assetId`, etc.
- `contractList`: List of tradable perpetual contracts
  - `contractId`, `contractName`, `baseCoinId`, `quoteCoinId`, `tickSize`, `minOrderSize`, `maxLeverage`, etc.
- `global`: Global configuration
  - `nativeChainId`, `chainId`, `contractAddress`, `layerTwoChainId`, etc.
- `multiChainList`: Multi-chain deposit/withdrawal configuration

---

### 2. Quote Service

#### 2.1 Get 24-Hour Ticker

**SDK Method**: `Get24HourQuote(ctx context.Context, contractId string) (*quote.ResultListTicker, error)`

**API Path**: `GET /api/v2/public/quote/getTicker`

**Description**: Query 24-hour ticker data for a specific contract

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string | Yes | Contract ID (e.g., "10000001" for BTCUSDC) |

**Real Request**:
```
GET /api/v2/public/quote/getTicker?contractId=10000001
```

**Real Response**:
```json
{
  "code": "SUCCESS",
  "data": [{
    "contractId": "10000001",
    "contractName": "BTCUSDC",
    "lastPrice": "69735.9",
    "markPrice": "69810.2",
    "indexPrice": "69840.778565787",
    "open": "68880.1",
    "high": "71325.2",
    "low": "68658.9",
    "close": "69735.9",
    "fundingRate": "0.00000208",
    "fundingTime": "1773301800000",
    "volume": "43.218",
    "value": "3006471.2907",
    "trades": "94345",
    "openTime": "1773215100000",
    "closeTime": "1773301500000"
  }],
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302047031",
  "responseTime": "1773302047032",
  "traceId": "92fbe6f0c5204b92deaa0d51256211a1"
}
```

#### 2.2 Get Ticker Summary

**SDK Method**: `GetQuoteSummary(ctx context.Context, contractID string) (*quote.ResultGetTickerSummaryModel, error)`

**API Path**: `GET /api/v2/public/quote/getTicketSummary`

**Description**: Get ticker summary for a specific contract

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string | Yes | Contract ID |

#### 2.3 Get K-Line Data

**SDK Method**: `GetKLine(ctx context.Context, params quote.GetKLineParams) (*quote.ResultPageDataKline, error)`

**API Path**: `GET /api/v2/public/quote/getKline`

**Description**: Query K-line (candlestick) data with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string | Yes | Contract ID |
| `priceType` | string | Yes | Price type: `LAST_PRICE`, `MARK_PRICE`, `INDEX_PRICE` |
| `interval` | string | Yes | Time interval: `1m`, `5m`, `15m`, `30m`, `1h`, `4h`, `1d`, `1w` |
| `startTime` | uint64 | No | Start timestamp (milliseconds, inclusive) |
| `endTime` | uint64 | No | End timestamp (milliseconds, exclusive) |
| `size` | int | No | Number of records to return (default: 100, max: 1000) |
| `offsetData` | string | No | Pagination offset token |

#### 2.4 Get Order Book Depth

**SDK Method**: `GetOrderBookDepth(ctx context.Context, params quote.GetOrderBookDepthParams) (*quote.ResultListDepth, error)`

**API Path**: `GET /api/v2/public/quote/getDepth`

**Description**: Query order book depth (bids and asks)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string | Yes | Contract ID |
| `depth` | int | No | Depth level: `5`, `15`, `50`, `100` (default: 15) |

#### 2.5 Get Multi-Contract K-Line

**SDK Method**: `GetMultiContractKLine(ctx context.Context, params quote.GetMultiContractKLineParams) (*quote.ResultListContractKline, error)`

**API Path**: `GET /api/v2/public/quote/getMultiContractKLine`

**Description**: Query K-line data for multiple contracts simultaneously

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractIdList` | []string | Yes | List of contract IDs (comma-separated) |
| `priceType` | string | Yes | Price type: `LAST_PRICE`, `MARK_PRICE`, `INDEX_PRICE` |
| `interval` | string | Yes | Time interval |

---

### 3. Funding Rate Service

#### 3.1 Get Latest Funding Rate

**SDK Method**: `GetLatestFundingRate(ctx context.Context, params funding.GetLatestFundingRateParams) (*funding.ResultListFundingRate, error)`

**API Path**: `GET /api/v2/public/funding/getLatestFundingRate`

**Description**: Query latest funding rate for specified contracts

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string | No | Single contract ID or comma-separated list |

**Real Request**:
```
GET /api/v2/public/funding/getLatestFundingRate?contractId=10000001
```

**Real Response**:
```json
{
  "code": "SUCCESS",
  "data": [{
    "contractId": "10000001",
    "fundingTime": "1773301800000",
    "fundingTimestamp": "1772622060000",
    "oraclePrice": "69796.12355079",
    "indexPrice": "69796.123551790",
    "fundingRate": "0.00000208",
    "isSettlement": true,
    "forecastFundingRate": "0.00000208",
    "previousFundingRate": "0.00000208",
    "previousFundingTimestamp": "1772622000000",
    "premiumIndex": "0.0001",
    "avgPremiumIndex": "0",
    "premiumIndexTimestamp": "1772622060000",
    "impactMarginNotional": "500",
    "impactAskPrice": "69785.8",
    "impactBidPrice": "69784.6",
    "fundingRateIntervalMin": "10"
  }],
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302060386",
  "responseTime": "1773302060393",
  "traceId": "e9c9359958245aa730c9c0be120a2f4f"
}
```

#### 3.2 Get Funding Rate Page

**SDK Method**: `GetFundingRate(ctx context.Context, params funding.GetFundingRateParams) (*funding.ResultPageDataFundingRate, error)`

**API Path**: `GET /api/v2/public/funding/getFundingRatePage`

**Description**: Query historical funding rate records with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string | Yes | Contract ID |
| `startTime` | uint64 | No | Start timestamp (milliseconds, inclusive) |
| `endTime` | uint64 | No | End timestamp (milliseconds, exclusive) |
| `size` | int | No | Page size (default: 100, max: 500) |
| `offsetData` | string | No | Pagination offset token from previous response |

---

## II. Private API (Authentication Required)

All private APIs require L2 signature authentication. See [Authentication Documentation](./api-v2/authentication.md) for details.

### 1. Account Service

#### 1.1 Get Account Asset

**SDK Method**: `GetAccountAsset(ctx context.Context) (*account.GetAccountAssetResponse, error)`

**API Path**: `GET /api/v2/private/account/getAccountAsset`

**Description**: Get complete account asset information including positions and collaterals

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID (auto-filled by SDK) |

**Real Request**:
```
GET /api/v2/private/account/getAccountAsset?accountId=724625476626153743
```

**Real Response**:
```json
{
  "code": "SUCCESS",
  "data": {
    "account": {
      "id": "724625476626153743",
      "userId": "600162890833461506",
      "accountName": "main",
      "clientAccountId": "main",
      "ethAddress": "0x...",
      "l2Key": "0x...",
      "status": "NORMAL",
      "isLiquidating": false,
      "contractIdToTradeSetting": {
        "10000001": {
          "maxLeverage": "10",
          "isSetMaxLeverage": true
        }
      }
    },
    "positionList": [{
      "contractId": "10000001",
      "size": "0",
      "openSize": "0",
      "openValue": "0"
    }],
    "collateralList": [{
      "coinId": "1000",
      "amount": "18.958424",
      "totalAmount": "18.958424",
      "availableAmount": "18.958424",
      "lockedAmount": "0"
    }]
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302057455",
  "responseTime": "1773302057742",
  "traceId": "1d6cfd37d3554f5fb1f9b982bd10ebba"
}
```

#### 1.2 Get Account Positions

**SDK Method**: `GetAccountPositions(ctx context.Context) (*account.ListPositionResponse, error)`

**API Path**: `GET /api/v2/private/account/getAccountAsset` (extracts position data)

**Description**: Get account positions (extracted from GetAccountAsset response)

#### 1.3 Get Position by Contract ID

**SDK Method**: `GetPositionByContractID(ctx context.Context, contractIDs []string) (*account.ListPositionResponse, error)`

**API Path**: `GET /api/v2/private/account/getPositionByContractId`

**Description**: Get position information for specific contracts

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractIdList` | string | Yes | Comma-separated contract IDs |

#### 1.4 Get Position Transaction Page

**SDK Method**: `GetPositionTransactionPage(ctx context.Context, params account.GetPositionTransactionPageParams) (*account.PageDataPositionTransactionResponse, error)`

**API Path**: `GET /api/v2/private/account/getPositionTransactionPage`

**Description**: Get position transaction history with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | No | Filter by contract ID |
| `startTime` | uint64 | No | Start time filter (milliseconds) |
| `endTime` | uint64 | No | End time filter (milliseconds) |
| `size` | int | No | Page size (default: 100, max: 500) |
| `offsetData` | string | No | Pagination offset token |

#### 1.5 Get Position Transaction by ID

**SDK Method**: `GetPositionTransactionByID(ctx context.Context, transactionIDs []string) (*account.ListPositionTransactionResponse, error)`

**API Path**: `GET /api/v2/private/account/getPositionTransactionById`

**Description**: Batch query position transactions by IDs

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `idList` | string | Yes | Comma-separated transaction IDs |

#### 1.6 Get Position Term Page

**SDK Method**: `GetPositionTermPage(ctx context.Context, params account.GetPositionTermPageParams) (*account.PageDataPositionTermResponse, error)`

**API Path**: `GET /api/v2/private/account/getPositionTermPage`

**Description**: Get position term records (each opening/closing cycle) with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | No | Filter by contract ID |
| `startTime` | uint64 | No | Start time filter |
| `endTime` | uint64 | No | End time filter |
| `size` | int | No | Page size |
| `offsetData` | string | No | Pagination offset token |

#### 1.7 Get Position Orders ⭐ V2 New

**SDK Method**: `GetPositionOrders(ctx context.Context, params account.GetPositionOrdersParams) (*account.GetPositionOrdersResponse, error)`

**API Path**: `GET /api/v2/private/account/getPositionOrders`

**Description**: Get historical orders associated with a specific position term

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `positionTermId` | string(int64) | Yes | Position term ID |

#### 1.8 Get Collateral by Coin ID

**SDK Method**: `GetCollateralByCoinID(ctx context.Context, coinIDs []string) (*account.ListCollateralResponse, error)`

**API Path**: `GET /api/v2/private/account/getCollateralByCoinId`

**Description**: Get collateral information for specific coins

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinIdList` | string | Yes | Comma-separated coin IDs |

#### 1.9 Get Collateral Transaction Page

**SDK Method**: `GetCollateralTransactionPage(ctx context.Context, params account.GetCollateralTransactionPageParams) (*account.PageDataCollateralTransactionResponse, error)`

**API Path**: `GET /api/v2/private/account/getCollateralTransactionPage`

**Description**: Get collateral transaction history with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | No | Filter by coin ID |
| `startTime` | uint64 | No | Start time filter |
| `endTime` | uint64 | No | End time filter |
| `size` | int | No | Page size |
| `offsetData` | string | No | Pagination offset token |

#### 1.10 Get Collateral Transaction by ID

**SDK Method**: `GetCollateralTransactionByID(ctx context.Context, transactionIDs []string) (*account.ListCollateralTransactionResponse, error)`

**API Path**: `GET /api/v2/private/account/getCollateralTransactionById`

**Description**: Batch query collateral transactions by IDs

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `idList` | string | Yes | Comma-separated transaction IDs |

#### 1.11 Get Account by ID

**SDK Method**: `GetAccountByID(ctx context.Context) (*account.AccountResponse, error)`

**API Path**: `GET /api/v2/private/account/getAccountById`

**Description**: Get account basic information

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |

#### 1.12 Get Account Deleverage Light

**SDK Method**: `GetAccountDeleverageLight(ctx context.Context) (*account.GetAccountDeleverageLightResponse, error)`

**API Path**: `GET /api/v2/private/account/getAccountDeleverageLight`

**Description**: Get account ADL (Auto-Deleveraging) indicator light status

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |

#### 1.13 Get Account Asset Snapshot Page

**SDK Method**: `GetAccountAssetSnapshotPage(ctx context.Context, params account.GetAccountAssetSnapshotPageParams) (*account.PageDataAccountAssetSnapshotResponse, error)`

**API Path**: `GET /api/v2/private/account/getAccountAssetSnapshotPage`

**Description**: Get account asset snapshots (daily snapshots for historical tracking)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | No | Filter by coin ID |
| `size` | int | No | Page size |
| `offsetData` | string | No | Pagination offset token |

**Real Request**:
```
GET /api/v2/private/account/getAccountAssetSnapshotPage?accountId=724625476626153743&coinId=1000&size=10
```

**Real Response**:
```json
{
  "code": "SUCCESS",
  "data": {
    "dataList": [{
      "accountId": "724625476626153743",
      "coinId": "1000",
      "snapshotTime": "1773291600000",
      "totalEquity": "88.875359",
      "unrealizePnl": "0.000000",
      "totalRealizePnl": "-6.302000"
    }],
    "nextPageOffsetData": "0880ACF0F3CD33"
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302058028",
  "responseTime": "1773302058308",
  "traceId": "2521376ff19130003837645a24f931d3"
}
```

#### 1.14 Update Leverage Setting ⭐ V2 New

**SDK Method**: `UpdateLeverageSetting(ctx context.Context, contractID string, leverage string) error`

**API Path**: `POST /api/v2/private/account/updateLeverageSetting`

**Description**: Update leverage setting for a specific contract

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | Yes | Contract ID |
| `maxLeverage` | string(decimal) | Yes | Maximum leverage (e.g., "10", "20") |

---

### 2. Asset Service

#### 2.1 Get All Orders Page

**SDK Method**: `GetAllOrdersPage(ctx context.Context, params asset.GetAllOrdersPageParams) (*asset.ResultPageDataAssetOrder, error)`

**API Path**: `GET /api/v2/private/assets/getAllOrdersPage`

**Description**: Query deposit and withdrawal order history with pagination and filtering

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | No | Filter by coin ID |
| `typeList` | string | No | Order types filter (comma-separated): `ORDER_TYPE_NORMAL_DEPOSIT`, `ORDER_TYPE_NORMAL_WITHDRAW`, etc. |
| `startTime` | uint64 | No | Start time (inclusive, milliseconds) |
| `endTime` | uint64 | No | End time (exclusive, milliseconds) |
| `size` | int | No | Page size (max: 500) |
| `offsetData` | string | No | Pagination offset token |

**Note**: The `typeList` parameter accepts the following order types:
- `ORDER_TYPE_NORMAL_DEPOSIT` - Normal deposit
- `ORDER_TYPE_NORMAL_WITHDRAW` - Normal withdrawal
- `ORDER_TYPE_CROSS_DEPOSIT` - Cross-chain deposit
- `ORDER_TYPE_CROSS_WITHDRAW` - Cross-chain withdrawal
- `ORDER_TYPE_FAST_WITHDRAW` - Fast withdrawal
- And 6 additional internal types

#### 2.2 Create Normal Withdraw

**SDK Method**: `CreateNormalWithdraw(ctx context.Context, params *asset.CreateNormalWithdrawParams, md *metadata.MetaData) (*asset.ResultCreateNormalWithdraw, error)`

**API Path**: `POST /api/v2/private/assets/createNormalWithdraw`

**Description**: Create a normal withdrawal order (auto L2 signature)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | Yes | Coin ID to withdraw |
| `amount` | string(decimal) | Yes | Withdrawal amount |
| `ethAddress` | string | Yes | Target Ethereum address |
| `erc20Address` | string | No | ERC20 token contract address (if applicable) |
| `clientWithdrawId` | string | No | Client-defined withdrawal ID for idempotency |
| `l2Nonce` | string(int64) | Yes | L2 nonce (auto-generated by SDK) |
| `l2ExpireTime` | string(int64) | Yes | L2 expiration time (auto-generated by SDK) |
| `l2Signature` | string | Yes | L2 signature (auto-generated by SDK) |

**Note**: SDK automatically handles L2 signature generation. Users only need to provide `accountId`, `coinId`, `amount`, and `ethAddress`.

#### 2.3 Get Withdraw Sign Info

**SDK Method**: `GetWithdrawSignInfo(ctx context.Context, params asset.GetWithdrawSignInfoParams) (*asset.ResultGetWithdrawSignInfo, error)`

**API Path**: `GET /api/v2/private/assets/getNormalWithdrawSignInfo` or `GET /api/v2/private/assets/getCrossWithdrawSignInfo`

**Description**: Get withdrawal signature information (for cross-chain or fast withdrawal)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | Yes | Coin ID |

#### 2.4 Create Cross Withdraw

**SDK Method**: `CreateCrossWithdraw(ctx context.Context, params asset.CreateCrossWithdrawParams) (*asset.ResultCreateCrossWithdraw, error)`

**API Path**: `POST /api/v2/private/assets/createCrossWithdraw`

**Description**: Create a cross-chain withdrawal order

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | Yes | Coin ID |
| `chainId` | string(int64) | Yes | Target chain ID |
| `amount` | string(decimal) | Yes | Withdrawal amount |
| `toAddress` | string | Yes | Target address on destination chain |
| `clientWithdrawId` | string | No | Client withdrawal ID |
| `l2Signature` | string | Yes | L2 signature |
| Additional L2 params | | Yes | L2 nonce, expireTime, etc. |

---

### 3. Order Service

#### 3.1 Create Order

**SDK Method**: `CreateOrder(ctx context.Context, params *order.CreateOrderParams, metadata *metadatapkg.MetaData, l2Price decimal.Decimal) (*order.ResultCreateOrder, error)`

**API Path**: `POST /api/v2/private/order/createOrder`

**Description**: Create a trading order with automatic L2 signature

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `contractId` | string(int64) | Yes | Contract ID |
| `side` | string | Yes | Order side: `BUY` or `SELL` |
| `type` | string | Yes | Order type: `LIMIT`, `MARKET`, `STOP_LIMIT`, `STOP_MARKET`, `TAKE_PROFIT_LIMIT`, `TAKE_PROFIT_MARKET` |
| `price` | string(decimal) | Yes* | Order price (*not required for MARKET orders) |
| `size` | string(decimal) | Yes | Order size |
| `timeInForce` | string | Yes | Time in force: `GOOD_TIL_CANCEL`, `IMMEDIATE_OR_CANCEL`, `FILL_OR_KILL`, `POST_ONLY` |
| `clientOrderId` | string | No | Client-defined order ID for idempotency |
| `reduceOnly` | boolean | No | Whether this is a reduce-only order (default: false) |
| `triggerPrice` | string(decimal) | No | Trigger price for conditional orders (STOP/TAKE_PROFIT types) |
| `triggerPriceType` | string | No | Trigger price type: `LAST_PRICE`, `MARK_PRICE` (default: LAST_PRICE) |
| `isPositionTpsl` | boolean | No | Whether this is a position TP/SL order |
| `isSetOpenTp` | boolean | No | Whether to attach take-profit order on creation |
| `openTp` | object | No | Take-profit order parameters (if isSetOpenTp = true) |
| `isSetOpenSl` | boolean | No | Whether to attach stop-loss order on creation |
| `openSl` | object | No | Stop-loss order parameters (if isSetOpenSl = true) |
| L2 signature fields | | Yes | `l2Nonce`, `l2Value`, `l2Size`, `l2LimitFee`, `l2ExpireTime`, `l2Signature` (auto-generated by SDK) |

**Real Request** (LIMIT Order):
```json
{
  "accountId": "724625476626153743",
  "contractId": "10000001",
  "side": "SELL",
  "type": "LIMIT",
  "price": "71074.8",
  "size": "0.001",
  "timeInForce": "GOOD_TIL_CANCEL",
  "clientOrderId": "sdk-v2-1773302047079667844",
  "reduceOnly": false,
  "l2Nonce": "2637799806",
  "l2Value": "71.0748",
  "l2Size": "0.001",
  "l2LimitFee": "1",
  "l2ExpireTime": "1775894047079",
  "expireTime": "1775202847079",
  "l2Signature": "0xfdd3e520d84f4b8214bd34b1b6ba5013852dcd668d90711b..."
}
```

**Real Response**:
```json
{
  "code": "SUCCESS",
  "data": {
    "orderId": "726881470857084943"
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302047172",
  "responseTime": "1773302047459",
  "traceId": "1f61347b84a2c6ee461a8aa9b8e0b41d"
}
```

**Real Request** (STOP_MARKET Order):
```json
{
  "accountId": "724625476626153743",
  "contractId": "10000001",
  "side": "SELL",
  "type": "STOP_MARKET",
  "price": "0",
  "size": "0.001",
  "triggerPrice": "66042.9",
  "triggerPriceType": "LAST_PRICE",
  "timeInForce": "IMMEDIATE_OR_CANCEL",
  "clientOrderId": "stop-market-sl-1773300663768631932",
  "reduceOnly": true,
  "l2Nonce": "2012469256",
  "l2Value": "66.0429",
  "l2Size": "0.001",
  "l2LimitFee": "0",
  "l2ExpireTime": "1775892663768",
  "expireTime": "1775201463768",
  "l2Signature": "0xa2bc232fd3fe3551c24b1c55449eab55a0a3829b..."
}
```

#### 3.2 Cancel Order by ID

**SDK Method**: `CancelOrder(ctx context.Context, params *order.CancelOrderParams) (interface{}, error)` (with `OrderId` set)

**API Path**: `POST /api/v2/private/order/cancelOrderById`

**Description**: Cancel one or more orders by order ID

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `orderIdList` | []string | Yes | List of order IDs to cancel |

**Real Request**:
```json
{
  "accountId": "724625476626153743",
  "orderIdList": ["726881470857084943"]
}
```

**Real Response**:
```json
{
  "code": "SUCCESS",
  "data": {
    "cancelResultMap": {
      "726881470857084943": "SUCCESS"
    }
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1773302047580",
  "responseTime": "1773302047923",
  "traceId": "78fdfbf5c16f484d87e832ad810d026f"
}
```

#### 3.3 Cancel Order by Client Order ID ⭐ V2 New

**SDK Method**: `CancelOrder(ctx context.Context, params *order.CancelOrderParams) (interface{}, error)` (with `ClientId` set)

**API Path**: `POST /api/v2/private/order/cancelOrderByClientOrderId`

**Description**: Cancel orders by client-defined order ID

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `clientOrderIdList` | []string | Yes | List of client order IDs to cancel |

#### 3.4 Cancel All Orders by Contract

**SDK Method**: `CancelOrder(ctx context.Context, params *order.CancelOrderParams) (interface{}, error)` (with `ContractId` set)

**API Path**: `POST /api/v2/private/order/cancelAllOrder`

**Description**: Cancel all active orders for a specific contract

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `filterContractIdList` | []string | No | List of contract IDs to filter (cancels orders for these contracts) |

#### 3.5 Get Maximum Order Size

**SDK Method**: `GetMaxOrderSize(ctx context.Context, contractID string, price decimal.Decimal) (*order.ResultGetMaxCreateOrderSize, error)`

**API Path**: `POST /api/v2/private/order/getMaxCreateOrderSize`

**Description**: Get maximum order size that can be created at a given price

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | Yes | Contract ID |
| `price` | string(decimal) | Yes | Order price |

**Response Fields**:
- `maxBuySize`: Maximum size for buy orders
- `maxSellSize`: Maximum size for sell orders
- `ask1Price`: Best ask price
- `bid1Price`: Best bid price

#### 3.6 Get Orders by ID

**SDK Method**: `GetOrdersByID(ctx context.Context, orderIDs []string) (*order.ResultListOrder, error)`

**API Path**: `GET /api/v2/private/order/getOrderById`

**Description**: Batch query orders by order IDs

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `orderIdList` | string | Yes | Comma-separated order IDs |

#### 3.7 Get Orders by Client Order ID

**SDK Method**: `GetOrdersByClientOrderID(ctx context.Context, clientOrderIDs []string) (*order.ResultListOrder, error)`

**API Path**: `GET /api/v2/private/order/getOrderByClientOrderId`

**Description**: Batch query orders by client-defined order IDs

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `clientOrderIdList` | string | Yes | Comma-separated client order IDs |

#### 3.8 Get Active Orders

**SDK Method**: `GetActiveOrders(ctx context.Context, params *order.GetActiveOrderParams) (*order.ResultPageDataOrder, error)`

**API Path**: `GET /api/v2/private/order/getActiveOrderPage`

**Description**: Get active (open/untriggered) orders with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | No | Filter by contract ID |
| `startTime` | uint64 | No | Start time filter |
| `endTime` | uint64 | No | End time filter |
| `size` | int | No | Page size |
| `offsetData` | string | No | Pagination offset token |

#### 3.9 Get History Order Page

**SDK Method**: `GetHistoryOrderPage(ctx context.Context, params *order.GetHistoryOrderPageParams) (*order.GetHistoryOrderPageResponse, error)`

**API Path**: `POST /api/v2/private/order/getHistoryOrderPage` ⭐ V2 New (POST method)

**Description**: Get historical orders with advanced filtering (supports POST method for complex filters)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | No | Filter by contract ID |
| `orderType` | string | No | Filter by order type |
| `orderSide` | string | No | Filter by order side |
| `startTime` | uint64 | No | Start time filter |
| `endTime` | uint64 | No | End time filter |
| `size` | int | No | Page size |
| `offsetData` | string | No | Pagination offset token |

#### 3.10 Get Order Fill Transactions

**SDK Method**: `GetOrderFillTransactions(ctx context.Context, params *order.OrderFillTransactionParams) (*order.ResultPageDataOrderFillTransaction, error)`

**API Path**: `GET /api/v2/private/order/getHistoryOrderFillTransactionPage`

**Description**: Get order fill/execution transaction history with pagination

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `contractId` | string(int64) | No | Filter by contract ID |
| `orderId` | string(int64) | No | Filter by specific order ID |
| `startTime` | uint64 | No | Start time filter |
| `endTime` | uint64 | No | End time filter |
| `size` | int | No | Page size |
| `offsetData` | string | No | Pagination offset token |

---

### 4. Transfer Service

#### 4.1 Create Transfer Out

**SDK Method**: `CreateTransferOut(ctx context.Context, params *transfer.CreateTransferOutParams, metadata *metadatapkg.MetaData) (*transfer.ResultCreateTransferOut, error)`

**API Path**: `POST /api/v2/private/transfer/createTransferOut`

**Description**: Create a transfer-out order (transfer from trading account to wallet)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | Yes | Coin ID |
| `amount` | string(decimal) | Yes | Transfer amount |
| `clientTransferOutId` | string | No | Client transfer ID for idempotency |
| L2 signature fields | | Yes | Auto-generated by SDK |

#### 4.2 Get Transfer Out by ID

**SDK Method**: `GetTransferOutById(ctx context.Context, params transfer.GetTransferOutByIdParams) (*transfer.ResultListTransferOut, error)`

**API Path**: `GET /api/v2/private/transfer/getTransferOutById`

**Description**: Query transfer-out orders by IDs

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `idList` | string | Yes | Comma-separated transfer-out IDs |

#### 4.3 Get Transfer In by ID

**SDK Method**: `GetTransferInById(ctx context.Context, params transfer.GetTransferInByIdParams) (*transfer.ResultListTransferIn, error)`

**API Path**: `GET /api/v2/private/transfer/getTransferInById`

**Description**: Query transfer-in orders by IDs

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `idList` | string | Yes | Comma-separated transfer-in IDs |

#### 4.4 Get Withdraw Available Amount

**SDK Method**: `GetWithdrawAvailableAmount(ctx context.Context, params transfer.GetWithdrawAvailableAmountParams) (*transfer.ResultGetTransferOutAvailableAmount, error)`

**API Path**: `GET /api/v2/private/transfer/getTransferOutAvailableAmount`

**Description**: Get available amount for transfer-out (withdrawal from trading account)

**Request Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountId` | string(int64) | Yes | Account ID |
| `coinId` | string(int64) | Yes | Coin ID |

---

## III. WebSocket API

EdgeX SDK provides WebSocket support for real-time market data and account updates.

### WebSocket Connection URLs

- **Public WebSocket**: `wss://[domain]/api/v1/public/ws`
- **Private WebSocket**: `wss://[domain]/api/v1/private/ws`

**Note**: WebSocket connections use V1 endpoint paths (not V2) as per API specification.

### 1. Public WebSocket

**SDK Methods**:
- `ConnectPublic(ctx context.Context) error` - Establish public WebSocket connection
- `SubscribeMarketTicker(contractID string, handler MessageHandler) error` - Subscribe to 24-hour ticker
- `SubscribeKLine(contractID string, interval string, handler MessageHandler) error` - Subscribe to K-line data
- `SubscribeDepth(contractID string, handler MessageHandler) error` - Subscribe to order book depth
- `SubscribeTrades(contractID string, handler MessageHandler) error` - Subscribe to latest trades

**Supported Channels**:
| Channel Format | Description | Example |
|----------------|-------------|---------|
| `ticker.{contractId}` | 24-hour ticker for specific contract | `ticker.10000001` |
| `ticker.all` | All contracts ticker | `ticker.all` |
| `ticker.all.1s` | All contracts ticker (1-second updates) | `ticker.all.1s` |
| `kline.{priceType}.{contractId}.{interval}` | K-line data | `kline.LAST_PRICE.10000001.1m` |
| `depth.{contractId}.{depth}` | Order book depth | `depth.10000001.15` |
| `trades.{contractId}` | Latest trades | `trades.10000001` |

**Intervals for K-line**: `1m`, `5m`, `15m`, `30m`, `1h`, `4h`, `1d`, `1w`

**Depth Levels**: `5`, `15`, `50`, `100`

### 2. Private WebSocket

**SDK Methods**:
- `ConnectPrivate(ctx context.Context) error` - Establish private WebSocket connection with authentication
- `OnPrivateMessage(msgType string, handler MessageHandler) error` - Register handler for specific event type

**Auto-Push Events** (No Subscription Required):

After successful authentication, the server automatically pushes the following events:

| Event Type | Description |
|------------|-------------|
| `Snapshot` | Initial snapshot of account state (sent immediately after connection) |
| `ACCOUNT_UPDATE` | Account information updated |
| `DEPOSIT_UPDATE` | Deposit record updated |
| `WITHDRAW_UPDATE` | Withdrawal record updated |
| `TRANSFER_IN_UPDATE` | Transfer-in record updated |
| `TRANSFER_OUT_UPDATE` | Transfer-out record updated |
| `ORDER_UPDATE` | Order status or details updated |
| `FORCE_WITHDRAW_UPDATE` | Forced withdrawal event |
| `FORCE_TRADE_UPDATE` | Forced trade event (liquidation/ADL) |
| `FUNDING_SETTLEMENT` | Funding fee settlement |
| `ORDER_FILL_FEE_INCOME` | Order fill fee income event |
| `START_LIQUIDATING` | Account liquidation started |
| `FINISH_LIQUIDATING` | Account liquidation finished |

**Message Structure**:
```json
{
  "type": "trade-event",
  "content": {
    "event": "ORDER_UPDATE",
    "version": "1000",
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
    }
  }
}
```

**Data Arrays**: Each event may populate one or more of the data arrays based on what changed:
- `account`: Account information
- `collateral`: Collateral (margin) balances
- `collateralTransaction`: Collateral transaction records
- `position`: Position information
- `positionTransaction`: Position transaction records
- `deposit`: Deposit records
- `withdraw`: Withdrawal records
- `transferIn`: Transfer-in records
- `transferOut`: Transfer-out records
- `order`: Order records
- `orderFillTransaction`: Order fill transaction records

---

## IV. Summary

### API Statistics

| Service Module | Public Endpoints | Private Endpoints | Total |
|----------------|------------------|-------------------|-------|
| **Metadata** | 2 | 0 | 2 |
| **Quote** | 5 | 0 | 5 |
| **Funding** | 2 | 0 | 2 |
| **Account** | 0 | 14 | 14 |
| **Asset** | 0 | 4 | 4 |
| **Order** | 0 | 10 | 10 |
| **Transfer** | 0 | 4 | 4 |
| **WebSocket** | 1 connection | 1 connection | 2 |
| **Total** | **9 + 1 WS** | **32 + 1 WS** | **41 + 2 WS** |

### V2 New Features

The following features are new or enhanced in V2:

1. **`UpdateLeverageSetting`** - Update account leverage for specific contracts
2. **`GetPositionOrders`** - Get historical orders associated with a position term
3. **`CancelOrderByClientOrderId`** - Cancel orders using client-defined IDs
4. **`GetHistoryOrderPage` (POST method)** - Enhanced historical order query with complex filters in request body
5. **Conditional Order Support** - Full support for STOP and TAKE_PROFIT order types with:
   - `triggerPrice` - Conditional trigger price
   - `triggerPriceType` - Trigger based on LAST_PRICE or MARK_PRICE
   - `isSetOpenTp` / `openTp` - Attach take-profit orders on creation
   - `isSetOpenSl` / `openSl` - Attach stop-loss orders on creation
   - `isPositionTpsl` - Position-level TP/SL orders

### Order Types Supported

| Order Type | Execution | Trigger Condition | Use Case |
|-----------|-----------|-------------------|----------|
| `LIMIT` | At specified price or better | Immediate | Normal limit order |
| `MARKET` | At current market price | Immediate | Quick execution |
| `STOP_LIMIT` | At specified price | When trigger price reached | Stop-loss with price protection |
| `STOP_MARKET` | At market price | When trigger price reached | Stop-loss at market |
| `TAKE_PROFIT_LIMIT` | At specified price | When profit target reached | Take profit with price guarantee |
| `TAKE_PROFIT_MARKET` | At market price | When profit target reached | Take profit at market |

### Time In Force Options

| TIF | Description |
|-----|-------------|
| `GOOD_TIL_CANCEL` | Order remains active until filled or cancelled |
| `IMMEDIATE_OR_CANCEL` | Execute immediately, cancel unfilled portion |
| `FILL_OR_KILL` | Execute completely immediately or cancel entirely |
| `POST_ONLY` | Only add liquidity (maker), cancel if would take liquidity |

---

## V. Authentication

- **Public API**: No authentication required
- **Private API**: Requires L2 signature authentication via HTTP headers:
  - `X-edgeX-Api-Signature`: Your L2 EIP-712 signature
  - `X-edgeX-Api-Timestamp`: Current timestamp in milliseconds
- **WebSocket**:
  - Public: No authentication
  - Private: Authentication via custom headers or `SEC_WEBSOCKET_PROTOCOL`

For detailed authentication and signature methods, please refer to:
- [Authentication Documentation](./api-v2/authentication.md)
- [L2 Signature Documentation](./api-v2/sign.md)

---

**Document Version**: V2.1  
**Last Updated**: 2026-03-12  
**Tested Against**: EdgeX Testnet  
**SDK Repository**: [edgex-golang-sdk](https://github.com/edgex-Tech/edgex-golang-sdk)
