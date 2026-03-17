# Order Private API

<a id="opIdgetMaxCreateOrderSize"></a>

## POST Get Maximum Order Creation Size

POST /api/v2/private/order/getMaxCreateOrderSize

**Note**: This endpoint requires L2 signature authentication.

> Body Request Parameters

```json
{
  "accountId": "551109015904453258",
  "contractId": "10000001",
  "price": "97463.4"
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|object|No|none|
|» accountId|string(int64)|Yes|Account ID|
|» contractId|string(int64)|Yes|Contract ID|
|» price|string(decimal)|No|Order price|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "maxBuySize": "0.004",
    "maxSellSize": "0.009",
    "ask1Price": "97532.1",
    "bid1Price": "97496.1"
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734665285662",
  "responseTime": "1734665285679",
  "traceId": "38fc6d0f57f3e64c8c6239fae7d35f84"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» maxBuySize|string(decimal)|Maximum buy size|
|» maxSellSize|string(decimal)|Maximum sell size|
|» ask1Price|string(decimal)|Best ask price|
|» bid1Price|string(decimal)|Best bid price|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdcreateOrder"></a>

## POST Create Order

POST /api/v2/private/order/createOrder

**Note**: This endpoint requires L2 signature authentication.

### Minimal Limit Order Checklist

Before calling `createOrder`, make sure you have:

1. A valid private REST signature (`X-edgeX-Api-Key`, `X-edgeX-Passphrase`, `X-edgeX-Api-Timestamp`, `X-edgeX-Api-Signature`)
2. A valid EIP-712 L2 signature payload (`l2Nonce`, `l2Value`, `l2Size`, `l2LimitFee`, `l2ExpireTime`, `l2Signature`)
3. Required business fields such as `accountId`, `contractId`, `side`, `size`, `type`, and `clientOrderId`

For signature construction details, see [Authentication](../authentication.md) and [L2 Signature Guide](../sign.md).

> Body Request Parameters

```json
{
  "accountId": "543429922991899150",
  "contractId": "10000001",
  "side": "BUY",
  "size": "0.001",
  "price": "97393.8",
  "clientOrderId": "21163368294502694",
  "type": "LIMIT",
  "timeInForce": "GOOD_TIL_CANCEL",
  "reduceOnly": false,
  "triggerPrice": "",
  "triggerPriceType": "LAST_PRICE",
  "expireTime": "1736476772359",
  "sourceKey": "",
  "isPositionTpsl": false,
  "isSetOpenTp": false,
  "openTp": null,
  "isSetOpenSl": false,
  "openSl": null,
  "l2Nonce": "808219",
  "l2Value": "97.3938",
  "l2Size": "0.001",
  "l2LimitFee": "0.048697",
  "l2ExpireTime": "1737254372359",
  "l2Signature": "0537b3051bb9ffb98bb842ff6cadf69807a8dbf74f94d8c6106cf4f59b1fd2dc01aa2d93420408d60298831ddb82f7b482574ecb0fc285294dfeec732379ec40",
  "extraType": "",
  "extraDataJson": ""
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|object|No|none|
|» accountId|string(int64)|Yes|**Required** - Account ID (Request validation, will throw exception if empty)|
|» contractId|string(int64)|Yes|**Required** - Contract ID (Request validation, will throw exception if empty)|
|» side|string|Yes|Buy/Sell direction. Enum: `BUY`, `SELL`. **Required by business rules** - defaults to UNKNOWN if not provided|
|» size|string(decimal)|Yes|Order quantity. **Required by business rules** - backend will validate|
|» price|string(decimal)|Yes|Order price (worst acceptable price). **Required by business rules** for limit orders, enter 0 for market orders|
|» clientOrderId|string|Yes|Client-defined ID for idempotency checks. **Required by business rules**|
|» type|string|Yes|Order type. Enum: `LIMIT`, `MARKET`, `STOP_LIMIT`, `STOP_MARKET`, `TAKE_PROFIT_LIMIT`, `TAKE_PROFIT_MARKET`. **Required by business rules** - defaults to UNKNOWN if not provided|
|» timeInForce|string|Yes|Order execution policy. Enum: `GOOD_TIL_CANCEL`, `FILL_OR_KILL`, `IMMEDIATE_OR_CANCEL`, `POST_ONLY`. **Required by business rules** - Market orders must use `IMMEDIATE_OR_CANCEL`|
|» reduceOnly|boolean|No|Whether this is a reduce-only order. Defaults to false if not provided|
|» triggerPrice|string(decimal)|No|Trigger price. Required for conditional orders. Enter 0 or empty string if not applicable|
|» triggerPriceType|string|No|Price type for trigger. Enum: `LAST_PRICE`, `INDEX_PRICE`, `ORACLE_PRICE`, `ASK1_PRICE`, `BID1_PRICE`. Required for conditional orders|
|» expireTime|string(int64)|No|Expiration time in milliseconds. Request parsing accepts empty values, but business validation may reject them (converts to 0)|
|» sourceKey|string|No|Source key, UUID|
|» isPositionTpsl|boolean|No|Whether this is a position take-profit/stop-loss order. Defaults to false|
|» openTpslParentOrderId|string(int64)|No|Order ID of the opening order for TP/SL|
|» isSetOpenTp|boolean|No|Whether take-profit is set for opening order. Defaults to false|
|» openTp|object|No|Take-profit parameters for opening order. Required when `isSetOpenTp` is true|
|» isSetOpenSl|boolean|No|Whether stop-loss is set for opening order. Defaults to false|
|» openSl|object|No|Stop-loss parameters for opening order. Required when `isSetOpenSl` is true|
|» l2Nonce|string(int64)|Yes|L2 signature nonce. First 32 bits of sha256(`clientOrderId`). Request parsing accepts empty values, but business validation may reject them|
|» l2Value|string(decimal)|Yes|L2 signature order value. Request parsing accepts empty values, but business validation may reject them|
|» l2Size|string(decimal)|Yes|L2 signature order quantity. Request parsing accepts empty values, but business validation may reject them|
|» l2LimitFee|string(decimal)|Yes|Maximum acceptable fee for L2 signature. Request parsing accepts empty values, but business validation may reject them|
|» l2ExpireTime|string(int64)|Yes|L2 signature expiration time in milliseconds. Must be >= `expireTime` + 8 days. Request parsing accepts empty values, but business validation may reject them|
|» l2Signature|string|Yes|L2 signature (128 characters). Request parsing accepts empty values, but business validation may reject them but business logic requires valid signature|
|» extraType|string|No|Additional type for upper-layer business use|
|» extraDataJson|string|No|Additional data in JSON format|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "orderId": "564814927928230158"
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734662372560",
  "responseTime": "1734662372575",
  "traceId": "364c0020d1fe90bbfcca3cf3a9d54759"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» orderId|string(int64)|Created order ID|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdcancelOrderById"></a>

## POST Cancel Order by Order ID

POST /api/v2/private/order/cancelOrderById

**Note**: This endpoint requires L2 signature authentication.

> Body Request Parameters

```json
{
  "accountId": "551109015904453258",
  "orderIdList": [
    "564827797948727434"
  ]
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|object|No|none|
|» accountId|string(int64)|Yes|Account ID|
|» orderIdList|array[string]|No|Array of order IDs to cancel|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "cancelResultMap": {
      "564827797948727434": "SUCCESS"
    }
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734665453034",
  "responseTime": "1734665453043",
  "traceId": "5597699fd044ed965bfed23fb3728ba5"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» cancelResultMap|object|Map of order ID to cancellation result|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdcancelOrderByClientOrderId"></a>

## POST Cancel Order by Client Order ID

POST /api/v2/private/order/cancelOrderByClientOrderId

**Note**: This endpoint requires L2 signature authentication. This is a new endpoint in V2 API.

> Body Request Parameters

```json
{
  "accountId": "551109015904453258",
  "clientOrderIdList": [
    "client-order-123",
    "client-order-456"
  ]
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|object|No|none|
|» accountId|string(int64)|Yes|Account ID|
|» clientOrderIdList|array[string]|No|Array of client order IDs to cancel|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "cancelResultMap": {
      "client-order-123": "SUCCESS",
      "client-order-456": "SUCCESS"
    }
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734665453034",
  "responseTime": "1734665453043",
  "traceId": "5597699fd044ed965bfed23fb3728ba5"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» cancelResultMap|object|Map of client order ID to cancellation result|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdcancelAllOrder"></a>

## POST Cancel All Orders

POST /api/v2/private/order/cancelAllOrder

**Note**: This endpoint requires L2 signature authentication.

Cancels all active orders under the account with optional filtering.

> Body Request Parameters

```json
{
  "accountId": "551109015904453258",
  "filterCoinIdList": [],
  "filterContractIdList": [],
  "filterOrderTypeList": [],
  "filterOrderStatusList": [],
  "filterIsPositionTpsl": []
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|object|No|none|
|» accountId|string(int64)|Yes|Account ID|
|» filterCoinIdList|array[string]|No|Filter by collateral coin IDs. If empty, cancels for all coins.|
|» filterContractIdList|array[string]|No|Filter by contract IDs. If empty, cancels for all contracts.|
|» filterOrderTypeList|array[string]|No|Filter by order types. If empty, cancels all types. Enum values: `LIMIT`, `MARKET`, `STOP_LIMIT`, `STOP_MARKET`, `TAKE_PROFIT_LIMIT`, `TAKE_PROFIT_MARKET`|
|» filterOrderStatusList|array[string]|No|Filter by order status. If empty, cancels all statuses. Enum values: `PENDING`, `OPEN`, `UNTRIGGERED`|
|» filterIsPositionTpsl|array[boolean]|No|Filter by position TP/SL orders. If empty, cancels all orders.|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "cancelResultMap": {
      "564828209955209354": "SUCCESS",
      "564828209955209355": "SUCCESS"
    }
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734665771719",
  "responseTime": "1734665771743",
  "traceId": "c0a1da9b75ad55d64ce0e98f86279c34"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» cancelResultMap|object|Map of order ID to cancellation result|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdgetOrderById"></a>

## GET Get Orders by Order IDs

GET /api/v2/private/order/getOrderById

Retrieves orders by account ID and order IDs (batch operation).

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|orderIdList|array[string]|No|Array of order IDs|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": [
    {
      "id": "564829588270612618",
      "userId": "543429922866069763",
      "accountId": "551109015904453258",
      "coinId": "1000",
      "contractId": "10000001",
      "side": "BUY",
      "price": "96260.7",
      "size": "0.001",
      "clientOrderId": "9311381563209122",
      "type": "LIMIT",
      "timeInForce": "GOOD_TIL_CANCEL",
      "reduceOnly": false,
      "triggerPrice": "0",
      "triggerPriceType": "UNKNOWN_PRICE_TYPE",
      "expireTime": "1736480267612",
      "status": "OPEN",
      "cumFillSize": "0",
      "cumFillValue": "0",
      "cumFillFee": "0",
      "createdTime": "1734665867870",
      "updatedTime": "1734665867876"
    }
  ],
  "msg": null,
  "errorParam": null,
  "requestTime": "1734665901264",
  "responseTime": "1734665901281",
  "traceId": "65ca2097b4d9cfb487bd9cef53097040"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|array[object]|Array of order objects|
|» id|string(int64)|Order ID|
|» userId|string(int64)|User ID|
|» accountId|string(int64)|Account ID|
|» coinId|string(int64)|Collateral coin ID|
|» contractId|string(int64)|Contract ID|
|» side|string|Buy/Sell direction. Enum: `BUY`, `SELL`|
|» price|string(decimal)|Order price|
|» size|string(decimal)|Order quantity|
|» clientOrderId|string|Client-defined order ID|
|» type|string|Order type|
|» timeInForce|string|Order execution policy|
|» reduceOnly|boolean|Whether this is a reduce-only order|
|» triggerPrice|string(decimal)|Trigger price|
|» triggerPriceType|string|Trigger price type|
|» expireTime|string(int64)|Expiration time|
|» status|string|Order status. Enum: `PENDING`, `OPEN`, `FILLED`, `CANCELING`, `CANCELED`, `UNTRIGGERED`|
|» cumFillSize|string(decimal)|Cumulative filled quantity|
|» cumFillValue|string(decimal)|Cumulative filled value|
|» cumFillFee|string(decimal)|Cumulative filled fee|
|» createdTime|string(int64)|Creation time|
|» updatedTime|string(int64)|Update time|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdgetOrderByClientOrderId"></a>

## GET Get Orders by Client Order IDs

GET /api/v2/private/order/getOrderByClientOrderId

Retrieves orders by account ID and client order IDs (batch operation).

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|clientOrderIdList|array[string]|No|Array of client-defined order IDs|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": [
    {
      "id": "564829588270612618",
      "userId": "543429922866069763",
      "accountId": "551109015904453258",
      "coinId": "1000",
      "contractId": "10000001",
      "side": "BUY",
      "price": "96260.7",
      "size": "0.001",
      "clientOrderId": "9311381563209122",
      "type": "LIMIT",
      "timeInForce": "GOOD_TIL_CANCEL",
      "reduceOnly": false,
      "status": "OPEN",
      "cumFillSize": "0",
      "cumFillValue": "0",
      "cumFillFee": "0",
      "createdTime": "1734665867870",
      "updatedTime": "1734665867876"
    }
  ],
  "msg": null,
  "errorParam": null,
  "requestTime": "1734665947238",
  "responseTime": "1734665947256",
  "traceId": "64913ab9c62058bc2d9edaeafc3da271"
}
```

### Response Parameters

Same structure as Get Orders by Order IDs endpoint.

<a id="opIdgetActiveOrderPage"></a>

## GET Get Active Orders (Paginated)

GET /api/v2/private/order/getActiveOrderPage

Retrieves active orders under the account with pagination and filtering.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|size|integer|No|Number of items to fetch. Must be > 0 and <= 200. Default: 100|
|offsetData|string|No|Pagination offset. If empty, retrieves the first page.|
|filterCoinIdList|array[string]|No|Filter by collateral coin IDs. If empty, fetches for all coins.|
|filterContractIdList|array[string]|No|Filter by contract IDs. If empty, fetches for all contracts.|
|filterTypeList|array[string]|No|Filter by order types. If empty, fetches all types.|
|filterStatusList|array[string]|No|Filter by order status. If empty, fetches all statuses.|
|filterIsLiquidateList|array[boolean]|No|Filter by liquidation orders. If empty, fetches all orders.|
|filterIsDeleverageList|array[boolean]|No|Filter by deleverage orders. If empty, fetches all orders.|
|filterIsPositionTpslList|array[boolean]|No|Filter by position TP/SL orders. If empty, fetches all orders.|
|filterStartCreatedTimeInclusive|string(int64)|No|Filter orders created after this time (inclusive). If 0 or empty, from earliest.|
|filterEndCreatedTimeExclusive|string(int64)|No|Filter orders created before this time (exclusive). If 0 or empty, to latest.|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "dataList": [
      {
        "id": "564815695875932430",
        "userId": "543429922866069763",
        "accountId": "543429922991899150",
        "coinId": "1000",
        "contractId": "10000001",
        "side": "BUY",
        "price": "97444.5",
        "size": "0.001",
        "clientOrderId": "553364074986685",
        "type": "LIMIT",
        "status": "OPEN",
        "createdTime": "1734662555665",
        "updatedTime": "1734662555672"
      }
    ],
    "nextPageOffsetData": ""
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734662566830",
  "responseTime": "1734662566836",
  "traceId": "4a97b2e8da4933980f399581dd4a1264"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» dataList|array[object]|Array of order objects|
|» nextPageOffsetData|string|Offset for next page. Empty string if no more data.|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdgetHistoryOrderPage"></a>

## GET Get Historical Orders (Paginated)

GET /api/v2/private/order/getHistoryOrderPage

Retrieves historical orders with pagination and filtering.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|size|integer|No|Number of items to fetch. Must be > 0 and <= 100. Default: 100|
|offsetData|string|No|Pagination offset. If empty, retrieves the first page.|
|filterCoinIdList|array[string]|No|Filter by collateral coin IDs. If empty, fetches for all coins.|
|filterContractIdList|array[string]|No|Filter by contract IDs. If empty, fetches for all contracts.|
|filterTypeList|array[string]|No|Filter by order types. If empty, fetches all types.|
|filterStatusList|array[string]|No|Filter by order status. If empty, fetches all statuses.|
|filterIsLiquidateList|array[boolean]|No|Filter by liquidation orders. If empty, fetches all orders.|
|filterIsDeleverageList|array[boolean]|No|Filter by deleverage orders. If empty, fetches all orders.|
|filterIsPositionTpslList|array[boolean]|No|Filter by position TP/SL orders. If empty, fetches all orders.|
|filterStartCreatedTimeInclusive|string(int64)|No|Filter orders created after this time (inclusive). If 0 or empty, from earliest.|
|filterEndCreatedTimeExclusive|string(int64)|No|Filter orders created before this time (exclusive). If 0 or empty, to latest.|
|filterOrderIdList|array[string]|No|Filter by specific order IDs. If empty, fetches all orders.|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "dataList": [
      {
        "id": "564815695875932430",
        "userId": "543429922866069763",
        "accountId": "543429922991899150",
        "coinId": "1000",
        "contractId": "10000001",
        "side": "BUY",
        "price": "97444.5",
        "size": "0.001",
        "clientOrderId": "553364074986685",
        "type": "LIMIT",
        "status": "FILLED",
        "cumFillSize": "0.001",
        "cumFillValue": "97.4445",
        "cumFillFee": "0.017540",
        "createdTime": "1734662555665",
        "updatedTime": "1734662617992"
      }
    ],
    "nextPageOffsetData": ""
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734662697584",
  "responseTime": "1734662697601",
  "traceId": "1cd03694d7da308cb13603f34b0836e6"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» dataList|array[object]|Array of order objects|
|» nextPageOffsetData|string|Offset for next page. Empty string if no more data.|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="opIdgetHistoryOrderPagePost"></a>

## POST Get Historical Orders (Paginated)

POST /api/v2/private/order/getHistoryOrderPage

**Note**: This is a new POST version of the GET endpoint in V2 API, allowing for more complex filtering via request body.

Retrieves historical orders with pagination and filtering.

> Body Request Parameters

```json
{
  "accountId": "543429922991899150",
  "size": 100,
  "offsetData": "",
  "filterCoinIdList": [],
  "filterContractIdList": ["10000001"],
  "filterTypeList": [],
  "filterStatusList": ["FILLED"],
  "filterIsLiquidateList": [],
  "filterIsDeleverageList": [],
  "filterIsPositionTpslList": [],
  "filterStartCreatedTimeInclusive": "0",
  "filterEndCreatedTimeExclusive": "0",
  "filterOrderIdList": []
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|object|No|none|
|» accountId|string(int64)|Yes|Account ID|
|» size|integer|No|Number of items to fetch. Must be > 0 and <= 100. Default: 100|
|» offsetData|string|No|Pagination offset. If empty, retrieves the first page.|
|» filterCoinIdList|array[string]|No|Filter by collateral coin IDs. If empty, fetches for all coins.|
|» filterContractIdList|array[string]|No|Filter by contract IDs. If empty, fetches for all contracts.|
|» filterTypeList|array[string]|No|Filter by order types. If empty, fetches all types.|
|» filterStatusList|array[string]|No|Filter by order status. If empty, fetches all statuses.|
|» filterIsLiquidateList|array[boolean]|No|Filter by liquidation orders. If empty, fetches all orders.|
|» filterIsDeleverageList|array[boolean]|No|Filter by deleverage orders. If empty, fetches all orders.|
|» filterIsPositionTpslList|array[boolean]|No|Filter by position TP/SL orders. If empty, fetches all orders.|
|» filterStartCreatedTimeInclusive|string(int64)|No|Filter orders created after this time (inclusive). If 0 or empty, from earliest.|
|» filterEndCreatedTimeExclusive|string(int64)|No|Filter orders created before this time (exclusive). If 0 or empty, to latest.|
|» filterOrderIdList|array[string]|No|Filter by specific order IDs. If empty, fetches all orders.|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "dataList": [
      {
        "id": "564815695875932430",
        "userId": "543429922866069763",
        "accountId": "543429922991899150",
        "coinId": "1000",
        "contractId": "10000001",
        "side": "BUY",
        "price": "97444.5",
        "size": "0.001",
        "clientOrderId": "553364074986685",
        "type": "LIMIT",
        "status": "FILLED",
        "cumFillSize": "0.001",
        "cumFillValue": "97.4445",
        "cumFillFee": "0.017540",
        "createdTime": "1734662555665",
        "updatedTime": "1734662617992"
      }
    ],
    "nextPageOffsetData": ""
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734662697584",
  "responseTime": "1734662697601",
  "traceId": "1cd03694d7da308cb13603f34b0836e6"
}
```

### Response Parameters

Same structure as GET version of this endpoint.

<a id="opIdgetHistoryOrderFillTransactionPage"></a>

## GET Get Historical Order Fill Transactions (Paginated)

GET /api/v2/private/order/getHistoryOrderFillTransactionPage

Retrieves historical order fill transactions with pagination and filtering.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|size|integer|No|Number of items to fetch. Must be > 0 and <= 100. Default: 100|
|offsetData|string|No|Pagination offset. If empty, retrieves the first page.|
|filterCoinIdList|array[string]|No|Filter by collateral coin IDs. If empty, fetches for all coins.|
|filterContractIdList|array[string]|No|Filter by contract IDs. If empty, fetches for all contracts.|
|filterOrderIdList|array[string]|No|Filter by order IDs. If empty, fetches for all orders.|
|filterIsLiquidateList|array[boolean]|No|Filter by liquidation orders. If empty, fetches all orders.|
|filterIsDeleverageList|array[boolean]|No|Filter by deleverage orders. If empty, fetches all orders.|
|filterIsPositionTpslList|array[boolean]|No|Filter by position TP/SL orders. If empty, fetches all orders.|
|filterStartCreatedTimeInclusive|string(int64)|No|Filter transactions created after this time (inclusive). If 0 or empty, from earliest.|
|filterEndCreatedTimeExclusive|string(int64)|No|Filter transactions created before this time (exclusive). If 0 or empty, to latest.|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "dataList": [
      {
        "id": "564815957260763406",
        "userId": "543429922866069763",
        "accountId": "543429922991899150",
        "coinId": "1000",
        "contractId": "10000001",
        "orderId": "564815695875932430",
        "orderSide": "BUY",
        "fillSize": "0.001",
        "fillValue": "97.4445",
        "fillFee": "0.017540",
        "fillPrice": "97444.5",
        "liquidateFee": "0",
        "realizePnl": "-0.017540",
        "direction": "MAKER",
        "matchTime": "1734662617982",
        "createdTime": "1734662617984",
        "updatedTime": "1734662617992"
      }
    ],
    "nextPageOffsetData": ""
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734662681040",
  "responseTime": "1734662681051",
  "traceId": "770fcce6222c2d88b65b4ecb36e84c43"
}
```

### Response Parameters

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|Response data|
|» dataList|array[object]|Array of order fill transaction objects|
|» nextPageOffsetData|string|Offset for next page. Empty string if no more data.|
|msg|string|Message|
|errorParam|object|Error parameters|
|requestTime|string(timestamp)|Server request receive time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|
