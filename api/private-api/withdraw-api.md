# WithdrawPrivateApi

<a id="opIdgetWithdrawById"></a>

## GET Get Withdrawals By Withdraw ID

GET /api/v1/private/withdraw/getWithdrawById

### Request Parameters

| Name           | Location | Type   | Required | Description                                                                                    |
| -------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId      | query    | string | Yes       | Account ID                                                                                     |
| withdrawIdList | query    | string | Yes      | List of withdraw IDs to retrieve. Used to batch fetch withdraw records by their IDs           |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "id": "563516408235819790",
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "amount": "10.000000",
            "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
            "erc20Address": "0x...",
            "clientWithdrawId": "client_withdraw_123",
            "riskSignature": {
                "r": "0x...",
                "s": "0x...",
                "v": "0x..."
            },
            "l2Nonce": "123456789",
            "l2ExpireTime": "1734352781355",
            "l2Signature": {
                "r": "0x...",
                "s": "0x...",
                "v": "0x..."
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "SUCCESS_L2_APPROVED",
            "collateralTransactionId": "563516408265179918",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#withdrawlist) |

<a id="opIdgetWithdrawByClientWithdrawId"></a>

## GET Get Withdrawals By Client Withdraw ID

GET /api/v1/private/withdraw/getWithdrawByClientWithdrawId

### Request Parameters

| Name                    | Location | Type   | Required | Description                                                                                           |
| ----------------------- | -------- | ------ | -------- | ----------------------------------------------------------------------------------------------------- |
| accountId               | query    | string | Yes       | Account ID                                                                                            |
| clientWithdrawIdList    | query    | string | Yes      | List of client-defined withdraw IDs. Used to batch fetch withdraw records by client-specified IDs    |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "id": "563516408235819790",
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "amount": "10.000000",
            "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
            "erc20Address": "0x...",
            "clientWithdrawId": "client_withdraw_123",
            "riskSignature": {
                "r": "0x...",
                "s": "0x...",
                "v": "0x..."
            },
            "l2Nonce": "123456789",
            "l2ExpireTime": "1734352781355",
            "l2Signature": {
                "r": "0x...",
                "s": "0x...",
                "v": "0x..."
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "SUCCESS_L2_APPROVED",
            "collateralTransactionId": "563516408265179918",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#withdrawlist) |

<a id="opIdgetActiveWithdraw"></a>

## GET Get Active Withdrawals with Pagination

GET /api/v1/private/withdraw/getActiveWithdraw

### Request Parameters

| Name                              | Location | Type   | Required | Description                                                                                   |
| --------------------------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------- |
| accountId                         | query    | string | Yes       | Account ID                                                                                    |
| size                              | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100. Default: 100 |
| offsetData                        | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                       |
| filterCoinIdList                  | query    | string | No       | Filter withdrawals by specified coin IDs. If not provided, all coin withdrawals are retrieved |
| filterStatusList                  | query    | string | No       | Filter withdrawals by specified statuses. If not provided, all status withdrawals are retrieved |
| filterStartCreatedTimeInclusive   | query    | string | No       | Filter withdrawals created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndCreatedTimeExclusive     | query    | string | No       | Filter withdrawals created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "id": "563516408235819790",
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "amount": "10.000000",
                "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
                "erc20Address": "0x...",
                "clientWithdrawId": "client_withdraw_123",
                "riskSignature": {
                    "r": "0x...",
                    "s": "0x...",
                    "v": "0x..."
                },
                "l2Nonce": "123456789",
                "l2ExpireTime": "1734352781355",
                "l2Signature": {
                    "r": "0x...",
                    "s": "0x...",
                    "v": "0x..."
                },
                "extraType": "",
                "extraDataJson": "",
                "status": "SUCCESS_L2_APPROVED",
                "collateralTransactionId": "563516408265179918",
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
        "nextPageOffsetData": ""
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#pagedatawithdraw) |

<a id="opIdgetWithdrawAvailableAmount"></a>

## GET Get Withdraw Available Amount

GET /api/v1/private/withdraw/getWithdrawAvailableAmount

### Request Parameters

| Name      | Location | Type   | Required | Description                                                                                    |
| --------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId | query    | string | Yes       | Account ID                                                                                     |
| coinId    | query    | string | Yes       | Coin ID to check available withdrawal amount                                                   |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "availableAmount": "100.000000"
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#getwithdrawavailableamount) |

<a id="opIdcreateWithdraw"></a>

## POST Create Withdraw

POST /api/v1/private/withdraw/createWithdraw

### Request Parameters

| Name              | Location | Type   | Required | Description                                                                                    |
| ----------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId         | body     | string | Yes       | Account ID                                                                                     |
| coinId            | body     | string | Yes       | Currency ID                                                                                    |
| amount            | body     | string | Yes       | Withdraw amount (decimal format)                                                               |
| ethAddress        | body     | string | Yes       | Ethereum address for the withdrawal                                                            |
| erc20Address      | body     | string | Yes       | ERC20 token contract address                                                                   |
| clientWithdrawId  | body     | string | Yes       | Client-defined ID for idempotency verification                                                 |
| riskSignature     | body     | string | Yes       | Risk control signature (128 characters fixed length)                                          |
| l2Nonce           | body     | string | Yes       | L2 signature nonce. First 32 bits of sha256(client_withdraw_id)                               |
| l2ExpireTime      | body     | string | Yes       | L2 signature expiration time in milliseconds. For signature generation/verification, use hours: l2_expire_time / 3600000 |
| l2Signature       | body     | string | Yes       | L2 signature submitted (128 characters fixed length)                                          |
| extraType         | body     | string | Yes       | Additional type for upper-layer business use                                                   |
| extraDataJson     | body     | string | No       | Extra data in JSON format, defaults to empty string                                           |

### Request Body Schema

```json
{
    "accountId": "543429922991899150",
    "coinId": "1000",
    "amount": "10.000000",
    "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
    "erc20Address": "0x...",
    "clientWithdrawId": "client_withdraw_123",
    "riskSignature": "0x...",
    "l2Nonce": "123456789",
    "l2ExpireTime": "1734352781355",
    "l2Signature": "0x...",
    "extraType": "",
    "extraDataJson": ""
}
```

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "withdrawId": "563516408235819790"
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#createwithdraw) |

# Data Models

<a id="withdrawlist"></a>
### WithdrawList

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [WithdrawModel](#withdrawmodel)[]  | false    | none        | Withdraw List            | Array of withdraw records                                                |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="pagedatawithdraw"></a>
### PageDataWithdraw

| Name               | Type                               | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ---------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [WithdrawModel](#withdrawmodel)[]  | false    | none        | Data List                  | Array of withdraw records                                           |
| nextPageOffsetData | string                             | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="withdrawmodel"></a>
### WithdrawModel

| Name                     | Type            | Required | Constraints | Description                         | Notes                                                                     |
| ------------------------ | --------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| id                       | string(int64)   | false    | none        | Withdraw ID                         | Unique identifier for the withdraw record                                 |
| userId                   | string(int64)   | false    | none        | User ID                             | ID of the owning user                                                  |
| accountId                | string(int64)   | false    | none        | Account ID                          | ID of the owning account                                                 |
| coinId                   | string(int64)   | false    | none        | Coin ID                             | ID of the collateral coin                                                |
| amount                   | string          | false    | none        | Withdraw Amount                     | Amount of the withdrawal                                                  |
| ethAddress               | string          | false    | none        | Ethereum Address                    | Ethereum address for the withdrawal                                       |
| erc20Address             | string          | false    | none        | ERC20 Contract Address              | Contract address of the withdrawn token                                   |
| clientWithdrawId         | string          | false    | none        | Client Withdraw ID                  | Client-defined ID for idempotency verification                           |
| riskSignature            | [L2SignatureModel](#l2signaturemodel) | false    | none        | Risk Signature                      | Risk control signature                                                   |
| l2Nonce                  | string(int64)   | false    | none        | L2 Nonce                            | L2 signature nonce. First 32 bits of sha256(client_withdraw_id)          |
| l2ExpireTime             | string(int64)   | false    | none        | L2 Expire Time                      | L2 signature expiration time in milliseconds                             |
| l2Signature              | [L2SignatureModel](#l2signaturemodel) | false    | none        | L2 Signature                        | L2 signature submitted                                                   |
| extraType                | string          | false    | none        | Extra Type                          | Additional type for upper-layer business use                             |
| extraDataJson            | string          | false    | none        | Extra Data JSON                     | Extra data in JSON format                                                |
| status                   | string          | false    | none        | Withdraw Status                     | Current status of the withdrawal                                         |
| collateralTransactionId  | string(int64)   | false    | none        | Collateral Transaction ID           | Associated collateral transaction ID when status is SUCCESS_XXX/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorTxId               | string(int64)   | false    | none        | Censor Transaction ID               | Censorship processing sequence number when status is SUCCESS_XXX/FAILED_XXX |
| censorTime               | string(int64)   | false    | none        | Censor Time                         | Censorship processing time when status is SUCCESS_XXX/FAILED_XXX         |
| censorFailCode           | string          | false    | none        | Censor Fail Code                    | Censorship failure code                                                  |
| censorFailReason         | string          | false    | none        | Censor Fail Reason                  | Censorship failure reason                                                |
| l2TxId                   | string(int64)   | false    | none        | L2 Transaction ID                   | Layer 2 transaction ID                                                   |
| l2RejectTime             | string(int64)   | false    | none        | L2 Reject Time                      | Layer 2 rejection time                                                   |
| l2RejectCode             | string          | false    | none        | L2 Reject Code                      | Layer 2 rejection code                                                   |
| l2RejectReason           | string          | false    | none        | L2 Reject Reason                    | Layer 2 rejection reason                                                 |
| l2ApprovedTime           | string(int64)   | false    | none        | L2 Approved Time                    | Layer 2 approval time                                                    |
| createdTime              | string(int64)   | false    | none        | Created Time                        | Record creation time                                                     |
| updatedTime              | string(int64)   | false    | none        | Updated Time                        | Record last update time                                                  |

<a id="l2signaturemodel"></a>
### L2SignatureModel

| Name | Type   | Required | Constraints | Description                         | Notes                                                                     |
| ---- | ------ | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| r    | string | false    | none        | Signature R Component               | Bigint represented as hex string                                          |
| s    | string | false    | none        | Signature S Component               | Bigint represented as hex string                                          |
| v    | string | false    | none        | Signature V Component               | Bigint represented as hex string                                          |

<a id="getwithdrawavailableamount"></a>
### GetWithdrawAvailableAmount

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [GetWithdrawAvailableAmountModel](#getwithdrawavailableamountmodel) | false    | none        | Available Amount Response | Response data for available withdrawal amount                            |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="getwithdrawavailableamountmodel"></a>
### GetWithdrawAvailableAmountModel

| Name            | Type   | Required | Constraints | Description                         | Notes                                                                     |
| --------------- | ------ | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| availableAmount | string | false    | none        | Available Amount                    | Available amount for withdrawal (decimal format)                         |

<a id="createwithdraw"></a>
### CreateWithdraw

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [CreateWithdrawModel](#createwithdrawmodel) | false    | none        | Create Withdraw Response | Response data for withdraw creation                                      |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="createwithdrawmodel"></a>
### CreateWithdrawModel

| Name       | Type          | Required | Constraints | Description                         | Notes                                                                     |
| ---------- | ------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| withdrawId | string(int64) | false    | none        | Withdraw ID                         | ID of the created withdraw record                                         |
