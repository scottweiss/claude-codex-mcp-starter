# MCP (Model Context Protocol) Setup Guide

Quick setup guide for enabling Claude Code to communicate with Codex and other AI models via MCP servers.

## 🚀 Quick Start (Recommended)

### One-Line Setup

If you have Claude Desktop with MCP support and the Codex CLI installed:

```bash
# Add Codex MCP server (replace with your actual API key)
claude mcp add codex -s user -e OPENAI_API_KEY=$OPENAI_API_KEY -- codex mcp -c model=gpt-5 -c model_reasoning_effort="high"

# Restart Claude Desktop
# You're done! 🎉
```

That's it! Claude can now delegate tasks to Codex.

### Verify Setup

1. Restart Claude Desktop
2. Check MCP servers: `claude mcp list`
3. Test in Claude: "Can you test the connection to Codex?"

## 📋 Prerequisites

- Claude Desktop (with MCP support)
- Codex CLI installed:
  - `npm install -g @openai/codex` (npm), or
  - `brew install --cask codex` (Homebrew, macOS)
- OpenAI API key **or** a ChatGPT Plus/Pro/Team/Edu/Enterprise account

## 🔧 Alternative Setup Methods

### Method 0: ChatGPT Plan Sign-In (No API Key Required)

If you have a ChatGPT Plus, Pro, Team, Edu, or Enterprise plan, you can authenticate without an API key:

```bash
# Run Codex and select "Sign in with ChatGPT"
codex
# Then add the MCP server (no OPENAI_API_KEY env needed)
claude mcp add codex -s user -- codex mcp -c model=gpt-5 -c model_reasoning_effort="high"
```

### Method 1: Using Node-based Codex MCP Server

If you prefer the Node.js server implementation:

```bash
# Clone and build the server
git clone https://github.com/modelcontextprotocol/codex-mcp-server.git
cd codex-mcp-server
npm install
npm run build

# Add to Claude
claude mcp add codex -s user \
  -e OPENAI_API_KEY=$OPENAI_API_KEY \
  -e OPENAI_MODEL=gpt-5 \
  -- node /absolute/path/to/codex-mcp-server/dist/index.js
```

### Method 2: Manual Configuration

For advanced control, edit the config file directly:

**Location:**
- macOS/Linux: `~/.config/claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

**Basic Configuration:**
```json
{
  "mcpServers": {
    "codex": {
      "command": "codex",
      "args": ["mcp", "-c", "model=gpt-5", "-c", "model_reasoning_effort=high"],
      "env": {
        "OPENAI_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

**Advanced Configuration with Options:**
```json
{
  "mcpServers": {
    "codex": {
      "command": "codex",
      "args": ["mcp", "-c", "model=gpt-5", "-c", "model_reasoning_effort=high"],
      "env": {
        "OPENAI_API_KEY": "your-api-key-here",
        "CODEX_SANDBOX": "workspace-write",
        "CODEX_APPROVAL_POLICY": "on-request",
        "CODEX_WORKING_DIR": "/path/to/project"
      }
    }
  }
}
```

## 🛡️ Security Options

### Sandbox Modes
- `read-only`: Can only read files
- `workspace-write`: Can modify files in workspace (recommended)
- `danger-full-access`: Full system access (use with caution)

### Approval Policies
- `never`: No approval needed
- `on-request`: Approve specific operations
- `on-failure`: Approve when commands fail
- `untrusted`: Always require approval

## 🧪 Adding Other MCP Servers

### Playwright (Browser Automation)
```bash
claude mcp add playwright -s user -- npx -y @modelcontextprotocol/server-playwright
```

### GitHub
```bash
claude mcp add github -s user -e GITHUB_TOKEN=$GITHUB_TOKEN -- npx -y @modelcontextprotocol/server-github
```

### File System
```bash
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem /path/to/project
```

## 🔍 Troubleshooting

### MCP Server Not Found
```bash
# List all MCP servers
claude mcp list

# Remove and re-add
claude mcp remove codex
claude mcp add codex -s user -e OPENAI_API_KEY=$OPENAI_API_KEY -- codex mcp -c model=gpt-5 -c model_reasoning_effort="high"
```

### Permission Issues
- Check API key is valid
- Verify sandbox mode settings
- Ensure Claude Desktop has necessary permissions

### Windows-Specific
On Windows (not WSL), wrap npx commands:
```bash
claude mcp add my-server -- cmd /c npx -y @some/package
```

## 📝 Environment Variables

Create a `.env` file for your project:

```bash
# Required (if not using ChatGPT plan sign-in)
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-5  # Best with high reasoning effort
MODEL_REASONING_EFFORT=high  # For maximum quality

# Optional
CODEX_SANDBOX=workspace-write
CODEX_APPROVAL_POLICY=on-request
CODEX_WORKING_DIR=/path/to/project
```

## ✅ Testing Your Setup

### Verify Connection

In Claude, ask: "Can you list the repository root files?"

Expected response from Codex:
- Brief preamble: "Listing repository root files..."
- Command: `rg --files | head -n 5`
- Output: 5 file names (no edits performed)

### Common Test Issues

- **"command not found"**: Ensure `codex` is on PATH
- **"Permission denied"**: Sandbox is in `read-only`, switch to `workspace-write`
- **"`rg` not found"**: Install ripgrep (`brew install ripgrep` or `apt install ripgrep`)
- **"Patch failed"**: Concurrent edits, pull latest changes first

### Full Integration Test

1. **Basic Test**: "Can you reach Codex?"
2. **Code Generation**: "Use Codex to write a Python hello world"
3. **Collaboration**: "Work with Codex to implement a simple function"

## 📚 Resources

- [Claude MCP Documentation](https://docs.anthropic.com/claude/docs/mcp)
- [Codex CLI Documentation](https://developers.openai.com/codex)
- [MCP Protocol Spec](https://modelcontextprotocol.io)

---

*Note: GPT-5 with high reasoning effort provides best results. Fallback options: gpt-4o, gpt-4.1*