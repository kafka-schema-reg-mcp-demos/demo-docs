# 🤖 AI-Powered Kafka Schema Management

**Transform how you manage Kafka schemas with natural language commands through any MCP-compatible AI client**

[![🚀 Live Demo](https://img.shields.io/badge/🚀-Live%20Demo-blue?style=for-the-badge)](https://github.com/aywengo/demo-deployment) [![📚 Documentation](https://img.shields.io/badge/📚-Documentation-green?style=for-the-badge)](https://github.com/aywengo/kafka-schema-reg-mcp) [![🎨 Demo Schemas](https://img.shields.io/badge/🎨-Demo%20Schemas-orange?style=for-the-badge)](https://github.com/aywengo/demo-schemas)

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

## 🛠️ Quick Setup for Your IDE

Choose your preferred development environment:

### Claude Desktop Setup

```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "--network", "demo-deployment_default",
        "-e", "SCHEMA_REGISTRY_URL=http://dev-registry:8081",
        "aywengo/kafka-schema-reg-mcp:v2.0.0-rc1"
      ]
    }
  }
}
```

### VS Code Copilot Setup

Create `.vscode/mcp.json` in your workspace:
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

### Cursor IDE Setup

Create `.cursor/mcp.json` in your project:
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

### JetBrains Setup

In **Settings → Tools → AI Assistant → Model Context Protocol (MCP)**:
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
<h3>🏗️ <a href="https://github.com/aywengo/demo-deployment">Demo Environment</a></h3>
<p>Production-ready deployment with realistic infrastructure</p>
<ul>
<li>3-tier registry setup</li>
<li>GitHub OAuth authentication</li>
<li>Monitoring & observability</li>
<li>Docker Compose deployment</li>
</ul>
</td>
<td width="25%" align="center">
<h3>🎨 <a href="https://github.com/aywengo/demo-schemas">Business Schemas</a></h3>
<p>Real-world schemas from multiple industries</p>
<ul>
<li>E-commerce platform</li>
<li>IoT sensor data</li>
<li>Financial services</li>
<li>SaaS multi-tenancy</li>
</ul>
</td>
<td width="25%" align="center">
<h3>📚 <a href="https://github.com/aywengo/demo-docs">Documentation</a></h3>
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

**🌐 Kafka Ecosystem Resources:**
- **[Kafka Documentation](https://kafka.apache.org/documentation/)** - Complete Apache Kafka guide
- **[Confluent Documentation](https://docs.confluent.io/)** - Enterprise platform documentation  
- **[Schema Registry API](https://docs.confluent.io/platform/current/schema-registry/develop/api.html)** - REST API reference
- **[Kafka Community](https://kafka.apache.org/contact)** - Get involved with the broader ecosystem

---

## 🎬 See It In Action

### **🎯 Enterprise Scenarios**

**Scenario 1: Schema Evolution for E-commerce Growth**
```
Developer: "Our user profile schema needs to support international addresses. 
           Can you add country and postal_code fields that are backward compatible?"

AI Assistant: I'll add those fields as optional with defaults to maintain backward compatibility.
              [Registers new schema version]
              ✅ New version registered successfully
              ✅ Backward compatibility verified
              ✅ Ready for gradual rollout
```

**Scenario 2: Multi-Environment Promotion**
```
DevOps Engineer: "The order-events schema tested successfully in staging. 
                  Promote it to production."

AI Assistant: I'll migrate the order-events schema from staging to production.
              [Performs cross-registry migration]
              ✅ Schema migrated successfully  
              ✅ Production registry updated
              ✅ All versions preserved
```

**Scenario 3: Compliance and Documentation**
```
Data Governance: "Export all schemas from our fintech context for the audit."

AI Assistant: I'll export all fintech schemas with their documentation and evolution history.
              [Generates comprehensive export]
              ✅ 12 schemas exported in JSON format
              ✅ Evolution history included
              ✅ Compliance metadata attached
```

### **🔒 GitHub OAuth Integration**

Realistic permission management using familiar GitHub authentication:

| GitHub Permission | MCP Access | Use Case |
|------------------|------------|-----------|
| `public_repo` | **Read** | View schemas, export documentation |
| `repo` | **Write** | Register schemas, update configs |
| `admin:org` | **Admin** | Manage production, delete subjects |

---

## 🏗️ Architecture: Enterprise-Ready Design

```
┌─────────────────────────────────────────────────────────────────┐
│                     MCP-Compatible Clients                      │
│  Claude Desktop │ VS Code Copilot │ Cursor │ JetBrains │ Others │
└─────────────────────┬───────────────────────────────────────────┘
                      │ MCP Protocol
┌─────────────────────▼───────────────────────────────────────────┐
│                  MCP Server (GitHub OAuth)                      │
│  ┌─────────────┐ ┌─────────────┐  ┌─────────────┐ ┌──────────┐  │
│  │   Schema    │ │  Context    │  │   Config    │ │   Mode   │  │
│  │ Management  │ │ Management  │  │ Management  │ │ Control  │  │
│  └─────────────┘ └─────────────┘  └─────────────┘ └──────────┘  │
└─────────────┬───────────────┬───────────────┬───────────────────┘
              │               │               │
    ┌─────────▼─────────┐ ┌───▼────────┐ ┌───▼──────────┐
    │   Development     │ │  Staging   │ │ Production   │
    │   Registry        │ │  Registry  │ │   Registry   │
    │   :8081           │ │   :8082    │ │    :8083     │
    │ (Full Access)     │ │ (Limited)  │ │ (Read-Only)  │
    └───────────────-───┘ └────────────┘ └──────────────┘
```

### **🔧 Key Architecture Benefits**

- **🤖 AI-First**: Natural language interface eliminates learning curve
- **🏢 Enterprise-Grade**: Multi-registry, context isolation, role-based access
- **🔒 Secure**: GitHub OAuth with granular permissions
- **📈 Scalable**: Handles complex schema evolution and governance
- **🌐 Cloud-Ready**: Docker-based deployment for any environment
- **🔌 Client-Agnostic**: Works with any MCP-compatible AI client

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

**Try it:** [Developer Quick Start →](getting-started.md#developers)

### **🔧 DevOps Engineers**  
> *"Multi-environment schema governance that actually works"*

**What you get:**
- Automated schema promotion pipelines
- Cross-registry migration and synchronization
- Production-safe readonly modes
- Comprehensive monitoring and observability

**Try it:** [DevOps Deployment Guide →](architecture.md#deployment)

### **📊 Data Engineers**
> *"Schema documentation and governance on autopilot"*

**What you get:**
- Automatic schema documentation generation
- Evolution tracking and compatibility analysis
- Bulk export and backup capabilities
- Compliance-ready audit trails

**Try it:** [Data Governance Workflows →](use-cases.md#data-governance)

### **🏢 Platform Teams**
> *"Enterprise schema management without the enterprise complexity"*

**What you get:**
- Multi-tenant context isolation
- Role-based access control via GitHub
- Centralized schema registry management
- Self-service developer experience

**Try it:** [Platform Setup Guide →](tutorials/platform-setup.md)

---

## 🚀 Quick Start (5 Minutes)

### **Option 1: Full Demo Experience**
```bash
# Clone and start the complete demo environment
git clone https://github.com/aywengo/demo-deployment.git
cd demo-deployment
docker-compose -f docker-compose.github-oauth.yml up -d

# Configure your MCP client (examples above)
# Restart your IDE/AI client and start managing schemas with AI!
```

### **Option 2: Try with Your Registry**
```bash
# Use the MCP server with your existing Schema Registry
docker run -p 38000:8000 \
  -e SCHEMA_REGISTRY_URL=http://your-registry:8081 \
  aywengo/kafka-schema-reg-mcp:v2.0.0-rc1
```

**📖 Detailed Setup:** [Complete Getting Started Guide →](getting-started.md)

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

**🎨 Explore All Schemas:** [Demo Schemas Repository →](https://github.com/aywengo/demo-schemas)

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

## 🔗 Complete Ecosystem Links

| Component | Purpose | Repository | Status |
|-----------|---------|------------|---------| 
| **🤖 MCP Server** | Core AI integration | [kafka-schema-reg-mcp](https://github.com/aywengo/kafka-schema-reg-mcp) | ✅ Production Ready |
| **🏗️ Demo Deployment** | Infrastructure & OAuth | [demo-deployment](https://github.com/aywengo/demo-deployment) | ✅ Ready to Deploy |
| **🎨 Demo Schemas** | Business examples | [demo-schemas](https://github.com/aywengo/demo-schemas) | ✅ 14 schemas, 39 versions |
| **📚 Documentation** | Guides & tutorials | [demo-docs](https://github.com/aywengo/demo-docs) | ✅ Comprehensive guides |

---

## 🎉 Join the Future of Schema Management

### **⭐ Star the Project**
Help others discover AI-powered schema management:
- ⭐ [kafka-schema-reg-mcp](https://github.com/aywengo/kafka-schema-reg-mcp)
- ⭐ [demo-deployment](https://github.com/aywengo/demo-deployment)  
- ⭐ [demo-schemas](https://github.com/aywengo/demo-schemas)

### **🚀 Get Started Today**
1. **[Try the Demo →](https://github.com/aywengo/demo-deployment)** - Full environment in 5 minutes
2. **[Read the Docs →](getting-started.md)** - Comprehensive setup guide  
3. **[Explore Use Cases →](use-cases.md)** - Real-world scenarios
4. **[Check the Architecture →](architecture.md)** - Technical deep-dive

### **🤝 Community & Support**
- **Issues**: [GitHub Issues](https://github.com/aywengo/kafka-schema-reg-mcp/issues)
- **Discussions**: [GitHub Discussions](https://github.com/aywengo/kafka-schema-reg-mcp/discussions)
- **Contributions**: See [Contributing Guide](https://github.com/aywengo/kafka-schema-reg-mcp/blob/main/CONTRIBUTING.md)

---

**🤖 Ready to transform your schema management with AI?** [Start with the demo environment →](https://github.com/aywengo/demo-deployment)

---

<div align="center">

**Built with ❤️ for the [Apache Kafka](https://kafka.apache.org/) and AI community**

[Documentation](https://github.com/aywengo/demo-docs) • [Demo](https://github.com/aywengo/demo-deployment) • [Schemas](https://github.com/aywengo/demo-schemas) • [MCP Server](https://github.com/aywengo/kafka-schema-reg-mcp)

**Powered by:** [Apache Kafka](https://kafka.apache.org/) • [Confluent Schema Registry](https://docs.confluent.io/platform/current/schema-registry/) • [Apache Avro](https://avro.apache.org/) • [Model Context Protocol](https://modelcontextprotocol.io/)

</div>
