# TransferPrivateApi

<a id="opIdgetTransferInById"></a>

## Get Transfer In Records By Transfer In ID

GET /api/v1/private/transfer/getTransferInById

### Request Parameters

| Name             | Location | Type   | Required | Description                                                                                    |
| ---------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId        | query    | string | Yes       | Account ID                                                                                     |
| transferInIdList | query    | string | Yes      | List of transfer in IDs to retrieve. Used to batch fetch transfer in records by their IDs     |

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
            "senderAccountId": "543429922991899151",
            "senderL2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
            "senderTransferOutId": "563516408235819789",
            "clientTransferId": "client_transfer_123",
            "isConditionTransfer": false,
            "conditionFactRegistryAddress": "",
            "conditionFactErc20Address": "",
            "conditionFactAmount": "",
            "conditionFact": "",
            "transferReason": "NORMAL_TRANSFER",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#transferinlist) |

<a id="opIdgetActiveTransferIn"></a>

## Get Active Transfer In Records with Pagination

GET /api/v1/private/transfer/getActiveTransferIn

### Request Parameters

| Name                              | Location | Type   | Required | Description                                                                                   |
| --------------------------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------- |
| accountId                         | query    | string | Yes       | Account ID                                                                                    |
| size                              | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100. Default: 100 |
| offsetData                        | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                       |
| filterCoinIdList                  | query    | string | No       | Filter transfer in records by specified coin IDs. If not provided, all coin transfers are retrieved |
| filterStatusList                  | query    | string | No       | Filter transfer in records by specified statuses. If not provided, all status transfers are retrieved |
| filterTransferReasonList          | query    | string | No       | Filter transfer in records by specified transfer reasons. If not provided, all reason transfers are retrieved |
| filterStartCreatedTimeInclusive   | query    | string | No       | Filter transfer in records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndCreatedTimeExclusive     | query    | string | No       | Filter transfer in records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time |

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
                "senderAccountId": "543429922991899151",
                "senderL2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
                "senderTransferOutId": "563516408235819789",
                "clientTransferId": "client_transfer_123",
                "isConditionTransfer": false,
                "conditionFactRegistryAddress": "",
                "conditionFactErc20Address": "",
                "conditionFactAmount": "",
                "conditionFact": "",
                "transferReason": "NORMAL_TRANSFER",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#pagedatatransferin) |

<a id="opIdgetTransferOutById"></a>

## Get Transfer Out Records By Transfer Out ID

GET /api/v1/private/transfer/getTransferOutById

### Request Parameters

| Name              | Location | Type   | Required | Description                                                                                    |
| ----------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId         | query    | string | Yes       | Account ID                                                                                     |
| transferOutIdList | query    | string | Yes      | List of transfer out IDs to retrieve. Used to batch fetch transfer out records by their IDs   |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "id": "563516408235819789",
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "amount": "10.000000",
            "receiverAccountId": "543429922991899151",
            "receiverL2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
            "clientTransferId": "client_transfer_123",
            "isConditionTransfer": false,
            "conditionFactRegistryAddress": "",
            "conditionFactErc20Address": "",
            "conditionFactAmount": "",
            "conditionFact": "",
            "transferReason": "NORMAL_TRANSFER",
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
            "receiverTransferInId": "563516408235819790",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#transferoutlist) |

<a id="opIdgetActiveTransferOut"></a>

## Get Active Transfer Out Records with Pagination

GET /api/v1/private/transfer/getActiveTransferOut

### Request Parameters

| Name                              | Location | Type   | Required | Description                                                                                   |
| --------------------------------- | -------- | ------ | -------- | --------------------------------------------------------------------------------------------- |
| accountId                         | query    | string | Yes       | Account ID                                                                                    |
| size                              | query    | string | No       | Number of items to retrieve. Must be greater than 0 and less than or equal to 100. Default: 100 |
| offsetData                        | query    | string | No       | Pagination offset. If empty or not provided, the first page is retrieved                       |
| filterCoinIdList                  | query    | string | No       | Filter transfer out records by specified coin IDs. If not provided, all coin transfers are retrieved |
| filterStatusList                  | query    | string | No       | Filter transfer out records by specified statuses. If not provided, all status transfers are retrieved |
| filterTransferReasonList          | query    | string | No       | Filter transfer out records by specified transfer reasons. If not provided, all reason transfers are retrieved |
| filterStartCreatedTimeInclusive   | query    | string | No       | Filter transfer out records created after or at the specified start time (inclusive). If not provided or 0, retrieves records from the earliest time |
| filterEndCreatedTimeExclusive     | query    | string | No       | Filter transfer out records created before the specified end time (exclusive). If not provided or 0, retrieves records up to the latest time |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "id": "563516408235819789",
                "userId": "543429922866069763",
                "accountId": "543429922991899150",
                "coinId": "1000",
                "amount": "10.000000",
                "receiverAccountId": "543429922991899151",
                "receiverL2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
                "clientTransferId": "client_transfer_123",
                "isConditionTransfer": false,
                "conditionFactRegistryAddress": "",
                "conditionFactErc20Address": "",
                "conditionFactAmount": "",
                "conditionFact": "",
                "transferReason": "NORMAL_TRANSFER",
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
                "receiverTransferInId": "563516408235819790",
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#pagedatatransferout) |

<a id="opIdgetTransferOutAvailableAmount"></a>

## Get Transfer Out Available Amount

GET /api/v1/private/transfer/getTransferOutAvailableAmount

### Request Parameters

| Name      | Location | Type   | Required | Description                                                                                    |
| --------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| accountId | query    | string | Yes       | Account ID                                                                                     |
| coinId    | query    | string | Yes       | Coin ID to check available transfer amount for                                                |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "availableAmount": "1000.000000"
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#gettransferoutavailableamount) |

<a id="opIdcreateTransferOut"></a>

## Create Transfer Out 

POST /api/v1/private/transfer/createTransferOut

> Body Request Parameters

```json
{
    "accountId": "543429922991899150",
    "coinId": "1000",
    "amount": "10.000000",
    "receiverAccountId": "543429922991899151",
    "receiverL2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
    "clientTransferId": "client_transfer_123",
    "transferReason": "NORMAL_TRANSFER",
    "l2Nonce": "123456789",
    "l2ExpireTime": "1734352781355",
    "l2Signature": {
        "r": "0x...",
        "s": "0x...",
        "v": "0x..."
    },
    "extraType": "",
    "extraDataJson": ""
}
```

### Request Parameters

| Name                          | Location | Type   | Required | Description                                                                                    |
| ----------------------------- | -------- | ------ | -------- | ---------------------------------------------------------------------------------------------- |
| body                          | body     | object | Yes       | none                                                                                           |
| » accountId                   | body     | string | Yes       | Account ID                                                                                     |
| » coinId                      | body     | string | Yes       | Coin ID for the transfer asset                                                                 |
| » amount                      | body     | string | Yes       | Transfer amount                                                                                |
| » receiverAccountId           | body     | string | Yes       | Receiver account ID                                                                            |
| » receiverL2Key               | body     | string | Yes       | Receiver account L2 key (bigint as hex string)                                                |
| » clientTransferId            | body     | string | Yes       | Client-defined ID for idempotency verification and signature nonce generation                 |
| » transferReason              | body     | string | Yes       | Transfer reason                                                                                |
| » l2Nonce                     | body     | string | Yes       | L2 signature nonce. First 32 bits of sha256(client_transfer_id)                               |
| » l2ExpireTime                | body     | string | Yes       | L2 signature expiration time in milliseconds                                                  |
| » l2Signature                 | body     | object | Yes       | L2 signature submitted                                                                         |
| »» r                          | body     | string | Yes       | Signature R component (bigint as hex string)                                                  |
| »» s                          | body     | string | Yes       | Signature S component (bigint as hex string)                                                  |
| »» v                          | body     | string | Yes       | Signature V component (bigint as hex string)                                                  |
| » extraType                   | body     | string | No       | Additional type for upper-layer business use                                                  |
| » extraDataJson               | body     | string | No       | Extra data in JSON format                                                                      |

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "transferOutId": "563516408235819789"
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                                  | default response | [Result](#createtransferout) |

# Data Models

<a id="transferinlist"></a>
### TransferInList

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [TransferInModel](#transferinmodel)[]  | false    | none        | Transfer In List         | Array of transfer in records                                             |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="pagedatatransferin"></a>
### PageDataTransferIn

| Name               | Type                               | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ---------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [TransferInModel](#transferinmodel)[]  | false    | none        | Data List                  | Array of transfer in records                                        |
| nextPageOffsetData | string                             | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="transferoutlist"></a>
### TransferOutList

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [TransferOutModel](#transferoutmodel)[]  | false    | none        | Transfer Out List        | Array of transfer out records                                            |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="pagedatatransferout"></a>
### PageDataTransferOut

| Name               | Type                               | Required | Constraints | Description                 | Notes                                                               |
| ------------------ | ---------------------------------- | -------- | ----------- | --------------------------- | ------------------------------------------------------------------- |
| dataList           | [TransferOutModel](#transferoutmodel)[]  | false    | none        | Data List                  | Array of transfer out records                                       |
| nextPageOffsetData | string                             | false    | none        | Next Page Offset        | Offset for retrieving the next page. If no next page data, empty string. |

<a id="transferinmodel"></a>
### TransferInModel

| Name                     | Type            | Required | Constraints | Description                         | Notes                                                                     |
| ------------------------ | --------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| id                       | string(int64)   | false    | none        | Transfer In ID                      | Unique identifier for the transfer in record                             |
| userId                   | string(int64)   | false    | none        | User ID                             | ID of the owning user                                                  |
| accountId                | string(int64)   | false    | none        | Account ID                          | ID of the owning account                                                 |
| coinId                   | string(int64)   | false    | none        | Coin ID                             | ID of the collateral coin                                                |
| amount                   | string          | false    | none        | Transfer Amount                     | Amount of the transfer                                                    |
| senderAccountId          | string(int64)   | false    | none        | Sender Account ID                   | ID of the sending account                                                |
| senderL2Key              | string          | false    | none        | Sender L2 Key                       | Sender account L2 key (bigint as hex string)                            |
| senderTransferOutId      | string(int64)   | false    | none        | Sender Transfer Out ID              | ID of the corresponding transfer out record                              |
| clientTransferId         | string          | false    | none        | Client Transfer ID                  | Client-defined ID for idempotency verification                           |
| isConditionTransfer      | boolean         | false    | none        | Is Condition Transfer               | Whether this is a conditional transfer                                   |
| conditionFactRegistryAddress | string      | false    | none        | Condition Fact Registry Address     | Fact registry contract address for conditional transfers                 |
| conditionFactErc20Address | string         | false    | none        | Condition Fact ERC20 Address        | ERC20 address used for fact generation in conditional transfers          |
| conditionFactAmount      | string          | false    | none        | Condition Fact Amount               | Amount used for fact generation in conditional transfers                 |
| conditionFact            | string          | false    | none        | Condition Fact                      | Fact for conditional transfers                                           |
| transferReason           | string          | false    | none        | Transfer Reason                     | Reason for the transfer                                                  |
| extraType                | string          | false    | none        | Extra Type                          | Additional type for upper-layer business use                             |
| extraDataJson            | string          | false    | none        | Extra Data JSON                     | Extra data in JSON format                                                |
| status                   | string          | false    | none        | Transfer Status                     | Current status of the transfer                                           |
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

<a id="transferoutmodel"></a>
### TransferOutModel

| Name                     | Type            | Required | Constraints | Description                         | Notes                                                                     |
| ------------------------ | --------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| id                       | string(int64)   | false    | none        | Transfer Out ID                     | Unique identifier for the transfer out record                            |
| userId                   | string(int64)   | false    | none        | User ID                             | ID of the owning user                                                  |
| accountId                | string(int64)   | false    | none        | Account ID                          | ID of the owning account                                                 |
| coinId                   | string(int64)   | false    | none        | Coin ID                             | ID of the collateral coin                                                |
| amount                   | string          | false    | none        | Transfer Amount                     | Amount of the transfer                                                    |
| receiverAccountId        | string(int64)   | false    | none        | Receiver Account ID                 | ID of the receiving account                                              |
| receiverL2Key            | string          | false    | none        | Receiver L2 Key                     | Receiver account L2 key (bigint as hex string)                          |
| clientTransferId         | string          | false    | none        | Client Transfer ID                  | Client-defined ID for idempotency verification                           |
| isConditionTransfer      | boolean         | false    | none        | Is Condition Transfer               | Whether this is a conditional transfer                                   |
| conditionFactRegistryAddress | string      | false    | none        | Condition Fact Registry Address     | Fact registry contract address for conditional transfers                 |
| conditionFactErc20Address | string         | false    | none        | Condition Fact ERC20 Address        | ERC20 address used for fact generation in conditional transfers          |
| conditionFactAmount      | string          | false    | none        | Condition Fact Amount               | Amount used for fact generation in conditional transfers                 |
| conditionFact            | string          | false    | none        | Condition Fact                      | Fact for conditional transfers                                           |
| transferReason           | string          | false    | none        | Transfer Reason                     | Reason for the transfer                                                  |
| l2Nonce                  | string(int64)   | false    | none        | L2 Nonce                            | L2 signature nonce. First 32 bits of sha256(client_transfer_id)          |
| l2ExpireTime             | string(int64)   | false    | none        | L2 Expire Time                      | L2 signature expiration time in milliseconds                             |
| l2Signature              | [L2SignatureModel](#l2signaturemodel) | false    | none        | L2 Signature                        | L2 signature submitted                                                   |
| extraType                | string          | false    | none        | Extra Type                          | Additional type for upper-layer business use                             |
| extraDataJson            | string          | false    | none        | Extra Data JSON                     | Extra data in JSON format                                                |
| status                   | string          | false    | none        | Transfer Status                     | Current status of the transfer                                           |
| receiverTransferInId     | string(int64)   | false    | none        | Receiver Transfer In ID             | ID of the corresponding transfer in record                               |
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

<a id="gettransferoutavailableamount"></a>
### GetTransferOutAvailableAmount

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [GetTransferOutAvailableAmountModel](#gettransferoutavailableamountmodel) | false    | none        | Available Amount Response | Response data for available transfer amount                              |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="gettransferoutavailableamountmodel"></a>
### GetTransferOutAvailableAmountModel

| Name            | Type   | Required | Constraints | Description                         | Notes                                                                     |
| --------------- | ------ | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| availableAmount | string | false    | none        | Available Amount                    | Available amount for transfer (decimal format)                           |

<a id="createtransferout"></a>
### CreateTransferOut

| Name           | Type                               | Required | Constraints | Description              | Notes                                                                    |
| -------------- | ---------------------------------- | -------- | ----------- | ------------------------ | ------------------------------------------------------------------------ |
| code           | string                             | false    | none        | Status Code              | Returns "SUCCESS" on success; otherwise, it indicates failure.            |
| data           | [CreateTransferOutModel](#createtransferoutmodel) | false    | none        | Create Transfer Out Response | Response data for transfer out creation                                  |
| errorParam     | object                             | false    | none        | Error Parameters         | Error message parameter information                                       |
| requestTime    | string(timestamp)                  | false    | none        | Server Request Time     | Time at which the server received the request                             |
| responseTime   | string(timestamp)                  | false    | none        | Server Response Time    | Time at which the server sent the response                                |
| traceId        | string                             | false    | none        | Trace ID                | Invocation trace ID                                                     |

<a id="createtransferoutmodel"></a>
### CreateTransferOutModel

| Name          | Type          | Required | Constraints | Description                         | Notes                                                                     |
| ------------- | ------------- | -------- | ----------- | ----------------------------------- | ------------------------------------------------------------------------- |
| transferOutId | string(int64) | false    | none        | Transfer Out ID                     | ID of the created transfer out record                                    |
