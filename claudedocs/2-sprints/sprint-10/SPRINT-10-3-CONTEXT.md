# SPRINT 10 - 技術上下文 (Technical Context)

---

## 📋 文件資訊 (Document Information)

| 欄位 | 內容 |
|------|------|
| **版本** | v2.1 |
| **Sprint 編號** | Sprint 10 |
| **Sprint 週期** | Week 28-30 (3 週) |
| **Phase** | Phase 1C - 工作流編輯器 Backend (Workflow Editor Backend) |
| **計劃日期** | 2026-04-21 ~ 2026-05-11 |
| **狀態** | 📋 計劃階段 (Planned) |
| **創建日期** | 2025-11-14 |
| **最後更新** | 2025-11-14 |

---

## 📑 目錄 (Table of Contents)

1. [技術上下文說明](#技術上下文說明)
2. [Phase 1C 啟動技術準備](#phase-1c-啟動技術準備)
3. [Workflow Definition CRUD 技術上下文](#workflow-definition-crud-技術上下文)
4. [Node Registry System 技術上下文](#node-registry-system-技術上下文)
5. [Edge Validation Engine 技術上下文](#edge-validation-engine-技術上下文)
6. [技術決策背景](#技術決策背景)
7. [架構整合考量](#架構整合考量)
8. [參考文檔列表](#參考文檔列表)

---

## 技術上下文說明

### 文檔目的

本文檔為 Sprint 10 提供詳細的技術上下文,幫助開發團隊理解:
- **為什麼要這樣設計** - 設計決策的理由和背景
- **技術挑戰** - 實施過程中可能遇到的技術難點
- **最佳實踐** - 參考的業界標準和成功案例
- **架構整合** - 如何與現有系統整合

### 使用指南

**開發前閱讀**:
- 理解設計決策背景
- 識別技術挑戰
- 了解架構整合點

**開發中參考**:
- 查找具體技術實現細節
- 解決技術問題
- 驗證設計決策

**Review 時檢查**:
- 確認實現符合設計意圖
- 驗證架構整合正確
- 檢查最佳實踐遵循

---

## Phase 1C 啟動技術準備

### Phase 1B 完成技術總結

**已交付的技術基礎設施**:

```yaml
Sprint 7-9 技術成果:
  Framework Abstraction Layer:
    ✅ 5 個核心介面設計 (IWorkflowEngine, ITaskGenerator, etc.)
    ✅ SemanticKernelAdapter 完整實作
    ✅ 可替換性驗證機制
    ✅ Feature Flags 動態切換
    ✅ 性能基準測試 (開銷 <5%)

  Workflow Execution Engine:
    ✅ 7 種執行狀態管理
    ✅ 錯誤處理與重試機制
    ✅ 執行日誌記錄
    ✅ Agent 狀態查詢

  Multi-Agent Coordination:
    ✅ Sequential Coordination
    ✅ Parallel Coordination
    ✅ Race condition 處理
    ✅ Result aggregation

  Agent Messaging:
    ✅ Message 傳遞機制
    ✅ Message 路由與分發
    ✅ Message 持久化

  Task Generator:
    ✅ LLM 驅動任務規劃
    ✅ 任務依賴分析
    ✅ 執行計劃生成

參考文檔:
  - claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md (Phase 1B 回顧)
  - docs/architecture/ADR-011-framework-abstraction-layer.md
```

### Phase 1C 技術目標與範疇

**Sprint 10-11 技術交付**:

```yaml
Sprint 10 (本 Sprint):
  Workflow Definition Management:
    🎯 WorkflowDefinition 實體與 Repository
    🎯 Workflow CRUD API (RESTful)
    🎯 Version 欄位設計
    🎯 Soft delete 機制

  Node Registry:
    🎯 Node Type 註冊機制
    🎯 JSON Schema validation
    🎯 Node Compatibility Matrix
    🎯 Dynamic Node registration

  Edge Validation:
    🎯 Edge connection rules
    🎯 Circular dependency detection (DFS)
    🎯 Data flow validation
    🎯 Error message generation

Sprint 11 (下一個):
  Workflow Validation:
    ⏳ Complete validation API
    ⏳ Schema versioning
    ⏳ Migration strategies
    ⏳ Performance optimization

參考文檔:
  - docs/api/workflow-api-design.md
  - docs/database/workflow-schema.md
```

### Phase 1C → Phase 1D 技術銜接

**為 Phase 1D (Frontend) 準備的 API**:

```yaml
技術要求:
  API 穩定性:
    - RESTful 設計原則
    - API 版本化 (v1)
    - 向後兼容保證

  API 文檔:
    - Swagger/OpenAPI 3.0 完整文檔
    - 請求/響應範例
    - 錯誤代碼說明

  錯誤處理:
    - 標準化錯誤格式
    - HTTP 狀態碼規範
    - 詳細錯誤訊息

  性能基準:
    - API 響應時間 P95 <200ms
    - Validation 延遲 <500ms
    - 支援 >1000 nodes workflow

參考文檔:
  - docs/architecture/ADR-012-workflow-editor-technology.md
  - docs/api/api-documentation-standards.md
```

---

## Workflow Definition CRUD 技術上下文

### 設計背景

**業務需求**:
- IT 開發者需要透過 API 創建和管理 Workflow Definition
- 支援 Workflow 版本控制和重用
- 為視覺化編輯器提供 Backend 支持

**技術挑戰**:
1. **靈活的存儲格式**: Workflow definition 需要支援不同結構
2. **版本控制**: 如何管理 Workflow 的多個版本
3. **Soft Delete**: 避免誤刪,支援恢復
4. **性能優化**: 大型 Workflow (>100 nodes) 的查詢效率

### Domain Model 設計

**WorkflowDefinition 實體**:

```csharp
// Domain/Entities/Workflows/WorkflowDefinition.cs
public class WorkflowDefinition : BaseEntity, IAggregateRoot
{
    // Identity
    public Guid Id { get; private set; }

    // Metadata
    public string Name { get; private set; }
    public string? Description { get; private set; }
    public int Version { get; private set; }
    public WorkflowStatus Status { get; private set; }

    // Content
    public string DefinitionJson { get; private set; }

    // Navigation properties
    private readonly List<NodeDefinition> _nodes = new();
    public IReadOnlyCollection<NodeDefinition> Nodes => _nodes.AsReadOnly();

    private readonly List<EdgeDefinition> _edges = new();
    public IReadOnlyCollection<EdgeDefinition> Edges => _edges.AsReadOnly();

    // Audit fields
    public DateTime CreatedAt { get; private set; }
    public DateTime? UpdatedAt { get; private set; }
    public bool IsDeleted { get; private set; }

    // Factory method
    public static WorkflowDefinition Create(
        string name,
        string description,
        string definitionJson)
    {
        var workflow = new WorkflowDefinition
        {
            Id = Guid.NewGuid(),
            Name = name,
            Description = description,
            DefinitionJson = definitionJson,
            Version = 1,
            Status = WorkflowStatus.Draft,
            CreatedAt = DateTime.UtcNow
        };

        // Raise domain event
        workflow.AddDomainEvent(new WorkflowDefinitionCreatedEvent(workflow.Id, name));

        return workflow;
    }

    // Business logic
    public void Update(string name, string description, string definitionJson)
    {
        Name = name;
        Description = description;
        DefinitionJson = definitionJson;
        UpdatedAt = DateTime.UtcNow;

        AddDomainEvent(new WorkflowDefinitionUpdatedEvent(Id, name));
    }

    public void Activate()
    {
        if (Status == WorkflowStatus.Draft)
        {
            Status = WorkflowStatus.Active;
            UpdatedAt = DateTime.UtcNow;
            AddDomainEvent(new WorkflowDefinitionActivatedEvent(Id));
        }
    }

    public void Archive()
    {
        Status = WorkflowStatus.Archived;
        UpdatedAt = DateTime.UtcNow;
        AddDomainEvent(new WorkflowDefinitionArchivedEvent(Id));
    }

    public void SoftDelete()
    {
        IsDeleted = true;
        UpdatedAt = DateTime.UtcNow;
        AddDomainEvent(new WorkflowDefinitionDeletedEvent(Id));
    }

    public void AddNode(NodeDefinition node)
    {
        _nodes.Add(node);
        UpdatedAt = DateTime.UtcNow;
    }

    public void AddEdge(EdgeDefinition edge)
    {
        _edges.Add(edge);
        UpdatedAt = DateTime.UtcNow;
    }
}

public enum WorkflowStatus
{
    Draft = 0,      // 草稿,可編輯
    Active = 1,     // 啟用,可執行
    Archived = 2    // 封存,只讀
}
```

**設計決策**:

1. **Aggregate Root Pattern**:
   - WorkflowDefinition 是 Aggregate Root
   - Nodes 和 Edges 是 Value Objects
   - 透過 WorkflowDefinition 統一管理

2. **Encapsulation**:
   - Private setters 保護 Domain invariants
   - Factory method 確保創建邏輯一致
   - Business methods 封裝業務規則

3. **Domain Events**:
   - 所有狀態變更觸發 Domain Events
   - 支援 Event Sourcing 和 Audit Trail
   - 便於整合其他 Bounded Context

4. **Soft Delete**:
   - IsDeleted 欄位標記刪除
   - 支援恢復功能
   - 保留 Audit Trail

### NodeDefinition 與 EdgeDefinition

**NodeDefinition 設計**:

```csharp
public class NodeDefinition : ValueObject
{
    public Guid Id { get; private set; }
    public Guid WorkflowDefinitionId { get; private set; }
    public string NodeId { get; private set; } // Unique within workflow
    public string NodeType { get; private set; } // Agent, Tool, Condition, etc.
    public string ConfigurationJson { get; private set; }
    public Position Position { get; private set; }

    // Navigation property
    public WorkflowDefinition WorkflowDefinition { get; private set; }

    public static NodeDefinition Create(
        string nodeId,
        string nodeType,
        string configurationJson,
        Position position)
    {
        // Validate node type exists in Node Registry
        // Validate configuration matches node type schema

        return new NodeDefinition
        {
            Id = Guid.NewGuid(),
            NodeId = nodeId,
            NodeType = nodeType,
            ConfigurationJson = configurationJson,
            Position = position
        };
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return NodeId;
        yield return NodeType;
        yield return ConfigurationJson;
    }
}

public record Position(int X, int Y);
```

**EdgeDefinition 設計**:

```csharp
public class EdgeDefinition : ValueObject
{
    public Guid Id { get; private set; }
    public Guid WorkflowDefinitionId { get; private set; }
    public string SourceNodeId { get; private set; }
    public string TargetNodeId { get; private set; }
    public string? Condition { get; private set; } // Optional condition expression

    // Navigation property
    public WorkflowDefinition WorkflowDefinition { get; private set; }

    public static EdgeDefinition Create(
        string sourceNodeId,
        string targetNodeId,
        string? condition = null)
    {
        // Validate source and target nodes exist
        // Validate condition syntax if provided

        return new EdgeDefinition
        {
            Id = Guid.NewGuid(),
            SourceNodeId = sourceNodeId,
            TargetNodeId = targetNodeId,
            Condition = condition
        };
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return SourceNodeId;
        yield return TargetNodeId;
        yield return Condition ?? string.Empty;
    }
}
```

### Database Schema

**PostgreSQL Tables**:

```sql
-- WorkflowDefinitions table
CREATE TABLE "WorkflowDefinitions" (
    "Id" uuid PRIMARY KEY,
    "Name" varchar(200) NOT NULL,
    "Description" varchar(1000),
    "Version" integer NOT NULL DEFAULT 1,
    "Status" integer NOT NULL DEFAULT 0,
    "DefinitionJson" jsonb NOT NULL,
    "CreatedAt" timestamp with time zone NOT NULL,
    "UpdatedAt" timestamp with time zone,
    "IsDeleted" boolean NOT NULL DEFAULT false
);

-- NodeDefinitions table
CREATE TABLE "NodeDefinitions" (
    "Id" uuid PRIMARY KEY,
    "WorkflowDefinitionId" uuid NOT NULL REFERENCES "WorkflowDefinitions"("Id") ON DELETE CASCADE,
    "NodeId" varchar(100) NOT NULL,
    "NodeType" varchar(50) NOT NULL,
    "ConfigurationJson" jsonb NOT NULL,
    "PositionX" integer NOT NULL,
    "PositionY" integer NOT NULL,
    UNIQUE ("WorkflowDefinitionId", "NodeId")
);

-- EdgeDefinitions table
CREATE TABLE "EdgeDefinitions" (
    "Id" uuid PRIMARY KEY,
    "WorkflowDefinitionId" uuid NOT NULL REFERENCES "WorkflowDefinitions"("Id") ON DELETE CASCADE,
    "SourceNodeId" varchar(100) NOT NULL,
    "TargetNodeId" varchar(100) NOT NULL,
    "Condition" text,
    UNIQUE ("WorkflowDefinitionId", "SourceNodeId", "TargetNodeId")
);

-- Indexes
CREATE INDEX "IX_WorkflowDefinitions_Name" ON "WorkflowDefinitions"("Name");
CREATE INDEX "IX_WorkflowDefinitions_Status" ON "WorkflowDefinitions"("Status");
CREATE INDEX "IX_WorkflowDefinitions_CreatedAt" ON "WorkflowDefinitions"("CreatedAt" DESC);
CREATE INDEX "IX_WorkflowDefinitions_IsDeleted" ON "WorkflowDefinitions"("IsDeleted");

CREATE INDEX "IX_NodeDefinitions_WorkflowDefinitionId" ON "NodeDefinitions"("WorkflowDefinitionId");
CREATE INDEX "IX_NodeDefinitions_NodeType" ON "NodeDefinitions"("NodeType");

CREATE INDEX "IX_EdgeDefinitions_WorkflowDefinitionId" ON "EdgeDefinitions"("WorkflowDefinitionId");
```

**設計考量**:

1. **JSONB 欄位**:
   - DefinitionJson 和 ConfigurationJson 使用 JSONB
   - 支援靈活的 schema evolution
   - 可使用 PostgreSQL JSONB 查詢功能

2. **Cascade Delete**:
   - Nodes 和 Edges 在 Workflow 刪除時自動刪除
   - 簡化刪除邏輯

3. **Unique Constraints**:
   - NodeId 在同一個 Workflow 內唯一
   - Edge (Source, Target) 組合唯一

4. **Indexes**:
   - 優化常見查詢 (Name, Status, CreatedAt)
   - 支援 Soft Delete 過濾

### Repository Pattern

**IWorkflowDefinitionRepository**:

```csharp
public interface IWorkflowDefinitionRepository : IRepository<WorkflowDefinition>
{
    // Basic CRUD
    Task<WorkflowDefinition?> GetByIdAsync(Guid id, CancellationToken cancellationToken = default);
    Task<WorkflowDefinition?> GetByIdWithDetailsAsync(Guid id, CancellationToken cancellationToken = default);
    Task AddAsync(WorkflowDefinition workflowDefinition, CancellationToken cancellationToken = default);
    void Update(WorkflowDefinition workflowDefinition);
    void Delete(WorkflowDefinition workflowDefinition);

    // Query methods
    Task<PagedResult<WorkflowDefinition>> GetPagedAsync(
        int pageNumber,
        int pageSize,
        string? searchTerm = null,
        WorkflowStatus? status = null,
        CancellationToken cancellationToken = default);

    Task<IEnumerable<WorkflowDefinition>> GetByStatusAsync(
        WorkflowStatus status,
        CancellationToken cancellationToken = default);

    Task<bool> ExistsAsync(Guid id, CancellationToken cancellationToken = default);

    // Business queries
    Task<IEnumerable<WorkflowDefinition>> GetRecentAsync(
        int count,
        CancellationToken cancellationToken = default);
}
```

### CQRS Pattern

**Commands**:

```csharp
// CreateWorkflowDefinitionCommand
public record CreateWorkflowDefinitionCommand : IRequest<Result<Guid>>
{
    public string Name { get; init; }
    public string? Description { get; init; }
    public string DefinitionJson { get; init; }
    public List<NodeDefinitionDto> Nodes { get; init; }
    public List<EdgeDefinitionDto> Edges { get; init; }
}

// UpdateWorkflowDefinitionCommand
public record UpdateWorkflowDefinitionCommand : IRequest<Result<bool>>
{
    public Guid Id { get; init; }
    public string Name { get; init; }
    public string? Description { get; init; }
    public string DefinitionJson { get; init; }
    public List<NodeDefinitionDto> Nodes { get; init; }
    public List<EdgeDefinitionDto> Edges { get; init; }
}

// DeleteWorkflowDefinitionCommand (Soft Delete)
public record DeleteWorkflowDefinitionCommand(Guid Id) : IRequest<Result<bool>>;

// ActivateWorkflowDefinitionCommand
public record ActivateWorkflowDefinitionCommand(Guid Id) : IRequest<Result<bool>>;
```

**Queries**:

```csharp
// GetWorkflowDefinitionByIdQuery
public record GetWorkflowDefinitionByIdQuery(Guid Id)
    : IRequest<Result<WorkflowDefinitionDto>>;

// GetWorkflowDefinitionsQuery (Paged)
public record GetWorkflowDefinitionsQuery : IRequest<Result<PagedResult<WorkflowDefinitionDto>>>
{
    public int PageNumber { get; init; } = 1;
    public int PageSize { get; init; } = 10;
    public string? SearchTerm { get; init; }
    public WorkflowStatus? Status { get; init; }
}
```

### API 設計

**RESTful Endpoints**:

```yaml
POST /api/v1/workflow-definitions:
  描述: 創建新的 Workflow Definition
  請求: CreateWorkflowDefinitionRequest
  響應: 201 Created + WorkflowDefinitionDto
  錯誤: 400 Bad Request, 500 Internal Server Error

GET /api/v1/workflow-definitions:
  描述: 取得 Workflow Definition 列表 (分頁)
  查詢參數: ?pageNumber=1&pageSize=10&searchTerm=xxx&status=Active
  響應: 200 OK + PagedResult<WorkflowDefinitionDto>
  錯誤: 400 Bad Request, 500 Internal Server Error

GET /api/v1/workflow-definitions/{id}:
  描述: 取得單一 Workflow Definition (包含 Nodes 和 Edges)
  響應: 200 OK + WorkflowDefinitionDto
  錯誤: 404 Not Found, 500 Internal Server Error

PUT /api/v1/workflow-definitions/{id}:
  描述: 更新 Workflow Definition
  請求: UpdateWorkflowDefinitionRequest
  響應: 200 OK + WorkflowDefinitionDto
  錯誤: 400 Bad Request, 404 Not Found, 500 Internal Server Error

DELETE /api/v1/workflow-definitions/{id}:
  描述: 刪除 Workflow Definition (Soft Delete)
  響應: 204 No Content
  錯誤: 404 Not Found, 500 Internal Server Error

POST /api/v1/workflow-definitions/{id}/activate:
  描述: 啟用 Workflow Definition
  響應: 200 OK + WorkflowDefinitionDto
  錯誤: 400 Bad Request, 404 Not Found, 500 Internal Server Error
```

**參考文檔**: docs/api/workflow-api-design.md

---

## Node Registry System 技術上下文

### 設計背景

**業務需求**:
- 支援多種 Node types (Agent, Tool, Condition, Parallel, Sequential)
- 每種 Node type 有不同的 configuration schema
- 支援動態註冊新的 Node types
- 為視覺化編輯器提供 Node type 查詢 API

**技術挑戰**:
1. **Extensibility**: 如何設計可擴展的 Node Registry
2. **Schema Validation**: 如何驗證 Node configuration 符合 schema
3. **Node Compatibility**: 如何定義 Node 之間的兼容性
4. **Performance**: 如何優化 Node type 查詢性能

### NodeTypeDefinition 設計

**核心數據結構**:

```csharp
public class NodeTypeDefinition
{
    public string Type { get; set; } // e.g., "Agent", "Tool", "Condition"
    public string DisplayName { get; set; } // e.g., "AI Agent"
    public string Description { get; set; }
    public string Category { get; set; } // e.g., "Core", "Custom"
    public string IconUrl { get; set; }

    // JSON Schema for configuration validation
    public string ConfigurationSchema { get; set; }

    // Compatibility rules
    public List<string> CompatibleSourceTypes { get; set; } // Can receive from
    public List<string> CompatibleTargetTypes { get; set; } // Can send to

    // Additional metadata
    public Dictionary<string, object> Metadata { get; set; }
}
```

**內建 Node Types**:

```csharp
public static class BuiltInNodeTypes
{
    public static readonly NodeTypeDefinition Agent = new()
    {
        Type = "Agent",
        DisplayName = "AI Agent",
        Description = "執行 AI Agent 任務",
        Category = "Core",
        IconUrl = "/icons/agent.svg",
        ConfigurationSchema = @"{
            ""type"": ""object"",
            ""properties"": {
                ""agentId"": { ""type"": ""string"", ""format"": ""uuid"" },
                ""parameters"": { ""type"": ""object"" }
            },
            ""required"": [""agentId""]
        }",
        CompatibleSourceTypes = new List<string> { "Start", "Tool", "Condition", "Agent" },
        CompatibleTargetTypes = new List<string> { "Tool", "Condition", "Agent", "End" }
    };

    public static readonly NodeTypeDefinition Tool = new()
    {
        Type = "Tool",
        DisplayName = "Tool Call",
        Description = "調用 Tool/Plugin",
        Category = "Core",
        IconUrl = "/icons/tool.svg",
        ConfigurationSchema = @"{
            ""type"": ""object"",
            ""properties"": {
                ""toolName"": { ""type"": ""string"" },
                ""parameters"": { ""type"": ""object"" }
            },
            ""required"": [""toolName""]
        }",
        CompatibleSourceTypes = new List<string> { "Start", "Agent", "Tool", "Condition" },
        CompatibleTargetTypes = new List<string> { "Agent", "Tool", "Condition", "End" }
    };

    public static readonly NodeTypeDefinition Condition = new()
    {
        Type = "Condition",
        DisplayName = "Condition",
        Description = "條件判斷 (基礎)",
        Category = "Core",
        IconUrl = "/icons/condition.svg",
        ConfigurationSchema = @"{
            ""type"": ""object"",
            ""properties"": {
                ""condition"": { ""type"": ""string"" },
                ""trueBranch"": { ""type"": ""string"" },
                ""falseBranch"": { ""type"": ""string"" }
            },
            ""required"": [""condition""]
        }",
        CompatibleSourceTypes = new List<string> { "Agent", "Tool" },
        CompatibleTargetTypes = new List<string> { "Agent", "Tool", "End" }
    };

    // ... more node types
}
```

### Node Registry Service

**INodeTypeRegistry 介面**:

```csharp
public interface INodeTypeRegistry
{
    // Registration
    void RegisterNodeType(NodeTypeDefinition nodeType);
    void UnregisterNodeType(string nodeType);

    // Query
    NodeTypeDefinition? GetNodeType(string nodeType);
    IEnumerable<NodeTypeDefinition> GetAllNodeTypes();
    IEnumerable<NodeTypeDefinition> GetNodeTypesByCategory(string category);

    // Validation
    bool IsValidNodeType(string nodeType);
    bool ValidateConfiguration(string nodeType, string configurationJson);
    ValidationResult ValidateConfigurationDetailed(string nodeType, string configurationJson);

    // Compatibility
    bool AreCompatible(string sourceNodeType, string targetNodeType);
    IEnumerable<string> GetCompatibleTargetTypes(string sourceNodeType);
}
```

**NodeTypeRegistry 實作**:

```csharp
public class NodeTypeRegistry : INodeTypeRegistry
{
    private readonly ConcurrentDictionary<string, NodeTypeDefinition> _nodeTypes = new();
    private readonly ILogger<NodeTypeRegistry> _logger;

    public NodeTypeRegistry(ILogger<NodeTypeRegistry> logger)
    {
        _logger = logger;

        // Register built-in node types
        RegisterBuiltInNodeTypes();
    }

    private void RegisterBuiltInNodeTypes()
    {
        RegisterNodeType(BuiltInNodeTypes.Agent);
        RegisterNodeType(BuiltInNodeTypes.Tool);
        RegisterNodeType(BuiltInNodeTypes.Condition);
        RegisterNodeType(BuiltInNodeTypes.Parallel);
        RegisterNodeType(BuiltInNodeTypes.Sequential);

        _logger.LogInformation("Registered {Count} built-in node types", _nodeTypes.Count);
    }

    public void RegisterNodeType(NodeTypeDefinition nodeType)
    {
        if (_nodeTypes.TryAdd(nodeType.Type, nodeType))
        {
            _logger.LogInformation("Registered node type: {Type}", nodeType.Type);
        }
        else
        {
            _logger.LogWarning("Node type already registered: {Type}", nodeType.Type);
        }
    }

    public NodeTypeDefinition? GetNodeType(string nodeType)
    {
        _nodeTypes.TryGetValue(nodeType, out var definition);
        return definition;
    }

    public bool ValidateConfiguration(string nodeType, string configurationJson)
    {
        var definition = GetNodeType(nodeType);
        if (definition == null)
            return false;

        try
        {
            var schema = JsonSchema.FromJsonAsync(definition.ConfigurationSchema).Result;
            var instance = JsonDocument.Parse(configurationJson);
            var errors = schema.Validate(instance.RootElement);

            return !errors.Any();
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Configuration validation failed for {NodeType}", nodeType);
            return false;
        }
    }

    public bool AreCompatible(string sourceNodeType, string targetNodeType)
    {
        var sourceDef = GetNodeType(sourceNodeType);
        if (sourceDef == null)
            return false;

        return sourceDef.CompatibleTargetTypes.Contains(targetNodeType);
    }
}
```

### JSON Schema Validation

**為什麼使用 JSON Schema**:
- 標準化: JSON Schema Draft 7 是業界標準
- 靈活性: 支援複雜的 validation rules
- 工具支持: 多種語言有成熟的 JSON Schema 庫
- 自動文檔: 可從 JSON Schema 生成 API 文檔

**JSON Schema 範例**:

```json
{
  "type": "object",
  "title": "Agent Node Configuration",
  "properties": {
    "agentId": {
      "type": "string",
      "format": "uuid",
      "description": "Agent ID"
    },
    "parameters": {
      "type": "object",
      "description": "Agent parameters",
      "additionalProperties": true
    },
    "timeout": {
      "type": "integer",
      "minimum": 1,
      "maximum": 3600,
      "default": 30,
      "description": "Timeout in seconds"
    }
  },
  "required": ["agentId"]
}
```

**參考文檔**:
- docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
- https://json-schema.org/

---

## Edge Validation Engine 技術上下文

### 設計背景

**業務需求**:
- 驗證 Edge connection 是否合法
- 檢測 Workflow 是否有 circular dependency
- 驗證 data flow 是否正確
- 提供詳細錯誤訊息和修復建議

**技術挑戰**:
1. **Circular Dependency Detection**: 需要高效的圖算法
2. **Node Compatibility**: 如何定義和驗證 Node 兼容性
3. **Data Flow Validation**: 如何驗證輸出類型匹配輸入類型
4. **Error Messages**: 如何生成有用的錯誤訊息

### Node Compatibility Matrix

**設計原則**:
- 每種 Node type 定義可以連接的 source 和 target types
- 使用 whitelist 策略 (明確定義允許的連接)
- 支援動態擴展 (新 Node types 可註冊自己的兼容性規則)

**Compatibility Matrix 範例**:

```yaml
Agent:
  CompatibleSourceTypes: [Start, Tool, Condition, Agent]
  CompatibleTargetTypes: [Tool, Condition, Agent, End]

Tool:
  CompatibleSourceTypes: [Start, Agent, Tool, Condition]
  CompatibleTargetTypes: [Agent, Tool, Condition, End]

Condition:
  CompatibleSourceTypes: [Agent, Tool]
  CompatibleTargetTypes: [Agent, Tool, End]

Parallel:
  CompatibleSourceTypes: [Start, Agent, Tool]
  CompatibleTargetTypes: [Agent, Tool, End]

Sequential:
  CompatibleSourceTypes: [Start, Agent, Tool]
  CompatibleTargetTypes: [Agent, Tool, End]
```

### Circular Dependency Detection

**算法選擇: DFS (Depth-First Search)**

**為什麼選擇 DFS**:
- 時間複雜度: O(V + E), V = nodes, E = edges
- 空間複雜度: O(V), 遞迴棧深度
- 簡單實現: 遞迴邏輯清晰
- 早期檢測: 發現循環即可停止

**DFS 實作**:

```csharp
public class CircularDependencyDetector
{
    private readonly Dictionary<string, List<string>> _adjacencyList;
    private readonly HashSet<string> _visited = new();
    private readonly HashSet<string> _recursionStack = new();

    public CircularDependencyDetector(IEnumerable<EdgeDefinition> edges)
    {
        _adjacencyList = BuildAdjacencyList(edges);
    }

    private Dictionary<string, List<string>> BuildAdjacencyList(IEnumerable<EdgeDefinition> edges)
    {
        var adjacencyList = new Dictionary<string, List<string>>();

        foreach (var edge in edges)
        {
            if (!adjacencyList.ContainsKey(edge.SourceNodeId))
                adjacencyList[edge.SourceNodeId] = new List<string>();

            adjacencyList[edge.SourceNodeId].Add(edge.TargetNodeId);
        }

        return adjacencyList;
    }

    public bool HasCircularDependency()
    {
        foreach (var node in _adjacencyList.Keys)
        {
            if (!_visited.Contains(node))
            {
                if (DFS(node))
                    return true;
            }
        }

        return false;
    }

    private bool DFS(string node)
    {
        _visited.Add(node);
        _recursionStack.Add(node);

        if (_adjacencyList.TryGetValue(node, out var neighbors))
        {
            foreach (var neighbor in neighbors)
            {
                if (!_visited.Contains(neighbor))
                {
                    if (DFS(neighbor))
                        return true;
                }
                else if (_recursionStack.Contains(neighbor))
                {
                    // Found circular dependency
                    return true;
                }
            }
        }

        _recursionStack.Remove(node);
        return false;
    }

    public List<string> FindCircularPath()
    {
        // Returns the path of circular dependency
        // Implementation details...
    }
}
```

**性能分析**:
- 最壞情況: O(V + E), 遍歷所有節點和邊
- 平均情況: O(V), 大部分 Workflow 沒有循環
- 空間: O(V), 遞迴棧和 visited/recursionStack 集合

### Edge Validation Service

**IEdgeValidationService 介面**:

```csharp
public interface IEdgeValidationService
{
    // Single edge validation
    Task<ValidationResult> ValidateEdgeAsync(
        string sourceNodeId,
        string targetNodeId,
        string sourceNodeType,
        string targetNodeType);

    // Workflow validation
    Task<ValidationResult> ValidateWorkflowAsync(
        IEnumerable<NodeDefinition> nodes,
        IEnumerable<EdgeDefinition> edges);

    // Circular dependency detection
    Task<CircularDependencyResult> DetectCircularDependencyAsync(
        IEnumerable<EdgeDefinition> edges);

    // Data flow validation
    Task<ValidationResult> ValidateDataFlowAsync(
        IEnumerable<NodeDefinition> nodes,
        IEnumerable<EdgeDefinition> edges);
}
```

**EdgeValidationService 實作**:

```csharp
public class EdgeValidationService : IEdgeValidationService
{
    private readonly INodeTypeRegistry _nodeTypeRegistry;
    private readonly ILogger<EdgeValidationService> _logger;

    public async Task<ValidationResult> ValidateEdgeAsync(
        string sourceNodeId,
        string targetNodeId,
        string sourceNodeType,
        string targetNodeType)
    {
        var errors = new List<string>();

        // Check node types are valid
        if (!_nodeTypeRegistry.IsValidNodeType(sourceNodeType))
        {
            errors.Add($"Invalid source node type: {sourceNodeType}");
        }

        if (!_nodeTypeRegistry.IsValidNodeType(targetNodeType))
        {
            errors.Add($"Invalid target node type: {targetNodeType}");
        }

        // Check compatibility
        if (!_nodeTypeRegistry.AreCompatible(sourceNodeType, targetNodeType))
        {
            errors.Add($"Node types {sourceNodeType} and {targetNodeType} are not compatible");

            var compatibleTypes = _nodeTypeRegistry.GetCompatibleTargetTypes(sourceNodeType);
            errors.Add($"Suggestion: {sourceNodeType} can connect to: {string.Join(", ", compatibleTypes)}");
        }

        return new ValidationResult
        {
            IsValid = !errors.Any(),
            Errors = errors
        };
    }

    public async Task<CircularDependencyResult> DetectCircularDependencyAsync(
        IEnumerable<EdgeDefinition> edges)
    {
        var detector = new CircularDependencyDetector(edges);
        var hasCircular = detector.HasCircularDependency();

        if (hasCircular)
        {
            var circularPath = detector.FindCircularPath();
            return new CircularDependencyResult
            {
                HasCircularDependency = true,
                CircularPath = circularPath,
                ErrorMessage = $"Circular dependency detected: {string.Join(" -> ", circularPath)} -> {circularPath[0]}"
            };
        }

        return new CircularDependencyResult { HasCircularDependency = false };
    }
}
```

### Validation API

**API Endpoints**:

```yaml
POST /api/v1/workflow-validation/edge:
  描述: 驗證單一 Edge 是否合法
  請求:
    {
      "sourceNodeId": "node-1",
      "targetNodeId": "node-2",
      "sourceNodeType": "Agent",
      "targetNodeType": "Tool"
    }
  響應:
    {
      "isValid": true,
      "errors": []
    }

POST /api/v1/workflow-validation/workflow:
  描述: 驗證完整 Workflow
  請求:
    {
      "nodes": [...],
      "edges": [...]
    }
  響應:
    {
      "isValid": false,
      "errors": [
        "Circular dependency detected: node-1 -> node-2 -> node-3 -> node-1",
        "Invalid edge: Agent cannot connect to Condition directly"
      ],
      "suggestions": [
        "Add an intermediate Tool node between Agent and Condition"
      ]
    }
```

---

## 技術決策背景

### TD-076: Workflow Schema 存儲格式

**問題**: 如何存儲靈活的 Workflow Definition schema?

**選項**:
1. **關聯式表結構**: 每個 Node/Edge 一個 row
   - 優點: 查詢靈活, 支援 JOIN
   - 缺點: 大型 Workflow 查詢慢, schema 變更困難

2. **JSONB 欄位**: 整個 Workflow definition 存為 JSONB
   - 優點: 靈活, 支援 schema evolution
   - 缺點: 查詢複雜, 需要 JSONB 查詢語法

3. **混合模式**: Metadata 關聯式 + Content JSONB
   - 優點: 平衡查詢性能和靈活性
   - 缺點: 複雜度較高

**決策**: 選擇選項 3 (混合模式)
- WorkflowDefinitions 表存儲 metadata (Name, Status, Version)
- DefinitionJson JSONB 欄位存儲完整 definition
- NodeDefinitions 和 EdgeDefinitions 表存儲結構化數據
- 優點: 支援快速查詢 metadata 和靈活的 definition

**參考**: docs/database/workflow-schema.md

### TD-077: Node Registry 實作策略

**問題**: Node Registry 應該是靜態還是動態?

**選項**:
1. **靜態註冊**: 所有 Node types 在啟動時註冊
   - 優點: 簡單, 性能好
   - 缺點: 不支援動態擴展

2. **動態註冊**: 支援運行時註冊 Node types
   - 優點: 靈活, 支援 Plugin 擴展
   - 缺點: 複雜度高, 需要管理註冊生命週期

**決策**: 選擇選項 2 (動態註冊)
- 內建 Node types 在啟動時註冊
- 支援運行時動態註冊自定義 Node types
- 使用 ConcurrentDictionary 確保線程安全
- 為未來 Plugin system 預留擴展空間

**參考**: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md

### TD-078: Circular Dependency Detection 算法

**問題**: 使用 DFS 還是 BFS 檢測循環?

**選項**:
1. **DFS (Depth-First Search)**:
   - 優點: 簡單, 遞迴實現, 早期檢測
   - 缺點: 遞迴棧深度限制 (實際不是問題)

2. **BFS (Breadth-First Search)**:
   - 優點: 迭代實現, 無遞迴棧限制
   - 缺點: 複雜, 不如 DFS 直觀

**決策**: 選擇 DFS
- 時間複雜度 O(V + E) 與 BFS 相同
- 實現簡單, 代碼可讀性高
- 遞迴深度通常 <100 (實際 Workflow 不會太深)
- 早期檢測: 發現循環即可停止

**參考**: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md

---

## 架構整合考量

### 與 Phase 1B 整合

**Workflow Execution Engine 整合**:

```yaml
Phase 1B 提供:
  - IWorkflowEngine: Workflow 執行介面
  - Workflow 執行狀態管理
  - Multi-Agent 協調

Phase 1C 整合:
  - WorkflowDefinition → IWorkflowEngine.ExecuteAsync()
  - WorkflowDefinition.DefinitionJson → Workflow execution plan
  - NodeDefinitions → Agent tasks
  - EdgeDefinitions → Task dependencies

整合點:
  1. WorkflowDefinition.Id → WorkflowExecutionContext.WorkflowId
  2. NodeDefinition.NodeType → Agent type mapping
  3. EdgeDefinition.Condition → Task execution condition

參考文檔:
  - docs/architecture/ADR-011-framework-abstraction-layer.md
```

### 與 Phase 1D 整合 (Frontend)

**API 契約定義**:

```yaml
Frontend 需求:
  - 取得所有 Node types (GET /api/v1/node-types)
  - 取得 Node type schema (GET /api/v1/node-types/{type}/schema)
  - 驗證 Edge connection (POST /api/v1/workflow-validation/edge)
  - 即時 Workflow 驗證 (POST /api/v1/workflow-validation/workflow)
  - CRUD Workflow Definition

Backend 提供:
  - RESTful API (OpenAPI 3.0)
  - 標準化錯誤格式
  - 詳細錯誤訊息
  - 修復建議

參考文檔:
  - docs/api/workflow-api-design.md
  - docs/architecture/ADR-012-workflow-editor-technology.md
```

---

## 參考文檔列表

### 核心文檔 (35+)

```yaml
規劃文檔:
  1. claudedocs/1-planning/MVP-SCOPE-DEFINITION.md
  2. claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md
  3. claudedocs/1-planning/DEVELOPMENT-STRATEGY.md
  4. claudedocs/1-planning/DEPENDENCY-MATRIX.md
  5. claudedocs/1-planning/RISK-REGISTER.md

架構文檔:
  6. docs/architecture/Architecture-Design-Document.md
  7. docs/architecture/ADR-004-repository-pattern.md
  8. docs/architecture/ADR-005-cqrs-pattern.md
  9. docs/architecture/ADR-011-framework-abstraction-layer.md
  10. docs/architecture/ADR-012-workflow-editor-technology.md
  11. docs/architecture/C4-architecture-diagrams.md

API & Database:
  12. docs/api/workflow-api-design.md
  13. docs/api/api-documentation-standards.md
  14. docs/database/workflow-schema.md
  15. docs/database/database-schema.md

技術實施:
  16. docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
  17. docs/technical-implementation/01-backend-net9/02-domain-layer-implementation.md
  18. docs/technical-implementation/01-backend-net9/03-application-layer-implementation.md
  19. docs/technical-implementation/01-backend-net9/04-infrastructure-layer-implementation.md

測試文檔:
  20. docs/testing/unit-testing-guidelines.md
  21. docs/testing/integration-testing-guidelines.md
  22. docs/testing/test-strategy.md

性能與安全:
  23. docs/performance/performance-targets.md
  24. docs/security/Security-Requirements.md

User Stories:
  25. docs/user-stories/modules/module-07/US-7.1-Workflow-Editor-Backend-API.md
  26. docs/user-stories/implementation-strategy.md

UI/UX:
  27. docs/ux-design/user-research/personas.md
  28. docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-index.md

Sprint 文檔:
  29. claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md
  30. claudedocs/2-sprints/sprint-10/SPRINT-10-1-OVERVIEW.md
  31. claudedocs/2-sprints/sprint-10/SPRINT-10-2-PLAN.md

開發標準:
  32. docs/development-standards/coding-standards.md
  33. docs/development-standards/code-quality-standards.md
  34. docs/development-standards/definition-of-done.md

其他:
  35. docs/brief.md
  36. CLAUDE.md
  37. README.md
```

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**關鍵技術**: .NET 9, EF Core 9, PostgreSQL 16, JSON Schema, DFS Algorithm
**核心決策**: 3 個 (TD-076, TD-077, TD-078)
