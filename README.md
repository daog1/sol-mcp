# Solana Instruction Parsing MCP

## Overview

This project provides a Model Context Protocol (MCP) server for parsing and analyzing Solana blockchain instructions. It leverages IDL (Interface Definition Language) datasets from over 1,000 programs to decode transaction data, supporting formats like Anchor, Shank, and Codama. The MCP integrates with AI tools to enable deep-dive analysis of Solana transactions, including inner instructions and security assessments.

## Features

- **Transaction Retrieval**: Fetch full Solana transaction details with inner instructions.
- **Instruction Analysis**: Parse specific instructions within a transaction, identifying programs, functions, and parameters.
- **Raw Data Parsing**: Decode instruction data from hex or base64 formats using program IDs.
- **Filtered Analysis**: Analyze transactions with optional filtering by program IDs.
- **Security Risk Assessment**: Provides insights into potential risks in instructions.
- **Support for Multiple Formats**: Compatible with Anchor, Shank, and Codama IDLs.

## Main Functions

1. `get_solana_transaction`: Retrieves basic transaction information.
2. `analyze_solana_instruction`: Deep-dive analysis of a specific instruction by index.
3. `analyze_instruction_data`: Parses raw instruction data for a given program ID.
4. `get_transaction_with_inner_instructions`: Hierarchical view of all instructions, including CPIs (Cross-Program Invocations).

## Usage

Access the MCP server at: [https://solmcp.daog1.workers.dev/](https://solmcp.daog1.workers.dev/)

Use tools like MCP Inspector to interact with the functions. Examples:

### Analyze Specific Instruction
- Signature: `4mSCpNds15mvygBex3aCfyMyZ8JYyzTuNikeYT2LgADH5DZgz6jnMZkLuwhAmqUHRVu2LXgmcVxXzeRvXPesPb5a`
- Instruction Index: 6
- Output: Details on Jupiter V6 route instruction with parameters like routePlan, inAmount, etc.

### Parse Instruction Data
- Program ID: `JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4`
- Instruction Data: `e517cb977ae3ad2a020000005f006400015f016401027e12de0e00000000def5fb0200000000320000`
- Output: Parsed parameters for the route function.

### Get Transaction with Inner Instructions
- Signature: `4mSCpNds15mvygBex3aCfyMyZ8JYyzTuNikeYT2LgADH5DZgz6jnMZkLuwhAmqUHRVu2LXgmcVxXzeRvXPesPb5a`
- Filter Program IDs: `["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4"]`
- Output: Hierarchical instruction details for Jupiter V6.

## Installation

No local installation required. Access via the provided URL. For integration with AI tools, configure MCP clients to connect to the endpoint.

## API Reference

### analyze_solana_instruction
- **Description**: Deep-dive analysis of a specific instruction within a Solana transaction.
- **Parameters**:
  - `signature`: Transaction signature (base58-encoded, 88 characters)
  - `instruction_index`: Zero-based index of the instruction
  - `rpc_endpoint`: Optional Solana RPC endpoint (defaults to mainnet-beta)

### analyze_instruction_data
- **Description**: Parse raw Solana instruction data.
- **Parameters**:
  - `program_id`: The program ID (public key)
  - `instruction_data`: Raw data as hex or base64
  - `accounts`: Optional array of account public keys
  - `data_format`: 'hex' or 'base64' (default 'hex')
  - `idl_file`: Optional IDL file name

### get_transaction_with_inner_instructions
- **Description**: Retrieve transaction with recursive inner instruction parsing.
- **Parameters**:
  - `signature`: Transaction signature
  - `filter_program_ids`: Optional array of program IDs to filter
  - `rpc_endpoint`: Optional RPC endpoint

## Examples

See the original article for detailed examples: [Introduction to Solana Instruction Parsing MCP](https://dev.to/xiaodao/introduction-to-solana-instruction-parsing-mcp-1mk6)

## Contributing

Built by 晓道 (Xiaodao). Feedback and contributions welcome. Visit the author's profile or contact via Telegram: https://t.me/realDAO

## License

Refer to the original project for licensing details.

## Tags

#solana #anchor #shank #codama #blockchain #mcp #ai #cryptocurrency