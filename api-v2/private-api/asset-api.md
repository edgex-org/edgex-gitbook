# Asset Private API

## When to Use This Page

Use this page when you need to create withdrawal orders, query withdrawable balances, or inspect deposit and withdrawal history for an account.

## Minimal Workflow

1. Query withdrawable amount or cross-chain signing info when needed.
2. Build the withdrawal request with the exact `l2*` fields required by the endpoint.
3. Submit the withdrawal order.
4. Poll the order-history endpoint to track status changes.

## Common Notes

- These endpoints use the same private REST authentication described in [Authentication](../authentication.md).
- If an endpoint also requires an L2 signature, keep the backend field names exactly as returned, including `l2Nonce`, `l2ExpireTime`, and `l2Signature`.
- For client-defined IDs, keep your generation rule stable so retries remain idempotent.

<a id="opIdcreateNormalWithdraw"></a>

## POST Create Normal Withdrawal Order

POST /api/v2/private/assets/createNormalWithdraw

**Note**: Asset withdrawal endpoints use the same private REST authentication described in [Authentication](../authentication.md). If an endpoint also requires an L2 signature, prefer the `l2*` field names used across V2 documentation.

> Body Request Parameters

```json
{
    "accountId": "551109015904453258",
    "coinId": "1000",
    "amount": "1.000000",
    "fee": "0.5",
    "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
    "clientWithdrawId": "745410645654877",
    "l2Signature": "0x007bf80407c6a7bb14f5ca3b848a5d908627993f23b073c902e359a6fa4a6a92040cea4c98e25e35ad1d8cc4e18758c463c45bf451299ce55aa49abbdb916d03",
    "signer": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
    "l2Nonce": 123456,
    "l2ExpireTime": "1735887600000"
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|[CreateNormalWithdrawParam](#schemacreatenormalwithdrawparam)|No|Request body for creating a normal withdrawal order.|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "withdrawId": "1054639949233524736"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734674558154",
    "responseTime": "1734674558171",
    "traceId": "9e3bfe2b9e1ef82583cb96f36e43e537"
}
```

### Response

| Status Code | Status Code Description | Description      | Data Model |
|-------------|-------------------------|------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Successful response | [Result](#createnormalwithdraw) |

<a id="opIdcreateCrossWithdraw"></a>

## POST Create Cross-Chain Withdrawal Order

POST /api/v2/private/assets/createCrossWithdraw

> Body Request Parameters

```json
{
  "accountId": "string",
  "coinId": "string",
  "amount": "string",
  "ethAddress": "string",
  "erc20Address": "string",
  "lpAccountId": "string",
  "clientCrossWithdrawId": "string",
  "expireTime": "string",
  "l2Signature": "string",
  "fee": "string",
  "chainId": "string",
  "mpcAddress": "string",
  "mpcSignature": "string",
  "mpcSignTime": "string"
}
```

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|body|[CreateCrossWithdrawParam](#schemacreatecrosswithdrawparam)|No|Request body for creating a cross-chain withdrawal order.|

> Response Example

> 200 Response

```json
{
  "code": "string",
  "msg": "string",
  "requestTime": "string",
  "responseTime": "string"
}
```

### Response

| Status Code | Status Code Description | Description      | Data Model |
|-------------|-------------------------|------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Successful response | [Result](#schemaresultcreatecrosswithdraw) |

<a id="opIdgetNormalWithdrawableAmount"></a>

## GET User's Normal Withdrawable Amount

GET /api/v2/private/assets/getNormalWithdrawableAmount

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|address|string|Yes|User address|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "amount": "0"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734674463329",
    "responseTime": "1734674464181",
    "traceId": "6e8a3b8326f00683cf73f701f3edcfb6"
}
```

### Response

| Status Code | Status Code Description | Description      | Data Model |
|-------------|-------------------------|------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Successful response | [Result](#schemaresultgetnormalwithdrawableamount) |

<a id="opIdgetCrossWithdrawSignInfo"></a>

## GET Get Information Required for Cross-Chain Withdrawal Signature

GET /api/v2/private/assets/getCrossWithdrawSignInfo

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|chainId|string|No|Chain ID|
|amount|string|No|Withdrawal amount|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "lpAccountId": "542076087396467085",
        "crossWithdrawL2Key": "0x03bf794b4433e0a8b353da361bb7284c670914d27ed04698e6abed0bf1198028",
        "crossWithdrawMaxAmount": "48799.686154",
        "fee": "2"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734674557578",
    "responseTime": "1734674557997",
    "traceId": "3aa3d5c94c7bc9aef69f590e188058ef"
}
```

### Response

| Status Code | Status Code Description | Description      | Data Model |
|-------------|-------------------------|------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Successful response | [Result](#schemaresultgetcrosswithdrawsigninfo) |

<a id="opIdgetAllOrdersPage"></a>

## GET Aggregate Query of All Deposit and Withdrawal Order Records

GET /api/v2/private/assets/getAllOrdersPage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string|Yes|Account ID|
|startTime|string|No|Start time, Unix time in seconds|
|endTime|string|No|End time, Unix time in seconds|
|chainId|string|No|Chain ID|
|typeList|string|No|Order type list|
|size|string|Yes|Number of items per page. Must be > 0 and <= 100.|
|offsetData|string|No|Offset for page retrieval. If not provided, returns first page|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "orderId": "1054639949233524736",
                "time": "1734674558",
                "type": "ORDER_TYPE_NORMAL_WITHDRAW",
                "status": 3,
                "amount": "1",
                "fee": "",
                "txId": "",
                "chain": "Sepolia - Testnet",
                "address": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
                "coin": "USDT",
                "chainId": "11155111",
                "transferSenderAccountId": "0",
                "transferReceiverAccountId": "0"
            }
        ],
        "nextPageOffsetData": "b3fa59aa-8b42-49a3-9729-d7e89d8d9c8d"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734675194541",
    "responseTime": "1734675194553",
    "traceId": "d6f1fe9e521e306a49f30158257a07a2"
}
```

### Response

| Status Code | Status Code Description | Description      | Data Model |
|-------------|-------------------------|------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Successful response | [Result](#schemaresultpagedatassetorder) |

## Data Models

<a id="schemaresultpagedatassetorder"></a>
### AssetOrderPageResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[PageDataAssetOrder](#schemapagedataassetorder)|Paginated asset-order data.|
|errorParam|object|Structured error details returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|


<a id="schemapagedataassetorder"></a>
### AssetOrderPage

|Name|Type|Description|
|---|---|---|
|dataList|[[AssetOrder](#schemaassetorder)]|Asset-order records for the current page.|
|nextPageOffsetData|string|Offset token for the next page. Empty when there is no next page.|


<a id="schemaassetorder"></a>

### AssetOrderRecord

|Name|Type|Description|
|---|---|---|
|orderId|string(int64)|Order ID|
|time|string(int64)|Order creation time|
|type|string|Order type|
|status|integer(int32)|Order status|
|amount|string|Order amount|
|fee|string|Order fee|
|txId|string|Chain transaction ID.|
|chain|string|Chain|
|address|string|Address|
|coin|string|Coin|
|chainId|string|Chain ID|
|transferSenderAccountId|string|Transfer out account ID|
|transferReceiverAccountId|string|Transfer in account ID|

#### Enum Values

| Property | Value                     |
|----------|---------------------------|
| type     | UNKNOWN_ORDER_TYPE        |
| type     | ORDER_TYPE_NORMAL_DEPOSIT |
| type     | ORDER_TYPE_CROSS_DEPOSIT  |
| type     | ORDER_TYPE_NORMAL_WITHDRAW|
| type     | ORDER_TYPE_CROSS_WITHDRAW |
| type     | ORDER_TYPE_FAST_WITHDRAW  |
| type     | ORDER_TYPE_TRANSFER_IN    |
| type     | ORDER_TYPE_TRANSFER_OUT   |
| type     | UNRECOGNIZED            |



<a id="schemaresultlistcrosswithdraw"></a>

### CrossWithdrawListResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[[CrossWithdraw](#schemacrosswithdraw)]|Cross-chain withdrawal records returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|


<a id="schemacrosswithdraw"></a>

### CrossWithdrawRecord

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Withdrawal order ID|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral coin ID|
|amount|string|Withdrawal amount|
|ethAddress|string|ETH address for withdrawal, may differ from the account's ETH address.|
|erc20Address|string|L1 ERC20 contract address for the withdrawn asset|
|lpAccountId|string(int64)|LP account ID for L2 receiving user transfers|
|lpAccountL2Key|string(int64)|L2 key for the receiving account|
|clientCrossWithdrawId|string|Client-defined ID for idempotent checks|
|fee|string|Transaction fee|
|chainId|string|Chain ID for withdrawal|
|l2Nonce|string(int64)|L2 signature nonce.  First 32 bits of sha256(client_withdraw_id)|
|l2ExpireTime|string(int64)|L2 signature expiration time. Unix time in hours, must be at least 24 hours after order creation.|
|l2Signature|[L2Signature](#schemal2signature)|L2 signature information|
|extraType|string|Optional business-specific type used by upstream services.|
|extraDataJson|string|Optional extra metadata in JSON format. Empty string when unused.|
|status|string|Normal withdrawal order status|
|collateralTransactionId|string|Related collateral detail ID.  Exists when status=SUCCESS_XXX/FAILED_L2_REJECTED|
|censorTxId|string(int64)|Censorship processing sequence number. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECTED|
|censorTime|string(int64)|Censorship processing time. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECTED|
|censorFailCode|string|Censorship failure error code. Exists when status=FAILED_CENSOR_FAILURE|
|censorFailReason|string|Censorship failure reason. Exists when status=FAILED_CENSOR_FAILURE|
|l2TxId|string(int64)|L2 transaction ID.  Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECTED|
|l2HandleTime|string(int64)|L2 processing time.  Exists when status=SUCCESS_L1_CONFIRMING/SUCCESS_L1_WITHDRAWING/SUCCESS_L1_COMPLETED/FAILED_L2_REJECTED|
|l2RejectCode|string|L2 reject error code. Exists when status=FAILED_L2_REJECTED|
|l2RejectReason|string|L2 reject reason. Exists when status=FAILED_L2_REJECTED|
|l1ConfirmedTx|[L1Tx](#schemal1tx)|L1 transaction information|
|l1ConfirmedTime|string(int64)|L1 transaction confirmation time|
|l1CompletedTx|[L1Tx](#schemal1tx)|L1 transaction information|
|l1CompletedEthAddress|string|L1 withdrawal completion ETH address|
|l1CompletedTime|string(int64)|L1 withdrawal completion time|
|l1RejectedReasonCode|string|L1 rejection reason code|
|l1RejectedReasonMsg|string|L1 rejection reason message|
|riskSignature|[L2Signature](#schemal2signature)|L2 signature information|
|transferOutId|string(int64)|Transfer out order ID|
|createdTime|string(int64)|Creation time|
|updatedTime|string(int64)|Update time|

#### Enum Values

| Property | Value                          |
|----------|--------------------------------|
| status   | CROSS_WITHDRAW_UNKNOWN         |
| status   | CROSS_WITHDRAW_PENDING_RISK_CHECKING|
| status   | CROSS_WITHDRAW_PENDING_CHECKING |
| status   | CROSS_WITHDRAW_SUCCESS_SUBMIT_CENSOR    |
| status   | CROSS_WITHDRAW_PENDING_CENSOR_CHECKING_ACCOUNT |
| status   | CROSS_WITHDRAW_PENDING_CENSORING    |
| status   | CROSS_WITHDRAW_PENDING_L2_APPROVING |
| status   | CROSS_WITHDRAW_PENDING_L1_SUBMIT  |
| status   | CROSS_WITHDRAW_PENDING_L1_CONFIRMING|
| status   | CROSS_WITHDRAW_SUCCESS            |
| status   | CROSS_WITHDRAW_FAILED_RISK_CHECK_FAILURE    |
| status   | CROSS_WITHDRAW_FAILED_TRANSFER_REJECTED     |
| status   | CROSS_WITHDRAW_FAILED_CENSOR_CHECKING_ACCOUNT_REJECTED  |
| status   | CROSS_WITHDRAW_FAILED_CENSORING    |
| status   | CROSS_WITHDRAW_FAILED_L2_REJECTED |
| status   | CROSS_WITHDRAW_FAILED_L1_SUBMIT_REJECTED  |
| status   | CROSS_WITHDRAW_FAILED_L1_REJECTED |
| status   | CROSS_WITHDRAW_FAILED_USER_BALANCE_NOT_ENOUGH |
| status   | UNRECOGNIZED                  |

<a id="schemal1tx"></a>

### L1TransactionRecord

|Name|Type|Description|
|---|---|---|
|hash|string|Transaction hash|
|index|integer(int32)|Index of the tx hash|
|time|string(int64)|Tx chain timestamp, in milliseconds|
|blockHeight|string(int64)|Block height of the tx|


<a id="schemal2signature"></a>

### L2SignaturePayload

|Name|Type|Description|
|---|---|---|
|r|string|Bigint for hex string|
|s|string|Bigint for hex string|
|v|string|Bigint for hex string|

<a id="schemaresultgetcrosswithdrawsigninfo"></a>

### GetCrossWithdrawSignInfoResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[GetCrossWithdrawSignInfo](#schemagetcrosswithdrawsigninfo)|Signing parameters required before creating a cross-chain withdrawal.|
|errorParam|object|Structured error details returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|


<a id="schemagetcrosswithdrawsigninfo"></a>
### GetCrossWithdrawSignInfoPayload

|Name|Type|Description|
|---|---|---|
|lpAccountId|string|LP account ID for L2 receiving user transfers|
|crossWithdrawL2Key|string|L2 key for fast withdrawal account|
|crossWithdrawMaxAmount|string|Maximum amount for fast cross-chain withdrawal|
|fee|string|Transaction fee|




<a id="schemaresultlistnormalwithdraw"></a>

### NormalWithdrawListResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[[NormalWithdraw](#schemanormalwithdraw)]|Normal withdrawal records returned by the server.|
|errorParam|object|Structured error details returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|


<a id="schemanormalwithdraw"></a>

### NormalWithdrawRecord

|Name|Type|Description|
|---|---|---|
|id|string(int64)|Withdrawal order ID|
|userId|string(int64)|User ID|
|accountId|string(int64)|Account ID|
|coinId|string(int64)|Collateral coin ID|
|amount|string|Withdrawal amount|
|ethAddress|string|ETH address for withdrawal, may differ from the account's ETH address.|
|clientWithdrawId|string|Client-defined ID for idempotent checks|
|l2Nonce|string(int64)|L2 signature nonce. First 32 bits of sha256(client_withdraw_id)|
|l2ExpireTime|string(int64)|L2 signature expiration time. Unix time in hours, must be at least 24 hours after order creation.|
|l2Signature|[L2Signature](#schemal2signature)|L2 signature information|
|status|string|Normal withdrawal order status|
|tradeWithdrawId|string(int64)|Corresponding trading service withdraw order ID|
|riskSignature|[L2Signature](#schemal2signature)|L2 signature information|
|l1ConfirmedTx|[L1Tx](#schemal1tx)|L1 transaction information|
|l1ConfirmedTime|string(int64)|L1 transaction confirmation time|
|l1CompletedTime|string(int64)|L1 withdrawal completion time|
|createdTime|string(int64)|Creation time|
|updatedTime|string(int64)|Update time|

#### Enum Values

| Property | Value                              |
|----------|------------------------------------|
| status   | NORMAL_WITHDRAW_UNKNOWN           |
| status   | NORMAL_WITHDRAW_PENDING_RISK_CHECKING |
| status   | NORMAL_WITHDRAW_PENDING_TRADE_PROCESSING |
| status   | NORMAL_WITHDRAW_PENDING_L2_APPROVING |
| status   | NORMAL_WITHDRAW_PENDING_L1_CONFIRMING|
| status   | NORMAL_WITHDRAW_PENDING_L1_WITHDRAWING|
| status   | NORMAL_WITHDRAW_SUCCESS_L1_COMPLETED |
| status   | NORMAL_WITHDRAW_FAILED_RISK_CHECK_FAILURE   |
| status   | NORMAL_WITHDRAW_FAILED_CENSOR_FAILURE      |
| status   | NORMAL_WITHDRAW_FAILED_L2_REJECTED  |
| status   | UNRECOGNIZED                    |

<a id="schemaresultgetnormalwithdrawableamount"></a>

### GetNormalWithdrawableAmountResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[GetNormalWithdrawableAmount](#schemagetnormalwithdrawableamount)|Withdrawable amount available for the queried address.|
|errorParam|object|Structured error details returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|


<a id="schemagetnormalwithdrawableamount"></a>

### GetNormalWithdrawableAmountPayload

|Name|Type|Description|
|---|---|---|
|amount|string|Withdrawable amount|


<a id="schemaresultcreatecrosswithdraw"></a>

### CreateCrossWithdrawResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[CreateCrossWithdraw](#schemacreatecrosswithdraw)|Identifiers returned after creating a cross-chain withdrawal order.|
|errorParam|object|Structured error details returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|



<a id="schemacreatecrosswithdraw"></a>

### CreateCrossWithdrawPayload

|Name|Type|Description|
|---|---|---|
|crossWithdrawId|string(int64)|Cross-chain withdrawal order ID|


<a id="schemacreatecrosswithdrawparam"></a>

### CreateCrossWithdrawRequest

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|coinId|string(int64)|Yes|Coin ID|
|amount|string|Yes|Withdrawal amount|
|ethAddress|string|No|Withdrawal address. If empty, withdraw to the corresponding address of the current account.|
|erc20Address|string|No|L1 ERC20 contract address for the withdrawn asset|
|lpAccountId|string|No|LP account ID for L2 receiving user transfers|
|clientCrossWithdrawId|string|No|Client-defined ID, used for signature & idempotent check. Must be filled.|
|expireTime|string(int64)|No|Expiration time|
|l2Signature|string|No|L2 signature|
|fee|string|No|Gas + fee obtained from front-end|
|chainId|string|Yes|Chain ID for withdrawal|
|mpcAddress|string|No|Which mpc address initiated the withdraw|
|mpcSignature|string|No|Signature of the mpc address to the withdraw field|
|mpcSignTime|string|No|MPC signature timestamp in seconds.|



<a id="createnormalwithdraw"></a>

### CreateNormalWithdrawResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code|
|data|[CreateNormalWithdraw](#schemacreatenormalwithdraw)|Identifiers returned after creating a normal withdrawal order.|
|errorParam|object|Structured error details returned by the server.|
|requestTime|string(timestamp)|Timestamp when the server received the request.|
|responseTime|string(timestamp)|Timestamp when the server returned the response.|
|traceId|string|Call trace ID|



<a id="schemacreatenormalwithdraw"></a>

### CreateNormalWithdrawResult

|Name|Type|Description|
|---|---|---|
|withdrawId|string(int64)|Withdrawal order ID|


<a id="schemacreatenormalwithdrawparam"></a>

### CreateNormalWithdrawRequest

|Name|Type|Required|Description|
|---|---|---|---|
|accountId|string(int64)|Yes|Account ID|
|coinId|string(int64)|Yes|Coin ID|
|amount|string|Yes|Withdrawal amount|
|fee|string|Yes|Withdrawal fee|
|ethAddress|string|No|Withdrawal address. If empty, withdraw to the corresponding address of the current account.|
|clientWithdrawId|string|No|Client-defined ID, used for signature & idempotent check. Must be filled.|
|signature|string|No|EIP-712 signature required by the backend for V2 withdrawal requests.|
|signer|string|No|Signer address that produced the EIP-712 signature.|
|nonce|integer(int64)|No|Nonce used in the V2 signature payload.|
|l2ExpireTime|string(int64)|No|L2 signature expiration time (milliseconds)|
