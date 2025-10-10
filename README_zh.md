# Solana 指令解析 MCP

## 概述

此项目提供了一个用于解析和分析 Solana 区块链指令的模型上下文协议 (MCP) 服务器。它利用超过 1,000 个程序的 IDL（接口定义语言）数据集来解码交易数据，支持 Anchor、Shank 和 Codama 等格式。MCP 与 AI 工具集成，以实现对 Solana 交易的深入分析。

## 功能

- **交易检索**：获取完整的 Solana 交易详情，包括内部指令。
- **指令分析**：解析交易中的特定指令，识别程序、函数和参数。
- **原始数据解析**：使用程序 ID 从十六进制或 base64 格式解码指令数据。
- **过滤分析**：可选按程序 ID 过滤分析交易。
- **程序子调用分析**：分析交易中指定程序的所有子调用。
- **账户数据解析**：检索并基于账户所有者解析账户信息。
- **多格式支持**：兼容 Anchor、Shank 和 Codama IDL。

## 主要功能

1. `get_solana_transaction`：通过签名检索和全面分析 Solana 交易。返回详细的信息，包括所有指令、账户余额变化、交易费用和执行元数据。
2. `analyze_solana_instruction`：深入分析 Solana 交易中的特定指令。解析指令数据，识别调用的程序和函数，提取参数，并提供安全风险评估。
3. `analyze_instruction_data`：解析原始 Solana 指令数据以提取函数名称和参数。支持已知程序的 IDL 解析，对未知程序自动回退到通用指令解析。
4. `get_transaction_with_inner_instructions`：检索 Solana 交易并递归解析所有内部指令 (CPI)。返回指令执行的层次视图，可选择按特定程序 ID 过滤。
5. `get_program_subcalls`：分析交易中指定程序的所有子调用。处理来自 message.compiledInstructions 的顶级指令和来自 tx.meta?.innerInstructions 的嵌套调用。
6. `get_account_data_with_parsing`：检索 Solana 账户信息并基于账户所有者解析。自动检测账户类型并为不同 Solana 程序提供程序特定分析。

## 使用方法

访问 MCP 服务器： [https://solmcp.daog1.workers.dev/](https://solmcp.daog1.workers.dev/)

使用如 MCP Inspector 等工具与功能交互。示例：

### 分析特定指令
- 签名：`4mSCpNds15mvygBex3aCfyMyZ8JYyzTuNikeYT2LgADH5DZgz6jnMZkLuwhAmqUHRVu2LXgmcVxXzeRvXPesPb5a`
- 指令索引：6
- 输出：Jupiter V6 route 指令的详情，包括 routePlan、inAmount 等参数。

### 解析指令数据
- 程序 ID：`JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4`
- 指令数据：`e517cb977ae3ad2a020000005f006400015f016401027e12de0e00000000def5fb0200000000320000`
- 输出：route 函数的解析参数。

### 获取包含内部指令的交易
- 签名：`4mSCpNds15mvygBex3aCfyMyZ8JYyzTuNikeYT2LgADH5DZgz6jnMZkLuwhAmqUHRVu2LXgmcVxXzeRvXPesPb5a`
- 过滤程序 ID：`["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4"]`
- 输出：Jupiter V6 的层次指令详情。

## 安装

无需本地安装。通过提供的 URL 访问。对于与 AI 工具集成，请配置 MCP 客户端连接到端点。

## API 参考

### analyze_solana_instruction
- **描述**：深入分析 Solana 交易中的特定指令。
- **参数**：
  - `signature`：交易签名（base58 编码，88 字符）
  - `instruction_index`：指令的零基索引
  - `rpc_endpoint`：可选 Solana RPC 端点（默认为 mainnet-beta）

### analyze_instruction_data
- **描述**：解析原始 Solana 指令数据。
- **参数**：
  - `program_id`：程序 ID（公钥）
  - `instruction_data`：原始数据为十六进制或 base64
  - `accounts`：可选账户公钥数组
  - `data_format`：'hex' 或 'base64'（默认 'hex'）
  - `idl_file`：可选 IDL 文件名

### get_transaction_with_inner_instructions
- **描述**：检索 Solana 交易并递归解析所有内部指令 (CPI)。返回指令执行的层次视图，可选择按特定程序 ID 过滤。
- **参数**：
  - `signature`：交易签名（base58 编码字符串，88 字符）
  - `filter_program_ids`：可选程序 ID 数组用于过滤结果 - 仅包括调用这些程序的指令
  - `rpc_endpoint`：可选 Solana RPC 端点 URL（如果未指定，默认为 mainnet-beta）

### get_program_subcalls
- **描述**：分析交易中指定程序的所有子调用。处理来自 message.compiledInstructions 的顶级指令和来自 tx.meta?.innerInstructions 的嵌套调用。
- **参数**：
  - `signature`：交易签名（base58 编码字符串，88 字符）
  - `program_ids`：用于过滤子调用的程序 ID 数组
  - `include_nested`：可选布尔值以包含来自 innerInstructions 的嵌套子调用（默认：true）
  - `rpc_endpoint`：可选 Solana RPC 端点 URL（如果未指定，默认为 mainnet-beta）

### get_account_data_with_parsing
- **描述**：检索 Solana 账户信息并基于账户所有者解析。自动检测账户类型并为不同 Solana 程序提供程序特定分析。
- **参数**：
  - `account`：账户公钥（base58 编码字符串）
  - `rpc_endpoint`：可选 Solana RPC 端点 URL（如果未指定，默认为 mainnet-beta）

## 示例

详细示例请参见原文：[Solana 指令解析 MCP 介绍](https://dev.to/xiaodao/introduction-to-solana-instruction-parsing-mcp-1mk6)

## 贡献

由 晓道 (Xiaodao) 构建。欢迎反馈和贡献。

## 许可证

许可证详情请参考原项目。

## 标签

#solana #anchor #shank #codama #blockchain #mcp #ai #cryptocurrency
