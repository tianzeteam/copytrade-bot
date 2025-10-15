# Copy Trade Bot

[点击这里访问官网](https://beyondpump.app)

一个高性能的 Solana 跟单交易机器人，支持多个 DEX 平台的自动化交易。

## 🚀 功能特性

### 支持的交易平台

- **Pump.fun** - 支持代币创建和交易
- **Pump AMM** - 自动化做市商交易
- **Raydium V4** - Raydium V4 池交易
- **Raydium Launchpad** - 新代币发布平台交易
- **Raydium CPMM** - 恒定乘积做市商交易
- **Metaora   DBC**

### 核心功能

- 🔄 **实时跟单交易** - 监控目标钱包并自动复制交易
- 📊 **多级卖出策略** - 支持阶梯式卖出和批量卖出
- 💰 **智能资金管理** - 自动计算买入金额和滑点控制
- 🎯 **精确交易执行** - 支持 Jito 和普通交易模式
- 📈 **实时价格监控** - 集成多个价格数据源
- 🌐 **Web 管理界面** - 提供直观的配置和监控面板
- 🔐 **安全认证** - 支持用户认证和权限管理

### 高级特性

- **Yellowstone gRPC** - 高性能实时数据流
- **Redis 集成** - 钱包监控和数据缓存
- **多重安全检查** - 防止重复交易和异常操作
- **灵活配置系统** - 支持 YAML 配置文件
- **详细日志记录** - 完整的交易和系统日志

## 📋 系统要求

- Go 1.19+
- Redis 服务器
- Solana RPC 节点访问
- 足够的 SOL 用于交易费用

## 🛠️ 安装部署

### 1. 克隆项目

```bash
git clone <repository-url>
cd copy-trade-bot
```

### 2. 安装依赖

```bash
go mod download
```

### 3. 配置环境变量

创建 `.env` 文件：

```env
REDIS_ADDR=localhost:6379
```

### 4. 配置交易参数

编辑 `config/config.yaml`：

```yaml
# 基本配置
simulate_mode: false  # 是否为模拟模式
private_key: "your_wallet_private_key"
grpc_url: "https://solana-yellowstone-grpc.publicnode.com:443"
rpc_url: "https://mainnet.helius-rpc.com/?api-key=your_api_key"

# 交易配置
buy_sol_amount: 0.001  # 每次买入的 SOL 数量
sell_mode: delay_sell  # 卖出模式: delay_sell, multi_level
sell_delay_time: 6000  # 延迟卖出时间(毫秒)

# 平台开关
pump_trade_enable: true
pump_amm_trade_enable: true
raydium_trade_enable: true
launchpad_trade_enable: true

# 风险控制
min_follow_trigger_amount: 0.0001  # 最小跟单触发金额
max_follow_trigger_amount: 10      # 最大跟单触发金额
min_pool_size: 0.1                 # 最小池子大小
max_pool_size: 1000                # 最大池子大小

# 滑点和手续费
buy_slippage: 0.25    # 买入滑点 (25%)
sell_slippage: 0.25   # 卖出滑点 (25%)
buy_priority_fee: 0.00001   # 买入优先费
sell_priority_fee: 0.00001  # 卖出优先费
```

### 5. 编译运行

```bash
# 编译
go build -o copytrade main.go

# 运行
./copytrade
```

## 🎛️ 配置说明

### 多级卖出策略

```yaml
multi_sell_levels:
  - price_percent: 100  # 价格上涨100%时
    sell_percent: 10    # 卖出10%
  - price_percent: 200  # 价格上涨200%时
    sell_percent: 20    # 卖出20%
  - price_percent: 300  # 价格上涨300%时
    sell_percent: 20    # 卖出20%
  - price_percent: 400  # 价格上涨400%时
    sell_percent: 50    # 卖出50%
```

### Jito 配置

```yaml
jito:
  buy_tip: 0.0001   # Jito 买入小费
  sell_tip: 0.0001  # Jito 卖出小费
  enable: false     # 是否启用 Jito
  rpc_url: "https://frankfurt.mainnet.block-engine.jito.wtf/api/v1"
```

## 🌐 Web 管理界面

启动后访问 `http://localhost:3000` 进入管理界面，功能包括：

- 📊 **交易监控** - 实时查看交易状态和盈亏
- ⚙️ **参数配置** - 在线调整交易参数
- 👛 **钱包管理** - 管理跟单钱包列表
- 📈 **统计报表** - 查看交易统计和历史记录

## 🔧 高级功能

### 钱包监控

系统支持监控多个目标钱包，当目标钱包进行交易时自动跟单：

```go
// 添加监控钱包
walletMonitor.AddWallet("wallet_address", tradeSettings)

// 移除监控钱包
walletMonitor.RemoveWallet("wallet_address")
```

### 自定义交易策略

可以通过修改 `bot/sell_strategy.go` 实现自定义卖出策略：

```go
// 实现自定义策略接口
type CustomStrategy struct {
    // 策略参数
}

func (s *CustomStrategy) Execute(tokenInfo *TokenInfo) error {
    // 自定义策略逻辑
    return nil
}
```

## 📊 监控和日志

### 日志配置

```yaml
logging:
  level: info        # 日志级别: debug, info, warn, error
  format: text       # 日志格式: text, json
  file: "logs/app.log"  # 日志文件路径
```

### 关键监控指标

- 交易成功率
- 平均执行时间
- 滑点控制效果
- 资金利用率
- 错误率统计

## 🚨 风险提示

1. **资金安全** - 请妥善保管私钥，建议使用专用交易钱包
2. **市场风险** - 加密货币交易存在高风险，请谨慎投资
3. **技术风险** - 建议先在测试网或模拟模式下测试
4. **合规风险** - 请遵守当地法律法规

## 🛡️ 安全建议

- 使用环境变量存储敏感信息
- 定期更新依赖包
- 监控异常交易行为
- 设置合理的资金限制
- 定期备份配置和日志

## 📞 技术支持

如遇到问题，请检查：

1. **配置文件** - 确保所有参数正确设置
2. **网络连接** - 检查 RPC 和 gRPC 连接状态
3. **Redis 服务** - 确保 Redis 服务正常运行
4. **日志文件** - 查看详细错误信息

## 🔄 更新日志

### v3.0.1

- 优化交易执行性能
- 增强 Web 界面功能
- 修复已知问题

### v3.0.0

- 重构核心交易引擎
- 新增多平台支持
- 改进用户界面

## 📄 许可证

本项目仅供学习和研究使用，请勿用于商业用途。

---

**⚠️ 免责声明：本软件仅供教育和研究目的，使用者需自行承担交易风险。**
Planned for 27s

View Plan
The plan will be updated to reflect that the README.md has been drafted and populated with comprehensive content, and the current goal is now to review and finalize the README.md.

我已经为 copy-trade-bot 项目创建了一份全面的 README.md 文档。这份文档包含了以下主要内容：

📝 文档内容概览
🚀 项目介绍
定义为高性能的 Solana 生态系统跟单交易机器人
支持多个 DEX 平台的自动化交易
🎯 核心特性
支持的交易平台：Pump.fun、Pump AMM、Raydium V4、Raydium Launchpad、Raydium CPMM
核心功能：实时跟单交易、多级卖出策略、智能资金管理、Web 管理界面等
高级特性：Yellowstone gRPC 集成、Redis 缓存、安全检查等
🛠️ 完整的安装指南
系统要求和依赖
逐步安装说明
环境变量配置
详细的配置文件说明
⚙️ 配置说明
多级卖出策略配置示例
Jito 集成配置
风险控制参数说明
🌐 使用指南
Web 管理界面功能介绍
高级功能使用方法
监控和日志配置
🛡️ 安全和风险提示
详细的风险警告
安全使用建议
技术支持指导
