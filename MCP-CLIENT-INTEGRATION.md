# 🔌 MCP Client Integration Guide

**Complete setup guide for using the Kafka Schema Registry MCP Server with any MCP-compatible IDE or AI assistant**

The Model Context Protocol (MCP) is an open standard that enables AI models to interact with external tools and services through a unified interface. This guide provides detailed setup instructions for integrating our Kafka Schema Registry MCP Server with popular development environments and AI assistants.

---

## 🌟 What is MCP?

The **Model Context Protocol (MCP)** is like a "universal adapter" for AI systems, similar to how USB standardized device connections. It allows AI assistants to:

- **Connect to external tools** and data sources consistently
- **Perform actions** through standardized interfaces  
- **Access real-time data** without custom integrations
- **Work across platforms** with the same server implementation

### Key Benefits
- **🔌 Plug & Play**: One server works with multiple AI clients
- **🔒 Secure**: Standardized authentication and permissions
- **📈 Scalable**: Add new capabilities without rebuilding integrations
- **🌐 Universal**: Works with any MCP-compliant client

---

## 🎨 Supported Clients & IDEs

### **✅ Fully Supported**

<table>
<tr>
<td width="25%" align="center">
<h4>🤖 Claude Desktop</h4>
<p><strong>Status:</strong> ✅ Production Ready</p>
<ul>
<li>Native MCP integration</li>
<li>Conversational interface</li>
<li>Multi-registry support</li>
<li>OAuth integration</li>
</ul>
<p><a href="#claude-desktop">Setup Guide →</a></p>
</td>
<td width="25%" align="center">
<h4>💻 VS Code + Copilot</h4>
<p><strong>Status:</strong> ✅ Preview (Agent Mode)</p>
<ul>
<li>GitHub Copilot integration</li>
<li>Workspace configuration</li>
<li>Tool discovery</li>
<li>Auto-discovery from Claude</li>
</ul>
<p><a href="#vs-code-copilot">Setup Guide →</a></p>
</td>
<td width="25%" align="center">
<h4>⚡ Cursor IDE</h4>
<p><strong>Status:</strong> ✅ Production Ready</p>
<ul>
<li>One-click installation</li>
<li>Agent & Composer modes</li>
<li>Project/global config</li>
<li>OAuth server support</li>
</ul>
<p><a href="#cursor-ide">Setup Guide →</a></p>
</td>
<td width="25%" align="center">
<h4>🧠 JetBrains IDEs</h4>
<p><strong>Status:</strong> ✅ Production Ready (2025.1+)</p>
<ul>
<li>IntelliJ, PyCharm, WebStorm</li>
<li>AI Assistant integration</li>
<li>Codebase-mode support</li>
<li>MCP proxy support</li>
</ul>
<p><a href="#jetbrains-ides">Setup Guide →</a></p>
</td>
</tr>
</table>

### **🌐 Additional Supported Clients**

| Client | Status | Notes |
|--------|--------|--------|
| **Microsoft Copilot Studio** | ✅ GA | Enterprise MCP integration with connectors |
| **Eclipse IDE** | ✅ Preview | GitHub Copilot with MCP support |
| **Xcode** | ✅ Preview | GitHub Copilot agent mode |
| **Emacs** | ✅ Community | MCP client with gptel/llm plugins |
| **Warp Terminal** | ✅ Preview | AI-powered terminal with MCP |
| **Windsurf IDE** | ✅ Community | AI-first IDE with MCP support |
| **Zed Editor** | ✅ Community | Collaborative editor with MCP |
| **Replit** | ✅ Preview | Online IDE with MCP integration |

---

## 🤖 Claude Desktop

**Status:** ✅ Production Ready | **Platform:** macOS, Windows, Linux

Claude Desktop offers the most mature MCP integration with native support for our Kafka Schema Registry server.

### Prerequisites
- Claude Desktop installed ([download here](https://claude.ai/chat))
- Docker (for running the MCP server)

### Configuration

**macOS Configuration Path:**
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows Configuration Path:**
```
%APPDATA%/Claude/claude_desktop_config.json
```

**Linux Configuration Path:**
```
~/.config/claude-desktop/config.json
```

### Basic Configuration
```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "host",
        "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### Multi-Registry Configuration
```json
{
  "mcpServers": {
    "kafka-schema-registry-multi": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "demo-deployment_default",
        "-e", "SCHEMA_REGISTRY_NAME_1=development",
        "-e", "SCHEMA_REGISTRY_URL_1=http://dev-registry:8081",
        "-e", "SCHEMA_REGISTRY_NAME_2=staging",
        "-e", "SCHEMA_REGISTRY_URL_2=http://staging-registry:8082",
        "-e", "SCHEMA_REGISTRY_NAME_3=production",
        "-e", "SCHEMA_REGISTRY_URL_3=http://prod-registry:8083",
        "-e", "READONLY_3=true",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### GitHub OAuth Configuration
```json
{
  "mcpServers": {
    "kafka-schema-registry-oauth": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "demo-deployment_default",
        "-e", "ENABLE_AUTH=true",
        "-e", "AUTH_PROVIDER=github",
        "-e", "GITHUB_CLIENT_ID",
        "-e", "GITHUB_CLIENT_SECRET",
        "-e", "SCHEMA_REGISTRY_NAME_1=development",
        "-e", "SCHEMA_REGISTRY_URL_1=http://dev-registry:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ],
      "env": {
        "GITHUB_CLIENT_ID": "your_github_client_id",
        "GITHUB_CLIENT_SECRET": "your_github_client_secret"
      }
    }
  }
}
```

### Verification
1. Restart Claude Desktop
2. Look for the 🔨 **tools** icon in the chat interface
3. Test: `"What MCP tools are available?"`
4. Test: `"List all schema registries"`

---

## 💻 VS Code + Copilot

**Status:** ✅ Preview (Agent Mode) | **Platform:** Cross-platform

VS Code integrates MCP through GitHub Copilot's agent mode, providing AI-powered schema management directly in your editor.

### Prerequisites
- VS Code 1.99+ ([download here](https://code.visualstudio.com/))
- GitHub Copilot extension installed and enabled
- GitHub Copilot subscription
- Docker (for running the MCP server)

### Workspace Configuration

Create `.vscode/mcp.json` in your workspace root:
```json
{
  "servers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "host",
        "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### User Configuration

Add to VS Code User Settings (`Ctrl+Shift+P` → "Preferences: Open User Settings (JSON)"):
```json
{
  "mcp": {
    "servers": {
      "kafka-schema-registry": {
        "command": "docker",
        "args": [
          "run", "--rm", "-i",
          "--network", "host",
          "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
          "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
        ]
      }
    }
  }
}
```

### Auto-Discovery from Claude Desktop

If you already have Claude Desktop configured:
```json
{
  "mcp": {
    "autodiscovery": {
      "claudeDesktop": true
    }
  }
}
```

### Activation & Usage
1. Open GitHub Copilot Chat
2. Select **Agent** from the popup menu
3. Click the 🔧 **tools** icon to see available MCP servers
4. Use natural language: `"@agent list all kafka schemas"`

### Advanced Features
- **Context Integration**: Use `#` to add specific tools to prompts
- **Workspace Awareness**: MCP servers have access to workspace folders
- **Settings Management**: Toggle tools on/off as needed

---

## ⚡ Cursor IDE

**Status:** ✅ Production Ready | **Platform:** Cross-platform

Cursor offers excellent MCP support with both one-click installation and manual configuration options.

### Prerequisites
- Cursor IDE installed ([download here](https://cursor.com/))
- Docker (for running the MCP server)

### Option 1: One-Click Installation (Recommended)

1. In Cursor, go to **Settings** → **MCP**
2. Browse the curated collection of MCP servers
3. Find "Kafka Schema Registry MCP Server"
4. Click **"Add to Cursor"** for instant setup with OAuth support

### Option 2: Manual Configuration

#### Project-Specific Configuration
Create `.cursor/mcp.json` in your project root:
```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "host",
        "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

#### Global Configuration
Add to Cursor's global settings:
```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "host",
        "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ],
      "env": {
        "API_KEY": "your_api_key_if_needed"
      }
    }
  }
}
```

### Transport Types Supported
- **stdio**: Standard input/output (recommended for local development)
- **SSE**: Server-Sent Events (for distributed teams)
- **HTTP**: Streamable HTTP (for cloud deployments)

### Usage Modes
- **Agent Mode**: Autonomous AI operations with tool access
- **Composer Mode**: Interactive code generation with context
- **Chat Mode**: Conversational interface with MCP tools

### Verification
1. Check that the server indicator turns **green**
2. Open Agent or Composer mode
3. Ask: `"What Kafka schema tools are available?"`
4. Test: `"Show me all schemas in the development registry"`

---

## 🧠 JetBrains IDEs

**Status:** ✅ Production Ready (2025.1+) | **Platform:** Cross-platform

JetBrains IDEs support MCP through the AI Assistant with codebase-aware schema operations.

### Prerequisites
- IntelliJ IDEA 2025.1+ (or PyCharm, WebStorm, etc.)
- AI Assistant plugin enabled
- Docker (for running the MCP server)

### Supported IDEs
- **IntelliJ IDEA** 2025.1+
- **PyCharm** 2025.1+
- **WebStorm** 2025.1+
- **PhpStorm** 2025.1+
- **CLion** 2025.1+
- **GoLand** 2025.1+
- **RubyMine** 2025.1+
- **DataGrip** 2025.1+
- **Android Studio** (with AI Assistant)

### Configuration

1. Go to **Settings** → **Tools** → **AI Assistant** → **Model Context Protocol (MCP)**
2. Click **"+"** to add a new server
3. Enter the configuration:

```json
{
  "servers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "host",
        "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### JetBrains MCP Proxy Configuration

For advanced integration with the JetBrains ecosystem:
```json
{
  "servers": {
    "jetbrains-proxy": {
      "command": "npx",
      "args": ["-y", "@jetbrains/mcp-proxy"]
    },
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "host",
        "-e", "SCHEMA_REGISTRY_URL=http://localhost:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### Usage
1. Enable **"Codebase"** mode in AI Assistant chat
2. Type `/` to see available MCP tools
3. Ask context-aware questions: `"How do the schemas in this project relate to our Kafka topics?"`
4. Request actions: `"Register a schema for the User class in this project"`

### Special Features
- **IDE Integration**: Direct access to IDE's built-in webserver
- **Project Context**: MCP servers can access project structure
- **Code Awareness**: AI understands your codebase when working with schemas
- **Multi-IDE Support**: Same configuration works across JetBrains IDEs

### Verification
1. Check MCP server status in settings
2. Look for green indicators next to configured servers
3. Test in AI Assistant: `"What schema management tools are available?"`
4. Check logs: **Help** → **Show Log in Explorer/Finder/Nautilus** → **mcp** directory

---

## 🌐 Other Supported Clients

### Microsoft Copilot Studio
**Enterprise MCP integration for building custom AI agents**

```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "type": "mcp",
      "url": "http://your-mcp-server:38000",
      "authentication": {
        "type": "github",
        "clientId": "your_client_id",
        "clientSecret": "your_client_secret"
      }
    }
  }
}
```

### Eclipse IDE
**GitHub Copilot integration with MCP support**

Similar configuration to VS Code, using Eclipse's GitHub Copilot extension.

### Emacs
**MCP client with gptel/llm integration**

```elisp
(use-package mcp
  :config
  (setq mcp-servers
    '(("kafka-schema-registry"
       :command "docker"
       :args ("run" "--rm" "-i"
              "-e" "SCHEMA_REGISTRY_URL=http://localhost:8081"
              "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1")))))
```

### Warp Terminal
**AI-powered terminal with MCP support**

Configure through Warp's MCP settings to add schema management capabilities to your terminal AI.

---

## 🛠️ Configuration Best Practices

### Environment Variables
Always use environment variables for sensitive information:
```json
{
  "env": {
    "SCHEMA_REGISTRY_URL": "http://localhost:8081",
    "SCHEMA_REGISTRY_USER": "your_username",
    "SCHEMA_REGISTRY_PASSWORD": "your_password",
    "GITHUB_CLIENT_ID": "your_github_client_id",
    "GITHUB_CLIENT_SECRET": "your_github_client_secret"
  }
}
```

### Network Configuration
- **Local Development**: Use `--network host` for Docker
- **Multi-Service**: Use custom Docker networks
- **Production**: Configure proper network security

### Security Considerations
- Never hardcode secrets in configuration files
- Use OAuth when possible
- Restrict network access appropriately
- Regularly rotate authentication tokens

### Performance Optimization
- Use Docker image caching
- Configure appropriate resource limits
- Monitor MCP server performance
- Implement proper logging

---

## 🔍 Troubleshooting

### Common Issues

#### **MCP Server Not Responding**
```bash
# Check if server is running
docker ps | grep kafka-schema-reg-mcp

# Check server logs
docker logs <container_id>

# Test direct connection
curl http://localhost:38000/health
```

#### **Authentication Failures**
```bash
# Verify GitHub token
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
  https://api.github.com/user

# Check OAuth configuration
echo $GITHUB_CLIENT_ID
echo $GITHUB_CLIENT_SECRET
```

#### **Network Connectivity Issues**
```bash
# Check network connectivity
docker network ls

# Test registry connectivity
curl http://localhost:8081/subjects

# Verify Docker network
docker network inspect demo-deployment_default
```

### Debug Mode
Enable debug mode in the MCP server:
```json
{
  "env": {
    "DEBUG": "true",
    "LOG_LEVEL": "debug"
  }
}
```

### Client-Specific Debugging

| Client | Debug Method |
|--------|-------------|
| **Claude Desktop** | Check Claude logs in system logs |
| **VS Code** | Developer Console (`Help` → `Toggle Developer Tools`) |
| **Cursor** | Settings → MCP → View server status |
| **JetBrains** | Help → Show Log → mcp directory |

---

## 🚀 Advanced Configurations

### Multi-Registry Setup
```json
{
  "mcpServers": {
    "kafka-dev": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "-e", "SCHEMA_REGISTRY_URL=http://dev-registry:8081",
        "-e", "REGISTRY_NAME=development",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    },
    "kafka-prod": {
      "command": "docker", 
      "args": [
        "run", "--rm", "-i",
        "-e", "SCHEMA_REGISTRY_URL=http://prod-registry:8083",
        "-e", "REGISTRY_NAME=production",
        "-e", "READONLY=true",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### Custom Transport Configuration
```json
{
  "mcpServers": {
    "kafka-schema-registry-sse": {
      "transport": "sse",
      "url": "http://localhost:38000/sse",
      "headers": {
        "Authorization": "Bearer your_token"
      }
    }
  }
}
```

### Load Balancing Setup
```json
{
  "mcpServers": {
    "kafka-lb": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "-e", "SCHEMA_REGISTRY_URLS=http://registry1:8081,http://registry2:8081",
        "-e", "LOAD_BALANCE=true",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

---

## 📈 Next Steps

### **Explore the Demo**
1. **[Full Demo Setup](getting-started.md)** - Complete environment setup
2. **[Use Cases](use-cases.md)** - Real-world scenarios
3. **[Architecture](architecture.md)** - Technical deep-dive

### **Contribute to MCP Ecosystem**
1. **[Create your own MCP server](https://modelcontextprotocol.io/docs/tools/creating-servers)** 
2. **[Contribute to existing servers](https://github.com/modelcontextprotocol/servers)**
3. **[Join the MCP community](https://modelcontextprotocol.io/community)**

### **Production Deployment**
1. **[Deploy to Kubernetes](tutorials/kubernetes-deployment.md)**
2. **[CI/CD Integration](tutorials/cicd-integration.md)**
3. **[Monitoring & Observability](tutorials/monitoring.md)**

---

**🎉 Ready to transform your schema management with AI?** Choose your preferred IDE and start building with natural language schema operations!

---

<div align="center">

**Built with ❤️ for the AI and Apache Kafka communities**

[Documentation](../README.md) • [Demo](https://github.com/aywengo/demo-deployment) • [MCP Server](https://github.com/aywengo/kafka-schema-reg-mcp) • [Model Context Protocol](https://modelcontextprotocol.io/)

</div>
