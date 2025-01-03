# TransferPrivateApi

<a id="opIdcreateTransferOut"></a>

## POST Create Transfer Out Order

POST /api/v1/private/transfer/createTransferOut

> Body Request Parameters

```json
{
    "accountId": "543429922991899150",
    "coinId": "1000",
    "amount": "1.000000",
    "receiverAccountId": "551109015904453258",
    "receiverL2Key": "0x03eec711e360695bb44b1170057a25340303c1f16893a8def7450e44294405a8",
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

| Name | Location | Type | Required | Description |
|---|---|---|---|---|
| body | body | [CreateTransferOutParam](#schemacreatetransferoutparam) | No | none |

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
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultcreatetransferout) |

<a id="opIdgetTransferOutById"></a>

## GET Get Transfer Out Orders by ID

GET /api/v1/private/transfer/getTransferOutById

### Request Parameters

| Name | Location | Type | Required | Description |
|---|---|---|---|---|
| accountId | query | string | No | Account ID |
| transferOutIdList | query | string | No | Transfer out ID |

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
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultlisttransferout) |


<a id="opIdgetWithdrawAvailableAmount_1"></a>

## GET Get Available Withdrawal Amount

GET /api/v1/private/transfer/getTransferOutAvailableAmount

### Request Parameters

| Name | Location | Type | Required | Description |
|---|---|---|---|---|
| accountId | query | string | No | Account ID |
| coinId | query | string | No | Coin ID |

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
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultgettransferoutavailableamount) |

<a id="opIdgetTransferInById"></a>

## GET Get Transfer In Orders by ID

GET /api/v1/private/transfer/getTransferInById

### Request Parameters

| Name | Location | Type | Required | Description |
|---|---|---|---|---|
| accountId | query | string | No | Account ID |
| transferInIdList | query | string | No | Transfer In ID |

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
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultlisttransferin) |


# Data Models


<a id="schemaresultpagedatatransferin"></a>
### schemaresultpagedatatransferin

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| code | string | false | none | Status code. "SUCCESS" for success, other values indicate failure. |
| data | [PageDataTransferIn](#schemapagedatatransferin) | false | none | Generic paginated response data |
| errorParam | object | false | none | Parameter information in error messages |
| requestTime | string(timestamp) | false | none | Server request receiving timestamp |
| responseTime | string(timestamp) | false | none | Server response returning timestamp |
| traceId | string | false | none | Call trace ID |


<a id="schemapagedatatransferin"></a>
### schemapagedatatransferin

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| dataList | [[TransferIn](#schematransferin)] | false | none | Data list |
| nextPageOffsetData | string | false | none | Offset to retrieve the next page. If no next page data, the value will be an empty string |

<a id="schematransferin"></a>
### schematransferin

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| id | string(int64) | false | none | Transfer In order ID |
| userId | string(int64) | false | none | User ID |
| accountId | string(int64) | false | none | Account ID |
| coinId | string(int64) | false | none | Coin ID |
| amount | string | false | none | Transfer amount |
| senderAccountId | string(int64) | false | none | Sender Account ID |
| senderL2Key | string | false | none | Sender account L2 key. bigint for hex str |
| senderTransferOutId | string(int64) | false | none | Sender transfer out order ID |
| clientTransferId | string | false | none | Client defined ID. Used for idempotent checks and signature generation nonce |
| isConditionTransfer | boolean | false | none | Whether it is a conditional transfer |
| conditionFactRegistryAddress | string | false | none | Address of condition fact registry contract. Required when is_condition_transfer=true |
| conditionFactErc20Address | string | false | none | ERC20 address used to generate the condition fact. Required when is_conditional_transfer=true |
| conditionFactAmount | string | false | none | Amount used to generate condition fact. Required when is_conditional_transfer=true. |
| conditionFact | string | false | none | The conditional transfer fact. Required when is_condition_transfer=true |
| transferReason | string | false | none | Transfer reason |
| extraType | string | false | none | Additional type. Used by upper layer business |
| extraDataJson | string | false | none | Additional data in JSON format. Defaults to empty string |
| status | string | false | none | Transfer status |
| collateralTransactionId | string(int64) | false | none | ID of related collateral detail. Exists when status=SUCCESS_XXX/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorTxId | string(int64) | false | none | Censor processing sequence. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorTime | string(int64) | false | none | Censor processing time. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorFailCode | string | false | none | Censor failure error code. Exists when status=FAILED_CENSOR_FAILURE |
| censorFailReason | string | false | none | Censor failure reason. Exists when status=FAILED_CENSOR_FAILURE |
| l2TxId | string(int64) | false | none | L2 push transaction ID. Exists when censor_status=CENSOR_SUCCESS/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED |
| l2RejectTime | string(int64) | false | none | L2 rejection time. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED |
| l2RejectCode | string | false | none | L2 rejection error code. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED |
| l2RejectReason | string | false | none | L2 rejection reason. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED |
| l2ApprovedTime | string(int64) | false | none | L2 batch verification time. Exists when status=L2_APPROVED/L2_REJECT_APPROVED |
| createdTime | string(int64) | false | none | Creation time |
| updatedTime | string(int64) | false | none | Update time |

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
### schemaresultpagedatatransferout

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| code | string | false | none | Status code. "SUCCESS" for success, other values indicate failure. |
| data | [PageDataTransferOut](#schemapagedatatransferout) | false | none | Generic paginated response data |
| errorParam | object | false | none | Parameter information in error messages |
| requestTime | string(timestamp) | false | none | Server request receiving timestamp |
| responseTime | string(timestamp) | false | none | Server response returning timestamp |
| traceId | string | false | none | Call trace ID |


<a id="schemapagedatatransferout"></a>
### schemapagedatatransferout

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| dataList | [[TransferOut](#schematransferout)] | false | none | Data list |
| nextPageOffsetData | string | false | none | Offset to retrieve the next page. If no next page data, the value will be an empty string |


<a id="schematransferout"></a>
### schematransferout

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| id | string(int64) | false | none | Transfer out order ID |
| userId | string(int64) | false | none | User ID |
| accountId | string(int64) | false | none | Account ID |
| coinId | string(int64) | false | none | Coin ID |
| amount | string | false | none | Transfer amount |
| receiverAccountId | string(int64) | false | none | Receiver Account ID |
| receiverL2Key | string | false | none | Receiver account L2 key. bigint for hex str |
| clientTransferId | string | false | none | Client defined ID. Used for idempotent checks and signature generation nonce |
| isConditionTransfer | boolean | false | none | Whether it is a conditional transfer |
| conditionFactRegistryAddress | string | false | none | Address of condition fact registry contract. Required when is_conditional_transfer=true |
| conditionFactErc20Address | string | false | none | ERC20 address used to generate the condition fact. Required when is_conditional_transfer=true |
| conditionFactAmount | string | false | none | Amount used to generate condition fact. Required when is_conditional_transfer=true. |
| conditionFact | string | false | none | The conditional transfer fact. Required when is_conditional_transfer=true |
| transferReason | string | false | none | Transfer reason |
| l2Nonce | string(int64) | false | none | L2 signature nonce. Take the first 32 bits of sha256(client_transfer_id) |
| l2ExpireTime | string(int64) | false | none | L2 signature expiration time in milliseconds. When generating/verifying the signature, the hour should be used: l2_expire_time / 3600000 |
| l2Signature | [L2Signature](#schemal2signature) | false | none | L2 signature information |
| extraType | string | false | none | Additional type. Used by upper layer business |
| extraDataJson | string | false | none | Additional data in JSON format. Defaults to empty string |
| status | string | false | none | Transfer status |
| receiverTransferInId | string(int64) | false | none | ID of receiver transfer in order. |
| collateralTransactionId | string(int64) | false | none | ID of related collateral detail. Exists when status=SUCCESS_XXX/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorTxId | string(int64) | false | none | Censor processing sequence. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorTime | string(int64) | false | none | Censor processing time. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECT/FAILED_L2_REJECT_APPROVED |
| censorFailCode | string | false | none | Censor failure error code. Exists when status=FAILED_CENSOR_FAILURE |
| censorFailReason | string | false | none | Censor failure reason. Exists when status=FAILED_CENSOR_FAILURE |
| l2TxId | string(int64) | false | none | L2 push transaction ID. Exists when censor_status=CENSOR_SUCCESS/L2_APPROVED/L2_REJECT/L2_REJECT_APPROVED |
| l2RejectTime | string(int64) | false | none | L2 rejection time. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED |
| l2RejectCode | string | false | none | L2 rejection error code. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED |
| l2RejectReason | string | false | none | L2 rejection reason. Exists when censor_status=L2_REJECT/L2_REJECT_APPROVED |
| l2ApprovedTime | string(int64) | false | none | L2 batch verification time. Exists when status=L2_APPROVED/L2_REJECT_APPROVED |
| createdTime | string(int64) | false | none | Creation time |
| updatedTime | string(int64) | false | none | Update time |

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
### schemal2signature

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| r | string | false | none | bigint for hex str |
| s | string | false | none | bigint for hex str |
| v | string | false | none | bigint for hex str |


<a id="schemaresultlisttransferin"></a>
### schemaresultlisttransferin

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| code | string | false | none | Status code. "SUCCESS" for success, other values indicate failure. |
| data | [[TransferIn](#schematransferin)] | false | none | Correct response data |
| errorParam | object | false | none | Parameter information in error messages |
| requestTime | string(timestamp) | false | none | Server request receiving timestamp |
| responseTime | string(timestamp) | false | none | Server response returning timestamp |
| traceId | string | false | none | Call trace ID |


<a id="schemaresultgettransferoutavailableamount"></a>
### schemaresultgettransferoutavailableamount

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| code | string | false | none | Status code. "SUCCESS" for success, other values indicate failure. |
| data | [GetTransferAvailableAmount](#schemagettransferavailableamount) | false | none | Get Transfer Available Amount - Response |
| errorParam | object | false | none | Parameter information in error messages |
| requestTime | string(timestamp) | false | none | Server request receiving timestamp |
| responseTime | string(timestamp) | false | none | Server response returning timestamp |
| traceId | string | false | none | Call trace ID |


<a id="schemagettransferavailableamount"></a>
### schemagettransferavailableamount

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| availableAmount | string(decimal) | false | none | Available amount |


<a id="schemaresultlisttransferout"></a>
### schemaresultlisttransferout

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| code | string | false | none | Status code. "SUCCESS" for success, other values indicate failure. |
| data | [[TransferOut](#schematransferout)] | false | none | Correct response data |
| errorParam | object | false | none | Parameter information in error messages |
| requestTime | string(timestamp) | false | none | Server request receiving timestamp |
| responseTime | string(timestamp) | false | none | Server response returning timestamp |
| traceId | string | false | none | Call trace ID |


<a id="schemaresultcreatetransferout"></a>
### schemaresultcreatetransferout

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| code | string | false | none | Status code. "SUCCESS" for success, other values indicate failure. |
| data | [CreateTransferOut](#schemacreatetransferout) | false | none | Create Transfer Out Order - Response |
| errorParam | object | false | none | Parameter information in error messages |
| requestTime | string(timestamp) | false | none | Server request receiving timestamp |
| responseTime | string(timestamp) | false | none | Server response returning timestamp |
| traceId | string | false | none | Call trace ID |


<a id="schemacreatetransferout"></a>
### schemacreatetransferout

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| transferOutId | string(int64) | false | none | Transfer out order ID |



<a id="schemacreatetransferoutparam"></a>
### schemacreatetransferoutparam

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| accountId | string(int64) | false | none | Account ID |
| coinId | string(int64) | false | none | Asset ID |
| amount | string | false | none | Transfer amount |
| receiverAccountId | string | false | none | Receiver account ID |
| receiverL2Key | string | false | none | Receiver account L2 key. bigint for hex str |
| clientTransferId | string | false | none | Client defined ID. Used for idempotent checks and signature generation nonce |
| transferReason | string | false | none | Transfer reason |
| l2Nonce | string(int64) | false | none | L2 signature nonce. Take the first 32 bits of sha256(client_withdraw_id) |
| l2ExpireTime | string(int64) | false | none | L2 signature expiration time in milliseconds. When generating/verifying the signature, the hour should be used: l2_expire_time / 3600000 |
| l2Signature | string | false | none | L2 signature |
| extraType | string | false | none | Additional type. Used by upper layer business |
| extraDataJson | string | false | none | Additional data in JSON format. Defaults to empty string |

#### Enum Values

| Property | Value |
|---|---|
| transferReason | UNKNOWN_TRANSFER_REASON |
| transferReason | USER_TRANSFER |
| transferReason | FAST_WITHDRAW |
| transferReason | CROSS_DEPOSIT |
| transferReason | CROSS_WITHDRAW |
| transferReason | UNRECOGNIZED |
