---
name: mcp-tool-integration
description: Implementing and connecting Model Context Protocol (MCP) tools. Use when you need to extend an agent's capabilities with external data sources or execution environments.
---

# MCP Tool Integration

## Overview
Model Context Protocol (MCP) is the standard for connecting AI agents to external tools and context. This skill guides the integration and development of new MCP tools.

## When to Use
- You need to access data outside the agent's current context
- You are building custom tools for Claude Desktop or other MCP-compatible clients
- Integrating REST APIs, databases, or local services via MCP

## Process
1. **Define the Protocol**: Choose standard stdio or SSE depending on the deployment environment.
2. **Design Tool Schemas**: Create clear, descriptive JSON schemas for tool inputs.
3. **Implement Handlers**: Write the execution logic.
4. **Test Locally**: Verify tool execution and error handling.
5. **Configure Client**: Add the MCP server to the agent's configuration file.

## Common Rationalizations

| Rationalization | Why It Is Wrong |
|---|---|
| "The handler works, so the tool is done." | Agents rely on schemas, names, and descriptions as much as handler code. |
| "Errors can just throw." | MCP clients need predictable errors so agents can recover or explain the failure. |
| "The client config is obvious." | A working server is not useful until the target client can discover and invoke it. |

## Red Flags

- Tool names or descriptions are vague
- Inputs accept broad strings instead of structured fields
- The handler returns raw internal errors or stack traces
- No local client invocation has been tested

## Verification

Before finishing, confirm:

- Tool schemas describe the inputs an agent should actually provide
- Handler success and error paths were exercised locally
- Client configuration is documented and tested
- Secrets, tokens, and local paths are not exposed in tool responses
