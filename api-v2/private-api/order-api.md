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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|no|none|
|» accountId|body|string(int64)|yes|Account ID|
|» contractId|body|string(int64)|yes|Contract ID|
|» price|body|string(decimal)|yes|Order price|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» maxBuySize|string(decimal)|true|Maximum buy size|
|» maxSellSize|string(decimal)|true|Maximum sell size|
|» ask1Price|string(decimal)|true|Best ask price|
|» bid1Price|string(decimal)|true|Best bid price|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

<a id="opIdcreateOrder"></a>

## POST Create Order

POST /api/v2/private/order/createOrder

**Note**: This endpoint requires L2 signature authentication.

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|no|none|
|» accountId|body|string(int64)|yes|**Required** - Account ID (HTTP layer validates, will throw exception if empty)|
|» contractId|body|string(int64)|yes|**Required** - Contract ID (HTTP layer validates, will throw exception if empty)|
|» side|body|string|no|Buy/Sell direction. Enum: `BUY`, `SELL`. **Business required** - defaults to UNKNOWN if not provided|
|» size|body|string(decimal)|no|Order quantity. **Business required** - backend will validate|
|» price|body|string(decimal)|no|Order price (worst acceptable price). **Business required** for limit orders, enter 0 for market orders|
|» clientOrderId|body|string|no|Client-defined ID for idempotency checks. **Business required**|
|» type|body|string|no|Order type. Enum: `LIMIT`, `MARKET`, `STOP_LIMIT`, `STOP_MARKET`, `TAKE_PROFIT_LIMIT`, `TAKE_PROFIT_MARKET`. **Business required** - defaults to UNKNOWN if not provided|
|» timeInForce|body|string|no|Order execution policy. Enum: `GOOD_TIL_CANCEL`, `FILL_OR_KILL`, `IMMEDIATE_OR_CANCEL`, `POST_ONLY`. **Business required** - Market orders must use `IMMEDIATE_OR_CANCEL`|
|» reduceOnly|body|boolean|no|Whether this is a reduce-only order. Defaults to false if not provided|
|» triggerPrice|body|string(decimal)|no|Trigger price. Required for conditional orders. Enter 0 or empty string if not applicable|
|» triggerPriceType|body|string|no|Price type for trigger. Enum: `LAST_PRICE`, `INDEX_PRICE`, `ORACLE_PRICE`, `ASK1_PRICE`, `BID1_PRICE`. Required for conditional orders|
|» expireTime|body|string(int64)|no|Expiration time in milliseconds. HTTP layer accepts empty (converts to 0)|
|» sourceKey|body|string|no|Source key, UUID|
|» isPositionTpsl|body|boolean|no|Whether this is a position take-profit/stop-loss order. Defaults to false|
|» openTpslParentOrderId|body|string(int64)|no|Order ID of the opening order for TP/SL|
|» isSetOpenTp|body|boolean|no|Whether take-profit is set for opening order. Defaults to false|
|» openTp|body|object|no|Take-profit parameters for opening order. Required when `isSetOpenTp` is true|
|» isSetOpenSl|body|boolean|no|Whether stop-loss is set for opening order. Defaults to false|
|» openSl|body|object|no|Stop-loss parameters for opening order. Required when `isSetOpenSl` is true|
|» l2Nonce|body|string(int64)|no|L2 signature nonce. First 32 bits of sha256(`clientOrderId`). HTTP layer accepts empty|
|» l2Value|body|string(decimal)|no|L2 signature order value. HTTP layer accepts empty|
|» l2Size|body|string(decimal)|no|L2 signature order quantity. HTTP layer accepts empty|
|» l2LimitFee|body|string(decimal)|no|Maximum acceptable fee for L2 signature. HTTP layer accepts empty|
|» l2ExpireTime|body|string(int64)|no|L2 signature expiration time in milliseconds. Must be >= `expireTime` + 8 days. HTTP layer accepts empty|
|» l2Signature|body|string|no|L2 signature (128 characters). HTTP layer accepts empty but business logic requires valid signature|
|» extraType|body|string|no|Additional type for upper-layer business use|
|» extraDataJson|body|string|no|Additional data in JSON format|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» orderId|string(int64)|true|Created order ID|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|no|none|
|» accountId|body|string(int64)|yes|Account ID|
|» orderIdList|body|array[string]|yes|Array of order IDs to cancel|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» cancelResultMap|object|true|Map of order ID to cancellation result|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

<a id="opIdcancelOrderByClientOrderId"></a>

## POST Cancel Order by Client Order ID ⭐ [V2 NEW]

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|no|none|
|» accountId|body|string(int64)|yes|Account ID|
|» clientOrderIdList|body|array[string]|yes|Array of client order IDs to cancel|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» cancelResultMap|object|true|Map of client order ID to cancellation result|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|no|none|
|» accountId|body|string(int64)|yes|Account ID|
|» filterCoinIdList|body|array[string]|no|Filter by collateral coin IDs. If empty, cancels for all coins.|
|» filterContractIdList|body|array[string]|no|Filter by contract IDs. If empty, cancels for all contracts.|
|» filterOrderTypeList|body|array[string]|no|Filter by order types. If empty, cancels all types. Enum values: `LIMIT`, `MARKET`, `STOP_LIMIT`, `STOP_MARKET`, `TAKE_PROFIT_LIMIT`, `TAKE_PROFIT_MARKET`|
|» filterOrderStatusList|body|array[string]|no|Filter by order status. If empty, cancels all statuses. Enum values: `PENDING`, `OPEN`, `UNTRIGGERED`|
|» filterIsPositionTpsl|body|array[boolean]|no|Filter by position TP/SL orders. If empty, cancels all orders.|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» cancelResultMap|object|true|Map of order ID to cancellation result|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

<a id="opIdgetOrderById"></a>

## GET Get Orders by Order IDs

GET /api/v2/private/order/getOrderById

Retrieves orders by account ID and order IDs (batch operation).

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string(int64)|yes|Account ID|
|orderIdList|query|array[string]|yes|Array of order IDs|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|array[object]|true|Array of order objects|
|» id|string(int64)|true|Order ID|
|» userId|string(int64)|true|User ID|
|» accountId|string(int64)|true|Account ID|
|» coinId|string(int64)|true|Collateral coin ID|
|» contractId|string(int64)|true|Contract ID|
|» side|string|true|Buy/Sell direction. Enum: `BUY`, `SELL`|
|» price|string(decimal)|true|Order price|
|» size|string(decimal)|true|Order quantity|
|» clientOrderId|string|true|Client-defined order ID|
|» type|string|true|Order type|
|» timeInForce|string|true|Order execution policy|
|» reduceOnly|boolean|true|Whether this is a reduce-only order|
|» triggerPrice|string(decimal)|true|Trigger price|
|» triggerPriceType|string|true|Trigger price type|
|» expireTime|string(int64)|true|Expiration time|
|» status|string|true|Order status. Enum: `PENDING`, `OPEN`, `FILLED`, `CANCELING`, `CANCELED`, `UNTRIGGERED`|
|» cumFillSize|string(decimal)|true|Cumulative filled quantity|
|» cumFillValue|string(decimal)|true|Cumulative filled value|
|» cumFillFee|string(decimal)|true|Cumulative filled fee|
|» createdTime|string(int64)|true|Creation time|
|» updatedTime|string(int64)|true|Update time|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

<a id="opIdgetOrderByClientOrderId"></a>

## GET Get Orders by Client Order IDs

GET /api/v2/private/order/getOrderByClientOrderId

Retrieves orders by account ID and client order IDs (batch operation).

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string(int64)|yes|Account ID|
|clientOrderIdList|query|array[string]|yes|Array of client-defined order IDs|

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string(int64)|yes|Account ID|
|size|query|integer|no|Number of items to fetch. Must be > 0 and <= 200. Default: 100|
|offsetData|query|string|no|Pagination offset. If empty, retrieves the first page.|
|filterCoinIdList|query|array[string]|no|Filter by collateral coin IDs. If empty, fetches for all coins.|
|filterContractIdList|query|array[string]|no|Filter by contract IDs. If empty, fetches for all contracts.|
|filterTypeList|query|array[string]|no|Filter by order types. If empty, fetches all types.|
|filterStatusList|query|array[string]|no|Filter by order status. If empty, fetches all statuses.|
|filterIsLiquidateList|query|array[boolean]|no|Filter by liquidation orders. If empty, fetches all orders.|
|filterIsDeleverageList|query|array[boolean]|no|Filter by deleverage orders. If empty, fetches all orders.|
|filterIsPositionTpslList|query|array[boolean]|no|Filter by position TP/SL orders. If empty, fetches all orders.|
|filterStartCreatedTimeInclusive|query|string(int64)|no|Filter orders created after this time (inclusive). If 0 or empty, from earliest.|
|filterEndCreatedTimeExclusive|query|string(int64)|no|Filter orders created before this time (exclusive). If 0 or empty, to latest.|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» dataList|array[object]|true|Array of order objects|
|» nextPageOffsetData|string|true|Offset for next page. Empty string if no more data.|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

<a id="opIdgetHistoryOrderPage"></a>

## GET Get Historical Orders (Paginated)

GET /api/v2/private/order/getHistoryOrderPage

Retrieves historical orders with pagination and filtering.

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string(int64)|yes|Account ID|
|size|query|integer|no|Number of items to fetch. Must be > 0 and <= 100. Default: 100|
|offsetData|query|string|no|Pagination offset. If empty, retrieves the first page.|
|filterCoinIdList|query|array[string]|no|Filter by collateral coin IDs. If empty, fetches for all coins.|
|filterContractIdList|query|array[string]|no|Filter by contract IDs. If empty, fetches for all contracts.|
|filterTypeList|query|array[string]|no|Filter by order types. If empty, fetches all types.|
|filterStatusList|query|array[string]|no|Filter by order status. If empty, fetches all statuses.|
|filterIsLiquidateList|query|array[boolean]|no|Filter by liquidation orders. If empty, fetches all orders.|
|filterIsDeleverageList|query|array[boolean]|no|Filter by deleverage orders. If empty, fetches all orders.|
|filterIsPositionTpslList|query|array[boolean]|no|Filter by position TP/SL orders. If empty, fetches all orders.|
|filterStartCreatedTimeInclusive|query|string(int64)|no|Filter orders created after this time (inclusive). If 0 or empty, from earliest.|
|filterEndCreatedTimeExclusive|query|string(int64)|no|Filter orders created before this time (exclusive). If 0 or empty, to latest.|
|filterOrderIdList|query|array[string]|no|Filter by specific order IDs. If empty, fetches all orders.|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» dataList|array[object]|true|Array of order objects|
|» nextPageOffsetData|string|true|Offset for next page. Empty string if no more data.|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|

<a id="opIdgetHistoryOrderPagePost"></a>

## POST Get Historical Orders (Paginated) ⭐ [V2 NEW]

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|no|none|
|» accountId|body|string(int64)|yes|Account ID|
|» size|body|integer|no|Number of items to fetch. Must be > 0 and <= 100. Default: 100|
|» offsetData|body|string|no|Pagination offset. If empty, retrieves the first page.|
|» filterCoinIdList|body|array[string]|no|Filter by collateral coin IDs. If empty, fetches for all coins.|
|» filterContractIdList|body|array[string]|no|Filter by contract IDs. If empty, fetches for all contracts.|
|» filterTypeList|body|array[string]|no|Filter by order types. If empty, fetches all types.|
|» filterStatusList|body|array[string]|no|Filter by order status. If empty, fetches all statuses.|
|» filterIsLiquidateList|body|array[boolean]|no|Filter by liquidation orders. If empty, fetches all orders.|
|» filterIsDeleverageList|body|array[boolean]|no|Filter by deleverage orders. If empty, fetches all orders.|
|» filterIsPositionTpslList|body|array[boolean]|no|Filter by position TP/SL orders. If empty, fetches all orders.|
|» filterStartCreatedTimeInclusive|body|string(int64)|no|Filter orders created after this time (inclusive). If 0 or empty, from earliest.|
|» filterEndCreatedTimeExclusive|body|string(int64)|no|Filter orders created before this time (exclusive). If 0 or empty, to latest.|
|» filterOrderIdList|body|array[string]|no|Filter by specific order IDs. If empty, fetches all orders.|

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

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string(int64)|yes|Account ID|
|size|query|integer|no|Number of items to fetch. Must be > 0 and <= 100. Default: 100|
|offsetData|query|string|no|Pagination offset. If empty, retrieves the first page.|
|filterCoinIdList|query|array[string]|no|Filter by collateral coin IDs. If empty, fetches for all coins.|
|filterContractIdList|query|array[string]|no|Filter by contract IDs. If empty, fetches for all contracts.|
|filterOrderIdList|query|array[string]|no|Filter by order IDs. If empty, fetches for all orders.|
|filterIsLiquidateList|query|array[boolean]|no|Filter by liquidation orders. If empty, fetches all orders.|
|filterIsDeleverageList|query|array[boolean]|no|Filter by deleverage orders. If empty, fetches all orders.|
|filterIsPositionTpslList|query|array[boolean]|no|Filter by position TP/SL orders. If empty, fetches all orders.|
|filterStartCreatedTimeInclusive|query|string(int64)|no|Filter transactions created after this time (inclusive). If 0 or empty, from earliest.|
|filterEndCreatedTimeExclusive|query|string(int64)|no|Filter transactions created before this time (exclusive). If 0 or empty, to latest.|

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

|Name|Type|Required|Description|
|---|---|---|---|
|code|string|true|Status code. "SUCCESS" for success, otherwise failure.|
|data|object|true|Response data|
|» dataList|array[object]|true|Array of order fill transaction objects|
|» nextPageOffsetData|string|true|Offset for next page. Empty string if no more data.|
|msg|string|false|Message|
|errorParam|object|false|Error parameters|
|requestTime|string(timestamp)|true|Server request receive time|
|responseTime|string(timestamp)|true|Server response return time|
|traceId|string|true|Call trace ID|
