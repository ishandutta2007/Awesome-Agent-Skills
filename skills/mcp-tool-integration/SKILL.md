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
- "The tool works in my shell, so MCP will work too." MCP adds schema,
  transport, client configuration, and error-handling boundaries.
- "The schema can be loose." Loose schemas push ambiguity into the agent and
  create unreliable tool calls.
- "Secrets can go in examples." Configuration docs must show placeholders and
  keep credentials in the user's secret store.

## Red Flags
- Tool descriptions do not explain when the agent should call them.
- Handlers return raw stack traces or provider errors to the model.
- Client configuration examples include real tokens, host-specific paths, or
  hidden environment assumptions.
- The server has no local smoke test for at least one successful call and one
  validation failure.

## Verification
- Tool input schemas reject invalid requests with clear errors.
- The MCP server starts from a clean checkout using documented commands.
- At least one client can list and call the tools successfully.
- Logs and example configs contain no secrets or private user data.
