# MetaDataPublicApi

<a id="opIdgetServerTime"></a>

## GET Server Time

GET /api/v1/public/meta/getServerTime

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

GET /api/v1/public/meta/getMetaData

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

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, others for failure.|
|data|[MetaData](#schemametadata)|false|none|Global metadata|
|errorParam|object|false|none|Parameter information in error message|
|requestTime|string(timestamp)|false|none|Server request receiving time|
|responseTime|string(timestamp)|false|none|Server response returning time|
|traceId|string|false|none|Call traceId|



<a id="schemametadata"></a>
### schemametadata

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|global|[Global](#schemaglobal)|false|none|Global meta information|
|coinList|[[Coin](#schemacoin)]|false|none|All coin meta information|
|contractList|[[Contract](#schemacontract)]|false|none|All contract meta information|
|multiChain|[MultiChain](#schemamultichain)|false|none|Cross-chain withdrawal related class|



<a id="schemamultichain"></a>
### schemamultichain

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|coinId|string(int64)|false|none|Asset id for deposit and withdrawal|
|maxWithdraw|string|false|none|Maximum withdrawal amount|
|minWithdraw|string|false|none|Minimum withdrawal amount|
|minDeposit|string|false|none|Minimum deposit amount|
|chainList|[[Chain](#schemachain)]|false|none|Supported chains|


<a id="schemachain"></a>
### schemachain

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|chain|string|false|none|Main chain name|
|chainId|string(int64)|false|none|chainId|
|chainIconUrl|string|false|none|Main chain icon url|
|contractAddress|string|false|none|Contract address|
|depositGasFeeLess|boolean|false|none|Whether to charge deposit fee|
|feeLess|boolean|false|none|Whether to exempt from fees|
|feeRate|string|false|none|Fee rate|
|gasLess|boolean|false|none|Whether to charge gas fee|
|gasToken|string|false|none|Main chain token name|
|minFee|string|false|none|Minimum withdrawal fee. If gas + value*fee_rate is less than min_fee, it will be charged according to min_fee|
|rpcUrl|string|false|none|Online node service of the chain|
|webTxUrl|string|false|none|Transaction tx link|
|withdrawGasFeeLess|boolean|false|none|Whether to charge withdrawal fee|
|tokenList|[[MultiChainToken](#schemamultichaintoken)]|false|none|Collection of cross-chain related token information|
|txConfirm|string(int64)|false|none|Number of confirmations for on-chain deposit|
|blockTime|string|false|none|Block time|
|appRpcUrl|string|false|none|none|


<a id="schemamultichaintoken"></a>
### schemamultichaintoken

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|tokenAddress|string|false|none|Token contract address|
|decimals|string(int64)|false|none|Token precision|
|iconUrl|string|false|none|Token icon url|
|token|string|false|none|Token name|
|pullOff|boolean|false|none|Whether to delist, default is false|
|withdrawEnable|boolean|false|none|Whether to support withdrawal of this type of asset|
|useFixedRate|boolean|false|none|Whether to use a fixed exchange rate|
|fixedRate|string|false|none|Fixed exchange rate|



<a id="schemacontract"></a>
### schemacontract

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|contractId|string(int64)|false|none|Perpetual contract pair identifier|
|contractName|string|false|none|Perpetual contract pair name|
|baseCoinId|string(int64)|false|none|e.g., 10000001 (BTC)|
|quoteCoinId|string(int64)|false|none|e.g., 1001 (USD/USDT)|
|tickSize|string(decimal)|false|none|Minimum price increment (quoteCoinId)|
|stepSize|string(decimal)|false|none|Minimum quantity increment (baseCoinId)|
|minOrderSize|string(decimal)|false|none|Minimum order quantity (baseCoinId)|
|maxOrderSize|string(decimal)|false|none|Maximum order quantity (baseCoinId)|
|maxOrderBuyPriceRatio|string(decimal)|false|none|Maximum limit buy order price ratio (compared to oracle price), decimal (quote_coin_id)|
|minOrderSellPriceRatio|string(decimal)|false|none|Minimum limit sell order price ratio (compared to oracle price), decimal (quote_coin_id)|
|maxPositionSize|string(decimal)|false|none|Maximum position quantity (baseCoinId)|
|riskTierList|[[RiskTier](#schemarisktier)]|false|none|List of risk limit tiers|
|defaultTakerFeeRate|string(decimal)|false|none|Default taker fee rate for the contract|
|defaultMakerFeeRate|string(decimal)|false|none|Default maker fee rate for the contract|
|defaultLeverage|string(decimal)|false|none|Initial default leverage multiplier when user has not set a trading leverage|
|liquidateFeeRate|string(decimal)|false|none|Liquidation fee rate|
|enableTrade|boolean|false|none|Whether trading is allowed. true: allowed, false: not allowed|
|enableDisplay|boolean|false|none|Whether to display. true: display, false: hide|
|enableOpenPosition|boolean|false|none|Whether opening positions is allowed. true: allowed to open and close, false: only allowed to close positions|
|fundingInterestRate|string(decimal)|false|none|Default value of overall interest rate, e.g., 0.0003|
|fundingImpactMarginNotional|string(decimal)|false|none|Quantity for calculating depth-weighted bid/ask price, e.g., 8000|
|fundingMaxRate|string(decimal)|false|none|Maximum funding rate, e.g., 0.000234|
|fundingMinRate|string(decimal)|false|none|Minimum funding rate, e.g., -0.000234|
|fundingRateIntervalMin|string(decimal)|false|none|Settlement interval of funding rate (in minutes, must be an integer multiple of 60 minutes, settlement starts from 00:00 UTC) decimal|
|displayDigitMerge|string(decimal)|false|none|Depth merge. e.g., "1,0.1,0.001"|
|displayMaxLeverage|string(decimal)|false|none|Maximum leverage multiplier to display, decimal. e.g., 20|
|displayMinLeverage|string(decimal)|false|none|Minimum leverage multiplier to display, decimal. e.g., 1|
|displayNewIcon|boolean|false|none|Whether it is a newly listed pair|
|displayHotIcon|boolean|false|none|Whether it is a hot pair|
|matchServerName|string|false|none|Matching service name, e.g., xxx-match-server-a. This value cannot be changed once configured, otherwise data migration is required.|
|starkExSyntheticAssetId|string(int64)|false|none|Synthetic asset id of the current pair, bigint for hex str.|
|starkExResolution|string(int64)|false|none|Processing precision of the quantity held by the current pair, bigint for hex str|
|starkExOraclePriceQuorum|string(int64)|false|none|Legal number of oracle prices, bigint for hex str|
|starkExOraclePriceSignedAssetId|[string]|false|none|bigint for hex str|
|starkExOraclePriceSigner|[string]|false|none|bigint for hex str|


<a id="schemarisktier"></a>
### schemarisktier

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|tier|integer(int32)|false|none|Tier, starting from 1|
|positionValueUpperBound|string(decimal)|false|none|Upper limit of position value for the tier (inclusive)|
|maxLeverage|string(decimal)|false|none|Maximum available leverage for the tier|
|maintenanceMarginRate|string(decimal)|false|none|Maintenance margin rate for the tier (only for display, the actual maintenance margin rate used is stark_ex_risk / 2^32 as an accurate maintenance margin rate), decimal|
|starkExRisk|string(int64)|false|none|1 ≤ risk < 2^32|
|starkExUpperBound|string(int64)|false|none|bigint. 0 ≤ upper_bound ≤ 2^128-1|



<a id="schemacoin"></a>
### schemacoin

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|coinId|string(int64)|false|none|Coin id|
|coinName|string|false|none|Coin name|
|stepSize|string(decimal)|false|none|Minimum quantity unit|
|showStepSize|string(decimal)|false|none|Minimum unit displayed to the user|
|iconUrl|string(url)|false|none|Coin icon url|
|starkExAssetId|string(int64)|false|none|starkex asset id. If empty, it means it does not exist|
|starkExResolution|string|false|none|starkex processing precision. If empty, it means it does not exist|

<a id="schemaglobal"></a>
### schemaglobal

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|appName|string|false|none|xxx|
|appEnv|string|false|none|dev/testnet/mainnet|
|appOnlySignOn|string|false|none|https://xxx.exchange|
|feeAccountId|string(int64)|false|none|Fee account id|
|feeAccountL2Key|string|false|none|Fee account l2Key, bigint for hex str|
|poolAccountId|string(int64)|false|none|Asset pool account id|
|poolAccountL2Key|string|false|none|Asset pool account l2Key, bigint for hex str|
|fastWithdrawAccountId|string(int64)|false|none|Fast withdrawal account id|
|fastWithdrawAccountL2Key|string|false|none|Fast withdrawal account l2Key, bigint for hex str|
|fastWithdrawMaxAmount|string|false|none|Maximum amount for fast withdrawal|
|fastWithdrawRegistryAddress|string|false|none|Fast withdrawal account address|
|starkExChainId|string|false|none|Chain id of starkex. bigint for hex str|
|starkExContractAddress|string|false|none|starkex contract address.|
|starkExCollateralCoin|[Coin](#schemacoin)|false|none|Coin meta information|
|starkExMaxFundingRate|integer(int32)|false|none|Maximum funding rate per second after starkex precision processing. i.e. stark_ex_max_funding_rate * 2^32 is the actual maximum funding rate per second. E.g.: 1120|
|starkExOrdersTreeHeight|integer(int32)|false|none|Order merkle tree height. E.g.: 64|
|starkExPositionsTreeHeight|integer(int32)|false|none|Account merkle tree height. E.g.: 64|
|starkExFundingValidityPeriod|integer(int32)|false|none|Funding rate submission validity period in seconds. E.g.: 86400|
|starkExPriceValidityPeriod|integer(int32)|false|none|Oracle price submission validity period in seconds. E.g.: 86400|
|maintenanceReason|string|false|none|Maintenance reason, empty if no maintenance|



<a id="getservertime"></a>
### getservertime

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|code|string|false|none|Status code. "SUCCESS" for success, others for failure.|
|data|[GetServerTime](#schemagetservertime)|false|none|Server time|
|errorParam|object|false|none|Parameter information in error message|
|requestTime|string(timestamp)|false|none|Server request receiving time|
|responseTime|string(timestamp)|false|none|Server response returning time|
|traceId|string|false|none|Call traceId|


### schemagetservertime

|Name|Type|Required|Constraints|Description|
|---|---|---|---|---|
|timeMillis|string(int64)|false|none|Server timestamp, milliseconds|


