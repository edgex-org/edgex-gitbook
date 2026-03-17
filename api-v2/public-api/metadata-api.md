# Metadata Public API

**Field naming note**: This metadata response still contains several historical `starkEx*` field names returned by the backend. Keep those field names unchanged when parsing API responses; in V2 documentation they should be read as legacy response names, not as a recommendation to use the old V1 signing model.

<a id="opIdgetServerTime"></a>

## GET Server Time

GET /api/v2/public/meta/getServerTime

> Example Response

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "timeMillis": "1734596189305"
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734596189305",
    "responseTime": "1734596189305",
    "traceId": "a69e6ec51701d7246cb344a719c99cbf"
}
```

### Response Body

|Status Code|Status Code Description|Description|Data Model|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#getservertime)|

<a id="opIdgetMetaData"></a>

## GET Meta Data

GET /api/v2/public/meta/getMetaData

> Example Response

> 200 Response

```json
{
    "code": "SUCCESS",
    "data": {
        "global": {
            "appName": "edgeX",
            "appEnv": "testnet",
            "appOnlySignOn": "https://testnet.edgex.exchange",
            "feeAccountId": "123456",
            "feeAccountL2Key": "0x1e240",
            "poolAccountId": "542076087396467085",
            "poolAccountL2Key": "0x3bf794b4433e0a8b353da361bb7284c670914d27ed04698e6abed0bf1198028",
            "fastWithdrawAccountId": "542076087396467085",
            "fastWithdrawAccountL2Key": "0x3bf794b4433e0a8b353da361bb7284c670914d27ed04698e6abed0bf1198028",
            "fastWithdrawMaxAmount": "100000",
            "fastWithdrawRegistryAddress": "0xb2846943C2EdA3830Fb784d2a6de93435267b11D",
            "starkExChainId": "0xaa36a7",
            "starkExContractAddress": "0xa3Cb2622C532e46c4376FAd4AbFDf9eDC717BABf",
            "starkExCollateralCoin": {
                "coinId": "1000",
                "coinName": "USDT",
                "stepSize": "0.000001",
                "showStepSize": "0.0001",
                "iconUrl": "https://static.edgex.exchange/icons/coin/USDT.svg",
                "starkExAssetId": "0x33bda5c923bae4e84825b74762d5482889b9512465fbffc50d1ae4b82e345c3",
                "starkExResolution": "0xf4240"
            },
            "starkExMaxFundingRate": 1120,
            "starkExOrdersTreeHeight": 64,
            "starkExPositionsTreeHeight": 64,
            "starkExFundingValidityPeriod": 604800,
            "starkExPriceValidityPeriod": 31536000,
            "maintenanceReason": ""
        },
        "coinList": [
            {
                "coinId": "1000",
                "coinName": "USDT",
                "stepSize": "0.000001",
                "showStepSize": "0.0001",
                "iconUrl": "https://static.edgex.exchange/icons/coin/USDT.svg",
                "starkExAssetId": "0x33bda5c923bae4e84825b74762d5482889b9512465fbffc50d1ae4b82e345c3",
                "starkExResolution": "0xf4240"
            },
            {
                "coinId": "1001",
                "coinName": "BTC",
                "stepSize": "0.001",
                "showStepSize": "0.001",
                "iconUrl": "https://static.edgex.exchange/icons/coin/BTC.svg",
                "starkExAssetId": null,
                "starkExResolution": null
            }
        ],
        "contractList": [
            {
                "contractId": "10000001",
                "contractName": "BTCUSDT",
                "baseCoinId": "1001",
                "quoteCoinId": "1000",
                "tickSize": "0.1",
                "stepSize": "0.001",
                "minOrderSize": "0.001",
                "maxOrderSize": "50.000",
                "maxOrderBuyPriceRatio": "0.05",
                "minOrderSellPriceRatio": "0.05",
                "maxPositionSize": "60.000",
                "riskTierList": [
                    {
                        "tier": 1,
                        "positionValueUpperBound": "50000",
                        "maxLeverage": "100",
                        "maintenanceMarginRate": "0.005",
                        "starkExRisk": "21474837",
                        "starkExUpperBound": "214748364800000000000"
                    },
                    {
                        "tier": 22,
                        "positionValueUpperBound": "79228162514264337593543",
                        "maxLeverage": "6",
                        "maintenanceMarginRate": "0.105",
                        "starkExRisk": "450971567",
                        "starkExUpperBound": "340282366920938463463374607431768211455"
                    }
                ],
                "defaultTakerFeeRate": "0.00055",
                "defaultMakerFeeRate": "0.0002",
                "defaultLeverage": "50",
                "liquidateFeeRate": "0.01",
                "enableTrade": true,
                "enableDisplay": true,
                "enableOpenPosition": true,
                "fundingInterestRate": "0.0003",
                "fundingImpactMarginNotional": "10",
                "fundingMaxRate": "0.000234",
                "fundingMinRate": "-0.000234",
                "fundingRateIntervalMin": "240",
                "displayDigitMerge": "0.1,0.5,1,2,5",
                "displayMaxLeverage": "50",
                "displayMinLeverage": "1",
                "displayNewIcon": false,
                "displayHotIcon": true,
                "matchServerName": "edgex-match-server",
                "starkExSyntheticAssetId": "0x425443322d31300000000000000000",
                "starkExResolution": "0x2540be400",
                "starkExOraclePriceQuorum": "0x1",
                "starkExOraclePriceSignedAssetId": [
                    "0x425443555344000000000000000000004d616b6572",
                    "0x425443555344000000000000000000005374437277",
                    "0x4254435553440000000000000000000053746f726b",
                    "0x425443555344000000000000000000004465787472",
                    "0x4254435553440000000000000000000053744b6169"
                ],
                "starkExOraclePriceSigner": [
                    "0x28253746dcd68a62df58cda44db2613ab11c8d17deb036feaec5ece1f8a16c1",
                    "0x41dbe627aeab66504b837b3abd88ae2f58ba6d98ee7bbd7f226c4684d9e6225",
                    "0xcc85afe4ca87f9628370c432c447e569a01dc96d160015c8039959db8521c4",
                    "0x2af704df5467285c5d1bd7c08ee33c49057fb2a05ecdc4f949293190f28ce7e",
                    "0x63f0f8507cc674ff668985a1ea854d3b73835a8181bfbb4564ae422bf68a2c0"
                ]
            },
            {
                "contractId": "10000002",
                "contractName": "ETHUSDT",
                "baseCoinId": "1002",
                "quoteCoinId": "1000",
                "tickSize": "0.01",
                "stepSize": "0.01",
                "minOrderSize": "0.01",
                "maxOrderSize": "500.00",
                "maxOrderBuyPriceRatio": "0.05",
                "minOrderSellPriceRatio": "0.05",
                "maxPositionSize": "800.00",
                "riskTierList": [
                    {
                        "tier": 1,
                        "positionValueUpperBound": "50000",
                        "maxLeverage": "100",
                        "maintenanceMarginRate": "0.005",
                        "starkExRisk": "21474837",
                        "starkExUpperBound": "214748364800000000000"
                    },
                    {
                        "tier": 22,
                        "positionValueUpperBound": "79228162514264337593543",
                        "maxLeverage": "6",
                        "maintenanceMarginRate": "0.105",
                        "starkExRisk": "450971567",
                        "starkExUpperBound": "340282366920938463463374607431768211455"
                    }
                ],
                "defaultTakerFeeRate": "0.00055",
                "defaultMakerFeeRate": "0.0002",
                "defaultLeverage": "50",
                "liquidateFeeRate": "0.01",
                "enableTrade": true,
                "enableDisplay": true,
                "enableOpenPosition": true,
                "fundingInterestRate": "0.0003",
                "fundingImpactMarginNotional": "100",
                "fundingMaxRate": "0.000234",
                "fundingMinRate": "-0.000234",
                "fundingRateIntervalMin": "240",
                "displayDigitMerge": "0.01,0.02,0.04,0.1,0.2",
                "displayMaxLeverage": "50",
                "displayMinLeverage": "1",
                "displayNewIcon": true,
                "displayHotIcon": false,
                "matchServerName": "edgex-match-server",
                "starkExSyntheticAssetId": "0x4554482d3900000000000000000000",
                "starkExResolution": "0x3b9aca00",
                "starkExOraclePriceQuorum": "0x1",
                "starkExOraclePriceSignedAssetId": [
                    "0x455448555344000000000000000000004d616b6572",
                    "0x455448555344000000000000000000005374437277",
                    "0x4554485553440000000000000000000053746f726b",
                    "0x455448555344000000000000000000004465787472",
                    "0x4554485553440000000000000000000053744b6169"
                ],
                "starkExOraclePriceSigner": [
                    "0x28253746dcd68a62df58cda44db2613ab11c8d17deb036feaec5ece1f8a16c1",
                    "0x41dbe627aeab66504b837b3abd88ae2f58ba6d98ee7bbd7f226c4684d9e6225",
                    "0xcc85afe4ca87f9628370c432c447e569a01dc96d160015c8039959db8521c4",
                    "0x2af704df5467285c5d1bd7c08ee33c49057fb2a05ecdc4f949293190f28ce7e",
                    "0x63f0f8507cc674ff668985a1ea854d3b73835a8181bfbb4564ae422bf68a2c0"
                ]
            }
        ],
        "multiChain": {
            "coinId": "1000",
            "maxWithdraw": "100000",
            "minWithdraw": "0",
            "minDeposit": "10",
            "chainList": [
                {
                    "chain": "Sepolia - Testnet",
                    "chainId": "11155111",
                    "chainIconUrl": "https://static.edgex.exchange/icons/chain/sepolia.svg",
                    "contractAddress": "0xC820e27D4821071129D4fB04CcD9ae8a370373bc",
                    "depositGasFeeLess": false,
                    "feeLess": false,
                    "feeRate": "0.0001",
                    "gasLess": false,
                    "gasToken": "ETH",
                    "minFee": "2",
                    "rpcUrl": "https://rpc.edgex.exchange/RMZZpeTnB6hjfcm8xNNyo6cKa9Zn4qgB/eth-sepolia",
                    "webTxUrl": "https://sepolia.etherscan.io/tx/",
                    "withdrawGasFeeLess": false,
                    "tokenList": [
                        {
                            "tokenAddress": "0xd98B590ebE0a3eD8C144170bA4122D402182976f",
                            "decimals": "6",
                            "iconUrl": "https://static.edgex.exchange/icons/coin/USDT.svg",
                            "token": "USDT",
                            "pullOff": false,
                            "withdrawEnable": true,
                            "useFixedRate": false,
                            "fixedRate": ""
                        }
                    ],
                    "txConfirm": "10",
                    "blockTime": "12",
                    "allowAaDeposit": true,
                    "allowAaWithdraw": false,
                    "appRpcUrl": "https://rpc.edgex.exchange/GujYf2XWDvzXDpQdXno92DGRhfy7HuLK/eth-sepolia"
                },
                {
                    "chain": "BNB - Testnet",
                    "chainId": "97",
                    "chainIconUrl": "https://static.edgex.exchange/icons/chain/sepolia.svg",
                    "contractAddress": "0xBe8dCAE2b5E58BdEe4695F7f366fF0A8B0A414D1",
                    "depositGasFeeLess": false,
                    "feeLess": false,
                    "feeRate": "0.0001",
                    "gasLess": false,
                    "gasToken": "BSC",
                    "minFee": "2",
                    "rpcUrl": "https://rpc.edgex.exchange/RMZZpeTnB6hjfcm8xNNyo6cKa9Zn4qgB/bsc-testnet",
                    "webTxUrl": "https://testnet.bscscan.com/tx/",
                    "withdrawGasFeeLess": false,
                    "tokenList": [
                        {
                            "tokenAddress": "0xda6c748A7593826e410183F05893dbB363D025a1",
                            "decimals": "6",
                            "iconUrl": "https://static.edgex.exchange/icons/coin/USDT.svg",
                            "token": "USDT",
                            "pullOff": false,
                            "withdrawEnable": true,
                            "useFixedRate": false,
                            "fixedRate": ""
                        }
                    ],
                    "txConfirm": "10",
                    "blockTime": "3",
                    "allowAaDeposit": false,
                    "allowAaWithdraw": false,
                    "appRpcUrl": "https://rpc.edgex.exchange/GujYf2XWDvzXDpQdXno92DGRhfy7HuLK/bsc-testnet"
                },
                {
                    "chain": "Arbitrum - Testnet",
                    "chainId": "421614",
                    "chainIconUrl": "https://static.edgex.exchange/icons/chain/sepolia.svg",
                    "contractAddress": "0xeeA926DB072E839063321776ddAdaddeECdF9718",
                    "depositGasFeeLess": false,
                    "feeLess": false,
                    "feeRate": "0.0001",
                    "gasLess": false,
                    "gasToken": "ETH",
                    "minFee": "2",
                    "rpcUrl": "https://rpc.edgex.exchange/RMZZpeTnB6hjfcm8xNNyo6cKa9Zn4qgB/arbitrum-sepolia",
                    "webTxUrl": "https://sepolia.arbiscan.io/tx/",
                    "withdrawGasFeeLess": false,
                    "tokenList": [
                        {
                            "tokenAddress": "0x608babb39bb03C038b8DABc3D4bF4e0D02d455Cd",
                            "decimals": "18",
                            "iconUrl": "https://static.edgex.exchange/icons/coin/USDT.svg",
                            "token": "USDT",
                            "pullOff": false,
                            "withdrawEnable": true,
                            "useFixedRate": false,
                            "fixedRate": ""
                        }
                    ],
                    "txConfirm": "10",
                    "blockTime": "3",
                    "allowAaDeposit": true,
                    "allowAaWithdraw": true,
                    "appRpcUrl": "https://rpc.edgex.exchange/GujYf2XWDvzXDpQdXno92DGRhfy7HuLK/arbitrum-sepolia"
                }
            ]
        }
    },
    "msg": null,
    "errorParam": null,
    "requestTime": "1734595526342",
    "responseTime": "1734595526343",
    "traceId": "1ee9b62c30925f0df6bd6c8604f32df4"
}
```

### Response Body

|Status Code|Status Code Description|Description|Data Model|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|default response|[Result](#MetadataResult)|

# Data Models


<a id="MetadataResult"></a>
### MetadataResult

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failure.|
|data|[MetaData](#schemametadata)|Global metadata|
|errorParam|object|Parameter information in error message|
|requestTime|string(timestamp)|Server request receiving time|
|responseTime|string(timestamp)|Server response returning time|
|traceId|string|Call traceId|



<a id="schemametadata"></a>
### schemametadata

|Name|Type|Description|
|---|---|---|
|global|[Global](#schemaglobal)|Global meta information|
|coinList|[[Coin](#schemacoin)]|All coin meta information|
|contractList|[[Contract](#schemacontract)]|All contract meta information|
|multiChain|[MultiChain](#schemamultichain)|Cross-chain withdrawal related class|



<a id="schemamultichain"></a>
### schemamultichain

|Name|Type|Description|
|---|---|---|
|coinId|string(int64)|Asset id for deposit and withdrawal|
|maxWithdraw|string|Maximum withdrawal amount|
|minWithdraw|string|Minimum withdrawal amount|
|minDeposit|string|Minimum deposit amount|
|chainList|[[Chain](#schemachain)]|Supported chains|


<a id="schemachain"></a>
### schemachain

|Name|Type|Description|
|---|---|---|
|chain|string|Main chain name|
|chainId|string(int64)|chainId|
|chainIconUrl|string|Main chain icon url|
|contractAddress|string|Contract address|
|depositGasFeeLess|boolean|Whether to charge deposit fee|
|feeLess|boolean|Whether to exempt from fees|
|feeRate|string|Fee rate|
|gasLess|boolean|Whether to charge gas fee|
|gasToken|string|Main chain token name|
|minFee|string|Minimum withdrawal fee. If gas + value*fee_rate is less than min_fee, it will be charged according to min_fee|
|rpcUrl|string|Online node service of the chain|
|webTxUrl|string|Transaction tx link|
|withdrawGasFeeLess|boolean|Whether to charge withdrawal fee|
|tokenList|[[MultiChainToken](#schemamultichaintoken)]|Collection of cross-chain related token information|
|txConfirm|string(int64)|Number of confirmations for on-chain deposit|
|blockTime|string|Block time|
|appRpcUrl|string|none|


<a id="schemamultichaintoken"></a>
### schemamultichaintoken

|Name|Type|Description|
|---|---|---|
|tokenAddress|string|Token contract address|
|decimals|string(int64)|Token precision|
|iconUrl|string|Token icon url|
|token|string|Token name|
|pullOff|boolean|Whether to delist, default is false|
|withdrawEnable|boolean|Whether to support withdrawal of this type of asset|
|useFixedRate|boolean|Whether to use a fixed exchange rate|
|fixedRate|string|Fixed exchange rate|



<a id="schemacontract"></a>
### schemacontract

|Name|Type|Description|
|---|---|---|
|contractId|string(int64)|Perpetual contract pair identifier|
|contractName|string|Perpetual contract pair name|
|baseCoinId|string(int64)|e.g., 10000001 (BTC)|
|quoteCoinId|string(int64)|e.g., 1001 (USD/USDT)|
|tickSize|string(decimal)|Minimum price increment (quoteCoinId)|
|stepSize|string(decimal)|Minimum quantity increment (baseCoinId)|
|minOrderSize|string(decimal)|Minimum order quantity (baseCoinId)|
|maxOrderSize|string(decimal)|Maximum order quantity (baseCoinId)|
|maxOrderBuyPriceRatio|string(decimal)|Maximum limit buy order price ratio (compared to oracle price), decimal (quote_coin_id)|
|minOrderSellPriceRatio|string(decimal)|Minimum limit sell order price ratio (compared to oracle price), decimal (quote_coin_id)|
|maxPositionSize|string(decimal)|Maximum position quantity (baseCoinId)|
|riskTierList|[[RiskTier](#schemarisktier)]|List of risk limit tiers|
|defaultTakerFeeRate|string(decimal)|Default taker fee rate for the contract|
|defaultMakerFeeRate|string(decimal)|Default maker fee rate for the contract|
|defaultLeverage|string(decimal)|Initial default leverage multiplier when user has not set a trading leverage|
|liquidateFeeRate|string(decimal)|Liquidation fee rate|
|enableTrade|boolean|Whether trading is allowed. true: allowed, false: not allowed|
|enableDisplay|boolean|Whether to display. true: display, false: hide|
|enableOpenPosition|boolean|Whether opening positions is allowed. true: allowed to open and close, false: only allowed to close positions|
|fundingInterestRate|string(decimal)|Default value of overall interest rate, e.g., 0.0003|
|fundingImpactMarginNotional|string(decimal)|Quantity for calculating depth-weighted bid/ask price, e.g., 8000|
|fundingMaxRate|string(decimal)|Maximum funding rate, e.g., 0.000234|
|fundingMinRate|string(decimal)|Minimum funding rate, e.g., -0.000234|
|fundingRateIntervalMin|string(decimal)|Settlement interval of funding rate (in minutes, must be an integer multiple of 60 minutes, settlement starts from 00:00 UTC) decimal|
|displayDigitMerge|string(decimal)|Depth merge. e.g., "1,0.1,0.001"|
|displayMaxLeverage|string(decimal)|Maximum leverage multiplier to display, decimal. e.g., 20|
|displayMinLeverage|string(decimal)|Minimum leverage multiplier to display, decimal. e.g., 1|
|displayNewIcon|boolean|Whether it is a newly listed pair|
|displayHotIcon|boolean|Whether it is a hot pair|
|matchServerName|string|Matching service name, e.g., xxx-match-server-a. This value cannot be changed once configured, otherwise data migration is required.|
|starkExSyntheticAssetId|string(int64)|Synthetic asset id of the current pair, bigint for hex str.|
|starkExResolution|string(int64)|Processing precision of the quantity held by the current pair, bigint for hex str|
|starkExOraclePriceQuorum|string(int64)|Legal number of oracle prices, bigint for hex str|
|starkExOraclePriceSignedAssetId|[string]|bigint for hex str|
|starkExOraclePriceSigner|[string]|bigint for hex str|


<a id="schemarisktier"></a>
### schemarisktier

|Name|Type|Description|
|---|---|---|
|tier|integer(int32)|Tier, starting from 1|
|positionValueUpperBound|string(decimal)|Upper limit of position value for the tier (inclusive)|
|maxLeverage|string(decimal)|Maximum available leverage for the tier|
|maintenanceMarginRate|string(decimal)|Maintenance margin rate for the tier (only for display, the actual maintenance margin rate used is stark_ex_risk / 2^32 as an accurate maintenance margin rate), decimal|
|starkExRisk|string(int64)|1 ≤ risk < 2^32|
|starkExUpperBound|string(int64)|bigint. 0 ≤ upper_bound ≤ 2^128-1|



<a id="schemacoin"></a>
### schemacoin

|Name|Type|Description|
|---|---|---|
|coinId|string(int64)|Coin id|
|coinName|string|Coin name|
|stepSize|string(decimal)|Minimum quantity unit|
|showStepSize|string(decimal)|Minimum unit displayed to the user|
|iconUrl|string(url)|Coin icon url|
|starkExAssetId|string(int64)|Historical response field name for the asset id used by backend metadata. If empty, the value is not available.|
|starkExResolution|string|Historical response field name for backend precision metadata. If empty, the value is not available.|

<a id="schemaglobal"></a>
### schemaglobal

|Name|Type|Description|
|---|---|---|
|appName|string|xxx|
|appEnv|string|dev/testnet/mainnet|
|appOnlySignOn|string|https://xxx.exchange|
|feeAccountId|string(int64)|Fee account id|
|feeAccountL2Key|string|Fee account l2Key, bigint for hex str|
|poolAccountId|string(int64)|Asset pool account id|
|poolAccountL2Key|string|Asset pool account l2Key, bigint for hex str|
|fastWithdrawAccountId|string(int64)|Fast withdrawal account id|
|fastWithdrawAccountL2Key|string|Fast withdrawal account l2Key, bigint for hex str|
|fastWithdrawMaxAmount|string|Maximum amount for fast withdrawal|
|fastWithdrawRegistryAddress|string|Fast withdrawal account address|
|starkExChainId|string|Historical response field name for the chain id in backend metadata. Bigint in hex string format.|
|starkExContractAddress|string|Historical response field name for the contract address used by backend metadata.|
|starkExCollateralCoin|[Coin](#schemacoin)|Coin meta information|
|starkExMaxFundingRate|integer(int32)|Maximum funding rate per second after starkex precision processing. i.e. stark_ex_max_funding_rate * 2^32 is the actual maximum funding rate per second. E.g.: 1120|
|starkExOrdersTreeHeight|integer(int32)|Order merkle tree height. E.g.: 64|
|starkExPositionsTreeHeight|integer(int32)|Account merkle tree height. E.g.: 64|
|starkExFundingValidityPeriod|integer(int32)|Funding rate submission validity period in seconds. E.g.: 86400|
|starkExPriceValidityPeriod|integer(int32)|Oracle price submission validity period in seconds. E.g.: 86400|
|maintenanceReason|string|Maintenance reason, empty if no maintenance|



<a id="getservertime"></a>
### getservertime

|Name|Type|Description|
|---|---|---|
|code|string|Status code. "SUCCESS" for success, others for failure.|
|data|[GetServerTime](#schemagetservertime)|Server time|
|errorParam|object|Parameter information in error message|
|requestTime|string(timestamp)|Server request receiving time|
|responseTime|string(timestamp)|Server response returning time|
|traceId|string|Call traceId|


### schemagetservertime

|Name|Type|Description|
|---|---|---|
|timeMillis|string(int64)|Server timestamp, milliseconds|
