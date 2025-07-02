# MCP Hello World Server

A beginner-friendly **Model Context Protocol (MCP) server** built with Python that demonstrates how to create custom tools for AI assistants like Claude for Desktop.

> 📖 **Based on**: [Building a Basic MCP Server with Python](https://medium.com/data-engineering-with-dremio/building-a-basic-mcp-server-with-python-4c34c41031ed) by Alex Merced

## What is MCP?

**Model Context Protocol (MCP)** is a way to let AI models like Claude securely interact with your local data and custom tools. Think of it as building your own mini API — but instead of exposing it to the internet, you're exposing it to an AI assistant on your machine.

With MCP, you can:
- Let Claude read files or query databases
- Create tools that do useful work (summarize datasets, fetch APIs)
- Add reusable prompts to guide AI behavior

## What We Built

This project creates a local MCP server with:

- ✅ **Two custom tools** for data analysis:
  - `summarize_csv_file` - Analyzes CSV files
  - `summarize_parquet_file` - Analyzes Parquet files
- ✅ **Clean, modular architecture** for easy extension  
- ✅ **Sample data files** in both CSV and Parquet formats
- ✅ **Seamless integration** with Claude for Desktop

## Project Structure

```
mix_server/
├── data/                 # Sample data files
│   ├── sample.csv
│   ├── sample.parquet  
│   ├── employees.csv
│   ├── employees.parquet
│   ├── products.csv
│   └── products.parquet
├── tools/                # MCP tool definitions
│   ├── csv_tools.py
│   └── parquet_tools.py
├── utils/                # Reusable utilities
│   └── file_reader.py
├── server.py             # MCP server instance
├── main.py              # Entry point
├── pyproject.toml       # Dependencies
└── documentation.md     # Technical details
```

## Prerequisites

- Python 3.10+
- [uv](https://docs.astral.sh/uv/) (fast Python project manager)
- [Claude for Desktop](https://www.anthropic.com/claude)

## Quick Start

### 1. Install uv (if not already installed)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. Set up the project

```bash
# Dependencies are already configured in pyproject.toml
cd mix_server
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
uv sync
```

### 3. Test the server (optional)

```bash
uv run main.py
```

You should see: `Starting MCP server 'mix_server' with transport 'stdio'`

Press Ctrl+C to stop.

### 4. Configure Claude for Desktop

Edit Claude's config file:

**macOS/Linux:**
```bash
code ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

**Windows:**
```bash
notepad %APPDATA%\Claude\claude_desktop_config.json
```

Add this configuration (replace `/ABSOLUTE/PATH/TO/mix_server` with your actual path):

```json
{
  "mcpServers": {
    "mix_server": {
      "command": "uv",
      "args": [
        "--directory",
        "/ABSOLUTE/PATH/TO/mix_server",
        "run",
        "main.py"
      ]
    }
  }
}
```

### 5. Restart Claude for Desktop

After restarting, you should see a hammer icon (🔨) indicating tools are available.

## Usage

Ask Claude natural language questions like:

- *"Summarize the CSV file named sample.csv"*
- *"How many rows are in employees.parquet?"*
- *"What's in the products.csv file?"*

Claude will automatically:
1. Detect which tool to use
2. Spawn your MCP server process
3. Call the appropriate function
4. Return results in natural language
5. Clean up the process when done

## How It Works

### Key Architecture Points

- **No ports**: Uses stdio communication, not HTTP
- **On-demand**: Claude spawns/terminates the server as needed  
- **Secure**: Only local process communication, no network exposure
- **Modular**: Easy to add new tools by creating files in `tools/`

### The Magic: `@mcp.tool()` Decorator

```python
@mcp.tool()
def summarize_csv_file(filename: str) -> str:
    """
    Summarize a CSV file by reporting its number of rows and columns.
    Args:
        filename: Name of the CSV file in the /data directory
    Returns:
        A string describing the file's dimensions.
    """
    return read_csv_summary(filename)
```

This decorator automatically:
- Registers the function as an MCP tool
- Makes it callable by AI models
- Uses the docstring for tool description
- Handles parameter validation

## Sample Data

The project includes sample datasets:

- **sample.csv** - User signup data (5 rows)
- **employees.csv** - Employee data (8 rows)  
- **products.csv** - Product inventory (10 rows)

Each CSV has a corresponding Parquet version for testing both tools.

## Extending the Server

### Add a New Tool

1. Create a new function in `tools/` (or existing file):

```python
@mcp.tool()
def count_csv_rows(filename: str) -> str:
    """Count total rows in a CSV file."""
    df = pd.read_csv(f"data/{filename}")
    return f"File {filename} has {len(df)} rows"
```

2. Import it in `main.py`:

```python
import tools.your_new_tool
```

3. Restart Claude - your tool is now available!

### Ideas for More Tools

- Filter data based on column values
- Calculate statistics (mean, median, etc.)
- Export data to different formats
- Connect to databases
- Fetch data from APIs

## Troubleshooting

**Tools not appearing in Claude?**
- Check that the config file path is correct
- Verify `uv` is in your system PATH
- Ensure all file paths are absolute paths
- Restart Claude for Desktop

**Server won't start?**
- Run `uv run main.py` manually to check for errors
- Verify dependencies with `uv sync`
- Check that data files exist in `data/` directory

**Import errors?**
- Make sure you're in the `mix_server` directory
- Activate the virtual environment: `source .venv/bin/activate`

## Next Steps

This is just the beginning! Consider:

- **Resources**: Use `@mcp.resource()` to expose static/dynamic data
- **Prompts**: Create templates with `@mcp.prompt()` 
- **Async tools**: Handle databases/APIs with `async def`
- **Custom clients**: Build your own MCP client with the SDK

## Resources

- 📖 [Original Tutorial](https://medium.com/data-engineering-with-dremio/building-a-basic-mcp-server-with-python-4c34c41031ed)
- 🔧 [MCP Documentation](https://modelcontextprotocol.io/)
- 🐍 [FastMCP Library](https://github.com/pydantic/fastmcp)
- 🤖 [Claude for Desktop](https://www.anthropic.com/claude)

## License

MIT License - feel free to use this as a template for your own MCP servers!

---

**Happy coding!** 🚀 You now have a solid foundation for building AI-powered tools that go beyond text generation.