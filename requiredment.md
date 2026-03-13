
# 目标：生成 api-v2 接口文档
还是按照 private-api/public-api/websocket 去分类
wss 需要这两个接口 /api/v1/private/user/ws 这个接口暂时不需要
/api/v1/public/ws
/api/v1/private/ws

对应项目：/home/ec2-user/workspace/edgex-http-gateway sit-v2 分支

接口范围：
/home/ec2-user/workspace/edgex-gitbook/SDK接口清单.md 包括这个清单里面列出的 v1 版本 对应的接口还有如下接口：
/api/v2/private/account/updateLeverageSetting	POST	更新杠杆设置
/api/v2/private/account/getPositionOrders	GET	获取持仓订单
/api/v2/private/order/cancelOrderByClientOrderId	POST	根据客户端订单ID取消订单
/api/v2/private/order/getHistoryOrderPage	POST	分页获取历史订单(POST版本)


先按照 /home/ec2-user/workspace/edgex-gitbook/SDK接口清单.md 这个文档的模式生成 对应的 v2 版本接口清单

然后再本项目添加 v2-api 目录按照 v1 文档的组织形式生成 v2 版本的 API 文档；


/home/ec2-user/workspace/edgex-websocket-gateway wss 接口网关；对应的也是 sit-v2 分支； v1 版本的 wss 文档呈现形式并不好；需要把 wss 返回值的参数细节介绍清楚，然后在生成对应的请求值返回值势力；

## 接口对应的项目扫描报告
/home/ec2-user/workspace/edgex-gitbook/API接口扫描报告.md

接口实现细节需要去对应的项目 sit-v2 分支去查找；需要确定每个参数的是否必传属性；网关层代码可能无法判断，需要去到具体的业务实现类去判断；

## 要求

生成的文档中不要包含中文；字段描述要求简洁清晰，可以参考 v1 部分描述和 接口中 schema 文档中的描述字段；


注意当前 gitbook 版本，生成的 md 文件需要复符合当前版本能识别的语法；

---

# 中文实现方案

## 一、需求理解

### 目标
为 EdgeX 的 HTTP Gateway 和 WebSocket Gateway 生成完整的 V2 版本 API 英文文档，遵循现有 V1 文档的结构和风格。

### 范围

1. **HTTP Gateway API** (来源：`edgex-http-gateway` sit-v2 分支)
   - 公开接口（Public API）
   - 私有接口（Private API，需要认证）
   - 基于 `SDK接口清单.md` 中列出的 V1 接口对应的 V2 版本
   - 额外的 V2 新增接口：
     - `/api/v2/private/account/updateLeverageSetting` (POST) - 更新杠杆设置
     - `/api/v2/private/account/getPositionOrders` (GET) - 获取持仓订单
     - `/api/v2/private/order/cancelOrderByClientOrderId` (POST) - 根据客户端订单ID取消订单
     - `/api/v2/private/order/getHistoryOrderPage` (POST) - 分页获取历史订单(POST版本)

2. **WebSocket API** (来源：`edgex-websocket-gateway` sit-v2 分支)
   - `/api/v1/public/ws` - 公开市场数据
   - `/api/v1/private/ws` - 私有账户更新
   - 改进现有文档，提供详细的参数描述和完整示例
   - **注意**：`/api/v1/private/user/ws` 暂时不需要

### 核心要求
- 所有文档必须使用英文（不包含中文）
- 字段描述简洁清晰
- 遵循现有 V1 文档的格式和风格
- 兼容当前 GitBook 的 Markdown 语法
- 通过分析业务逻辑代码确定参数的必填/可选属性
- 参考 OpenAPI/Swagger 注解和 Schema 文档

### 参考资料
- `SDK接口清单.md` - SDK 接口映射关系
- `API接口扫描报告.md` - 接口扫描报告（实际接口数量需重新统计）
- 现有 V1 文档结构：`api/` 目录

---

## 二、实施步骤

### 阶段 1：环境准备与代码分析（步骤 1-3）

**步骤 1：访问并分析 edgex-http-gateway 项目 (sit-v2 分支)**
- 进入 `/home/ec2-user/workspace/edgex-http-gateway`
- 切换并验证 sit-v2 分支
- 定位 Controller 包：`exchange.edgex.code.gateway.controller.api.v2.pub` 和 `exchange.edgex.code.gateway.controller.api.v2.pri`
- 识别 OpenAPI/Swagger 注解和 Schema 定义
- 统计实际需要文档化的接口数量（基于 SDK清单 + 4个新增接口）

**步骤 2：访问并分析 edgex-websocket-gateway 项目 (sit-v2 分支)**
- 进入 `/home/ec2-user/workspace/edgex-websocket-gateway`
- 切换并验证 sit-v2 分支
- 定位 WebSocket 端点实现代码
- 分析消息 Schema 和事件类型定义
- 重点关注 `/api/v1/public/ws` 和 `/api/v1/private/ws` 两个接口

**步骤 3：分析微服务实现以确定参数验证规则**
- 对于每个端点，追踪 gRPC 服务调用到后端微服务
- 检查业务逻辑代码判断参数的必填/可选属性
- 记录验证规则、约束条件和默认值
- 涉及的微服务：edgex-trade-proxy, edgex-user-server, edgex-history-server, edgex-assets-server 等

### 阶段 2：生成 V2 SDK 接口清单（步骤 4）

**步骤 4：创建 V2 SDK 接口映射文档**
- 文件：`/home/ec2-user/workspace/edgex-gitbook/SDK接口清单-v2.md`
- 格式：遵循现有 `SDK接口清单.md` 的结构
- 内容：
  - 映射所有 V2 HTTP 端点到 SDK 方法
  - 按服务模块组织（公开接口、私有接口分类）
  - 包含 V2 新增的 4 个接口
  - 保持与 V1 格式一致（表格包含：SDK方法、接口路径、说明）

### 阶段 3：创建 V2 API 文档结构（步骤 5-10）

**步骤 5：创建 api-v2 目录结构**
```
/home/ec2-user/workspace/edgex-gitbook/
├── api-v2/
│   ├── README.md
│   ├── authentication.md（链接到现有文档或更新）
│   ├── sign.md（链接到现有文档或更新）
│   ├── public-api/
│   │   ├── README.md
│   │   ├── metadata-api.md
│   │   ├── quote-api.md
│   │   ├── funding-api.md
│   │   └── ... (其他公开接口)
│   ├── private-api/
│   │   ├── README.md
│   │   ├── account-api.md
│   │   ├── order-api.md
│   │   ├── asset-api.md
│   │   ├── transfer-api.md
│   │   └── ... (其他私有接口)
│   └── websocket-api/
│       ├── README.md
│       ├── public-websocket.md  (公开 WebSocket: /api/v1/public/ws)
│       └── private-websocket.md (私有 WebSocket: /api/v1/private/ws)
```

**步骤 6：生成公开接口 API 文档**
- 基于 SDK清单中的公开接口生成文档：
  - MetaDataPublicController → metadata-api.md (getServerTime, getMetaData)
  - QuotePublicController → quote-api.md (getDepth, getKline, getTicker 等)
  - FundingPublicController → funding-api.md (getFundingRatePage, getLatestFundingRate)
- 每个文件包含：
  - 接口名称和描述
  - HTTP 方法和路径
  - 请求参数表格（名称、位置、类型、必填、描述）
  - 响应示例（JSON 格式）
  - 响应结构表格
  - 数据模型说明

**步骤 7：生成私有接口 API 文档**
- 基于 SDK清单中的私有接口生成文档：
  - AccountPrivateController → account-api.md (账户相关接口，包含新增的 updateLeverageSetting 和 getPositionOrders)
  - OrderPrivateController → order-api.md (订单相关接口，包含新增的 cancelOrderByClientOrderId 和 POST版本的 getHistoryOrderPage)
  - AssetsPrivateController → asset-api.md (资产相关接口)
  - TransferPrivateController → transfer-api.md (划转相关接口)
  - 其他私有接口按需生成
- 文档结构与公开接口相同
- 注明需要认证

**步骤 8：生成增强版 WebSocket API 文档（拆分为两个文件）**

**8.1 公开 WebSocket 文档**
- 文件：`/home/ec2-user/workspace/edgex-gitbook/api-v2/websocket-api/public-websocket.md`
- 内容：`/api/v1/public/ws` 接口文档
- 包含：
  - 连接说明（无需认证）
  - Ping/Pong 心跳机制
  - 订阅/取消订阅机制
  - 各个频道详细说明：
    - metadata（元数据）
    - ticker.{contractId}（24小时行情）
    - kline.{priceType}.{contractId}.{interval}（K线数据）
    - depth.{contractId}.{depth}（订单簿深度）
    - trades.{contractId}（最新成交）
  - 每个频道包含：
    - 订阅请求格式
    - 订阅响应格式
    - 推送消息格式
    - 字段级别的详细参数说明
    - 完整的请求/响应示例
    - 数据类型说明（Snapshot, Changed 等）

**8.2 私有 WebSocket 文档**
- 文件：`/home/ec2-user/workspace/edgex-gitbook/api-v2/websocket-api/private-websocket.md`
- 内容：`/api/v1/private/ws` 接口文档
- 包含：
  - 连接认证说明（Web 和 App/API 两种方式）
  - Ping/Pong 心跳机制
  - 自动推送机制（无需订阅）
  - 所有事件类型详细说明：
    - Snapshot（快照数据）
    - ACCOUNT_UPDATE（账户更新）
    - DEPOSIT_UPDATE（充值更新）
    - WITHDRAW_UPDATE（提现更新）
    - TRANSFER_IN_UPDATE（转入更新）
    - TRANSFER_OUT_UPDATE（转出更新）
    - ORDER_UPDATE（订单更新）
    - FORCE_WITHDRAW_UPDATE（强制提现更新）
    - FORCE_TRADE_UPDATE（强制交易更新）
    - FUNDING_SETTLEMENT（资金费率结算）
    - ORDER_FILL_FEE_INCOME（订单成交手续费收入）
    - START_LIQUIDATING（开始强平）
    - FINISH_LIQUIDATING（完成强平）
  - 每个事件包含：
    - 触发条件
    - 消息结构
    - 字段级别的详细参数说明（重点加强）
    - data 对象中各个数组的字段说明
    - 完整的示例消息
  - 错误消息处理

**步骤 9：创建索引/README 文件**
- `api-v2/README.md`：V2 API 文档总览
- `api-v2/public-api/README.md`：公开接口索引
- `api-v2/private-api/README.md`：私有接口索引
- `api-v2/websocket-api/README.md`：WebSocket 接口索引，包含两个子文档的导航
- 每个 README 提供到子文档的导航链接

**步骤 10：更新 SUMMARY.md 用于 GitBook 导航**
- 添加 V2 API 章节到目录
- 保持一致的缩进和结构
- 链接所有新建的文档文件
- WebSocket API 作为子章节包含两个文档链接

### 阶段 4：文档内容生成（步骤 11-12）

**步骤 11：提取并格式化端点文档**

对于每个端点：

1. **从源代码提取信息**：
   - OpenAPI 注解（@Operation, @Parameter, @ApiResponse）
   - Controller 方法签名
   - Request/Response DTO 类定义
   - 验证注解（@NotNull, @NotEmpty, @Min, @Max 等）

2. **追踪到业务逻辑**：
   - 跟踪 gRPC 服务调用
   - 检查 Service 实现中的参数使用
   - 确定实际的必填/可选状态
   - 记录验证规则和约束条件

3. **格式化文档**：
   - 使用 GitBook 兼容的 Markdown 语法
   - 遵循 V1 文档风格
   - 使用表格展示参数和响应结构
   - 包含带有真实值的 JSON 示例
   - 添加锚点 ID 用于交叉引用

**步骤 12：质量保证和一致性检查**
- 验证所有描述均为英文（无中文）
- 检查相似端点之间的一致性
- 验证 GitBook Markdown 语法
- 确保参数描述简洁清晰
- 与 V1 文档交叉对照保持一致性
- 验证所有链接和锚点有效

### 阶段 5：最终审查与集成（步骤 13）

**步骤 13：构建并验证文档**
- 运行 GitBook 构建测试（如有工具）
- 检查断开的链接
- 验证导航结构
- 审查渲染后的文档格式
- 测试所有代码示例的有效性

---

## 三、交付成果

1. **V2 SDK 接口清单**：`SDK接口清单-v2.md`
   - 完整映射 V2 端点到 SDK 方法

2. **V2 API 文档结构**：`api-v2/` 目录
   - 公开接口文档（基于 SDK清单）
   - 私有接口文档（基于 SDK清单 + 4个新增接口）
   - 增强版 WebSocket API 文档（拆分为两个独立文件）
   - 支持文件（README、认证、签名说明）

3. **更新的导航**：修改后的 `SUMMARY.md`
   - 将 V2 文档集成到 GitBook 结构中

4. **质量标准**：
   - 所有内容均为英文
   - 遵循 V1 风格的一致格式
   - 准确的参数必填/可选标记
   - 全面的示例和描述
   - GitBook 兼容语法

---

## 四、技术方法

### 代码分析策略
1. **Controller 层**：提取端点定义、HTTP 方法、路径
2. **DTO 层**：提取请求/响应 Schema、字段类型、验证规则
3. **Service 层**：从业务逻辑判断参数要求
4. **微服务集成**：追踪 gRPC 调用了解完整参数使用情况

### 文档格式（基于 V1 风格）
```markdown
# [控制器名称]

<a id="opId[操作ID]"></a>

## [HTTP_方法] [端点描述]

[HTTP_方法] [/api/v2/路径/到/端点]

### Request Parameters

| Name | Location | Type | Required | Description |
|------|----------|------|----------|-------------|
| param1 | query/path/body | string | Yes/No | 参数1的描述 |

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": { ... },
  "msg": null
}
```

### Response

| Status Code | Description | Data Model |
|-------------|-------------|------------|
| 200 | OK | [Result](#schema) |
```

### 参数发现流程
1. 检查 `@RequestParam`, `@PathVariable`, `@RequestBody` 注解
2. 检查 `@NotNull`, `@NotEmpty` 验证注解
3. 追踪到 Service 层的业务验证
4. 记录 `defaultValue` 属性中的默认值
5. 注意 `@Min`, `@Max`, `@Pattern` 等约束

---

## 五、风险与应对

**风险 1**：实际需要文档化的接口数量不确定
- **应对**：先统计 SDK清单中列出的接口数量，加上 4 个新增接口，确定实际范围

**风险 2**：缺少部分微服务代码（edgex-opt-server, edgex-privy-fund-server）
- **应对**：仅基于网关 Controller 层生成文档；在无法确定后端验证的地方添加说明

**风险 3**：从代码中参数要求不明确
- **应对**：不明确时标记为"可选"；添加注释建议咨询后端服务

**风险 4**：GitBook 语法兼容性
- **应对**：频繁测试构建；参考现有 V1 文档使用经过验证的语法模式

**风险 5**：WebSocket 文档参数细节复杂
- **应对**：拆分为两个独立文件，每个接口单独详细说明；重点关注字段级别的参数描述

---

## 六、时间估算

- **阶段 1**（代码分析）：2-3 小时
- **阶段 2**（SDK 清单）：30 分钟
- **阶段 3**（结构创建）：30 分钟
- **阶段 4**（内容生成）：
  - HTTP 接口文档：4-6 小时（基于实际接口数量）
  - WebSocket 文档（两个文件）：2-3 小时
- **阶段 5**（质量保证与集成）：1-2 小时

**总计**：约 10-15 小时

---

## 七、关键调整说明

1. **接口数量**：不使用 API扫描报告 中的 197 个接口数量，而是基于 `SDK接口清单.md` 中列出的接口 + 4个新增接口
2. **WebSocket 文档拆分**：
   - 原计划：单个 `websocket-api.md` 文件
   - 调整为：`websocket-api/` 目录，包含三个文件：
     - `README.md`（索引）
     - `public-websocket.md`（`/api/v1/public/ws`）
     - `private-websocket.md`（`/api/v1/private/ws`）
   - 每个文件独立详细描述一个接口，包含完整的参数说明和示例
3. **重点关注**：WebSocket 文档需要特别加强参数细节描述，这是本次改进的重点

---

## 八、备注

- 优先处理 SDK清单中列出的端点，然后添加 4 个 V2 新增端点
- WebSocket 文档需要特别关注字段级别的详细参数描述
- 保持与 V1 命名约定和术语的一致性
- 所有时间戳使用毫秒（Unix 纪元时间）
- 使用来自 testnet 环境的真实示例值
- 在可用的地方记录错误代码和消息

---

# Implementation Plan

## Understanding of Requirements

### Objective
Generate comprehensive API v2 documentation for EdgeX HTTP Gateway and WebSocket Gateway, following the same structure and style as existing v1 documentation.

### Scope
1. **HTTP Gateway APIs** (from `edgex-http-gateway` sit-v2 branch):
   - Public API endpoints
   - Private API endpoints (authentication required)
   - Additional v2 endpoints not in v1:
     - `/api/v2/private/account/updateLeverageSetting` (POST)
     - `/api/v2/private/account/getPositionOrders` (GET)
     - `/api/v2/private/order/cancelOrderByClientOrderId` (POST)
     - `/api/v2/private/order/getHistoryOrderPage` (POST version)

2. **WebSocket APIs** (from `edgex-websocket-gateway` sit-v2 branch):
   - `/api/v1/public/ws` (public market data)
   - `/api/v1/private/ws` (private account updates)
   - Improve documentation with detailed parameter descriptions and examples

### Key Requirements
- All documentation in English (no Chinese)
- Field descriptions should be concise and clear
- Follow existing v1 documentation format and style
- Compatible with current GitBook syntax
- Determine required/optional parameters from business logic implementations
- Reference OpenAPI/Swagger schemas where available

### Reference Materials
- `/home/ec2-user/workspace/edgex-gitbook/SDK接口清单.md` - SDK interface mapping
- `/home/ec2-user/workspace/edgex-gitbook/API接口扫描报告.md` - API scanning report (197 endpoints in v2)
- Existing v1 documentation structure in `/home/ec2-user/workspace/edgex-gitbook/api/`

---

## Implementation Steps

### Phase 1: Environment Setup & Code Analysis (Steps 1-3)

**Step 1: Access and analyze the edgex-http-gateway project (sit-v2 branch)**
- Navigate to `/home/ec2-user/workspace/edgex-http-gateway`
- Checkout and verify sit-v2 branch
- Locate controller packages: `exchange.edgex.code.gateway.controller.api.v2.pub` and `exchange.edgex.code.gateway.controller.api.v2.pri`
- Identify OpenAPI/Swagger annotations and schemas

**Step 2: Access and analyze the edgex-websocket-gateway project (sit-v2 branch)**
- Navigate to `/home/ec2-user/workspace/edgex-websocket-gateway`
- Checkout and verify sit-v2 branch
- Locate WebSocket endpoint implementations
- Document message schemas and event types

**Step 3: Analyze microservice implementations for parameter validation**
- For each endpoint, trace gRPC service calls to backend microservices
- Check business logic to determine required vs optional parameters
- Document validation rules, constraints, and default values
- Reference services: edgex-trade-proxy, edgex-user-server, edgex-history-server, etc.

### Phase 2: Generate SDK Interface List v2 (Step 4)

**Step 4: Create V2 SDK interface mapping document**
- File: `/home/ec2-user/workspace/edgex-gitbook/SDK接口清单-v2.md`
- Format: Follow existing SDK接口清单.md structure
- Content:
  - Map all v2 HTTP endpoints to SDK method equivalents
  - Organize by service modules (Public API, Private API categories)
  - Include new v2-specific endpoints
  - Maintain consistency with v1 format (tables with SDK method, API path, description)

### Phase 3: Create v2 API Documentation Structure (Steps 5-10)

**Step 5: Create v2-api directory structure**
```
/home/ec2-user/workspace/edgex-gitbook/
├── api-v2/
│   ├── README.md
│   ├── authentication.md (link to existing or update)
│   ├── sign.md (link to existing or update)
│   ├── public-api/
│   │   ├── README.md
│   │   ├── metadata-api.md
│   │   ├── quote-api.md
│   │   ├── funding-api.md
│   │   └── ... (other public APIs)
│   ├── private-api/
│   │   ├── README.md
│   │   ├── account-api.md
│   │   ├── order-api.md
│   │   ├── asset-api.md
│   │   ├── transfer-api.md
│   │   ├── vault-api.md
│   │   ├── user-api.md
│   │   └── ... (other private APIs)
│   └── websocket-api.md
```

**Step 6: Generate Public API documentation files**
- For each public controller in API scan report:
  - MetaDataPublicController → metadata-api.md
  - QuotePublicController → quote-api.md
  - FundingPublicController → funding-api.md
  - IndexPublicController → index-api.md
  - VaultPublicController → vault-api.md
  - AccountPublicController → account-public-api.md
  - etc.
- Each file includes:
  - Endpoint name and description
  - HTTP method and path
  - Request parameters table (name, location, type, required, description)
  - Response examples (JSON)
  - Response schema table
  - Data models section

**Step 7: Generate Private API documentation files**
- For each private controller in API scan report:
  - AccountPrivateController → account-api.md
  - OrderPrivateController → order-api.md
  - AssetsPrivateController → asset-api.md
  - TransferPrivateController → transfer-api.md
  - UserPrivateController → user-api.md
  - VaultPrivateController → vault-api.md
  - WithdrawPrivateController → withdraw-api.md
  - DepositPrivateController → deposit-api.md
  - etc.
- Same structure as public API files
- Include authentication requirements

**Step 8: Generate enhanced WebSocket API documentation**
- File: `/home/ec2-user/workspace/edgex-gitbook/api-v2/websocket-api.md`
- Improve upon v1 documentation by:
  - Adding detailed parameter descriptions for each field
  - Providing comprehensive request/response examples
  - Documenting all event types with field-level details
  - Adding data type definitions for nested objects
  - Including connection authentication details
  - Documenting ping/pong mechanism clearly
  - Adding error handling examples

**Step 9: Create index/README files**
- `api-v2/README.md`: Overview of v2 API documentation
- `api-v2/public-api/README.md`: Public API index
- `api-v2/private-api/README.md`: Private API index
- Each README provides navigation to sub-documents

**Step 10: Update SUMMARY.md for GitBook navigation**
- Add v2 API section to table of contents
- Maintain consistent indentation and structure
- Link all new documentation files

### Phase 4: Documentation Content Generation (Steps 11-12)

**Step 11: Extract and format endpoint documentation**
For each endpoint:
1. Extract from source code:
   - OpenAPI annotations (@Operation, @Parameter, @ApiResponse)
   - Controller method signatures
   - Request/Response DTO classes
   - Validation annotations (@NotNull, @NotEmpty, @Min, @Max, etc.)

2. Trace to business logic:
   - Follow gRPC service calls
   - Check parameter usage in service implementations
   - Determine actual required/optional status
   - Document validation rules and constraints

3. Format documentation:
   - Use GitBook-compatible Markdown
   - Follow v1 documentation style
   - Use tables for parameters and response schemas
   - Include JSON examples with realistic values
   - Add anchor IDs for cross-referencing

**Step 12: Quality assurance and consistency check**
- Verify all English descriptions (no Chinese)
- Check consistency across similar endpoints
- Validate GitBook Markdown syntax
- Ensure parameter descriptions are concise and clear
- Cross-reference with v1 documentation for consistency
- Verify all links and anchors work

### Phase 5: Final Review and Integration (Step 13)

**Step 13: Build and verify documentation**
- Run `gitbook build` or equivalent to test
- Check for broken links
- Verify navigation structure
- Review rendered documentation for formatting issues
- Test all code examples for validity

---

## Deliverables

1. **SDK Interface List V2**: `SDK接口清单-v2.md`
   - Complete mapping of v2 endpoints to SDK methods

2. **V2 API Documentation Structure**: `api-v2/` directory
   - Public API documentation (10+ endpoints)
   - Private API documentation (100+ endpoints)
   - Enhanced WebSocket API documentation
   - Supporting files (README, authentication, signatures)

3. **Updated Navigation**: Modified `SUMMARY.md`
   - Integrated v2 documentation into GitBook structure

4. **Quality Standards**:
   - All content in English
   - Consistent formatting following v1 style
   - Accurate parameter required/optional flags
   - Comprehensive examples and descriptions
   - GitBook-compatible syntax

---

## Technical Approach

### Code Analysis Strategy
1. **Controller Layer**: Extract endpoint definitions, HTTP methods, paths
2. **DTO Layer**: Extract request/response schemas, field types, validations
3. **Service Layer**: Determine parameter requirements from business logic
4. **Microservice Integration**: Trace gRPC calls to understand full parameter usage

### Documentation Format (Based on V1 Style)
```markdown
# [ControllerName]

<a id="opId[operationId]"></a>

## [HTTP_METHOD] [Endpoint Description]

[HTTP_METHOD] [/api/v2/path/to/endpoint]

### Request Parameters

| Name | Location | Type | Required | Description |
|------|----------|------|----------|-------------|
| param1 | query/path/body | string | Yes/No | Description of param1 |

> Response Example

> 200 Response

```json
{
  "code": "SUCCESS",
  "data": { ... },
  "msg": null
}
```

### Response

| Status Code | Description | Data Model |
|-------------|-------------|------------|
| 200 | OK | [Result](#schema) |
```

### Parameter Discovery Process
1. Check `@RequestParam`, `@PathVariable`, `@RequestBody` annotations
2. Check `@NotNull`, `@NotEmpty` validation annotations
3. Trace to service layer for business validation
4. Document default values from `defaultValue` attributes
5. Note constraints from `@Min`, `@Max`, `@Pattern` etc.

---

## Risks and Mitigation

**Risk 1**: Missing microservice code (edgex-opt-server, edgex-privy-fund-server)
- **Mitigation**: Document based on gateway controller layer only; add notes where backend validation cannot be determined

**Risk 2**: Parameter requirements unclear from code
- **Mitigation**: Mark as "optional" if unclear; add note to consult backend service for validation details

**Risk 3**: GitBook syntax compatibility
- **Mitigation**: Test build frequently; reference existing v1 docs for proven syntax patterns

**Risk 4**: Large number of endpoints (197 total)
- **Mitigation**: Prioritize endpoints in SDK接口清单.md first; batch process similar endpoints; use templates for consistency

---

## Timeline Estimate

- **Phase 1** (Code Analysis): 2-3 hours
- **Phase 2** (SDK List): 30 minutes
- **Phase 3** (Structure): 30 minutes
- **Phase 4** (Content Generation): 6-8 hours (automated with manual review)
- **Phase 5** (QA & Integration): 1-2 hours

**Total**: ~10-14 hours of focused work

---

## Notes

- Focus on SDK清单 listed endpoints first, then add v2-specific endpoints
- WebSocket documentation needs special attention for detailed parameter descriptions
- Maintain consistency with v1 naming conventions and terminology
- All timestamps should be in milliseconds (Unix epoch)
- Use realistic example values from testnet environment
- Document error codes and messages where available

