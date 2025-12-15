# 数字货币策略工厂

基于遗传编程自动生成数字货币交易策略的工具，支持策略生成、回测评估和实盘交易。

## 版本信息

**版本：** v1.0  
**功能：** 使用遗传编程自动生成和优化数字货币交易策略，支持实盘交易和授权管理

## 主要特性

- ✅ 支持Binance和OKX交易所
- ✅ 自动生成和优化交易策略
- ✅ 策略回测和性能评估
- ✅ 策略代码自动生成
- ✅ 可视化表达式树
- ✅ 净值曲线分析
- ✅ **实盘交易功能** - 支持自动执行生成的策略
- ✅ **授权管理系统** - MAC地址绑定，支持按次授权和按月授权
- ✅ **丰富的技术指标** - 支持28种技术指标参与策略组合

## 安装依赖

### 1. 克隆或下载项目

```bash
git clone <repository_url>
cd crypto_strategy_factory
```

### 2. 安装Python依赖

```bash
pip install -r requirements.txt
```

主要依赖：
- `gplearn` - 遗传编程库
- `PyQt5` - GUI界面
- `pandas`, `numpy` - 数据处理
- `matplotlib` - 图表绘制

### 3. 配置交易所API（可选）

编辑 `user_config/exchange_config.json`：

```json
{
    "binance": {
        "api_key": "YOUR_BINANCE_API_KEY",
        "api_secret": "YOUR_BINANCE_API_SECRET",
        "testnet": false
    },
    "okx": {
        "api_key": "YOUR_OKX_API_KEY",
        "api_secret": "YOUR_OKX_API_SECRET",
        "passphrase": "YOUR_OKX_PASSPHRASE",
        "testnet": false
    }
}
```

**注意：** 公开数据（K线数据）通常不需要API密钥，但配置API密钥可以避免访问频率限制。

## 使用方法

### 启动程序

```bash
python main.py
```

### 使用步骤

1. **选择交易所和交易对**
   - 从下拉列表选择交易所（Binance或OKX）
   - 输入或选择交易对（如BTCUSDT）
   - 点击"刷新交易对列表"可以从交易所获取最新列表

2. **选择K线周期**
   - 选择K线周期（1m, 5m, 1h, 1d等）

3. **设置训练时间段**
   - 选择训练数据的开始和结束日期
   - 选择验证数据的开始和结束日期

4. **配置回测参数**
   - 初始资金（默认10000 USDT）
   - 手续费率（默认0.04%）
   - 杠杆倍数（默认1.0倍，支持1-125倍）
   - 合约乘数（默认1.0）

5. **设置GP参数**
   - 种群大小（建议100-500）
   - 迭代次数（建议50-100）
   - 锦标赛大小（建议20-50）
   - 简洁系数（控制表达式复杂度，建议0.001-0.01）

6. **设置适应度权重**（可选）
   - 收益率权重
   - 夏普比率权重
   - 回撤权重
   - 胜率权重

7. **开始生成策略**
   - 点击"开始生成"按钮
   - 等待GP进化完成
   - 查看生成的策略和回测结果

8. **导出策略**（需要授权）
   - 点击"导出策略"按钮
   - 系统会检查导出授权（按次授权，每次导出消耗一次）
   - 策略代码将保存到 `generated/strategies/` 目录

9. **实盘交易**（需要授权）
   - 点击"🚀 实盘交易"按钮
   - 选择要运行的策略
   - 配置交易参数（交易所、交易对、杠杆等）
   - 系统会检查实盘交易授权（3天体验版或按月授权）
   - 启动后自动执行策略，实时显示账户和持仓信息

10. **授权管理**
    - 点击"🔐 授权管理"按钮
    - 查看本机MAC地址
    - 激活授权码（从授权渠道获取）
    - 查看授权状态和使用情况

## 项目结构

```
crypto_strategy_factory/
├── main.py                      # 主入口文件
├── requirements.txt             # 依赖列表
├── README.md                    # 项目说明
│
├── api/                         # 交易所API（已包含）
│   ├── binance/                 # Binance API
│   └── okx/                     # OKX API
│
├── core/                        # 核心模块
│   ├── gp_engine.py            # GP引擎
│   ├── factor_evaluator.py     # 因子评估器
│   ├── factor_functions.py     # 因子函数库
│   └── strategy_generator.py   # 策略生成器
│
├── services/                    # 数据服务层
│   ├── exchange_client.py      # 交易所客户端基类
│   ├── binance_client.py       # Binance客户端
│   ├── okx_client.py           # OKX客户端
│   ├── symbol_utils.py         # 交易对工具函数
│   └── historical_data_downloader.py  # 历史数据下载器
│
├── data/                        # 数据存储
│   └── historical_data_storage.py  # 历史数据存储管理
│
├── ui/                          # UI界面
│   ├── strategy_factory_window.py  # 主窗口
│   ├── expression_tree_widget.py   # 表达式树组件
│   ├── live_trading_window.py      # 实盘交易窗口
│   └── license_dialog.py           # 授权管理对话框
│
├── utils/                       # 工具类
│   ├── logger.py               # 日志工具
│   ├── path_utils.py           # 路径工具
│   └── license_manager.py      # 授权管理器
│
├── tools/                       # 工具脚本
│   ├── generate_license_interactive.py  # 授权码生成工具
│   └── generate_license.bat            # 授权码生成批处理
│
├── user_config/                 # 用户配置
│   ├── exchange_config.json    # 交易所配置
│   └── backtest_config.json    # 回测配置
│
├── user_data/                   # 用户数据
│   └── historical_data/        # 历史数据缓存
│
├── generated/                   # 生成的策略
│   └── strategies/             # 策略代码
│
├── logs/                        # 日志文件
│
├── build.spec                   # PyInstaller打包配置
├── build.py                     # 打包脚本
├── build.bat                    # 打包批处理
│
└── docs/                        # 文档
    ├── implementation_plan.md   # 实施计划
    ├── project_structure.md     # 项目结构
    ├── license_system.md        # 授权系统说明
    ├── supported_indicators.md  # 技术指标说明
    ├── build_guide.md           # 打包指南
    └── ...
```

## 核心概念

### 遗传编程（GP）

遗传编程是一种进化计算技术，通过模拟自然选择过程来生成和优化程序（在本项目中是交易因子表达式）。

### 因子表达式

因子表达式是由技术指标和数学函数组成的公式，用于生成交易信号。例如：
```
add(mul(ma(close, 5), 0.5), sub(rsi(close, 14), 50))
```

### 技术指标支持

系统支持28种技术指标参与策略组合，包括：

**基础价格数据（4个）：**
- close（收盘价）、open（开盘价）、high（最高价）、low（最低价）

**技术指标（24个）：**
- 移动平均：MA、EMA
- 波动性：STD、ATR
- 动量指标：RSI、MACD、KDJ、CCI、Williams %R、ROC、Momentum、CMO
- 成交量指标：OBV、MFI、AD

详细说明请参考 [技术指标文档](docs/supported_indicators.md)

### 回测评估

生成的因子表达式会在历史数据上进行回测，评估以下指标：
- 总收益率
- 夏普比率
- 最大回撤
- 胜率
- 收益/回撤比

## 授权系统

### 授权类型

1. **导出策略授权（EXPORT）**
   - 按次授权，每次导出策略消耗一次
   - 可指定交易所限制（Binance或OKX）
   - 与MAC地址绑定

2. **实盘交易授权**
   - **体验版（LIVE_TRIAL）**：3天试用期
   - **正式版（LIVE）**：按月授权，最少1个月
   - 与MAC地址绑定

### 使用流程

1. 打开软件，点击"🔐 授权管理"
2. 查看本机MAC地址
3. 从授权渠道获取授权码
4. 在"激活授权"标签页中粘贴授权码并激活
5. 在"授权状态"标签页中查看授权状态

详细说明请参考 [授权系统文档](docs/license_system.md)

## 打包发布

### 打包方法

1. **使用批处理文件（推荐）**
   ```bash
   build.bat
   ```

2. **使用Python脚本**
   ```bash
   python build.py
   ```

3. **直接使用PyInstaller**
   ```bash
   pyinstaller build.spec
   ```

打包完成后，可执行文件位于 `dist/数字货币策略工厂.exe`

### 打包配置

- **单文件模式**：所有内容打包成一个exe文件
- **无控制台窗口**：GUI应用，不显示控制台
- **已禁用UPX和strip**：避免Windows环境下的工具依赖问题

详细说明请参考 [打包指南](docs/build_guide.md) 和 [README_BUILD.md](README_BUILD.md)

## 注意事项

1. **数据质量**
   - 确保K线数据完整且时间连续
   - 建议使用至少100条K线数据进行训练

2. **网络连接**
   - 首次使用需要下载历史数据
   - 需要稳定的网络连接
   - 某些地区可能需要代理

3. **API限制**
   - 交易所API可能有访问频率限制
   - 建议配置API密钥以避免限制

4. **策略风险**
   - 本工具生成的策略仅用于回测验证
   - 实盘交易存在风险，请谨慎使用
   - 建议在充分测试后再考虑实盘使用

5. **杠杆风险**
   - 杠杆倍数越高，风险和收益都放大
   - 建议在回测时使用合理的杠杆倍数（1x-10x）

6. **授权限制**
   - 授权码与MAC地址绑定，复制到其他电脑无法使用
   - 导出策略需要按次授权
   - 实盘交易需要有效授权（体验版或正式版）

## 常见问题

### Q: 如何获取交易所API密钥？

A: 
- Binance: https://www.binance.com/zh-CN/my/settings/api-management
- OKX: https://www.okx.com/account/my-api

### Q: 数据下载失败怎么办？

A: 
1. 检查网络连接
2. 检查交易对符号是否正确
3. 检查交易所API配置
4. 查看日志文件获取详细错误信息

### Q: GP进化时间太长怎么办？

A: 
1. 减少种群大小
2. 减少迭代次数
3. 减少训练数据量
4. 使用更短的K线周期

### Q: 生成的策略如何实盘使用？

A: 
1. 使用软件内置的"实盘交易"功能，直接运行策略
2. 或导出策略代码，集成到其他实盘交易系统
3. 建议在模拟环境中充分测试
4. 实盘交易存在风险，请谨慎使用
5. 实盘交易需要有效授权（3天体验版或按月授权）

### Q: 如何获取授权码？

A: 
1. 联系授权渠道购买授权码
2. 提供本机MAC地址（在"授权管理"中查看）
3. 根据需求选择授权类型：
   - 导出策略：按次授权，可指定交易所
   - 实盘交易：3天体验版或按月授权

### Q: 授权码可以在其他电脑使用吗？

A: 
不可以。授权码与MAC地址绑定，只能在授权的机器上使用。如需在其他电脑使用，需要为该电脑单独购买授权码。

### Q: 如何打包软件？

A: 
1. 确保已安装所有依赖：`pip install -r requirements.txt`
2. 安装PyInstaller：`pip install pyinstaller`
3. 运行打包脚本：`build.bat` 或 `python build.py`
4. 打包完成后，可执行文件在 `dist/` 目录

## 开发计划

- [x] 数据服务层实现
- [x] 核心GP引擎适配
- [x] UI界面适配
- [x] 主入口文件创建
- [x] 实盘交易功能
- [x] 授权管理系统
- [x] 技术指标扩展（28种指标）
- [x] 打包发布功能
- [ ] 端到端测试
- [ ] 性能优化
- [ ] 更多交易所支持

## 许可证

## 联系方式

VX:Bogutongjin20230618

本项目仅供学习和研究使用。

## 免责声明

本工具生成的策略仅用于回测验证，不构成投资建议。实盘交易存在风险，请谨慎使用。使用本工具进行实盘交易的一切后果由用户自行承担。

