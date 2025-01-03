# FundingPublicApi

<a id="opIdgetLatestFundingRate"></a>

## GET Get Latest Funding Rate by Contract ID

GET /api/v1/public/funding/getLatestFundingRate

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|contractId|query|string| No |Contract ID|

> Example Response

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": [
        {
            "contractId": "10000001",
            "fundingTime": "1734595200000",
            "fundingTimestamp": "1734597720000",
            "oraclePrice": "101559.9220921285450458526611328125",
            "indexPrice": "101522.558968500",
            "fundingRate": "-0.00005537",
            "isSettlement": false,
            "forecastFundingRate": "-0.00012293",
            "previousFundingRate": "0.00000567",
            "previousFundingTimestamp": "1734595140000",
            "premiumIndex": "-0.00036207",
            "avgPremiumIndex": "-0.00032293",
            "premiumIndexTimestamp": "1734597720000",
            "impactMarginNotional": "100",
            "impactAskPrice": "101485.8",
            "impactBidPrice": "101484.7",
            "interestRate": "0.0003",
            "predictedFundingRate": "0.00005000",
            "fundingRateIntervalMin": "240",
            "starkExFundingIndex": "101559.9220921285450458526611328125"
        }
    ],
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597737870",
    "responseTime": "1734597737873",
    "traceId": "5e27ebfb0ae79f51bbd347d2bf3585f9"
}
```

### Response Codes

|Status Code|Status Code Description|Description|Data Model|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#schemapagedatafundingrate>)|

<a id="opIdgetFundingRatePage"></a>

## GET Get Funding Rate History by Contract ID with Pagination

GET /api/v1/public/funding/getFundingRatePage

### Request Parameters

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|contractId|query|string| No |Contract ID|
|size|query|string| No |Number of items to retrieve. Must be greater than 0 and less than or equal to 100|
|offsetData|query|string| No |Pagination offset. If not provided or empty, retrieves the first page|
|filterSettlementFundingRate|query|string| No |If true, only query settlement funding rates (funding rate settlement occurs every 8 hours, with a predicted funding rate calculated every minute)|
|filterBeginTimeInclusive|query|string| No |Start time. If not provided, retrieves the oldest data|
|filterEndTimeExclusive|query|string| No |End time. If not provided, retrieves the latest data|

> Example Response

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "dataList": [
            {
                "contractId": "10000001",
                "fundingTime": "1733702400000",
                "fundingTimestamp": "1733702400000",
                "oraclePrice": "101120.888977311551570892333984375",
                "indexPrice": "101121.681521500",
                "fundingRate": "0.00005000",
                "isSettlement": true,
                "forecastFundingRate": "",
                "previousFundingRate": "0.00005000",
                "previousFundingTimestamp": "1733702340000",
                "premiumIndex": "0.00022566",
                "avgPremiumIndex": "0.00017953",
                "premiumIndexTimestamp": "1733702400000",
                "impactMarginNotional": "500",
                "impactAskPrice": "101269.6",
                "impactBidPrice": "101269.1",
                "interestRate": "0.0003",
                "predictedFundingRate": "0.00005000",
                "fundingRateIntervalMin": "240",
                "starkExFundingIndex": "101120.888977311551570892333984375"
            }
        ],
        "nextPageOffsetData": "0880A08A97B532"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734597585432",
    "responseTime": "1734597586672",
    "traceId": "02465a59be5d19088ba7e4b5c6b94f6d"
}
```

### Response Codes

|Status Code|Status Code Description|Description|Data Model|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#pagedatafundingrate>)|

# Data Models

<a id="pagedatafundingrate"></a>
### pagedatafundingrate

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise indicates failure|
|data|[PageDataFundingRate](#schemapagedatafundingrate)|false|none|General pagination response|
|errorParam|object|false|none|Parameter information in the error message|
|requestTime|string(timestamp)|false|none|Server request reception time|
|responseTime|string(timestamp)|false|none|Server response time|
|traceId|string|false|none|Call trace ID|


<a id="schemapagedatafundingrate"></a>
### schemapagedatafundingrate

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|dataList|[[FundingRate](#schemafundingrate)]|false|none|Data list|
|nextPageOffsetData|string|false|none|Offset data to retrieve the next page. Empty string if there is no next page|

<a id="listfundingrate"></a>
### listfundingrate

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, otherwise indicates failure|
|data|[[FundingRate](#schemafundingrate)]|false|none|Successful response data|
|errorParam|object|false|none|Parameter information in the error message|
|requestTime|string(timestamp)|false|none|Server request reception time|
|responseTime|string(timestamp)|false|none|Server response time|
|traceId|string|false|none|Call trace ID|


<a id="schemafundingrate"></a>
### schemafundingrate

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|contractId|string(int64)|false|none|Contract ID|
|fundingTime|string(int64)|false|none|Funding rate settlement time. E.g., the funding rate for the 08:00-09:00 period is calculated from the previous 07:00-08:00 data, finalized at 08:00, and used for settlement at 09:00|
|fundingTimestamp|string(int64)|false|none|Funding rate calculation time in milliseconds|
|oraclePrice|string|false|none|Oracle price|
|indexPrice|string|false|none|Index price|
|fundingRate|string|false|none|Funding rate|
|isSettlement|boolean|false|none|Funding rate settlement flag|
|forecastFundingRate|string|false|none|Forecast funding rate|
|previousFundingRate|string|false|none|Previous funding rate|
|previousFundingTimestamp|string(int64)|false|none|Previous funding rate calculation time in milliseconds|
|premiumIndex|string|false|none|Premium index|
|avgPremiumIndex|string|false|none|Average premium index|
|premiumIndexTimestamp|string|false|none|Premium index calculation time|
|impactMarginNotional|string|false|none|Quantity required for deep weighted buy/sell price calculation|
|impactAskPrice|string|false|none|Deep weighted ask price|
|impactBidPrice|string|false|none|Deep weighted bid price|
|interestRate|string|false|none|Fixed interest rate|
|predictedFundingRate|string|false|none|Comprehensive interest rate (interestRate/frequency)|
|fundingRateIntervalMin|string(int64)|false|none|Funding rate time interval in minutes|
|starkExFundingIndex|string|false|none|StarkEx funding index|


