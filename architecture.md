# 🏗️ Architecture Overview

**Technical deep-dive into the AI-powered Kafka Schema Registry ecosystem**

This document provides a comprehensive technical overview of the architecture, design decisions, and implementation details behind the AI-powered schema management system.

## 🎯 System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              Claude Desktop                                 │
│                         (Natural Language Interface)                       │
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐               │
│  │   Developer     │ │    DevOps       │ │  Data Engineer  │               │
│  │   Workflows     │ │   Operations    │ │   Governance    │               │
│  └─────────────────┘ └─────────────────┘ └─────────────────┘               │
└─────────────────────────┬───────────────────────────────────────────────────┘
                          │ MCP Protocol (JSON-RPC over stdio)
┌─────────────────────────▼───────────────────────────────────────────────────┐
│                      MCP Server (Node.js/Python)                           │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                     Core MCP Engine                                 │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐    │   │
│  │  │    Tool     │ │  Resource   │ │    Auth     │ │    Task     │    │   │
│  │  │ Management  │ │ Management  │ │ Management  │ │ Management  │    │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                   Business Logic Layer                             │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐    │   │
│  │  │   Schema    │ │   Context   │ │   Config    │ │ Migration   │    │   │
│  │  │ Operations  │ │ Management  │ │ Management  │ │   Tools     │    │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘    │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐    │   │
│  │  │   Export    │ │ Statistics  │ │    Mode     │ │    Batch    │    │   │
│  │  │   System    │ │   & Audit   │ │  Control    │ │ Operations  │    │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                 Authentication & Authorization                      │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐    │   │
│  │  │   GitHub    │ │    OAuth    │ │    Scope    │ │    Token    │    │   │
│  │  │    OAuth    │ │    Flows    │ │ Enforcement │ │ Management  │    │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                   Registry Interface Layer                         │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐    │   │
│  │  │Multi-Registry│ │ Connection  │ │   Health    │ │    Load     │    │   │
│  │  │  Coordination│ │   Pooling   │ │ Monitoring  │ │ Balancing   │    │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────┬─────────────┬─────────────┬─────────────┬─────────────────────┘
              │             │             │             │
    ┌─────────▼─────────┐ ┌─▼───────────┐ ┌─▼───────────┐ ┌─▼───────────┐
    │   Development     │ │   Staging   │ │ Production  │ │   Archive   │
    │     Registry      │ │   Registry  │ │   Registry  │ │   Registry  │
    │    :8081         │ │    :8082    │ │    :8083    │ │    :8084    │
    │                  │ │             │ │             │ │             │
    │ ┌─────────────┐  │ │ ┌─────────┐ │ │ ┌─────────┐ │ │ ┌─────────┐ │
    │ │ ecommerce/  │  │ │ │staging/ │ │ │ │  prod/  │ │ │ │archive/ │ │
    │ │ ├─user-prof │  │ │ │├─user   │ │ │ │├─user   │ │ │ │├─legacy │ │
    │ │ ├─orders    │  │ │ │├─orders │ │ │ │├─orders │ │ │ │└─backup │ │
    │ │ └─products  │  │ │ │└─products│ │ │ │└─products│ │ │ └─────────┘ │
    │ └─────────────┘  │ │ └─────────┘ │ │ └─────────┘ │ │             │
    │ ┌─────────────┐  │ │ ┌─────────┐ │ │ ┌─────────┐ │ │             │
    │ │ fintech/    │  │ │ │fintech/ │ │ │ │fintech/ │ │ │             │
    │ │ ├─payments  │  │ │ │├─test   │ │ │ │├─payments│ │ │             │
    │ │ └─compliance│  │ │ │└─staging│ │ │ │└─compliance│ │ │             │
    │ └─────────────┘  │ │ └─────────┘ │ │ └─────────┘ │ │             │
    └──────────────────┘ └─────────────┘ └─────────────┘ └─────────────┘
         (Full Access)    (Limited Write)  (Read-Only)    (Backup Only)
```

## 🔧 Core Components

### **1. MCP Server Engine**

The heart of the system implementing the official MCP (Model Context Protocol) specification.

#### **MCP Protocol Implementation**
```python
# Core MCP server structure
class KafkaSchemaRegistryServer:
    def __init__(self):
        self.server = Server("kafka-schema-registry")
        self.registries = MultiRegistryManager()
        self.auth = AuthenticationManager()
        self.tasks = TaskManager()
        
    async def setup_mcp_handlers(self):
        # Tool handlers for schema operations
        @self.server.list_tools()
        async def handle_list_tools() -> list[Tool]:
            return [
                Tool(name="register_schema", description="..."),
                Tool(name="get_schema", description="..."),
                # ... 46 more tools
            ]
            
        # Resource handlers for status information  
        @self.server.list_resources()
        async def handle_list_resources() -> list[Resource]:
            return [
                Resource(uri="registry://status", name="Registry Status"),
                Resource(uri="registry://info", name="Registry Info")
            ]
```

#### **Multi-Registry Coordination**
```python
class MultiRegistryManager:
    def __init__(self):
        self.registries = {}
        self.default_registry = None
        self.readonly_modes = {}
        
    def add_registry(self, name: str, config: RegistryConfig):
        """Add a registry with specific configuration"""
        self.registries[name] = SchemaRegistryClient(config)
        
    async def execute_operation(self, operation: str, registry: str, **kwargs):
        """Execute operation with registry-specific rules"""
        if self.readonly_modes.get(registry, False):
            if operation in WRITE_OPERATIONS:
                raise PermissionError(f"Registry {registry} is in readonly mode")
        return await self.registries[registry].execute(operation, **kwargs)
```

### **2. Authentication & Authorization System**

#### **GitHub OAuth Integration**
```python
class GitHubOAuthProvider:
    def __init__(self, client_id: str, client_secret: str):
        self.client_id = client_id
        self.client_secret = client_secret
        self.oauth_client = GitHubOAuth(client_id, client_secret)
        
    async def authenticate(self, token: str) -> UserContext:
        """Authenticate user and extract permissions"""
        user_info = await self.oauth_client.get_user(token)
        permissions = await self.extract_permissions(user_info)
        return UserContext(user_info, permissions)
        
    async def extract_permissions(self, user_info: dict) -> set[str]:
        """Map GitHub scopes to MCP permissions"""
        scopes = user_info.get('scopes', [])
        permissions = set()
        
        if 'public_repo' in scopes:
            permissions.add('read')
        if 'repo' in scopes:
            permissions.add('read', 'write')
        if 'admin:org' in scopes:
            permissions.add('read', 'write', 'admin')
            
        return permissions
```

#### **Scope-Based Authorization**
```python
TOOL_PERMISSIONS = {
    # Read operations
    'list_subjects': {'read'},
    'get_schema': {'read'},
    'export_schema': {'read'},
    
    # Write operations  
    'register_schema': {'write'},
    'update_config': {'write'},
    
    # Admin operations
    'delete_subject': {'admin'},
    'migrate_schema': {'admin'},
}

def require_permissions(required_perms: set[str]):
    """Decorator to enforce permissions on MCP tools"""
    def decorator(func):
        async def wrapper(context: UserContext, *args, **kwargs):
            if not required_perms.issubset(context.permissions):
                raise PermissionError(f"Required permissions: {required_perms}")
            return await func(context, *args, **kwargs)
        return wrapper
    return decorator
```

### **3. Schema Context System**

#### **Context-Aware Operations**
```python
class ContextManager:
    def __init__(self, registry_client):
        self.client = registry_client
        
    async def create_context(self, context_name: str):
        """Create new schema context (logical namespace)"""
        # Context is a URL path prefix in Schema Registry
        await self.client.create_context(context_name)
        
    async def list_subjects_in_context(self, context: str) -> list[str]:
        """List all subjects within a specific context"""
        all_subjects = await self.client.list_subjects()
        context_prefix = f"{context}:"
        return [s for s in all_subjects if s.startswith(context_prefix)]
        
    async def register_schema_in_context(self, context: str, subject: str, schema: dict):
        """Register schema with context prefix"""
        full_subject = f"{context}:{subject}" if context else subject
        return await self.client.register_schema(full_subject, schema)
```

#### **Context Isolation Patterns**
```python
# Environment-based contexts
ENVIRONMENT_CONTEXTS = {
    'development': 'dev',
    'staging': 'staging', 
    'production': 'prod'
}

# Team-based contexts
TEAM_CONTEXTS = {
    'ecommerce-team': 'ecommerce',
    'payments-team': 'fintech',
    'analytics-team': 'analytics'
}

# Multi-tenant contexts  
TENANT_CONTEXTS = {
    'customer-a': 'tenant-a',
    'customer-b': 'tenant-b'
}
```

### **4. Schema Export System**

#### **Multi-Format Export Engine**
```python
class ExportEngine:
    def __init__(self):
        self.exporters = {
            'json': JSONExporter(),
            'avro_idl': AvroIDLExporter(),
            'confluence': ConfluenceExporter(),
            'zip': ZipExporter()
        }
        
    async def export_schema(self, subject: str, version: str, format: str) -> ExportResult:
        """Export single schema in specified format"""
        schema = await self.get_schema(subject, version)
        exporter = self.exporters[format]
        return await exporter.export(schema)
        
    async def export_context(self, context: str, format: str) -> ExportResult:
        """Export all schemas in a context"""
        subjects = await self.list_subjects_in_context(context)
        exports = []
        
        for subject in subjects:
            versions = await self.get_schema_versions(subject)
            for version in versions:
                export = await self.export_schema(subject, version, format)
                exports.append(export)
                
        return self.combine_exports(exports, format)
```

### **5. Migration System**

#### **Cross-Registry Migration**
```python
class MigrationEngine:
    def __init__(self, source_registry, target_registry):
        self.source = source_registry
        self.target = target_registry
        
    async def migrate_schema(self, subject: str, preserve_ids: bool = True) -> MigrationResult:
        """Migrate schema between registries"""
        # Get source schema with metadata
        source_schema = await self.source.get_schema_with_metadata(subject)
        
        # Check compatibility in target
        compatibility = await self.target.check_compatibility(subject, source_schema.schema)
        
        if not compatibility.is_compatible:
            raise MigrationError(f"Schema not compatible: {compatibility.messages}")
            
        # Perform migration with ID preservation
        if preserve_ids:
            await self.target.set_mode("IMPORT")
            
        result = await self.target.register_schema(
            subject, 
            source_schema.schema,
            schema_id=source_schema.id if preserve_ids else None
        )
        
        if preserve_ids:
            await self.target.set_mode("READWRITE")
            
        return MigrationResult(success=True, new_id=result.id)
```

## 🔒 Security Architecture

### **Authentication Flow**
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│    User     │    │   Claude    │    │ MCP Server  │    │   GitHub    │
│             │    │  Desktop    │    │             │    │    OAuth    │
└─────┬───────┘    └─────┬───────┘    └─────┬───────┘    └─────┬───────┘
      │                  │                  │                  │
      │ 1. Natural       │                  │                  │
      │    Language      │                  │                  │
      │ ──────────────── │                  │                  │
      │                  │ 2. MCP Tool      │                  │
      │                  │    Request       │                  │
      │                  │ ──────────────── │                  │
      │                  │                  │ 3. Auth Check    │
      │                  │                  │ ──────────────── │
      │                  │                  │                  │
      │                  │                  │ 4. Token Valid   │
      │                  │                  │ ←──────────────── │
      │                  │ 5. Tool Response │                  │
      │                  │ ←──────────────── │                  │
      │ 6. AI Response   │                  │                  │
      │ ←──────────────── │                  │                  │
```

### **Permission Enforcement Matrix**

| Operation | Public Repo | Private Repo | Org Admin | Registry Mode |
|-----------|-------------|--------------|-----------|---------------|
| **Read Operations** |
| `list_subjects` | ✅ | ✅ | ✅ | All |
| `get_schema` | ✅ | ✅ | ✅ | All |
| `export_schema` | ✅ | ✅ | ✅ | All |
| **Write Operations** |
| `register_schema` | ❌ | ✅ | ✅ | ReadWrite |
| `update_config` | ❌ | ✅ | ✅ | ReadWrite |
| **Admin Operations** |
| `delete_subject` | ❌ | ❌ | ✅ | ReadWrite |
| `migrate_schema` | ❌ | ❌ | ✅ | ReadWrite |

### **Registry Security Modes**

```python
class RegistrySecurityMode:
    READWRITE = "READWRITE"  # Full operations allowed
    READONLY = "READONLY"    # Only read operations  
    IMPORT = "IMPORT"        # Special mode for migrations
    
READONLY_OPERATIONS = {
    'list_subjects', 'get_schema', 'get_schema_versions',
    'check_compatibility', 'export_schema', 'get_config'
}

WRITE_OPERATIONS = {
    'register_schema', 'delete_subject', 'update_config',
    'create_context', 'delete_context'
}

ADMIN_OPERATIONS = {
    'delete_subject', 'migrate_schema', 'clear_context_batch'
}
```

## 📊 Performance & Scalability

### **Connection Management**
```python
class RegistryConnectionPool:
    def __init__(self, registry_config: RegistryConfig):
        self.pool = aiohttp.ClientSession(
            connector=aiohttp.TCPConnector(
                limit=100,  # Total connection pool size
                limit_per_host=20,  # Per-registry limit
                keepalive_timeout=300
            )
        )
        
    async def execute_request(self, method: str, url: str, **kwargs):
        """Execute request with retry and circuit breaking"""
        for attempt in range(3):
            try:
                async with self.pool.request(method, url, **kwargs) as resp:
                    return await resp.json()
            except aiohttp.ClientError as e:
                if attempt == 2:
                    raise RegistryConnectionError(f"Failed after 3 attempts: {e}")
                await asyncio.sleep(2 ** attempt)  # Exponential backoff
```

### **Async Task Management**
```python
class TaskManager:
    def __init__(self):
        self.executor = ThreadPoolExecutor(max_workers=10)
        self.active_tasks = {}
        
    async def submit_task(self, task_id: str, coro) -> TaskResult:
        """Submit long-running task for async execution"""
        task = asyncio.create_task(coro)
        self.active_tasks[task_id] = {
            'task': task,
            'status': 'running',
            'progress': 0,
            'created_at': datetime.utcnow()
        }
        
        # Monitor task completion
        task.add_done_callback(lambda t: self._task_completed(task_id, t))
        return TaskResult(task_id=task_id, status='submitted')
```

### **Caching Strategy**
```python
class SchemaCache:
    def __init__(self):
        self.schema_cache = TTLCache(maxsize=1000, ttl=300)  # 5 min TTL
        self.subject_cache = TTLCache(maxsize=100, ttl=60)   # 1 min TTL
        
    @cached(cache=schema_cache)
    async def get_schema(self, registry: str, subject: str, version: str) -> Schema:
        """Cached schema retrieval"""
        return await self.registries[registry].get_schema(subject, version)
        
    @cached(cache=subject_cache)
    async def list_subjects(self, registry: str, context: str = None) -> list[str]:
        """Cached subject listing"""
        return await self.registries[registry].list_subjects(context)
```

## 🔧 Development & Deployment

### **Development Environment**
```yaml
# docker-compose.dev.yml
version: '3.8'
services:
  mcp-server-dev:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - ./src:/app/src:ro
      - ./config:/app/config:ro
    environment:
      - DEBUG=true
      - LOG_LEVEL=debug
      - HOT_RELOAD=true
    ports:
      - "38000:8000"
      - "9229:9229"  # Debug port
      
  dev-registry:
    image: confluentinc/cp-schema-registry:7.4.0
    environment:
      SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: kafka:9092
      SCHEMA_REGISTRY_HOST_NAME: dev-registry
      SCHEMA_REGISTRY_LISTENERS: http://0.0.0.0:8081
    ports:
      - "8081:8081"
```

### **Production Deployment**
```yaml
# docker-compose.prod.yml  
version: '3.8'
services:
  mcp-server:
    image: aywengo/kafka-schema-reg-mcp:stable
    restart: unless-stopped
    environment:
      - ENABLE_AUTH=true
      - GITHUB_CLIENT_ID=${GITHUB_CLIENT_ID}
      - GITHUB_CLIENT_SECRET=${GITHUB_CLIENT_SECRET}
      - READONLY_3=true  # Production safety
    networks:
      - schema-registry-network
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      
  nginx:
    image: nginx:alpine
    ports:
      - "443:443"
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/ssl
    depends_on:
      - mcp-server
```

### **Kubernetes Deployment**
```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kafka-schema-mcp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kafka-schema-mcp
  template:
    metadata:
      labels:
        app: kafka-schema-mcp
    spec:
      containers:
      - name: mcp-server
        image: aywengo/kafka-schema-reg-mcp:stable
        ports:
        - containerPort: 8000
        env:
        - name: GITHUB_CLIENT_ID
          valueFrom:
            secretKeyRef:
              name: github-oauth
              key: client-id
        - name: GITHUB_CLIENT_SECRET
          valueFrom:
            secretKeyRef:
              name: github-oauth  
              key: client-secret
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 5
```

## 📈 Monitoring & Observability

### **Metrics Collection**
```python
from prometheus_client import Counter, Histogram, Gauge

# Define metrics
schema_operations = Counter('schema_operations_total', 
                           'Total schema operations', 
                           ['operation', 'registry', 'context'])

operation_duration = Histogram('schema_operation_duration_seconds',
                              'Duration of schema operations',
                              ['operation', 'registry'])

active_connections = Gauge('registry_connections_active',
                          'Active registry connections',
                          ['registry'])

class MetricsMiddleware:
    async def track_operation(self, operation: str, registry: str, context: str):
        """Track operation metrics"""
        start_time = time.time()
        try:
            result = await self.execute_operation(operation, registry, context)
            schema_operations.labels(
                operation=operation, 
                registry=registry, 
                context=context
            ).inc()
            return result
        finally:
            duration = time.time() - start_time
            operation_duration.labels(
                operation=operation,
                registry=registry
            ).observe(duration)
```

### **Health Checks**
```python
class HealthChecker:
    async def check_health(self) -> HealthStatus:
        """Comprehensive health check"""
        checks = {
            'registries': await self.check_registries(),
            'auth': await self.check_auth_service(),
            'database': await self.check_database(),
            'tasks': await self.check_task_queue()
        }
        
        overall_status = 'healthy' if all(
            check['status'] == 'healthy' for check in checks.values()
        ) else 'unhealthy'
        
        return HealthStatus(
            status=overall_status,
            checks=checks,
            timestamp=datetime.utcnow()
        )
        
    async def check_registries(self) -> dict:
        """Check all registry connections"""
        results = {}
        for name, registry in self.registries.items():
            try:
                await registry.list_subjects()
                results[name] = {'status': 'healthy', 'latency_ms': 0}
            except Exception as e:
                results[name] = {'status': 'unhealthy', 'error': str(e)}
        return results
```

### **Logging Strategy**
```python
import structlog

logger = structlog.get_logger()

class StructuredLogger:
    def log_operation(self, operation: str, context: dict):
        """Structured logging for operations"""
        logger.info(
            "schema_operation",
            operation=operation,
            registry=context.get('registry'),
            context=context.get('context'),
            user_id=context.get('user_id'),
            duration_ms=context.get('duration_ms'),
            success=context.get('success', True)
        )
        
    def log_auth_event(self, event: str, user_context: UserContext):
        """Log authentication events"""
        logger.info(
            "auth_event",
            event=event,
            user_id=user_context.user_id,
            permissions=list(user_context.permissions),
            github_login=user_context.github_login,
            timestamp=datetime.utcnow().isoformat()
        )
```

## 🚀 Advanced Features

### **Schema Evolution Analysis**
```python
class EvolutionAnalyzer:
    def __init__(self):
        self.compatibility_checker = CompatibilityChecker()
        
    async def analyze_evolution_path(self, subject: str, target_schema: dict) -> EvolutionAnalysis:
        """Analyze schema evolution compatibility"""
        versions = await self.get_schema_versions(subject)
        analysis = EvolutionAnalysis()
        
        for version in versions:
            current_schema = await self.get_schema(subject, version)
            compatibility = await self.compatibility_checker.check(
                current_schema, target_schema
            )
            analysis.add_compatibility_result(version, compatibility)
            
        return analysis
        
    def suggest_evolution_strategy(self, analysis: EvolutionAnalysis) -> list[EvolutionStep]:
        """Suggest safe evolution steps"""
        if analysis.is_backward_compatible():
            return [EvolutionStep.DIRECT_UPDATE]
        elif analysis.has_intermediate_compatibility():
            return self.plan_gradual_evolution(analysis)
        else:
            return [EvolutionStep.BREAKING_CHANGE_REQUIRED]
```

### **Intelligent Schema Suggestions**
```python
class SchemaSuggestionEngine:
    def __init__(self):
        self.field_patterns = self.load_field_patterns()
        self.naming_conventions = self.load_naming_conventions()
        
    async def suggest_schema_improvements(self, schema: dict) -> list[Suggestion]:
        """AI-powered schema improvement suggestions"""
        suggestions = []
        
        # Check naming conventions
        for field in schema.get('fields', []):
            if not self.naming_conventions.is_valid(field['name']):
                suggestions.append(
                    Suggestion(
                        type='naming',
                        field=field['name'],
                        suggestion=self.naming_conventions.suggest_name(field['name']),
                        reason='Follows standard naming conventions'
                    )
                )
                
        # Suggest missing common fields
        missing_fields = self.detect_missing_common_fields(schema)
        for field in missing_fields:
            suggestions.append(
                Suggestion(
                    type='field',
                    suggestion=field,
                    reason=f'Commonly used in {schema.get("namespace", "similar")} schemas'
                )
            )
            
        return suggestions
```

---

This architecture provides the foundation for AI-powered schema management that is:

- **🤖 AI-First**: Natural language interface through Claude Desktop
- **🏢 Enterprise-Ready**: Multi-registry, context isolation, security
- **📈 Scalable**: Async operations, connection pooling, caching
- **🔒 Secure**: OAuth integration, role-based permissions
- **🔧 Maintainable**: Modular design, comprehensive monitoring

[← Getting Started](getting-started.md) | [Use Cases →](use-cases.md) | [Back to Main](README.md)
