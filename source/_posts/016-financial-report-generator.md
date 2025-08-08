---
title: MCP实战：从零开始构建财务报告生成工具
date: 2025-07-31 20:00:00
categories: AI开发
tags:
  - MCP
  - 财务报告
  - AI工具
  - 自动化
  - 技术实战
abbrlink: financial-report-generator
cover: /images/kennanAi.png
---

## 🚀 前言

Model Context Protocol (MCP) 是一个开放协议，允许AI模型与外部工具和数据源进行安全、结构化的交互。本文将详细介绍如何从零开始构建一个基于MCP的财务报告生成工具，通过实际项目展示MCP的接入流程和核心概念。

> 本文首发于公众号「恺南AI实战派」，关注获取更多AI实战技术分享

## 🎯 项目概述

我们构建了一个完整的MCP服务器，提供交互式公司财报生成功能。项目支持10家知名公司（如NVIDIA、Apple、Microsoft等），能够生成包含财务数据、业务分析、风险评估等内容的详细财报。

**项目源码**: [https://github.com/KennanYang/financial-report](https://github.com/KennanYang/financial-report)

---

## 💡 MCP核心概念

### 什么是MCP？

MCP（Model Context Protocol）是一个开放协议，定义了AI模型与外部工具和数据源交互的标准方式。它包含两个核心概念：

1. **Tools（工具）**: 可调用的函数，具有明确的输入输出
2. **Resources（资源）**: 可读取的数据或文件，具有URI格式

### MCP架构

```
┌─────────────┐    MCP Protocol    ┌─────────────┐
│   AI Model  │ ◄────────────────► │ MCP Server  │
│  (Client)   │                    │             │
└─────────────┘                    └─────────────┘
                                            │
                                            ▼
                                    ┌─────────────┐
                                    │   Tools &   │
                                    │ Resources   │
                                    └─────────────┘
```

---

## 🔧 实战：构建MCP财务报告工具

### 第一步：环境准备

首先安装必要的依赖：

```bash
pip install mcp pydantic asyncio
```

### 第二步：创建MCP工具

我们以公司财报生成工具为例，展示MCP工具的实现：

```python
# src/mcp_server/tools/company_report_generator.py

import asyncio
import logging
from datetime import datetime
from pathlib import Path
from typing import Any, Dict, List

from mcp.types import (
    CallToolRequest,
    CallToolResult,
    ListToolsRequest,
    ListToolsResult,
    TextContent,
    Tool,
)

class CompanyReportGenerator:
    """公司财报生成工具"""
    
    def __init__(self):
        self.logger = logging.getLogger(__name__)
        self.output_dir = Path("output")
        self.output_dir.mkdir(exist_ok=True)
        
        # 预定义的公司列表
        self.companies = {
            "NVDA": {"name": "NVIDIA Corporation", "sector": "Technology", "industry": "Semiconductors"},
            "AAPL": {"name": "Apple Inc.", "sector": "Technology", "industry": "Consumer Electronics"},
            "MSFT": {"name": "Microsoft Corporation", "sector": "Technology", "industry": "Software"},
            # ... 更多公司
        }
    
    async def list_tools(self, request: ListToolsRequest) -> ListToolsResult:
        """列出可用的MCP工具"""
        tools = [
            Tool(
                name="list_companies",
                description="列出可用的公司列表",
                inputSchema={
                    "type": "object",
                    "properties": {},
                    "required": []
                }
            ),
            Tool(
                name="generate_company_report",
                description="生成指定公司的财务报告",
                inputSchema={
                    "type": "object",
                    "properties": {
                        "symbol": {
                            "type": "string",
                            "description": "公司股票代码"
                        },
                        "report_type": {
                            "type": "string",
                            "enum": ["annual", "quarterly"],
                            "description": "报告类型"
                        }
                    },
                    "required": ["symbol"]
                }
            )
        ]
        return ListToolsResult(tools=tools)
```

### 第三步：实现工具功能

```python
async def call_tool(self, request: CallToolRequest) -> CallToolResult:
    """调用MCP工具"""
    if request.name == "list_companies":
        return await self._list_companies()
    elif request.name == "generate_company_report":
        return await self._generate_company_report(request.arguments)
    else:
        raise ValueError(f"Unknown tool: {request.name}")

async def _list_companies(self) -> CallToolResult:
    """列出可用公司"""
    companies_list = []
    for symbol, info in self.companies.items():
        companies_list.append(f"{symbol}: {info['name']} ({info['sector']})")
    
    content = "可用公司列表：\n" + "\n".join(companies_list)
    return CallToolResult(
        content=[TextContent(type="text", text=content)]
    )

async def _generate_company_report(self, arguments: Dict[str, Any]) -> CallToolResult:
    """生成公司财报"""
    symbol = arguments.get("symbol", "").upper()
    report_type = arguments.get("report_type", "annual")
    
    if symbol not in self.companies:
        return CallToolResult(
            content=[TextContent(type="text", text=f"错误：未找到公司 {symbol}")]
        )
    
    company_info = self.companies[symbol]
    
    # 生成财报内容
    report_content = f"""
# {company_info['name']} ({symbol}) 财务报告

## 公司概况
- **行业**: {company_info['industry']}
- **板块**: {company_info['sector']}
- **报告类型**: {report_type}
- **生成时间**: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}

## 财务摘要
[此处包含财务数据和分析]

## 业务分析
[此处包含业务发展分析]

## 风险评估
[此处包含风险评估内容]

## 投资建议
[此处包含投资建议]
"""
    
    # 保存报告文件
    filename = f"{symbol}_{report_type}_report_{datetime.now().strftime('%Y%m%d_%H%M%S')}.md"
    filepath = self.output_dir / filename
    with open(filepath, "w", encoding="utf-8") as f:
        f.write(report_content)
    
    return CallToolResult(
        content=[TextContent(type="text", text=f"报告已生成：{filepath}\n\n{report_content}")]
    )
```

### 第四步：创建MCP服务器

```python
# src/mcp_server/main.py

import asyncio
import logging
from pathlib import Path

from mcp.server import Server
from mcp.server.stdio import stdio_server

from .tools.company_report_generator import CompanyReportGenerator

async def main():
    # 配置日志
    logging.basicConfig(level=logging.INFO)
    logger = logging.getLogger(__name__)
    
    # 创建MCP服务器
    server = Server("financial-report-generator")
    
    # 注册工具
    report_generator = CompanyReportGenerator()
    server.list_tools(report_generator.list_tools)
    server.call_tool(report_generator.call_tool)
    
    # 启动服务器
    async with stdio_server() as (read_stream, write_stream):
        await server.run(
            read_stream,
            write_stream,
            server.create_initialization_options()
        )

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 🎯 项目结构

```
financial-report/
├── src/
│   └── mcp_server/
│       ├── __init__.py
│       ├── main.py
│       └── tools/
│           ├── __init__.py
│           └── company_report_generator.py
├── output/
├── requirements.txt
├── README.md
└── setup.py
```

---

## 🚀 使用示例

### 1. 启动MCP服务器

```bash
cd financial-report
python -m src.mcp_server.main
```

### 2. 客户端调用

```python
import asyncio
from mcp.client.stdio import stdio_client

async def test_mcp_server():
    async with stdio_client() as (read_stream, write_stream):
        # 列出可用工具
        tools_response = await client.list_tools()
        print("可用工具:", tools_response.tools)
        
        # 生成财报
        report_response = await client.call_tool(
            name="generate_company_report",
            arguments={"symbol": "NVDA", "report_type": "annual"}
        )
        print("报告内容:", report_response.content)

if __name__ == "__main__":
    asyncio.run(test_mcp_server())
```

---

## 💡 技术要点

### 1. MCP协议规范

- **工具定义**：使用JSON Schema定义工具输入输出
- **异步处理**：所有工具调用都是异步的
- **错误处理**：完善的异常处理机制

### 2. 数据安全

- **输入验证**：严格验证用户输入
- **文件隔离**：输出文件独立存储
- **权限控制**：限制文件访问权限

### 3. 扩展性设计

- **模块化**：工具功能模块化设计
- **插件化**：支持动态添加新工具
- **配置化**：支持配置文件管理

---

## 🎯 项目成果

### 核心功能
1. **公司列表管理**：支持10家知名公司
2. **财报生成**：自动生成详细财务报告
3. **多格式输出**：支持Markdown、PDF等格式
4. **实时数据**：集成实时财务数据

### 技术特色
- **MCP标准**：完全遵循MCP协议规范
- **异步架构**：高性能异步处理
- **模块化设计**：易于扩展和维护
- **完整文档**：详细的API文档和使用说明

---

## 🔮 未来展望

### 功能扩展
1. **更多公司**：扩展到更多上市公司
2. **数据分析**：集成更深入的财务分析
3. **可视化**：添加图表和可视化功能
4. **多语言**：支持多语言报告生成

### 技术优化
1. **性能优化**：提升生成速度
2. **缓存机制**：添加数据缓存
3. **分布式**：支持分布式部署
4. **监控告警**：添加系统监控

---

## 💭 个人感悟

通过这个MCP财务报告生成工具的开发，我深刻体会到了MCP协议的价值：

1. **标准化**：MCP提供了AI工具的标准接口
2. **互操作性**：不同AI模型可以无缝使用同一工具
3. **安全性**：结构化的交互方式提高了安全性
4. **扩展性**：易于添加新功能和工具

MCP不仅是一个协议，更是AI工具生态的重要基础设施。它让我们能够构建更加智能、安全、可扩展的AI应用。

---

## 📞 交流互动

如果你也对MCP技术感兴趣，欢迎：

1. **访问项目仓库**：[GitHub项目地址](https://github.com/KennanYang/financial-report)
2. **查看在线演示**：[项目演示](https://github.com/KennanYang/financial-report)
3. **分享你的项目**：一起探讨MCP开发的可能性

---

## 🎯 写在最后

MCP技术为AI工具开发带来了新的可能性。通过标准化的协议，我们可以构建更加智能、安全、可扩展的AI应用。

这个财务报告生成工具只是一个开始，MCP的潜力远不止于此。期待看到更多基于MCP的创新应用！

**关注我，看一个非天才解题者的AI实战实录！**

---

*本文为恺南AI实战派原创，转载请注明出处。*
