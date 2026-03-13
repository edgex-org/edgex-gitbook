# EdgeX API 接口梳理

## 一、公开接口 (Public API)

### 1. 元数据服务 (MetaDataPublicApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `GET /api/v1/public/meta/getServerTime` | 元数据服务 | 获取服务器时间 |
| `GET /api/v1/public/meta/getMetaData` | 元数据服务 | 获取全局元数据(币种列表、合约列表、多链配置等) |

### 2. 行情服务 (QuotePublicApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `GET /api/v1/public/quote/getTicketSummary` | 行情服务 | 获取行情汇总信息 |
| `GET /api/v1/public/quote/getTicker` | 行情服务 | 查询24小时行情 |
| `GET /api/v1/public/quote/getMultiContractKline` | 行情服务 | 查询多合约K线 |
| `GET /api/v1/public/quote/getKline` | 行情服务 | 查询K线 |
| `GET /api/v1/public/quote/getExchangeLongShortRatio` | 行情服务 | 获取交易所多空比 |
| `GET /api/v1/public/quote/getDepth` | 行情服务 | 查询订单簿深度 |

### 3. 资金费率服务 (FundingPublicApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `GET /api/v1/public/funding/getLatestFundingRate` | 资金费率服务 | 获取最新资金费率 |
| `GET /api/v1/public/funding/getFundingRatePage` | 资金费率服务 | 分页获取资金费率历史 |

---

## 二、私有接口 (Private API)

### 1. 账户服务 (AccountPrivateApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `GET /api/v1/private/account/getPositionTransactionPage` | 账户服务 | 分页获取持仓交易记录 |
| `GET /api/v1/private/account/getPositionTransactionById` | 账户服务 | 根据ID批量获取持仓交易记录 |
| `GET /api/v1/private/account/getPositionTermPage` | 账户服务 | 分页获取持仓周期记录 |
| `GET /api/v1/private/account/getPositionByContractId` | 账户服务 | 根据合约ID获取持仓信息 |
| `GET /api/v1/private/account/getCollateralTransactionPage` | 账户服务 | 分页获取保证金交易记录 |
| `GET /api/v1/private/account/getCollateralTransactionById` | 账户服务 | 根据ID批量获取保证金交易记录 |
| `GET /api/v1/private/account/getCollateralByCoinId` | 账户服务 | 根据币种ID获取保证金信息 |
| `GET /api/v1/private/account/getAccountPage` | 账户服务 | 分页获取账户列表 |
| `GET /api/v1/private/account/getAccountDeleverageLight` | 账户服务 | 获取账户减仓灯号 |
| `GET /api/v1/private/account/getAccountById` | 账户服务 | 根据ID获取账户信息 |
| `GET /api/v1/private/account/getAccountAsset` | 账户服务 | 获取账户资产 |
| `GET /api/v1/private/account/getAccountAssetSnapshotPage` | 账户服务 | 分页获取账户资产快照 |

### 2. 资产服务 (AssetsPrivateApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `POST /api/v1/private/assets/createNormalWithdraw` | 资产服务 | 创建普通提现订单 |
| `POST /api/v1/private/assets/createCrossWithdraw` | 资产服务 | 创建跨链提现订单 |
| `GET /api/v1/private/assets/getNormalWithdrawableAmount` | 资产服务 | 获取普通提现可用额度 |
| `GET /api/v1/private/assets/getNormalWithdrawById` | 资产服务 | 根据ID获取普通提现订单 |
| `GET /api/v1/private/assets/getCrossWithdrawSignInfo` | 资产服务 | 获取跨链提现签名所需信息 |
| `GET /api/v1/private/assets/getCrossWithdrawById` | 资产服务 | 根据ID获取跨链提现订单 |
| `GET /api/v1/private/assets/getAllOrdersPage` | 资产服务 | 聚合查询所有充提订单记录 |

### 3. 订单服务 (OrderPrivateApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `POST /api/v1/private/order/getMaxCreateOrderSize` | 订单服务 | 获取最大下单数量 |
| `POST /api/v1/private/order/createOrder` | 订单服务 | 创建订单 |
| `POST /api/v1/private/order/cancelOrderById` | 订单服务 | 根据订单ID取消订单 |
| `POST /api/v1/private/order/cancelAllOrder` | 订单服务 | 取消账户下所有订单 |
| `GET /api/v1/private/order/getOrderById` | 订单服务 | 根据订单ID批量获取订单 |
| `GET /api/v1/private/order/getOrderByClientOrderId` | 订单服务 | 根据客户端订单ID批量获取订单 |
| `GET /api/v1/private/order/getHistoryOrderPage` | 订单服务 | 分页获取历史订单 |
| `GET /api/v1/private/order/getHistoryOrderFillTransactionPage` | 订单服务 | 分页获取历史成交明细 |
| `GET /api/v1/private/order/getHistoryOrderFillTransactionById` | 订单服务 | 根据ID批量获取历史成交明细 |
| `GET /api/v1/private/order/getHistoryOrderById` | 订单服务 | 根据ID批量获取历史订单 |
| `GET /api/v1/private/order/getHistoryOrderByClientOrderId` | 订单服务 | 根据客户端订单ID批量获取历史订单 |
| `GET /api/v1/private/order/getActiveOrderPage` | 订单服务 | 分页获取活跃订单 |

### 4. 划转服务 (TransferPrivateApi)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `POST /api/v1/private/transfer/createTransferOut` | 划转服务 | 创建转出订单 |
| `GET /api/v1/private/transfer/getTransferOutById` | 划转服务 | 根据ID获取转出订单 |
| `GET /api/v1/private/transfer/getTransferOutAvailableAmount` | 划转服务 | 获取可转出金额 |
| `GET /api/v1/private/transfer/getTransferInById` | 划转服务 | 根据ID获取转入订单 |

---

## 三、WebSocket 接口

### 1. 私有 WebSocket (用户账户信息)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `/api/v1/private/ws` | WebSocket服务 | 实时推送账户、订单、持仓、充提等信息 |

**推送事件类型:**
- `Snapshot` - 快照数据
- `ACCOUNT_UPDATE` - 账户更新
- `DEPOSIT_UPDATE` - 充值更新
- `WITHDRAW_UPDATE` - 提现更新
- `TRANSFER_IN_UPDATE` - 转入更新
- `TRANSFER_OUT_UPDATE` - 转出更新
- `ORDER_UPDATE` - 订单更新
- `FORCE_WITHDRAW_UPDATE` - 强制提现更新
- `FORCE_TRADE_UPDATE` - 强制交易更新
- `FUNDING_SETTLEMENT` - 资金费率结算
- `ORDER_FILL_FEE_INCOME` - 订单成交手续费收入
- `START_LIQUIDATING` - 开始强平
- `FINISH_LIQUIDATING` - 完成强平

### 2. 公开 WebSocket (市场数据)
| 接口路径 | 所属服务 | 描述 |
|---------|---------|------|
| `/api/v1/public/ws` | WebSocket服务 | 订阅行情数据 |

**支持的频道类型:**
- `metadata` - 元数据订阅
- `ticker.{contractId}` - 指定合约24小时行情
- `ticker.all` - 所有合约行情
- `ticker.all.1s` - 所有合约行情(周期推送)
- `kline.{priceType}.{contractId}.{interval}` - K线数据
  - priceType: `LAST_PRICE`(最新价), `MARK_PRICE`(标记价)
  - interval: `MINUTE_1`, `MINUTE_5`, `MINUTE_15`, `MINUTE_30`, `HOUR_1`, `HOUR_2`, `HOUR_4`, `HOUR_6`, `HOUR_8`, `HOUR_12`, `DAY_1`, `WEEK_1`, `MONTH_1`
- `depth.{contractId}.{depth}` - 订单簿深度
  - depth: `15`(15档), `200`(200档)
- `trades.{contractId}` - 最新成交

---

## 四、服务端点

- **HTTP 端点:** `https://pro.edgex.exchange`
- **WebSocket 端点:** `wss://quote.edgex.exchange`

---

## 五、接口统计汇总

| 服务类型 | 接口数量 |
|---------|---------|
| 公开接口 - 元数据服务 | 2 |
| 公开接口 - 行情服务 | 6 |
| 公开接口 - 资金费率服务 | 2 |
| 私有接口 - 账户服务 | 12 |
| 私有接口 - 资产服务 | 7 |
| 私有接口 - 订单服务 | 13 |
| 私有接口 - 划转服务 | 4 |
| WebSocket 接口 | 2 |
| **合计** | **48** |

---

## 六、认证说明

### HTTP API 认证
- 私有 API 需要在请求头中包含认证信息
- 详见 [authentication.md](authentication.md)

### WebSocket 认证
- **Web端**: 使用 Base64 编码的认证信息放在 `SEC_WEBSOCKET_PROTOCOL` 头中
- **App/API**: 可使用自定义请求头或 Web 端方式
- WebSocket 为 GET 请求,无需签名请求体

---

## 七、其他文档

- [签名说明](sign.md) - API 签名规则
- [认证说明](authentication.md) - API 认证方式
- [WebSocket API](websocket-api.md) - WebSocket 详细文档
