# OrderPrivateApi

<a id="opIdgetMaxCreateOrderSize"></a>

## POST Get Maximum Order Creation Size

POST /api/v1/private/order/getMaxCreateOrderSize

> Body Request Parameters

```json
{
  "accountId": "551109015904453258",
  "contractId": "10000001",
  "price": "97,463.4"
}
```

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|[GetMaxCreateOrderSizeParam](#schemagetmaxcreateordersizeparam)| No |none|

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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultgetmaxcreateordersize)|

<a id="opIdcreateOrder"></a>

## POST Create Order

POST /api/v1/private/order/createOrder

> Body Request Parameters

```json
{
    "price": "97393.8",
    "size": "0.001",
    "type": "LIMIT",
    "timeInForce": "GOOD_TIL_CANCEL",
    "reduceOnly": false,
    "isPositionTpsl": false,
    "isSetOpenTp": false,
    "isSetOpenSl": false,
    "accountId": "543429922991899150",
    "contractId": "10000001",
    "side": "BUY",
    "triggerPrice": "",
    "triggerPriceType": "LAST_PRICE",
    "clientOrderId": "21163368294502694",
    "expireTime": "1736476772359",
    "l2Nonce": "808219",
    "l2Value": "97.3938",
    "l2Size": "0.001",
    "l2LimitFee": "0.048697",
    "l2ExpireTime": "1737254372359",
    "l2Signature": "0537b3051bb9ffb98bb842ff6cadf69807a8dbf74f94d8c6106cf4f59b1fd2dc01aa2d93420408d60298831ddb82f7b482574ecb0fc285294dfeec732379ec40",
    "extraType": "",
    "extraDataJson": "",
    "symbol": "BTCUSDT",
    "showEqualValInput": true,
    "maxSellQTY": 0.009,
    "maxBuyQTY": 0.007
}
```
**Market Order Parameter Guidelines**

To prevent validation failures when placing market orders, please set the following parameters appropriately:

**For Buy Orders:**
- `l2Value = oracle_price × 10 × size` (set a higher value to ensure sufficient coverage)

**For Sell Orders:**
- `l2Value = tickSize` (use the minimum tick size value)

**For Fee Calculation:**
- `l2LimitFee = Ceil(value × fee_rate) × 10^6`

> **Important Notes:**
> - `l2Value` is NOT the actual order execution value - it's only used for validation purposes
> - `l2LimitFee` is NOT the actual transaction fee - it's only used for validation purposes
> - Setting these values too low will cause signature validation to fail
> - It's better to set slightly higher values to ensure successful order placement

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|[CreateOrderParam](#schemacreateorderparam)| No |none|

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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultcreateorder)|

### Response Data Structure

<a id="opIdcancelOrderById"></a>

## POST Cancel Order by Order ID

POST /api/v1/private/order/cancelOrderById

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
|body|body|[CancelOrderByIdParam](#schemacancelorderbyidparam)| No |none|

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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultcancelorder)|


## POST Cancel All Orders under Account

POST /api/v1/private/order/cancelAllOrder

> Body Request Parameters

```json
{
    "accountId": "551109015904453258"
}
```

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|[CancelAllOrderParam](#schemacancelallorderparam)| No |none|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "cancelResultMap": {
            "564828209955209354": "SUCCESS"
        }
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734665771719",
    "responseTime": "1734665771743",
    "traceId": "c0a1da9b75ad55d64ce0e98f86279c34"
}
```

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|Inline|

### Response Data Structure

<a id="opIdgetOrderById"></a>

## GET Get Orders by Account ID and Order IDs (Batch)

GET /api/v1/private/order/getOrderById

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|orderIdList|query|string| No |Order IDs|

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
            "sourceKey": "",
            "isPositionTpsl": false,
            "isLiquidate": false,
            "isDeleverage": false,
            "openTpslParentOrderId": "0",
            "isSetOpenTp": false,
            "openTp": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isSetOpenSl": false,
            "openSl": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isWithoutMatch": false,
            "withoutMatchFillSize": "0",
            "withoutMatchFillValue": "0",
            "withoutMatchPeerAccountId": "0",
            "withoutMatchPeerOrderId": "0",
            "maxLeverage": "50",
            "takerFeeRate": "0.000500",
            "makerFeeRate": "0.000180",
            "liquidateFeeRate": "0.01",
            "marketLimitPrice": "0",
            "marketLimitValue": "0",
            "l2Nonce": "3353661024",
            "l2Value": "96.260700",
            "l2Size": "0.001",
            "l2LimitFee": "0.048131",
            "l2ExpireTime": "1737257867612",
            "l2Signature": {
                "r": "0x072f299b86c199e161508ed554889d50476bc96d5a0c320bcd5b7e579b692c06",
                "s": "0x05b24b988f84bca951b3bb4e69b11e0aa1ce08cca38fea0f3afd94b5e7cd09f8",
                "v": ""
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "OPEN",
            "matchSequenceId": "35229771",
            "triggerTime": "0",
            "triggerPriceTime": "0",
            "triggerPriceValue": "0",
            "cancelReason": "UNKNOWN_ORDER_CANCEL_REASON",
            "cumFillSize": "0",
            "cumFillValue": "0",
            "cumFillFee": "0",
            "maxFillPrice": "0",
            "minFillPrice": "0",
            "cumLiquidateFee": "0",
            "cumRealizePnl": "0",
            "cumMatchSize": "0",
            "cumMatchValue": "0",
            "cumMatchFee": "0",
            "cumFailSize": "0",
            "cumFailValue": "0",
            "cumFailFee": "0",
            "cumApprovedSize": "0",
            "cumApprovedValue": "0",
            "cumApprovedFee": "0",
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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|Inline|

### Response Data Structure

<a id="opIdgetOrderByClientOrderId"></a>

## GET Get Orders by Client Order IDs (Batch)

GET /api/v1/private/order/getOrderByClientOrderId

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|clientOrderIdList|query|string| No |Client-defined order IDs|

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
            "sourceKey": "",
            "isPositionTpsl": false,
            "isLiquidate": false,
            "isDeleverage": false,
            "openTpslParentOrderId": "0",
            "isSetOpenTp": false,
            "openTp": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isSetOpenSl": false,
            "openSl": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isWithoutMatch": false,
            "withoutMatchFillSize": "0",
            "withoutMatchFillValue": "0",
            "withoutMatchPeerAccountId": "0",
            "withoutMatchPeerOrderId": "0",
            "maxLeverage": "50",
            "takerFeeRate": "0.000500",
            "makerFeeRate": "0.000180",
            "liquidateFeeRate": "0.01",
            "marketLimitPrice": "0",
            "marketLimitValue": "0",
            "l2Nonce": "3353661024",
            "l2Value": "96.260700",
            "l2Size": "0.001",
            "l2LimitFee": "0.048131",
            "l2ExpireTime": "1737257867612",
            "l2Signature": {
                "r": "0x072f299b86c199e161508ed554889d50476bc96d5a0c320bcd5b7e579b692c06",
                "s": "0x05b24b988f84bca951b3bb4e69b11e0aa1ce08cca38fea0f3afd94b5e7cd09f8",
                "v": ""
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "OPEN",
            "matchSequenceId": "35229771",
            "triggerTime": "0",
            "triggerPriceTime": "0",
            "triggerPriceValue": "0",
            "cancelReason": "UNKNOWN_ORDER_CANCEL_REASON",
            "cumFillSize": "0",
            "cumFillValue": "0",
            "cumFillFee": "0",
            "maxFillPrice": "0",
            "minFillPrice": "0",
            "cumLiquidateFee": "0",
            "cumRealizePnl": "0",
            "cumMatchSize": "0",
            "cumMatchValue": "0",
            "cumMatchFee": "0",
            "cumFailSize": "0",
            "cumFailValue": "0",
            "cumFailFee": "0",
            "cumApprovedSize": "0",
            "cumApprovedValue": "0",
            "cumApprovedFee": "0",
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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|Inline|


<a id="opIdgetHistoryOrderPage"></a>

## GET Get Historical Orders (Paginated)

GET /api/v1/private/order/getHistoryOrderPage

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|size|query|string| No |Number of items to fetch. Must be greater than 0 and less than or equal to 100.|
|offsetData|query|string| No |Pagination offset. If not provided or empty, retrieves the first page.|
|filterCoinIdList|query|string| No |Filters by collateral coin IDs. If empty, fetches orders for all collateral coin IDs.|
|filterContractIdList|query|string| No |Filters by contract IDs. If empty, fetches orders for all contracts.|
|filterTypeList|query|string| No |Filters by order types. If empty, fetches orders of all types.|
|filterStatusList|query|string| No |Filters by order status. If empty, fetches orders of all statuses.|
|filterIsLiquidateList|query|string| No |Filters by liquidate orders. If empty, fetches all orders|
|filterIsDeleverageList|query|string| No |Filters by deleverage orders. If empty, fetches all orders|
|filterIsPositionTpslList|query|string| No |Filters by position TP/SL orders. If empty, fetches all orders|
|filterStartCreatedTimeInclusive|query|string| No |Filters orders created after or on this time (inclusive). If empty or 0, retrieves from the earliest.|
|filterEndCreatedTimeExclusive|query|string| No |Filters orders created before this time (exclusive). If empty or 0, retrieves to the latest.|

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
                "timeInForce": "GOOD_TIL_CANCEL",
                "reduceOnly": false,
                "triggerPrice": "0",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "1736476955478",
                "sourceKey": "",
                "isPositionTpsl": false,
                "isLiquidate": false,
                "isDeleverage": false,
                "openTpslParentOrderId": "0",
                "isSetOpenTp": false,
                "openTp": {
                    "side": "UNKNOWN_ORDER_SIDE",
                    "price": "",
                    "size": "",
                    "clientOrderId": "",
                    "triggerPrice": "",
                    "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                    "expireTime": "0",
                    "l2Nonce": "0",
                    "l2Value": "",
                    "l2Size": "",
                    "l2LimitFee": "",
                    "l2ExpireTime": "0",
                    "l2Signature": {
                        "r": "",
                        "s": "",
                        "v": ""
                    }
                },
                "isSetOpenSl": false,
                "openSl": {
                    "side": "UNKNOWN_ORDER_SIDE",
                    "price": "",
                    "size": "",
                    "clientOrderId": "",
                    "triggerPrice": "",
                    "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                    "expireTime": "0",
                    "l2Nonce": "0",
                    "l2Value": "",
                    "l2Size": "",
                    "l2LimitFee": "",
                    "l2ExpireTime": "0",
                    "l2Signature": {
                        "r": "",
                        "s": "",
                        "v": ""
                    }
                },
                "isWithoutMatch": false,
                "withoutMatchFillSize": "0",
                "withoutMatchFillValue": "0",
                "withoutMatchPeerAccountId": "0",
                "withoutMatchPeerOrderId": "0",
                "maxLeverage": "50",
                "takerFeeRate": "0.000500",
                "makerFeeRate": "0.000180",
                "liquidateFeeRate": "0.01",
                "marketLimitPrice": "0",
                "marketLimitValue": "0",
                "l2Nonce": "2054491946",
                "l2Value": "97.444500",
                "l2Size": "0.001",
                "l2LimitFee": "0.048723",
                "l2ExpireTime": "1737254555478",
                "l2Signature": {
                    "r": "0x009af59c2963f1650449904fde059a83fed5beb4acdd67ffa22c551c4be977de",
                    "s": "0x04940a395f2d1b39c0f2b969e47fec478fbfea9ce227bfedd07d407175dcac3e",
                    "v": ""
                },
                "extraType": "",
                "extraDataJson": "",
                "status": "FILLED",
                "matchSequenceId": "35196430",
                "triggerTime": "0",
                "triggerPriceTime": "0",
                "triggerPriceValue": "0",
                "cancelReason": "UNKNOWN_ORDER_CANCEL_REASON",
                "cumFillSize": "0.001",
                "cumFillValue": "97.4445",
                "cumFillFee": "0.017540",
                "maxFillPrice": "97444.5",
                "minFillPrice": "97444.5",
                "cumLiquidateFee": "0",
                "cumRealizePnl": "-0.017540",
                "cumMatchSize": "0.001",
                "cumMatchValue": "97.4445",
                "cumMatchFee": "0.017540",
                "cumFailSize": "0",
                "cumFailValue": "0",
                "cumFailFee": "0",
                "cumApprovedSize": "0",
                "cumApprovedValue": "0",
                "cumApprovedFee": "0",
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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresult)|

<a id="opIdgetHistoryOrderFillTransactionPage"></a>

## GET Get Historical Order Fill Transactions (Paginated)

GET /api/v1/private/order/getHistoryOrderFillTransactionPage

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|size|query|string| No |Number of items to fetch. Must be greater than 0 and less than or equal to 100.|
|offsetData|query|string| No |Pagination offset. If not provided or empty, retrieves the first page.|
|filterCoinIdList|query|string| No |Filters by collateral coin IDs. If empty, fetches order fill transactions for all collateral coin IDs.|
|filterContractIdList|query|string| No |Filters by contract IDs. If empty, fetches order fill transactions for all contracts.|
|filterOrderIdList|query|string| No |Filters by order IDs. If empty, fetches order fill transactions for all orders.|
|filterIsLiquidateList|query|string| No |Filters by liquidate orders. If empty, fetches all orders|
|filterIsDeleverageList|query|string| No |Filters by deleverage orders. If empty, fetches all orders|
|filterIsPositionTpslList|query|string| No |Filters by position TP/SL orders. If empty, fetches all orders|
|filterStartCreatedTimeInclusive|query|string| No |Filters order fill transactions created after or on this time (inclusive). If empty or 0, retrieves from the earliest.|
|filterEndCreatedTimeExclusive|query|string| No |Filters order fill transactions created before this time (exclusive). If empty or 0, retrieves to the latest.|

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
                "isPositionTpsl": false,
                "isLiquidate": false,
                "isDeleverage": false,
                "isWithoutMatch": false,
                "matchSequenceId": "35196430",
                "matchIndex": 0,
                "matchTime": "1734662617982",
                "matchAccountId": "555790606509539863",
                "matchOrderId": "564815957235597591",
                "matchFillId": "05d14491-db7d-478a-9d9f-2dc55c3ff3ca",
                "positionTransactionId": "564815957294318862",
                "collateralTransactionId": "564815957294317838",
                "extraType": "",
                "extraDataJson": "",
                "censorStatus": "CENSOR_SUCCESS",
                "censorTxId": "893031",
                "censorTime": "1734662617988",
                "censorFailCode": "",
                "censorFailReason": "",
                "l2TxId": "1084582",
                "l2RejectTime": "0",
                "l2RejectCode": "",
                "l2RejectReason": "",
                "l2ApprovedTime": "0",
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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultpagedataorderfilltransaction)|

<a id="opIdgetHistoryOrderFillTransactionById"></a>

## GET Get Historical Order Fill Transactions by ID (Batch)

GET /api/v1/private/order/getHistoryOrderFillTransactionById

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|orderFillTransactionIdList|query|string| No |Order fill transaction IDs|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
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
            "isPositionTpsl": false,
            "isLiquidate": false,
            "isDeleverage": false,
            "isWithoutMatch": false,
            "matchSequenceId": "35196430",
            "matchIndex": 0,
            "matchTime": "1734662617982",
            "matchAccountId": "555790606509539863",
            "matchOrderId": "564815957235597591",
            "matchFillId": "05d14491-db7d-478a-9d9f-2dc55c3ff3ca",
            "positionTransactionId": "564815957294318862",
            "collateralTransactionId": "564815957294317838",
            "extraType": "",
            "extraDataJson": "",
            "censorStatus": "CENSOR_SUCCESS",
            "censorTxId": "893031",
            "censorTime": "1734662617988",
            "censorFailCode": "",
            "censorFailReason": "",
            "l2TxId": "1084582",
            "l2RejectTime": "0",
            "l2RejectCode": "",
            "l2RejectReason": "",
            "l2ApprovedTime": "0",
            "createdTime": "1734662617984",
            "updatedTime": "1734662617992"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734666041607",
    "responseTime": "1734666041619",
    "traceId": "629acb9f32074715a1ea9befd171c452"
}
```

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultpagedataorderfilltransaction)|

<a id="opIdgetHistoryOrderById"></a>

## GET Get Historical Orders by ID (Batch)

GET /api/v1/private/order/getHistoryOrderById

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|orderIdList|query|string| No |Order IDs|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
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
            "timeInForce": "GOOD_TIL_CANCEL",
            "reduceOnly": false,
            "triggerPrice": "0",
            "triggerPriceType": "UNKNOWN_PRICE_TYPE",
            "expireTime": "1736476955478",
            "sourceKey": "",
            "isPositionTpsl": false,
            "isLiquidate": false,
            "isDeleverage": false,
            "openTpslParentOrderId": "0",
            "isSetOpenTp": false,
            "openTp": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isSetOpenSl": false,
            "openSl": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isWithoutMatch": false,
            "withoutMatchFillSize": "0",
            "withoutMatchFillValue": "0",
            "withoutMatchPeerAccountId": "0",
            "withoutMatchPeerOrderId": "0",
            "maxLeverage": "50",
            "takerFeeRate": "0.000500",
            "makerFeeRate": "0.000180",
            "liquidateFeeRate": "0.01",
            "marketLimitPrice": "0",
            "marketLimitValue": "0",
            "l2Nonce": "2054491946",
            "l2Value": "97.444500",
            "l2Size": "0.001",
            "l2LimitFee": "0.048723",
            "l2ExpireTime": "1737254555478",
            "l2Signature": {
                "r": "0x009af59c2963f1650449904fde059a83fed5beb4acdd67ffa22c551c4be977de",
                "s": "0x04940a395f2d1b39c0f2b969e47fec478fbfea9ce227bfedd07d407175dcac3e",
                "v": ""
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "FILLED",
            "matchSequenceId": "35196430",
            "triggerTime": "0",
            "triggerPriceTime": "0",
            "triggerPriceValue": "0",
            "cancelReason": "UNKNOWN_ORDER_CANCEL_REASON",
            "cumFillSize": "0.001",
            "cumFillValue": "97.4445",
            "cumFillFee": "0.017540",
            "maxFillPrice": "97444.5",
            "minFillPrice": "97444.5",
            "cumLiquidateFee": "0",
            "cumRealizePnl": "-0.017540",
            "cumMatchSize": "0.001",
            "cumMatchValue": "97.4445",
            "cumMatchFee": "0.017540",
            "cumFailSize": "0",
            "cumFailValue": "0",
            "cumFailFee": "0",
            "cumApprovedSize": "0",
            "cumApprovedValue": "0",
            "cumApprovedFee": "0",
            "createdTime": "1734662555665",
            "updatedTime": "1734662617992"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734666077826",
    "responseTime": "1734666077846",
    "traceId": "5773d3492c5913ba4f8c93071f3e426e"
}
```

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultpagedataorder)|

### Response Data Structure

<a id="opIdgetHistoryOrderByClientOrderId"></a>

## GET Get Historical Orders by Client Order IDs (Batch)

GET /api/v1/private/order/getHistoryOrderByClientOrderId

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|clientOrderIdList|query|string| No |Order client order id|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
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
            "timeInForce": "GOOD_TIL_CANCEL",
            "reduceOnly": false,
            "triggerPrice": "0",
            "triggerPriceType": "UNKNOWN_PRICE_TYPE",
            "expireTime": "1736476955478",
            "sourceKey": "",
            "isPositionTpsl": false,
            "isLiquidate": false,
            "isDeleverage": false,
            "openTpslParentOrderId": "0",
            "isSetOpenTp": false,
            "openTp": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isSetOpenSl": false,
            "openSl": {
                "side": "UNKNOWN_ORDER_SIDE",
                "price": "",
                "size": "",
                "clientOrderId": "",
                "triggerPrice": "",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "0",
                "l2Nonce": "0",
                "l2Value": "",
                "l2Size": "",
                "l2LimitFee": "",
                "l2ExpireTime": "0",
                "l2Signature": {
                    "r": "",
                    "s": "",
                    "v": ""
                }
            },
            "isWithoutMatch": false,
            "withoutMatchFillSize": "0",
            "withoutMatchFillValue": "0",
            "withoutMatchPeerAccountId": "0",
            "withoutMatchPeerOrderId": "0",
            "maxLeverage": "50",
            "takerFeeRate": "0.000500",
            "makerFeeRate": "0.000180",
            "liquidateFeeRate": "0.01",
            "marketLimitPrice": "0",
            "marketLimitValue": "0",
            "l2Nonce": "2054491946",
            "l2Value": "97.444500",
            "l2Size": "0.001",
            "l2LimitFee": "0.048723",
            "l2ExpireTime": "1737254555478",
            "l2Signature": {
                "r": "0x009af59c2963f1650449904fde059a83fed5beb4acdd67ffa22c551c4be977de",
                "s": "0x04940a395f2d1b39c0f2b969e47fec478fbfea9ce227bfedd07d407175dcac3e",
                "v": ""
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "FILLED",
            "matchSequenceId": "35196430",
            "triggerTime": "0",
            "triggerPriceTime": "0",
            "triggerPriceValue": "0",
            "cancelReason": "UNKNOWN_ORDER_CANCEL_REASON",
            "cumFillSize": "0.001",
            "cumFillValue": "97.4445",
            "cumFillFee": "0.017540",
            "maxFillPrice": "97444.5",
            "minFillPrice": "97444.5",
            "cumLiquidateFee": "0",
            "cumRealizePnl": "-0.017540",
            "cumMatchSize": "0.001",
            "cumMatchValue": "97.4445",
            "cumMatchFee": "0.017540",
            "cumFailSize": "0",
            "cumFailValue": "0",
            "cumFailFee": "0",
            "cumApprovedSize": "0",
            "cumApprovedValue": "0",
            "cumApprovedFee": "0",
            "createdTime": "1734662555665",
            "updatedTime": "1734662617992"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734666143318",
    "responseTime": "1734666143331",
    "traceId": "e19f71177e8cf0c7f34d45d85064c07c"
}
```

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultpagedataorder)|

### Response Data Structure

<a id="opIdgetActiveOrderPage"></a>

## GET Get Active Orders (Paginated)

GET /api/v1/private/order/getActiveOrderPage

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|accountId|query|string| No |Account ID|
|size|query|string| No |Number of items to fetch. Must be greater than 0 and less than or equal to 200.|
|offsetData|query|string| No |Pagination offset. If not provided or empty, retrieves the first page.|
|filterCoinIdList|query|string| No |Filters by collateral coin IDs. If empty, fetches active orders for all collateral coin IDs.|
|filterContractIdList|query|string| No |Filters by contract IDs. If empty, fetches active orders for all contracts.|
|filterTypeList|query|string| No |Filters by order types. If empty, fetches orders of all types.|
|filterStatusList|query|string| No |Filters by order status. If empty, fetches orders of all statuses.|
|filterIsLiquidateList|query|string| No |Filters by liquidate orders. If empty, fetches all orders|
|filterIsDeleverageList|query|string| No |Filters by deleverage orders. If empty, fetches all orders|
|filterIsPositionTpslList|query|string| No |Filters by position TP/SL orders. If empty, fetches all orders|
|filterStartCreatedTimeInclusive|query|string| No |Filters orders created after or on this time (inclusive). If empty or 0, retrieves from the earliest.|
|filterEndCreatedTimeExclusive|query|string| No |Filters orders created before this time (exclusive). If empty or 0, retrieves to the latest.|

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
                "timeInForce": "GOOD_TIL_CANCEL",
                "reduceOnly": false,
                "triggerPrice": "0",
                "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                "expireTime": "1736476955478",
                "sourceKey": "",
                "isPositionTpsl": false,
                "isLiquidate": false,
                "isDeleverage": false,
                "openTpslParentOrderId": "0",
                "isSetOpenTp": false,
                "openTp": {
                    "side": "UNKNOWN_ORDER_SIDE",
                    "price": "",
                    "size": "",
                    "clientOrderId": "",
                    "triggerPrice": "",
                    "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                    "expireTime": "0",
                    "l2Nonce": "0",
                    "l2Value": "",
                    "l2Size": "",
                    "l2LimitFee": "",
                    "l2ExpireTime": "0",
                    "l2Signature": {
                        "r": "",
                        "s": "",
                        "v": ""
                    }
                },
                "isSetOpenSl": false,
                "openSl": {
                    "side": "UNKNOWN_ORDER_SIDE",
                    "price": "",
                    "size": "",
                    "clientOrderId": "",
                    "triggerPrice": "",
                    "triggerPriceType": "UNKNOWN_PRICE_TYPE",
                    "expireTime": "0",
                    "l2Nonce": "0",
                    "l2Value": "",
                    "l2Size": "",
                    "l2LimitFee": "",
                    "l2ExpireTime": "0",
                    "l2Signature": {
                        "r": "",
                        "s": "",
                        "v": ""
                    }
                },
                "isWithoutMatch": false,
                "withoutMatchFillSize": "0",
                "withoutMatchFillValue": "0",
                "withoutMatchPeerAccountId": "0",
                "withoutMatchPeerOrderId": "0",
                "maxLeverage": "50",
                "takerFeeRate": "0.000500",
                "makerFeeRate": "0.000180",
                "liquidateFeeRate": "0.01",
                "marketLimitPrice": "0",
                "marketLimitValue": "0",
                "l2Nonce": "2054491946",
                "l2Value": "97.444500",
                "l2Size": "0.001",
                "l2LimitFee": "0.048723",
                "l2ExpireTime": "1737254555478",
                "l2Signature": {
                    "r": "0x009af59c2963f1650449904fde059a83fed5beb4acdd67ffa22c551c4be977de",
                    "s": "0x04940a395f2d1b39c0f2b969e47fec478fbfea9ce227bfedd07d407175dcac3e",
                    "v": ""
                },
                "extraType": "",
                "extraDataJson": "",
                "status": "OPEN",
                "matchSequenceId": "35195888",
                "triggerTime": "0",
                "triggerPriceTime": "0",
                "triggerPriceValue": "0",
                "cancelReason": "UNKNOWN_ORDER_CANCEL_REASON",
                "cumFillSize": "0",
                "cumFillValue": "0",
                "cumFillFee": "0",
                "maxFillPrice": "0",
                "minFillPrice": "0",
                "cumLiquidateFee": "0",
                "cumRealizePnl": "0",
                "cumMatchSize": "0",
                "cumMatchValue": "0",
                "cumMatchFee": "0",
                "cumFailSize": "0",
                "cumFailValue": "0",
                "cumFailFee": "0",
                "cumApprovedSize": "0",
                "cumApprovedValue": "0",
                "cumApprovedFee": "0",
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

### Response

|Status Code|Status Code Description|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemaresultpagedataorder)|

# Data Models


<a id="schemaresultlistorderfilltransaction"></a>
### schemaresultlistorderfilltransaction

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[[OrderFillTransaction](#schemaorderfilltransaction)]|false|none|Successful response data.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|


<a id="schemaorderfilltransaction"></a>
### schemaorderfilltransaction

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|id|string(int64)|false|none|Unique identifier.|
|userId|string(int64)|false|none|User ID.|
|accountId|string(int64)|false|none|Account ID.|
|coinId|string(int64)|false|none|Collateral coin ID.|
|contractId|string(int64)|false|none|Contract ID.|
|orderId|string(int64)|false|none|Order ID.|
|orderSide|string|false|none|Buy/Sell direction.|
|fillSize|string|false|none|Actual filled quantity.|
|fillValue|string|false|none|Actual filled value.|
|fillFee|string|false|none|Actual filled fee.|
|fillPrice|string|false|none|Fill price (not precise, for display purposes only).|
|liquidateFee|string|false|none|Liquidation fee if it's a liquidation (forced liquidation) transaction.|
|realizePnl|string|false|none|Realized profit and loss (only available if the fill includes closing a position).|
|direction|string|false|none|Actual fill direction.|
|isPositionTpsl|boolean|false|none|Whether this is a position take-profit/stop-loss order.|
|isLiquidate|boolean|false|none|Whether this is a liquidation (forced liquidation) fill.|
|isDeleverage|boolean|false|none|Whether this is an auto-deleverage fill.|
|isWithoutMatch|boolean|false|none|Whether this order was filled directly without matching.|
|matchSequenceId|string(int64)|false|none|Sequence ID after submitting to the matching engine.|
|matchIndex|integer(int32)|false|none|Index for multiple fills after submitting to the matching engine.|
|matchTime|string(int64)|false|none|Time after submitting to the matching engine.|
|matchAccountId|string(int64)|false|none|Counterparty account ID.|
|matchOrderId|string(int64)|false|none|Counterparty order ID.|
|matchFillId|string|false|none|Fill ID returned by the matching engine.|
|positionTransactionId|string(int64)|false|none|Associated position transaction ID.|
|collateralTransactionId|string(int64)|false|none|Associated collateral transaction ID.|
|extraType|string|false|none|Additional type for upper-layer business use.|
|extraDataJson|string|false|none|Additional data in JSON format. Defaults to an empty string.|
|censorStatus|string|false|none|Current censorship status.|
|censorTxId|string(int64)|false|none|Censorship processing sequence ID. Exists when `censor_status` is `CENSOR_SUCCESS`/`CENSOR_FAILURE`/`L2_APPROVED`/`L2_REJECT`/`L2_REJECT_APPROVED`.|
|censorTime|string(int64)|false|none|Censorship processing time. Exists when `censor_status` is `CENSOR_SUCCESS`/`CENSOR_FAILURE`/`L2_APPROVED`/`L2_REJECT`/`L2_REJECT_APPROVED`.|
|censorFailCode|string|false|none|Censorship failure error code. Exists when `censor_status` is `CENSOR_FAILURE`.|
|censorFailReason|string|false|none|Censorship failure reason. Exists when `censor_status` is `CENSOR_FAILURE`.|
|l2TxId|string(int64)|false|none|L2 push transaction ID. Exists when `censor_status` is `CENSOR_SUCCESS`/`L2_APPROVED`/`L2_REJECT`/`L2_REJECT_APPROVED`.|
|l2RejectTime|string(int64)|false|none|L2 rejection time. Exists when `censor_status` is `L2_REJECT`/`L2_REJECT_APPROVED`.|
|l2RejectCode|string|false|none|L2 rejection error code. Exists when `censor_status` is `L2_REJECT`/`L2_REJECT_APPROVED`.|
|l2RejectReason|string|false|none|L2 rejection reason. Exists when `censor_status` is `L2_REJECT`/`L2_REJECT_APPROVED`.|
|l2ApprovedTime|string(int64)|false|none|L2 batch verification time. Exists when `status` is `L2_APPROVED`/`L2_REJECT_APPROVED`.|
|createdTime|string(int64)|false|none|Creation time.|
|updatedTime|string(int64)|false|none|Update time.|

#### Enum Values

|Property|Value|
|---|---|
|orderSide|UNKNOWN_ORDER_SIDE|
|orderSide|BUY|
|orderSide|SELL|
|orderSide|UNRECOGNIZED|
|direction|UNKNOWN_LIQUIDITY_DIRECTION|
|direction|MAKER|
|direction|TAKER|
|direction|UNRECOGNIZED|
|censorStatus|UNKNOWN_TRANSACTION_STATUS|
|censorStatus|INIT|
|censorStatus|CENSOR_SUCCESS|
|censorStatus|CENSOR_FAILURE|
|censorStatus|L2_APPROVED|
|censorStatus|L2_REJECT|
|censorStatus|L2_REJECT_APPROVED|
|censorStatus|UNRECOGNIZED|

<a id="schemaresultpagedataorderfilltransaction"></a>
### schemaresultpagedataorderfilltransaction

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[PageDataOrderFillTransaction](#schemapagedataorderfilltransaction)|false|none|Generic paginated response.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|


<a id="schemapagedataorderfilltransaction"></a>
### schemapagedataorderfilltransaction

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|dataList|[[OrderFillTransaction](#schemaorderfilltransaction)]|false|none|Data list.|
|nextPageOffsetData|string|false|none|Offset for retrieving the next page. Empty string if no more data available.|


<a id="schemaresultpagedataorder"></a>
### schemaresultpagedataorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[PageDataOrder](#schemapagedataorder)|false|none|Generic paginated response.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|


<a id="schemapagedataorder"></a>
### schemapagedataorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|dataList|[[Order](#schemaorder)]|false|none|Data list.|
|nextPageOffsetData|string|false|none|Offset for retrieving the next page. Empty string if no more data available.|


<a id="schemaorder"></a>
### schemaorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|id|string(int64)|false|none|Order ID. Value greater than 0.|
|userId|string(int64)|false|none|User ID.|
|accountId|string(int64)|false|none|Account ID.|
|coinId|string(int64)|false|none|Collateral coin ID.|
|contractId|string(int64)|false|none|Contract ID.|
|side|string|false|none|Buy/Sell direction.|
|price|string|false|none|Order price (worst acceptable price), actual type is decimal.|
|size|string|false|none|Order quantity, actual type is decimal.|
|clientOrderId|string|false|none|Client-defined ID for idempotency checks.|
|type|string|false|none|Order type.|
|timeInForce|string|false|none|Order execution policy. Relevant when `type` is `LIMIT`/`STOP_LIMIT`/`TAKE_PROFIT_LIMIT`.|
|reduceOnly|boolean|false|none|Whether this is a reduce-only order.|
|triggerPrice|string|false|none|Trigger price. Relevant when `type` is `STOP_LIMIT`/`STOP_MARKET`/`TAKE_PROFIT_LIMIT`/`TAKE_PROFIT_MARKET`. If 0, the field is empty. Actual type is decimal.|
|triggerPriceType|string|false|none|Price type: Last price, Mark price, etc. Relevant when `type` is `STOP_LIMIT`/`STOP_MARKET`/`TAKE_PROFIT_LIMIT`/`TAKE_PROFIT_MARKET`.|
|expireTime|string(int64)|false|none|Expiration time.|
|sourceKey|string|false|none|Source key, UUID.|
|isPositionTpsl|boolean|false|none|Whether this is a position take-profit/stop-loss order.|
|isLiquidate|boolean|false|none|Whether this is a liquidation (forced liquidation) order.|
|isDeleverage|boolean|false|none|Whether this is an auto-deleverage order.|
|openTpslParentOrderId|string(int64)|false|none|Order ID of the opening order for a take-profit or stop-loss order.|
|isSetOpenTp|boolean|false|none|Whether take-profit is set for opening order.|
|openTp|[OpenTpSl](#schemaopentpsl)|false|none|Take-profit/stop-loss parameters for opening order.|
|isSetOpenSl|boolean|false|none|Whether stop-loss is set for opening order.|
|openSl|[OpenTpSl](#schemaopentpsl)|false|none|Take-profit/stop-loss parameters for opening order.|
|isWithoutMatch|boolean|false|none|Whether this order is directly filled without matching.|
|withoutMatchFillSize|string|false|none|Off-exchange fill quantity (valid only when `is_without_match` is true).|
|withoutMatchFillValue|string|false|none|Off-exchange fill value (valid only when `is_without_match` is true).|
|withoutMatchPeerAccountId|string(int64)|false|none|Off-exchange counterparty account ID (valid only when `is_without_match` is true).|
|withoutMatchPeerOrderId|string(int64)|false|none|Off-exchange counterparty order ID (valid only when `is_without_match` is true).|
|maxLeverage|string|false|none|Leverage used when placing the order. Actual type is decimal.|
|takerFeeRate|string|false|none|Taker fee rate when placing the order. Actual type is decimal.|
|makerFeeRate|string|false|none|Maker fee rate when placing the order. Actual type is decimal.|
|liquidateFeeRate|string|false|none|Liquidation fee rate when placing the order. Actual type is decimal.|
|marketLimitPrice|string|false|none|Limit price for submitting market orders to the matching engine (only exists for market orders, 0 otherwise). Actual type is decimal.|
|marketLimitValue|string|false|none|Limit value for submitting market orders to the matching engine (only exists for market orders, 0 otherwise). Actual type is decimal.|
|l2Nonce|string(int64)|false|none|L2 signature nonce. Takes the first 32 bits of sha256(`client_order_id`).|
|l2Value|string|false|none|L2 signature order value (the actual filled price must be equal to or better than `l2_value` / `l2_size`). May differ from `price` x `size`. Actual type is decimal.|
|l2Size|string|false|none|L2 signature order quantity. May differ from the `size` field. Actual type is decimal.|
|l2LimitFee|string|false|none|Maximum acceptable fee for L2 signature.|
|l2ExpireTime|string(int64)|false|none|L2 signature expiration time in milliseconds. The hour value should be used when generating/verifying the signature, i.e. `l2_expire_time` / 3600000. Note that this value must be greater or equal to `expire_time` + 8 * 24 * 60 * 60 * 1000 (8 days).|
|l2Signature|[L2Signature](#schemal2signature)|false|none|L2 signature information.|
|extraType|string|false|none|Additional type for upper-layer business use.|
|extraDataJson|string|false|none|Additional data in JSON format. Defaults to an empty string.|
|status|string|false|none|Order status.|
|matchSequenceId|string(int64)|false|none|Sequence ID handled by the matching engine.|
|triggerTime|string(int64)|false|none|Conditional order trigger time.|
|triggerPriceTime|string(int64)|false|none|Conditional order trigger price time.|
|triggerPriceValue|string|false|none|Conditional order trigger price value.|
|cancelReason|string|false|none|Order cancellation reason.|
|cumFillSize|string(decimal)|false|none|Cumulative filled quantity after censorship. Actual type is decimal.|
|cumFillValue|string(decimal)|false|none|Cumulative filled value after censorship. Actual type is decimal.|
|cumFillFee|string(decimal)|false|none|Cumulative filled fee after censorship. Actual type is decimal.|
|maxFillPrice|string(decimal)|false|none|Maximum filled price for the current order after censorship. Actual type is decimal.|
|minFillPrice|string(decimal)|false|none|Minimum filled price for the current order after censorship. Actual type is decimal.|
|cumLiquidateFee|string(decimal)|false|none|Cumulative liquidation fee after censorship. Actual type is decimal.|
|cumRealizePnl|string(decimal)|false|none|Cumulative realized PnL after censorship. Actual type is decimal.|
|cumMatchSize|string(decimal)|false|none|Cumulative matched quantity. Actual type is decimal.|
|cumMatchValue|string(decimal)|false|none|Cumulative matched value. Actual type is decimal.|
|cumMatchFee|string(decimal)|false|none|Cumulative matched fee. Actual type is decimal.|
|cumFailSize|string|false|none|Cumulative failed/L2 rejected quantity. Actual type is decimal.|
|cumFailValue|string|false|none|Cumulative failed/L2 rejected value. Actual type is decimal.|
|cumFailFee|string|false|none|Cumulative failed/L2 rejected fee. Actual type is decimal.|
|cumApprovedSize|string|false|none|Cumulative quantity approved by L2.|
|cumApprovedValue|string|false|none|Cumulative value approved by L2.|
|cumApprovedFee|string|false|none|Cumulative fee approved by L2.|
|createdTime|string(int64)|false|none|Creation time.|
|updatedTime|string(int64)|false|none|Update time.|

#### Enum Values

|Property|Value|
|---|---|
|side|UNKNOWN_ORDER_SIDE|
|side|BUY|
|side|SELL|
|side|UNRECOGNIZED|
|type|UNKNOWN_ORDER_TYPE|
|type|LIMIT|
|type|MARKET|
|type|STOP_LIMIT|
|type|STOP_MARKET|
|type|TAKE_PROFIT_LIMIT|
|type|TAKE_PROFIT_MARKET|
|type|UNRECOGNIZED|
|timeInForce|UNKNOWN_TIME_IN_FORCE|
|timeInForce|GOOD_TIL_CANCEL|
|timeInForce|FILL_OR_KILL|
|timeInForce|IMMEDIATE_OR_CANCEL|
|timeInForce|POST_ONLY|
|timeInForce|UNRECOGNIZED|
|triggerPriceType|UNKNOWN_PRICE_TYPE|
|triggerPriceType|ORACLE_PRICE|
|triggerPriceType|INDEX_PRICE|
|triggerPriceType|LAST_PRICE|
|triggerPriceType|ASK1_PRICE|
|triggerPriceType|BID1_PRICE|
|triggerPriceType|OPEN_INTEREST|
|triggerPriceType|UNRECOGNIZED|
|status|UNKNOWN_ORDER_STATUS|
|status|PENDING|
|status|OPEN|
|status|FILLED|
|status|CANCELING|
|status|CANCELED|
|status|UNTRIGGERED|
|status|UNRECOGNIZED|
|cancelReason|UNKNOWN_ORDER_CANCEL_REASON|
|cancelReason|USER_CANCELED|
|cancelReason|EXPIRE_CANCELED|
|cancelReason|COULD_NOT_FILL|
|cancelReason|REDUCE_ONLY_CANCELED|
|cancelReason|LIQUIDATE_CANCELED|
|cancelReason|MARGIN_NOT_ENOUGH|
|cancelReason|SYSTEM_LIMIT_EVICTED|
|cancelReason|ADMIN_CANCELED|
|cancelReason|INTERNAL_FAILED|
|cancelReason|UNRECOGNIZED|

<a id="schemal2signature"></a>
### schemal2signature

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|r|string|false|none|Big integer as a hex string.|
|s|string|false|none|Big integer as a hex string.|
|v|string|false|none|Big integer as a hex string.|


<a id="schemaopentpsl"></a>
### schemaopentpsl

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|side|string|false|none|Buy/sell direction. This field is required.|
|price|string|false|none|Order price (worst acceptable price), actual type is decimal. Required, enter 0 for market orders.|
|size|string|false|none|Order quantity, actual type is decimal. Required.|
|clientOrderId|string|false|none|Client-defined ID for signature and idempotency checks. This field is required.|
|triggerPrice|string|false|none|Trigger price. This field is required.|
|triggerPriceType|string|false|none|Price type: Last price, Mark price, etc. This field is required.|
|expireTime|string(int64)|false|none|Expiration time.|
|l2Nonce|string(int64)|false|none|L2 signature nonce. Takes the first 32 bits of sha256(`client_order_id`).|
|l2Value|string|false|none|L2 signature order value (the actual filled price must be equal to or better than `l2_value` / `l2_price`). May differ from `price` x `size`. Actual type is decimal.|
|l2Size|string|false|none|L2 signature order quantity. May differ from the `size` field. Actual type is decimal.|
|l2LimitFee|string|false|none|Maximum acceptable fee for L2 signature.|
|l2ExpireTime|string(int64)|false|none|L2 signature expiration time in unix hour. Must be at least 10 hours after `expire_time`.|
|l2Signature|[L2Signature](#schemal2signature)|false|none|L2 signature information.|

#### Enum Values

|Property|Value|
|---|---|
|side|UNKNOWN_ORDER_SIDE|
|side|BUY|
|side|SELL|
|side|UNRECOGNIZED|
|triggerPriceType|UNKNOWN_PRICE_TYPE|
|triggerPriceType|ORACLE_PRICE|
|triggerPriceType|INDEX_PRICE|
|triggerPriceType|LAST_PRICE|
|triggerPriceType|ASK1_PRICE|
|triggerPriceType|BID1_PRICE|
|triggerPriceType|OPEN_INTEREST|
|triggerPriceType|UNRECOGNIZED|


<a id="schemaresultlistorder"></a>
### schemaresultlistorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[[Order](#schemaorder)]|false|none|Successful response data.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|


<a id="schemacancelallorderparam"></a>
### schemacancelallorderparam

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|accountId|string(int64)|false|none|Account ID.|
|filterCoinIdList|[string]|false|none|Filter to cancel active orders for specific collateral coin IDs. If empty, cancels all.|
|filterContractIdList|[string]|false|none|Filter to cancel active orders for specific contract IDs. If empty, cancels all.|
|filterOrderTypeList|[string]|false|none|Filter to cancel orders of specific types. If empty, cancels all types.|
|filterOrderStatusList|[string]|false|none|Filter to cancel orders of specific statuses. If empty, cancels all statuses.|
|filterIsPositionTpsl|[boolean]|false|none|Filter to cancel only corresponding position take-profit/stop-loss orders. If empty, cancels all contract orders.|


<a id="schemacancelorderbyclientorderid"></a>
<### schemacancelorderbyclientorderid

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|cancelResultMap|object|false|none|None|

#### Enum Values

|Property|Value|
|---|---|
|**additionalProperties**|UNKNOWN_ORDER_CANCEL_RESULT|
|**additionalProperties**|SUCCESS|
|**additionalProperties**|SUCCESS_ORDER_CANCELING|
|**additionalProperties**|SUCCESS_ORDER_CANCELED|
|**additionalProperties**|FAILED_ORDER_NOT_FOUND|
|**additionalProperties**|FAILED_ORDER_FILLED|
|**additionalProperties**|FAILED_ORDER_UNKNOWN_STATUS|
|**additionalProperties**|UNRECOGNIZED|




<a id="schemaresultcancelorder"></a>
### schemaresultcancelorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[CancelOrder](#schemacancelorder)|false|none|Response for canceling orders.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|


<a id="schemacancelorder"></a>
### schemacancelorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|cancelResultMap|object|false|none|None|

#### Enum Values

|Property|Value|
|---|---|
|**additionalProperties**|UNKNOWN_ORDER_CANCEL_RESULT|
|**additionalProperties**|SUCCESS|
|**additionalProperties**|SUCCESS_ORDER_CANCELING|
|**additionalProperties**|SUCCESS_ORDER_CANCELED|
|**additionalProperties**|FAILED_ORDER_NOT_FOUND|
|**additionalProperties**|FAILED_ORDER_FILLED|
|**additionalProperties**|FAILED_ORDER_UNKNOWN_STATUS|
|**additionalProperties**|UNRECOGNIZED|

<a id="schemacancelorderbyidparam"></a>
### schemacancelorderbyidparam

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|accountId|string(int64)|false|none|Account ID.|
|orderIdList|[string]|true|none|Order ID.|


<a id="schemaresultcreateorder"></a>
###  schemaresultcreateorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[CreateOrder](#schemacreateorder)|false|none|Response for creating orders.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|

<a id="schemacreateorder"></a>
### schemacreateorder

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|orderId|string(int64)|false|none|Order ID.|


<a id="schemacreateorderparam"></a>
### schemacreateorderparam

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|accountId|string(int64)|false|none|Account ID.|
|contractId|string(int64)|false|none|Contract ID.|
|side|string|false|none|Buy/sell direction. This field is required.|
|size|string|false|none|Order quantity. Actual type is decimal. This field is required.|
|price|string|false|none|Order price (worst acceptable price). Actual type is decimal. This field is required, enter 0 for market orders.|
|clientOrderId|string|false|none|Client-defined ID for idempotency checks. This field is required.|
|type|string|false|none|Order type. This field is required.|
|timeInForce|string|false|none|Order execution policy. Relevant when `type` is `LIMIT`/`STOP_LIMIT`/`TAKE_PROFIT_LIMIT`. This field is required, and should be `IMMEDIATE_OR_CANCEL` for market orders.|
|reduceOnly|boolean|false|none|Whether this is a reduce-only order. This field is required.|
|triggerPrice|string|false|none|Trigger price. Relevant when `type` is `STOP_LIMIT`/`STOP_MARKET`/`TAKE_PROFIT_LIMIT`/`TAKE_PROFIT_MARKET`. If 0, the field is empty. Actual type is decimal. Required for conditional orders.|
|triggerPriceType|string|false|none|Price type: Last price, Mark price, etc. Relevant when the order is conditional. Required for conditional orders.|
|expireTime|string(int64)|false|none|Expiration time.|
|sourceKey|string|false|none|Source key, UUID.|
|isPositionTpsl|boolean|false|none|Whether this is a position take-profit/stop-loss order. This field is required, defaults to false.|
|openTpslParentOrderId|string(int64)|false|none|Order ID of the opening order for a take-profit or stop-loss order.|
|isSetOpenTp|boolean|false|none|Whether to set take-profit for the opening order. This field is required.|
|openTp|[OpenTpSlParam](#schemaopentpslparam)|false|none|Take-profit/stop-loss parameters for the opening order.||isSetOpenSl|boolean|false|none|Whether to set stop-loss for the opening order. This field is required.|
|openSl|[OpenTpSlParam](#schemaopentpslparam)|false|none|Take-profit/stop-loss parameters for the opening order.|
|l2Nonce|string(int64)|false|none|L2 signature nonce. Takes the first 32 bits of sha256(`client_order_id`).|
|l2Value|string|false|none|L2 signature order value (the actual filled price must be equal to or better than `l2_value` / `l2_price`). May differ from `price` x `size`. Actual type is decimal.|
|l2Size|string|false|none|L2 signature order quantity. May differ from the `size` field. Actual type is decimal.|
|l2LimitFee|string|false|none|Maximum acceptable fee for L2 signature.|
|l2ExpireTime|string(int64)|false|none|L2 signature expiration time in milliseconds. The hour value should be used when generating/verifying the signature, i.e. `l2_expire_time` / 3600000. Note that this value must be greater or equal to `expire_time` + 8 * 24 * 60 * 60 * 1000 (8 days).|
|l2Signature|string|false|none|L2 signature.|
|extraType|string|false|none|Additional type for upper-layer business use.|
|extraDataJson|string|false|none|Additional data in JSON format. Defaults to an empty string.|

#### Enum Values

|Property|Value|
|---|---|
|side|UNKNOWN_ORDER_SIDE|
|side|BUY|
|side|SELL|
|side|UNRECOGNIZED|
|type|UNKNOWN_ORDER_TYPE|
|type|LIMIT|
|type|MARKET|
|type|STOP_LIMIT|
|type|STOP_MARKET|
|type|TAKE_PROFIT_LIMIT|
|type|TAKE_PROFIT_MARKET|
|type|UNRECOGNIZED|
|timeInForce|UNKNOWN_TIME_IN_FORCE|
|timeInForce|GOOD_TIL_CANCEL|
|timeInForce|FILL_OR_KILL|
|timeInForce|IMMEDIATE_OR_CANCEL|
|timeInForce|POST_ONLY|
|timeInForce|UNRECOGNIZED|
|triggerPriceType|UNKNOWN_PRICE_TYPE|
|triggerPriceType|ORACLE_PRICE|
|triggerPriceType|INDEX_PRICE|
|triggerPriceType|LAST_PRICE|
|triggerPriceType|ASK1_PRICE|
|triggerPriceType|BID1_PRICE|
|triggerPriceType|OPEN_INTEREST|
|triggerPriceType|UNRECOGNIZED|


<a id="schemaopentpslparam"></a>
### schemaopentpslparam

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|side|string|false|none|Buy/sell direction. This field is required.|
|price|string|false|none|Order price (worst acceptable price). Actual type is decimal. This field is required, enter 0 for market orders.|
|size|string|false|none|Order quantity. Actual type is decimal. This field is required.|
|clientOrderId|string|false|none|Client-defined ID for signature and idempotency checks. This field is required.|
|triggerPrice|string|false|none|Trigger price. This field is required.|
|triggerPriceType|string|false|none|Price type: Last price, Mark price, etc. This field is required.|
|expireTime|string(int64)|false|none|Expiration time.|
|l2Nonce|string(int64)|false|none|L2 signature nonce. Takes the first 32 bits of sha256(`client_order_id`).|
|l2Value|string|false|none|L2 signature order value (the actual filled price must be equal to or better than `l2_value` / `l2_price`). May differ from `price` x `size`. Actual type is decimal.|
|l2Size|string|false|none|L2 signature order quantity. May differ from the `size` field. Actual type is decimal.|
|l2LimitFee|string|false|none|Maximum acceptable fee for L2 signature.|
|l2ExpireTime|string|false|none|L2 signature expiration time in unix hour. Must be at least 10 hours after `expire_time`.|
|l2Signature|string|false|none|L2 signature.|

#### Enum Values

|Property|Value|
|---|---|
|side|UNKNOWN_ORDER_SIDE|
|side|BUY|
|side|SELL|
|side|UNRECOGNIZED|
|triggerPriceType|UNKNOWN_PRICE_TYPE|
|triggerPriceType|ORACLE_PRICE|
|triggerPriceType|INDEX_PRICE|
|triggerPriceType|LAST_PRICE|
|triggerPriceType|ASK1_PRICE|
|triggerPriceType|BID1_PRICE|
|triggerPriceType|OPEN_INTEREST|
|triggerPriceType|UNRECOGNIZED|


<a id="schemaresultgetmaxcreateordersize"></a>
### schemaresultgetmaxcreateordersize

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise failure.|
|data|[GetMaxCreateOrderSize](#schemagetmaxcreateordersize)|false|none|Response for getting the maximum order size.|
|errorParam|object|false|none|Parameter information in error messages.|
|requestTime|string(timestamp)|false|none|Server request receive time.|
|responseTime|string(timestamp)|false|none|Server response return time.|
|traceId|string|false|none|Call trace ID.|


<a id="schemagetmaxcreateordersize"></a>
### schemagetmaxcreateordersize

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|maxBuySize|string(decimal)|false|none|Maximum buy size.|
|maxSellSize|string(decimal)|false|none|Maximum sell size.|
|ask1Price|string(decimal)|false|none|Best ask price.|
|bid1Price|string(decimal)|false|none|Best bid price.|


<a id="schemagetmaxcreateordersizeparam"></a>
### schemagetmaxcreateordersizeparam

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|accountId|string(int64)|false|none|Account ID.|
|contractId|string(int64)|false|none|Contract ID.|
|price|string|false|none|Order price.|
