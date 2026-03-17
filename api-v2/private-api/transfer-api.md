# Transfer Private API

<a id="opIdgetTransferInById"></a>

## GET Get Transfer In Orders by ID

GET /api/v2/private/transfer/getTransferInById

**Query encoding note**: For list-style GET parameters such as `transferInIdList` and `transferOutIdList`, use the same serialization format expected by your SDK or backend gateway. If your client does not support repeated query keys automatically, send the values in the exact format used by your signing payload to avoid signature mismatches.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|transferInIdList|string|No|Transfer In ID list. Serialize this list using the exact query format expected by your client or SDK when generating the request signature.|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "id": "564819036173500554",
            "userId": "543429922866069763",
            "accountId": "551109015904453258",
            "coinId": "1000",
            "amount": "1.000000",
            "senderAccountId": "543429922991899150",
            "senderL2Key": "0x5580341e2c99823a0a35356b8ac84e372dd38fd1f4b50f607b931ec8038c211",
            "senderTransferOutId": "564819036077031694",
            "clientTransferId": "543429922991899150:3877531064364166",
            "isConditionTransfer": false,
            "conditionFactRegistryAddress": "",
            "conditionFactErc20Address": "",
            "conditionFactAmount": "",
            "conditionFact": "",
            "transferReason": "USER_TRANSFER",
            "extraType": "",
            "extraDataJson": "",
            "status": "SUCCESS_CENSOR_SUCCESS",
            "collateralTransactionId": "564819036219637898",
            "censorTxId": "893179",
            "censorTime": "1734663352062",
            "censorFailCode": "",
            "censorFailReason": "",
            "l2TxId": "1084730",
            "l2RejectTime": "0",
            "l2RejectCode": "",
            "l2RejectReason": "",
            "l2ApprovedTime": "0",
            "createdTime": "1734663352054",
            "updatedTime": "1734663352065"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734663945432",
    "responseTime": "1734663945452",
    "traceId": "eb4cfe0a20f14b62b4fdbbd046255171"
}
```

### Response Codes

| Status Code | Description | Notes | Schema |
|---|---|---|---|
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlisttransferin) |

<a id="opIdgetTransferOutById"></a>

## GET Get Transfer Out Orders by ID

GET /api/v2/private/transfer/getTransferOutById

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|transferOutIdList|string|No|Transfer Out ID list. Serialize this list using the exact query format expected by your client or SDK when generating the request signature.|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "id": "564819036077031694",
            "userId": "543429922866069763",
            "accountId": "543429922991899150",
            "coinId": "1000",
            "amount": "1.000000",
            "receiverAccountId": "551109015904453258",
            "receiverL2Key": "0x3eec711e360695bb44b1170057a25340303c1f16893a8def7450e44294405a8",
            "clientTransferId": "3877531064364166",
            "isConditionTransfer": false,
            "conditionFactRegistryAddress": "",
            "conditionFactErc20Address": "",
            "conditionFactAmount": "",
            "conditionFact": "",
            "transferReason": "USER_TRANSFER",
            "l2Nonce": "2280110103",
            "l2ExpireTime": "1735873200000",
            "l2Signature": {
                "r": "0x0141279ec45ce1ea37b11cfa4683cfab8443bcbf8da3f066cef3e437862573f9",
                "s": "0x034efe12eee1be3fc715c7b511f69e3ba32ec67a9ac89538fbb73de46fefc5e5",
                "v": ""
            },
            "extraType": "",
            "extraDataJson": "",
            "status": "SUCCESS_CENSOR_SUCCESS",
            "receiverTransferInId": "564819036173500554",
            "collateralTransactionId": "564819036223832334",
            "censorTxId": "893179",
            "censorTime": "1734663352062",
            "censorFailCode": "",
            "censorFailReason": "",
            "l2TxId": "1084730",
            "l2RejectTime": "0",
            "l2RejectCode": "",
            "l2RejectReason": "",
            "l2ApprovedTime": "0",
            "createdTime": "1734663352031",
            "updatedTime": "1734663352066"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734663607244",
    "responseTime": "1734663607271",
    "traceId": "39366eb1153313ba1415851a08762265"
}
```

### Response Codes

| Status Code | Description | Notes | Schema |
|---|---|---|---|
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlisttransferout) |

<a id="opIdgetTransferOutAvailableAmount"></a>

## GET Get Transfer Out Available Amount

GET /api/v2/private/transfer/getTransferOutAvailableAmount

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|coinId|string|Yes|Coin ID|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "availableAmount": "10.964371"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734663286946",
    "responseTime": "1734663286951",
    "traceId": "957f0396a8e6059b027b99d232f8b113"
}
```

### Response Codes

| Status Code | Description | Notes | Schema |
|---|---|---|---|
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultgettransferoutavailableamount) |

<a id="opIdcreateTransferOut"></a>

## POST Create Transfer Out Order

POST /api/v2/private/transfer/createTransferOut

> Body Request Parameters

```json
{
    "accountId": "543429922991899150",
    "coinId": "1000",
    "amount": "1.000000",
    "receiverAccountId": "551109015904453258",
    "clientTransferId": "3877531064364166",
    "transferReason": "USER_TRANSFER",
    "l2Nonce": "2280110103",
    "l2ExpireTime": "1735873200000",
    "l2Signature": "0141279ec45ce1ea37b11cfa4683cfab8443bcbf8da3f066cef3e437862573f9034efe12eee1be3fc715c7b511f69e3ba32ec67a9ac89538fbb73de46fefc5e5",
    "extraType": "",
    "extraDataJson": ""
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|[CreateTransferOutParam](#schemacreatetransferoutparam)|No|Request body for creating a transfer-out order.|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "transferOutId": "564819036077031694"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734663351997",
    "responseTime": "1734663352035",
    "traceId": "33728335fc663ba9230e61d4f4b924df"
}
```

### Response Codes

| Status Code | Description | Notes | Schema |
|---|---|---|---|
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultcreatetransferout) |

<a id="schemaresultpagedatatransferin"></a>
### TransferInPageResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[PageDataTransferIn](#schemapagedatatransferin)|Generic paginated response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemapagedatatransferin"></a>
### TransferInPage

|Name|Type|Description|
|---|---|---|
|dataList|[[TransferIn](#schematransferin)]|Data list|
|nextPageOffsetData|string|Offset to retrieve the next page. If no next page data, the value will be an empty string|

<a id="schematransferin"></a>
### TransferInRecord

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Transfer In order ID|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Coin ID|
|amount|string|Transfer amount|
|senderAccountId|string(int64)|Sender Account ID|
|senderL2Key|string|Sender account L2 key. bigint for hex str|
|senderTransferOutId|string(int64)|Sender transfer out order ID|
|clientTransferId|string|Client defined ID. Used for idempotent checks and signature generation nonce|
|isConditionTransfer|boolean|Whether it is a conditional transfer|
|conditionFactRegistryAddress|string|Address of condition fact registry contract. Required when is_condition_transfer=true|
|conditionFactErc20Address|string|ERC20 address used to generate the condition fact. Required when is_conditional_transfer=true|
|conditionFactAmount|string|Amount used to generate condition fact. Required when is_conditional_transfer=true.|
|conditionFact|string|The conditional transfer fact. Required when is_condition_transfer=true|
|transferReason|string|Transfer reason|
|extraType|string|Additional type. Used by upper layer business|
|extraDataJson|string|Additional data in JSON format. Defaults to empty string|
|status|string|Transfer status|
|collateralTransactionId|string(int64)|ID of related collateral detail. Exists when status=SUCCESS_XXX/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED|
|censorTxId|string(int64)|Censor processing sequence. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED|
|censorTime|string(int64)|Censor processing time. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED|
|censorFailCode|string|Censor failure error code. Exists when status=FAILED_CENSOR_FAILURE|
|censorFailReason|string|Censor failure reason. Exists when status=FAILED_CENSOR_FAILURE|
|l2TxId|string(int64)|L2 push transaction ID. Exists when censor_status=CENSOR_SUCCESS/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED|
|l2RejectTime|string(int64)|L2 rejection time. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED|
|l2RejectCode|string|L2 rejection error code. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED|
|l2RejectReason|string|L2 rejection reason. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED|
|l2ApprovedTime|string(int64)|L2 batch verification time. Exists when status=L2_APPROVED/L2_REJECT_APPROVED|
|createdTime|string(int64)|Creation time|
|updatedTime|string(int64)|Update time|

#### Enum Values

| Property | Value |
|---|---|
| transferReason | UNKNOWN_TRANSFER_REASON |
| transferReason | USER_TRANSFER |
| transferReason | FAST_WITHDRAW |
| transferReason | CROSS_DEPOSIT |
| transferReason | CROSS_WITHDRAW |
| transferReason | UNRECOGNIZED |
| status | UNKNOWN_TRANSFER_STATUS |
| status | PENDING_CHECKING |
| status | PENDING_CENSORING |
| status | SUCCESS_CENSOR_SUCCESS |
| status | SUCCESS_L2_APPROVED |
| status | FAILED_CHECK_INVALID |
| status | FAILED_CENSOR_FAILURE |
| status | FAILED_L2_REJECT |
| status | FAILED_L2_REJECT_APPROVED |
| status | UNRECOGNIZED |


<a id="schemaresultpagedatatransferout"></a>
### TransferOutPageResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[PageDataTransferOut](#schemapagedatatransferout)|Generic paginated response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemapagedatatransferout"></a>
### TransferOutPage

|Name|Type|Description|
|---|---|---|
|dataList|[[TransferOut](#schematransferout)]|Data list|
|nextPageOffsetData|string|Offset to retrieve the next page. If no next page data, the value will be an empty string|


<a id="schematransferout"></a>
### TransferOutRecord

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Transfer out order ID|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Coin ID|
|amount|string|Transfer amount|
|receiverAccountId|string(int64)|Receiver Account ID|
|receiverL2Key|string|Receiver account L2 key. bigint for hex str|
|clientTransferId|string|Client defined ID. Used for idempotent checks and signature generation nonce|
|isConditionTransfer|boolean|Whether it is a conditional transfer|
|conditionFactRegistryAddress|string|Address of condition fact registry contract. Required when is_conditional_transfer=true|
|conditionFactErc20Address|string|ERC20 address used to generate the condition fact. Required when is_conditional_transfer=true|
|conditionFactAmount|string|Amount used to generate condition fact. Required when is_conditional_transfer=true.|
|conditionFact|string|The conditional transfer fact. Required when is_conditional_transfer=true|
|transferReason|string|Transfer reason|
|l2Nonce|string(int64)|L2 signature nonce. Take the first 32 bits of sha256(client_transfer_id)|
|l2ExpireTime|string(int64)|L2 signature expiration time in milliseconds. When generating/verifying the signature, the hour should be used: l2_expire_time / 3600000|
|l2Signature|[L2Signature](#schemal2signature)|L2 signature information|
|extraType|string|Additional type. Used by upper layer business|
|extraDataJson|string|Additional data in JSON format. Defaults to empty string|
|status|string|Transfer status|
|receiverTransferInId|string(int64)|ID of receiver transfer in order.|
|collateralTransactionId|string(int64)|ID of related collateral detail. Exists when status=SUCCESS_XXX/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED|
|censorTxId|string(int64)|Censor processing sequence. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED|
|censorTime|string(int64)|Censor processing time. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED|
|censorFailCode|string|Censor failure error code. Exists when status=FAILED_CENSOR_FAILURE|
|censorFailReason|string|Censor failure reason. Exists when status=FAILED_CENSOR_FAILURE|
|l2TxId|string(int64)|L2 push transaction ID. Exists when censor_status=CENSOR_SUCCESS/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED|
|l2RejectTime|string(int64)|L2 rejection time. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED|
|l2RejectCode|string|L2 rejection error code. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED|
|l2RejectReason|string|L2 rejection reason. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED|
|l2ApprovedTime|string(int64)|L2 batch verification time. Exists when status=L2_APPROVED/L2_REJECT_APPROVED|
|createdTime|string(int64)|Creation time|
|updatedTime|string(int64)|Update time|

#### Enum Values

| Property | Value |
|---|---|
| transferReason | UNKNOWN_TRANSFER_REASON |
| transferReason | USER_TRANSFER |
| transferReason | FAST_WITHDRAW |
| transferReason | CROSS_DEPOSIT |
| transferReason | CROSS_WITHDRAW |
| transferReason | UNRECOGNIZED |
| status | UNKNOWN_TRANSFER_STATUS |
| status | PENDING_CHECKING |
| status | PENDING_CENSORING |
| status | SUCCESS_CENSOR_SUCCESS |
| status | SUCCESS_L2_APPROVED |
| status | FAILED_CHECK_INVALID |
| status | FAILED_CENSOR_FAILURE |
| status | FAILED_L2_REJECT |
| status | FAILED_L2_REJECT_APPROVED |
| status | UNRECOGNIZED |

<a id="schemal2signature"></a>
### L2SignaturePayload

|Name|Type|Description|
|---|---|---|
|r|string|bigint for hex str|
|s|string|bigint for hex str|
|v|string|bigint for hex str|


<a id="schemaresultlisttransferin"></a>
### TransferInListResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[[TransferIn](#schematransferin)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemaresultgettransferoutavailableamount"></a>
### GetTransferOutAvailableAmountResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[GetTransferAvailableAmount](#schemagettransferavailableamount)|Get Transfer Available Amount - Response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemagettransferavailableamount"></a>
### TransferOutAvailableAmount

|Name|Type|Description|
|---|---|---|
|availableAmount|string(decimal)|Available amount|


<a id="schemaresultlisttransferout"></a>
### TransferOutListResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[[TransferOut](#schematransferout)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemaresultcreatetransferout"></a>
### CreateTransferOutResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[CreateTransferOut](#schemacreatetransferout)|Create Transfer Out Order - Response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemacreatetransferout"></a>
### CreateTransferOutResult

|Name|Type|Description|
|---|---|---|
|transferOutId|string(int64)|Transfer out order ID|



<a id="schemacreatetransferoutparam"></a>
### CreateTransferOutRequest

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|**Required** - Account ID (will throw exception if empty)|
|coinId|string(int64)|Yes|**Required** - Asset ID (will throw exception if empty)|
|amount|string|Yes|**Required** - Transfer amount (will throw exception if empty)|
|receiverAccountId|string|Yes|Receiver account ID (business required but HTTP layer accepts empty)|
|clientTransferId|string|Yes|Client defined ID. Used for idempotent checks and signature generation nonce|
|transferReason|string|Yes|Transfer reason. Defaults to UNKNOWN_TRANSFER_REASON if not provided|
|l2Nonce|string(int64)|Yes|L2 signature nonce. Take the first 32 bits of sha256(client_transfer_id)|
|l2ExpireTime|string(int64)|Yes|L2 signature expiration time in milliseconds. When generating/verifying the signature, the hour should be used: l2_expire_time / 3600000|
|l2Signature|string|Yes|L2 signature|
|extraType|string|No|Additional type. Used by upper layer business|
|extraDataJson|string|No|Additional data in JSON format. Defaults to empty string|

#### Enum Values

| Property | Value |
|---|---|
| transferReason | UNKNOWN_TRANSFER_REASON |
| transferReason | USER_TRANSFER |
| transferReason | FAST_WITHDRAW |
| transferReason | CROSS_DEPOSIT |
| transferReason | CROSS_WITHDRAW |
| transferReason | UNRECOGNIZED |

<a id="schemaresultgetbatchtransferoutavailableamount"></a>
### GetBatchTransferOutAvailableAmountResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, other values indicate failure.|
|data|[GetBatchTransferOutAvailableAmount](#schemagetbatchtransferoutavailableamount)|Get Batch Transfer Out Available Amount - Response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request receiving timestamp|
|responseTime|string(timestamp)|Server response returning timestamp|
|traceId|string|Call trace ID|


<a id="schemagetbatchtransferoutavailableamount"></a>
### BatchTransferOutAvailableAmount

|Name|Type|Description|
|---|---|---|
|accountID2trnasferOutAvaliableAmountMap|object|Batch query result (accountId -> availableAmount)|
|totalCount|integer|Total number of accounts queried|
|successCount|integer|Number of successful queries|
|failedCount|integer|Number of failed queries|
