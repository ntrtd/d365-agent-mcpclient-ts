# Implementation Plan: d365-agent-mcpclient-ts

This document outlines the phased implementation plan for the `d365-agent-mcpclient-ts` repository. This library provides a TypeScript client for interacting with Model Context Protocol (MCP) servers, primarily the `d365-agent-mcpserver-dotnet`.

## Overall Goal
To develop a robust, well-tested, and easy-to-use TypeScript MCP client library that enables the `d365-agent-orchestrator` (LangGraph agents and CopilotKit actions) to seamlessly communicate with MCP servers, especially the `d365-agent-mcpserver-dotnet` for Dynamics 365 operations.

---

## Phase 1: Core MCP Client Functionality

*   **Objectives:**
    *   Establish the TypeScript library project structure.
    *   Implement core MCP request types (`initialize`, `discoverTools`, `executeTool`).
    *   Handle basic MCP responses and errors.
    *   Publish an initial version of the npm package.
*   **Tasks:**
    1.  **Project Setup:**
        *   Initialize a TypeScript library project (e.g., using `tsup`, `tsc`, or a similar build system).
        *   Set up linting (ESLint), formatting (Prettier), testing framework (e.g., Jest, Vitest).
        *   Configure `package.json` for publishing to npm (e.g., as `@d365-agent/mcpclient-ts`).
    2.  **Define Core MCP Types:**
        *   Define TypeScript interfaces/types for `McpRequest`, `McpResponse`, `McpContentItem`, `McpError`, `McpToolParameter`, `McpToolSchema`, `McpToolDescription`, `McpServerDescription`. These should align with the MCP specification and potentially reuse types from `submodules/modelcontextprotocol/src/protocol.ts` or a core MCP types package if available.
    3.  **Implement `McpClient` Class:**
        *   `constructor(options: { baseUrl: string; defaultHeaders?: Record<string, string>; timeout?: number; })`: Initializes with MCP server base URL and optional configurations.
        *   `private async _sendRequest(payload: McpRequest): Promise<McpResponse>`: Internal method for sending JSON-RPC requests and receiving responses.
        *   `async initialize(clientInfo: any): Promise<McpResponse>`: Implements the `initialize` handshake.
        *   `async discoverTools(): Promise<McpServerDescription>`: Implements the `discoverTools` request and parses the server description.
        *   `async executeTool(toolName: string, parameters: Record<string, any>): Promise<McpResponse>`: Implements the `executeTool` request.
    4.  **HTTP Transport:**
        *   Use a reliable HTTP client library (e.g., `axios`, `node-fetch`, or native `fetch` if environment supports it) for making requests.
    5.  **Error Handling:**
        *   Implement basic error handling for network issues (e.g., server unreachable, timeouts).
        *   Parse and represent MCP error responses from the server.
    6.  **Basic Request/Response Validation:**
        *   (Optional, depending on strictness) Basic validation of request parameters against tool schemas obtained from `discoverTools`.
    7.  **Unit Tests:**
        *   Write unit tests for `McpClient` methods, mocking HTTP transport and server responses. Test success and error cases.
    8.  **CI/CD Pipeline:**
        *   Set up CI to run linters, tests, and build the library.
        *   Set up CD to publish the package to npm on new versions/tags.
*   **Deliverables:**
    *   Functional `McpClient` capable of basic MCP interactions.
    *   Published npm package.

---

## Phase 2: Enhancements for LangGraph & Orchestrator Integration

*   **Objectives:**
    *   Improve ease of use within the `d365-agent-orchestrator` (LangGraph agents and CopilotKit actions).
    *   Add support for more advanced MCP features if needed.
*   **Tasks:**
    1.  **LangGraph Tool Wrappers/Helpers:**
        *   Investigate and potentially develop helper functions or classes that make it easier to use discovered MCP tools as LangChain/LangGraph compatible tools. This might involve dynamically creating functions or classes based on the schemas returned by `discoverTools`.
    2.  **CopilotKit Action Integration:**
        *   Ensure the `McpClient` can be easily used within CopilotKit action handlers in `d365-agent-orchestrator` for simpler, direct D365 MCP tool calls.
    3.  **Streaming Support (If Applicable):**
        *   If the MCP server (`d365-agent-mcpserver-dotnet`) supports streaming responses for certain tools (e.g., for long-running operations or large data sets), implement client-side handling for these streams.
    4.  **Advanced Error Handling:**
        *   Introduce more specific typed errors for different MCP error conditions or D365-specific errors relayed by the MCP server.
    5.  **Typed Tool Execution (Exploration):**
        *   Explore mechanisms for providing better type safety when calling `executeTool`. This could involve generics or code generation techniques based on `discoverTools` output, though this can be complex.
*   **Testing:**
    *   Unit tests for any new helper functions or wrappers.
    *   Integration tests demonstrating usage within a sample LangGraph agent.

---

## Phase 3 & 4: Long-term Maintenance & Advanced Features

*   **Objectives:**
    *   Ensure ongoing compatibility and robustness.
    *   Add advanced features as required by the evolving D365 AI Agent system.
*   **Tasks:**
    1.  **MCP Protocol Evolution:**
        *   Adapt the client to support new versions or features of the Model Context Protocol as it evolves.
    2.  **Performance Optimizations:**
        *   Profile and optimize client performance if it becomes a bottleneck (e.g., connection management, request/response parsing).
    3.  **Enhanced Tool Generation (Advanced):**
        *   Consider developing a CLI tool or utility within this package that can connect to an MCP server, run `discoverTools`, and generate fully typed TypeScript client-side functions for each discovered tool, simplifying development in `d365-agent-orchestrator`.
    4.  **Security Enhancements:**
        *   Review and enhance security aspects, such as handling of authentication tokens or sensitive data in requests/responses.
    5.  **Documentation & Examples:**
        *   Maintain comprehensive API documentation and usage examples.
    6.  **Bug Fixes & Dependency Updates:**
        *   Address any reported bugs and keep dependencies up-to-date.
*   **Testing:**
    *   Ongoing regression testing.
    *   Tests for any new features.

This plan focuses on delivering a reliable and developer-friendly TypeScript MCP client that serves as the communication backbone between the `d365-agent-orchestrator` and MCP-compliant tool servers.
