# vs-mcp

A Model Context Protocol (MCP) server that provides tools to locate and launch Visual Studio on Windows.

## Overview

This project implements an MCP server using the [MCP C# SDK](https://github.com/modelcontextprotocol/csharp-sdk). It exposes a tool that AI assistants can use to find and launch Visual Studio installations, optionally opening a specific solution or folder.

## Features

- **Visual Studio Launcher**: Automatically locates the latest Visual Studio installation using `vswhere.exe` and launches it
- **Solution/Folder Support**: Optionally open a specific `.sln`, `.slnx`, or folder path when launching
- **Self-contained**: Ships with a bundled `vswhere.exe` so it works without relying on a machine-wide installation
- **Cross-platform Build**: While Visual Studio only runs on Windows, the server can be built for multiple platforms

## Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download) or later
- Visual Studio installed on Windows (for the launcher tool to work)

## Building

```bash
# Clone the repository
git clone https://github.com/andysterland/vs-mcp.git
cd vs-mcp

# Build the project
dotnet build

# Run the MCP server
dotnet run --project src/visual-studio-mcp
```

## Publishing

To publish as a self-contained single-file executable:

```bash
dotnet publish src/visual-studio-mcp -c Release
```

## Available Tools

### `LaunchVisualStudio`

Locates the installed location of Visual Studio and launches it.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `pathToOpen` | string | No | Optional path to a solution (`.sln`/`.slnx`) or folder to open |

**Example Usage:**
- Launch Visual Studio: `LaunchVisualStudio()`
- Open a solution: `LaunchVisualStudio("C:\\Projects\\MySolution.sln")`
- Open a folder: `LaunchVisualStudio("C:\\Projects\\MyProject")`

## MCP Configuration

This server uses stdio transport for communication. To use it with an MCP-compatible client, configure it as follows:

```json
{
  "servers": {
    "visualstudio-launcher": {
      "command": "dotnet",
      "args": ["run", "--project", "path/to/vs-mcp/src/visual-studio-mcp"]
    }
  }
}
```

Or if using the published executable:

```json
{
  "servers": {
    "visualstudio-launcher": {
      "command": "path/to/VisualStudioMCP.exe"
    }
  }
}
```

## License

See [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.