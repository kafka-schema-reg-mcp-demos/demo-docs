# 🤖 AI-Powered Kafka Schema Management

**Transform how you manage Kafka schemas with natural language commands through any MCP-compatible AI client**

[![🚀 Live Demo](https://img.shields.io/badge/🚀-Live%20Demo-blue?style=for-the-badge)](https://github.com/kafka-schema-reg-mcp-demos/demo-deployment) [![📚 Documentation](https://img.shields.io/badge/📚-Documentation-green?style=for-the-badge)](https://github.com/aywengo/kafka-schema-reg-mcp) [![🎨 Demo Schemas](https://img.shields.io/badge/🎨-Demo%20Schemas-orange?style=for-the-badge)](https://github.com/kafka-schema-reg-mcp-demos/demo-schemas)

---

## 🎯 What if managing Kafka schemas was as simple as talking to any AI assistant?

Instead of wrestling with APIs, CLIs, and complex configurations, imagine:

```
You: "Register a new user profile schema for our e-commerce platform with id, name, email, and preferences"

AI Assistant: I'll create and register that schema for you in the development registry under the ecommerce context.

✅ Schema registered successfully!
✅ Compatibility verified with existing schemas
✅ Available in development environment
```

**That's the power of the Kafka Schema Registry MCP Server** - bringing AI-driven schema management to any MCP-compatible IDE or AI assistant.

---

## 🏗️ Architecture: Local vs Remote Setup

Our Kafka Schema Registry MCP implementation supports both **local development** and **remote enterprise** deployments:

### **🏠 Local Development Setup**
Perfect for development, testing, and learning:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Claude App    │    │   VS Code       │    │   Cursor IDE    │
│   (Desktop)     │    │   + Copilot     │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │ HTTP/SSE
                    ┌─────────────▼─────────────┐
                    │     Local MCP Server      │
                    │    (docker-compose)       │
                    │      localhost:38000      │
                    └─────────────┬─────────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        │                         │                         │
  ┌─────▼─────┐            ┌─────▼─────┐            ┌─────▼─────┐
  │    Dev    │            │  Staging  │            │   Prod    │
  │ Registry  │            │ Registry  │            │ Registry  │
  │   :8081   │            │   :8082   │            │   :8083   │
  └───────────┘            └───────────┘            └───────────┘
```

### **🌍 Remote Enterprise Setup**
Production-ready deployment with centralized authentication:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Claude App    │    │   VS Code       │    │  JetBrains IDE  │
│   (Desktop)     │    │   + Copilot     │    │    + AI         │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │ HTTPS/SSE + GitHub OAuth
                    ┌─────────────▼─────────────┐
                    │    Remote MCP Server      │
                    │   (Kubernetes/Cloud)      │
                    │   your-domain.com:443     │
                    └─────────────┬─────────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        │                         │                         │
  ┌─────▼─────┐            ┌─────▼─────┐            ┌─────▼─────┐
  │Enterprise │            │Enterprise │            │Enterprise │
  │   Dev     │            │ Staging   │            │   Prod    │
  │ Registry  │            │ Registry  │            │ Registry  │
  └───────────┘            └───────────┘            └───────────┘
```

---

## 🔌 MCP Client Compatibility

The **Model Context Protocol (MCP)** is an open standard that enables AI models to interact with external tools and services through a unified interface. Our Kafka Schema Registry MCP Server works seamlessly with any MCP-compatible client:

### **🎨 Supported AI Clients & IDEs**

<table>
<tr>
<td width="25%" align="center">
<h4>🤖 <a href="#claude-desktop-setup">Claude Desktop</a></h4>
<p><strong>Status:</strong> ✅ Full Support</p>
<ul>
<li>Native MCP integration</li>
<li>Natural language commands</li>
<li>Multi-registry support</li>
<li>GitHub OAuth integration</li>
</ul>
</td>
<td width="25%" align="center">
<h4>💻 <a href="#vs-code-copilot-setup">VS Code + Copilot</a></h4>
<p><strong>Status:</strong> ✅ Agent Mode (Preview)</p>
<ul>
<li>GitHub Copilot agent mode</li>
<li>Workspace/user configuration</li>
<li>Tool integration in chat</li>
<li>Auto-discovery support</li>
</ul>
</td>
<td width="25%" align="center">
<h4>⚡ <a href="#cursor-ide-setup">Cursor IDE</a></h4>
<p><strong>Status:</strong> ✅ Full Support</p>
<ul>
<li>One-click MCP installation</li>
<li>Project/global configuration</li>
<li>Agent & Composer modes</li>
<li>OAuth server support</li>
</ul>
</td>
<td width="25%" align="center">
<h4>🧠 <a href="#jetbrains-setup">JetBrains IDEs</a></h4>
<p><strong>Status:</strong> ✅ Full Support (2025.1+)</p>
<ul>
<li>IntelliJ IDEA, PyCharm, WebStorm</li>
<li>AI Assistant integration</li>
<li>Codebase-mode support</li>
<li>MCP proxy support</li>
</ul>
</td>
</tr>
</table>

### **🌐 Additional MCP Clients**

- **Microsoft Copilot Studio**: Enterprise-grade MCP integration
- **Eclipse IDE**: Copilot with MCP support  
- **Xcode**: GitHub Copilot agent mode
- **Emacs**: MCP client with gptel/llm integration
- **Warp Terminal**: AI-powered terminal with MCP
- **And many more**: [See full list →](https://modelcontextprotocol.io/clients)

---

## 🚀 Quick Setup Guide

Choose your deployment model:

### **🏠 Option 1: Local Development Setup**

#### Step 1: Start Local Infrastructure

```bash
# Clone the demo deployment
git clone https://github.com/kafka-schema-reg-mcp-demos/demo-deployment.git
cd demo-deployment

# Configure GitHub OAuth (optional for local dev)
cp .env.example .env
# Edit .env with your GitHub OAuth credentials

# Start the complete environment
docker-compose -f docker-compose.github-oauth.yml up -d

# Wait for services to be ready (1-2 minutes)
docker-compose ps

# Load demo data
./scripts/setup-demo-data.sh
```

This starts:
- **MCP Server**: `localhost:38000` (with GitHub OAuth)
- **Schema Registries**: Dev (8081), Staging (8082), Production (8083)
- **Demo UI**: `localhost:3000`
- **Monitoring**: Prometheus (9090), Grafana (3001)

#### Step 2: Configure Your IDE/AI Client

**🔑 Authentication Options:**
- **With GitHub OAuth**: Use personal access token with org membership
- **Local Development**: Set `DEV_MODE=true` in .env (no auth required)

Now configure your client to connect to the **existing MCP server**:

### **🌍 Option 2: Remote Enterprise Setup**

For production deployments, deploy the MCP server to your infrastructure (Kubernetes, Docker Swarm, etc.) and configure clients to connect to the remote endpoint.

---

## 🛠️ Client Configuration Examples

> **Important**: Clients connect to existing MCP servers via HTTP/SSE, they do **not** create Docker containers

### Claude Desktop Setup

Connect to your local or remote MCP server:

```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "command": "mcp-client-http", 
      "args": [
        "--url", "http://localhost:38000",
        "--auth", "github-oauth",
        "--org", "kafka-schema-reg-mcp-demos"
      ],
      "env": {
        "GITHUB_TOKEN": "your_github_personal_access_token"
      }
    }
  }
}
```

For remote servers, change the URL:
```json
"--url", "https://your-mcp-server.company.com"
```

### VS Code + Copilot Setup

Create `.vscode/settings.json` in your workspace:

```json
{
  "mcp.servers": {
    "kafka-schema-registry": {
      "transport": "sse",
      "endpoint": "http://localhost:38000/mcp/sse", 
      "headers": {
        "Authorization": "Bearer your_github_personal_access_token",
        "X-GitHub-Org": "kafka-schema-reg-mcp-demos"
      }
    }
  }
}
```

### Cursor IDE Setup

Create `.cursor/settings.json` in your project:

```json
{
  "mcp.servers": {
    "kafka-schema-registry": {
      "transport": "sse",
      "endpoint": "http://localhost:38000/mcp/sse",
      "headers": {
        "Authorization": "Bearer your_github_personal_access_token",
        "X-GitHub-Org": "kafka-schema-reg-mcp-demos"
      }
    }
  }
}
```

### JetBrains IDEs Setup

In **Settings → Tools → AI Assistant → Model Context Protocol (MCP)**:

```json
{
  "servers": {
    "kafka-schema-registry": {
      "transport": "sse",
      "endpoint": "http://localhost:38000/mcp/sse",
      "headers": {
        "Authorization": "Bearer your_github_personal_access_token",
        "X-GitHub-Org": "kafka-schema-reg-mcp-demos"
      }
    }
  }
}
```

---

## 🔐 GitHub OAuth Authentication

### Setting Up GitHub Authentication

1. **Create GitHub OAuth App**:
   - Go to: https://github.com/settings/applications/new
   - Application name: "My Schema Registry MCP"
   - Homepage URL: `http://localhost:3000` (or your domain)
   - Authorization callback: `http://localhost:38000/auth/callback`

2. **Generate Personal Access Token**:
   - Go to: https://github.com/settings/tokens
   - Generate token with appropriate scopes:
     - `public_repo` - Read access to schemas
     - `repo` - Write access to schemas  
     - `admin:org` - Full admin access (if org member)

### Permission Levels

| GitHub Scope | MCP Access | Description |
|--------------|------------|-------------|
| `public_repo` | **Read** | View schemas, export documentation |
| `repo` | **Write** | Register schemas, update configs (+ read) |
| `admin:org` | **Admin** | Delete subjects, manage production (+ write + read) |

### Team-Based Access Control

Configure team-based permissions in your `.env`:

```bash
GITHUB_ORG=kafka-schema-reg-mcp-demos
GITHUB_TEAM_READ=community      # Read access
GITHUB_TEAM_WRITE=developers    # Write access  
GITHUB_TEAM_ADMIN=maintainers   # Admin access
```

---

## 🌟 Why This Changes Everything

### **Before**: Traditional Schema Management
```bash
# Complex CLI commands
confluent schema-registry subject list --config-file config.properties

# Manual JSON crafting  
curl -X POST -H "Content-Type: application/json" \
  --data '{"schema": "{\"type\":\"record\",...}"}' \
  http://localhost:8081/subjects/user-profile/versions

# Separate tools for different operations
# No AI assistance or natural language interface
# Context switching between documentation and CLI
```

### **After**: AI-Powered Schema Management
```
You: "Show me all schemas in the production environment"
You: "Check if this schema evolution is backward compatible"  
You: "Migrate user-events schema from staging to production"
You: "Export all e-commerce schemas for documentation"
```

**Any AI assistant handles all the complexity while you focus on the business logic.**

---

## 🎬 Enterprise Use Cases

### **🎯 Multi-Environment Schema Promotion**

```
DevOps Engineer: "The order-events schema tested successfully in staging. 
                  Promote it to production."

AI Assistant: I'll migrate the order-events schema from staging to production.
              [Performs cross-registry migration]
              ✅ Schema migrated successfully  
              ✅ Production registry updated
              ✅ All versions preserved
```

### **🔄 Schema Evolution for Growth**

```
Developer: "Our user profile schema needs to support international addresses. 
           Can you add country and postal_code fields that are backward compatible?"

AI Assistant: I'll add those fields as optional with defaults to maintain backward compatibility.
              [Registers new schema version]
              ✅ New version registered successfully
              ✅ Backward compatibility verified
              ✅ Ready for gradual rollout
```

### **📊 Compliance and Documentation**

```
Data Governance: "Export all schemas from our fintech context for the audit."

AI Assistant: I'll export all fintech schemas with their documentation and evolution history.
              [Generates comprehensive export]
              ✅ 12 schemas exported in JSON format
              ✅ Evolution history included
              ✅ Compliance metadata attached
```

---

## 🚀 Complete Demo Ecosystem

We've built a comprehensive demonstration that showcases enterprise-grade schema management:

<table>
<tr>
<td width="25%" align="center">
<h3>🤖 <a href="https://github.com/aywengo/kafka-schema-reg-mcp">MCP Server</a></h3>
<p>The core AI integration that connects any MCP client to Kafka Schema Registry</p>
<ul>
<li>48 MCP tools for complete schema operations</li>
<li>Multi-registry support (dev/staging/prod)</li>
<li>Context-based organization</li>
<li>GitHub OAuth integration</li>
</ul>
</td>
<td width="25%" align="center">
<h3>🏗️ <a href="https://github.com/kafka-schema-reg-mcp-demos/demo-deployment">Demo Environment</a></h3>
<p>Production-ready deployment with realistic infrastructure</p>
<ul>
<li>3-tier registry setup</li>
<li>GitHub OAuth authentication</li>
<li>Monitoring & observability</li>
<li>Docker Compose deployment</li>
</ul>
</td>
<td width="25%" align="center">
<h3>🎨 <a href="https://github.com/kafka-schema-reg-mcp-demos/demo-schemas">Business Schemas</a></h3>
<p>Real-world schemas from multiple industries</p>
<ul>
<li>E-commerce platform</li>
<li>IoT sensor data</li>
<li>Financial services</li>
<li>SaaS multi-tenancy</li>
</ul>
</td>
<td width="25%" align="center">
<h3>📚 <a href="https://github.com/kafka-schema-reg-mcp-demos/demo-docs">Documentation</a></h3>
<p>Comprehensive guides and tutorials</p>
<ul>
<li>Getting started guides</li>
<li>Architecture deep-dives</li>
<li>Use case walkthroughs</li>
<li>Integration patterns</li>
</ul>
</td>
</tr>
</table>

---

## 🔗 Apache Kafka & Confluent Integration

Built for the **[Apache Kafka](https://kafka.apache.org/)** ecosystem with full **[Confluent Schema Registry](https://docs.confluent.io/platform/current/schema-registry/fundamentals/index.html)** compatibility:

### **🎯 Supported Platforms**
- **[Confluent Platform](https://www.confluent.io/product/confluent-platform/)** - Full enterprise distribution with advanced features
- **[Confluent Cloud](https://www.confluent.io/confluent-cloud/)** - Fully managed cloud service
- **[Apache Kafka](https://kafka.apache.org/)** - Open source distribution
- **[Amazon MSK](https://aws.amazon.com/msk/)** - AWS managed Kafka with Schema Registry
- **[Azure Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/)** - Azure managed Kafka service

### **📋 Schema Format Support**
- **[Apache Avro](https://avro.apache.org/)** - Primary focus with full evolution support
- **[JSON Schema](https://json-schema.org/)** - Web-friendly schema definition
- **[Protocol Buffers](https://protobuf.dev/)** - Google's language-neutral serialization

### **🔧 Enterprise Features**
- **[Schema Evolution](https://docs.confluent.io/platform/current/schema-registry/avro.html#schema-evolution-and-compatibility)** - Backward, forward, and full compatibility
- **[Schema Contexts](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#contexts)** - Logical grouping and multi-tenancy
- **[Compatibility Levels](https://docs.confluent.io/platform/current/schema-registry/avro.html#compatibility-types)** - Fine-grained evolution control
- **[Subject Naming](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#subject-name-strategy)** - Flexible naming strategies

---

## 🌟 Real-World Schema Examples

Our demo includes production-ready schemas across multiple industries:

### **🛒 E-commerce Platform**
```json
{
  "type": "record",
  "name": "UserProfile", 
  "namespace": "com.ecommerce.user",
  "fields": [
    {"name": "user_id", "type": "string"},
    {"name": "email", "type": "string"},
    {"name": "preferences", "type": {
      "type": "record",
      "name": "UserPreferences",
      "fields": [
        {"name": "newsletter", "type": "boolean", "default": true},
        {"name": "notifications", "type": "boolean", "default": true}
      ]
    }}
  ]
}
```

### **🏦 Financial Services**
```json
{
  "type": "record",
  "name": "Transaction",
  "namespace": "com.fintech.payments", 
  "fields": [
    {"name": "transaction_id", "type": "string"},
    {"name": "amount", "type": {"type": "bytes", "logicalType": "decimal"}},
    {"name": "currency", "type": "string"},
    {"name": "compliance_data", "type": {
      "type": "record", 
      "name": "ComplianceInfo",
      "fields": [
        {"name": "kyc_verified", "type": "boolean"},
        {"name": "risk_score", "type": "int"}
      ]
    }}
  ]
}
```

### **🌐 IoT Platform** 
```json
{
  "type": "record",
  "name": "SensorReading",
  "namespace": "com.iot.sensors",
  "fields": [
    {"name": "device_id", "type": "string"},
    {"name": "timestamp", "type": {"type": "long", "logicalType": "timestamp-millis"}},
    {"name": "temperature", "type": "float"},
    {"name": "humidity", "type": "float"},
    {"name": "location", "type": {
      "type": "record",
      "name": "GeoLocation", 
      "fields": [
        {"name": "latitude", "type": "double"},
        {"name": "longitude", "type": "double"}
      ]
    }}
  ]
}
```

**🎨 Explore All Schemas:** [Demo Schemas Repository →](https://github.com/kafka-schema-reg-mcp-demos/demo-schemas)

---

## 📈 Evolution Patterns Demonstrated

### **✅ Backward Compatible Evolution**
```
// v1: Basic user profile
{"name": "user_id", "type": "string"}
{"name": "email", "type": "string"}

// v2: Add optional preferences (backward compatible)
{"name": "preferences", "type": ["null", "UserPreferences"], "default": null}
```

### **🔄 Schema Migration Across Environments**
```
Development → Staging → Production
     ↓            ↓           ↓
Context: dev   Context: staging   Context: prod
Access: Full   Access: Limited    Access: ReadOnly
```

### **🏢 Multi-Tenant Organization**
```
ecommerce/          fintech/           iot-platform/
├── user-profile    ├── transactions   ├── sensor-readings
├── order-events    ├── accounts       ├── device-status  
└── products        └── compliance     └── alerts
```

---

## 🎯 Perfect For Your Team

### **👨‍💻 Developers**
> *"Finally, schema management that doesn't interrupt my flow"*

**What you get:**
- Natural language schema operations in your favorite IDE
- Instant compatibility checking before deployment
- AI-assisted schema design and evolution
- Context-aware environment management

**Compatible with:** Claude Desktop, VS Code Copilot, Cursor, JetBrains IDEs

### **🔧 DevOps Engineers**  
> *"Multi-environment schema governance that actually works"*

**What you get:**
- Automated schema promotion pipelines
- Cross-registry migration and synchronization
- Production-safe readonly modes
- Comprehensive monitoring and observability

### **📊 Data Engineers**
> *"Schema documentation and governance on autopilot"*

**What you get:**
- Automatic schema documentation generation
- Evolution tracking and compatibility analysis
- Bulk export and backup capabilities
- Compliance-ready audit trails

### **🏢 Platform Teams**
> *"Enterprise schema management without the enterprise complexity"*

**What you get:**
- Multi-tenant context isolation
- Role-based access control via GitHub
- Centralized schema registry management
- Self-service developer experience

---

## 🔗 Complete Ecosystem Links

| Component | Purpose | Repository | Status |
|-----------|---------|------------|---------|
| **🤖 MCP Server** | Core AI integration | [kafka-schema-reg-mcp](https://github.com/aywengo/kafka-schema-reg-mcp) | ✅ Production Ready |
| **🏗️ Demo Deployment** | Infrastructure & OAuth | [demo-deployment](https://github.com/kafka-schema-reg-mcp-demos/demo-deployment) | ✅ Ready to Deploy |
| **🎨 Demo Schemas** | Business examples | [demo-schemas](https://github.com/kafka-schema-reg-mcp-demos/demo-schemas) | ✅ 14 schemas, 39 versions |
| **📚 Documentation** | Guides & tutorials | [demo-docs](https://github.com/kafka-schema-reg-mcp-demos/demo-docs) | ✅ Comprehensive guides |

---

## 🎉 Join the Future of Schema Management

### **⭐ Star the Project**
Help others discover AI-powered schema management:
- ⭐ [kafka-schema-reg-mcp](https://github.com/aywengo/kafka-schema-reg-mcp)
- ⭐ [demo-deployment](https://github.com/kafka-schema-reg-mcp-demos/demo-deployment)  
- ⭐ [demo-schemas](https://github.com/kafka-schema-reg-mcp-demos/demo-schemas)

### **🚀 Get Started Today**
1. **[Try the Local Demo →](#-option-1-local-development-setup)** - Full environment in 5 minutes
2. **[Read the Setup Guide →](#-quick-setup-guide)** - Comprehensive deployment instructions
3. **[Configure Your IDE →](#-client-configuration-examples)** - Connect your favorite AI assistant
4. **[Explore Use Cases →](#-enterprise-use-cases)** - Real-world scenarios

### **🤝 Community & Support**
- **Issues**: [GitHub Issues](https://github.com/aywengo/kafka-schema-reg-mcp/issues)
- **Discussions**: [GitHub Discussions](https://github.com/aywengo/kafka-schema-reg-mcp/discussions)
- **Contributions**: See [Contributing Guide](https://github.com/aywengo/kafka-schema-reg-mcp/blob/main/CONTRIBUTING.md)

---

**🤖 Ready to transform your schema management with AI?** [Start with the local setup →](#-option-1-local-development-setup)

---

<div align="center">

**Built with ❤️ for the [Apache Kafka](https://kafka.apache.org/) and AI community**

[Documentation](https://github.com/kafka-schema-reg-mcp-demos/demo-docs) • [Demo](https://github.com/kafka-schema-reg-mcp-demos/demo-deployment) • [Schemas](https://github.com/kafka-schema-reg-mcp-demos/demo-schemas) • [MCP Server](https://github.com/aywengo/kafka-schema-reg-mcp)

**Powered by:** [Apache Kafka](https://kafka.apache.org/) • [Confluent Schema Registry](https://docs.confluent.io/platform/current/schema-registry/) • [Apache Avro](https://avro.apache.org/) • [Model Context Protocol](https://modelcontextprotocol.io/)

</div>
