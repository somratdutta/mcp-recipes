# MCP Server Documentation

## How MCP Servers Work (Different from Web Servers)

Your MCP server uses **stdio** (standard input/output) communication, not HTTP ports. This is fundamentally different from traditional web servers.

### Traditional Web Server vs MCP Server

**Traditional Web Server:**
- Listens on a port (like :3000, :8080)
- Accepts HTTP requests from browsers/clients
- You can visit `http://localhost:3000` in a browser
- Runs continuously, waiting for requests
- Uses HTTP protocol (GET, POST, etc.)

**Your MCP Server:**
- Communicates via stdin/stdout (text streams)
- No HTTP, no ports, no web interface
- Only communicates when a client (like Claude) spawns the process
- Uses JSON-RPC protocol over stdio
- Process starts when needed, terminates when done

### How Claude Connects to Your Server

When you configured Claude for Desktop, you provided this config:

```json
{
  "mcpServers": {
    "mix_server": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/mix_server", 
        "run",
        "main.py"
      ]
    }
  }
}
```

This tells Claude to:
1. **Spawn a new process** by running `uv run main.py`
2. **Communicate via stdio** - send JSON messages back and forth
3. **Keep the process alive** during the conversation
4. **Terminate the process** when done

### What You'll See When Running

When you run `uv run main.py`, you'll see:

```
Starting MCP server 'mix_server' with transport 'stdio'
```

Then the server **waits quietly** for input. No port number, no "Server running at localhost:3000" message.

### Why Stdio Instead of HTTP?

1. **Security**: No network exposure, only local process communication
2. **Simplicity**: No need to manage ports, CORS, authentication
3. **Process Isolation**: Each Claude conversation gets its own server instance
4. **Standard**: MCP protocol is designed around JSON-RPC over stdio
5. **Resource Efficient**: Process starts only when needed

### Other MCP Transport Options

While your server uses stdio, MCP also supports:

- **SSE (Server-Sent Events)**: Would run on a port like `:3001`
- **WebSocket**: Would also use a port
- **Stdio**: What you're using (no port needed)

To use a different transport, you'd modify your server like:

```python
# For SSE transport (would use a port)
mcp = FastMCP("mix_server", transport="sse")

# Your current setup (stdio)
mcp = FastMCP("mix_server")  # defaults to stdio
```

### Communication Flow

1. **Claude starts your server process**
2. **Claude sends JSON-RPC messages** via stdin
3. **Your server processes requests** and calls your tools
4. **Your server responds** with JSON via stdout
5. **Claude receives responses** and continues conversation
6. **Process terminates** when conversation ends

### Testing Your Server

You can't test it in a browser because there's no HTTP interface. Instead:
1. **Use Claude for Desktop** (as intended)
2. **Use MCP CLI tools** for debugging
3. **Write your own MCP client** using the SDK

The beauty of stdio transport is that it's simple, secure, and works perfectly with Claude for Desktop's process management system!
