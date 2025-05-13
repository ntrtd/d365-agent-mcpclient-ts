# Dynamics 365 AI Agent - MCP Client TypeScript (d365-agent-mcpclient-ts)

This repository contains the source code for the TypeScript client library for the Model Context Protocol (MCP). This client is a crucial component enabling TypeScript/Node.js applications, particularly the `d365-agent-orchestrator`, to communicate with MCP-compliant servers.

## Overview

The `d365-agent-mcpclient-ts` provides the necessary functionalities for a TypeScript application to:
*   Discover tools available on an MCP server.
*   Execute those tools with specified parameters.
*   Handle responses and errors according to the MCP specification.

In the D365 AI Agent architecture, this client is primarily used by the LangGraph agents and simpler CopilotKit actions within the `d365-agent-orchestrator` to interact with the `d365-agent-mcpserver-dotnet` (which exposes Dynamics 365 operations as MCP tools).

## Key Features & Responsibilities

*   **MCP Compliance:** Implements the client-side requirements of the Model Context Protocol.
*   **Tool Discovery:** Provides a method to fetch and parse the `McpServerDescription` from an MCP server, detailing available tools and their schemas.
*   **Tool Execution:** Enables calling specific tools on an MCP server by name, passing structured parameters, and receiving structured responses or errors.
*   **Typed Interactions (Goal):** Aims to provide as much type safety as possible for tool parameters and responses, potentially through generics or code generation based on discovered schemas.
*   **Error Handling:** Manages network errors and interprets MCP error responses from the server.
*   **Extensibility:** Designed to connect to any MCP-compliant server, though its primary target in this project is `d365-agent-mcpserver-dotnet`.

## Technology Stack

*   **TypeScript**
*   HTTP client library (e.g., `axios`, `node-fetch`)
*   Potentially schema validation libraries (e.g., Zod) for request/response validation.
*   Built to be consumed as an npm package (e.g., `@d365-agent/mcpclient-ts`).
*   May leverage core MCP types and interfaces from `submodules/typescript-sdk/packages/mcp-core` or a similar shared MCP type definition library.

## Core API (Conceptual)

```typescript
// Interface for MCP Client options
interface McpClientOptions {
  baseUrl: string; // URL of the MCP Server
  // Other options like custom headers, timeout, etc.
}

// Interface for MCP Response (simplified)
interface McpResponse {
  isError: boolean;
  content: Array<{ type: string; text?: string; data?: any; [key: string]: any; }>;
  // ... other MCP response fields
}

// McpClient class
class McpClient {
  constructor(options: McpClientOptions);
  async discoverTools(): Promise<McpServerDescription>; // McpServerDescription would detail available tools and their schemas
  async executeTool(toolName: string, parameters: Record<string, any>): Promise<McpResponse>;
}
```

## Getting Started

(Details to be added - typically involves installing the npm package and instantiating the `McpClient` with the target server URL).
Refer to the `implementation.md` in this repository for a detailed development plan.

## Contribution

(Details on contribution guidelines if applicable)
