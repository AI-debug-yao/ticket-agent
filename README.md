# CASE Ticket & Revenue Assistant

北京环球影城门票订单和餐饮营收智能分析助手，基于 Qwen Agent 构建的智能对话系统。

## 项目简介

本项目提供两个核心功能模块：
- **门票订单分析助手**：分析门票销售数据、用户画像、销售渠道等
- **餐饮营收分析助手**：分析餐饮消费数据、用户类型、影响因素等

系统支持自然语言交互，自动生成 SQL 查询，执行数据分析，并提供可视化图表展示。

## 功能特性

### 门票助手功能
- 🎫 门票销售统计（一日票/二日票）
- 👥 用户画像分析（年龄、性别、地区分布）
- 📊 销售渠道分析
- 📈 时间序列分析（周/月统计）
- 📉 自动数据可视化（柱状图、堆积图）

### 餐饮营收助手功能
- 🍽️ 餐饮营收统计与分析
- 👤 不同用户类型人均消费计算（年卡/门票/促销票）
- 🎭 活动期间消费分析（春节、王者荣耀、酷夏、万圣节等）
- 🌡️ 影响因素分析（天气、节假日、票价、促销等）
- 🌲 决策树模型分析关键影响因素
- 📊 智能图表生成

## 环境要求

- Python 3.8 - 3.11
- 阿里云 DashScope API Key

## 安装依赖

```bash
# 使用您的 Python 解释器安装依赖
python3 -m pip install -r requirements.txt
```

## 配置

1. 复制或使用已创建的 `.env` 文件
2. 在 `.env` 文件中配置您的 DashScope API Key：

```env
# DashScope API Key (必填)
DASHSCOPE_API_KEY=your_actual_api_key_here

# DashScope Timeout (可选，默认 30 秒)
DASHSCOPE_TIMEOUT=30
```

**重要**：请将 `your_actual_api_key_here` 替换为您从阿里云 DashScope 获取的真实 API Key！

## 使用

### 门票助手

项目包含三个版本的门票助手，您可以根据需要选择：

#### 1. assistant_ticket_bot-1.py
**基础版本** - 轻量级查询工具
- 仅执行 SQL 查询
- 返回 Markdown 表格数据
- 不包含可视化功能
- 适合快速数据查询

```bash
python3 assistant_ticket_bot-1.py
```

**典型问题示例：**
- "查询2023年4月份的门票销售情况"
- "统计不同省份的用户数量"

#### 2. assistant_ticket_bot-2.py
**增强版本** - 基础可视化
- 执行 SQL 查询
- 自动生成简单柱状图
- 支持多数据系列对比
- 自动推断 X/Y 轴字段

```bash
python3 assistant_ticket_bot-2.py
```

**典型问题示例：**
- "2023年4、5、6月一日门票，二日门票的销量多少？帮我按照周进行统计"
- "2023年7月的不同省份的入园人数统计"

#### 3. assistant_ticket_bot-3.py
**高级版本** - 丰富可视化
- 执行 SQL 查询
- 自动生成堆积柱状图
- 优化中文字体显示
- 支持复杂数据透视

```bash
python3 assistant_ticket_bot-3.py
```

**典型问题示例：**
- "帮我查看2023年10月1-7日销售渠道订单金额排名"
- "按月份统计不同类型门票的销售趋势"

### 餐饮营收助手

功能最全面的分析工具，包含机器学习分析能力：

```bash
python3 assistant_revenue_bot.py
```

**核心功能：**

1. **SQL 查询** - 灵活查询餐饮营收数据
2. **人均消费计算** - 计算特定用户类型在特定活动期间的人均餐饮消费
3. **影响因素分析** - 使用决策树模型分析哪些因素对餐饮消费影响最大
4. **自定义绘图** - 支持输入 Python 代码生成自定义图表

**典型问题示例：**
- "请计算万圣节期间的年卡用户园内人均餐饮消费"
- "分析哪些因素对餐饮服务营收的变大影响较大，诸如大型活动、节假日、票价、促销、天气等"
- "分析哪些因素对餐饮平均消费的变大影响较大"
- "统计2023年各月份的餐饮总营收"

## 使用流程

1. 启动相应的助手程序
2. 浏览器自动打开 Web UI 界面（或手动访问提示的地址）
3. 在聊天界面输入您的问题（可使用预设的建议问题）
4. 助手自动分析问题，生成 SQL，执行查询，返回结果和可视化图表

## 技术栈

- **大语言模型**：Qwen Turbo（阿里云 DashScope）
- **Agent 框架**：Qwen Agent
- **数据库**：MySQL（阿里云 RDS）
- **数据处理**：Pandas, NumPy
- **可视化**：Matplotlib
- **机器学习**：Scikit-learn（线性回归、决策树）
- **Web UI**：Qwen Agent GUI

## 数据库说明

项目连接到阿里云 RDS MySQL 数据库，包含两个核心表：

### tkt_orders（门票订单表）
- order_time: 订单日期
- account_id: 预订用户ID
- gov_id: 商品使用人身份证号
- gender: 使用人性别
- age: 年龄
- province: 使用人省份
- SKU: 商品SKU名
- product_serial_no: 商品ID
- eco_main_order_id: 订单ID
- sales_channel: 销售渠道
- status: 商品状态
- order_value: 订单金额
- quantity: 商品数量

### ubr_revenue（餐饮营收表）
- date: 日期
- ticket_price: 票价
- operating_hours: 运营时长
- total_attendance: 总入园人数
- ap_attendance: 年卡入园人数
- ticket_attendance: 门票入园人数
- promotional_ticket_attendance: 促销票入园人数
- media_cost_index: 媒体成本指数
- marquee_event: 大型活动
- max_temperature/min_temperature: 最高/最低温度
- week_days: 星期
- is_national_holiday: 是否法定假日
- beijing_guest_ratio: 北京游客比例
- age_group_*: 各年龄段比例
- total_fb_revenue: 总餐饮消费
- rev_per_cap: 人均餐饮消费

## 项目结构

```
CASE-ticket-agent/
├── .env                          # 环境变量配置
├── .gitignore                    # Git 忽略文件
├── README.md                     # 项目说明文档
├── requirements.txt              # Python 依赖
├── assistant_ticket_bot-1.py     # 门票助手（基础版，无可视化）
├── assistant_ticket_bot-2.py     # 门票助手（增强版，简单柱状图）
├── assistant_ticket_bot-3.py     # 门票助手（高级版，丰富可视化）
├── assistant_revenue_bot.py      # 餐饮营收助手（含机器学习分析）
└── image_show/                   # 生成的可视化图表保存目录
```

## 常见问题

**Q: 如何获取 DashScope API Key？**
A: 访问阿里云 DashScope 控制台（https://dashscope.console.aliyun.com/）注册并创建 API Key。

**Q: 图表保存在哪里？**
A: 所有生成的图表自动保存在 `image_show/` 目录下，文件名包含时间戳。

**Q: 支持哪些活动类型分析？**
A: 支持：无活动、春节、王者荣耀、酷夏、万圣节恐怖夜。

