# EdgeX Golang SDK 接口清单

## 概述

本文档梳理了 EdgeX Golang SDK 当前暴露的所有公开接口，按照服务模块分类。

---

## 一、公开接口（Public API）

### 1. 元数据服务 (Metadata)

| SDK方法 | 接口路径 | 说明 |
|---------|---------|------|
| `GetServerTime` | `GET /api/v1/public/meta/getServerTime` | 获取服务器时间 |
| `GetMetaData` | `GET /api/v1/public/meta/getMetaData` | 获取全局元数据(币种列表、合约列表、多链配置等) |


---

## 二、私有接口（Private API）

### 1. 账户服务 (Account)

| SDK方法 | 接口路径 | 说明 |
|---------|---------|------|
| `GetAccountAsset` | `GET /api/v1/private/account/getAccountAsset` | 获取账户资产 |
| `GetAccountPositions` | `GET /api/v1/private/account/getAccountAsset` | 获取账户持仓（从资产接口中提取） |
| `GetPositionByContractID` | `GET /api/v1/private/account/getPositionByContractId` | 根据合约ID获取持仓信息 |
| `GetPositionTransactionPage` | `GET /api/v1/private/account/getPositionTransactionPage` | 分页获取持仓交易记录 |
| `GetPositionTransactionByID` | `GET /api/v1/private/account/getPositionTransactionById` | 根据ID批量获取持仓交易记录 |
| `GetPositionTermPage` | `GET /api/v1/private/account/getPositionTermPage` | 分页获取持仓周期记录 |
| `GetCollateralByCoinID` | `GET /api/v1/private/account/getCollateralByCoinId` | 根据币种ID获取保证金信息 |
| `GetCollateralTransactionPage` | `GET /api/v1/private/account/getCollateralTransactionPage` | 分页获取保证金交易记录 |
| `GetCollateralTransactionByID` | `GET /api/v1/private/account/getCollateralTransactionById` | 根据ID批量获取保证金交易记录 |
| `GetAccountByID` | `GET /api/v1/private/account/getAccountById` | 根据ID获取账户信息 |
| `GetAccountDeleverageLight` | `GET /api/v1/private/account/getAccountDeleverageLight` | 获取账户减仓灯号 |
| `GetAccountAssetSnapshotPage` | `GET /api/v1/private/account/getAccountAssetSnapshotPage` | 分页获取账户资产快照 |
| `UpdateLeverageSetting` | `POST /api/v1/private/account/updateLeverageSetting` | 更新账户杠杆设置 |


### 2. 资产服务 (Asset)

| SDK方法 | 接口路径 | 说明 |
|---------|---------|------|
| `GetAllOrdersPage` | `GET /api/v1/private/assets/getAllOrdersPage` | 聚合查询所有充提订单记录 |
| `GetNormalWithdrawById` | `GET /api/v1/private/assets/getNormalWithdrawById` | 根据ID获取普通提现订单 |
| `GetNormalWithdrawableAmount` | `GET /api/v1/private/assets/getNormalWithdrawableAmount` | 获取普通提现可用额度 |
| `CreateNormalWithdraw` | `POST /api/v1/private/assets/createNormalWithdraw` | 创建普通提现订单（自动L2签名） |
| `GetCrossWithdrawById` | `GET /api/v1/private/assets/getCrossWithdrawById` | 根据ID获取跨链提现订单 |
| `GetCrossWithdrawSignInfo` | `GET /api/v1/private/assets/getCrossWithdrawSignInfo` | 获取跨链提现签名所需信息 |
| `CreateCrossWithdraw` | `POST /api/v1/private/assets/createCrossWithdraw` | 创建跨链提现订单 |
| `GetCoinRate` | `GET /api/v1/private/assets/getCoinRate` | 获取币种汇率 |
| `GetFastWithdrawById` | `GET /api/v1/private/assets/getFastWithdrawById` | 根据ID获取快速提现订单 |
| `GetFastWithdrawSignInfo` | `GET /api/v1/private/assets/getFastWithdrawSignInfo` | 获取快速提现签名信息 |

---

### 3. 订单服务 (Order)

| SDK方法 | 接口路径 | 说明 |
|---------|---------|------|
| `CreateOrder` | `POST /api/v1/private/order/createOrder` | 创建订单（自动L2签名和价格计算） |
| `CancelOrder` | `POST /api/v1/private/order/cancelOrderById` 或 `cancelOrderByClientOrderId` 或 `cancelAllOrder` | 取消订单（支持多种方式） |
| `GetMaxOrderSize` | `POST /api/v1/private/order/getMaxCreateOrderSize` | 获取最大下单数量 |
| `GetOrdersByID` | `GET /api/v1/private/order/getOrderById` | 根据订单ID批量获取订单 |
| `GetOrdersByClientOrderID` | `GET /api/v1/private/order/getOrderByClientOrderId` | 根据客户端订单ID批量获取订单 |
| `GetActiveOrders` | `GET /api/v1/private/order/getActiveOrderPage` | 分页获取活跃订单 |
| `GetOrderFillTransactions` | `GET /api/v1/private/order/getHistoryOrderFillTransactionPage` | 分页获取历史成交明细 |

---

### 4. 划转服务 (Transfer)

| SDK方法 | 接口路径 | 说明 |
|---------|---------|------|
| `CreateTransferOut` | `POST /api/v1/private/transfer/createTransferOut` | 创建转出订单（自动L2签名） |
| `GetTransferOutById` | `GET /api/v1/private/transfer/getTransferOutById` | 根据ID获取转出订单 |
| `GetWithdrawAvailableAmount` | `GET /api/v1/private/transfer/getTransferOutAvailableAmount` | 获取可转出金额 |
| `GetTransferInById` | `GET /api/v1/private/transfer/getTransferInById` | 根据ID获取转入订单 |


---

## 三、WebSocket 接口

### 1. WebSocket 客户端 (ws.Client)

| 方法 | 说明 |
|------|------|
| `NewClient` | 创建WebSocket客户端（支持公开和私有连接） |
| `Connect` | 建立WebSocket连接 |
| `Close` | 关闭WebSocket连接 |
| `Subscribe` | 订阅频道（公开WebSocket） |
| `Unsubscribe` | 取消订阅频道（公开WebSocket） |
| `OnMessage` | 注册消息处理器 |
| `OnMessageHook` | 注册全局消息钩子 |
| `OnConnect` | 注册连接成功回调 |
| `OnDisconnect` | 注册连接断开回调 |

### 2. WebSocket 管理器 (ws.Manager)

| 方法 | 说明 |
|------|------|
| `ConnectPublic` | 连接公开WebSocket（市场数据） |
| `ConnectPrivate` | 连接私有WebSocket（账户数据） |
| `SubscribeMarketTicker` | 订阅行情数据 |
| `SubscribeKLine` | 订阅K线数据 |
| `SubscribeDepth` | 订阅订单簿深度 |
| `SubscribeTrades` | 订阅最新成交 |
| `OnPrivateMessage` | 处理私有消息（账户、订单、持仓更新等） |
| `OnPublicMessage` | 处理公开消息 |
| `Close` | 关闭所有连接 |

**使用示例:**
```go
// 创建WebSocket管理器
manager := &ws.Manager{}

// 连接公开WebSocket
err := manager.ConnectPublic(ctx)

// 订阅行情
err = manager.SubscribeMarketTicker(contractID, func(msg []byte) {
    // 处理行情数据
    fmt.Printf("Ticker: %s\n", string(msg))
})

// 订阅K线
err = manager.SubscribeKLine(contractID, "MINUTE_1", func(msg []byte) {
    // 处理K线数据
    fmt.Printf("KLine: %s\n", string(msg))
})

// 订阅深度
err = manager.SubscribeDepth(contractID, func(msg []byte) {
    // 处理深度数据
    fmt.Printf("Depth: %s\n", string(msg))
})

// 订阅成交
err = manager.SubscribeTrades(contractID, func(msg []byte) {
    // 处理成交数据
    fmt.Printf("Trades: %s\n", string(msg))
})

// 连接私有WebSocket
err = manager.ConnectPrivate(ctx)

// 处理私有消息
err = manager.OnPrivateMessage("ACCOUNT_UPDATE", func(msg []byte) {
    // 处理账户更新
    fmt.Printf("Account Update: %s\n", string(msg))
})

err = manager.OnPrivateMessage("ORDER_UPDATE", func(msg []byte) {
    // 处理订单更新
    fmt.Printf("Order Update: %s\n", string(msg))
})

// 关闭所有连接
manager.Close()
```

**私有WebSocket推送事件类型:**
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

**公开WebSocket频道类型:**
- `metadata` - 元数据订阅
- `ticker.{contractId}` - 指定合约24小时行情
- `ticker.all` - 所有合约行情
- `ticker.all.1s` - 所有合约行情(周期推送)
- `kline.{priceType}.{contractId}.{interval}` - K线数据
- `depth.{contractId}.{depth}` - 订单簿深度
- `trades.{contractId}` - 最新成交

---

## 四、SDK 特性

### 1. 自动签名处理
SDK内置StarkEx L2签名逻辑，自动处理以下操作的签名：
- ✅ 创建订单 (CreateOrder)
- ✅ 创建提现 (CreateNormalWithdraw)
- ✅ 创建划转 (CreateTransferOut)
- ✅ HTTP请求认证签名
- ✅ WebSocket连接认证签名

### 2. 元数据缓存
支持配置元数据缓存TTL，减少重复请求：
```go
client, err := sdk.NewClient(&sdk.ClientConfig{
    BaseURL:          "https://pro.edgex.exchange",
    AccountID:        accountID,
    StarkPriKey:      privateKey,
    MetaDataCacheTTL: &cacheTTL, // 可选，如 5 * time.Minute
})
```

### 3. 市价单智能处理
SDK自动计算市价单价格：
- **买单**: 使用预言机价格 × 10
- **卖单**: 使用最小tickSize

### 4. 错误处理
所有接口统一返回错误处理：
```go
result, err := client.CreateOrder(ctx, params)
if err != nil {
    // 处理错误
    log.Printf("Error: %v", err)
    return
}
// 使用结果
```

### 5. 类型安全
SDK提供完整的类型定义：
- 强类型参数结构体
- 枚举类型常量
- 完整的响应结构

---

## 五、快速开始

### 安装
```bash
go get github.com/edgex-Tech/edgex-golang-sdk
```

### 初始化客户端
```go
import (
    "github.com/edgex-Tech/edgex-golang-sdk/sdk"
)

// 创建客户端
client, err := sdk.NewClient(&sdk.ClientConfig{
    BaseURL:     "https://pro.edgex.exchange",
    AccountID:   123456789,
    StarkPriKey: "your_stark_private_key",
})
if err != nil {
    panic(err)
}
```

### 基本使用
```go
ctx := context.Background()

// 获取元数据
metadata, err := client.GetMetaData(ctx)

// 获取账户资产
asset, err := client.GetAccountAsset(ctx)

// 创建订单
order, err := client.CreateOrder(ctx, &order.CreateOrderParams{
    ContractId: "10000001",
    Price:      "50000.0",
    Size:       "0.1",
    Side:       order.OrderSideBuy,
    Type:       order.OrderTypeLimit,
    ExpireTime: time.Now().Add(24 * time.Hour),
})
```

---

## 六、接口统计

| 服务模块 | 接口数量 | 说明 |
|---------|---------|------|
| 元数据服务 | 2 | 服务器时间、全局元数据 |
| 行情服务 | 5 | 行情汇总、24h行情、K线、深度 |
| 资金费率服务 | 2 | 最新费率、历史费率 |
| 账户服务 | 13 | 资产、持仓、交易记录、快照等 |
| 资产服务 | 10 | 充提订单、提现、跨链提现等 |
| 订单服务 | 7 | 创建订单、取消订单、查询订单等 |
| 划转服务 | 4 | 转入、转出、可用额度查询 |
| WebSocket | 2个客户端 + 8个订阅方法 | 公开和私有数据推送 |
| **总计** | **43 个 HTTP 接口 + WebSocket 支持** | 完整的交易功能覆盖 |

---

## 七、版本信息

- **SDK版本**: v1.x
- **支持的API版本**: v1
- **更新时间**: 2026-03-10
