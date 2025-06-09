# 🚀 Getting Started with AI-Powered Schema Management

**From zero to AI-powered schema management in under 10 minutes**

This guide walks you through setting up the complete demo environment and integrating it with Claude Desktop for natural language schema operations.

## 📋 Prerequisites

Before you begin, ensure you have:

- **Docker** and **Docker Compose** installed
- **Claude Desktop** installed ([download here](https://claude.ai/chat))
- **GitHub account** (for OAuth authentication)
- **6GB+ RAM** available
- **Ports available**: 3000, 8081-8083, 9090-9092, 38000

## 🎯 Quick Start Options

Choose your path based on your needs:

### **🚀 Option 1: Full Demo Experience (Recommended)**
*Complete environment with GitHub OAuth, multi-registry setup, and realistic schemas*

### **⚡ Option 2: Connect to Existing Registry**
*Use the MCP server with your existing Kafka Schema Registry*

### **🛠️ Option 3: Development Setup**
*Local development environment for customization*

---

## 🚀 Option 1: Full Demo Experience

### Step 1: Create GitHub OAuth App

1. **Go to GitHub Settings**
   - Navigate to **Settings** → **Developer settings** → **OAuth Apps**
   - Click **"New OAuth App"**

2. **Configure OAuth App**
   ```
   Application name: Kafka Schema Registry MCP Demo
   Homepage URL: http://localhost:3000
   Authorization callback URL: http://localhost:38000/auth/callback
   ```

3. **Save Credentials**
   - Copy the **Client ID** and **Client Secret**
   - You'll need these for the environment configuration

### Step 2: Clone and Configure Demo Environment

```bash
# Clone the demo deployment repository
git clone https://github.com/aywengo/demo-deployment.git
cd demo-deployment

# Create environment configuration
cp .env.github-oauth .env

# Edit with your GitHub OAuth credentials
vim .env  # Add your GITHUB_CLIENT_ID and GITHUB_CLIENT_SECRET
```

**Environment Configuration Example:**
```bash
# .env file
GITHUB_CLIENT_ID=your_github_client_id_here
GITHUB_CLIENT_SECRET=your_github_client_secret_here
GITHUB_ORG=your_organization_name  # Optional
```

### Step 3: Start the Complete Demo Environment

```bash
# Start all services (this may take 2-3 minutes)
docker-compose -f docker-compose.github-oauth.yml up -d

# Verify all services are running
docker-compose ps
```

**Expected Services:**
```
SERVICE              STATUS
dev-registry         ✅ Up (port 8081)
staging-registry     ✅ Up (port 8082)  
production-registry  ✅ Up (port 8083)
mcp-server          ✅ Up (port 38000)
demo-ui             ✅ Up (port 3000)
monitoring          ✅ Up (port 9090)
```

### Step 4: Load Demo Schemas

```bash
# Populate registries with realistic business schemas
./scripts/setup-demo-data.sh

# Verify schemas are loaded
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
  http://localhost:38000/subjects?registry=development
```

### Step 5: Configure Claude Desktop

```bash
# Copy the demo configuration
cp config-examples/claude_desktop_demo.json \
   ~/Library/Application\ Support/Claude/claude_desktop_config.json

# For Linux users:
cp config-examples/claude_desktop_demo.json \
   ~/.config/claude-desktop/config.json
```

**Configuration File Content:**
```json
{
  "mcpServers": {
    "kafka-schema-registry-demo": {
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
        "-e", "SCHEMA_REGISTRY_NAME_2=staging",
        "-e", "SCHEMA_REGISTRY_URL_2=http://staging-registry:8082",
        "-e", "SCHEMA_REGISTRY_NAME_3=production", 
        "-e", "SCHEMA_REGISTRY_URL_3=http://prod-registry:8083",
        "-e", "READONLY_3=true",
        "aywengo/kafka-schema-reg-mcp:stable"
      ],
      "env": {
        "GITHUB_CLIENT_ID": "your_github_client_id",
        "GITHUB_CLIENT_SECRET": "your_github_client_secret"
      }
    }
  }
}
```

### Step 6: Test the Integration

1. **Restart Claude Desktop**
2. **Look for the 🔨 tools icon** in the interface
3. **Try these commands:**

```
"List all schema contexts"
"Show me subjects in the development registry"
"Register a test schema with name and email fields"
"Check what registries are available"
```

**🎉 Success!** You now have AI-powered schema management with Claude Desktop.

---

## ⚡ Option 2: Connect to Existing Registry

If you already have a Kafka Schema Registry running:

### Step 1: Start MCP Server

```bash
# Using Docker (recommended)
docker run -p 38000:8000 \
  -e SCHEMA_REGISTRY_URL=http://your-registry:8081 \
  -e SCHEMA_REGISTRY_USER=your_username \
  -e SCHEMA_REGISTRY_PASSWORD=your_password \
  aywengo/kafka-schema-reg-mcp:stable

# Or using Python directly
git clone https://github.com/aywengo/kafka-schema-reg-mcp.git
cd kafka-schema-reg-mcp
pip install -r requirements.txt
export SCHEMA_REGISTRY_URL=http://your-registry:8081
python kafka_schema_registry_unified_mcp.py
```

### Step 2: Configure Claude Desktop

```json
{
  "mcpServers": {
    "kafka-schema-registry": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "-e", "SCHEMA_REGISTRY_URL=http://your-registry:8081",
        "-e", "SCHEMA_REGISTRY_USER=your_username",
        "-e", "SCHEMA_REGISTRY_PASSWORD=your_password",
        "aywengo/kafka-schema-reg-mcp:stable"
      ]
    }
  }
}
```

### Step 3: Test Connection

```
"Test connection to my schema registry"
"List all available subjects"
```

---

## 🛠️ Option 3: Development Setup

For customization and development:

### Step 1: Clone All Repositories

```bash
# Create workspace directory
mkdir kafka-schema-mcp-workspace
cd kafka-schema-mcp-workspace

# Clone all components
git clone https://github.com/aywengo/kafka-schema-reg-mcp.git
git clone https://github.com/aywengo/demo-deployment.git
git clone https://github.com/aywengo/demo-schemas.git
git clone https://github.com/aywengo/demo-docs.git
```

### Step 2: Start Development Environment

```bash
# Start registries only
cd demo-deployment
docker-compose up -d dev-registry staging-registry production-registry

# Run MCP server locally
cd ../kafka-schema-reg-mcp
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
export SCHEMA_REGISTRY_URL=http://localhost:8081
python kafka_schema_registry_unified_mcp.py
```

### Step 3: Configure Claude Desktop for Local Development

```json
{
  "mcpServers": {
    "kafka-schema-registry-dev": {
      "command": "python",
      "args": ["/path/to/kafka-schema-reg-mcp/kafka_schema_registry_unified_mcp.py"],
      "env": {
        "SCHEMA_REGISTRY_URL": "http://localhost:8081"
      }
    }
  }
}
```

---

## 🎯 Your First AI Schema Operations

Once everything is set up, try these progressive examples:

### **🔰 Beginner: Basic Operations**

```
"Show me all available schema registries"
"List subjects in the development registry" 
"Get the latest version of the user-profile schema"
```

### **⚡ Intermediate: Schema Management**

```
"Register a new product schema with id, name, price, and category fields"
"Check if adding an optional description field would be backward compatible"
"Export the user-profile schema in JSON format"
```

### **🚀 Advanced: Multi-Registry Operations**

```
"Compare the development and staging registries"
"Migrate the order-events schema from staging to production"
"Show me evolution history for all schemas in the ecommerce context"
```

### **🏢 Enterprise: Governance & Compliance**

```
"Export all schemas from the fintech context for compliance audit"
"Set the compatibility level for user-events to BACKWARD_TRANSITIVE"
"Generate a report of all schema changes in the last month"
```

---

## 🔍 Verification & Testing

### Check Service Health

```bash
# Verify all services are running
docker-compose ps

# Check MCP server health  
curl http://localhost:38000/health

# Test registry connectivity
curl http://localhost:8081/subjects
```

### Test GitHub OAuth Flow

```bash
# Get your GitHub token (for testing)
# Go to GitHub Settings → Developer settings → Personal access tokens
export GITHUB_TOKEN="ghp_your_token_here"

# Test authenticated access
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
  http://localhost:38000/subjects

# Check your permissions
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
  http://localhost:38000/auth/user
```

### Verify Claude Desktop Integration

1. **🔨 Tools Icon**: Should appear in Claude Desktop interface
2. **MCP Status**: Try "What MCP tools are available?"
3. **Schema Operations**: Try "List all schema contexts"

---

## 🚨 Troubleshooting

### Common Issues and Solutions

#### **Claude Desktop Not Showing Tools**

```bash
# Check configuration file location
# macOS: ~/Library/Application Support/Claude/claude_desktop_config.json
# Linux: ~/.config/claude-desktop/config.json

# Validate JSON syntax
python -m json.tool claude_desktop_config.json

# Restart Claude Desktop completely
```

#### **Docker Services Not Starting**

```bash
# Check port conflicts
netstat -tulpn | grep :8081

# Free up memory
docker system prune

# Check logs
docker-compose logs mcp-server
```

#### **GitHub OAuth Not Working**

```bash
# Verify callback URL in GitHub OAuth app
# Should be: http://localhost:38000/auth/callback

# Check environment variables
docker-compose exec mcp-server env | grep GITHUB

# Test token generation
curl -X POST http://localhost:38000/auth/login
```

#### **Schema Operations Failing**

```bash
# Check registry connectivity
curl http://localhost:8081/subjects

# Verify MCP server logs
docker-compose logs mcp-server

# Test authentication
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
  http://localhost:38000/test-auth
```

### **🆘 Getting Help**

- **📖 Documentation**: [demo-docs](https://github.com/aywengo/demo-docs)
- **🐛 Issues**: [GitHub Issues](https://github.com/aywengo/kafka-schema-reg-mcp/issues)
- **💬 Discussions**: [GitHub Discussions](https://github.com/aywengo/kafka-schema-reg-mcp/discussions)

---

## 🎉 Next Steps

### **🎓 Learn More**
- **[Architecture Overview](architecture.md)** - Understand the technical design
- **[Use Cases](use-cases.md)** - Explore real-world scenarios  
- **[Tutorials](tutorials/)** - Role-specific guides

### **🚀 Advanced Setup**
- **[Production Deployment](tutorials/production-deployment.md)**
- **[Kubernetes Setup](tutorials/kubernetes-deployment.md)**
- **[CI/CD Integration](tutorials/cicd-integration.md)**

### **🤝 Get Involved**
- **⭐ Star the repositories** to help others discover this project
- **🐛 Report issues** or suggest improvements
- **📝 Contribute** to documentation or code

---

**🎊 Congratulations!** You now have AI-powered schema management running with Claude Desktop. Start exploring natural language schema operations and see how it transforms your workflow.

[← Back to Main](README.md) | [Architecture →](architecture.md) | [Use Cases →](use-cases.md)
