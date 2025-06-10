# 👨‍💻 Developer Quick Start

**Get productive with AI-powered schema management in 15 minutes using any MCP-compatible IDE**

This tutorial gets you hands-on with the Kafka Schema Registry MCP server using realistic e-commerce schemas. You'll learn to register, evolve, and manage schemas using natural language commands in your preferred development environment.

## 🎯 What You'll Learn

- Set up any MCP-compatible IDE with Schema Registry integration
- Register your first schema using natural language
- Perform schema evolution with compatibility checking
- Export schemas for documentation and backup
- Debug schema issues with AI assistance

## 🔌 IDE Compatibility

This tutorial works with any MCP-compatible client:

| IDE | Status | Notes |
|-----|--------|-------|
| **🤖 Claude Desktop** | ✅ Full Support | Conversational interface |
| **💻 VS Code + Copilot** | ✅ Agent Mode | In-editor assistance |
| **⚡ Cursor** | ✅ Full Support | AI-first development |
| **🧠 JetBrains IDEs** | ✅ Full Support (2025.1+) | Professional tools |

**📖 Need setup help?** See our **[MCP Client Integration Guide](../MCP-CLIENT-INTEGRATION.md)** for detailed configuration instructions.

## 📋 Prerequisites

- **Any MCP-compatible IDE** (choose from above)
- **Docker** and **Docker Compose** for demo environment
- **Basic Kafka knowledge** (helpful but not required)
- **15 minutes** of focused time

## ⏱️ Duration: 15 minutes

## 🚀 Step 1: Start Demo Environment (3 minutes)

### Launch the Complete Demo

```bash
# Clone the demo environment
git clone https://github.com/aywengo/demo-deployment.git
cd demo-deployment

# Start with the simple configuration (no OAuth needed for local dev)
docker-compose up -d

# Wait for services to start (about 2 minutes)
docker-compose ps
```

**Expected Output:**
```
SERVICE              STATUS
dev-registry         ✅ Up (port 8081)
staging-registry     ✅ Up (port 8082)
production-registry  ✅ Up (port 8083)
mcp-server          ✅ Up (port 38000)
```

### Verify Services

```bash
# Check registry connectivity
curl http://localhost:8081/subjects
# Should return: []

# Check MCP server health
curl http://localhost:38000/health
# Should return: {"status": "healthy"}
```

## 🔧 Step 2: Configure Your MCP Client (2 minutes)

Choose your preferred development environment:

### 🤖 Claude Desktop Setup

```bash
# Copy the ready-made configuration
cp config-examples/claude_desktop_config.json \
   ~/Library/Application\ Support/Claude/claude_desktop_config.json

# For Linux users:
cp config-examples/claude_desktop_config.json \
   ~/.config/claude-desktop/config.json

# Restart Claude Desktop
```

### 💻 VS Code + Copilot Setup

Create `.vscode/mcp.json` in your workspace:
```json
{
  "servers": {
    "kafka-schema-registry": {
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
        "aywengo/kafka-schema-reg-mcp:stable"
      ]
    }
  }
}
```

**Activate**: Open Copilot Chat → Select **Agent** → Look for tools 🔧 icon

### ⚡ Cursor Setup

Create `.cursor/mcp.json` in your project:
```json
{
  "mcpServers": {
    "kafka-schema-registry": {
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
        "aywengo/kafka-schema-reg-mcp:stable"
      ]
    }
  }
}
```

**Verify**: Check that server indicator turns **green**

### 🧠 JetBrains Setup

1. Go to **Settings** → **Tools** → **AI Assistant** → **Model Context Protocol (MCP)**
2. Add server with the same Docker configuration as above
3. Enable **"Codebase"** mode in AI Assistant

## 🎨 Step 3: Your First Schema Registration (5 minutes)

### Test Basic Connectivity

**What to type** (natural language - works in any MCP client):
```
"What schema registries are available?"
```

**Expected AI Response** (may vary by client):
```
I'll check what Schema Registry instances are configured and available.

✅ Available Schema Registries:
1. **development** - http://dev-registry:8081 (ReadWrite)
2. **staging** - http://staging-registry:8082 (ReadWrite)  
3. **production** - http://prod-registry:8083 (ReadOnly)

All registries are healthy and accessible.
```

### Register Your First Schema

**What to type:**
```
"Register a user profile schema for our e-commerce platform with fields for user_id, email, name, and preferences"
```

**Expected AI Response:**
```
I'll create and register a user profile schema for your e-commerce platform with the fields you specified.

✅ Schema Registration Successful!

**Details:**
- **Subject**: user-profile
- **Registry**: development
- **Context**: default
- **Schema ID**: 1
- **Version**: 1

**Schema Structure:**
{
  "type": "record",
  "name": "UserProfile",
  "namespace": "com.ecommerce.user",
  "doc": "User profile information for e-commerce platform",
  "fields": [
    {
      "name": "user_id",
      "type": "string",
      "doc": "Unique identifier for the user"
    },
    {
      "name": "email", 
      "type": "string",
      "doc": "User's email address"
    },
    {
      "name": "name",
      "type": "string", 
      "doc": "User's full name"
    },
    {
      "name": "preferences",
      "type": {
        "type": "record",
        "name": "UserPreferences",
        "fields": [
          {
            "name": "newsletter",
            "type": "boolean",
            "default": true,
            "doc": "Newsletter subscription preference"
          },
          {
            "name": "notifications",
            "type": "boolean", 
            "default": true,
            "doc": "Push notification preference"
          }
        ]
      },
      "doc": "User preference settings"
    }
  ]
}

The schema is now available in the development registry and ready for use by your applications.
```

### Verify Registration

**What to type:**
```
"Show me all subjects in the development registry"
```

**Expected AI Response:**
```
📋 **Development Registry Subjects:**
1. **user-profile** (1 version)

**Subject Details:**
- Latest Version: 1
- Schema Type: AVRO
- Compatibility: BACKWARD (default)
- Last Modified: Just now
```

## 🔄 Step 4: Schema Evolution (3 minutes)

### Add New Fields with Compatibility Check

**What to type:**
```
"I need to add address information to the user profile. Add a shipping_address field that includes street, city, state, and postal_code. Make sure it's backward compatible."
```

**Expected AI Response:**
```
I'll add shipping address information to your user profile schema while ensuring backward compatibility.

🔍 **Compatibility Analysis:**
✅ Adding optional field with default is backward compatible
✅ Existing consumers will continue working unchanged
✅ New consumers can access enhanced address data

**New Field Structure:**
{
  "name": "shipping_address",
  "type": [
    "null",
    {
      "type": "record",
      "name": "ShippingAddress", 
      "fields": [
        {"name": "street", "type": "string"},
        {"name": "city", "type": "string"},
        {"name": "state", "type": "string"},
        {"name": "postal_code", "type": "string"}
      ]
    }
  ],
  "default": null,
  "doc": "Optional shipping address information"
}

✅ **Schema Evolution Successful!**
- **New Version**: 2
- **Schema ID**: 2  
- **Compatibility**: BACKWARD_COMPATIBLE
- **Existing consumers**: Will continue working without changes
```

### Verify Evolution

**What to type:**
```
"Show me the version history for user-profile"
```

**Expected AI Response:**
```
📚 **user-profile Version History:**

**Version 2** (Latest) - Just registered
- Added: shipping_address field (optional)
- Compatibility: BACKWARD with v1
- Changes: Enhanced for international shipping

**Version 1** - Initial version  
- Fields: user_id, email, name, preferences
- Status: Active, compatible with v2
- Usage: Safe for existing consumers
```

## 📤 Step 5: Schema Export and Documentation (2 minutes)

### Export Schema for Documentation

**What to type:**
```
"Export the latest user-profile schema in JSON format for our API documentation"
```

**Expected AI Response:**
```
📄 **Schema Export Complete**

**Export Details:**
- **Subject**: user-profile
- **Version**: 2 (latest)
- **Format**: JSON
- **Size**: 1.2 KB

**Exported Schema:**
The schema has been formatted with complete field documentation, default values, and type information suitable for API documentation. The export includes:

- Complete field specifications
- Documentation strings for all fields
- Default value information
- Nested record structures
- Type constraints and validation rules

You can use this export for:
- API documentation generation
- Consumer code generation
- Schema registry backups
- Compliance documentation
```

### Export All Schemas for Backup

**What to type:**
```
"Export all schemas from the development registry for backup purposes"
```

**Expected AI Response:**
```
💾 **Complete Registry Backup**

**Backup Summary:**
- **Registry**: development  
- **Schemas Exported**: 1 subject, 2 versions
- **Total Size**: 2.4 KB
- **Format**: JSON with metadata

**Included Data:**
- All schema versions and metadata
- Compatibility configurations
- Subject-level settings
- Registry-level configurations
- Export timestamp and lineage

**Backup Uses:**
- Disaster recovery procedures
- Registry migration preparation  
- Compliance audit requirements
- Development environment replication
```

## ✅ Verification & Next Steps

### Verify Your Progress

At this point, you should have:

1. ✅ **Demo environment running** with 3 registries
2. ✅ **MCP client connected** to MCP server
3. ✅ **Schema registered** (user-profile v1)
4. ✅ **Schema evolved** safely (user-profile v2)
5. ✅ **Export capabilities** tested and working

### Test Your Knowledge

Try these additional commands to reinforce your learning:

```
"Check if I can safely remove the email field from user-profile"
"Show me the configuration settings for the development registry"
"Create a product schema with id, name, price, and category fields"
"Compare the development and staging registries"
```

### Common Issues and Solutions

#### **🔧 Tools/MCP Not Available**

**Claude Desktop:**
```bash
# Check configuration file location and syntax
python -m json.tool ~/Library/Application\ Support/Claude/claude_desktop_config.json

# Restart Claude Desktop completely
```

**VS Code Copilot:**
```bash
# Ensure agent mode is activated
# Look for tools icon in Copilot Chat
# Restart VS Code if needed
```

**Cursor:**
```bash
# Check server indicator is green
# Verify .cursor/mcp.json exists
# Check Cursor settings → MCP
```

**JetBrains:**
```bash
# Enable "Codebase" mode in AI Assistant
# Check Settings → Tools → AI Assistant → MCP
# Restart IDE if needed
```

#### **❌ Connection Errors**
```bash
# Verify all services are running
docker-compose ps

# Check MCP server logs
docker-compose logs mcp-server

# Test direct registry connectivity  
curl http://localhost:8081/subjects
```

#### **🚫 Schema Registration Failures**

Test with these debug commands:
```
"Test the connection to all registries"
"Check what subjects exist in development"
"Show me the current registry configuration"
```

## 🚀 Next Steps

### **📚 Continue Learning**
- **[Schema Evolution Guide](schema-evolution.md)** - Master complex evolution patterns
- **[MCP Client Integration](../MCP-CLIENT-INTEGRATION.md)** - Optimize your IDE setup
- **[Multi-Registry Setup](multi-registry-setup.md)** - Manage dev/staging/prod environments

### **🎯 Try Advanced Scenarios**
- Register schemas in different contexts (ecommerce, fintech, iot)
- Set up cross-registry migration workflows
- Implement schema validation in CI/CD pipelines
- Configure role-based access with GitHub OAuth

### **🛠️ Production Readiness**
- **[Production Deployment](production-deployment.md)** - Deploy for real workloads
- **[Monitoring Setup](monitoring-setup.md)** - Implement observability
- **[Security Configuration](../architecture.md#security-architecture)** - Add authentication

### **🤝 Get Involved**
- ⭐ **Star the repositories** to help others discover this project
- 🐛 **Report issues** or suggest improvements  
- 📝 **Contribute** schemas, tutorials, or improvements
- 💬 **Join discussions** in GitHub Discussions

## 🎨 IDE-Specific Tips

### **Claude Desktop**
- Use conversational follow-ups: "Tell me more about that schema"
- Ask for explanations: "Why did you choose that field type?"
- Request iterations: "Make the schema more concise"

### **VS Code Copilot**
- Use `@agent` prefix for schema operations
- Integrate with Git workflows for schema versioning
- Generate code that uses your schemas directly

### **Cursor**
- Use agent mode for autonomous multi-step operations
- Try composer mode for collaborative schema design
- Leverage codebase context for schema relationships

### **JetBrains**
- Type `/` to see available MCP tools
- Use codebase mode for project-aware schema operations
- Integrate with debugging and profiling workflows

---

**🎊 Congratulations!** You've successfully completed the Developer Quick Start and experienced the power of AI-driven schema management in your preferred development environment. You can now use natural language to manage complex schema operations that traditionally required multiple API calls and deep Schema Registry knowledge.

**What you've learned:**
- ✅ Natural language schema operations via any MCP-compatible IDE
- ✅ Safe schema evolution with compatibility checking
- ✅ Multi-registry environment management
- ✅ Schema export and documentation workflows
- ✅ AI-assisted debugging and troubleshooting

[← Tutorials Overview](README.md) | [Schema Evolution →](schema-evolution.md) | [MCP Integration Guide →](../MCP-CLIENT-INTEGRATION.md)
