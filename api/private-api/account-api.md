# AccountPrivateApi

<a id="opIdgetPositionTransactionPage"></a>

## GET Get Position Transaction Page

GET /api/v1/private/account/getPositionTransactionPage

### Request Parameters

| Name                            | Location | Type   | Required | Description                                                                                                |
| ------------------------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------- |
| accountId                       | query    | string | No       | Account ID                                                                                                 |
| size                            | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100                             |
| offsetData                      | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                                     |
| filterCoinIdList                | query    | string | No       | Filter position transaction records by specified coin IDs. If not provided, all collateral transaction records are retrieved |
| filterContractIdList            | query    | string | No       | Filter position transaction records by specified contract IDs. If not provided, all position transaction records are retrieved|
| filterTypeList                  | query    | string | No       | Filter position transaction records by specified types. If not provided, all position transaction records are retrieved|
| filterStartCreatedTimeInclusive | query    | string | No       | Filter position transaction records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndCreatedTimeExclusive   | query    | string | No       | Filter position transaction records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time    |
| filterCloseOnly                 | query    | string | No       | Whether to return only position transactions that include closing positions.  `true`: only return records with closing; `false`: return all records|
| filterOpenOnly                  | query    | string | No       | Whether to return only position transactions that include opening positions. `true`: only return records with opening; `false`: return all records|

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

GET /api/v1/private/account/getPositionTransactionById

### Request Parameters

| Name                      | Location | Type   | Required | Description         |
| ------------------------- | -------- | ------ | -------- | ------------------- |
| accountId                 | query    | string | No       | Account ID          |
| positionTransactionIdList | query    | string | No       | Position Transaction IDs |

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

GET /api/v1/private/account/getPositionTermPage

### Request Parameters

| Name                            | Location | Type   | Required | Description                                                                                               |
| ------------------------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| accountId                       | query    | string | No       | Account ID                                                                                                |
| size                            | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100                             |
| offsetData                      | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                                     |
| filterCoinIdList                | query    | string | No       | Filter position term records by specified coin IDs. If not provided, all position term records are retrieved |
| filterContractIdList            | query    | string | No       | Filter position term records by specified contract IDs. If not provided, all position term records are retrieved |
| filterIsLongPosition            | query    | string | No       | Filter position term records by position direction. If not provided, all position term records are retrieved |
| filterStartCreatedTimeInclusive | query    | string | No       | Filter position term records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndCreatedTimeExclusive   | query    | string | No       | Filter position term records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time    |

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

GET /api/v1/private/account/getPositionByContractId

### Request Parameters

| Name           | Location | Type   | Required | Description        |
| -------------- | -------- | ------ | -------- | ------------------ |
| accountId      | query    | string | No       | Account ID         |
| contractIdList | query    | string | No       | Specified contract IDs |

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

GET /api/v1/private/account/getCollateralTransactionPage

### Request Parameters

| Name                            | Location | Type   | Required | Description                                                                                             |
| ------------------------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| accountId                       | query    | string | No       | Account ID                                                                                                |
| size                            | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100                             |
| offsetData                      | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                                     |
| filterCoinIdList                | query    | string | No       | Filter collateral transaction records by specified coin IDs. If not provided, all collateral transaction records are retrieved |
| filterTypeList                  | query    | string | No       | Filter collateral transaction records by specified transaction types. If not provided, all collateral transaction records are retrieved |
| filterStartCreatedTimeInclusive | query    | string | No       | Filter collateral transaction records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndCreatedTimeExclusive   | query    | string | No       | Filter collateral transaction records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time    |

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

GET /api/v1/private/account/getCollateralTransactionById

### Request Parameters

| Name                        | Location | Type   | Required | Description         |
| --------------------------- | -------- | ------ | -------- | ------------------- |
| accountId                   | query    | string | No       | Account ID          |
| collateralTransactionIdList | query    | string | No       | Collateral Transaction IDs |

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

GET /api/v1/private/account/getCollateralByCoinId

### Request Parameters

| Name        | Location | Type   | Required | Description                                                                                      |
| ----------- | -------- | ------ | -------- | ------------------------------------------------------------------------------------------------ |
| accountId   | query    | string | No       | Account ID                                                                                       |
| coinIdList  | query    | string | No       | Filter collateral information by specified coin IDs. If not provided, all collateral information is retrieved |

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

GET /api/v1/private/account/getAccountPage

### Request Parameters

| Name       | Location | Type   | Required | Description                                                                                   |
| ---------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------- |
| size       | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100                 |
| offsetData | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                       |

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

GET /api/v1/private/account/getAccountDeleverageLight

### Request Parameters

| Name      | Location | Type   | Required | Description |
| --------- | -------- | ------ | -------- | ----------- |
| accountId | query    | string | No       | Account ID  |

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

GET /api/v1/private/account/getAccountById

### Request Parameters

| Name      | Location | Type   | Required | Description |
| --------- | -------- | ------ | -------- | ----------- |
| accountId | query    | string | No       | Account ID  |

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

GET /api/v1/private/account/getAccountAsset

### Request Parameters

| Name      | Location | Type   | Required | Description |
| --------- | -------- | ------ | -------- | ----------- |
| accountId | query    | string | No       | Account ID  |

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

GET /api/v1/private/account/getAccountAssetSnapshotPage

### Request Parameters

| Name                       | Location | Type   | Required | Description                                                                                                   |
| -------------------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------
| accountId                | query    | string | No       | Account ID                                                                                                    |
| size                     | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 1000                             |
| offsetData               | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                                         |
| coinId                   | query    | string | Yes      | Filter by the specified coin ID.                                                                               |
| filterTimeTag            | query    | string | No       | Specifies time tag. If not provided or 0, returns snapshots by the hour. 1 returns snapshots by the day      |
| filterStartTimeInclusive | query    | string | No       | Filter snapshots created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndTimeExclusive   | query    | string | No       | Filter snapshots created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time  |

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

<a id="opIdregisterAccount"></a>

## POST Register Account(Create Sub-Account)

POST /api/v1/private/account/registerAccount

> Body Request Parameters

```json
{
    "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
    "l2KeyYCoordinate": "0x6ea3dd81a7fc864893c8c6f674e4a4510c369f939bdc0259a0980dfde882c2d",
    "clientAccountId": "Sub-Account-1"
}
```

### Request Parameters

| Name                | Location | Type   | Required | Description                                                                                    |
| ------------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| body                | body     | object | Yes       | none                                                                                           |
| » l2Key             | body     | string | Yes       | L2 account key, globally unique. Corresponds to starkKey in starkEx (bigint as hex string)    |
| » l2KeyYCoordinate  | body     | string | Yes       | Only used to verify if l2Signature is valid. Not returned to client users (bigint as hex string) |
| » clientAccountId   | body     | string | Yes       | Client account ID for idempotency verification                                                 |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "accountId": "543429922991899150"
    },
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#registeraccount) |

# Data Models


<a id="accountassetsnapshot"></a>
### accountassetsnapshot

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [PageDataAccountAssetSnapshot](#schemapagedataaccountassetsnapshot) | false    | none        | Generic Paginated Response |                                                                          |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| » **additionalProperties** | string                                | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="schemapagedataaccountassetsnapshot"></a>
### schemapagedataaccountassetsnapshot

| Name               | Type                               | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ---------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [[AccountAssetSnapshot](#schemaaccountassetsnapshot)] | false    | none        | Data List                  |                                                                     |
| nextPageOffsetData | string                             | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="schemaaccountassetsnapshot"></a>
### schemaaccountassetsnapshot

| Name             | Type            | Required | Constraints | Description                         | Notes                                                                     |
| ---------------- | --------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| userId           | string(int64)   | false    | none        | User ID                             | ID of the owning user                                                  |
| accountId        | string(int64)   | false    | none        | Account ID                          | ID of the owning account                                                 |
| coinId           | string(int64)   | false    | none        | Collateral Coin ID                 | ID of the associated collateral coin                                        |
| timeTag          | integer(int32)  | false    | none        | Time Tag                            | Time tag. 1 represents the snapshot time is for the whole day.           |
| snapshotTime     | string(int64)   | false    | none        | Snapshot Time                       | Snapshot time, hourly timestamp at the top of the hour.                  |
| totalEquity      | string          | false    | none        | Total Collateral Value              | Current total value of the collateral                                      |
| termRealizePnl  | string          | false    | none        | Term Realized PnL                   | Realized PnL for the term                                               |
| unrealizePnl     | string          | false    | none        | Unrealized PnL                      | Unrealized PnL                                                            |
| totalRealizePnl  | string          | false    | none        | Total Realized PnL                 | Total realized PnL of the position                                          |

<a id="schemaresultgetaccountasset"></a>
### schemaresultgetaccountasset

| Name           | Type                            | Required | Constraints | Description              | Notes                                                                  |
| -------------- | ------------------------------- | -------- | ----------- | ------------------------ | ---------------------------------------------------------------------- |
| code           | string                          | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [GetAccountAsset](#schemagetaccountasset) | false    | none        | Get Account Asset Response | Response structure for fetching account asset data.                       |
| errorParam     | object                          | false    | none        | Error Parameters         | Error message parameter information                                     |
| requestTime    | string(timestamp)               | false    | none        | Server Request Time     | Time at which the server received the request                           |
| responseTime   | string(timestamp)               | false    | none        | Server Response Time    | Time at which the server sent the response                              |
| traceId        | string                          | false    | none        | Trace ID                | Invocation trace ID                                                  |


<a id="schemagetaccountasset"></a>
### schemagetaccountasset

| Name                  | Type                                | Required | Constraints | Description                                                                                      | Notes                                                                                          |
| --------------------- | ----------------------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| account               | [Account](#schemaaccount)           | false    | none        | Account Information                                                                              | Account information data.                                                                  |
| collateralList        | [[Collateral](#schemacollateral)]   | false    | none        | Collateral Information List                                                                   | List of collateral information data.                                                           |
| positionList          | [[Position](#schemaposition)]       | false    | none        | Perpetual Contract Position List                                                                 | List of perpetual contract position information.                                               |
| version               | string(int64)                       | false    | none        | Account Version                                                                                | Account version number, incremented with each update.                                        |
| positionAssetList     | [[PositionAsset](#schemapositionasset)] | false    | none        | Position Asset Information List                                                                  | List of position asset information.                                                              |
| collateralAssetModelList | [[CollateralAsset](#schemacollateralasset)] | false    | none        | Account-Level Asset Information List                                                               | List of account-level asset information.                                                          |
| oraclePriceList       | [[IndexPrice](#schemaindexprice)]   | false    | none        | Oracle Price List                                                  | List of all oracle prices used to calculate assets (only those used).                                                          |

<a id="schemaindexprice"></a>
### schemaindexprice

| Name                  | Type                  | Required | Constraints | Description                        | Notes                                                                            |
| --------------------- | --------------------- | -------- | ----------- | ---------------------------------- | -------------------------------------------------------------------------------- |
| contractId            | string(int64)         | false    | none        | Contract ID                        | Contract ID                                                                      |
| priceType             | string                | false    | none        | Price Type                         |                                                                                  |
| priceValue            | string                | false    | none        | Price Value                        | Price value                                                                      |
| createdTime           | string(int64)         | false    | none        | Creation Time                      | Time of creation                                                                 |
| oraclePriceSignature  | [[OraclePriceSignature](#schemaoraclepricesignature)] | false    | none        | Oracle Price Signature Information | Oracle price signature information, only exists when price_type=ORACLE_PRICE. |

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

| Name             | Type                    | Required | Constraints | Description                                                              | Notes                                                                                                    |
| ---------------- | ----------------------- | -------- | ----------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| contractId       | string(int64)           | false    | none        | Contract ID                                                              | Contract ID                                                                                            |
| signer           | string                  | false    | none        | Signer ID                                                                | Signer identifier                                                                                        |
| price            | string                  | false    | none        | Signed Price                                                             | The price signed (price after stark ex precision processing)                                                    |
| externalAssetId  | string                  | false    | none        | Concatenated Asset and Oracle Names        | Concatenation of the asset name and the oracle name (both in hex encoding).                                               |
| signature        | [L2Signature](#schemal2signature) | false    | none        | L2 Signature Information                                            | L2 signature information                                                                               |
| timestamp        | string(int64)           | false    | none        | Signature Creation Time                                                | The time the signature was created.                                                                     |

<a id="schemal2signature"></a>
### schemal2signature

| Name | Type   | Required | Constraints | Description      | Notes                  |
| ---- | ------ | -------- | ----------- | ---------------- | ---------------------- |
| r    | string | false    | none        | R Value          | Bigint for hex string |
| s    | string | false    | none        | S Value          | Bigint for hex string |
| v    | string | false    | none        | V Value          | Bigint for hex string |

<a id="schemacollateralasset"></a>
### schemacollateralasset

| Name                    | Type          | Required | Constraints | Description                                                                      | Notes                                                                              |
| ----------------------- | ------------- | -------- | ----------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| userId                  | string(int64) | false    | none        | User ID                                                                          | ID of the owning user.                                                               |
| accountId               | string(int64) | false    | none        | Account ID                                                                       | ID of the owning account.                                                              |
| coinId                  | string(int64) | false    | none        | Collateral Coin ID                                                               | ID of the associated collateral coin.                                                  |
| totalEquity             | string        | false    | none        | Total Collateral Value                                                           | Current total value of the collateral.                                                |
| totalPositionValueAbs    | string        | false    | none        | Sum of Absolute Position Values                                                     | Sum of the absolute position values for the current collateral                        |
| initialMarginRequirement| string        | false    | none        | Initial Margin Requirement                                                     | The initial margin requirement for the current collateral.                              |
| starkExRiskValue        | string        | false    | none        | Total StarkEx Risk Value                                                           | The total starkEx risk amount for the current collateral.                             |
| pendingWithdrawAmount  | string        | false    | none        | Pending Withdrawal Amount                                                      | The amount of collateral pending withdrawal.                                          |
| pendingTransferOutAmount| string        | false    | none        | Pending Transfer Out Amount                                                     | The amount of collateral pending transfer out.                                       |
| orderFrozenAmount       | string        | false    | none        | Order Frozen Amount                                                             | The amount of collateral frozen by orders.                                            |
| availableAmount         | string        | false    | none        | Available Amount                                                                | The amount of collateral available for use.                                           |

<a id="schemapositionasset"></a>
### schemapositionasset

| Name                    | Type          | Required | Constraints | Description                            | Notes                                                                          |
| ----------------------- | ------------- | -------- | ----------- | -------------------------------------- | ------------------------------------------------------------------------------ |
| userId                  | string(int64) | false    | none        | User ID                                | ID of the owning user.                                                         |
| accountId               | string(int64) | false    | none        | Account ID                             | ID of the owning account.                                                        |
| coinId                  | string(int64) | false    | none        | Collateral Coin ID                     | ID of the associated collateral coin.                                             |
| contractId              | string(int64) | false    | none        | Contract ID                             | ID of the associated contract.                                                  |
| positionValue           | string        | false    | none        | Position Value                         | Position value, positive for long positions, negative for short positions.         |
| maxLeverage             | string        | false    | none        | Maximum Leverage                      | The maximum leverage for current contract position.                              |
| initialMarginRequirement| string        | false    | none        | Initial Margin Requirement            | Initial margin requirement for the position.                                   |
| starkExRiskRate         | string        |false    | none        | StarkEx Risk Rate                      | StarkEx risk rate calculated based on risk tiers. Similar to maintenance margin rate with different precision. |
| starkExRiskValue        | string        | false    | none        | StarkEx Risk Value                   | StarkEx risk amount, similar to maintenance margin, with different precision.     |
| avgEntryPrice           | string        | false    | none        | Average Entry Price                   | Average entry price.                                                            |
| liquidatePrice          | string        | false    | none        | Liquidation Price                     | Liquidation price (force liquidation price). If oracle price reaches this price, liquidation is triggered.        |
| bankruptPrice           | string        | false    | none        | Bankruptcy Price                      | Bankruptcy price. If the oracle price reaches this level, account total value is less than 0.     |
| worstClosePrice         | string        | false    | none        | Worst Close Price                     | The worst closing price. The closing transaction price can not be worse than this price. |
| unrealizePnl            | string        | false    | none        | Unrealized PnL                         | Unrealized profit and loss for the position.                                     |
| termRealizePnl          | string        | false    | none        | Term Realized PnL                    | Realized PnL for the term.                                            |
| totalRealizePnl        | string        | false    | none        | Total Realized PnL                   | Total realized PnL of the position.                                         |

<a id="schemaposition"></a>
### schemaposition

| Name                | Type          | Required | Constraints | Description                                                         | Notes                                                                                      |
| ------------------- | ------------- | -------- | ----------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| userId              | string(int64) | false    | none        | User ID                                                             | ID of the owning user.                                                                     |
| accountId           | string(int64) | false    | none        | Account ID                                                          | ID of the owning account.                                                                |
| coinId              | string(int64) | false    | none        | Collateral Coin ID                                                  | ID of the associated collateral coin.                                                       |
| contractId          | string(int64) | false    | none        | Contract ID                                                          | ID of the associated contract.                                                              |
| openSize            | string        | false    | none        | Current Open Size                                                   | Current open size (positive for long, negative for short).                                   |
| openValue           | string        | false    | none        | Current Open Value                                                  | Current open value (increases upon opening, proportionally decreases upon closing).        |
| openFee             | string        | false    | none        | Current Open Fee                                                     | Current allocated open fee (increases upon opening, proportionally decreases upon closing). |
| fundingFee          | string        | false    | none        | Current Funding Fee                                                  | Current allocated funding fee (increases upon settlement, proportionally decreases upon closing). |
| longTermCount       | integer(int32) | false    | none        | Long Position Term Count                                            | Long position term count. Starts from 1, increases by one upon complete closure of a position |
| longTermStat        | [PositionStat](#schemapositionstat) | false    | none        | Long Position Cumulative Statistics | Cumulative statistics for the position.                                              |
| longTermCreatedTime | string        | false    | none        | Long Position Term Creation Time                                            | Creation time for the long position term.                                                 |
| longTermUpdatedTime | string        | false    | none        | Long Position Term Update Time                                            | Update time for the long position term.                                                  |
| shortTermCount      | integer(int32) | false    | none        | Short Position Term Count                                           | Short position term count. Starts from 1, increases by one upon complete closure of a position|
| shortTermStat        | [PositionStat](#schemapositionstat) | false    | none        | Short Position Cumulative Statistics| Cumulative statistics for the position.                                              |
| shortTermCreatedTime| string        | false    | none        | Short Position Term Creation Time                                           | Creation time for the short position term.                                                |
| shortTermUpdatedTime| string        | false    | none        | Short Position Term Update Time                                           | Update time for the short position term.                                                |
| longTotalStat       | [PositionStat](#schemapositionstat) | false    | none        | Long Cumulative Statistics                | Cumulative statistics for the position.                                              |
| shortTotalStat      | [PositionStat](#schemapositionstat) | false    | none        | Short Cumulative Statistics                | Cumulative statistics for the position.                                              |
| createdTime         | string(int64) | false    | none        | Creation Time                                                         | Creation time.                                                                             |
| updatedTime         | string(int64) | false    | none        | Update Time                                                        | Update time.                                                                            |


<a id="schemapositionstat"></a>
### schemapositionstat

| Name            | Type   | Required | Constraints | Description              | Notes                                                               |
| --------------- | ------ | -------- | ----------- | ------------------------ | ------------------------------------------------------------------- |
| cumOpenSize     | string | false    | none        | Cumulative Open Size     | Cumulative open size.                                               |
| cumOpenValue    | string | false    | none        | Cumulative Open Value   | Cumulative open value.                                              |
| cumOpenFee      | string | false    | none        | Cumulative Open Fee     | Cumulative open fees.                                               |
| cumCloseSize    | string | false    | none        | Cumulative Close Size   | Cumulative close size.                                              |
| cumCloseValue   | string | false    | none        | Cumulative Close Value  | Cumulative close value.                                             |
| cumCloseFee     | string | false    | none        | Cumulative Close Fee    | Cumulative close fees.                                              |
| cumFundingFee   | string | false    | none        | Cumulative Funding Fee  | Cumulative funding fees settled.                                      |
| cumLiquidateFee | string | false    | none        | Cumulative Liquidate Fee| Cumulative liquidation fees.                                          |

<a id="schemacollateral"></a>
### schemacollateral

| Name                  | Type            | Required | Constraints | Description                                   | Notes                                                                    |
| --------------------- | --------------- | -------- | ----------- | --------------------------------------------- | ------------------------------------------------------------------------ |
| userId                | string(int64)   | false    | none        | User ID                                       | ID of the owning user.                                                  |
| accountId             | string(int64)   | false    | none        | Account ID                                    | ID of the owning account.                                                 |
| coinId                | string(int64)   | false    | none        | Coin ID                                       | Collateral coin ID.                                                       |
| amount                | string(decimal) | false    | none        | Collateral Amount                             | Collateral amount, actually of decimal type.                               |
| legacyAmount          | string(decimal) | false    | none        | Legacy Amount                                 | Legacy balance field, for display purposes only, not for calculations.   |
| cumDepositAmount      | string(decimal) | false    | none        | Cumulative Deposit Amount                     | Cumulative deposit amount.                                               |
| cumWithdrawAmount     | string(decimal) | false    | none        | Cumulative Withdrawal Amount                  | Cumulative withdrawal amount.                                             |
| cumTransferInAmount   | string(decimal) | false    | none        | Cumulative Transfer In Amount                  | Cumulative transfer in amount.                                              |
| cumTransferOutAmount  | string(decimal) | false    | none        | Cumulative Transfer Out Amount                 | Cumulative transfer out amount.                                             |
| cumPositionBuyAmount  | string(decimal) | false    | none        | Cumulative Position Buy Amount                | Cumulative collateral amount deducted from position buy.                     |
| cumPositionSellAmount | string(decimal) | false    | none        | Cumulative Position Sell Amount                | Cumulative collateral amount added from position sell.                    |
| cumFillFeeAmount      | string(decimal) | false    | none        | Cumulative Fill Fee Amount                    | Cumulative transaction fee amount.                                            |
| cumFundingFeeAmount   | string(decimal) | false    | none        | Cumulative Funding Fee Amount                 | Cumulative funding fee amount.                                              |
| cumFillFeeIncomeAmount| string(decimal) | false    | none        | Cumulative Order Fill Fee Income Amount       | Cumulative amount from order fill fee income.                                 |
| createdTime           | string(int64)   | false    | none        | Creation Time                                 | Creation time.                                                               |
| updatedTime           | string(int64)   | false    | none        | Update Time                                    | Update time.                                                                  |

<a id="schemaaccount"></a>
### schemaaccount

| Name                    | Type                | Required | Constraints | Description                                                                                                                                  | Notes                                                                                     |
| ----------------------- | ------------------- | -------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| id                      | string(int64)       | false    | none        | Account ID                                                                                                                                 | Account ID, must be greater than 0.                                                      |
| userId                  | string(int64)       | false    | none        | User ID                                                                                                                                    | ID of the owning user.                                                                    |
| ethAddress              | string              | false    | none        | Wallet ETH Address                                                                                                                          | Wallet ETH address.                                                                      |
| l2Key                   | string              | false    | none        | L2 Account Key                                                                                                                              | Account key on L2. Stark key in starkEx. Bigint for hex string                             |
| l2KeyYCoordinate        | string              | false    | none        | L2 Key Y Coordinate                                                                                                                        | Used only for verifying l2Signature. Not returned to end users. Bigint for hex string.    |
| clientAccountId         | string              | false    | none        | Client Account ID                                                                                                                            | Client account ID for idempotency check.                                                   |
| isSystemAccount         | boolean             | false    | none        | System Account                                                                                                                             | Whether it is a system account (system accounts are not subject to contract risk settings, use separate MQ for trade messages). |
| defaultTradeSetting     | [TradeSetting](#schematradesetting) | false    | none        | Default Trade Setting                                                                                                    | Trade settings. Trade setting calculation priority: Account contract trade settings -> Account default trade settings -> Contract configuration trade settings. Note: Only one of `is_set_fee_rate` and `is_set_fee_discount` can be true.                                     |
| contractIdToTradeSetting| object              | false    | none        | Contract-Level Account Trade Settings                                                                                                    | Account contract-level trade settings.                                                     |
| » **additionalProperties**| [TradeSetting](#schematradesetting) | false    | none        | Contract-Level Account Trade Settings                                                                                                    | Trade settings. Trade setting calculation priority: Account contract trade settings -> Account default trade settings -> Contract configuration trade settings. Note: Only one of `is_set_fee_rate` and `is_set_fee_discount` can be true.                                                     |
| maxLeverageLimit        | string              | false    | none        | Maximum Leverage Limit                                                                                                                       | User-set maximum leverage limit. If 0, uses the leverage limit of the corresponding trading contract.  |
| createOrderPerMinuteLimit| integer(int32)     | false    | none        | Order Creation Limit per Minute                                                                                                        | Order frequency limit per minute. If 0, default limit is used; if < 0, no limit is applied. |
| createOrderDelayMillis  | integer(int32)     | false    | none        | Order Creation Delay Milliseconds                                                                                                        | Order delay milliseconds, must be greater than or equal to 0.                         |
| extraType               | string              | false    | none        | Extra Type                                                                                                                                 | Extra type for upper-layer use.                                                            |
| extraDataJson           | string              | false    | none        | Extra Data                                                                                                                               | Extra data in JSON format, default is an empty string.                                     |
| status                  | string              | false    | none        | Account Status                                                                                                                            | Account status.                                                                           |
| isLiquidating           | boolean             | false    | none        | Is Liquidating                                                                                                                            | Whether is being liquidated.                                                              |
| createdTime             | string(int64)       | false    | none        | Creation Time                                                                                                                              | Creation time.                                                                            |
| updatedTime             | string(int64)       | false    | none        | Update Time                                                                                                                                 | Update time.                                                                               |

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

| Name              | Type            | Required | Constraints | Description                                                                                    | Notes                                                                                          |
| ----------------- | --------------- | -------- | ----------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| isSetFeeRate      | boolean         | false    | none        | Whether Fee Rate is Set                                                                        | Whether to set a specific fee rate value.                                                        |
| takerFeeRate      | string(decimal) | false    | none        | Taker Fee Rate                                                                                 | Taker fee rate, range [0, 1), valid only when is_set_fee_rate=true.                                    |
| makerFeeRate      | string(decimal) | false    | none        | Maker Fee Rate                                                                                 | Maker fee rate, range [0, 1), valid only when is_set_fee_rate=true.                                     |
| isSetFeeDiscount  | boolean         | false    | none        | Whether Fee Discount is Set                                                                  | Whether to set a fee discount.                                                                   |
| takerFeeDiscount  | string(decimal) | false    | none        | Taker Fee Discount                                                                             | Taker fee discount, range [0, 1), valid only when is_set_fee_discount=true.                      |
| makerFeeDiscount  | string(decimal) | false    | none        | Maker Fee Discount                                                                             | Maker fee discount, range [0, 1), valid only when is_set_fee_discount=true.                       |
| isSetMaxLeverage  | boolean         | false    | none        | Whether Maximum Leverage is Set                                                                  | Whether to set maximum trading leverage.                                                      |
| maxLeverage       | string(decimal) | false    | none        | Maximum Leverage                                                                             | Maximum trading leverage.                                                                          |


<a id="schemaresultaccount"></a>
### schemaresultaccount

| Name           | Type                                 | Required | Constraints | Description                | Notes                                                                    |
| -------------- | ------------------------------------ | -------- | ----------- | -------------------------- | ------------------------------------------------------------------------ |
| code           | string                               | false    | none        | Status Code                | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [Account](#schemaaccount)            | false    | none        | Account Information      | Account information data.                                                 |
| errorParam     | object                               | false    | none        | Error Parameters           | Error message parameter information                                       |
| requestTime    | string(timestamp)                    | false    | none        | Server Request Time       | Time at which the server received the request                             |
| responseTime   | string(timestamp)                    | false    | none        | Server Response Time      | Time at which the server sent the response                                |
| traceId        | string                               | false    | none        | Trace ID                  | Invocation trace ID                                                     |


<a id="getaccountdeleveragelight"></a>
### getaccountdeleveragelight

| Name           | Type                                   | Required | Constraints | Description                        | Notes                                                                  |
| -------------- | -------------------------------------- | -------- | ----------- | ---------------------------------- | ---------------------------------------------------------------------- |
| code           | string                                 | false    | none        | Status Code                        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [GetAccountDeleverageLight](#schemagetaccountdeleveragelight) | false    | none        | Get Account Deleverage Light Response  | Response structure for fetching deleverage light information.         |
| errorParam     | object                                 | false    | none        | Error Parameters                   | Error message parameter information                                     |
| requestTime    | string(timestamp)                      | false    | none        | Server Request Time               | Time at which the server received the request                           |
| responseTime   | string(timestamp)                      | false    | none        | Server Response Time              | Time at which the server sent the response                              |
| traceId        | string                                 | false    | none        | Trace ID                          | Invocation trace ID                                                  |

<a id="schemagetaccountdeleveragelight"></a>
### Response structure for fetching deleverage light information.

| Name                         | Type   | Required | Constraints | Description                                                                          | Notes                                                                         |
| ---------------------------- | ------ | -------- | ----------- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| positionContractIdToLightNumberMap | object | false    | none        | Map from Position Contract ID to Light Number                                   | Maps position contract ID to light number. `light_number` ranges from 1-5, which represent 1-5 lights. |


<a id="account"></a>
### account

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [PageDataAccount](#schemapagedataaccount)   | false    | none        | Generic Paginated Response | Generic paginated response.                                                                     |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                  |


<a id="schemapagedataaccount"></a>
### Generic Paginated Response

| Name               | Type                     | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ------------------------ | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [[Account](#schemaaccount)]       | false    | none        | Data List                  | List of account data.                                                               |
| nextPageOffsetData | string                   | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="collateral"></a>
### collateral

| Name           | Type                      | Required | Constraints | Description        | Notes                                                                  |
| -------------- | ------------------------- | -------- | ----------- | ------------------ | ---------------------------------------------------------------------- |
| code           | string                    | false    | none        | Status Code        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [[Collateral](#schemacollateral)] | false    | none        | Response Data     | Correct response data.                                                  |
| errorParam     | object                    | false    | none        | Error Parameters   | Error message parameter information                                     |
| requestTime    | string(timestamp)         | false    | none        | Server Request Time | Time at which the server received the request                           |
| responseTime   | string(timestamp)         | false    | none        | Server Response Time| Time at which the server sent the response                              |
| traceId        | string                    | false    | none        | Trace ID           | Invocation trace ID                                                  |


<a id="collateraltransaction"></a>
### collateraltransaction

| Name           | Type                            | Required | Constraints | Description        | Notes                                                                  |
| -------------- | ------------------------------- | -------- | ----------- | ------------------ | ---------------------------------------------------------------------- |
| code           | string                          | false    | none        | Status Code        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [[CollateralTransaction](#schemacollateraltransaction)] | false    | none        | Response Data     | Correct response data.                                                  |
| errorParam     | object                          | false    | none        | Error Parameters   | Error message parameter information                                     |
| requestTime    | string(timestamp)               | false    | none        | Server Request Time | Time at which the server received the request                           |
| responseTime   | string(timestamp)               | false    | none        | Server Response Time| Time at which the server sent the response                              |
| traceId        | string                          | false    | none        | Trace ID           | Invocation trace ID                                                  |


<a id="schemacollateraltransaction"></a>
### Collateral transaction details

| Name                    | Type            | Required | Constraints | Description                                                                                    | Notes                                                                                             |
| ----------------------- | --------------- | -------- | ----------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| id                      | string(int64)   | false    | none        | Unique Identifier                                                                              | Unique identifier.                                                                                |
| userId                  | string(int64)   | false    | none        | User ID                                                                                        | ID of the owning user.                                                                           |
| accountId               | string(int64)   | false    | none        | Account ID                                                                                     | ID of the owning account.                                                                       |
| coinId                  | string(int64)   | false    | none        | Coin ID                                                                                        | Collateral coin ID.                                                                             |
| type                    | string          | false    | none        | Detail Type                                                                                    | Detail type.                                                                                      |
| deltaAmount             | string(decimal) | false    | none        | Collateral Change Amount                                                                       | Amount of the collateral change.                                                                |
| deltaLegacyAmount       | string(decimal) | false    | none        | Legacy Balance Change Amount                                                                   | Change amount of the legacy balance field.                                                    |
| beforeAmount            | string(decimal) | false    | none        | Collateral Amount Before Change                                                                | Collateral amount before the change.                                                             |
| beforeLegacyAmount      | string(decimal) | false    | none        | Legacy Balance Amount Before Change                                                            | Legacy balance before the change.                                                                  |
| fillCloseSize           | string(decimal) | false    | none        | Transaction Close Size                                                                       | Transaction close size (positive for buy, negative for sell).                                          |
| fillCloseValue          | string          | false    | none        | Transaction Close Value                                                                       | Transaction close value (positive for buy, negative for sell).                                     |
| fillCloseFee            | string          | false    | none        | Transaction Close Fee                                                                          | Transaction close fee (typically zero or negative).                                             |
| fillOpenSize            | string(decimal) | false    | none        | Transaction Open Size                                                                        | Transaction open size (positive for buy, negative for sell).                                          |
| fillOpenValue           | string          | false    | none        | Transaction Open Value                                                                        | Transaction open value (positive for buy, negative for sell).                                     |
| fillOpenFee             | string          | false    | none        | Transaction Open Fee                                                                           | Transaction open fee (typically zero or negative).                                             |
| fillPrice               | string(decimal) | false    | none        | Transaction Price                                                                              | Transaction price (not precise, for display).                                              |
| liquidateFee            | string(decimal) | false    | none        | Liquidation Fee                                                                                | Liquidation fee (if close transaction is a liquidation, typically zero or negative).            |
| realizePnl              | string(decimal) | false    | none        | Realized Profit and Loss                                                                        | Realized profit and loss from a close (if a close transaction. Not precise, for display). |
| isLiquidate             | boolean         | false    | none        | Is Liquidation                                                                                   | Whether the transaction is a liquidation.                                                      |
| isDeleverage            | boolean         | false    | none        | Is Auto-Deleveraging                                                                         | Whether the transaction is from auto-deleveraging.                                            |
| fundingTime             | string(int64)   | false    | none        | Funding Settlement Time                                                                      | Funding settlement time.                                                                        |
| fundingRate             | string(decimal) | false    | none        | Funding Rate                                                                                    | Funding rate.                                                                                    |
| fundingIndexPrice       | string(decimal) | false    | none        | Funding Index Price                                                                           | Index price related to funding rate.                                                         |
| fundingOraclePrice      | string(decimal) | false    | none        | Funding Oracle Price                                                                          | Oracle price related to funding rate.                                                        |
| fundingPositionSize     | string(decimal) | false    | none        | Position Size During Funding Settlement                                                      | Position size during funding settlement (positive for long, negative for short).             |
| depositId               | string(int64)   | false    | none        | Deposit Order ID                                                                               | Associated deposit order ID when type=DEPOSIT.                                                 |
| withdrawId              | string(int64)   | false    | none        | Withdrawal Order ID                                                                            | Associated withdrawal order ID when type=WITHDRAW.                                              |
| transferInId            | string(int64)   | false    | none        | Transfer In Order ID                                                                           | Associated transfer-in order ID when type=TRANSFER_IN.                                           |
| transferOutId           | string(int64)   | false    | none        | Transfer Out Order ID                                                                          | Associated transfer-out order ID when type=TRANSFER_OUT.                                          |
| transferReason          | string          | false    | none        | Transfer Reason                                                                               | Transfer reason when type=TRANSFER_IN/TRANSFER_OUT.                                         |
| orderId                 | string(int64)   | false    | none        | Order ID                                                                                       | Associated order ID when type=POSITION_BUY/POSITION_SELL/FILL_FEE_INCOME.                      |
| orderFillTransactionId  | string(int64)   | false    | none        | Order Fill Transaction ID                                                                    | Associated order fill transaction ID when type=POSITION_BUY/POSITION_SELL/FILL_FEE_INCOME.   |
| orderAccountId          | string(int64)   | false    | none        | Order Account ID                                                                             | Associated order account ID when type=FILL_FEE_INCOME.                                         |
| positionContractId      | string(int64)   | false    | none        | Position Contract ID                                                                           | Associated position contract ID when type=POSITION_BUY/POSITION_SELL/POSITION_FUNDING/FILL_FEE_INCOME. |
| positionTransactionId  | string(int64)   | false    | none        | Position Transaction ID                                                                    | Associated position transaction ID when type=POSITION_BUY/POSITION_SELL/POSITION_FUNDING.     |
| forceWithdrawId         | string          | false    | none        | Force Withdrawal Order ID                                                                      | Associated force withdrawal order ID when type=WITHDRAW.                                    |
| forceTradeId            | string          | false    | none        | Force Trade ID                                                                                 | Associated force trade order ID when type=POSITION_BUY/POSITION_SELL.                           |
| extraType               | string          | false    | none        | Extra Type                                                                                     | Extra type for upper-layer business use.                                                     |
| extraDataJson           | string          | false    | none        | Extra Data                                                                                    | Extra data in JSON format, default is empty string.                                          |
| censorStatus            | string          | false    | none        | Current Censoring Status                                                                       | Current censoring status.                                                                     |
| censorTxId              | string(int64)   | false    | none        | Censoring Processing Sequence Number                                                       | Censoring processing sequence number, exists when censor_status=CENSOR_SUCCESS/CENSOR_FAILURE/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED.   |
| censorTime              | string(int64)   | false    | none        | Censoring Processing Time                                                                    | Censoring processing time, exists when censor_status=CENSOR_SUCCESS/CENSOR_FAILURE/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED.   |
| censorFailCode          | string          | false    | none        | Censoring Failure Code                                                                         | Censoring failure code, exists when censor_status=CENSOR_FAILURE.                           |
| censorFailReason        | string          | false    | none        | Censoring Failure Reason                                                                       | Censoring failure reason, exists when censor_status=CENSOR_FAILURE.                         |
| l2TxId                  | string(int64)   | false    | none        | L2 Push Transaction ID                                                                          | L2 push transaction ID, exists when censor_status=CENSOR_SUCCESS/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED.    |
| l2RejectTime            | string(int64)   | false    | none        | L2 Rejection Time                                                                             | L2 rejection time, exists when censor_status=L2_REJECT/L2_REJECT_APPROVED.                 |
| l2RejectCode            | string          | false    | none        | L2 Rejection Error Code                                                                        | L2 rejection error code, exists when censor_status=L2_REJECT/L2_REJECT_APPROVED.             |
| l2RejectReason          | string          | false    | none        | L2 Rejection Reason                                                                          | L2 rejection reason, exists when censor_status=L2_REJECT/L2_REJECT_APPROVED.             |
| l2ApprovedTime          | string(int64)   | false    | none        | L2 Batch Verification Time                                                                   | L2 batch verification time, exists when censor_status=L2_APPROVED/L2_REJECT_APPROVED.         |
| createdTime             | string(int64)   | false    | none        | Creation Time                                                                                  | Creation time.                                                                              |
| updatedTime             | string(int64)   | false    | none        | Update Time                                                                                   | Update time.                                                                                 |

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

| Name           | Type                                     | Required | Constraints | Description                        | Notes                                                                  |
| -------------- | ---------------------------------------- | -------- | ----------- | ---------------------------------- | ---------------------------------------------------------------------- |
| code           | string                                   | false    | none        | Status Code                        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [PageDataCollateralTransaction](#schemapagedatacollateraltransaction) | false    | none        | Generic Paginated Response | Generic paginated response.                                                              |
| errorParam     | object                                   | false    | none        | Error Parameters                   | Error message parameter information                                     |
| requestTime    | string(timestamp)                        | false    | none        | Server Request Time               | Time at which the server received the request                           |
| responseTime   | string(timestamp)                        | false    | none        | Server Response Time              | Time at which the server sent the response                              |
| traceId        | string                                   | false    | none        | Trace ID                          | Invocation trace ID                                                  |


<a id="schemapagedatacollateraltransaction"></a>
### schemapagedatacollateraltransaction

| Name               | Type                             | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | -------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [[CollateralTransaction](#schemacollateraltransaction)] | false    | none        | Data List                  | List of collateral transaction data.                                               |
| nextPageOffsetData | string                           | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |


### position

| Name           | Type                   | Required | Constraints | Description        | Notes                                                                  |
| -------------- | ---------------------- | -------- | ----------- | ------------------ | ---------------------------------------------------------------------- |
| code           | string                 | false    | none        | Status Code        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [[Position](#schemaposition)] | false    | none        | Response Data     | Correct response data.                                                  |
| errorParam     | object                 | false    | none        | Error Parameters   | Error message parameter information                                     |
| requestTime    | string(timestamp)      | false    | none        | Server Request Time | Time at which the server received the request                           |
| responseTime   | string(timestamp)      | false    | none        | Server Response Time| Time at which the server sent the response                              |
| traceId        | string                 | false    | none        | Trace ID           | Invocation trace ID                                                  |


<a id="positionterm"></a>
### positionterm

| Name           | Type                                   | Required | Constraints | Description                | Notes                                                                    |
| -------------- | -------------------------------------- | -------- | ----------- | -------------------------- | ------------------------------------------------------------------------ |
| code           | string                                 | false    | none        | Status Code                | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [PageDataPositionTerm](#schemapagedatapositionterm)  | false    | none        | Generic Paginated Response | Generic paginated response.                                                              |
| errorParam     | object                                 | false    | none        | Error Parameters           | Error message parameter information                                       |
| requestTime    | string(timestamp)                      | false    | none        | Server Request Time       | Time at which the server received the request                           |
| responseTime   | string(timestamp)                      | false    | none        | Server Response Time      | Time at which the server sent the response                              |
| traceId        | string                                 | false    | none        | Trace ID                  | Invocation trace ID                                                     |


<a id="schemapagedatapositionterm"></a>
### schemapagedatapositionterm

| Name               | Type                           | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ------------------------------ | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [[PositionTerm](#schemapositionterm)] | false    | none        | Data List                  | List of position term data.                                                    |
| nextPageOffsetData | string                         | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |


<a id="schemapositionterm"></a>
### schemapositionterm

| Name            | Type          | Required | Constraints | Description                         | Notes                                                                        |
| --------------- | ------------- | -------- | ----------- | ----------------------------------- | ---------------------------------------------------------------------------- |
| userId          | string        | false    | none        | User ID                             | ID of the owning user.                                                                     |
| accountId       | string        | false    | none        | Account ID                          | ID of the owning account.                                                                   |
| coinId          | string        | false    | none        | Collateral Coin ID                 | ID of the associated collateral coin.                                                 |
| contractId      | string        | false    | none        | Contract ID                         | ID of the associated contract.                                                           |
| termCount       | integer(int32) | false    | none        | Term Count                          | Term count. Starts from 1, increases by one each time a position is fully closed and then re-opened.        |
| cumOpenSize     | string        | false    | none        | Cumulative Open Size                | Cumulative open size.                                                               |
| cumOpenValue    | string        | false    | none        | Cumulative Open Value              | Cumulative open value.                                                              |
| cumOpenFee      | string        | false    | none        | Cumulative Open Fee                | Cumulative open fees.                                                                |
| cumCloseSize    | string        | false    | none        | Cumulative Close Size              | Cumulative close size.                                                              |
| cumCloseValue   | string        | false    | none        | Cumulative Close Value             | Cumulative close value.                                                                |
| cumCloseFee     | string        | false    | none        | Cumulative Close Fee               | Cumulative close fees.                                                               |
| cumFundingFee   | string        | false    | none        | Cumulative Funding Fee            | Cumulative funding fees that have been settled.                                |
| cumLiquidateFee | string        | false    | none        | Cumulative Liquidation Fee          | Cumulative liquidation fees.                                                       |
| createdTime     | string(int64) | false    | none        | Creation Time                      | Creation time.                                                                      |
| updatedTime     | string(int64) | false    | none        | Update Time                        | Update time.                                                                      |
| currentLeverage | string        | false    | none        | Leverage at Close             | Leverage multiple at the time of close position.                                                      |


<a id="positiontransaction"></a>
### positiontransaction

| Name           | Type                                 | Required | Constraints | Description        | Notes                                                                  |
| -------------- | ------------------------------------ | -------- | ----------- | ------------------ | ---------------------------------------------------------------------- |
| code           | string                               | false    | none        | Status Code        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [[PositionTransaction](#schemapositiontransaction)] | false    | none        | Response Data     | Correct response data.                                                  |
| errorParam     | object                               | false    | none        | Error Parameters   | Error message parameter information                                     |
| requestTime    | string(timestamp)                    | false    | none        | Server Request Time | Time at which the server received the request                           |
| responseTime   | string(timestamp)                    | false    | none        | Server Response Time| Time at which the server sent the response                              |
| traceId        | string                               | false    | none        | Trace ID           | Invocation trace ID                                                  |


<a id="schemapositiontransaction"></a>
### schemapositiontransaction

| Name                    | Type          | Required | Constraints | Description                                                                              | Notes                                                                                            |
| ----------------------- | ------------- | -------- | ----------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| id                      | string(int64) | false    | none        | Unique Identifier                                                                        | Unique identifier.                                                                               |
| userId                  | string(int64) | false    | none        | User ID                                                                                  | ID of the owning user.                                                                         |
| accountId               | string(int64) | false    | none        | Account ID                                                                               | ID of the owning account.                                                                      |
| coinId                  | string(int64) | false    | none        | Collateral Coin ID                                                                       | ID of the associated collateral coin.                                                            |
| contractId              | string(int64) | false    | none        | Contract ID                                                                               | ID of the associated contract.                                                                  |
| type                    | string        | false    | none        | Detail Type                                                                              | Detail type.                                                                                     |
| deltaOpenSize           | string        | false    | none        | Change in Open Size                                                                    | Change in holding size.                                                                         |
| deltaOpenValue          | string        | false    | none        | Change in Open Value                                                                   | Change in open value.                                                                          |
| deltaOpenFee            | string        | false    | none        | Change in Open Fee                                                                      | Change in open fee.                                                                           |
| deltaFundingFee         | string        | false    | none        | Change in Funding Fee                                                                   | Change in funding fee.                                                                        |
| beforeOpenSize          | string        | false    | none        | Open Size Before Change                                                                | Holding size before the change.                                                                  |
| beforeOpenValue         | string        | false    | none        | Open Value Before Change                                                               | Open value before the change.                                                                   |
| beforeOpenFee           | string        | false    | none        | Open Fee Before Change                                                                 | Open fee before the change.                                                                    |
| beforeFundingFee        | string        | false    | none        | Funding Fee Before Change                                                              | Funding fee before the change.                                                                  |
| fillCloseSize           | string        | false    | none        | Transaction Close Size                                                                  | Transaction close size (positive for buy, negative for sell).                                          |
| fillCloseValue          | string        | false    | none        | Transaction Close Value                                                                  | Transaction close value (positive for buy, negative for sell).                                      |
| fillCloseFee            | string        | false    | none        | Transaction Close Fee                                                                     | Transaction close fee (typically zero or negative).                                            |
| fillOpenSize            | string        | false    | none        | Transaction Open Size                                                                  | Transaction open size (positive for buy, negative for sell).                                          |
| fillOpenValue           | string        | false    | none        | Transaction Open Value                                                                   | Transaction open value (positive for buy, negative for sell).                                      |
| fillOpenFee             | string        | false    | none        | Transaction Open Fee                                                                      | Transaction open fee (typically zero or negative).                                            |
| fillPrice               | string        | false    | none        | Transaction Price                                                                         | Transaction price (not precise, for display).                                              |
| liquidateFee            | string        | false    | none        | Liquidation Fee                                                                           | Liquidation fee (if close transaction is a liquidation, typically zero or negative).          |
| realizePnl              | string        | false    | none        | Realized Profit and Loss                                                                 | Realized profit and loss from a close (if a close transaction. Not precise, for display). |
| isLiquidate             | boolean       | false    | none        | Is Liquidation                                                                             | Whether the transaction is a liquidation.                                                       |
| isDeleverage            | boolean       | false    | none        | Is Auto-Deleveraging                                                                      | Whether the transaction is from auto-deleveraging.                                              |
| fundingTime             | string(int64) | false    | none        | Funding Settlement Time                                                                  | Funding settlement time.                                                                       |
| fundingRate             | string        | false    | none        | Funding Rate                                                                             | Funding rate.                                                                                   |
| fundingIndexPrice       | string        | false    | none        | Funding Index Price                                                                     | Index price related to funding rate.                                                         |
| fundingOraclePrice      | string        | false    | none        | Funding Oracle Price                                                                    | Oracle price related to funding rate.                                                        |
| fundingPositionSize     | string        | false    | none        | Position Size During Funding Settlement                                                  | Position size during funding settlement (positive for long, negative for short).               |
| orderId                 | string(int64) | false    | none        | Order ID                                                                                  | Associated order ID.                                                                              |
| orderFillTransactionId  | string(int64) | false    | none        | Order Fill Transaction ID                                                               | Associated order fill transaction ID.                                                       |
| collateralTransactionId | string(int64) | false    | none        | Collateral Transaction ID                                                             | Associated collateral transaction detail ID.                                                |
| forceTradeId            | string        | false    | none        | Force Trade ID                                                                            | Associated force trade order ID.                                                               |
| extraType               | string        | false    | none        | Extra Type                                                                               | Extra type for upper-layer business use.                                                     |
| extraDataJson           | string        | false    | none        | Extra Data                                                                              | Extra data in JSON format, default is empty string.                                           |
| censorStatus            | string        | false    | none        | Current Censoring Status                                                                  | Current censoring status.                                                                       |
| censorTxId              | string(int64) | false    | none        | Censoring Processing Sequence Number                                                       | Censoring processing sequence number, exists when censor_status=CENSOR_SUCCESS/CENSOR_FAILURE/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED.   |
| censorTime              | string(int64) | false    | none        | Censoring Processing Time                                                                   | Censoring processing time, exists when censor_status=CENSOR_SUCCESS/CENSOR_FAILURE/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED.   |
| censorFailCode          | string        | false    | none        | Censoring Failure Code                                                                     | Censoring failure code, exists when censor_status=CENSOR_FAILURE.                              |
| censorFailReason        | string        | false    | none        | Censoring Failure Reason                                                                   | Censoring failure reason, exists when censor_status=CENSOR_FAILURE.                            |
| l2TxId                  | string(int64) | false    | none        | L2 Push Transaction ID                                                                  | L2 push transaction ID, exists when censor_status=CENSOR_SUCCESS/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED.   |
| l2RejectTime            | string(int64) | false    | none        | L2 Rejection Time                                                                        | L2 rejection time, exists when censor_status=L2_REJECT/L2_REJECT_APPROVED.                  |
| l2RejectCode            | string        | false    | none        | L2 Rejection Error Code                                                                    | L2 rejection error code, exists when censor_status=L2_REJECT/L2_REJECT_APPROVED.               |
| l2RejectReason          | string        | false    | none        | L2 Rejection Reason                                                                      | L2 rejection reason, exists when censor_status=L2_REJECT/L2_REJECT_APPROVED.                  |
| l2ApprovedTime          | string(int64) | false    | none        | L2 Batch Verification Time                                                               | L2 batch verification time, exists when censor_status=L2_APPROVED/L2_REJECT_APPROVED.          |
| createdTime             | string(int64) | false    | none        | Creation Time                                                                                | Creation time.                                                                                  |
| updatedTime             | string(int64) | false    | none        | Update Time                                                                               | Update time.                                                                                    |

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

| Name           | Type                                       | Required | Constraints | Description                        | Notes                                                                  |
| -------------- | ------------------------------------------ | -------- | ----------- | ---------------------------------- | ---------------------------------------------------------------------- |
| code           | string                                     | false    | none        | Status Code                        | Returns "SUCCESS" on success; otherwise, it indicates failure.          |
| data           | [PageDataPositionTransaction](#schemapagedatapositiontransaction) | false    | none        | Generic Paginated Response         | Generic paginated response.                                                              |
| errorParam     | object                                     | false    | none        | Error Parameters                   | Error message parameter information                                     |
| requestTime    | string(timestamp)                          | false    | none        | Server Request Time               | Time at which the server received the request                           |
| responseTime   | string(timestamp)                          | false    | none        | Server Response Time              | Time at which the server sent the response                              |
| traceId        | string                                     | false    | none        | Trace ID                          | Invocation trace ID                                                  |


<a id="schemapagedatapositiontransaction"></a>
### schemapagedatapositiontransaction

| Name               | Type                                  | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ------------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [[PositionTransaction](#schemapositiontransaction)] | false    | none        | Data List                  | List of position transaction data.                                          |
| nextPageOffsetData | string                                | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="registeraccount"></a>
### RegisterAccount

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [RegisterAccountModel](#registeraccountmodel) | false    | none        | Register Account Response | Response data for account registration                                   |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="registeraccountmodel"></a>
### RegisterAccountModel

| Name      | Type          | Required | Constraints | Description                         | Notes                                                                     |
| --------- | ------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| accountId | string(int64) | false    | none        | Account ID                          | ID of the newly created account                                           |


