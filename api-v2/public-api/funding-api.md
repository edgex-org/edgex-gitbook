# Funding Public API

**Field naming note**: Some funding responses still expose historical backend field names such as `starkExFundingIndex`. Keep the server field names unchanged in your parser even though V2 signing and integration semantics have moved away from the old StarkEx model.

## When to Use This Page

Use funding endpoints when you need the latest funding rate, historical funding records, or funding-related displays in a market-data UI.

## Minimal Calls

Get latest funding rate:

```bash
curl -X GET "https://<api-domain>/api/v2/public/funding/getLatestFundingRate?contractId=10000001"
```

Get funding rate history:

```bash
curl -X GET "https://<api-domain>/api/v2/public/funding/getFundingRatePageByContractId?contractId=10000001&page=1&limit=20"
```

## Common Notes

- Funding endpoints are public and do not require authentication.
- Use metadata first if you need contract metadata or display precision before presenting funding data.
- Preserve legacy response field names exactly as returned by the server.

**Field naming note**: Some funding responses still expose historical backend field names such as `starkExFundingIndex`. Keep the server field names unchanged in your parser even though V2 signing and integration semantics have moved away from the old StarkEx model.

<a id="opIdgetLatestFundingRate"></a>

## GET Get Latest Funding Rate by Contract ID

GET /api/v2/public/funding/getLatestFundingRate

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractId|array[string]|No|Contract ID array|

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
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful response|[Result](#schemapagedatafundingrate>)|

<a id="opIdgetFundingRatePage"></a>

## GET Get Funding Rate History by Contract ID with Pagination

GET /api/v2/public/funding/getFundingRatePage

### Request Parameters

|Name|Type|Required|Description|
|---|---|---|---|
|contractId|string|No|Contract ID|
|size|integer|No|Number of items to retrieve. Must be greater than 0 and less than or equal to 100. Default: 100|
|offsetData|string|No|Pagination offset. If not provided or empty, retrieves the first page|
|filterSettlementFundingRate|boolean|No|If true, only query settlement funding rates (funding rate settlement occurs every 8 hours, with a predicted funding rate calculated every minute). Default: false|
|filterBeginTimeInclusive|string|No|Start time in milliseconds. If not provided, retrieves the oldest data|
|filterEndTimeExclusive|string|No|End time in milliseconds. If not provided, retrieves the latest data|

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
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|successful response|[Result](#pagedatafundingrate>)|

## Data Models

<a id="pagedatafundingrate"></a>
### FundingRatePage

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise indicates failure|
|data|[PageDataFundingRate](#schemapagedatafundingrate)|General pagination response|
|errorParam|object|Parameter information in the error message|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response time|
|traceId|string|Call trace ID|


<a id="schemapagedatafundingrate"></a>
### FundingRatePageSchema

|Name|Type|Description|
|---|---|---|
|dataList|[[FundingRate](#schemafundingrate)]|Data list|
|nextPageOffsetData|string|Offset data to retrieve the next page. Empty string if there is no next page|

<a id="listfundingrate"></a>
### FundingRateListResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, otherwise indicates failure|
|data|[[FundingRate](#schemafundingrate)]|Successful response data|
|errorParam|object|Parameter information in the error message|
|requestTime|string(timestamp)|Server request reception time|
|responseTime|string(timestamp)|Server response time|
|traceId|string|Call trace ID|


<a id="schemafundingrate"></a>
### FundingRateItem

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Contract ID|
|fundingTime|string(int64)|Funding rate settlement time. E.g., the funding rate for the 08:00-09:00 period is calculated from the previous 07:00-08:00 data, finalized at 08:00, and used for settlement at 09:00|
|fundingTimestamp|string(int64)|Funding rate calculation time in milliseconds|
|oraclePrice|string|Oracle price|
|indexPrice|string|Index price|
|fundingRate|string|Funding rate|
|isSettlement|boolean|Funding rate settlement flag|
|forecastFundingRate|string|Forecast funding rate|
|previousFundingRate|string|Previous funding rate|
|previousFundingTimestamp|string(int64)|Previous funding rate calculation time in milliseconds|
|premiumIndex|string|Premium index|
|avgPremiumIndex|string|Average premium index|
|premiumIndexTimestamp|string|Premium index calculation time|
|impactMarginNotional|string|Quantity required for deep weighted buy/sell price calculation|
|impactAskPrice|string|Deep weighted ask price|
|impactBidPrice|string|Deep weighted bid price|
|interestRate|string|Fixed interest rate|
|predictedFundingRate|string|Comprehensive interest rate (interestRate/frequency)|
|fundingRateIntervalMin|string(int64)|Funding rate time interval in minutes|
|starkExFundingIndex|string|Historical field name for the funding index value returned by the server.|
