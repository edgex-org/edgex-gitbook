# Account Private API

**Field naming note**: Some response fields still use historical names such as `starkExRiskValue` and `starkExRiskRate`. Keep these names as-is when parsing server responses; they are field names, not a recommendation to use the V1 StarkEx signing model.

<a id="opIdgetPositionTransactionPage"></a>

## GET Get Position Transaction Page

GET /api/v2/private/account/getPositionTransactionPage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|size|string|No|Number of items to retrieve. Must be greater than 0 and less than or equal to 100|
|offsetData|string|No|Pagination offset. If empty or not provided, the first page is retrieved|
|filterCoinIdList|string|No|Filter position transaction records by specified coin IDs. If not provided, all collateral transaction records are retrieved|
|filterContractIdList|string|No|Filter position transaction records by specified contract IDs. If not provided, all position transaction records are retrieved|
|filterTypeList|string|No|Filter position transaction records by specified types. If not provided, all position transaction records are retrieved|
|filterStartCreatedTimeInclusive|string|No|Filter position transaction records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time|
|filterEndCreatedTimeExclusive|string|No|Filter position transaction records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time|
|filterCloseOnly|string|No|Whether to return only position transactions that include closing positions.  `true`: only return records with closing; `false`: return all records|
|filterOpenOnly|string|No|Whether to return only position transactions that include opening positions. `true`: only return records with opening; `false`: return all records|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "id": "564809510904923406",
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "contractId": "10000001",
                "type": "SELL_POSITION",
                "deltaOpenSize": "-0.001",
                "deltaOpenValue": "-96.813200",
                "deltaOpenFee": "0.048406",
                "deltaFundingFee": "0.000000",
                "beforeOpenSize": "0.001",
                "beforeOpenValue": "96.813200",
                "beforeOpenFee": "-0.048406",
                "beforeFundingFee": "0",
                "fillCloseSize": "-0.001",
                "fillCloseValue": "-96.857100",
                "fillCloseFee": "-0.048428",
                "fillOpenSize": "0.000",
                "fillOpenValue": "0.000000",
                "fillOpenFee": "0.000000",
                "fillPrice": "96857.1",
                "liquidateFee": "0",
                "realizePnl": "-0.004528",
                "isLiquidate": false,
                "isDeleverage": false,
                "fundingTime": "0",
                "fundingRate": "",
                "fundingIndexPrice": "",
                "fundingOraclePrice": "",
                "fundingPositionSize": "",
                "orderId": "564809510842007822",
                "orderFillTransactionId": "564809510875562254",
                "collateralTransactionId": "564809510904922382",
                "forceTradeId": "0",
                "extraType": "",
                "extraDataJson": "",
                "censorStatus": "CENSOR_SUCCESS",
                "censorTxId": "892720",
                "censorTime": "1734661081049",
                "censorFailCode": "",
                "censorFailReason": "",
                "l2TxId": "1084271",
                "l2RejectTime": "0",
                "l2RejectCode": "",
                "l2RejectReason": "",
                "l2ApprovedTime": "0",
                "createdTime": "1734661081049",
                "updatedTime": "1734661081053"
            }
        ],
        "nextPageOffsetData": ""
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734661416266",
    "responseTime": "1734661416277",
    "traceId": "a87a52a4e189045b7b7b9948ea7b5c54"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#positiontransactionpage) |

<a id="opIdgetPositionTransactionById"></a>

## GET Get Position Transactions By Account ID and Transaction ID

GET /api/v2/private/account/getPositionTransactionById

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|positionTransactionIdList|string|Yes|Position Transaction IDs|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "id": "564809510904923406",
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "contractId": "10000001",
                "type": "SELL_POSITION",
                "deltaOpenSize": "-0.001",
                "deltaOpenValue": "-96.813200",
                "deltaOpenFee": "0.048406",
                "deltaFundingFee": "0.000000",
                "beforeOpenSize": "0.001",
                "beforeOpenValue": "96.813200",
                "beforeOpenFee": "-0.048406",
                "beforeFundingFee": "0",
                "fillCloseSize": "-0.001",
                "fillCloseValue": "-96.857100",
                "fillCloseFee": "-0.048428",
                "fillOpenSize": "0.000",
                "fillOpenValue": "0.000000",
                "fillOpenFee": "0.000000",
                "fillPrice": "96857.1",
                "liquidateFee": "0",
                "realizePnl": "-0.004528",
                "isLiquidate": false,
                "isDeleverage": false,
                "fundingTime": "0",
                "fundingRate": "",
                "fundingIndexPrice": "",
                "fundingOraclePrice": "",
                "fundingPositionSize": "",
                "orderId": "564809510842007822",
                "orderFillTransactionId": "564809510875562254",
                "collateralTransactionId": "564809510904922382",
                "forceTradeId": "0",
                "extraType": "",
                "extraDataJson": "",
                "censorStatus": "CENSOR_SUCCESS",
                "censorTxId": "892720",
                "censorTime": "1734661081049",
                "censorFailCode": "",
                "censorFailReason": "",
                "l2TxId": "1084271",
                "l2RejectTime": "0",
                "l2RejectCode": "",
                "l2RejectReason": "",
                "l2ApprovedTime": "0",
                "createdTime": "1734661081049",
                "updatedTime": "1734661081053"
            }
        ],
        "nextPageOffsetData": ""
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734661416266",
    "responseTime": "1734661416277",
    "traceId": "a87a52a4e189045b7b7b9948ea7b5c54"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#positiontransaction) |

<a id="opIdgetPositionTermPage"></a>

## GET Get Position Term Page by Account ID

GET /api/v2/private/account/getPositionTermPage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|size|string|No|Number of items to retrieve. Must be greater than 0 and less than or equal to 100|
|offsetData|string|No|Pagination offset. If empty or not provided, the first page is retrieved|
|filterCoinIdList|string|No|Filter position term records by specified coin IDs. If not provided, all position term records are retrieved|
|filterContractIdList|string|No|Filter position term records by specified contract IDs. If not provided, all position term records are retrieved|
|filterIsLongPosition|string|No|Filter position term records by position direction. If not provided, all position term records are retrieved|
|filterStartCreatedTimeInclusive|string|No|Filter position term records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time|
|filterEndCreatedTimeExclusive|string|No|Filter position term records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "contractId": "10000001",
                "termCount": 2,
                "cumOpenSize": "0.001",
                "cumOpenValue": "96.813000",
                "cumOpenFee": "-0.048406",
                "cumCloseSize": "0",
                "cumCloseValue": "0",
                "cumCloseFee": "0",
                "cumFundingFee": "0",
                "cumLiquidateFee": "0",
                "createdTime": "1734661093450",
                "updatedTime": "1734661093450",
                "currentLeverage": "50"
            },
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "contractId": "10000001",
                "termCount": 1,
                "cumOpenSize": "0.001",
                "cumOpenValue": "96.813200",
                "cumOpenFee": "-0.048406",
                "cumCloseSize": "-0.001",
                "cumCloseValue": "-96.857100",
                "cumCloseFee": "-0.048428",
                "cumFundingFee": "0",
                "cumLiquidateFee": "0",
                "createdTime": "1734661018663",
                "updatedTime": "1734661081053",
                "currentLeverage": "50"
            }
        ],
        "nextPageOffsetData": ""
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734661416272",
    "responseTime": "1734661416281",
    "traceId": "ad4515e50fa7a57610736753d8f987aa"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#positionterm) |

<a id="opIdgetCollateralByCoinId"></a>

## GET Get Position By Account ID and Contract ID

GET /api/v2/private/account/getPositionByContractId

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|contractIdList|string|Yes|Specified contract IDs|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "contractId": "10000001",
            "openSize": "0.001",
            "openValue": "97.444500",
            "openFee": "-0.017540",
            "fundingFee": "0.000000",
            "longTermCount": 3,
            "longTermStat": {
                "cumOpenSize": "0.001",
                "cumOpenValue": "97.444500",
                "cumOpenFee": "-0.017540",
                "cumCloseSize": "0",
                "cumCloseValue": "0",
                "cumCloseFee": "0",
                "cumFundingFee": "0",
                "cumLiquidateFee": "0"
            },
            "longTermCreatedTime": "1734662617992",
            "longTermUpdatedTime": "1734662617992",
            "shortTermCount": 0,
            "shortTermStat": {
                "cumOpenSize": "0",
                "cumOpenValue": "0",
                "cumOpenFee": "0",
                "cumCloseSize": "0",
                "cumCloseValue": "0",
                "cumCloseFee": "0",
                "cumFundingFee": "0",
                "cumLiquidateFee": "0"
            },
            "shortTermCreatedTime": "0",
            "shortTermUpdatedTime": "0",
            "longTotalStat": {
                "cumOpenSize": "0.004",
                "cumOpenValue": "388.464500",
                "cumOpenFee": "-0.131882",
                "cumCloseSize": "-0.003",
                "cumCloseValue": "-291.736700",
                "cumCloseFee": "-0.083506",
                "cumFundingFee": "0",
                "cumLiquidateFee": "0"
            },
            "shortTotalStat": {
                "cumOpenSize": "0",
                "cumOpenValue": "0",
                "cumOpenFee": "0",
                "cumCloseSize": "0",
                "cumCloseValue": "0",
                "cumCloseFee": "0",
                "cumFundingFee": "0",
                "cumLiquidateFee": "0"
            },
            "createdTime": "1734661018663",
            "updatedTime": "1734662617992"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734664849770",
    "responseTime": "1734664849790",
    "traceId": "17a421d1b23652c5b3836239274b0352"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | Inline     |

### Response Data Structure

<a id="opIdgetCollateralTransactionPage"></a>

## GET Get Collateral Transaction Page by Account ID

GET /api/v2/private/account/getCollateralTransactionPage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|size|string|No|Number of items to retrieve. Must be greater than 0 and less than or equal to 100|
|offsetData|string|No|Pagination offset. If empty or not provided, the first page is retrieved|
|filterCoinIdList|string|No|Filter collateral transaction records by specified coin IDs. If not provided, all collateral transaction records are retrieved|
|filterTypeList|string|No|Filter collateral transaction records by specified transaction types. If not provided, all collateral transaction records are retrieved|
|filterStartCreatedTimeInclusive|string|No|Filter collateral transaction records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time|
|filterEndCreatedTimeExclusive|string|No|Filter collateral transaction records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time|

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

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | Inline     |

### Response Data Structure

<a id="opIdgetCollateralTransactionById"></a>

## GET Get Collateral Transactions By Account ID and Transaction ID

GET /api/v2/private/account/getCollateralTransactionById

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|collateralTransactionIdList|string|Yes|Collateral Transaction IDs|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "id": "563516408265179918",
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "type": "DEPOSIT",
            "deltaAmount": "10.000000",
            "deltaLegacyAmount": "10.000000",
            "beforeAmount": "6.000000",
            "beforeLegacyAmount": "6.000000",
            "fillCloseSize": "",
            "fillCloseValue": "",
            "fillCloseFee": "",
            "fillOpenSize": "",
            "fillOpenValue": "",
            "fillOpenFee": "",
            "fillPrice": "",
            "liquidateFee": "",
            "realizePnl": "",
            "isLiquidate": false,
            "isDeleverage": false,
            "fundingTime": "0",
            "fundingRate": "",
            "fundingIndexPrice": "",
            "fundingOraclePrice": "",
            "fundingPositionSize": "",
            "depositId": "563516408235819790",
            "withdrawId": "0",
            "transferInId": "0",
            "transferOutId": "0",
            "transferReason": "UNKNOWN_TRANSFER_REASON",
            "orderId": "0",
            "orderFillTransactionId": "0",
            "orderAccountId": "0",
            "positionContractId": "0",
            "positionTransactionId": "0",
            "forceWithdrawId": "0",
            "forceTradeId": "0",
            "extraType": "",
            "extraDataJson": "",
            "censorStatus": "L2_APPROVED",
            "censorTxId": "830852",
            "censorTime": "1734352781355",
            "censorFailCode": "",
            "censorFailReason": "",
            "l2TxId": "1022403",
            "l2RejectTime": "0",
            "l2RejectCode": "",
            "l2RejectReason": "",
            "l2ApprovedTime": "1734353551654",
            "createdTime": "1734352781355",
            "updatedTime": "1734353551715"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734664486740",
    "responseTime": "1734664486761",
    "traceId": "b3086f53c2d4503f6a4790b80f0e534b"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | Inline     |

### Response Data Structure

<a id="opIdgetCollateralByCoinId_1"></a>

## GET Get Collateral By Account ID and Coin ID

GET /api/v2/private/account/getCollateralByCoinId

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|coinIdList|string|Yes|Filter collateral information by specified coin IDs. If not provided, all collateral information is retrieved|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "amount": "-81.943188",
            "legacyAmount": "15.501312",
            "cumDepositAmount": "70.000000",
            "cumWithdrawAmount": "0",
            "cumTransferInAmount": "0",
            "cumTransferOutAmount": "-55.000000",
            "cumPositionBuyAmount": "-388.4645",
            "cumPositionSellAmount": "291.7367",
            "cumFillFeeAmount": "-0.215388",
            "cumFundingFeeAmount": "0",
            "cumFillFeeIncomeAmount": "0",
            "createdTime": "1730204434094",
            "updatedTime": "1734663352066"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734664569244",
    "responseTime": "1734664569260",
    "traceId": "4b7ff82fb92aa3b10d9fc0367a069270"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | Inline     |

### Response Data Structure

<a id="opIdgetAccountPage"></a>

## GET Get Account Page by User ID

GET /api/v2/private/account/getAccountPage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|size|string|No|Number of items to retrieve. Must be greater than 0 and less than or equal to 100|
|offsetData|string|No|Pagination offset. If empty or not provided, the first page is retrieved|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "id": "543429922991899150",
                "userId": "543429922866069763",
                "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
                "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
                "l2KeyYCoordinate": "0x6ea3dd81a7fc864893c8c6f674e4a4510c369f939bdc0259a0980dfde882c2d",
                "clientAccountId": "main",
                "isSystemAccount": false,
                "defaultTradeSetting": {
                    "isSetFeeRate": true,
                    "takerFeeRate": "0.000500",
                    "makerFeeRate": "0.000180",
                    "isSetFeeDiscount": false,
                    "takerFeeDiscount": "0",
                    "makerFeeDiscount": "0",
                    "isSetMaxLeverage": false,
                    "maxLeverage": "0"
                },
                "contractIdToTradeSetting": {
                    "10000001": {
                        "isSetFeeRate": false,
                        "takerFeeRate": "",
                        "makerFeeRate": "",
                        "isSetFeeDiscount": false,
                        "takerFeeDiscount": "",
                        "makerFeeDiscount": "",
                        "isSetMaxLeverage": true,
                        "maxLeverage": "50"
                    }
                },
                "maxLeverageLimit": "0",
                "createOrderPerMinuteLimit": 0,
                "createOrderDelayMillis": 0,
                "extraType": "",
                "extraDataJson": "",
                "status": "NORMAL",
                "isLiquidating": false,
                "createdTime": "1730204434094",
                "updatedTime": "1733993378059"
            }
        ],
        "nextPageOffsetData": "551109015904453258"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734661416005",
    "responseTime": "1734661416008",
    "traceId": "dc6a8442169c8cdb831ceb15c812b7fc"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#schemaresultaccount) |

<a id="opIdgetAccountDeleverageLight"></a>

## GET Get Account Deleverage Light

GET /api/v2/private/account/getAccountDeleverageLight

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "positionContractIdToLightNumberMap": {
            "10000001": 3
        }
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734661307929",
    "responseTime": "1734661307935",
    "traceId": "202ee8ba15ab633b68a35a8bc9756952"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#getaccountdeleveragelight) |

<a id="opIdgetAccountById"></a>

## GET Get Account By Account ID

GET /api/v2/private/account/getAccountById

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "id": "543429922991899150",
        "userId": "543429922866069763",
        "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
        "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
        "l2KeyYCoordinate": "0x6ea3dd81a7fc864893c8c6f674e4a4510c369f939bdc0259a0980dfde882c2d",
        "clientAccountId": "main",
        "isSystemAccount": false,
        "defaultTradeSetting": {
            "isSetFeeRate": true,
            "takerFeeRate": "0.000500",
            "makerFeeRate": "0.000180",
            "isSetFeeDiscount": false,
            "takerFeeDiscount": "0",
            "makerFeeDiscount": "0",
            "isSetMaxLeverage": false,
            "maxLeverage": "0"
        },
        "contractIdToTradeSetting": {
            "10000001": {
                "isSetFeeRate": false,
                "takerFeeRate": "",
                "makerFeeRate": "",
                "isSetFeeDiscount": false,
                "takerFeeDiscount": "",
                "makerFeeDiscount": "",
                "isSetMaxLeverage": true,
                "maxLeverage": "50"
            }
        },
        "maxLeverageLimit": "0",
        "createOrderPerMinuteLimit": 0,
        "createOrderDelayMillis": 0,
        "extraType": "",
        "extraDataJson": "",
        "status": "NORMAL",
        "isLiquidating": false,
        "createdTime": "1730204434094",
        "updatedTime": "1733993378059"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734664605752",
    "responseTime": "1734664605760",
    "traceId": "c7be70afbf00d7f879d2809e0f042dfe"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#account) |

<a id="opIdgetAccountAsset"></a>

## GET Account Asset

GET /api/v2/private/account/getAccountAsset

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "account": {
            "id": "543429922991899150",
            "userId": "543429922866069763",
            "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
            "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
            "l2KeyYCoordinate": "0x6ea3dd81a7fc864893c8c6f674e4a4510c369f939bdc0259a0980dfde882c2d",
            "clientAccountId": "main",
            "isSystemAccount": false,
            "defaultTradeSetting": {
                "isSetFeeRate": true,
                "takerFeeRate": "0.000500",
                "makerFeeRate": "0.000180",
                "isSetFeeDiscount": false,
                "takerFeeDiscount": "0",
                "makerFeeDiscount": "0",
                "isSetMaxLeverage": false,
                "maxLeverage": "0"
            },
            "contractIdToTradeSetting": {
                "10000001": {
                    "isSetFeeRate": false,
                    "takerFeeRate": "",
                    "makerFeeRate": "",
                    "isSetFeeDiscount": false,
                    "takerFeeDiscount": "",
                    "makerFeeDiscount": "",
                    "isSetMaxLeverage": true,
                    "maxLeverage": "50"
                }
            },
            "maxLeverageLimit": "0",
            "createOrderPerMinuteLimit": 0,
            "createOrderDelayMillis": 0,
            "extraType": "",
            "extraDataJson": "",
            "status": "NORMAL",
            "isLiquidating": false,
            "createdTime": "1730204434094",
            "updatedTime": "1733993378059"
        },
        "collateralList": [
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "amount": "-81.943188",
                "legacyAmount": "15.501312",
                "cumDepositAmount": "70.000000",
                "cumWithdrawAmount": "0",
                "cumTransferInAmount": "0",
                "cumTransferOutAmount": "-55.000000",
                "cumPositionBuyAmount": "-388.4645",
                "cumPositionSellAmount": "291.7367",
                "cumFillFeeAmount": "-0.215388",
                "cumFundingFeeAmount": "0",
                "cumFillFeeIncomeAmount": "0",
                "createdTime": "1730204434094",
                "updatedTime": "1734663352066"
            }
        ],
        "positionList": [
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "contractId": "10000001",
                "openSize": "0.001",
                "openValue": "97.444500",
                "openFee": "-0.017540",
                "fundingFee": "0.000000",
                "longTermCount": 3,
                "longTermStat": {
                    "cumOpenSize": "0.001",
                    "cumOpenValue": "97.444500",
                    "cumOpenFee": "-0.017540",
                    "cumCloseSize": "0",
                    "cumCloseValue": "0",
                    "cumCloseFee": "0",
                    "cumFundingFee": "0",
                    "cumLiquidateFee": "0"
                },
                "longTermCreatedTime": "1734662617992",
                "longTermUpdatedTime": "1734662617992",
                "shortTermCount": 0,
                "shortTermStat": {
                    "cumOpenSize": "0",
                    "cumOpenValue": "0",
                    "cumOpenFee": "0",
                    "cumCloseSize": "0",
                    "cumCloseValue": "0",
                    "cumCloseFee": "0",
                    "cumFundingFee": "0",
                    "cumLiquidateFee": "0"
                },
                "shortTermCreatedTime": "0",
                "shortTermUpdatedTime": "0",
                "longTotalStat": {
                    "cumOpenSize": "0.004",
                    "cumOpenValue": "388.464500",
                    "cumOpenFee": "-0.131882",
                    "cumCloseSize": "-0.003",
                    "cumCloseValue": "-291.736700",
                    "cumCloseFee": "-0.083506",
                    "cumFundingFee": "0",
                    "cumLiquidateFee": "0"
                },
                "shortTotalStat": {
                    "cumOpenSize": "0",
                    "cumOpenValue": "0",
                    "cumOpenFee": "0",
                    "cumCloseSize": "0",
                    "cumCloseValue": "0",
                    "cumCloseFee": "0",
                    "cumFundingFee": "0",
                    "cumLiquidateFee": "0"
                },
                "createdTime": "1734661018663",
                "updatedTime": "1734662617992"
            }
        ],
        "version": "1021",
        "positionAssetList": [
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "contractId": "10000001",
                "positionValue": "97.734426609240472316741943359375",
                "maxLeverage": "50",
                "initialMarginRequirement": "1.954688532184809446334838867187500000",
                "starkExRiskRate": "0.00500000012107193470001220703125",
                "starkExRiskValue": "0.48867214487909847796080764492643311314168386161327362060546875",
                "avgEntryPrice": "97444.5",
                "liquidatePrice": "82354.9",
                "bankruptPrice": "81943.1",
                "worstClosePrice": "81984.2",
                "unrealizePnl": "0.289926609240472316741943359375",
                "termRealizePnl": "0.000000",
                "totalRealizePnl": "0.716700"
            }
        ],
        "collateralAssetModelList": [
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "totalEquity": "15.791238609240472316741943359375",
                "totalPositionValueAbs": "97.734426609240472316741943359375",
                "initialMarginRequirement": "1.954688532184809446334838867187500000",
                "starkExRiskValue": "0.48867214487909847796080764492643311314168386161327362060546875",
                "pendingWithdrawAmount": "0",
                "pendingTransferOutAmount": "0",
                "orderFrozenAmount": "0",
                "availableAmount": "13.836550"
            }
        ],
        "oraclePriceList": []
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734664627939",
    "responseTime": "1734664627957",
    "traceId": "4a3a5cd027ea6c255c8c944567b634f1"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ---------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#accountassetsnapshot) |

<a id="opIdgetAccountAssetSnapshotPage"></a>

## GET Get Account Asset Snapshot Page by Account ID

GET /api/v2/private/account/getAccountAssetSnapshotPage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|size|string|No|Number of items to retrieve. Must be greater than 0 and less than or equal to 1000|
|offsetData|string|No|Pagination offset. If empty or not provided, the first page is retrieved|
|coinId|string|No|Filter by the specified coin ID.|
|filterTimeTag|string|No|Specifies time tag. If not provided or 0, returns snapshots by the hour. 1 returns snapshots by the day|
|filterStartTimeInclusive|string|No|Filter snapshots created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time|
|filterEndTimeExclusive|string|No|Filter snapshots created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "timeTag": 1,
                "snapshotTime": "1734652800000",
                "totalEquity": "16.000000",
                "termRealizePnl": "0",
                "unrealizePnl": "0",
                "totalRealizePnl": "0"
            },
            {
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "timeTag": 1,
                "snapshotTime": "1734566400000",
                "totalEquity": "16.000000",
                "termRealizePnl": "0",
                "unrealizePnl": "0",
                "totalRealizePnl": "0"
            }
        ],
        "nextPageOffsetData": ""
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734663257066",
    "responseTime": "1734663257075",
    "traceId": "f52222cd41b6ff8bcd059a57ecd986a1"
}
```

### Response

| Status Code | Status Code Description                                                                  | Description        | Data Model                                        |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------ | ------------------------------------------------- |
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#accountassetsnapshot>) |

# Data Models


<a id="accountassetsnapshot"></a>
### accountassetsnapshot

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[PageDataAccountAssetSnapshot](#schemapagedataaccountassetsnapshot)|Generic Paginated Response|
|errorParam|object|Error Parameters|
|» **additionalProperties**|string|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|

<a id="schemapagedataaccountassetsnapshot"></a>
### schemapagedataaccountassetsnapshot

|Name|Type|Description|
|---|---|---|
|dataList|[[AccountAssetSnapshot](#schemaaccountassetsnapshot)]|Data List|
|nextPageOffsetData|string|Next Page Offset|

<a id="schemaaccountassetsnapshot"></a>
### schemaaccountassetsnapshot

|Name|Type|Description|
|---|---|---|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral Coin ID|
|timeTag|integer(int32)|Time Tag|
|snapshotTime|string(int64)|Snapshot Time|
|totalEquity|string|Total Collateral Value|
|termRealizePnl|string|Term Realized PnL|
|unrealizePnl|string|Unrealized PnL|
|totalRealizePnl|string|Total Realized PnL|

<a id="schemaresultgetaccountasset"></a>
### schemaresultgetaccountasset

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[GetAccountAsset](#schemagetaccountasset)|Get Account Asset Response|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemagetaccountasset"></a>
### schemagetaccountasset

|Name|Type|Description|
|---|---|---|
|account|[Account](#schemaaccount)|Account Information|
|collateralList|[[Collateral](#schemacollateral)]|Collateral Information List|
|positionList|[[Position](#schemaposition)]|Perpetual Contract Position List|
|version|string(int64)|Account Version|
|positionAssetList|[[PositionAsset](#schemapositionasset)]|Position Asset Information List|
|collateralAssetModelList|[[CollateralAsset](#schemacollateralasset)]|Account-Level Asset Information List|
|oraclePriceList|[[IndexPrice](#schemaindexprice)]|Oracle Price List|

<a id="schemaindexprice"></a>
### schemaindexprice

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Contract ID|
|priceType|string|Price Type|
|priceValue|string|Price Value|
|createdTime|string(int64)|Creation Time|
|oraclePriceSignature|[[OraclePriceSignature](#schemaoraclepricesignature)]|Oracle Price Signature Information|

#### Enumerated Values

| Property  | Value                     |
| --------- | ------------------------- |
| priceType | UNKNOWN_PRICE_TYPE        |
| priceType | ORACLE_PRICE            |
| priceType | INDEX_PRICE               |
| priceType | LAST_PRICE                |
| priceType | ASK1_PRICE                |
| priceType | BID1_PRICE                |
| priceType | OPEN_INTEREST             |
| priceType | UNRECOGNIZED              |

<a id="schemaoraclepricesignature"></a>
### schemaoraclepricesignature

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Contract ID|
|signer|string|Signer ID|
|price|string|Signed Price|
|externalAssetId|string|Concatenated Asset and Oracle Names|
|signature|[L2Signature](#schemal2signature)|L2 Signature Information|
|timestamp|string(int64)|Signature Creation Time|

<a id="schemal2signature"></a>
### schemal2signature

|Name|Type|Description|
|---|---|---|
|r|string|R Value|
|s|string|S Value|
|v|string|V Value|

<a id="schemacollateralasset"></a>
### schemacollateralasset

|Name|Type|Description|
|---|---|---|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral Coin ID|
|totalEquity|string|Total Collateral Value|
|totalPositionValueAbs|string|Sum of Absolute Position Values|
|initialMarginRequirement|string|Initial Margin Requirement|
|starkExRiskValue|string|Total StarkEx Risk Value|
|pendingWithdrawAmount|string|Pending Withdrawal Amount|
|pendingTransferOutAmount|string|Pending Transfer Out Amount|
|orderFrozenAmount|string|Order Frozen Amount|
|availableAmount|string|Available Amount|

<a id="schemapositionasset"></a>
### schemapositionasset

|Name|Type|Description|
|---|---|---|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral Coin ID|
|contractId|string(int64)|Contract ID|
|positionValue|string|Position Value|
|maxLeverage|string|Maximum Leverage|
|initialMarginRequirement|string|Initial Margin Requirement|
|starkExRiskRate|string|StarkEx Risk Rate|
|starkExRiskValue|string|StarkEx Risk Value|
|avgEntryPrice|string|Average Entry Price|
|liquidatePrice|string|Liquidation Price|
|bankruptPrice|string|Bankruptcy Price|
|worstClosePrice|string|Worst Close Price|
|unrealizePnl|string|Unrealized PnL|
|termRealizePnl|string|Term Realized PnL|
|totalRealizePnl|string|Total Realized PnL|

<a id="schemaposition"></a>
### schemaposition

|Name|Type|Description|
|---|---|---|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral Coin ID|
|contractId|string(int64)|Contract ID|
|openSize|string|Current Open Size|
|openValue|string|Current Open Value|
|openFee|string|Current Open Fee|
|fundingFee|string|Current Funding Fee|
|longTermCount|integer(int32)|Long Position Term Count|
|longTermStat|[PositionStat](#schemapositionstat)|Long Position Cumulative Statistics|
|longTermCreatedTime|string|Long Position Term Creation Time|
|longTermUpdatedTime|string|Long Position Term Update Time|
|shortTermCount|integer(int32)|Short Position Term Count|
|shortTermStat|[PositionStat](#schemapositionstat)|Short Position Cumulative Statistics|
|shortTermCreatedTime|string|Short Position Term Creation Time|
|shortTermUpdatedTime|string|Short Position Term Update Time|
|longTotalStat|[PositionStat](#schemapositionstat)|Long Cumulative Statistics|
|shortTotalStat|[PositionStat](#schemapositionstat)|Short Cumulative Statistics|
|createdTime|string(int64)|Creation Time|
|updatedTime|string(int64)|Update Time|


<a id="schemapositionstat"></a>
### schemapositionstat

|Name|Type|Description|
|---|---|---|
|cumOpenSize|string|Cumulative Open Size|
|cumOpenValue|string|Cumulative Open Value|
|cumOpenFee|string|Cumulative Open Fee|
|cumCloseSize|string|Cumulative Close Size|
|cumCloseValue|string|Cumulative Close Value|
|cumCloseFee|string|Cumulative Close Fee|
|cumFundingFee|string|Cumulative Funding Fee|
|cumLiquidateFee|string|Cumulative Liquidate Fee|

<a id="schemacollateral"></a>
### schemacollateral

|Name|Type|Description|
|---|---|---|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Coin ID|
|amount|string(decimal)|Collateral Amount|
|legacyAmount|string(decimal)|Legacy Amount|
|cumDepositAmount|string(decimal)|Cumulative Deposit Amount|
|cumWithdrawAmount|string(decimal)|Cumulative Withdrawal Amount|
|cumTransferInAmount|string(decimal)|Cumulative Transfer In Amount|
|cumTransferOutAmount|string(decimal)|Cumulative Transfer Out Amount|
|cumPositionBuyAmount|string(decimal)|Cumulative Position Buy Amount|
|cumPositionSellAmount|string(decimal)|Cumulative Position Sell Amount|
|cumFillFeeAmount|string(decimal)|Cumulative Fill Fee Amount|
|cumFundingFeeAmount|string(decimal)|Cumulative Funding Fee Amount|
|cumFillFeeIncomeAmount|string(decimal)|Cumulative Order Fill Fee Income Amount|
|createdTime|string(int64)|Creation Time|
|updatedTime|string(int64)|Update Time|

<a id="schemaaccount"></a>
### schemaaccount

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Account ID|
|userId|string(int64)|User ID|
|ethAddress|string|Wallet ETH Address|
|l2Key|string|L2 Account Key|
|l2KeyYCoordinate|string|L2 Key Y Coordinate|
|clientAccountId|string|Client Account ID|
|isSystemAccount|boolean|System Account|
|defaultTradeSetting|[TradeSetting](#schematradesetting)|Default Trade Setting|
|contractIdToTradeSetting|object|Contract-Level Account Trade Settings|
|» **additionalProperties**|[TradeSetting](#schematradesetting)|Contract-Level Account Trade Settings|
|maxLeverageLimit|string|Maximum Leverage Limit|
|createOrderPerMinuteLimit|integer(int32)|Order Creation Limit per Minute|
|createOrderDelayMillis|integer(int32)|Order Creation Delay Milliseconds|
|extraType|string|Extra Type|
|extraDataJson|string|Extra Data|
|status|string|Account Status|
|isLiquidating|boolean|Is Liquidating|
|createdTime|string(int64)|Creation Time|
|updatedTime|string(int64)|Update Time|

#### Enumerated Values

| Property | Value                 |
| -------- | --------------------- |
| status   | UNKNOWN_ACCOUNT_STATUS |
| status   | CENSORING             |
| status   | NORMAL                |
| status   | DISABLED              |
| status   | INVALID               |
| status   | UNRECOGNIZED          |

<a id="schematradesetting"></a>
### schematradesetting

|Name|Type|Description|
|---|---|---|
|isSetFeeRate|boolean|Whether Fee Rate is Set|
|takerFeeRate|string(decimal)|Taker Fee Rate|
|makerFeeRate|string(decimal)|Maker Fee Rate|
|isSetFeeDiscount|boolean|Whether Fee Discount is Set|
|takerFeeDiscount|string(decimal)|Taker Fee Discount|
|makerFeeDiscount|string(decimal)|Maker Fee Discount|
|isSetMaxLeverage|boolean|Whether Maximum Leverage is Set|
|maxLeverage|string(decimal)|Maximum Leverage|


<a id="schemaresultaccount"></a>
### schemaresultaccount

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[Account](#schemaaccount)|Account Information|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="getaccountdeleveragelight"></a>
### getaccountdeleveragelight

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[GetAccountDeleverageLight](#schemagetaccountdeleveragelight)|Get Account Deleverage Light Response|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|

<a id="schemagetaccountdeleveragelight"></a>
### Response structure for fetching deleverage light information.

|Name|Type|Description|
|---|---|---|
|positionContractIdToLightNumberMap|object|Map from Position Contract ID to Light Number|


<a id="account"></a>
### account

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[PageDataAccount](#schemapagedataaccount)|Generic Paginated Response|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemapagedataaccount"></a>
### Generic Paginated Response

|Name|Type|Description|
|---|---|---|
|dataList|[[Account](#schemaaccount)]|Data List|
|nextPageOffsetData|string|Next Page Offset|

<a id="collateral"></a>
### collateral

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[[Collateral](#schemacollateral)]|Response Data|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="collateraltransaction"></a>
### collateraltransaction

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[[CollateralTransaction](#schemacollateraltransaction)]|Response Data|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemacollateraltransaction"></a>
### Collateral transaction details

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Unique Identifier|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Coin ID|
|type|string|Detail Type|
|deltaAmount|string(decimal)|Collateral Change Amount|
|deltaLegacyAmount|string(decimal)|Legacy Balance Change Amount|
|beforeAmount|string(decimal)|Collateral Amount Before Change|
|beforeLegacyAmount|string(decimal)|Legacy Balance Amount Before Change|
|fillCloseSize|string(decimal)|Transaction Close Size|
|fillCloseValue|string|Transaction Close Value|
|fillCloseFee|string|Transaction Close Fee|
|fillOpenSize|string(decimal)|Transaction Open Size|
|fillOpenValue|string|Transaction Open Value|
|fillOpenFee|string|Transaction Open Fee|
|fillPrice|string(decimal)|Transaction Price|
|liquidateFee|string(decimal)|Liquidation Fee|
|realizePnl|string(decimal)|Realized Profit and Loss|
|isLiquidate|boolean|Is Liquidation|
|isDeleverage|boolean|Is Auto-Deleveraging|
|fundingTime|string(int64)|Funding Settlement Time|
|fundingRate|string(decimal)|Funding Rate|
|fundingIndexPrice|string(decimal)|Funding Index Price|
|fundingOraclePrice|string(decimal)|Funding Oracle Price|
|fundingPositionSize|string(decimal)|Position Size During Funding Settlement|
|depositId|string(int64)|Deposit Order ID|
|withdrawId|string(int64)|Withdrawal Order ID|
|transferInId|string(int64)|Transfer In Order ID|
|transferOutId|string(int64)|Transfer Out Order ID|
|transferReason|string|Transfer Reason|
|orderId|string(int64)|Order ID|
|orderFillTransactionId|string(int64)|Order Fill Transaction ID|
|orderAccountId|string(int64)|Order Account ID|
|positionContractId|string(int64)|Position Contract ID|
|positionTransactionId|string(int64)|Position Transaction ID|
|forceWithdrawId|string|Force Withdrawal Order ID|
|forceTradeId|string|Force Trade ID|
|extraType|string|Extra Type|
|extraDataJson|string|Extra Data|
|censorStatus|string|Current Censoring Status|
|censorTxId|string(int64)|Censoring Processing Sequence Number|
|censorTime|string(int64)|Censoring Processing Time|
|censorFailCode|string|Censoring Failure Code|
|censorFailReason|string|Censoring Failure Reason|
|l2TxId|string(int64)|L2 Push Transaction ID|
|l2RejectTime|string(int64)|L2 Rejection Time|
|l2RejectCode|string|L2 Rejection Error Code|
|l2RejectReason|string|L2 Rejection Reason|
|l2ApprovedTime|string(int64)|L2 Batch Verification Time|
|createdTime|string(int64)|Creation Time|
|updatedTime|string(int64)|Update Time|

#### Enumerated Values

| Property       | Value                            |
| -------------- | -------------------------------- |
| type           | UNKNOWN_COLLATERAL_TRANSACTION_TYPE    |
| type           | DEPOSIT                       |
| type           | WITHDRAW                      |
| type           | TRANSFER_IN                   |
| type           | TRANSFER_OUT                  |
| type           | POSITION_BUY                  |
| type           | POSITION_SELL                 |
| type           | POSITION_FUNDING             |
| type           | FILL_FEE_INCOME               |
| type           | BUG_FIX_COLLATERAL_TRANSACTION_TYPE  |
| type           | UNRECOGNIZED                  |
| transferReason | UNKNOWN_TRANSFER_REASON       |
| transferReason | USER_TRANSFER                |
| transferReason | FAST_WITHDRAW                  |
| transferReason | CROSS_DEPOSIT                |
| transferReason | CROSS_WITHDRAW              |
| transferReason | UNRECOGNIZED                  |
| censorStatus   | UNKNOWN_TRANSACTION_STATUS      |
| censorStatus   | INIT                        |
| censorStatus   | CENSOR_SUCCESS                 |
| censorStatus   | CENSOR_FAILURE               |
| censorStatus   | L2_APPROVED                   |
| censorStatus   | L2_REJECT                     |
| censorStatus   | L2_REJECT_APPROVED             |
| censorStatus   | UNRECOGNIZED                   |


<a id="collateraltransaction>"></a>
### collateraltransaction

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[PageDataCollateralTransaction](#schemapagedatacollateraltransaction)|Generic Paginated Response|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemapagedatacollateraltransaction"></a>
### schemapagedatacollateraltransaction

|Name|Type|Description|
|---|---|---|
|dataList|[[CollateralTransaction](#schemacollateraltransaction)]|Data List|
|nextPageOffsetData|string|Next Page Offset|


### position

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[[Position](#schemaposition)]|Response Data|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="positionterm"></a>
### positionterm

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[PageDataPositionTerm](#schemapagedatapositionterm)|Generic Paginated Response|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemapagedatapositionterm"></a>
### schemapagedatapositionterm

|Name|Type|Description|
|---|---|---|
|dataList|[[PositionTerm](#schemapositionterm)]|Data List|
|nextPageOffsetData|string|Next Page Offset|


<a id="schemapositionterm"></a>
### schemapositionterm

|Name|Type|Description|
|---|---|---|
|userId|string|User ID|
|accountId|string|Account ID|
|coinId|string|Collateral Coin ID|
|contractId|string|Contract ID|
|termCount|integer(int32)|Term Count|
|cumOpenSize|string|Cumulative Open Size|
|cumOpenValue|string|Cumulative Open Value|
|cumOpenFee|string|Cumulative Open Fee|
|cumCloseSize|string|Cumulative Close Size|
|cumCloseValue|string|Cumulative Close Value|
|cumCloseFee|string|Cumulative Close Fee|
|cumFundingFee|string|Cumulative Funding Fee|
|cumLiquidateFee|string|Cumulative Liquidation Fee|
|createdTime|string(int64)|Creation Time|
|updatedTime|string(int64)|Update Time|
|currentLeverage|string|Leverage at Close|


<a id="positiontransaction"></a>
### positiontransaction

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[[PositionTransaction](#schemapositiontransaction)]|Response Data|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemapositiontransaction"></a>
### schemapositiontransaction

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Unique Identifier|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral Coin ID|
|contractId|string(int64)|Contract ID|
|type|string|Detail Type|
|deltaOpenSize|string|Change in Open Size|
|deltaOpenValue|string|Change in Open Value|
|deltaOpenFee|string|Change in Open Fee|
|deltaFundingFee|string|Change in Funding Fee|
|beforeOpenSize|string|Open Size Before Change|
|beforeOpenValue|string|Open Value Before Change|
|beforeOpenFee|string|Open Fee Before Change|
|beforeFundingFee|string|Funding Fee Before Change|
|fillCloseSize|string|Transaction Close Size|
|fillCloseValue|string|Transaction Close Value|
|fillCloseFee|string|Transaction Close Fee|
|fillOpenSize|string|Transaction Open Size|
|fillOpenValue|string|Transaction Open Value|
|fillOpenFee|string|Transaction Open Fee|
|fillPrice|string|Transaction Price|
|liquidateFee|string|Liquidation Fee|
|realizePnl|string|Realized Profit and Loss|
|isLiquidate|boolean|Is Liquidation|
|isDeleverage|boolean|Is Auto-Deleveraging|
|fundingTime|string(int64)|Funding Settlement Time|
|fundingRate|string|Funding Rate|
|fundingIndexPrice|string|Funding Index Price|
|fundingOraclePrice|string|Funding Oracle Price|
|fundingPositionSize|string|Position Size During Funding Settlement|
|orderId|string(int64)|Order ID|
|orderFillTransactionId|string(int64)|Order Fill Transaction ID|
|collateralTransactionId|string(int64)|Collateral Transaction ID|
|forceTradeId|string|Force Trade ID|
|extraType|string|Extra Type|
|extraDataJson|string|Extra Data|
|censorStatus|string|Current Censoring Status|
|censorTxId|string(int64)|Censoring Processing Sequence Number|
|censorTime|string(int64)|Censoring Processing Time|
|censorFailCode|string|Censoring Failure Code|
|censorFailReason|string|Censoring Failure Reason|
|l2TxId|string(int64)|L2 Push Transaction ID|
|l2RejectTime|string(int64)|L2 Rejection Time|
|l2RejectCode|string|L2 Rejection Error Code|
|l2RejectReason|string|L2 Rejection Reason|
|l2ApprovedTime|string(int64)|L2 Batch Verification Time|
|createdTime|string(int64)|Creation Time|
|updatedTime|string(int64)|Update Time|

#### Enumerated Values

| Property     | Value                         |
| ------------ | ----------------------------- |
| type         | UNKNOWN_POSITION_TRANSACTION_TYPE     |
| type         | BUY_POSITION                |
| type         | SELL_POSITION                |
| type         | SETTLE_FUNDING_FEE            |
| type         | BUG_FIX_POSITION_TRANSACTION_TYPE      |
| type         | UNRECOGNIZED                 |
| censorStatus | UNKNOWN_TRANSACTION_STATUS     |
| censorStatus | INIT                         |
| censorStatus | CENSOR_SUCCESS                 |
| censorStatus | CENSOR_FAILURE               |
| censorStatus | L2_APPROVED                    |
| censorStatus | L2_REJECT                      |
| censorStatus | L2_REJECT_APPROVED            |
| censorStatus | UNRECOGNIZED                  |


<a id="positiontransactionpage"></a>
### positiontransactionpage

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[PageDataPositionTransaction](#schemapagedatapositiontransaction)|Generic Paginated Response|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|


<a id="schemapagedatapositiontransaction"></a>
### schemapagedatapositiontransaction

|Name|Type|Description|
|---|---|---|
|dataList|[[PositionTransaction](#schemapositiontransaction)]|Data List|
|nextPageOffsetData|string|Next Page Offset|



<a id="opIdupdateLeverageSetting"></a>

## POST Update Leverage Setting

POST /api/v2/private/account/updateLeverageSetting

Update leverage setting for trading account.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|contractId|string(int64)|Yes|Contract ID. If 0, updates the account's default leverage setting|
|leverage|string|Yes|Leverage multiplier (e.g., "10", "20", "50")|

> Request Example

```json
{
  "accountId": "543429922991899150",
  "contractId": "10000001",
  "leverage": "50"
}
```

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": null,
  "msg": null,
  "errorParam": null,
  "requestTime": "1734661416266",
  "responseTime": "1734661416277",
  "traceId": "a87a52a4e189045b7b7b9948ea7b5c54"
}
```

### Response

| Status Code | Status Code Description | Description | Data Model |
| ----------- | ----------------------- | ----------- | ---------- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [Result](#resultvoid) |

**Notes:**
- This is a new endpoint added in V2
- Leverage setting affects margin requirements and liquidation risk
- Contract-specific leverage overrides account default leverage
- Setting contractId=0 updates the default leverage for all contracts

<a id="opIdgetPositionOrders"></a>

## GET Get Position Orders

GET /api/v2/private/account/getPositionOrders

Get historical orders associated with a specific position term.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|contractId|string(int64)|Yes|Contract ID|
|termCount|integer|Yes|Position term count|
|page|integer|No|Page number, must be ≥1. Default: 1|
|pageSize|integer|No|Items per page, range: 1-100. Default: 20|

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": {
    "page": 1,
    "pageSize": 20,
    "total": 5,
    "orderList": [
      {
        "id": "564809510842007822",
        "userId": "543429922866069763",
        "accountId": "543429922991899150",
        "coinId": "1000",
        "contractId": "10000001",
        "type": "LIMIT",
        "side": "BUY",
        "price": "96813.2",
        "size": "0.001",
        "unfillSize": "0.000",
        "executedSize": "0.001",
        "executedValue": "96.8132",
        "executedFee": "0.048406",
        "executedAvgPrice": "96813.2",
        "status": "FILLED",
        "timeInForce": "GTC",
        "expireTime": "1734747481049",
        "clientOrderId": "client123",
        "reduceOnly": false,
        "postOnly": false,
        "hidden": false,
        "createdTime": "1734661081049",
        "updatedTime": "1734661081053"
      }
    ]
  },
  "msg": null,
  "errorParam": null,
  "requestTime": "1734661416266",
  "responseTime": "1734661416277",
  "traceId": "a87a52a4e189045b7b7b9948ea7b5c54"
}
```

### Response

| Status Code | Status Code Description | Description | Data Model |
| ----------- | ----------------------- | ----------- | ---------- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [Result](#resultpositionorderpagemodel) |

<a id="resultpositionorderpagemodel"></a>
### resultpositionorderpagemodel

|Name|Type|Description|
|---|---|---|
|code|string|Status Code|
|data|[PositionOrderPageModel](#schemapositionorderpagemodel)|Position Order Page Data|
|errorParam|object|Error Parameters|
|requestTime|string(timestamp)|Server Request Time|
|responseTime|string(timestamp)|Server Response Time|
|traceId|string|Trace ID|

<a id="schemapositionorderpagemodel"></a>
### schemapositionorderpagemodel

|Name|Type|Description|
|---|---|---|
|page|integer|Current Page|
|pageSize|integer|Page Size|
|total|integer|Total Count|
|orderList|[[Order](#schemaorder)]|Order List|

**Notes:**
- This is a new endpoint added in V2
- Returns all orders that affected the specified position term
- Useful for tracking which orders opened, increased, or closed a position
- Term count can be obtained from the getPositionTermPage endpoint
