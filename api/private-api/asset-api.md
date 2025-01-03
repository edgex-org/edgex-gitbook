# AssetsPrivateApi

<a id="opIdcreateNormalWithdraw"></a>

## POST Create Normal Withdrawal Order

POST /api/v1/private/assets/createNormalWithdraw

> Body Request Parameters

```json
{
    "accountId": "551109015904453258",
    "coinId": "1000",
    "amount": "1.000000",
    "ethAddress": "0x1fB51aa234287C3CA1F957eA9AD0E148Bb814b7A",
    "clientWithdrawId": "745410645654877",
    "expireTime": "1735887600000",
    "l2Signature": "007bf80407c6a7bb14f5ca3b848a5d908627993f23b073c902e359a6fa4a6a92040cea4c98e25e35ad1d8cc4e18758c463c45bf451299ce55aa49abbdb916d03"
}
```

### Request Parameters

| Name              | Location | Type                                 | Required | Description |
|-------------------|----------|--------------------------------------|----------|-------------|
| body              | body     | [CreateNormalWithdrawParam](#schemacreatenormalwithdrawparam) | No       | None        |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#createnormalwithdraw) |

<a id="opIdcreateCrossWithdraw"></a>

## POST Create Cross-Chain Withdrawal Order

POST /api/v1/private/assets/createCrossWithdraw

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

| Name              | Location | Type                                 | Required | Description |
|-------------------|----------|--------------------------------------|----------|-------------|
| body              | body     | [CreateCrossWithdrawParam](#schemacreatecrosswithdrawparam) | No       | None        |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#schemaresultcreatecrosswithdraw) |

<a id="opIdgetNormalWithdrawableAmount"></a>

## GET User's Normal Withdrawable Amount

GET /api/v1/private/assets/getNormalWithdrawableAmount

### Request Parameters

| Name    | Location | Type   | Required | Description |
|---------|----------|--------|----------|-------------|
| address | query    | string | Yes      | User address |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#schemaresultgetnormalwithdrawableamount) |

<a id="opIdgetNormalWithdrawById"></a>

## GET Get Normal Withdrawal Orders by Account ID and Withdrawal ID

GET /api/v1/private/assets/getNormalWithdrawById

### Request Parameters

| Name               | Location | Type   | Required | Description |
|--------------------|----------|--------|----------|-------------|
| accountId          | query    | string | No       | Account ID  |
| normalWithdrawIdList | query    | string | No       | Withdrawal ID |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#schemaresultlistnormalwithdraw) |


<a id="opIdgetCrossWithdrawSignInfo"></a>

## GET Get Information Required for Cross-Chain Withdrawal Signature

GET /api/v1/private/assets/getCrossWithdrawSignInfo

### Request Parameters

| Name    | Location | Type   | Required | Description |
|---------|----------|--------|----------|-------------|
| chainId | query    | string | No       | Chain ID    |
| amount  | query    | string | No       | Withdrawal amount  |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#schemaresultgetcrosswithdrawsigninfo) |

<a id="opIdgetCrossWithdrawById"></a>

## GET Get Cross-Chain Withdrawal Orders by Account ID and Withdrawal ID

GET /api/v1/private/assets/getCrossWithdrawById

### Request Parameters

| Name                 | Location | Type   | Required | Description |
|----------------------|----------|--------|----------|-------------|
| accountId            | query    | string | No       | Account ID  |
| crossWithdrawIdList  | query    | string | No       | Withdrawal ID |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#schemaresultlistcrosswithdraw) |


<a id="opIdgetAllOrdersPage"></a>

## GET Aggregate Query of All Deposit and Withdrawal Order Records

GET /api/v1/private/assets/getAllOrdersPage

### Request Parameters

| Name           | Location | Type   | Required | Description                                         |
|----------------|----------|--------|----------|-----------------------------------------------------|
| accountId      | query    | string | No       | Account ID                                          |
| startTime      | query    | string | No       | Start time, Unix time in seconds                   |
| endTime        | query    | string | No       | End time, Unix time in seconds                     |
| chainId        | query    | string | No       | Chain ID                                          |
| typeList       | query    | string | No       | Order type list                                 |
| size           | query    | string | No       | Number of items per page. Must be > 0 and <= 100. |
| offsetData     | query    | string | No       | Offset for page retrieval. If not provided, returns first page |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Default response | [Result](#schemaresultpagedatassetorder) |

# Data Models

<a id="schemaresultpagedatassetorder"></a>
### schemaresultpagedatassetorder

| Name             | Type                             | Required | Constraints | Description                  | Notes                                                                 |
|------------------|----------------------------------|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code               | Returns "SUCCESS" for success, otherwise it's a failure.                |
| data             | [PageDataAssetOrder](#schemapagedataassetorder) | false    | none        | Generic paginated return |                                                                       |
| errorParam       | object                           | false    | none        | Error parameter information  |                                                                       |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time    |                                                                       |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time   |                                                                       |
| traceId          | string                           | false    | none        | Call trace ID                  |                                                                       |


<a id="schemapagedataassetorder"></a>
### schemapagedataassetorder

| Name             | Type                                 | Required | Constraints | Description                   | Notes                                               |
|------------------|--------------------------------------|----------|-------------|-------------------------------|-----------------------------------------------------|
| dataList         | [[AssetOrder](#schemaassetorder)]    | false    | none        | List of data |                                                     |
| nextPageOffsetData | string                                 | false    | none        | Offset for next page | Empty string when there are no further pages. |


<a id="schemaassetorder"></a>

### schemaassetorder

| Name                  | Type            | Required | Constraints | Description                | Notes                                 |
|-----------------------|-----------------|----------|-------------|----------------------------|---------------------------------------|
| orderId               | string(int64)    | false    | none        | Order ID                    |                                       |
| time                  | string(int64)    | false    | none        | Order creation time         |                                       |
| type                  | string           | false    | none        | Order type                   |                                       |
| status                | integer(int32)  | false    | none        | Order status                |                                       |
| amount                | string           | false    | none        | Order amount                |                                       |
| fee                   | string           | false    | none        | Order fee                    |                                       |
| txId                  | string           | false    | none        | Chain tx_id                 |                                       |
| chain                 | string           | false    | none        | Chain                        |                                       |
| address               | string           | false    | none        | Address                      |                                       |
| coin                  | string           | false    | none        | Coin                       |                                       |
| chainId               | string           | false    | none        | Chain ID                  |                                       |
| transferSenderAccountId | string           | false    | none        | Transfer out account ID     |                                       |
| transferReceiverAccountId | string           | false    | none        | Transfer in account ID      |                                       |

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

### schemaresultlistcrosswithdraw

| Name             | Type                                   | Required | Constraints | Description          | Notes                                                                 |
|------------------|----------------------------------------|----------|-------------|----------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code          |
| data             | [[CrossWithdraw](#schemacrosswithdraw)] | false    | none        | Correct response data |                                                                      |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time   |                                                                      |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time    |                                                                       |
| traceId          | string                           | false    | none        | Call trace ID          |                                                                       |


<a id="schemacrosswithdraw"></a>

### schemacrosswithdraw

| Name                      | Type            | Required | Constraints | Description                               | Notes                                                                           |
|---------------------------|-----------------|----------|-------------|-------------------------------------------|---------------------------------------------------------------------------------|
| id                        | string(int64)    | false    | none        | Withdrawal order ID                         |                                                                                 |
| userId                    | string(int64)    | false    | none        | User ID                                  |                                                                                 |
| accountId                 | string(int64)    | false    | none        | Account ID                               |                                                                                 |
| coinId                    | string(int64)    | false    | none        | Collateral coin ID                        |                                                                                 |
| amount                    | string           | false    | none        | Withdrawal amount                         |                                                                                 |
| ethAddress                | string           | false    | none        | ETH address for withdrawal, may differ from the account's ETH address.        |                                                                    |
| erc20Address              | string           | false    | none        | L1 ERC20 contract address for the withdrawn asset |                                                                                  |
| lpAccountId               | string(int64)    | false    | none        | LP account ID for L2 receiving user transfers |                                                                                 |
| lpAccountL2Key            | string(int64)    | false    | none        | L2 key for the receiving account         |                                                                                 |
| clientCrossWithdrawId     | string           | false    | none        | Client-defined ID for idempotent checks   |                                                                                 |
| fee                       | string           | false    | none        | Transaction fee                             |                                                                                |
| chainId                   | string           | false    | none        | Chain ID for withdrawal                   |                                                                                 |
| l2Nonce                   | string(int64)    | false    | none        | L2 signature nonce.  First 32 bits of sha256(client_withdraw_id)|                                      |
| l2ExpireTime              | string(int64)    | false    | none        | L2 signature expiration time. Unix time in hours, must be at least 24 hours after order creation.        |                                      |
| l2Signature               | [L2Signature](#schemal2signature) | false    | none        | L2 signature information            |                                                                                |
| extraType                 | string           | false    | none        | Additional type for upper-layer business usage     |                                                                                 |
| extraDataJson             | string           | false    | none        | Extra data, JSON format, defaults to empty string.  |                                                                                 |
| status                    | string           | false    | none        | Normal withdrawal order status            |                                                                                 |
| collateralTransactionId    | string           | false    | none        | Related collateral detail ID.  Exists when status=SUCCESS_XXX/FAILED_L2_REJECTED|                         |
| censorTxId                | string(int64)    | false    | none        | Censorship processing sequence number. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECTED |          |
| censorTime                | string(int64)    | false    | none        | Censorship processing time. Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECTED |                                |
| censorFailCode             | string           | false    | none        | Censorship failure error code. Exists when status=FAILED_CENSOR_FAILURE |                             |
| censorFailReason           | string           | false    | none        | Censorship failure reason. Exists when status=FAILED_CENSOR_FAILURE |                                       |
| l2TxId                     | string(int64)    | false    | none        | L2 transaction ID.  Exists when status=SUCCESS_XXX/FAILED_CENSOR_FAILURE/FAILED_L2_REJECTED |                                                                                  |
| l2HandleTime             | string(int64)    | false    | none        | L2 processing time.  Exists when status=SUCCESS_L1_CONFIRMING/SUCCESS_L1_WITHDRAWING/SUCCESS_L1_COMPLETED/FAILED_L2_REJECTED |      |
| l2RejectCode             | string           | false    | none        | L2 reject error code. Exists when status=FAILED_L2_REJECTED|                                        |
| l2RejectReason           | string           | false    | none        | L2 reject reason. Exists when status=FAILED_L2_REJECTED|                                         |
| l1ConfirmedTx              | [L1Tx](#schemal1tx) | false    | none        | L1 transaction information           |                                                                          |
| l1ConfirmedTime            | string(int64)    | false    | none        | L1 transaction confirmation time        |                                                                       |
| l1CompletedTx              | [L1Tx](#schemal1tx) | false    | none        | L1 transaction information           |                                                                          |
| l1CompletedEthAddress     | string           | false    | none        | L1 withdrawal completion ETH address     |                                                                        |
| l1CompletedTime            | string(int64)    | false    | none        | L1 withdrawal completion time           |                                                                          |
| l1RejectedReasonCode       | string           | false    | none        | L1 rejection reason code                |                                                                          |
| l1RejectedReasonMsg        | string           | false    | none        | L1 rejection reason message             |                                                                         |
| riskSignature              | [L2Signature](#schemal2signature) | false    | none        | L2 signature information            |                                                                                |
| transferOutId             | string(int64)    | false    | none        | Transfer out order ID                 |                                                                               |
| createdTime               | string(int64)    | false    | none        | Creation time                         |                                                                        |
| updatedTime               | string(int64)    | false    | none        | Update time                           |                                                                        |

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

### schemal1tx

| Name        | Type            | Required | Constraints | Description                 | Notes                                 |
|-------------|-----------------|----------|-------------|-----------------------------|---------------------------------------|
| hash        | string          | false    | none        | Transaction hash            |                                       |
| index       | integer(int32)  | false    | none        | Index of the tx hash       |                                       |
| time        | string(int64)    | false    | none        | Tx chain timestamp, in milliseconds       |                                  |
| blockHeight | string(int64)    | false    | none        | Block height of the tx       |                                       |


<a id="schemal2signature"></a>

### schemal2signature

| Name | Type   | Required | Constraints | Description           | Notes              |
|------|--------|----------|-------------|-----------------------|--------------------|
| r    | string | false    | none        | Bigint for hex string |                    |
| s    | string | false    | none        | Bigint for hex string |                    |
| v    | string | false    | none        | Bigint for hex string |                    |

<a id="schemaresultgetcrosswithdrawsigninfo"></a>

### schemaresultgetcrosswithdrawsigninfo

| Name             | Type                                        | Required | Constraints | Description                               | Notes                                                                 |
|------------------|---------------------------------------------|----------|-------------|-------------------------------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code                               | Returns "SUCCESS" for success, otherwise it's a failure.                |
| data             | [GetCrossWithdrawSignInfo](#schemagetcrosswithdrawsigninfo) | false    | none        | Get information required for cross-chain withdrawal signature - Response|                                                                      |
| errorParam       | object                           | false    | none        | Error parameter information            |                                                                       |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time               |                                                                       |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time              |                                                                       |
| traceId          | string                           | false    | none        | Call trace ID                              |                                                                       |


<a id="schemagetcrosswithdrawsigninfo"></a>
### schemagetcrosswithdrawsigninfo

| Name                 | Type   | Required | Constraints | Description                                  | Notes                                  |
|----------------------|--------|----------|-------------|----------------------------------------------|----------------------------------------|
| lpAccountId          | string | false    | none        | LP account ID for L2 receiving user transfers |                                        |
| crossWithdrawL2Key  | string | false    | none        | L2 key for fast withdrawal account      |                                        |
| crossWithdrawMaxAmount| string | false    | none        | Maximum amount for fast cross-chain withdrawal|                                         |
| fee                   | string | false    | none        | Transaction fee                                  |                                        |




<a id="schemaresultlistnormalwithdraw"></a>

### schemaresultlistnormalwithdraw

| Name             | Type                                   | Required | Constraints | Description          | Notes                                                                 |
|------------------|----------------------------------------|----------|-------------|----------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code          | Returns "SUCCESS" for success, otherwise it's a failure.                |
| data             | [[NormalWithdraw](#schemanormalwithdraw)] | false    | none        | Correct response data |                                                                       |
| errorParam       | object                           | false    | none        | Error parameter information|                                                                       |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time   |                                                                      |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time    |                                                                       |
| traceId          | string                           | false    | none        | Call trace ID          |                                                                       |


<a id="schemanormalwithdraw"></a>

### schemanormalwithdraw

| Name                | Type            | Required | Constraints | Description                                 | Notes                                                                           |
|---------------------|-----------------|----------|-------------|---------------------------------------------|---------------------------------------------------------------------------------|
| id                  | string(int64)    | false    | none        | Withdrawal order ID                         |                                                                                 |
| userId              | string(int64)    | false    | none        | User ID                                     |                                                                                 |
| accountId           | string(int64)    | false    | none        | Account ID                                   |                                                                                 |
| coinId              | string(int64)    | false    | none        | Collateral coin ID                           |                                                                                 |
| amount              | string           | false    | none        | Withdrawal amount                            |                                                                                 |
| ethAddress          | string           | false    | none        | ETH address for withdrawal, may differ from the account's ETH address.  |                                                                    |
| clientWithdrawId     | string           | false    | none        | Client-defined ID for idempotent checks     |                                                                                 |
| l2Nonce             | string(int64)    | false    | none        | L2 signature nonce. First 32 bits of sha256(client_withdraw_id)          |                                           |
| l2ExpireTime        | string(int64)    | false    | none        | L2 signature expiration time. Unix time in hours, must be at least 24 hours after order creation.   |                                           |
| l2Signature         | [L2Signature](#schemal2signature) | false    | none        | L2 signature information                |                                                                                |
| status              | string           | false    | none        | Normal withdrawal order status            |                                                                                 |
| tradeWithdrawId       | string(int64)   | false    | none        | Corresponding trading service withdraw order ID    |                                                                                 |
| riskSignature        | [L2Signature](#schemal2signature) | false    | none        | L2 signature information                |                                                                                |
| l1ConfirmedTx         | [L1Tx](#schemal1tx) | false    | none        | L1 transaction information                  |                                                                          |
| l1ConfirmedTime       | string(int64)    | false    | none        | L1 transaction confirmation time             |                                                                       |
| l1CompletedTime       | string(int64)    | false    | none        | L1 withdrawal completion time                |                                                                       |
| createdTime           | string(int64)    | false    | none        | Creation time                              |                                                                        |
| updatedTime           | string(int64)    | false    | none        | Update time                                |                                                                        |

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

### schemaresultgetnormalwithdrawableamount

| Name             | Type                                          Okay, continuing the translated documentation:

| Name             | Type                                          | Required | Constraints | Description                                 | Notes                                                                 |
|------------------|-----------------------------------------------|----------|-------------|---------------------------------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code                                 | Returns "SUCCESS" for success, otherwise it's a failure.                |
| data             | [GetNormalWithdrawableAmount](#schemagetnormalwithdrawableamount) | false    | none        | Query normal withdrawable claim amount by user address - Response |                                                                      |
| errorParam       | object                           | false    | none        | Error parameter information              |                                                                       |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time                |                                                                       |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time               |                                                                       |
| traceId          | string                           | false    | none        | Call trace ID                                |                                                                       |


<a id="schemagetnormalwithdrawableamount"></a>

### schemagetnormalwithdrawableamount

| Name   | Type   | Required | Constraints | Description         | Notes    |
|--------|--------|----------|-------------|---------------------|----------|
| amount | string | false    | none        | Withdrawable amount |          |


<a id="schemaresultcreatecrosswithdraw"></a>

### schemaresultcreatecrosswithdraw

| Name             | Type                                     | Required | Constraints | Description            | Notes                                                                 |
|------------------|------------------------------------------|----------|-------------|------------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code            | Returns "SUCCESS" for success, otherwise it's a failure.                |
| data             | [CreateCrossWithdraw](#schemacreatecrosswithdraw) | false    | none        | Create cross-chain withdrawal order - Response  |                                                                      |
| errorParam       | object                           | false    | none        | Error parameter information        |                                                                       |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time           |                                                                      |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time          |                                                                      |
| traceId          | string                           | false    | none        | Call trace ID            |                                                                       |



<a id="schemacreatecrosswithdraw"></a>

### schemacreatecrosswithdraw

| Name            | Type           | Required | Constraints | Description            | Notes    |
|-----------------|----------------|----------|-------------|------------------------|----------|
| crossWithdrawId | string(int64) | false    | none        | Cross-chain withdrawal order ID |          |


<a id="schemacreatecrosswithdrawparam"></a>

### schemacreatecrosswithdrawparam

| Name                | Type            | Required | Constraints | Description                                | Notes                               |
|---------------------|-----------------|----------|-------------|--------------------------------------------|-------------------------------------|
| accountId           | string(int64)   | false    | none        | Account ID                                 |                                     |
| coinId              | string(int64)   | false    | none        | Coin ID                                    |                                     |
| amount              | string          | false    | none        | Withdrawal amount                          |                                     |
| ethAddress          | string          | false    | none        | Withdrawal address. If empty, withdraw to the corresponding address of the current account. |                                     |
| erc20Address       | string          | false    | none        | L1 ERC20 contract address for the withdrawn asset|                                     |
| lpAccountId         | string          | false    | none        | LP account ID for L2 receiving user transfers|                                     |
| clientCrossWithdrawId| string          | false    | none        | Client-defined ID, used for signature & idempotent check. Must be filled.|      |
| expireTime          | string(int64)   | false    | none        | Expiration time                         |                                     |
| l2Signature         | string          | false    | none        | L2 signature                             |                                     |
| fee                 | string          | false    | none        | Gas + fee obtained from front-end           |                                     |
| chainId             | string          | false    | none        | Chain ID for withdrawal                    |                                     |
| mpcAddress         | string          | false    | none        | Which mpc address initiated the withdraw | |
| mpcSignature         | string          | false    | none        |  Signature of the mpc address to the withdraw field| |
| mpcSignTime         | string          | false    | none        | mpc signature timestamp,unix timestamp in seconds     |    |



<a id="createnormalwithdraw"></a>

### createnormalwithdraw

| Name             | Type                                    | Required | Constraints | Description              | Notes                                                                 |
|------------------|-----------------------------------------|----------|-------------|--------------------------|-----------------------------------------------------------------------|
| code             | string                           | false    | none        | Status code              | Returns "SUCCESS" for success, otherwise it's a failure.                |
| data             | [CreateNormalWithdraw](#schemacreatenormalwithdraw) | false    | none        | Create normal withdrawal order - Response  |                                                                      |
| errorParam       | object                           | false    | none        | Error parameter information          |                                                                       |
| requestTime      | string(timestamp)                 | false    | none        | Server request receive time             |                                                                      |
| responseTime     | string(timestamp)                 | false    | none        | Server response return time            |                                                                       |
| traceId          | string                           | false    | none        | Call trace ID              |                                                                       |



<a id="schemacreatenormalwithdraw"></a>

### schemacreatenormalwithdraw

| Name       | Type           | Required | Constraints | Description           | Notes    |
|------------|----------------|----------|-------------|-----------------------|----------|
| withdrawId | string(int64)  |false     | none         | Withdrawal order ID  |          |


<a id="schemacreatenormalwithdrawparam"></a>

### schemacreatenormalwithdrawparam

| Name             | Type            | Required | Constraints | Description                                | Notes                                 |
|------------------|-----------------|----------|-------------|--------------------------------------------|---------------------------------------|
| accountId        | string(int64)   | false    | none        | Account ID                                 |                                       |
| coinId           | string(int64)   | false    | none        | Coin ID                                    |                                       |
| amount           | string          | false    | none        | Withdrawal amount                          |                                       |
| ethAddress       | string          | false    | none        | Withdrawal address. If empty, withdraw to the corresponding address of the current account.  |                                      |
| clientWithdrawId  | string          | false    | none        | Client-defined ID, used for signature & idempotent check. Must be filled.   |                                       |
| expireTime       | string(int64)  | false    | none        | Expiration time                           |                                       |
| l2Signature      | string          | false    | none        | L2 signature                                |                                       |
