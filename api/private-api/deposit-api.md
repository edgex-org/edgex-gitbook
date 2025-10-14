# DepositPrivateApi

<a id="opIdgetDepositById"></a>

## Get Deposits By Deposit ID

GET /api/v1/private/deposit/getDepositById

### Request Parameters

| Name          | Location | Type   | Required | Description                                                                                    |
| ------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId     | query    | string | Yes       | Account ID                                                                                     |
| depositIdList | query    | string | Yes      | List of deposit IDs to retrieve. Used to batch fetch deposit records by their IDs             |

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
            "clientDepositId": "client_deposit_123",
            "l1Tx": {
                "hash": "0x...",
                "index": 0,
                "time": "1734352781355",
                "blockHeight": "12345678"
            },
            "riskSignature": {
                "r": "0x...",
                "s": "0x...",
                "v": "0x..."
            },
            "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#depositlist) |

<a id="opIdgetDepositByClientDepositId"></a>

## Get Deposits By Client Deposit ID

GET /api/v1/private/deposit/getDepositByClientDepositId

### Request Parameters

| Name                 | Location | Type   | Required | Description                                                                                           |
| -------------------- | -------- | ------ | -------- | ----------------------------------------------------------------------------------------------------- |
| accountId            | query    | string | Yes       | Account ID                                                                                            |
| clientDepositIdList  | query    | string | Yes      | List of client-defined deposit IDs. Used to batch fetch deposit records by client-specified IDs      |

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
            "clientDepositId": "client_deposit_123",
            "l1Tx": {
                "hash": "0x...",
                "index": 0,
                "time": "1734352781355",
                "blockHeight": "12345678"
            },
            "riskSignature": {
                "r": "0x...",
                "s": "0x...",
                "v": "0x..."
            },
            "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#depositlist) |

<a id="opIdgetActiveDeposit"></a>

## Get Active Deposits with Pagination

GET /api/v1/private/deposit/getActiveDeposit

### Request Parameters

| Name       | Location | Type   | Required | Description                                                                                   |
| ---------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------- |
| accountId  | query    | string | Yes       | Account ID                                                                                    |
| size       | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100. Default: 100 |
| offsetData | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                       |

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
                "clientDepositId": "client_deposit_123",
                "l1Tx": {
                    "hash": "0x...",
                    "index": 0,
                    "time": "1734352781355",
                    "blockHeight": "12345678"
                },
                "riskSignature": {
                    "r": "0x...",
                    "s": "0x...",
                    "v": "0x..."
                },
                "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#pagedatadeposit) |

<a id="opIdcreateDeposit"></a>

## Create Deposit

POST /api/v1/private/deposit/createDeposit

### Request Parameters

| Name             | Location | Type   | Required | Description                                                                                    |
| ---------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId        | body     | string | Yes       | Account ID                                                                                     |
| coinId           | body     | string | Yes       | Currency ID                                                                                    |
| amount           | body     | string | Yes       | Deposit amount (decimal format)                                                                |
| ethAddress       | body     | string | Yes       | Ethereum address for the deposit                                                               |
| erc20Address     | body     | string | Yes       | ERC20 token contract address                                                                   |
| clientDepositId  | body     | string | Yes       | Client-defined ID for idempotency verification                                                 |
| l1Tx             | body     | object | Yes       | L1 transaction information                                                                     |
| riskSignature    | body     | string | Yes       | l2Signature (128 characters fixed length, 0-63: r, 64-128: s   )                                          |
| l2Key            | body     | string | Yes       | L2 recipient account key                                                                       |
| extraType        | body     | string | Yes       | Additional type for upper-layer business use                                                   |
| extraDataJson    | body     | string | No       | Extra data in JSON format, defaults to empty string                                           |

### Request Body Schema

```json
{
    "accountId": "543429922991899150",
    "coinId": "1000",
    "amount": "10.000000",
    "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
    "erc20Address": "0x...",
    "clientDepositId": "client_deposit_123",
    "l1Tx": {
        "hash": "0x...",
        "index": 0,
        "time": "1734352781355",
        "blockHeight": "12345678"
    },
    "riskSignature": "0x...",
    "l2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
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
        "depositId": "563516408235819790"
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#createdeposit) |

<a id="opIdrequestRelayerSignAndBroadcast"></a>

## Request Relayer Sign and Broadcast

POST /api/v1/private/deposit/requestRelayerSignAndBroadcast

### Request Parameters

| Name         | Location | Type   | Required | Description                                                                                    |
| ------------ | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| deadline     | body     | string | Yes       | Deadline timestamp                                                                             |
| r            | body     | string | Yes       | Client signature r component                                                                   |
| s            | body     | string | Yes       | Client signature s component                                                                   |
| v            | body     | string | Yes       | Client signature v component                                                                   |
| type         | body     | string | Yes       | Transaction type                                                                               |
| amount       | body     | string | Yes       | Amount                                                                                         |
| owner        | body     | string | Yes       | Owner address                                                                                  |
| starkKey     | body     | string | Yes       | Stark key                                                                                      |
| positionId   | body     | string | Yes       | Position ID                                                                                    |
| chainId      | body     | string | Yes       | Chain ID                                                                                       |
| mpcSignature | body     | string | Yes       | MPC signature                                                                                  |

### Request Body Schema

```json
{
    "deadline": "1734352781355",
    "r": "0x...",
    "s": "0x...",
    "v": "27",
    "type": "",
    "amount": "10.000000",
    "owner": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
    "starkKey": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
    "positionId": "123456789",
    "chainId": "1",
    "mpcSignature": "0x..."
}
```

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "success": true
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#requestrelayersignandbroadcast) |

# Data Models

<a id="depositlist"></a>
### DepositList

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [DepositModel](#depositmodel)[]    | false    | none        | Deposit List             | Array of deposit records                                                 |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="pagedatadeposit"></a>
### PageDataDeposit

| Name               | Type                               | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ---------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [DepositModel](#depositmodel)[]    | false    | none        | Data List                  | Array of deposit records                                            |
| nextPageOffsetData | string                             | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="depositmodel"></a>
### DepositModel

| Name                     | Type            | Required | Constraints | Description                         | Notes                                                                     |
| ------------------------ | --------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| id                       | string(int64)   | false    | none        | Deposit ID                          | Unique identifier for the deposit record                                  |
| userId                   | string(int64)   | false    | none        | User ID                             | ID of the owning user                                                  |
| accountId                | string(int64)   | false    | none        | Account ID                          | ID of the owning account                                                 |
| coinId                   | string(int64)   | false    | none        | Coin ID                             | ID of the collateral coin                                                |
| amount                   | string          | false    | none        | Deposit Amount                      | Amount of the deposit                                                     |
| ethAddress               | string          | false    | none        | Ethereum Address                    | Ethereum address for the deposit, may differ from account's eth address  |
| erc20Address             | string          | false    | none        | ERC20 Contract Address              | Contract address of the deposited token                                  |
| clientDepositId          | string          | false    | none        | Client Deposit ID                   | Client-defined ID for idempotency verification                           |
| l1Tx                     | [L1TxModel](#l1txmodel) | false    | none        | L1 Transaction Info                 | Layer 1 transaction information                                          |
| riskSignature            | [L2SignatureModel](#l2signaturemodel) | false    | none        | Risk Signature                      | Risk control signature                                                   |
| l2Key                    | string          | false    | none        | L2 Key                              | Layer 2 recipient account key                                            |
| extraType                | string          | false    | none        | Extra Type                          | Additional type for upper-layer business use                             |
| extraDataJson            | string          | false    | none        | Extra Data JSON                     | Extra data in JSON format                                                |
| status                   | string          | false    | none        | Deposit Status                      | Current status of the deposit                                            |
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

<a id="l1txmodel"></a>
### L1TxModel

| Name        | Type            | Required | Constraints | Description                         | Notes                                                                     |
| ----------- | --------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| hash        | string          | false    | none        | Transaction Hash                    | Layer 1 transaction hash                                                  |
| index       | integer(int32)  | false    | none        | Transaction Index                   | Index of the transaction hash                                             |
| time        | string(int64)   | false    | none        | Transaction Time                    | On-chain timestamp in milliseconds                                       |
| blockHeight | string(int64)   | false    | none        | Block Height                        | Block height where the transaction is located                             |

<a id="l2signaturemodel"></a>
### L2SignatureModel

| Name | Type   | Required | Constraints | Description                         | Notes                                                                     |
| ---- | ------ | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| r    | string | false    | none        | Signature R Component               | Bigint represented as hex string                                          |
| s    | string | false    | none        | Signature S Component               | Bigint represented as hex string                                          |
| v    | string | false    | none        | Signature V Component               | Bigint represented as hex string                                          |

<a id="createdeposit"></a>
### CreateDeposit

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [CreateDepositModel](#createdepositmodel) | false    | none        | Create Deposit Response  | Response data for deposit creation                                       |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="createdepositmodel"></a>
### CreateDepositModel

| Name      | Type          | Required | Constraints | Description                         | Notes                                                                     |
| --------- | ------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| depositId | string(int64) | false    | none        | Deposit ID                          | ID of the created deposit record                                          |

<a id="requestrelayersignandbroadcast"></a>
### RequestRelayerSignAndBroadcast

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [RequestRelayerSignAndBroadcastModel](#requestrelayersignandbroadcastmodel) | false    | none        | Relayer Response         | Response data for relayer sign and broadcast                             |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="requestrelayersignandbroadcastmodel"></a>
### RequestRelayerSignAndBroadcastModel

| Name    | Type    | Required | Constraints | Description                         | Notes                                                                     |
| ------- | ------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| success | boolean | false    | none        | Success Status                      | Indicates whether the relayer sign and broadcast operation was successful |
