# Solana Instruction Parsing MCP

## Overview

This project provides a Model Context Protocol (MCP) server for parsing and analyzing Solana blockchain instructions. It leverages IDL (Interface Definition Language) datasets from over 1,000 programs to decode transaction data, supporting formats like Anchor, Shank, and Codama. The MCP integrates with AI tools to enable deep-dive analysis of Solana transactions, including inner instructions and security assessments.

## Features

- **Transaction Retrieval**: Fetch full Solana transaction details with inner instructions.
- **Instruction Analysis**: Parse specific instructions within a transaction, identifying programs, functions, and parameters.
- **Raw Data Parsing**: Decode instruction data from hex or base64 formats using program IDs.
- **Filtered Analysis**: Analyze transactions with optional filtering by program IDs.
- **Program Subcalls Analysis**: Analyze all subcalls from specified programs in a transaction.
- **Account Data Parsing**: Retrieve and parse account information based on account owner.
- **Security Risk Assessment**: Provides insights into potential risks in instructions.
- **Support for Multiple Formats**: Compatible with Anchor, Shank, and Codama IDLs.

## Main Functions

1. `get_solana_transaction`: Retrieve and comprehensively analyze a Solana transaction by signature. Returns detailed information including all instructions, account balance changes, transaction fees, and execution metadata.
2. `analyze_solana_instruction`: Deep-dive analysis of a specific instruction within a Solana transaction. Parses the instruction data, identifies the program and function called, extracts parameters, and provides security risk assessment.
3. `analyze_instruction_data`: Parse raw Solana instruction data to extract function names and parameters. Supports IDL-based parsing for known programs, with automatic fallback to generic instruction parsing for unknown programs.
4. `get_transaction_with_inner_instructions`: Retrieve a Solana transaction and recursively parse all inner instructions (CPIs). Returns a hierarchical view of instruction execution with optional filtering by specific program IDs.
5. `get_program_subcalls`: Analyze all subcalls from specified programs in a transaction. Processes both top-level instructions from message.compiledInstructions and nested calls from tx.meta?.innerInstructions.
6. `get_account_data_with_parsing`: Retrieve Solana account information and parse it based on the account owner. Automatically detects account type and provides program-specific analysis for different Solana programs.

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
- **Description**: Retrieve a Solana transaction and recursively parse all inner instructions (CPIs). Returns a hierarchical view of instruction execution with optional filtering by specific program IDs.
- **Parameters**:
  - `signature`: Transaction signature (base58-encoded string, 88 characters)
  - `filter_program_ids`: Optional array of program IDs to filter results - only instructions calling these programs will be included
  - `rpc_endpoint`: Optional Solana RPC endpoint URL (defaults to mainnet-beta if not specified)

### get_program_subcalls
- **Description**: Analyze all subcalls from specified programs in a transaction. Processes both top-level instructions from message.compiledInstructions and nested calls from tx.meta?.innerInstructions.
- **Parameters**:
  - `signature`: Transaction signature (base58-encoded string, 88 characters)
  - `program_ids`: Array of program IDs to filter subcalls by
  - `include_nested`: Optional boolean to include nested subcalls from innerInstructions (default: true)
  - `rpc_endpoint`: Optional Solana RPC endpoint URL (defaults to mainnet-beta if not specified)

### get_account_data_with_parsing
- **Description**: Retrieve Solana account information and parse it based on the account owner. Automatically detects account type and provides program-specific analysis for different Solana programs.
- **Parameters**:
  - `account`: Account public key (base58-encoded string)
  - `rpc_endpoint`: Optional Solana RPC endpoint URL (defaults to mainnet-beta if not specified)

## Examples

See the original article for detailed examples: [Introduction to Solana Instruction Parsing MCP](https://dev.to/xiaodao/introduction-to-solana-instruction-parsing-mcp-1mk6)

## Contributing

Built by 晓道 (Xiaodao). Feedback and contributions welcome.

## License

Refer to the original project for licensing details.

## Tags

#solana #anchor #shank #codama #blockchain #mcp #ai #cryptocurrency
