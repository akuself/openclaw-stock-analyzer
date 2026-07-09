---
name: openclaw-stock-analyzer
description: >-
  Use this skill when the user asks for financial market data, stock trend analysis, 
  crypto currency historical data (such as ETH, Bitcoin), or requests to generate and 
  export K-line price trend charts. This skill triggers a local Python script to fetch 
  live data and output visual charts.
metadata:
  author: akuself
  version: v1.0.0
dependencies:
  native_skills:
    - data_visualizer
  native_tools:
    - yahoo_finance
  python_packages:
    - yfinance      # 🌟 用于获取雅虎财经的股票/加密货币历史 K 线数据
    - mplfinance    # 🌟 专门用于绘制高颜值、标准的金融K线图（蜡烛图）
    - matplotlib    # 🌟 底层绘图基础库，用于控制图表的画布、保存和样式渲染
    - pandas        # 🌟 用于处理和清洗时间序列数据（如计算均线、K线指标）
    - requests      # 🌟 网络请求库，防止本地 Python 脚本需要紧急抓取其他补充数据
custom_components:
  tools:
    - name: local_chart_exporter
      source: tools/chart_exporter.py
      handler: export_line_chart
      description: "接收价格和信号数据，导出为标准的 OpenClaw 折线图渲染包"
  sub_prompts:
    - name: trend_analyst
      source: prompts/trend_analyst.txt
---
# 这是一个为 OpenClaw Agent 生态量身定制的复合型技能（Composite Skill），旨在赋予 Agent 抓取金融市场数据并自动化导出 K 线趋势图表的能力。

## 💡 核心功能

* **金融数据获取**：支持获取指定股票（如 AAPL）或虚拟货币（如 ETH/以太坊）的近期历史交易数据。
* **趋势图表生成**：调用本地 Python 脚本自动处理数据，并在本地生成可视化 K 线/走势图表。
* **AI 策略分析**：结合内置的金融分析师提示词，由 Agent 针对图表及数据给出市场走势的直观判断。

## 🛠️ 本地依赖与安装要求

为了让本技能中的 Python 工具（`tools/chart_exporter.py`）在用户的 Mac/Windows 电脑上顺利跑通，请确保本地已安装以下 Python 核心依赖库：

```bash
pip install matplotlib pandas requests