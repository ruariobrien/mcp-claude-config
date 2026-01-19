# MCP Server Configuration for Claude Desktop

Production-ready MCP (Model Context Protocol) server configurations for Claude Desktop on Windows with WSL2.

## Overview

This repo contains my working MCP server setup that connects Claude Desktop to:

- **Voice Mode** — Local STT/TTS via Whisper.cpp and Kokoro
- **Home Assistant** — Smart home control and automation
- **Obsidian** — Notes and knowledge management
- **Desktop Commander** — Local machine control
- **Ollama** — Local LLM inference
- **Playwright** — Browser automation

## Quick Start

1. Copy `claude_desktop_config.json` to `%APPDATA%\Claude\`
2. Update paths and tokens for your environment
3. Restart Claude Desktop

## Configuration

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "mcp-obsidian", "C:\\Users\\YOUR_USER\\Documents\\ObsidianVault"]
    },
    "voice-mode": {
      "command": "wsl",
      "args": ["bash", "-lc", "export PATH=$HOME/.local/bin:$PATH && STT_BASE_URL=http://127.0.0.1:2022/v1 TTS_BASE_URL=http://127.0.0.1:8880/v1 TTS_VOICE=af_bella uvx voice-mode"]
    },
    "desktop-commander": {
      "command": "npx",
      "args": ["-y", "desktop-commander"]
    },
    "hass-mcp": {
      "command": "wsl",
      "args": ["bash", "-lc", "export PATH=$HOME/.local/bin:$PATH && HA_URL=http://YOUR_HA_IP:8123 HA_TOKEN=YOUR_LONG_LIVED_TOKEN uvx hass-mcp"]
    },
    "ollama": {
      "command": "npx",
      "args": ["-y", "ollama-mcp"],
      "env": {
        "OLLAMA_HOST": "http://127.0.0.1:11434"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    }
  }
}
```

## Prerequisites

### Windows
- Claude Desktop
- Node.js (for npx)
- WSL2 with Ubuntu

### WSL2
```bash
# Install uv package manager
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add to PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verify
uvx --version
```

## MCP Servers

### Voice Mode
Local speech-to-text and text-to-speech using Whisper.cpp and Kokoro TTS.

**Requirements:**
- Whisper.cpp server running on port 2022
- Kokoro TTS Docker container on port 8880
- See [local-ai-voice](https://github.com/ruariobrien/local-ai-voice) for setup

### Home Assistant
Control smart home devices and automations via MCP.

**Requirements:**
- Home Assistant instance
- Long-lived access token (Settings → Users → Create Token)

### Obsidian
Read/write notes in your Obsidian vault.

**Requirements:**
- Obsidian vault directory path

## Troubleshooting

### All MCPs showing "failed"
1. Check JSON syntax (use a validator)
2. Fully quit Claude Desktop (system tray → Quit)
3. Restart Claude Desktop

### WSL-based MCPs not connecting
```bash
# Test manually in WSL
STT_BASE_URL=http://127.0.0.1:2022/v1 TTS_BASE_URL=http://127.0.0.1:8880/v1 uvx voice-mode
```

### uvx not found
```bash
# Ensure PATH is set
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## Hardware

Tested on:
- Intel i9-10900K
- 64GB DDR4-3200
- NVIDIA RTX 3090 24GB
- Windows 11 + WSL2 Ubuntu

## License

MIT
