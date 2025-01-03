# QuotePublicApi

<a id="opIdgetTicketSummary"></a>

## GET Get Quote Summary

GET /api/v1/public/quote/getTicketSummary

### Request Parameters

| Name | Location | Type   | Required | Description   |
|------|----------|--------|----------|---------------|
| period | query    | string | No       | Summary period |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#gettickersummarymodel) |

<a id="opIdgetTicker"></a>

## GET Query 24-Hour Quotes

GET /api/v1/public/quote/getTicker

### Request Parameters

| Name       | Location | Type   | Required | Description   |
|------------|----------|--------|----------|---------------|
| contractId | query    | string | No       | Contract ID   |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultlistticker) |


## GET Query Multi-Contract Quantitative K-Line

GET /api/v1/public/quote/getMultiContractKline

### Request Parameters

| Name                       | Location | Type   | Required | Description                             |
|----------------------------|----------|--------|----------|-----------------------------------------|
| contractIdList             | query    | string | No       | Collection of Contract IDs              |
| priceType                  | query    | string | No       | Price type                             |
| klineType                  | query    | string | No       | K-line type                            |
| size                       | query    | string | No       | Number to retrieve. Must be greater than 0 and less than or equal to 200 |
| filterBeginKlineTimeInclusive | query    | string | No       | Query start time (if 0, means from current time). Returns in descending order by time |
| filterEndKlineTimeExclusive   | query    | string | No       | Query end time                           |

> Request Example
```text
https://pro.edgex.exchange/api/v1/public/quote/getMultiContractKline?contractIdList=10000001&klineType=HOUR_1&filterBeginKlineTimeInclusive=1733416860000&filterEndKlineTimeExclusive=1734601200000&priceType=LAST_PRICE
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultlistcontractkline) |

### Response Data Structure

<a id="opIdgetKline"></a>

## GET Query K-Line

GET /api/v1/public/quote/getKline

### Request Parameters

| Name                       | Location | Type   | Required | Description                             |
|----------------------------|----------|--------|----------|-----------------------------------------|
| contractId                 | query    | string | No       | Contract ID                             |
| priceType                  | query    | string | No       | Price type                             |
| klineType                  | query    | string | No       | K-line type                            |
| size                       | query    | string | No       | Number to retrieve. Must be greater than 0 and less than or equal to 1000 |
| offsetData                 | query    | string | No       | Pagination offset. If empty, get the first page |
| filterBeginKlineTimeInclusive | query    | string | No       | Query start time (if 0, means from current time). Returns in descending order by time |
| filterEndKlineTimeExclusive   | query    | string | No       | Query end time                           |

> Request Example
```
https://pro.edgex.exchange/api/v1/public/quote/getKline?contractId=10000002&klineType=HOUR_1&filterBeginKlineTimeInclusive=1733416860000&filterEndKlineTimeExclusive=1734601200000&priceType=LAST_PRICE
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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultpagedatakline) |

<a id="opIdgetExchangeLongShortRatio"></a>

## GET Get Exchange Long Short Ratio

GET /api/v1/public/quote/getExchangeLongShortRatio

### Request Parameters

| Name                 | Location | Type   | Required | Description                                          |
|----------------------|----------|--------|----------|------------------------------------------------------|
| range                | query    | string | No       | If empty, return data with the smallest range     |
| filterContractIdList | query    | string | No       | If empty, return data for all contracts               |
| filterExchangeList   | query    | string | No       | If empty, return data for all exchanges               |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#gettickersummarymodel) |

<a id="opIdgetDepth"></a>

## GET Query Order Book Depth

GET /api/v1/public/quote/getDepth

### Request Parameters

| Name       | Location | Type   | Required | Description                   |
|------------|----------|--------|----------|-------------------------------|
| contractId | query    | string | No       | Contract ID                   |
| level      | query    | string | No       | Depth level. Currently 15 and 200 levels available |

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
| 200         | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | default response | [Result](#schemaresultlistdepth) |

### Response Data Structure

# Data Models

<a id="schemaresultlistdepth"></a>
### schemaresultlistdepth

| Name        | Type                   | Required | Constraints | Chinese Name | Description                                   |
|-------------|------------------------|----------|-------------|--------------|-----------------------------------------------|
| code        | string                 | false    | none        |              | Status code. "SUCCESS" for success, others for failures  |
| data        | [[Depth](#schemadepth)] | false    | none        |              | Correct response data                           |
| errorParam  | object                 | false    | none        |              | Parameter information in error messages           |
| requestTime | string(timestamp)      | false    | none        |              | Server request reception time              |
| responseTime| string(timestamp)      | false    | none        |              | Server response return time                  |
| traceId     | string                 | false    | none        |              | Call trace ID                              |

<a id="schemadepth"></a>
### schemadepth

| Name        | Type                | Required | Constraints | Chinese Name | Description          |
|-------------|---------------------|----------|-------------|--------------|----------------------|
| startVersion | string(int64)        | false    | none        |              | Start order book version number  |
| endVersion  | string(int64)        | false    | none        |              | End order book version number   |
| level       | integer(int32)      | false    | none        |              | Depth level         |
| contractId  | string(int64)        | false    | none        |              | Contract ID          |
| contractName | string              | false    | none        |              | Contract name        |
| asks        | [[BookOrder](#schemabookorder)]| false    | none        |              | Ask list             |
| bids        | [[BookOrder](#schemabookorder)]| false    | none        |              | Bid list             |
| depthType   | string              | false    | none        |              | Depth type           |

#### Enum Values

| Property  | Value               |
|-----------|---------------------|
| depthType | UNKNOWN_DEPTH_TYPE  |
| depthType | SNAPSHOT            |
| depthType | CHANGED             |
| depthType | UNRECOGNIZED       |


<a id="schemabookorder"></a>
### schemabookorder

| Name  | Type           | Required | Constraints | Chinese Name | Description |
|-------|----------------|----------|-------------|--------------|-------------|
| price | string(decimal)| false    | none        |              | Price       |
| size  | string(decimal)| false    | none        |              | Quantity    |


<a id="schemaresultpagedatakline"></a>

### schemaresultpagedatakline

| Name        | Type                 | Required | Constraints | Chinese Name | Description                                  |
|-------------|----------------------|----------|-------------|--------------|----------------------------------------------|
| code        | string               | false    | none        |              | Status code. "SUCCESS" for success, others for failures |
| data        | [PageDataKline](#schemapagedatakline) | false    | none        |              | Generic paginated response                       |
| errorParam  | object               | false    | none        |              | Parameter information in error messages           |
| requestTime | string(timestamp)    | false    | none        |              | Server request reception time              |
| responseTime| string(timestamp)    | false    | none        |              | Server response return time                  |
| traceId     | string               | false    | none        |              | Call trace ID                              |


<a id="schemapagedatakline"></a>
### schemapagedatakline

| Name               | Type             | Required | Constraints | Chinese Name | Description                          |
|--------------------|------------------|----------|-------------|--------------|--------------------------------------|
| dataList           | [[Kline](#schemakline)]| false    | none        |              | Data list                            |
| nextPageOffsetData | string           | false    | none        |              | Offset for the next page. If there is no next page, it's an empty string |


<a id="schemakline"></a>
### schemakline

| Name        | Type                | Required | Constraints | Chinese Name | Description            |
|-------------|---------------------|----------|-------------|--------------|------------------------|
| klineId     | string(int64)        | false    | none        |              | K-Line ID              |
| contractId  | string(int64)        | false    | none        |              | Perpetual contract ID  |
| contractName | string              | false    | none        |              | Perpetual contract name |
| klineType   | string              | false    | none        |              | K-Line type            |
| klineTime   | string(int64)        | false    | none        |              | K-Line time            |
| priceType   | string              | false    | none        |              | Price type of the K-line    |
| trades      | string(int64)        | false    | none        |              | Number of trades        |
| size        | string(decimal)      | false    | none        |              | Volume                 |
| value       | string(decimal)      | false    | none        |              | Value                  |
| high        | string(decimal)      | false    | none        |              | High price              |
| low         | string(decimal)      | false    | none        |              | Low price               |
| open        | string(decimal)      | false    | none        |              | Opening price          |
| close       | string(decimal)      | false    | none        |              | Closing price          |
| makerBuySize| string(decimal)     | false    | none        |              | Maker buy volume |
| makerBuyValue | string(decimal)     | false    | none        |              | Maker buy value |

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

| Name        | Type                  | Required | Constraints | Chinese Name | Description                                   |
|-------------|-----------------------|----------|-------------|--------------|-----------------------------------------------|
| code        | string                | false    | none        |              | Status code. "SUCCESS" for success, others for failures |
| data        | [[ContractMultiKline](#schemacontractmultikline)]| false    | none        |              | Correct response data                           |
| errorParam  | object                | false    | none        |              | Parameter information in error messages           |
| requestTime | string(timestamp)     | false    | none        |              | Server request reception time              |
| responseTime| string(timestamp)     | false    | none        |              | Server response return time                  |
| traceId     | string                | false    | none        |              | Call trace ID                              |


<a id="schemacontractmultikline"></a>
### schemacontractmultikline

| Name        | Type                | Required | Constraints | Chinese Name | Description               |
|-------------|---------------------|----------|-------------|--------------|---------------------------|
| contractId  | string(int64)        | false    | none        |              | Perpetual contract ID     |
| klineList   | [[Kline](#schemakline)]  | false    | none        |              | Collection of kline data |



<a id="schemaresultlistticker"></a>
### schemaresultlistticker

| Name        | Type                   | Required | Constraints | Chinese Name | Description                                   |
|-------------|------------------------|----------|-------------|--------------|-----------------------------------------------|
| code        | string                 | false    | none        |              | Status code. "SUCCESS" for success, others for failures  |
| data        | [[Ticker](#schematicker)] | false    | none        |              | Correct response data                           |
| errorParam  | object                 | false    | none        |              | Parameter information in error messages           |
| requestTime | string(timestamp)      | false    | none        |              | Server request reception time              |
| responseTime| string(timestamp)      | false    | none        |              | Server response return time                  |
| traceId     | string                 | false    | none        |              | Call trace ID                              |

<a id="schematicker"></a>
### schematicker

| Name            | Type           | Required | Constraints | Chinese Name | Description                    |
|-----------------|----------------|----------|-------------|--------------|--------------------------------|
| contractId      | string(int64)   | false    | none        |              | Contract ID                    |
| contractName    | string         | false    | none        |              | Contract Name                  |
| priceChange     | string(decimal) | false    | none        |              | Price change                   |
| priceChangePercent | string(decimal)| false    | none        |              | Price change percentage         |
| trades          | string(int64)   | false    | none        |              | 24-hour number of trades      |
| size            | string(decimal) | false    | none        |              | 24-hour trading volume         |
| value           | string(decimal) | false    | none        |              | 24-hour trading value          |
| high            | string(decimal) | false    | none        |              | 24-hour high price              |
| low             | string(decimal) | false    | none        |              | 24-hour low price              |
| open            | string(decimal) | false    | none        |              | 24-hour opening price          |
| close           | string(decimal) | false    | none        |              | 24-hour closing price          |
| highTime        | string(int64)   | false    | none        |              | 24-hour high price time        |
| lowTime         | string(int64)   | false    | none        |              | 24-hour low price time        |
| startTime       | string(int64)   | false    | none        |              | 24-hour quote start time       |
| endTime         | string(int64)   | false    | none        |              | 24-hour quote end time         |
| lastPrice       | string(decimal) | false    | none        |              | Latest trade price            |
| indexPrice      | string(decimal) | false    | none        |              | Current index price            |
| oraclePrice     | string(decimal) | false    | none        |              | Current oracle price         |
| openInterest    | string(decimal) | false    | none        |              | Open Interest                |
| fundingRate     | string         | false    | none        |              | Current already settled funding rate    |
| fundingTime     | string(int64)   | false    | none        |              | Funding rate settlement time   |
| nextFundingTime | string(int64)   | false    | none        |              | Next funding rate settlement time|


<a id="gettickersummarymodel"></a>
### gettickersummarymodel

| Name        | Type                      | Required | Constraints | Chinese Name | Description                               |
|-------------|---------------------------|----------|-------------|--------------|-------------------------------------------|
| code        | string                    | false    | none        |              | Status code. "SUCCESS" for success, others for failures |
| data        | [GetTickerSummary](#schemagettickersummary)  | false    | none        |              | Get quote summary response                |
| errorParam  | object                    | false    | none        |              | Parameter information in error messages         |
| requestTime | string(timestamp)         | false    | none        |              | Server request reception time            |
| responseTime| string(timestamp)         | false    | none        |              | Server response return time                |
| traceId     | string                    | false    | none        |              | Call trace ID                           |


<a id="schemagettickersummary"></a>
### schemagettickersummary

| Name          | Type                 | Required | Constraints | Chinese Name | Description      |
|---------------|----------------------|----------|-------------|--------------|------------------|
| tickerSummary | [TickerSummary](#schematickersummary) | false    | none        |              | Quote summary  |


<a id="schematickersummary"></a>
### schematickersummary

| Name         | Type   | Required | Constraints | Chinese Name | Description              |
|--------------|--------|----------|-------------|--------------|--------------------------|
| period       | string | false    | none        |              | Summary period             |
| trades       | string | false    | none        |              | Total exchange number of trades  |
| value        | string | false    | none        |              | Total traded value         |
| openInterest | string | false    | none        |              | Current total open interest |

#### Enum Values

| Property | Value            |
|----------|------------------|
| period   | UNKNOWN_PERIOD   |
| period   | LAST_DAY_1        |
| period   | LAST_DAY_7        |
| period   | LAST_DAY_30       |
| period   | UNRECOGNIZED     |


