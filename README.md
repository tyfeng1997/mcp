# MCP Weather Server

A TypeScript server implementation for the Model Context Protocol (MCP) using Server-Sent Events (SSE) transport to provide weather information tools.

## Overview

This server implements the Model Context Protocol and exposes weather forecast and alert tools through Server-Sent Events (SSE) transport. Unlike standard examples that typically use stdio for local communication, this server demonstrates how to implement remote connections via HTTP, allowing clients to connect over a network.

## Features

- Implements MCP over SSE transport
- Provides two weather tools:
  - `get-forecast`: Get weather forecasts for specific coordinates
  - `get-alerts`: Get active weather alerts for US states
- Connects to the US National Weather Service API
- Supports multiple client connections

## Prerequisites

- Node.js (v16 or newer)
- npm or yarn

## Installation

```bash
# Clone the repository
git clone https://github.com/tyfeng1997/mcp-server.git
cd mcp-server

# Install dependencies
npm install

# Build the TypeScript code
npm run build
```

## Usage

To start the server:

```bash
node build/index.js
```

This will start the server on port 3001 by default. You should see output similar to:

```
Weather MCP Server running on http://localhost:3001
Connect clients to http://localhost:3001/sse
```

### Available Tools

The server exposes two tools:

1. **get-forecast**

   - Description: Get weather forecast for a location
   - Parameters:
     - `latitude` (number): Latitude of the location (-90 to 90)
     - `longitude` (number): Longitude of the location (-180 to 180)
   - Note: Only US locations are supported by the NWS API

2. **get-alerts**
   - Description: Get weather alerts for a state
   - Parameters:
     - `state` (string): Two-letter US state code (e.g., CA, NY)

### Testing the Server

You can test if the server is running correctly by making a request to the SSE endpoint:

```bash
curl http://localhost:3001/sse
```

This should return an event with a session ID.

## Companion Client

This server is designed to work with the MCP client available at [tyfeng1997/mcp-client](https://github.com/tyfeng1997/mcp-client). The client connects to this server and allows AI assistants like Claude to use the weather tools through natural language.

## Customization

### Port Configuration

To change the port, modify the `PORT` constant in the code:

```typescript
const PORT = 3001; // Change to your desired port
```

### Adding More Tools

You can add more tools by following the pattern used for the existing tools:

```typescript
server.tool(
  "your-tool-name",
  "Your tool description",
  {
    // Parameters schema using zod
    param1: z.string().describe("Parameter description"),
    param2: z.number().describe("Parameter description"),
  },
  async ({ param1, param2 }) => {
    // Tool implementation
    return {
      content: [
        {
          type: "text",
          text: "Tool result",
        },
      ],
    };
  }
);
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
