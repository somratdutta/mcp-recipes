# MCP Recipes

A collection of experiments and implementations with Model Context Protocol (MCP) servers and everything in between.

## About

This repository serves as a playground for exploring the Model Context Protocol (MCP), which enables AI assistants to securely connect to external data sources and tools. Our experiments range from simple data processing servers to more complex integrations.

## What is MCP?

The Model Context Protocol is an open standard that allows AI assistants to work with external tools and data sources in a secure, standardized way. MCP servers can provide:
- Tools for data processing and analysis
- Access to databases and file systems
- Integration with external APIs
- Custom business logic and workflows

## Repository Structure

```
mcp-recipes/
├── hello-world/          # Basic MCP server example
│   ├── data/            # Sample data files (CSV, Parquet)
│   ├── tools/           # Tool implementations
│   │   ├── csv_tools.py    # CSV processing tools
│   │   └── parquet_tools.py # Parquet processing tools
│   ├── utils/           # Utility functions
│   ├── server.py        # Main MCP server implementation
│   ├── main.py          # Server runner
│   └── README.md        # Detailed documentation
└── README.md            # This file
```

## Getting Started

### Prerequisites

- Python 3.8+
- uv (Python package manager)

### Quick Start

1. Navigate to an example directory:
   ```bash
   cd hello-world
   ```

2. Install dependencies:
   ```bash
   uv sync
   ```

3. Run the MCP server:
   ```bash
   uv run python main.py
   ```

## Examples

### Hello World
The `hello-world` directory contains a comprehensive example MCP server that demonstrates:
- CSV and Parquet file processing
- Data analysis tools
- File system operations
- Structured data handling

This server provides tools for reading, analyzing, and manipulating tabular data in various formats.

## Contributing

Feel free to add your own MCP server experiments to this repository. Each experiment should:
- Be in its own directory
- Include a README with setup and usage instructions
- Follow the MCP specification
- Include example data or test cases where applicable

## Resources

- [MCP Specification](https://spec.modelcontextprotocol.io/)
- [MCP SDK Documentation](https://github.com/modelcontextprotocol/python-sdk)
- [Example MCP Servers](https://github.com/modelcontextprotocol/servers)

## License

This project is open source and available under the [MIT License](LICENSE). 