# Quote Public API

## When to Use This Page

Use quote endpoints for market data reads such as order book depth, K-line history, ticker snapshots, open interest, and market status. These endpoints are public and do not require authentication.

## Minimal Calls

Get depth:

```bash
curl -X GET "https://edgex-prod-v2.edgex.exchange/api/v2/public/quote/getDepth?contractId=10000001&level=15"
```

Get ticker summary:

```bash
curl -X GET "https://edgex-prod-v2.edgex.exchange/api/v2/public/quote/getTickerSummary?contractId=10000001"
```

## Common Notes

- Load metadata first so you know which `contractId`, precision, and enums are valid.
- Quote endpoints are read-only; no authentication headers are required.
- For charting or polling clients, prefer a stable refresh cadence and switch to WebSocket when you need real-time updates.

<a id="opIdgetDepth"></a>

## GET Query Order Book Depth

GET /api/v2/public/quote/getDepth

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractId|string|No|Contract ID|
|level|integer|Yes|Depth level. Currently 15 and 200 levels available|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "startVersion": "201223746",
            "endVersion": "201223747",
            "level": 15,
            "contractId": "10000001",
            "contractName": "BTCUSDT",
            "asks": [
                {
                    "price": "101695.9",
                    "size": "0.579"
                },
                {
                    "price": "101696.0",
                    "size": "0.923"
                },
                {
                    "price": "101703.0",
                    "size": "0.129"
                }
            ],
            "bids": [
                {
                    "price": "101695.5",
                    "size": "1.710"
                },
                {
                    "price": "101694.1",
                    "size": "0.189"
                },
                {
                    "price": "101692.9",
                    "size": "0.223"
                }
            ],
            "depthType": "SNAPSHOT"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734598036434",
    "responseTime": "1734598036435",
    "traceId": "99b69f04bac0df6e37961f249b9545e4"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model |
|-------------|-------------------------|-------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlistdepth) |

### Response Data Structure

<a id="opIdgetKline"></a>

## GET Query K-Line

GET /api/v2/public/quote/getKline

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractId|string|No|Contract ID|
|priceType|string|No|Price type|
|klineType|string|No|K-line type|
|size|integer|No|Number to retrieve. Must be greater than 0 and less than or equal to 1000. Default is 1000|
|offsetData|string|No|Pagination offset. If empty, get the first page|
|filterBeginKlineTimeInclusive|string|No|Query start time (if 0, means from current time). Returns in descending order by time|
|filterEndKlineTimeExclusive|string|No|Query end time|

> Request Example
```
https://pro.edgex.exchange/api/v2/public/quote/getKline?contractId=10000002&klineType=HOUR_1&filterBeginKlineTimeInclusive=1733416860000&filterEndKlineTimeExclusive=1734601200000&priceType=LAST_PRICE
```

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "klineId": "687194918450962784",
                "contractId": "10000002",
                "contractName": "ETHUSDT",
                "klineType": "HOUR_1",
                "klineTime": "1734595200000",
                "priceType": "LAST_PRICE",
                "trades": "3142",
                "size": "111.96",
                "value": "412199.6286",
                "high": "3694.59",
                "low": "3667.42",
                "open": "3694.57",
                "close": "3670.42",
                "makerBuySize": "52.21",
                "makerBuyValue": "192147.4907"
            }
        ],
        "nextPageOffsetData": ""
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734601267556",
    "responseTime": "1734601267581",
    "traceId": "72cfd2eeb27fc602aa64990ad84cd8dd"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model      |
|-------------|-------------------------|-------------------|-----------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultpagedatakline) |

<a id="opIdgetMultiContractKline"></a>

## GET Query Multi-Contract Quantitative K-Line

GET /api/v2/public/quote/getMultiContractKline

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractIdList|array|No|Collection of Contract IDs|
|priceType|string|No|Price type|
|klineType|string|No|K-line type|
|size|integer|No|Number to retrieve. Must be greater than 0 and less than or equal to 200. Default is 200|
|filterBeginKlineTimeInclusive|string|No|Query start time (if 0, means from current time). Returns in descending order by time|
|filterEndKlineTimeExclusive|string|No|Query end time|

> Request Example
```text
https://pro.edgex.exchange/api/v2/public/quote/getMultiContractKline?contractIdList=10000001&klineType=HOUR_1&filterBeginKlineTimeInclusive=1733416860000&filterEndKlineTimeExclusive=1734601200000&priceType=LAST_PRICE
```

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "contractId": "10000001",
            "klineList": [
                {
                    "klineId": "687194849731486048",
                    "contractId": "10000001",
                    "contractName": "BTCUSDT",
                    "klineType": "HOUR_1",
                    "klineTime": "1734595200000",
                    "priceType": "LAST_PRICE",
                    "trades": "3123",
                    "size": "7.947",
                    "value": "807240.1268",
                    "high": "101798.4",
                    "low": "101326.3",
                    "open": "101603.8",
                    "close": "101605.6",
                    "makerBuySize": "5.222",
                    "makerBuyValue": "530431.6634"
                }
            ]
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734601896988",
    "responseTime": "1734601897009",
    "traceId": "7edd9609a0c5976c1cb58bdee3d08088"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model |
|-------------|-------------------------|-------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlistcontractkline) |

<a id="opIdgetAccurateOpenInterest"></a>

## GET Get Accurate Open Interest

GET /api/v2/public/quote/getAccurateOpenInterest

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractIdList|array|No|Collection of Contract IDs|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "contractId": "10000001",
            "timestamp": "1734595200000",
            "size": "1234.567"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597508246",
    "responseTime": "1734597508250",
    "traceId": "a49014b0ad76a121193d4717294f85fc"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model |
|-------------|-------------------------|-------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlistopeninterest) |

<a id="opIdgetTicker"></a>

## GET Query 24-Hour Ticker

GET /api/v2/public/quote/getTicker

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractId|string|No|Contract ID|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "contractId": "10000001",
            "contractName": "BTCUSDT",
            "priceChange": "-2270.5",
            "priceChangePercent": "-0.021849",
            "trades": "79372",
            "size": "499.487",
            "value": "50821443.7464",
            "high": "105331.5",
            "low": "98755.0",
            "open": "103913.2",
            "close": "101642.7",
            "highTime": "1734524115631",
            "lowTime": "1734575388228",
            "startTime": "1734510600000",
            "endTime": "1734597000000",
            "lastPrice": "101642.7",
            "indexPrice": "101676.380723500",
            "oraclePrice": "101636.3750002346932888031005859375",
            "markPrice": "101636.3750002346932888031005859375",
            "openInterest": "0.105",
            "fundingRate": "-0.00012236",
            "fundingTime": "1734595200000",
            "nextFundingTime": "1734609600000"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597508246",
    "responseTime": "1734597508250",
    "traceId": "a49014b0ad76a121193d4717294f85fc"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model |
|-------------|-------------------------|-------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlistticker) |

<a id="opIdgetTicketSummary"></a>

## GET Get Ticker Summary

GET /api/v2/public/quote/getTicketSummary

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|period|string|No|Summary period|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "tickerSummary": {
            "period": "LAST_DAY_1",
            "trades": "31450",
            "value": "201048203.7979",
            "openInterest": "13.565"
        }
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734596957000",
    "responseTime": "1734596957003",
    "traceId": "574a8b43497ebd0bca55d0b257d034fa"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model      |
|-------------|-------------------------|-------------------|-----------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#gettickersummarymodel) |

<a id="opIdgetStatDayTrade"></a>

## GET Get Daily Trade Statistics

GET /api/v2/public/quote/getStatDayTrade

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|startDayTimeInclusive|string|Yes|Filter to get trade statistics for specified start time|
|endDayTimeExclusive|string|Yes|Filter to get trade statistics for specified end time|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "dayTime": "1734480000000",
            "totalTrades": "125000",
            "totalValue": "1234567890.50",
            "createTime": "1734510600000"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597836994",
    "responseTime": "1734597837001",
    "traceId": "60af97ec1357f9d00da50bada9e4364c"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model      |
|-------------|-------------------------|-------------------|-----------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultliststatdaytrade) |

<a id="opIdgetExchangeLongShortRatio"></a>

## GET Get Exchange Long Short Ratio

GET /api/v2/public/quote/getExchangeLongShortRatio

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|range|string|No|If empty, return data with the smallest range|
|filterContractIdList|array|No|If empty, return data for all contracts|
|filterExchangeList|array|No|If empty, return data for all exchanges|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "exchangeLongShortRatioList": [
            {
                "range": "30m",
                "contractId": "10000001",
                "exchange": "_total_",
                "buyRatio": "50.9900",
                "sellRatio": "49.0100",
                "buyVolUsd": "567855766.2701",
                "sellVolUsd": "545892952.7900",
                "createdTime": "1734597018839",
                "updatedTime": "1734597018839"
            }
        ],
        "allRangeList": [
            "30m",
            "1h",
            "4h",
            "12h",
            "24h"
        ]
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597836994",
    "responseTime": "1734597837001",
    "traceId": "60af97ec1357f9d00da50bada9e4364c"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model      |
|-------------|-------------------------|-------------------|-----------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#getexchangelongshortratiomodel) |

<a id="opIdfee"></a>

## GET Get Daily Fees

GET /api/v2/public/quote/fee

Get daily fees across all contracts in a time range.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|filterBeginKlineTimeInclusive|integer|Yes|Inclusive start timestamp (milliseconds)|
|filterEndKlineTimeExclusive|integer|Yes|Exclusive end timestamp (milliseconds)|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "dayTimestamp": 1734480000000,
            "fee": "12345.67",
            "revenue": "54321.09"
        },
        {
            "dayTimestamp": 1734566400000,
            "fee": "23456.78",
            "revenue": "65432.10"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597508246",
    "responseTime": "1734597508250",
    "traceId": "a49014b0ad76a121193d4717294f85fc"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model |
|-------------|-------------------------|-------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#schemaresultlistdailyestimatedfee) |

<a id="opIdgetMarketStatus"></a>

## GET Get Market Status

GET /api/v2/public/quote/getMarketStatus

Get stocks market status and limit price information.

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractId|integer|No|Contract ID; when specified, returns status and limit price for the contract|

> Response Example

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "status": true,
        "force": false,
        "contractId": 10000001,
        "markPrice": "101642.7",
        "limitLowPrice": "91478.43",
        "limitHighPrice": "111806.97"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597508246",
    "responseTime": "1734597508250",
    "traceId": "a49014b0ad76a121193d4717294f85fc"
}
```

### Response

| Status Code | Status Code Meaning     | Description       | Data Model |
|-------------|-------------------------|-------------------|------------|
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful response | [Result](#getmarketstatusmodel) |

# Data Models

<a id="schemaresultlistdepth"></a>
### schemaresultlistdepth

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[[Depth](#schemadepth)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemadepth"></a>
### schemadepth

|Name|Type|Description|
|---|---|---|
|startVersion|string(int64)|Start order book version number|
|endVersion|string(int64)|End order book version number|
|level|integer(int32)|Depth level|
|contractId|string(int64)|Contract ID|
|contractName|string|Contract name|
|asks|[[BookOrder](#schemabookorder)]|Ask list|
|bids|[[BookOrder](#schemabookorder)]|Bid list|
|depthType|string|Depth type|

#### Enum Values

| Property  | Value               |
|-----------|---------------------|
| depthType | UNKNOWN_DEPTH_TYPE  |
| depthType | SNAPSHOT            |
| depthType | CHANGED             |
| depthType | UNRECOGNIZED       |

<a id="schemabookorder"></a>
### schemabookorder

|Name|Type|Description|
|---|---|---|
|price|string(decimal)|Price|
|size|string(decimal)|Quantity|

<a id="schemaresultpagedatakline"></a>
### schemaresultpagedatakline

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[PageDataKline](#schemapagedatakline)|Generic paginated response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemapagedatakline"></a>
### schemapagedatakline

|Name|Type|Description|
|---|---|---|
|dataList|[[Kline](#schemakline)]|Data list|
|nextPageOffsetData|string|Offset for the next page. If there is no next page, it's an empty string|

<a id="schemakline"></a>
### schemakline

|Name|Type|Description|
|---|---|---|
|klineId|string(int64)|K-Line ID|
|contractId|string(int64)|Perpetual contract ID|
|contractName|string|Perpetual contract name|
|klineType|string|K-Line type|
|klineTime|string(int64)|K-Line time|
|priceType|string|Price type of the K-line|
|trades|string(int64)|Number of trades|
|size|string(decimal)|Volume|
|value|string(decimal)|Value|
|high|string(decimal)|High price|
|low|string(decimal)|Low price|
|open|string(decimal)|Opening price|
|close|string(decimal)|Closing price|
|makerBuySize|string(decimal)|Maker buy volume|
|makerBuyValue|string(decimal)|Maker buy value|

#### Enum Values

| Property  | Value              |
|-----------|--------------------|
| klineType | UNKNOWN_KLINE_TYPE  |
| klineType | MINUTE_1            |
| klineType | MINUTE_5            |
| klineType | MINUTE_15           |
| klineType | MINUTE_30           |
| klineType | HOUR_1              |
| klineType | HOUR_2              |
| klineType | HOUR_4              |
| klineType | HOUR_6              |
| klineType | HOUR_8              |
| klineType | HOUR_12             |
| klineType | DAY_1               |
| klineType | WEEK_1              |
| klineType | MONTH_1             |
| klineType | UNRECOGNIZED       |
| priceType | UNKNOWN_PRICE_TYPE |
| priceType | ORACLE_PRICE       |
| priceType | INDEX_PRICE        |
| priceType | LAST_PRICE         |
| priceType | ASK1_PRICE        |
| priceType | BID1_PRICE        |
| priceType | OPEN_INTEREST        |
| priceType | UNRECOGNIZED       |

<a id="schemaresultlistcontractkline"></a>
### schemaresultlistcontractkline

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[[ContractMultiKline](#schemacontractmultikline)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemacontractmultikline"></a>
### schemacontractmultikline

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Perpetual contract ID|
|klineList|[[Kline](#schemakline)]|Collection of kline data|

<a id="schemaresultlistopeninterest"></a>
### schemaresultlistopeninterest

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[[OpenInterest](#schemaopeninterest)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemaopeninterest"></a>
### schemaopeninterest

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Contract ID|
|timestamp|string|Statistic timestamp|
|size|string(int64)|Open interest size|

<a id="schemaresultlistticker"></a>
### schemaresultlistticker

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[[Ticker](#schematicker)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schematicker"></a>
### schematicker

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Contract ID|
|contractName|string|Contract Name|
|priceChange|string(decimal)|Price change|
|priceChangePercent|string(decimal)|Price change percentage|
|trades|string(int64)|24-hour number of trades|
|size|string(decimal)|24-hour trading volume|
|value|string(decimal)|24-hour trading value|
|high|string(decimal)|24-hour high price|
|low|string(decimal)|24-hour low price|
|open|string(decimal)|24-hour opening price|
|close|string(decimal)|24-hour closing price|
|highTime|string(int64)|24-hour high price time|
|lowTime|string(int64)|24-hour low price time|
|startTime|string(int64)|24-hour quote start time|
|endTime|string(int64)|24-hour quote end time|
|lastPrice|string(decimal)|Latest trade price|
|indexPrice|string(decimal)|Current index price|
|oraclePrice|string(decimal)|Current oracle price|
|markPrice|string(decimal)|Current mark price|
|openInterest|string(decimal)|Open Interest|
|fundingRate|string|Current already settled funding rate|
|fundingTime|string(int64)|Funding rate settlement time|
|nextFundingTime|string(int64)|Next funding rate settlement time|

<a id="gettickersummarymodel"></a>
### gettickersummarymodel

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[GetTickerSummary](#schemagettickersummary)|Get quote summary response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemagettickersummary"></a>
### schemagettickersummary

|Name|Type|Description|
|---|---|---|
|tickerSummary|[TickerSummary](#schematickersummary)|Quote summary|

<a id="schematickersummary"></a>
### schematickersummary

|Name|Type|Description|
|---|---|---|
|period|string|Summary period|
|trades|string|Total exchange number of trades|
|value|string|Total traded value|
|openInterest|string|Current total open interest|

#### Enum Values

| Property | Value            |
|----------|------------------|
| period   | UNKNOWN_PERIOD   |
| period   | LAST_DAY_1        |
| period   | LAST_DAY_7        |
| period   | LAST_DAY_30       |
| period   | UNRECOGNIZED     |

<a id="schemaresultliststatdaytrade"></a>
### schemaresultliststatdaytrade

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[[StatDayTrade](#schemastatdaytrade)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemastatdaytrade"></a>
### schemastatdaytrade

|Name|Type|Description|
|---|---|---|
|dayTime|string(int64)|Date|
|totalTrades|string(int64)|Total number of trades|
|totalValue|string|Total trading value|
|createTime|string(int64)|Creation time|

<a id="getexchangelongshortratiomodel"></a>
### getexchangelongshortratiomodel

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[GetExchangeLongShortRatio](#schemagetexchangelongshortratio)|Exchange long short ratio response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemagetexchangelongshortratio"></a>
### schemagetexchangelongshortratio

|Name|Type|Description|
|---|---|---|
|exchangeLongShortRatioList|[[ExchangeLongShortRatio](#schemaexchangelongshortratio)]|Exchange long short ratio data list|
|allRangeList|[string]|All range data, related to account configuration. Possible values: 1m, 3m, 5m, 15m, 30m, 1h, 4h, 6h, 8h, 12h, 1d, 1w|

<a id="schemaexchangelongshortratio"></a>
### schemaexchangelongshortratio

|Name|Type|Description|
|---|---|---|
|range|string|Time range|
|contractId|string(int64)|Contract ID|
|exchange|string|Exchange|
|buyRatio|string|Buy ratio|
|sellRatio|string|Sell ratio|
|buyVolUsd|string|Buy volume USD|
|sellVolUsd|string|Sell volume USD|
|createdTime|string(int64)|Creation time|
|updatedTime|string(int64)|Update time|

<a id="schemaresultlistdailyestimatedfee"></a>
### schemaresultlistdailyestimatedfee

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[[DailyEstimatedFee](#schemadailyestimatedfee)]|Correct response data|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemadailyestimatedfee"></a>
### schemadailyestimatedfee

|Name|Type|Description|
|---|---|---|
|dayTimestamp|integer(int64)|Day timestamp|
|fee|string(decimal)|Fee amount|
|revenue|string(decimal)|Revenue amount|

<a id="getmarketstatusmodel"></a>
### getmarketstatusmodel

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failures|
|data|[GetMarketStatus](#schemagetmarketstatus)|Market status response|
|errorParam|object|Parameter information in error messages|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response return time|
|traceId|string|Call trace ID|

<a id="schemagetmarketstatus"></a>
### schemagetmarketstatus

|Name|Type|Description|
|---|---|---|
|status|boolean|Current market status, true indicates open|
|force|boolean|Whether status is forcibly set by backend|
|contractId|integer(int64)|Current returned contract ID|
|markPrice|string|Contract latest mark price|
|limitLowPrice|string|Lower limit price|
|limitHighPrice|string|Upper limit price|
