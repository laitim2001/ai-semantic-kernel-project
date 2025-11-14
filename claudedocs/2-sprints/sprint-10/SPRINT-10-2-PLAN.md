# SPRINT-10-2-PLAN.md - Sprint 10 實施計劃：Workflow Editor Backend API 與 Phase 1C 啟動

**版本**: v2.1
**Sprint 編號**: Sprint 10
**Sprint 週期**: Week 28-30 (3 週)
**Phase**: Phase 1C - 工作流編輯器 Backend (Workflow Editor Backend)
**計劃日期**: 2026-04-21 ~ 2026-05-11
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 目錄 (Table of Contents)

### 第一部分: 規劃文檔參考
1. [Sprint 10 實施範圍概述](#第一部分-規劃文檔參考)
2. [架構決策引用](#架構決策引用)
3. [Phase 1C 啟動里程碑](#phase-1c-啟動里程碑)

### 第二部分: 本 Sprint 要建立什麼
4. [Workflow Editor Backend API 核心](#第二部分-本-sprint-要建立什麼)
5. [Phase 1: Workflow Definition CRUD](#phase-1-workflow-definition-crud-3-4-sp)
6. [Phase 2: Node Registry](#phase-2-node-registry-2-3-sp)
7. [Phase 3: Edge Validation](#phase-3-edge-validation-2-3-sp)
8. [Phase 4: 整合測試與文檔](#phase-4-整合測試與文檔-1-2-sp)

### 第三部分: 如何實施
9. [詳細實作指南 - Workflow Definition CRUD](#詳細實作指南-workflow-definition-crud)
10. [詳細實作指南 - Node Registry](#詳細實作指南-node-registry)
11. [詳細實作指南 - Edge Validation](#詳細實作指南-edge-validation)
12. [詳細實作指南 - Integration & Documentation](#詳細實作指南-integration--documentation)

### 第四部分: 編碼規範與最佳實踐
13. [編碼規範](#第四部分-編碼規範與最佳實踐)
14. [測試策略](#測試策略)
15. [API 設計原則](#api-設計原則)

### 第五部分: 質量保證
16. [質量檢查清單](#第五部分-質量保證)
17. [驗收測試](#驗收測試)
18. [Phase 1C Part 1 完成驗收](#phase-1c-part-1-完成驗收)

### 第六部分: 參考文檔
19. [完整參考文檔列表](#第六部分-參考文檔)

---

## 第一部分: 規劃文檔參考

### Sprint 10 實施範圍概述

**核心交付物** (8 SP):
```yaml
Phase 1: Workflow Definition CRUD (3-4 SP)
  目標: 建立 Workflow Definition 完整 CRUD API
  組件:
    - WorkflowDefinition 實體 (Domain Model)
    - WorkflowDefinitionRepository (Repository Pattern)
    - CQRS Commands/Queries (MediatR)
    - Workflow Definition API Controllers
    - Database Migrations
  關鍵決策:
    - RESTful API 設計
    - Soft Delete 策略
    - Version 欄位設計
    - JSON Schema 儲存格式

Phase 2: Node Registry (2-3 SP)
  目標: 建立 Node Type 註冊和查詢機制
  組件:
    - Node Type Registry Service
    - NodeTypeDefinition 模型
    - Node Schema Validation (JSON Schema)
    - Node Registry API
  關鍵決策:
    - Node Type 註冊機制 (靜態 vs 動態)
    - JSON Schema Draft 7 標準
    - Node Compatibility 定義
    - Extensibility 設計

Phase 3: Edge Validation (2-3 SP)
  目標: 建立 Edge Connection 驗證邏輯
  組件:
    - Edge Validation Service
    - Node Compatibility Matrix
    - Circular Dependency Detection (DFS)
    - Edge Validation API
  關鍵決策:
    - Compatibility Rules 定義
    - DFS vs BFS 算法選擇
    - Data Flow 驗證策略
    - 錯誤訊息設計

Phase 4: 整合測試與文檔 (1-2 SP)
  目標: 完成整合測試和 API 文檔
  組件:
    - Integration Tests (TestContainers)
    - API Documentation (Swagger/OpenAPI)
    - Sprint Retrospective
  關鍵決策:
    - TestContainers PostgreSQL 設定
    - Swagger 註解標準
    - 文檔範例編寫
```

**架構依賴關係**:
```
Phase 1A (Sprint 1-6) 成果 (前置依賴):
├─ Agent Management ✅
├─ Plugin System ✅
├─ Persona Framework ✅
├─ Knowledge Base ✅
├─ Code Interpreter ✅
└─ Text-to-SQL ✅

Phase 1B (Sprint 7-9) 成果 (前置依賴):
├─ Task Generator ✅
├─ Multi-Agent Coordination ✅
├─ Workflow Execution Engine ✅
├─ Agent Messaging ✅
├─ Framework Abstraction Layer ✅
└─ Phase 1B 100% 完成 ✅

Sprint 10 新建 (本 Sprint):
├─ Workflow Definition CRUD 🎯
├─ Node Registry 🎯
├─ Edge Validation 🎯
└─ Phase 1C Part 1 (50%) 🎉

Sprint 11 依賴 (後續):
├─ Workflow Validation API
├─ Schema Versioning
└─ Phase 1C 100% 完成

Phase 1D 依賴 (後續):
├─ Workflow Editor Frontend (Vue 3 + VueFlow)
├─ Visual Workflow Designer
└─ Real-time Collaboration
```

### 架構決策引用

**核心 ADR**:
- [ADR-012: Workflow Editor Technology](../../docs/architecture/ADR-012-workflow-editor-technology.md)
  - **決策**: Vue 3 + VueFlow + Module Federation
  - **理由**: 成熟的工作流編輯器方案,參考 n8n 架構
  - **實施影響**: Sprint 10-11 Backend API 設計需配合 Frontend 需求

**相關 ADR**:
- [ADR-005: CQRS Pattern](../../docs/architecture/adr/ADR-005-cqrs-pattern.md)
- [ADR-004: Repository Pattern](../../docs/architecture/adr/ADR-004-repository-pattern.md)
- [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- [ADR-011: Framework Abstraction Layer](../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md)

### Phase 1C 啟動里程碑

**🎉 Phase 1C 啟動標誌**:
```yaml
Phase 1B 完成 (Sprint 7-9) ✅:
  - Sprint 7: Task Generator + Coordination (13 SP) ✅
  - Sprint 8: Execution Engine + Messaging (13 SP) ✅
  - Sprint 9: Framework Abstraction Layer (9 SP) ✅
  - 總計: 35 SP, 9 週 (Week 19-27), 100% 完成 ✅

Phase 1C 開始 (Sprint 10-11) 🎯:
  - Sprint 10: Workflow Editor Backend API Part 1 (8 SP) 🎯
  - Sprint 11: Workflow Editor Backend API Part 2 (8 SP) ⏳
  - 總計: 16 SP, 6 週 (Week 28-33)
  - 預期完成: 2026-05-25

Phase 1C 目標:
  - ✅ Workflow Definition CRUD API
  - ✅ Node Registry 和 Schema Validation
  - ✅ Edge Validation 和 Circular Dependency Detection
  - ✅ Workflow Validation API
  - ✅ Schema Versioning
  - ✅ 為 Phase 1D (Frontend) 提供完整 API 支持
```

**Phase 1C 核心價值**:
```yaml
對 IT 開發者:
  - 可透過 API 創建和管理 Workflow Definition
  - 支援 Workflow 版本控制和重用
  - 驗證 Workflow 正確性,減少錯誤

對業務分析師:
  - 為視覺化編輯器提供 Backend 支持
  - 確保 Workflow 定義的一致性

對企業管理者:
  - Workflow 集中管理和版本控制
  - 為 Phase 1D (視覺化編輯器) 奠定基礎

技術價值:
  - 完整的 Workflow Definition 管理 API
  - 靈活的 Node Registry 擴展機制
  - 強大的 Validation 引擎
  - RESTful API 設計最佳實踐
```

---

## 第二部分: 本 Sprint 要建立什麼

### Workflow Editor Backend API 核心

**為什麼要建立 Workflow Editor Backend API?**
- **問題**: 當前只有 Workflow 執行能力,缺乏 Workflow 定義管理
- **需求**: Phase 1D 視覺化編輯器需要完整的 Backend API 支持
- **解決方案**: 建立 Workflow Definition CRUD + Node Registry + Edge Validation
- **預期收益**:
  - 支援 Workflow 的 CRUD 操作
  - 支援 Node types 動態註冊和查詢
  - 提供 Workflow 驗證能力
  - 為視覺化編輯器提供完整 API 基礎

**技術挑戰**:
- Workflow Definition 的靈活存儲格式 (JSON Schema)
- Node Registry 的可擴展性設計
- Edge Validation 的複雜性 (Circular Dependency, Data Flow)
- API 設計的 RESTful 原則和版本化
- 性能優化 (查詢、驗證)

**與 Phase 1B 的關係**:
```yaml
Phase 1B 提供:
  ✅ Workflow 執行能力 (Execution Engine)
  ✅ Multi-Agent 協調能力
  ✅ Framework Abstraction Layer

Phase 1C 提供:
  🎯 Workflow 定義能力 (Definition CRUD)
  🎯 Workflow 驗證能力 (Validation API)
  🎯 Node 註冊和查詢能力

整合:
  - Phase 1C 的 Workflow Definition → Phase 1B 的 Execution Engine
  - Phase 1D 的 Visual Editor → Phase 1C 的 Backend API
  - 完整的 Workflow 生命週期: 定義 → 驗證 → 執行
```

---

### Phase 1: Workflow Definition CRUD (3-4 SP)

**目標**: 建立 Workflow Definition 完整 CRUD API

**核心組件**:

1. **WorkflowDefinition 實體** (Domain Layer)
   ```csharp
   // WorkflowDefinition.cs
   public class WorkflowDefinition : BaseEntity
   {
       public Guid Id { get; set; }
       public string Name { get; set; } // Required, Max 200 chars
       public string? Description { get; set; } // Optional, Max 1000 chars
       public int Version { get; set; } // Version number, default 1
       public string DefinitionJson { get; set; } // JSON format
       public WorkflowStatus Status { get; set; } // Draft, Active, Archived
       public DateTime CreatedAt { get; set; }
       public DateTime? UpdatedAt { get; set; }
       public bool IsDeleted { get; set; } // Soft delete

       // Navigation properties
       public ICollection<NodeDefinition> Nodes { get; set; }
       public ICollection<EdgeDefinition> Edges { get; set; }
   }

   public class NodeDefinition
   {
       public Guid Id { get; set; }
       public Guid WorkflowDefinitionId { get; set; }
       public string NodeId { get; set; } // Unique within workflow
       public string NodeType { get; set; } // Agent, Tool, Condition, etc.
       public string ConfigurationJson { get; set; } // Node-specific config
       public Position Position { get; set; } // X, Y coordinates
   }

   public class EdgeDefinition
   {
       public Guid Id { get; set; }
       public Guid WorkflowDefinitionId { get; set; }
       public string SourceNodeId { get; set; }
       public string TargetNodeId { get; set; }
       public string? Condition { get; set; } // Optional condition expression
   }

   public enum WorkflowStatus
   {
       Draft = 0,
       Active = 1,
       Archived = 2
   }
   ```

2. **Repository 實現** (Infrastructure Layer)
   ```csharp
   // IWorkflowDefinitionRepository.cs
   public interface IWorkflowDefinitionRepository : IRepository<WorkflowDefinition>
   {
       Task<WorkflowDefinition?> GetByIdWithDetailsAsync(Guid id);
       Task<PagedResult<WorkflowDefinition>> GetPagedAsync(
           int pageNumber,
           int pageSize,
           string? searchTerm = null,
           WorkflowStatus? status = null);
       Task<bool> ExistsAsync(Guid id);
       Task<IEnumerable<WorkflowDefinition>> GetByStatusAsync(WorkflowStatus status);
   }

   // WorkflowDefinitionRepository.cs
   public class WorkflowDefinitionRepository : Repository<WorkflowDefinition>,
       IWorkflowDefinitionRepository
   {
       public WorkflowDefinitionRepository(ApplicationDbContext context)
           : base(context) { }

       public async Task<WorkflowDefinition?> GetByIdWithDetailsAsync(Guid id)
       {
           return await _dbSet
               .Include(w => w.Nodes)
               .Include(w => w.Edges)
               .FirstOrDefaultAsync(w => w.Id == id && !w.IsDeleted);
       }

       public async Task<PagedResult<WorkflowDefinition>> GetPagedAsync(
           int pageNumber,
           int pageSize,
           string? searchTerm = null,
           WorkflowStatus? status = null)
       {
           var query = _dbSet.Where(w => !w.IsDeleted);

           if (!string.IsNullOrEmpty(searchTerm))
           {
               query = query.Where(w => w.Name.Contains(searchTerm) ||
                                       w.Description!.Contains(searchTerm));
           }

           if (status.HasValue)
           {
               query = query.Where(w => w.Status == status.Value);
           }

           var totalCount = await query.CountAsync();
           var items = await query
               .OrderByDescending(w => w.CreatedAt)
               .Skip((pageNumber - 1) * pageSize)
               .Take(pageSize)
               .ToListAsync();

           return new PagedResult<WorkflowDefinition>(
               items, totalCount, pageNumber, pageSize);
       }
   }
   ```

3. **CQRS Commands/Queries** (Application Layer)
   ```csharp
   // CreateWorkflowDefinitionCommand.cs
   public record CreateWorkflowDefinitionCommand : IRequest<Result<Guid>>
   {
       public string Name { get; init; }
       public string? Description { get; init; }
       public string DefinitionJson { get; init; }
       public List<NodeDefinitionDto> Nodes { get; init; }
       public List<EdgeDefinitionDto> Edges { get; init; }
   }

   public class CreateWorkflowDefinitionCommandValidator
       : AbstractValidator<CreateWorkflowDefinitionCommand>
   {
       public CreateWorkflowDefinitionCommandValidator()
       {
           RuleFor(x => x.Name)
               .NotEmpty().WithMessage("Workflow name is required")
               .MaximumLength(200).WithMessage("Name too long");

           RuleFor(x => x.Description)
               .MaximumLength(1000).When(x => !string.IsNullOrEmpty(x.Description));

           RuleFor(x => x.DefinitionJson)
               .NotEmpty().WithMessage("Definition JSON is required")
               .Must(BeValidJson).WithMessage("Invalid JSON format");

           RuleFor(x => x.Nodes)
               .NotEmpty().WithMessage("At least one node is required");
       }

       private bool BeValidJson(string json)
       {
           try
           {
               JsonDocument.Parse(json);
               return true;
           }
           catch
           {
               return false;
           }
       }
   }

   public class CreateWorkflowDefinitionCommandHandler
       : IRequestHandler<CreateWorkflowDefinitionCommand, Result<Guid>>
   {
       private readonly IWorkflowDefinitionRepository _repository;
       private readonly IUnitOfWork _unitOfWork;
       private readonly ILogger<CreateWorkflowDefinitionCommandHandler> _logger;

       public async Task<Result<Guid>> Handle(
           CreateWorkflowDefinitionCommand request,
           CancellationToken cancellationToken)
       {
           try
           {
               var workflowDef = new WorkflowDefinition
               {
                   Id = Guid.NewGuid(),
                   Name = request.Name,
                   Description = request.Description,
                   DefinitionJson = request.DefinitionJson,
                   Version = 1,
                   Status = WorkflowStatus.Draft,
                   CreatedAt = DateTime.UtcNow,
                   Nodes = request.Nodes.Select(n => new NodeDefinition
                   {
                       NodeId = n.NodeId,
                       NodeType = n.NodeType,
                       ConfigurationJson = n.ConfigurationJson,
                       Position = n.Position
                   }).ToList(),
                   Edges = request.Edges.Select(e => new EdgeDefinition
                   {
                       SourceNodeId = e.SourceNodeId,
                       TargetNodeId = e.TargetNodeId,
                       Condition = e.Condition
                   }).ToList()
               };

               await _repository.AddAsync(workflowDef);
               await _unitOfWork.SaveChangesAsync(cancellationToken);

               _logger.LogInformation(
                   "Created workflow definition {Id} with name {Name}",
                   workflowDef.Id, workflowDef.Name);

               return Result<Guid>.Success(workflowDef.Id);
           }
           catch (Exception ex)
           {
               _logger.LogError(ex, "Error creating workflow definition");
               return Result<Guid>.Failure("Failed to create workflow definition");
           }
       }
   }

   // GetWorkflowDefinitionByIdQuery.cs
   public record GetWorkflowDefinitionByIdQuery(Guid Id)
       : IRequest<Result<WorkflowDefinitionDto>>;

   public class GetWorkflowDefinitionByIdQueryHandler
       : IRequestHandler<GetWorkflowDefinitionByIdQuery, Result<WorkflowDefinitionDto>>
   {
       private readonly IWorkflowDefinitionRepository _repository;

       public async Task<Result<WorkflowDefinitionDto>> Handle(
           GetWorkflowDefinitionByIdQuery request,
           CancellationToken cancellationToken)
       {
           var workflowDef = await _repository.GetByIdWithDetailsAsync(request.Id);

           if (workflowDef == null)
               return Result<WorkflowDefinitionDto>.Failure("Workflow not found");

           var dto = new WorkflowDefinitionDto
           {
               Id = workflowDef.Id,
               Name = workflowDef.Name,
               Description = workflowDef.Description,
               Version = workflowDef.Version,
               Status = workflowDef.Status.ToString(),
               DefinitionJson = workflowDef.DefinitionJson,
               Nodes = workflowDef.Nodes.Select(n => new NodeDefinitionDto
               {
                   NodeId = n.NodeId,
                   NodeType = n.NodeType,
                   ConfigurationJson = n.ConfigurationJson,
                   Position = n.Position
               }).ToList(),
               Edges = workflowDef.Edges.Select(e => new EdgeDefinitionDto
               {
                   SourceNodeId = e.SourceNodeId,
                   TargetNodeId = e.TargetNodeId,
                   Condition = e.Condition
               }).ToList(),
               CreatedAt = workflowDef.CreatedAt,
               UpdatedAt = workflowDef.UpdatedAt
           };

           return Result<WorkflowDefinitionDto>.Success(dto);
       }
   }
   ```

4. **API Controllers** (Presentation Layer)
   ```csharp
   // WorkflowDefinitionsController.cs
   [ApiController]
   [Route("api/v1/workflow-definitions")]
   [Produces("application/json")]
   public class WorkflowDefinitionsController : ControllerBase
   {
       private readonly IMediator _mediator;
       private readonly ILogger<WorkflowDefinitionsController> _logger;

       [HttpPost]
       [ProducesResponseType(typeof(CreateWorkflowResponse), StatusCodes.Status201Created)]
       [ProducesResponseType(StatusCodes.Status400BadRequest)]
       [ProducesResponseType(StatusCodes.Status500InternalServerError)]
       public async Task<IActionResult> CreateWorkflow(
           [FromBody] CreateWorkflowDefinitionCommand command)
       {
           var result = await _mediator.Send(command);

           if (result.IsSuccess)
           {
               return CreatedAtAction(
                   nameof(GetWorkflow),
                   new { id = result.Value },
                   new CreateWorkflowResponse { Id = result.Value });
           }

           return BadRequest(new { error = result.Error });
       }

       [HttpGet]
       [ProducesResponseType(typeof(PagedResponse<WorkflowDefinitionDto>),
           StatusCodes.Status200OK)]
       public async Task<IActionResult> GetWorkflows(
           [FromQuery] int pageNumber = 1,
           [FromQuery] int pageSize = 20,
           [FromQuery] string? searchTerm = null,
           [FromQuery] string? status = null)
       {
           WorkflowStatus? statusEnum = null;
           if (!string.IsNullOrEmpty(status) &&
               Enum.TryParse<WorkflowStatus>(status, true, out var parsed))
           {
               statusEnum = parsed;
           }

           var query = new GetWorkflowDefinitionsQuery(
               pageNumber, pageSize, searchTerm, statusEnum);
           var result = await _mediator.Send(query);

           return Ok(result);
       }

       [HttpGet("{id}")]
       [ProducesResponseType(typeof(WorkflowDefinitionDto), StatusCodes.Status200OK)]
       [ProducesResponseType(StatusCodes.Status404NotFound)]
       public async Task<IActionResult> GetWorkflow(Guid id)
       {
           var query = new GetWorkflowDefinitionByIdQuery(id);
           var result = await _mediator.Send(query);

           if (result.IsSuccess)
               return Ok(result.Value);

           return NotFound(new { error = result.Error });
       }

       [HttpPut("{id}")]
       [ProducesResponseType(StatusCodes.Status204NoContent)]
       [ProducesResponseType(StatusCodes.Status400BadRequest)]
       [ProducesResponseType(StatusCodes.Status404NotFound)]
       public async Task<IActionResult> UpdateWorkflow(
           Guid id,
           [FromBody] UpdateWorkflowDefinitionCommand command)
       {
           if (id != command.Id)
               return BadRequest(new { error = "ID mismatch" });

           var result = await _mediator.Send(command);

           if (result.IsSuccess)
               return NoContent();

           if (result.Error.Contains("not found"))
               return NotFound(new { error = result.Error });

           return BadRequest(new { error = result.Error });
       }

       [HttpDelete("{id}")]
       [ProducesResponseType(StatusCodes.Status204NoContent)]
       [ProducesResponseType(StatusCodes.Status404NotFound)]
       public async Task<IActionResult> DeleteWorkflow(Guid id)
       {
           var command = new DeleteWorkflowDefinitionCommand(id);
           var result = await _mediator.Send(command);

           if (result.IsSuccess)
               return NoContent();

           return NotFound(new { error = result.Error });
       }
   }
   ```

5. **Database Migrations**
   ```csharp
   // 20260421_AddWorkflowDefinition.cs
   public partial class AddWorkflowDefinition : Migration
   {
       protected override void Up(MigrationBuilder migrationBuilder)
       {
           migrationBuilder.CreateTable(
               name: "WorkflowDefinitions",
               columns: table => new
               {
                   Id = table.Column<Guid>(nullable: false),
                   Name = table.Column<string>(maxLength: 200, nullable: false),
                   Description = table.Column<string>(maxLength: 1000, nullable: true),
                   Version = table.Column<int>(nullable: false, defaultValue: 1),
                   DefinitionJson = table.Column<string>(type: "jsonb", nullable: false),
                   Status = table.Column<int>(nullable: false, defaultValue: 0),
                   CreatedAt = table.Column<DateTime>(nullable: false),
                   UpdatedAt = table.Column<DateTime>(nullable: true),
                   IsDeleted = table.Column<bool>(nullable: false, defaultValue: false)
               },
               constraints: table =>
               {
                   table.PrimaryKey("PK_WorkflowDefinitions", x => x.Id);
               });

           migrationBuilder.CreateTable(
               name: "NodeDefinitions",
               columns: table => new
               {
                   Id = table.Column<Guid>(nullable: false),
                   WorkflowDefinitionId = table.Column<Guid>(nullable: false),
                   NodeId = table.Column<string>(maxLength: 100, nullable: false),
                   NodeType = table.Column<string>(maxLength: 50, nullable: false),
                   ConfigurationJson = table.Column<string>(type: "jsonb", nullable: false),
                   Position_X = table.Column<int>(nullable: false),
                   Position_Y = table.Column<int>(nullable: false)
               },
               constraints: table =>
               {
                   table.PrimaryKey("PK_NodeDefinitions", x => x.Id);
                   table.ForeignKey(
                       name: "FK_NodeDefinitions_WorkflowDefinitions",
                       column: x => x.WorkflowDefinitionId,
                       principalTable: "WorkflowDefinitions",
                       principalColumn: "Id",
                       onDelete: ReferentialAction.Cascade);
               });

           migrationBuilder.CreateTable(
               name: "EdgeDefinitions",
               columns: table => new
               {
                   Id = table.Column<Guid>(nullable: false),
                   WorkflowDefinitionId = table.Column<Guid>(nullable: false),
                   SourceNodeId = table.Column<string>(maxLength: 100, nullable: false),
                   TargetNodeId = table.Column<string>(maxLength: 100, nullable: false),
                   Condition = table.Column<string>(maxLength: 500, nullable: true)
               },
               constraints: table =>
               {
                   table.PrimaryKey("PK_EdgeDefinitions", x => x.Id);
                   table.ForeignKey(
                       name: "FK_EdgeDefinitions_WorkflowDefinitions",
                       column: x => x.WorkflowDefinitionId,
                       principalTable: "WorkflowDefinitions",
                       principalColumn: "Id",
                       onDelete: ReferentialAction.Cascade);
               });

           // Indexes
           migrationBuilder.CreateIndex(
               name: "IX_WorkflowDefinitions_Name",
               table: "WorkflowDefinitions",
               column: "Name");

           migrationBuilder.CreateIndex(
               name: "IX_WorkflowDefinitions_Status",
               table: "WorkflowDefinitions",
               column: "Status");

           migrationBuilder.CreateIndex(
               name: "IX_WorkflowDefinitions_CreatedAt",
               table: "WorkflowDefinitions",
               column: "CreatedAt");

           migrationBuilder.CreateIndex(
               name: "IX_NodeDefinitions_WorkflowDefinitionId",
               table: "NodeDefinitions",
               column: "WorkflowDefinitionId");

           migrationBuilder.CreateIndex(
               name: "IX_EdgeDefinitions_WorkflowDefinitionId",
               table: "EdgeDefinitions",
               column: "WorkflowDefinitionId");
       }

       protected override void Down(MigrationBuilder migrationBuilder)
       {
           migrationBuilder.DropTable(name: "EdgeDefinitions");
           migrationBuilder.DropTable(name: "NodeDefinitions");
           migrationBuilder.DropTable(name: "WorkflowDefinitions");
       }
   }
   ```

**驗收標準**:
- ✅ WorkflowDefinition 實體設計符合 DDD 原則
- ✅ Repository 實現完整 CRUD 操作
- ✅ CQRS Commands/Queries 正確實現
- ✅ API Controllers 遵循 RESTful 設計
- ✅ Database Migrations 可正常 up/down
- ✅ 5 個 API 端點全部正常運作
- ✅ Soft delete 支持
- ✅ 分頁查詢正常
- ✅ 完整 Swagger 註解

---

### Phase 2: Node Registry (2-3 SP)

**目標**: 建立 Node Type 註冊和查詢機制

**核心組件**:

1. **Node Type Registry Service** (Application Layer)
   ```csharp
   // INodeTypeRegistry.cs
   public interface INodeTypeRegistry
   {
       void RegisterNodeType(NodeTypeDefinition nodeType);
       NodeTypeDefinition? GetNodeType(string nodeType);
       IEnumerable<NodeTypeDefinition> GetAllNodeTypes();
       JsonSchema GetNodeSchema(string nodeType);
       bool IsNodeTypeRegistered(string nodeType);
   }

   // NodeTypeDefinition.cs
   public class NodeTypeDefinition
   {
       public string Type { get; set; } // e.g., "Agent", "Tool", "Condition"
       public string DisplayName { get; set; }
       public string Description { get; set; }
       public string Category { get; set; } // "Execution", "Control", "Data"
       public JsonSchema ConfigurationSchema { get; set; }
       public Dictionary<string, PortDefinition> InputPorts { get; set; }
       public Dictionary<string, PortDefinition> OutputPorts { get; set; }
       public string IconUrl { get; set; }
       public NodeTypeMetadata Metadata { get; set; }
   }

   public class PortDefinition
   {
       public string Name { get; set; }
       public string DisplayName { get; set; }
       public string DataType { get; set; } // string, number, object, array
       public bool Required { get; set; }
       public string? Description { get; set; }
   }

   public class NodeTypeMetadata
   {
       public string Version { get; set; }
       public string Author { get; set; }
       public List<string> Tags { get; set; }
       public bool IsDeprecated { get; set; }
   }

   // NodeTypeRegistry.cs
   public class NodeTypeRegistry : INodeTypeRegistry
   {
       private readonly Dictionary<string, NodeTypeDefinition> _registeredTypes;
       private readonly ILogger<NodeTypeRegistry> _logger;

       public NodeTypeRegistry(ILogger<NodeTypeRegistry> logger)
       {
           _logger = logger;
           _registeredTypes = new Dictionary<string, NodeTypeDefinition>(
               StringComparer.OrdinalIgnoreCase);

           RegisterBuiltInNodeTypes();
       }

       private void RegisterBuiltInNodeTypes()
       {
           // Agent Node
           RegisterNodeType(new NodeTypeDefinition
           {
               Type = "Agent",
               DisplayName = "Agent",
               Description = "Execute an AI agent",
               Category = "Execution",
               ConfigurationSchema = JsonSchema.FromType<AgentNodeConfiguration>(),
               InputPorts = new Dictionary<string, PortDefinition>
               {
                   ["input"] = new PortDefinition
                   {
                       Name = "input",
                       DisplayName = "Input",
                       DataType = "string",
                       Required = true,
                       Description = "Input message for the agent"
                   }
               },
               OutputPorts = new Dictionary<string, PortDefinition>
               {
                   ["output"] = new PortDefinition
                   {
                       Name = "output",
                       DisplayName = "Output",
                       DataType = "string",
                       Required = false,
                       Description = "Agent's response"
                   },
                   ["error"] = new PortDefinition
                   {
                       Name = "error",
                       DisplayName = "Error",
                       DataType = "object",
                       Required = false,
                       Description = "Error information if execution fails"
                   }
               },
               IconUrl = "/icons/agent.svg",
               Metadata = new NodeTypeMetadata
               {
                   Version = "1.0.0",
                   Author = "System",
                   Tags = new List<string> { "ai", "agent", "execution" },
                   IsDeprecated = false
               }
           });

           // Tool Node
           RegisterNodeType(new NodeTypeDefinition
           {
               Type = "Tool",
               DisplayName = "Tool",
               Description = "Execute a tool/plugin",
               Category = "Execution",
               ConfigurationSchema = JsonSchema.FromType<ToolNodeConfiguration>(),
               InputPorts = new Dictionary<string, PortDefinition>
               {
                   ["input"] = new PortDefinition
                   {
                       Name = "input",
                       DisplayName = "Input",
                       DataType = "object",
                       Required = true,
                       Description = "Tool input parameters"
                   }
               },
               OutputPorts = new Dictionary<string, PortDefinition>
               {
                   ["output"] = new PortDefinition
                   {
                       Name = "output",
                       DisplayName = "Output",
                       DataType = "object",
                       Required = false,
                       Description = "Tool execution result"
                   },
                   ["error"] = new PortDefinition
                   {
                       Name = "error",
                       DisplayName = "Error",
                       DataType = "object",
                       Required = false
                   }
               },
               IconUrl = "/icons/tool.svg",
               Metadata = new NodeTypeMetadata
               {
                   Version = "1.0.0",
                   Author = "System",
                   Tags = new List<string> { "tool", "plugin", "execution" }
               }
           });

           // Condition Node
           RegisterNodeType(new NodeTypeDefinition
           {
               Type = "Condition",
               DisplayName = "Condition",
               Description = "Conditional branching",
               Category = "Control",
               ConfigurationSchema = JsonSchema.FromType<ConditionNodeConfiguration>(),
               InputPorts = new Dictionary<string, PortDefinition>
               {
                   ["input"] = new PortDefinition
                   {
                       Name = "input",
                       DisplayName = "Input",
                       DataType = "any",
                       Required = true
                   }
               },
               OutputPorts = new Dictionary<string, PortDefinition>
               {
                   ["true"] = new PortDefinition
                   {
                       Name = "true",
                       DisplayName = "True",
                       DataType = "any",
                       Required = false,
                       Description = "Condition evaluates to true"
                   },
                   ["false"] = new PortDefinition
                   {
                       Name = "false",
                       DisplayName = "False",
                       DataType = "any",
                       Required = false,
                       Description = "Condition evaluates to false"
                   }
               },
               IconUrl = "/icons/condition.svg",
               Metadata = new NodeTypeMetadata
               {
                   Version = "1.0.0",
                   Author = "System",
                   Tags = new List<string> { "control", "condition", "branching" }
               }
           });

           // Parallel Node
           RegisterNodeType(new NodeTypeDefinition
           {
               Type = "Parallel",
               DisplayName = "Parallel",
               Description = "Execute multiple nodes in parallel",
               Category = "Control",
               ConfigurationSchema = JsonSchema.FromType<ParallelNodeConfiguration>(),
               InputPorts = new Dictionary<string, PortDefinition>
               {
                   ["input"] = new PortDefinition
                   {
                       Name = "input",
                       DisplayName = "Input",
                       DataType = "any",
                       Required = true
                   }
               },
               OutputPorts = new Dictionary<string, PortDefinition>
               {
                   ["output"] = new PortDefinition
                   {
                       Name = "output",
                       DisplayName = "Output",
                       DataType = "array",
                       Required = false,
                       Description = "Array of results from parallel executions"
                   }
               },
               IconUrl = "/icons/parallel.svg",
               Metadata = new NodeTypeMetadata
               {
                   Version = "1.0.0",
                   Author = "System",
                   Tags = new List<string> { "control", "parallel", "concurrent" }
               }
           });

           // Sequential Node
           RegisterNodeType(new NodeTypeDefinition
           {
               Type = "Sequential",
               DisplayName = "Sequential",
               Description = "Execute multiple nodes in sequence",
               Category = "Control",
               ConfigurationSchema = JsonSchema.FromType<SequentialNodeConfiguration>(),
               InputPorts = new Dictionary<string, PortDefinition>
               {
                   ["input"] = new PortDefinition
                   {
                       Name = "input",
                       DisplayName = "Input",
                       DataType = "any",
                       Required = true
                   }
               },
               OutputPorts = new Dictionary<string, PortDefinition>
               {
                   ["output"] = new PortDefinition
                   {
                       Name = "output",
                       DisplayName = "Output",
                       DataType = "any",
                       Required = false,
                       Description = "Result from last node in sequence"
                   }
               },
               IconUrl = "/icons/sequential.svg",
               Metadata = new NodeTypeMetadata
               {
                   Version = "1.0.0",
                   Author = "System",
                   Tags = new List<string> { "control", "sequential", "series" }
               }
           });

           _logger.LogInformation("Registered {Count} built-in node types",
               _registeredTypes.Count);
       }

       public void RegisterNodeType(NodeTypeDefinition nodeType)
       {
           if (string.IsNullOrEmpty(nodeType.Type))
               throw new ArgumentException("Node type cannot be empty");

           if (_registeredTypes.ContainsKey(nodeType.Type))
           {
               _logger.LogWarning("Node type {Type} is already registered", nodeType.Type);
               return;
           }

           _registeredTypes[nodeType.Type] = nodeType;
           _logger.LogInformation("Registered node type: {Type}", nodeType.Type);
       }

       public NodeTypeDefinition? GetNodeType(string nodeType)
       {
           return _registeredTypes.GetValueOrDefault(nodeType);
       }

       public IEnumerable<NodeTypeDefinition> GetAllNodeTypes()
       {
           return _registeredTypes.Values.OrderBy(n => n.Category).ThenBy(n => n.Type);
       }

       public JsonSchema GetNodeSchema(string nodeType)
       {
           var nodeDef = GetNodeType(nodeType);
           if (nodeDef == null)
               throw new ArgumentException($"Node type '{nodeType}' not found");

           return nodeDef.ConfigurationSchema;
       }

       public bool IsNodeTypeRegistered(string nodeType)
       {
           return _registeredTypes.ContainsKey(nodeType);
       }
   }
   ```

2. **Node Schema Validation** (Application Layer)
   ```csharp
   // NodeConfigurationValidator.cs
   public class NodeConfigurationValidator : INodeConfigurationValidator
   {
       private readonly INodeTypeRegistry _nodeTypeRegistry;
       private readonly ILogger<NodeConfigurationValidator> _logger;

       public ValidationResult ValidateNodeConfiguration(
           string nodeType,
           string configurationJson)
       {
           var nodeTypeDef = _nodeTypeRegistry.GetNodeType(nodeType);
           if (nodeTypeDef == null)
           {
               return ValidationResult.Failure(
                   $"Node type '{nodeType}' is not registered");
           }

           try
           {
               var config = JsonDocument.Parse(configurationJson);
               var schema = nodeTypeDef.ConfigurationSchema;

               var validator = new JsonSchemaValidator();
               var validationResult = validator.Validate(config, schema);

               if (!validationResult.IsValid)
               {
                   var errors = validationResult.Errors
                       .Select(e => $"{e.Path}: {e.Message}")
                       .ToList();

                   return ValidationResult.Failure(errors);
               }

               return ValidationResult.Success();
           }
           catch (JsonException ex)
           {
               _logger.LogError(ex, "Invalid JSON format");
               return ValidationResult.Failure("Invalid JSON format");
           }
       }
   }
   ```

3. **Node Registry API** (Presentation Layer)
   ```csharp
   // NodeTypesController.cs
   [ApiController]
   [Route("api/v1/node-types")]
   [Produces("application/json")]
   public class NodeTypesController : ControllerBase
   {
       private readonly INodeTypeRegistry _nodeTypeRegistry;
       private readonly ILogger<NodeTypesController> _logger;

       [HttpGet]
       [ProducesResponseType(typeof(IEnumerable<NodeTypeDto>), StatusCodes.Status200OK)]
       public IActionResult GetAllNodeTypes([FromQuery] string? category = null)
       {
           var nodeTypes = _nodeTypeRegistry.GetAllNodeTypes();

           if (!string.IsNullOrEmpty(category))
           {
               nodeTypes = nodeTypes.Where(n =>
                   n.Category.Equals(category, StringComparison.OrdinalIgnoreCase));
           }

           var dtos = nodeTypes.Select(n => new NodeTypeDto
           {
               Type = n.Type,
               DisplayName = n.DisplayName,
               Description = n.Description,
               Category = n.Category,
               InputPorts = n.InputPorts,
               OutputPorts = n.OutputPorts,
               IconUrl = n.IconUrl,
               Metadata = n.Metadata
           });

           return Ok(dtos);
       }

       [HttpGet("{type}")]
       [ProducesResponseType(typeof(NodeTypeDetailDto), StatusCodes.Status200OK)]
       [ProducesResponseType(StatusCodes.Status404NotFound)]
       public IActionResult GetNodeType(string type)
       {
           var nodeType = _nodeTypeRegistry.GetNodeType(type);

           if (nodeType == null)
               return NotFound(new { error = $"Node type '{type}' not found" });

           var dto = new NodeTypeDetailDto
           {
               Type = nodeType.Type,
               DisplayName = nodeType.DisplayName,
               Description = nodeType.Description,
               Category = nodeType.Category,
               ConfigurationSchema = nodeType.ConfigurationSchema,
               InputPorts = nodeType.InputPorts,
               OutputPorts = nodeType.OutputPorts,
               IconUrl = nodeType.IconUrl,
               Metadata = nodeType.Metadata
           };

           return Ok(dto);
       }

       [HttpGet("{type}/schema")]
       [ProducesResponseType(typeof(JsonSchema), StatusCodes.Status200OK)]
       [ProducesResponseType(StatusCodes.Status404NotFound)]
       public IActionResult GetNodeSchema(string type)
       {
           try
           {
               var schema = _nodeTypeRegistry.GetNodeSchema(type);
               return Ok(schema);
           }
           catch (ArgumentException ex)
           {
               return NotFound(new { error = ex.Message });
           }
       }

       [HttpPost("validate")]
       [ProducesResponseType(typeof(ValidationResultDto), StatusCodes.Status200OK)]
       [ProducesResponseType(StatusCodes.Status400BadRequest)]
       public IActionResult ValidateNodeConfiguration(
           [FromBody] ValidateNodeConfigurationRequest request)
       {
           var validator = new NodeConfigurationValidator(_nodeTypeRegistry, _logger);
           var result = validator.ValidateNodeConfiguration(
               request.NodeType,
               request.ConfigurationJson);

           if (result.IsValid)
           {
               return Ok(new ValidationResultDto
               {
                   IsValid = true,
                   Message = "Node configuration is valid"
               });
           }

           return Ok(new ValidationResultDto
           {
               IsValid = false,
               Errors = result.Errors
           });
       }
   }
   ```

**驗收標準**:
- ✅ 5 種基礎 Node types 註冊 (Agent, Tool, Condition, Parallel, Sequential)
- ✅ 每種 Node type 有完整 JSON Schema 定義
- ✅ Node configuration 驗證正常
- ✅ 3 個 API 端點正常運作
- ✅ 支援按 Category 過濾
- ✅ 完整 Swagger 註解
- ✅ 單元測試覆蓋

---

### Phase 3: Edge Validation (2-3 SP)

**目標**: 建立 Edge Connection 驗證邏輯

**核心組件**:

1. **Edge Validation Service** (Application Layer)
   ```csharp
   // IEdgeValidationService.cs
   public interface IEdgeValidationService
   {
       Task<ValidationResult> ValidateEdgeAsync(
           string sourceNodeId,
           string targetNodeId,
           WorkflowDefinition workflow);

       Task<ValidationResult> ValidateWorkflowAsync(WorkflowDefinition workflow);

       Task<CircularDependencyCheckResult> DetectCircularDependencyAsync(
           WorkflowDefinition workflow);
   }

   // EdgeValidationService.cs
   public class EdgeValidationService : IEdgeValidationService
   {
       private readonly INodeTypeRegistry _nodeTypeRegistry;
       private readonly INodeCompatibilityMatrix _compatibilityMatrix;
       private readonly ILogger<EdgeValidationService> _logger;

       public async Task<ValidationResult> ValidateEdgeAsync(
           string sourceNodeId,
           string targetNodeId,
           WorkflowDefinition workflow)
       {
           var sourceNode = workflow.Nodes.FirstOrDefault(n => n.NodeId == sourceNodeId);
           var targetNode = workflow.Nodes.FirstOrDefault(n => n.NodeId == targetNodeId);

           if (sourceNode == null)
               return ValidationResult.Failure($"Source node '{sourceNodeId}' not found");

           if (targetNode == null)
               return ValidationResult.Failure($"Target node '{targetNodeId}' not found");

           // Check node compatibility
           var isCompatible = await _compatibilityMatrix.AreNodesCompatibleAsync(
               sourceNode.NodeType, targetNode.NodeType);

           if (!isCompatible)
           {
               return ValidationResult.Failure(
                   $"Node types '{sourceNode.NodeType}' and '{targetNode.NodeType}' are not compatible");
           }

           // Check if edge would create circular dependency
           var edges = workflow.Edges.ToList();
           edges.Add(new EdgeDefinition
           {
               SourceNodeId = sourceNodeId,
               TargetNodeId = targetNodeId
           });

           var circularCheck = await DetectCircularDependencyAsync(workflow);
           if (circularCheck.HasCircularDependency)
           {
               return ValidationResult.Failure(
                   $"This edge would create a circular dependency: {circularCheck.CyclePath}");
           }

           return ValidationResult.Success();
       }

       public async Task<ValidationResult> ValidateWorkflowAsync(
           WorkflowDefinition workflow)
       {
           var errors = new List<string>();

           // Validate all edges
           foreach (var edge in workflow.Edges)
           {
               var result = await ValidateEdgeAsync(
                   edge.SourceNodeId, edge.TargetNodeId, workflow);

               if (!result.IsValid)
                   errors.AddRange(result.Errors);
           }

           // Check for circular dependencies
           var circularCheck = await DetectCircularDependencyAsync(workflow);
           if (circularCheck.HasCircularDependency)
           {
               errors.Add($"Circular dependency detected: {circularCheck.CyclePath}");
           }

           // Check for disconnected nodes
           var disconnectedNodes = FindDisconnectedNodes(workflow);
           if (disconnectedNodes.Any())
           {
               errors.Add($"Disconnected nodes found: {string.Join(", ", disconnectedNodes)}");
           }

           if (errors.Any())
               return ValidationResult.Failure(errors);

           return ValidationResult.Success();
       }

       public async Task<CircularDependencyCheckResult> DetectCircularDependencyAsync(
           WorkflowDefinition workflow)
       {
           // Build adjacency list
           var graph = new Dictionary<string, List<string>>();
           foreach (var node in workflow.Nodes)
           {
               graph[node.NodeId] = new List<string>();
           }

           foreach (var edge in workflow.Edges)
           {
               if (graph.ContainsKey(edge.SourceNodeId))
                   graph[edge.SourceNodeId].Add(edge.TargetNodeId);
           }

           // DFS-based cycle detection
           var visited = new HashSet<string>();
           var recStack = new HashSet<string>();
           var path = new Stack<string>();

           foreach (var nodeId in graph.Keys)
           {
               if (!visited.Contains(nodeId))
               {
                   if (HasCycle(nodeId, graph, visited, recStack, path, out var cyclePath))
                   {
                       return new CircularDependencyCheckResult
                       {
                           HasCircularDependency = true,
                           CyclePath = cyclePath
                       };
                   }
               }
           }

           return new CircularDependencyCheckResult
           {
               HasCircularDependency = false
           };
       }

       private bool HasCycle(
           string nodeId,
           Dictionary<string, List<string>> graph,
           HashSet<string> visited,
           HashSet<string> recStack,
           Stack<string> path,
           out string cyclePath)
       {
           visited.Add(nodeId);
           recStack.Add(nodeId);
           path.Push(nodeId);

           foreach (var neighbor in graph[nodeId])
           {
               if (!visited.Contains(neighbor))
               {
                   if (HasCycle(neighbor, graph, visited, recStack, path, out cyclePath))
                       return true;
               }
               else if (recStack.Contains(neighbor))
               {
                   // Cycle detected
                   var cycleNodes = new List<string> { neighbor };
                   while (path.Count > 0)
                   {
                       var node = path.Pop();
                       cycleNodes.Add(node);
                       if (node == neighbor)
                           break;
                   }
                   cycleNodes.Reverse();
                   cyclePath = string.Join(" → ", cycleNodes);
                   return true;
               }
           }

           recStack.Remove(nodeId);
           path.Pop();
           cyclePath = string.Empty;
           return false;
       }

       private List<string> FindDisconnectedNodes(WorkflowDefinition workflow)
       {
           var connectedNodes = new HashSet<string>();

           foreach (var edge in workflow.Edges)
           {
               connectedNodes.Add(edge.SourceNodeId);
               connectedNodes.Add(edge.TargetNodeId);
           }

           return workflow.Nodes
               .Where(n => !connectedNodes.Contains(n.NodeId))
               .Select(n => n.NodeId)
               .ToList();
       }
   }
   ```

2. **Node Compatibility Matrix** (Domain Layer)
   ```csharp
   // INodeCompatibilityMatrix.cs
   public interface INodeCompatibilityMatrix
   {
       Task<bool> AreNodesCompatibleAsync(string sourceType, string targetType);
       IEnumerable<string> GetCompatibleTargets(string sourceType);
   }

   // NodeCompatibilityMatrix.cs
   public class NodeCompatibilityMatrix : INodeCompatibilityMatrix
   {
       private readonly Dictionary<string, HashSet<string>> _compatibilityRules;

       public NodeCompatibilityMatrix()
       {
           _compatibilityRules = new Dictionary<string, HashSet<string>>(
               StringComparer.OrdinalIgnoreCase);

           InitializeCompatibilityRules();
       }

       private void InitializeCompatibilityRules()
       {
           // Agent can connect to: Tool, Condition, Agent, Sequential, Parallel
           _compatibilityRules["Agent"] = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
           {
               "Tool", "Condition", "Agent", "Sequential", "Parallel"
           };

           // Tool can connect to: Agent, Condition, Tool, Sequential, Parallel
           _compatibilityRules["Tool"] = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
           {
               "Agent", "Condition", "Tool", "Sequential", "Parallel"
           };

           // Condition can connect to: any (true/false branches)
           _compatibilityRules["Condition"] = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
           {
               "Agent", "Tool", "Condition", "Sequential", "Parallel"
           };

           // Sequential can connect to: any
           _compatibilityRules["Sequential"] = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
           {
               "Agent", "Tool", "Condition", "Sequential", "Parallel"
           };

           // Parallel can connect to: any
           _compatibilityRules["Parallel"] = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
           {
               "Agent", "Tool", "Condition", "Sequential", "Parallel"
           };
       }

       public Task<bool> AreNodesCompatibleAsync(string sourceType, string targetType)
       {
           if (!_compatibilityRules.ContainsKey(sourceType))
               return Task.FromResult(false);

           return Task.FromResult(_compatibilityRules[sourceType].Contains(targetType));
       }

       public IEnumerable<string> GetCompatibleTargets(string sourceType)
       {
           return _compatibilityRules.GetValueOrDefault(sourceType, new HashSet<string>());
       }
   }
   ```

3. **Edge Validation API** (Presentation Layer)
   ```csharp
   // WorkflowValidationController.cs
   [ApiController]
   [Route("api/v1/workflow-validation")]
   [Produces("application/json")]
   public class WorkflowValidationController : ControllerBase
   {
       private readonly IEdgeValidationService _edgeValidationService;
       private readonly IWorkflowDefinitionRepository _workflowRepository;
       private readonly ILogger<WorkflowValidationController> _logger;

       [HttpPost("edge")]
       [ProducesResponseType(typeof(ValidationResultDto), StatusCodes.Status200OK)]
       [ProducesResponseType(StatusCodes.Status400BadRequest)]
       public async Task<IActionResult> ValidateEdge(
           [FromBody] ValidateEdgeRequest request)
       {
           var workflow = await _workflowRepository.GetByIdWithDetailsAsync(
               request.WorkflowId);

           if (workflow == null)
               return BadRequest(new { error = "Workflow not found" });

           var result = await _edgeValidationService.ValidateEdgeAsync(
               request.SourceNodeId, request.TargetNodeId, workflow);

           return Ok(new ValidationResultDto
           {
               IsValid = result.IsValid,
               Errors = result.Errors,
               Message = result.IsValid ? "Edge is valid" : "Edge validation failed"
           });
       }

       [HttpPost("workflow")]
       [ProducesResponseType(typeof(WorkflowValidationResultDto), StatusCodes.Status200OK)]
       [ProducesResponseType(StatusCodes.Status400BadRequest)]
       public async Task<IActionResult> ValidateWorkflow(
           [FromBody] ValidateWorkflowRequest request)
       {
           var workflow = await _workflowRepository.GetByIdWithDetailsAsync(
               request.WorkflowId);

           if (workflow == null)
               return BadRequest(new { error = "Workflow not found" });

           var result = await _edgeValidationService.ValidateWorkflowAsync(workflow);
           var circularCheck = await _edgeValidationService.DetectCircularDependencyAsync(workflow);

           return Ok(new WorkflowValidationResultDto
           {
               IsValid = result.IsValid,
               Errors = result.Errors,
               HasCircularDependency = circularCheck.HasCircularDependency,
               CyclePath = circularCheck.CyclePath,
               Message = result.IsValid ? "Workflow is valid" : "Workflow validation failed"
           });
       }

       [HttpGet("compatibility/{sourceType}")]
       [ProducesResponseType(typeof(IEnumerable<string>), StatusCodes.Status200OK)]
       public IActionResult GetCompatibleTargets(string sourceType)
       {
           var matrix = new NodeCompatibilityMatrix();
           var compatibleTargets = matrix.GetCompatibleTargets(sourceType);

           return Ok(compatibleTargets);
       }
   }
   ```

**驗收標準**:
- ✅ Edge connection rules 完整實現
- ✅ Circular dependency 檢測正確 (DFS algorithm)
- ✅ Disconnected nodes 檢測
- ✅ Node compatibility 驗證
- ✅ 詳細錯誤訊息
- ✅ 3 個 API 端點正常運作
- ✅ 完整 Swagger 註解
- ✅ 單元測試覆蓋 (包含 edge cases)

---

### Phase 4: 整合測試與文檔 (1-2 SP)

**目標**: 完成整合測試和 API 文檔

**核心組件**:

1. **整合測試** (Tests Layer)
   ```csharp
   // WorkflowDefinitionsControllerTests.cs
   public class WorkflowDefinitionsControllerTests : IntegrationTestBase
   {
       [Fact]
       public async Task CreateWorkflow_ShouldReturnCreatedWorkflow()
       {
           // Arrange
           var command = new CreateWorkflowDefinitionCommand
           {
               Name = "Test Workflow",
               Description = "Test description",
               DefinitionJson = "{\"version\": \"1.0\"}",
               Nodes = new List<NodeDefinitionDto>
               {
                   new()
                   {
                       NodeId = "node1",
                       NodeType = "Agent",
                       ConfigurationJson = "{\"agentId\": \"agent-1\"}"
                   }
               },
               Edges = new List<EdgeDefinitionDto>()
           };

           // Act
           var response = await _client.PostAsJsonAsync(
               "/api/v1/workflow-definitions", command);

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.Created);
           var result = await response.Content.ReadFromJsonAsync<CreateWorkflowResponse>();
           result.Id.Should().NotBeEmpty();
       }

       [Fact]
       public async Task GetWorkflow_ShouldReturnWorkflowWithDetails()
       {
           // Arrange
           var workflowId = await CreateTestWorkflowAsync();

           // Act
           var response = await _client.GetAsync(
               $"/api/v1/workflow-definitions/{workflowId}");

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.OK);
           var result = await response.Content.ReadFromJsonAsync<WorkflowDefinitionDto>();
           result.Id.Should().Be(workflowId);
           result.Nodes.Should().NotBeEmpty();
       }

       [Fact]
       public async Task UpdateWorkflow_ShouldUpdateWorkflow()
       {
           // Arrange
           var workflowId = await CreateTestWorkflowAsync();
           var command = new UpdateWorkflowDefinitionCommand
           {
               Id = workflowId,
               Name = "Updated Workflow",
               Description = "Updated description"
           };

           // Act
           var response = await _client.PutAsJsonAsync(
               $"/api/v1/workflow-definitions/{workflowId}", command);

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.NoContent);
       }

       [Fact]
       public async Task DeleteWorkflow_ShouldSoftDeleteWorkflow()
       {
           // Arrange
           var workflowId = await CreateTestWorkflowAsync();

           // Act
           var response = await _client.DeleteAsync(
               $"/api/v1/workflow-definitions/{workflowId}");

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.NoContent);

           // Verify soft delete
           var getResponse = await _client.GetAsync(
               $"/api/v1/workflow-definitions/{workflowId}");
           getResponse.StatusCode.Should().Be(HttpStatusCode.NotFound);
       }
   }

   // NodeTypesControllerTests.cs
   public class NodeTypesControllerTests : IntegrationTestBase
   {
       [Fact]
       public async Task GetAllNodeTypes_ShouldReturnAllRegisteredTypes()
       {
           // Act
           var response = await _client.GetAsync("/api/v1/node-types");

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.OK);
           var result = await response.Content.ReadFromJsonAsync<List<NodeTypeDto>>();
           result.Should().NotBeEmpty();
           result.Should().Contain(n => n.Type == "Agent");
           result.Should().Contain(n => n.Type == "Tool");
       }

       [Fact]
       public async Task GetNodeType_ShouldReturnNodeTypeDetails()
       {
           // Act
           var response = await _client.GetAsync("/api/v1/node-types/Agent");

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.OK);
           var result = await response.Content.ReadFromJsonAsync<NodeTypeDetailDto>();
           result.Type.Should().Be("Agent");
           result.ConfigurationSchema.Should().NotBeNull();
       }

       [Fact]
       public async Task ValidateNodeConfiguration_ShouldValidateConfiguration()
       {
           // Arrange
           var request = new ValidateNodeConfigurationRequest
           {
               NodeType = "Agent",
               ConfigurationJson = "{\"agentId\": \"agent-1\"}"
           };

           // Act
           var response = await _client.PostAsJsonAsync(
               "/api/v1/node-types/validate", request);

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.OK);
           var result = await response.Content.ReadFromJsonAsync<ValidationResultDto>();
           result.IsValid.Should().BeTrue();
       }
   }

   // WorkflowValidationControllerTests.cs
   public class WorkflowValidationControllerTests : IntegrationTestBase
   {
       [Fact]
       public async Task ValidateEdge_ShouldReturnValidForCompatibleNodes()
       {
           // Arrange
           var workflowId = await CreateTestWorkflowAsync();
           var request = new ValidateEdgeRequest
           {
               WorkflowId = workflowId,
               SourceNodeId = "node1",
               TargetNodeId = "node2"
           };

           // Act
           var response = await _client.PostAsJsonAsync(
               "/api/v1/workflow-validation/edge", request);

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.OK);
           var result = await response.Content.ReadFromJsonAsync<ValidationResultDto>();
           result.IsValid.Should().BeTrue();
       }

       [Fact]
       public async Task ValidateWorkflow_ShouldDetectCircularDependency()
       {
           // Arrange
           var workflowId = await CreateWorkflowWithCircularDependencyAsync();
           var request = new ValidateWorkflowRequest { WorkflowId = workflowId };

           // Act
           var response = await _client.PostAsJsonAsync(
               "/api/v1/workflow-validation/workflow", request);

           // Assert
           response.StatusCode.Should().Be(HttpStatusCode.OK);
           var result = await response.Content.ReadFromJsonAsync<WorkflowValidationResultDto>();
           result.HasCircularDependency.Should().BeTrue();
           result.CyclePath.Should().NotBeNullOrEmpty();
       }
   }
   ```

2. **API 文檔** (Documentation)
   ```markdown
   # Workflow Editor API v1.0

   ## Overview

   The Workflow Editor API provides endpoints for managing workflow definitions,
   node types, and workflow validation.

   ## Base URL
   ```
   https://api.example.com/api/v1
   ```

   ## Authentication

   All API requests require authentication using JWT tokens:
   ```
   Authorization: Bearer <token>
   ```

   ## API Endpoints

   ### Workflow Definitions

   #### Create Workflow
   ```http
   POST /workflow-definitions
   Content-Type: application/json

   {
     "name": "My Workflow",
     "description": "Workflow description",
     "definitionJson": "{\"version\": \"1.0\"}",
     "nodes": [
       {
         "nodeId": "node1",
         "nodeType": "Agent",
         "configurationJson": "{\"agentId\": \"agent-1\"}",
         "position": { "x": 100, "y": 100 }
       }
     ],
     "edges": []
   }
   ```

   **Response (201 Created)**:
   ```json
   {
     "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
   }
   ```

   #### Get Workflows (Paginated)
   ```http
   GET /workflow-definitions?pageNumber=1&pageSize=20&searchTerm=test
   ```

   **Response (200 OK)**:
   ```json
   {
     "items": [
       {
         "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
         "name": "My Workflow",
         "description": "Workflow description",
         "version": 1,
         "status": "Draft",
         "createdAt": "2026-04-21T10:00:00Z"
       }
     ],
     "totalCount": 100,
     "pageNumber": 1,
     "pageSize": 20
   }
   ```

   ### Node Types

   #### Get All Node Types
   ```http
   GET /node-types?category=Execution
   ```

   **Response (200 OK)**:
   ```json
   [
     {
       "type": "Agent",
       "displayName": "Agent",
       "description": "Execute an AI agent",
       "category": "Execution",
       "iconUrl": "/icons/agent.svg"
     }
   ]
   ```

   ### Workflow Validation

   #### Validate Edge
   ```http
   POST /workflow-validation/edge
   Content-Type: application/json

   {
     "workflowId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
     "sourceNodeId": "node1",
     "targetNodeId": "node2"
   }
   ```

   **Response (200 OK)**:
   ```json
   {
     "isValid": true,
     "message": "Edge is valid"
   }
   ```

   ## Error Codes

   | Code | Description |
   |------|-------------|
   | 400 | Bad Request - Invalid input |
   | 401 | Unauthorized - Missing or invalid token |
   | 404 | Not Found - Resource not found |
   | 500 | Internal Server Error |

   ## Rate Limiting

   API requests are limited to 100 requests per minute per user.
   ```

3. **Sprint Retrospective Document**
   - 將在 SPRINT-10-7-RETROSPECTIVE.md 完成

**驗收標準**:
- ✅ 覆蓋所有 API 端點的整合測試
- ✅ Happy path + Error cases 測試
- ✅ 使用 TestContainers (PostgreSQL)
- ✅ API 文檔清晰易懂
- ✅ 包含完整範例 (cURL, C#, TypeScript)
- ✅ 錯誤代碼說明完整

---

## 第三部分: 如何實施

### 詳細實作指南 - Workflow Definition CRUD

**Step 1: Domain Model 設計**

1. 建立 WorkflowDefinition 實體
   ```bash
   # 位置
   src/Core/Domain/Entities/Workflows/WorkflowDefinition.cs
   src/Core/Domain/Entities/Workflows/NodeDefinition.cs
   src/Core/Domain/Entities/Workflows/EdgeDefinition.cs
   ```

2. 實現要點:
   - 遵循 DDD 原則 (Aggregate Root, Value Objects)
   - 包含 Domain Events (WorkflowDefinitionCreated, etc.)
   - Soft Delete 支持 (IsDeleted 欄位)
   - Version 欄位 (用於版本控制)
   - JSON 欄位 (DefinitionJson, ConfigurationJson) 使用 PostgreSQL jsonb 類型

**Step 2: Repository 實現**

1. 建立 Repository 介面和實現
   ```bash
   # 位置
   src/Core/Application/Interfaces/IWorkflowDefinitionRepository.cs
   src/Infrastructure/Persistence/Repositories/WorkflowDefinitionRepository.cs
   ```

2. 實現要點:
   - 實現 IRepository<WorkflowDefinition> 基本方法
   - 包含 GetByIdWithDetailsAsync (使用 Include 載入 Nodes 和 Edges)
   - 實現分頁查詢 (PagedResult)
   - 實現搜尋功能 (Name, Description)
   - 實現 Soft Delete (查詢時過濾 IsDeleted)

**Step 3: CQRS Commands/Queries**

1. 建立 Commands
   ```bash
   # 位置
   src/Application/UseCases/Workflows/Commands/CreateWorkflowDefinitionCommand.cs
   src/Application/UseCases/Workflows/Commands/UpdateWorkflowDefinitionCommand.cs
   src/Application/UseCases/Workflows/Commands/DeleteWorkflowDefinitionCommand.cs
   ```

2. 建立 Queries
   ```bash
   # 位置
   src/Application/UseCases/Workflows/Queries/GetWorkflowDefinitionByIdQuery.cs
   src/Application/UseCases/Workflows/Queries/GetWorkflowDefinitionsQuery.cs
   ```

3. 實現要點:
   - 使用 MediatR IRequest<Result<T>>
   - 實現 FluentValidation Validators
   - Handler 使用 Repository 和 UnitOfWork
   - 錯誤處理和日誌記錄

**Step 4: API Controllers**

1. 建立 Controller
   ```bash
   # 位置
   src/API/Controllers/WorkflowDefinitionsController.cs
   ```

2. 實現要點:
   - 遵循 RESTful API 設計原則
   - 使用 [ApiController] 和 [Route] attributes
   - 使用 [ProducesResponseType] 定義響應類型
   - 實現 5 個端點 (POST, GET, GET by ID, PUT, DELETE)
   - 使用 MediatR 發送 Commands/Queries
   - 標準化錯誤處理

**Step 5: Database Migrations**

1. 建立 Migration
   ```bash
   cd src/AIAgentPlatform.API
   dotnet ef migrations add AddWorkflowDefinition --project ../Infrastructure
   ```

2. 實現要點:
   - WorkflowDefinitions 表 (主表)
   - NodeDefinitions 表 (1對多關係)
   - EdgeDefinitions 表 (1對多關係)
   - 索引優化 (Name, Status, CreatedAt)
   - JSONB 欄位 (PostgreSQL)

3. 應用 Migration
   ```bash
   dotnet ef database update
   ```

**測試要點**:
- 單元測試: Commands/Queries Handlers
- 整合測試: API 端點 (使用 TestContainers)
- 驗證: Soft Delete, 分頁, 搜尋

---

### 詳細實作指南 - Node Registry

**Step 1: Node Type Registry Service 設計**

1. 建立 INodeTypeRegistry 介面和實現
   ```bash
   # 位置
   src/Core/Application/Interfaces/INodeTypeRegistry.cs
   src/Application/Services/NodeTypeRegistry.cs
   src/Core/Domain/Models/NodeTypeDefinition.cs
   ```

2. 實現要點:
   - 使用 Dictionary<string, NodeTypeDefinition> 儲存註冊的 Node types
   - 在建構函數中註冊 5 種內建 Node types
   - 實現 RegisterNodeType, GetNodeType, GetAllNodeTypes 方法
   - 支援動態註冊 (未來擴展)

**Step 2: Node Schema 定義**

1. 建立 Node Configuration 類別
   ```bash
   # 位置
   src/Core/Domain/Models/NodeConfigurations/AgentNodeConfiguration.cs
   src/Core/Domain/Models/NodeConfigurations/ToolNodeConfiguration.cs
   src/Core/Domain/Models/NodeConfigurations/ConditionNodeConfiguration.cs
   # ... 其他 Node types
   ```

2. 實現要點:
   - 每種 Node type 有對應的 Configuration 類別
   - 使用 Data Annotations 或 FluentValidation 定義驗證規則
   - 使用 JSON Schema 庫生成 JSON Schema

**Step 3: Node Configuration Validation**

1. 建立 NodeConfigurationValidator
   ```bash
   # 位置
   src/Application/Validators/NodeConfigurationValidator.cs
   ```

2. 實現要點:
   - 使用 JSON Schema 驗證 configuration JSON
   - 提供詳細的錯誤訊息 (欄位路徑 + 錯誤描述)
   - 支援 JSON Schema Draft 7 標準

**Step 4: Node Registry API**

1. 建立 Controller
   ```bash
   # 位置
   src/API/Controllers/NodeTypesController.cs
   ```

2. 實現要點:
   - GET /api/v1/node-types (取得所有 Node types)
   - GET /api/v1/node-types/{type} (取得特定 Node type)
   - GET /api/v1/node-types/{type}/schema (取得 JSON Schema)
   - POST /api/v1/node-types/validate (驗證 Node configuration)
   - 支援按 Category 過濾

**測試要點**:
- 單元測試: NodeTypeRegistry, NodeConfigurationValidator
- 整合測試: Node Registry API 端點
- 驗證: JSON Schema validation 正確性

---

### 詳細實作指南 - Edge Validation

**Step 1: Edge Validation Service 設計**

1. 建立 IEdgeValidationService 介面和實現
   ```bash
   # 位置
   src/Core/Application/Interfaces/IEdgeValidationService.cs
   src/Application/Services/EdgeValidationService.cs
   ```

2. 實現要點:
   - ValidateEdgeAsync: 驗證單一 Edge connection
   - ValidateWorkflowAsync: 驗證完整 Workflow
   - DetectCircularDependencyAsync: 檢測 circular dependency (DFS 算法)
   - FindDisconnectedNodes: 檢測未連接的 Nodes

**Step 2: Node Compatibility Matrix**

1. 建立 INodeCompatibilityMatrix 介面和實現
   ```bash
   # 位置
   src/Core/Domain/Models/NodeCompatibilityMatrix.cs
   ```

2. 實現要點:
   - 使用 Dictionary<string, HashSet<string>> 定義 compatibility rules
   - 在建構函數中初始化 compatibility rules
   - AreNodesCompatibleAsync: 檢查兩個 Node types 是否相容
   - GetCompatibleTargets: 取得特定 Node type 的所有相容目標

**Step 3: Circular Dependency Detection**

1. 實現 DFS 算法
   - 建立 adjacency list (鄰接表) 表示 workflow graph
   - 使用 DFS 遍歷,維護 visited 和 recStack (recursion stack)
   - 檢測 back edge (返回邊) 表示有 cycle
   - 記錄 cycle path 用於錯誤訊息

2. 實現要點:
   - HasCycle 遞歸方法
   - 維護 path stack 追蹤路徑
   - 檢測到 cycle 時重建 cycle path

**Step 4: Edge Validation API**

1. 建立 Controller
   ```bash
   # 位置
   src/API/Controllers/WorkflowValidationController.cs
   ```

2. 實現要點:
   - POST /api/v1/workflow-validation/edge (驗證單一 Edge)
   - POST /api/v1/workflow-validation/workflow (驗證完整 Workflow)
   - GET /api/v1/workflow-validation/compatibility/{sourceType} (取得相容目標)
   - 提供詳細的驗證結果和錯誤訊息

**測試要點**:
- 單元測試: EdgeValidationService, NodeCompatibilityMatrix
- 單元測試: Circular dependency detection (各種 edge cases)
- 整合測試: Workflow Validation API 端點
- 測試案例:
  - Simple workflow (no cycles)
  - Workflow with cycle
  - Workflow with disconnected nodes
  - Complex workflow (100+ nodes)

---

### 詳細實作指南 - Integration & Documentation

**Step 1: 整合測試設計**

1. 設置 TestContainers
   ```bash
   # 位置
   tests/IntegrationTests/Setup/IntegrationTestBase.cs
   ```

2. 實現要點:
   - 使用 TestContainers.PostgreSQL 啟動測試資料庫
   - 使用 WebApplicationFactory<Program> 啟動測試 API
   - 實現 CreateTestWorkflowAsync 輔助方法
   - 每個測試後清理資料庫

**Step 2: 編寫整合測試**

1. Workflow Definitions API 測試
   ```bash
   # 位置
   tests/IntegrationTests/API/WorkflowDefinitionsControllerTests.cs
   ```

2. Node Types API 測試
   ```bash
   # 位置
   tests/IntegrationTests/API/NodeTypesControllerTests.cs
   ```

3. Workflow Validation API 測試
   ```bash
   # 位置
   tests/IntegrationTests/API/WorkflowValidationControllerTests.cs
   ```

4. 實現要點:
   - 測試所有 HTTP 方法 (GET, POST, PUT, DELETE)
   - 測試 Happy path 和 Error cases
   - 使用 FluentAssertions 進行斷言
   - 驗證響應狀態碼和內容

**Step 3: API 文檔編寫**

1. Swagger/OpenAPI 配置
   ```bash
   # 位置
   src/API/Program.cs (Swagger configuration)
   ```

2. 實現要點:
   - 配置 Swashbuckle
   - 為所有 API 端點添加 XML 註解
   - 添加範例 (Example Values)
   - 配置 Authorization (JWT)

3. 編寫 API 文檔
   ```bash
   # 位置
   docs/api/workflow-editor-api-v1.md
   ```

4. 實現要點:
   - Overview 和 Base URL
   - Authentication 說明
   - 所有 API 端點文檔 (請求/響應範例)
   - 錯誤代碼說明
   - Rate Limiting 說明
   - 使用範例 (cURL, C#, TypeScript)

**Step 4: Sprint Retrospective**

1. 編寫 Sprint 回顧文檔
   ```bash
   # 位置
   claudedocs/2-sprints/sprint-10/SPRINT-10-7-RETROSPECTIVE.md
   ```

2. 實現要點:
   - Sprint 完成總結
   - 技術決策記錄 (TD-076 onwards)
   - 問題與解決方案
   - Phase 1C Part 1 完成度評估
   - Sprint 11 準備度檢查

---

## 第四部分: 編碼規範與最佳實踐

### 編碼規範

**C# 編碼標準**:
```yaml
命名規範:
  - Classes: PascalCase (WorkflowDefinition)
  - Interfaces: PascalCase with I prefix (IWorkflowDefinitionRepository)
  - Methods: PascalCase (CreateWorkflowAsync)
  - Private fields: _camelCase (_repository)
  - Parameters: camelCase (workflowId)
  - Constants: PascalCase (MaxNameLength)

非同步方法:
  - 後綴 Async (CreateWorkflowAsync)
  - 返回 Task 或 Task<T>
  - 使用 async/await
  - 傳遞 CancellationToken

Null 處理:
  - 使用 Nullable Reference Types (C# 8+)
  - 使用 ? 標記 nullable 類型
  - 使用 null-conditional operator (?.)
  - 使用 null-coalescing operator (??)

依賴注入:
  - 使用建構子注入
  - 避免 Service Locator pattern
  - 依賴介面而非具體類別

參考: docs/development-standards/coding-standards.md
```

**API 設計原則**:
```yaml
RESTful 設計:
  - 使用名詞而非動詞 (/workflow-definitions 而非 /create-workflow)
  - 使用複數形式 (/workflow-definitions)
  - 使用 HTTP 方法表示操作 (GET, POST, PUT, DELETE)
  - 使用正確的 HTTP 狀態碼
    - 200 OK (GET 成功)
    - 201 Created (POST 成功)
    - 204 No Content (PUT/DELETE 成功)
    - 400 Bad Request (驗證失敗)
    - 404 Not Found (資源不存在)
    - 500 Internal Server Error (伺服器錯誤)

版本化:
  - 使用 URL 版本化 (/api/v1/)
  - 主要版本變更時增加版本號

錯誤處理:
  - 標準化錯誤格式 { "error": "message" }
  - 提供詳細錯誤訊息
  - 不暴露內部實作細節

參考: docs/api/workflow-api-design.md
```

**JSON 格式規範**:
```yaml
命名規範:
  - 使用 camelCase (而非 PascalCase)
  - 使用描述性名稱

數據類型:
  - 使用正確的 JSON 數據類型 (string, number, boolean, object, array, null)
  - 日期使用 ISO 8601 格式 (2026-04-21T10:00:00Z)
  - GUID 使用標準格式 (3fa85f64-5717-4562-b3fc-2c963f66afa6)

PostgreSQL JSONB:
  - 使用 jsonb 類型 (而非 json) 以獲得更好的性能
  - 可以在 JSONB 欄位上建立索引
  - 支援 JSONB 查詢運算符 (@>, ?, ?&, ?|)
```

---

### 測試策略

**測試金字塔**:
```yaml
單元測試 (70%):
  - Domain 實體
  - Application Services
  - Validators
  - Handlers (Commands/Queries)
  - 使用 Moq 模擬依賴

整合測試 (20%):
  - API 端點
  - Database 操作
  - Repository 實現
  - 使用 TestContainers

E2E 測試 (10%):
  - 關鍵用戶流程
  - 多步驟操作
  - (Phase 1D 完成後)

參考: docs/testing/test-strategy.md
```

**單元測試最佳實踐**:
```yaml
命名規範:
  - 方法名稱: MethodName_Scenario_ExpectedResult
  - 範例: CreateWorkflow_WithValidData_ShouldReturnWorkflowId

AAA Pattern:
  - Arrange: 設置測試數據和 mocks
  - Act: 執行被測試的方法
  - Assert: 驗證結果

測試覆蓋率:
  - Domain Layer: >90%
  - Application Layer: >85%
  - Infrastructure Layer: >70%
  - API Layer: 整合測試覆蓋

使用 FluentAssertions:
  - result.Should().Be(expected)
  - result.Should().NotBeNull()
  - collection.Should().HaveCount(5)

參考: docs/testing/unit-testing-guidelines.md
```

**整合測試最佳實踐**:
```yaml
TestContainers 使用:
  - 使用 PostgreSQL container 進行測試
  - 每個測試類別啟動一次 container
  - 每個測試後清理資料庫

WebApplicationFactory:
  - 使用 WebApplicationFactory<Program> 啟動測試 API
  - 配置測試環境 (appsettings.Testing.json)
  - 使用 HttpClient 發送 HTTP 請求

測試隔離:
  - 每個測試獨立運行
  - 不依賴其他測試的結果
  - 使用 Transaction 或清理邏輯確保隔離

參考: docs/testing/integration-testing-guidelines.md
```

---

### API 設計原則

**RESTful API 最佳實踐**:
```yaml
資源命名:
  - 使用名詞 (/workflow-definitions)
  - 使用複數形式
  - 使用 kebab-case

HTTP 方法:
  - GET: 讀取資源 (安全, 冪等)
  - POST: 創建資源 (非冪等)
  - PUT: 更新資源 (冪等)
  - DELETE: 刪除資源 (冪等)

狀態碼:
  - 2xx: 成功
    - 200 OK (GET, PUT 成功)
    - 201 Created (POST 成功,返回 Location header)
    - 204 No Content (DELETE 成功)
  - 4xx: 客戶端錯誤
    - 400 Bad Request (驗證失敗)
    - 401 Unauthorized (未認證)
    - 403 Forbidden (無權限)
    - 404 Not Found (資源不存在)
  - 5xx: 伺服器錯誤
    - 500 Internal Server Error

參考: docs/api/workflow-api-design.md
```

**分頁查詢設計**:
```yaml
查詢參數:
  - pageNumber: 頁碼 (從 1 開始)
  - pageSize: 每頁筆數 (預設 20, 最大 100)
  - searchTerm: 搜尋關鍵字 (選填)
  - status: 狀態過濾 (選填)
  - sortBy: 排序欄位 (選填)
  - sortOrder: 排序方向 (asc/desc)

響應格式:
  {
    "items": [...],
    "totalCount": 100,
    "pageNumber": 1,
    "pageSize": 20,
    "totalPages": 5
  }

範例:
  GET /api/v1/workflow-definitions?pageNumber=1&pageSize=20&searchTerm=test&status=Active

參考: docs/api/api-documentation-standards.md
```

**錯誤處理設計**:
```yaml
錯誤格式:
  {
    "error": "Validation failed",
    "details": [
      "Name is required",
      "Description too long"
    ],
    "code": "VALIDATION_ERROR"
  }

錯誤類型:
  - VALIDATION_ERROR: 驗證失敗
  - NOT_FOUND: 資源不存在
  - UNAUTHORIZED: 未認證
  - FORBIDDEN: 無權限
  - INTERNAL_ERROR: 伺服器錯誤

實現:
  - 使用 Problem Details (RFC 7807)
  - 提供詳細錯誤訊息
  - 不暴露內部實作細節
  - 記錄錯誤日誌

參考: docs/api/workflow-api-design.md (Error Handling 章節)
```

---

## 第五部分: 質量保證

### 質量檢查清單

**代碼質量**:
```yaml
✅ 編碼規範:
  - [ ] 遵循 C# coding standards
  - [ ] 使用正確的命名規範
  - [ ] 非同步方法使用 Async 後綴
  - [ ] 使用 Nullable Reference Types
  - [ ] 建構子注入依賴

✅ 設計模式:
  - [ ] 遵循 SOLID 原則
  - [ ] 使用 Repository Pattern
  - [ ] 使用 CQRS Pattern (MediatR)
  - [ ] 使用 Adapter Pattern (Node Registry)
  - [ ] 使用 Strategy Pattern (Validation)

✅ 錯誤處理:
  - [ ] Try-catch 包裹外部調用
  - [ ] 記錄錯誤日誌
  - [ ] 返回 Result<T> 而非拋出例外 (Application Layer)
  - [ ] 提供詳細錯誤訊息

✅ 性能優化:
  - [ ] 使用 async/await 避免阻塞
  - [ ] 使用 Include 預載入關聯數據
  - [ ] 使用索引優化查詢
  - [ ] 避免 N+1 查詢問題

參考: docs/development-standards/code-quality-standards.md
```

**測試覆蓋率**:
```yaml
✅ 單元測試:
  - [ ] Domain 實體測試 (>90%)
  - [ ] Application Services 測試 (>85%)
  - [ ] Validators 測試 (100%)
  - [ ] Handlers 測試 (>85%)
  - [ ] 使用 Moq 模擬依賴

✅ 整合測試:
  - [ ] Workflow Definitions API 測試
  - [ ] Node Types API 測試
  - [ ] Workflow Validation API 測試
  - [ ] Happy path + Error cases
  - [ ] 使用 TestContainers

✅ 測試質量:
  - [ ] 測試命名清晰
  - [ ] 使用 AAA Pattern
  - [ ] 使用 FluentAssertions
  - [ ] 測試獨立運行

參考: docs/testing/test-strategy.md
```

**API 質量**:
```yaml
✅ RESTful 設計:
  - [ ] 遵循 RESTful 原則
  - [ ] 使用正確的 HTTP 方法
  - [ ] 使用正確的狀態碼
  - [ ] 版本化 API (v1)

✅ 文檔完整:
  - [ ] Swagger/OpenAPI 完整配置
  - [ ] 所有端點有 XML 註解
  - [ ] 提供請求/響應範例
  - [ ] 錯誤代碼說明

✅ 錯誤處理:
  - [ ] 標準化錯誤格式
  - [ ] 詳細錯誤訊息
  - [ ] 不暴露內部細節

參考: docs/api/workflow-api-design.md
```

**數據庫質量**:
```yaml
✅ Schema 設計:
  - [ ] 正規化設計 (3NF)
  - [ ] 正確的外鍵約束
  - [ ] 索引優化
  - [ ] JSONB 欄位使用

✅ Migrations:
  - [ ] Migration 可正常 up/down
  - [ ] 包含測試數據 seed
  - [ ] 索引定義完整

✅ 性能:
  - [ ] 查詢延遲 <100ms (P95)
  - [ ] 包含關聯數據查詢 <200ms (P95)

參考: docs/database/workflow-schema.md
```

---

### 驗收測試

**功能驗收**:
```yaml
✅ Workflow Definition CRUD:
  - [ ] 可以創建 Workflow Definition
  - [ ] 可以取得 Workflow Definition 列表 (分頁)
  - [ ] 可以取得單一 Workflow Definition (包含 Nodes/Edges)
  - [ ] 可以更新 Workflow Definition
  - [ ] 可以刪除 Workflow Definition (soft delete)
  - [ ] 搜尋功能正常 (Name, Description)
  - [ ] 狀態過濾正常 (Draft, Active, Archived)

✅ Node Registry:
  - [ ] 可以取得所有 Node types
  - [ ] 可以取得特定 Node type 詳情
  - [ ] 可以取得 Node type JSON Schema
  - [ ] Node configuration 驗證正常
  - [ ] 支援按 Category 過濾

✅ Edge Validation:
  - [ ] 可以驗證單一 Edge connection
  - [ ] 可以驗證完整 Workflow
  - [ ] Circular dependency 檢測正確
  - [ ] Disconnected nodes 檢測正確
  - [ ] Node compatibility 驗證正確
  - [ ] 提供詳細錯誤訊息

參考: docs/development-standards/definition-of-done.md
```

**性能驗收**:
```yaml
✅ API 響應時間:
  - [ ] GET /workflow-definitions (列表): P95 <200ms
  - [ ] GET /workflow-definitions/{id}: P95 <150ms
  - [ ] POST /workflow-definitions: P95 <300ms
  - [ ] PUT /workflow-definitions/{id}: P95 <300ms
  - [ ] DELETE /workflow-definitions/{id}: P95 <150ms

✅ Validation 性能:
  - [ ] Edge validation: <100ms per edge
  - [ ] Workflow validation: <500ms (up to 100 nodes)
  - [ ] Circular dependency detection: <200ms (up to 100 nodes)

✅ Database 性能:
  - [ ] Workflow Definition 查詢: <100ms
  - [ ] 包含 Nodes/Edges 查詢: <200ms
  - [ ] 分頁查詢: <150ms

參考: docs/performance/performance-targets.md
```

**安全驗收**:
```yaml
✅ API 安全:
  - [ ] 所有端點需要 JWT 認證
  - [ ] 輸入驗證完整 (FluentValidation)
  - [ ] SQL Injection 防護 (參數化查詢)
  - [ ] XSS 防護 (輸入清理)

✅ 錯誤處理:
  - [ ] 不暴露內部實作細節
  - [ ] 不返回 stack trace 到客戶端
  - [ ] 記錄錯誤日誌

參考: docs/security/Security-Requirements.md
```

---

### Phase 1C Part 1 完成驗收

**Sprint 10 完成標準**:
```yaml
✅ 功能完整性:
  - [ ] US 7.1 Part 1 (50%) 完成
  - [ ] Workflow Definition CRUD API 100% 實現
  - [ ] Node Registry 100% 實現
  - [ ] Edge Validation 基礎實現
  - [ ] 所有 API 端點正常運作

✅ 質量標準:
  - [ ] 單元測試覆蓋率 >85%
  - [ ] 整合測試覆蓋所有 API 端點
  - [ ] 所有測試 100% 通過
  - [ ] 無 P0/P1 Bugs

✅ 文檔完整:
  - [ ] API 文檔完整 (Swagger + Markdown)
  - [ ] Database Schema 文檔更新
  - [ ] Sprint Retrospective 完成
  - [ ] 技術決策記錄 (TD-076 onwards)

✅ 性能達標:
  - [ ] API 響應時間達標 (P95 <200ms)
  - [ ] Validation 性能達標 (<500ms)
  - [ ] Database 查詢性能達標 (<200ms)

✅ Phase 1C 進度:
  - [ ] Phase 1C Part 1 完成度: 50%
  - [ ] Sprint 11 準備度檢查通過
  - [ ] 技術債務清單整理完成

參考: docs/development-standards/definition-of-done.md
```

**Sprint 11 準備度檢查**:
```yaml
✅ 前置條件:
  - [ ] Sprint 10 所有任務 100% 完成
  - [ ] Workflow Definition API 穩定並有文檔
  - [ ] Node Registry API 正常運作
  - [ ] Edge Validation 基礎實現完成
  - [ ] 無阻塞性 bugs

✅ Sprint 11 依賴:
  - [ ] Workflow Definition 數據模型穩定
  - [ ] Node Registry 可供 Workflow Validation 使用
  - [ ] Edge Validation Service 可擴展

✅ 技術債務評估:
  - [ ] 識別所有技術債務
  - [ ] 評估債務優先級 (P0/P1/P2/P3)
  - [ ] 規劃債務償還時間 (Sprint 11 vs Phase 2)

參考: SPRINT-10-7-RETROSPECTIVE.md (Sprint 11 準備度評估)
```

---

## 第六部分: 參考文檔

### 完整參考文檔列表

**項目管理文檔**:
```yaml
1. claudedocs/1-planning/MVP-SCOPE-DEFINITION.md
   - Phase 1C 完整定義
   - Sprint 10-11 範圍

2. claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md
   - Sprint 10-11 Story Points 分析
   - 任務分解與時間估算

3. claudedocs/1-planning/DEVELOPMENT-STRATEGY.md
   - 工作流編輯器開發策略
   - Phase 1C-1D 銜接

4. claudedocs/1-planning/DEPENDENCY-MATRIX.md
   - Sprint 10 依賴分析
   - Phase 關聯分析

5. claudedocs/1-planning/RISK-REGISTER.md
   - RISK-027: Node Registry 可擴展性
   - RISK-028: Edge Validation 複雜度
   - RISK-029: Workflow Schema 演進

6. claudedocs/1-planning/TECHNICAL-DECISIONS-LOG.md
   - TD-076+: Sprint 10 技術決策記錄
```

**架構文檔**:
```yaml
7. docs/architecture/Architecture-Design-Document.md
   - 系統整體架構
   - 工作流編輯器架構

8. docs/architecture/ADR-012-workflow-editor-technology.md
   - 工作流編輯器技術選型
   - Vue 3 + VueFlow 決策

9. docs/architecture/ADR-005-cqrs-pattern.md
   - CQRS pattern 應用
   - MediatR 使用指南

10. docs/architecture/ADR-004-repository-pattern.md
    - Repository pattern 設計
    - EF Core 配置

11. docs/architecture/C4-architecture-diagrams.md
    - C4 架構圖 (Workflow Editor)
```

**API 與數據庫文檔**:
```yaml
12. docs/api/workflow-api-design.md
    - Workflow Definition API 設計
    - Node Registry API 設計
    - Edge Validation API 設計

13. docs/api/api-documentation-standards.md
    - API 文檔標準
    - Swagger/OpenAPI 規範

14. docs/database/workflow-schema.md
    - WorkflowDefinition Schema 設計
    - NodeDefinition/EdgeDefinition 設計

15. docs/database/database-schema.md
    - 完整數據庫 Schema
    - ER Diagram
```

**技術實施文檔**:
```yaml
16. docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
    - 工作流編排實施指南
    - Node Registry 設計
    - Edge Validation 設計

17. docs/technical-implementation/01-backend-net9/02-domain-layer-implementation.md
    - Domain 實體設計
    - Domain events

18. docs/technical-implementation/01-backend-net9/03-application-layer-implementation.md
    - CQRS 實施
    - Use Cases 實現

19. docs/technical-implementation/01-backend-net9/04-infrastructure-layer-implementation.md
    - Repository 實現
    - EF Core configuration

20. docs/technical-implementation/01-backend-net9/05-api-layer-implementation.md
    - Controllers 實現
    - Error handling
```

**開發標準文檔**:
```yaml
21. docs/development-standards/coding-standards.md
    - C# coding standards
    - Naming conventions

22. docs/development-standards/code-quality-standards.md
    - Quality gates
    - SonarQube rules

23. docs/development-standards/definition-of-done.md
    - Sprint DoD
    - Feature DoD

24. docs/development-standards/Non-Functional-Requirements.md
    - Performance requirements
    - Security requirements
```

**測試文檔**:
```yaml
25. docs/testing/unit-testing-guidelines.md
    - Unit test 編寫規範
    - Mocking strategies

26. docs/testing/integration-testing-guidelines.md
    - Integration test 編寫規範
    - TestContainers 使用

27. docs/testing/test-strategy.md
    - 測試策略
    - 測試金字塔

28. docs/testing/performance-testing.md
    - Performance testing guidelines
    - Benchmarking methods
```

**性能與安全文檔**:
```yaml
29. docs/performance/performance-targets.md
    - API 響應時間目標
    - Validation 性能目標

30. docs/security/Security-Requirements.md
    - API 安全要求
    - Authentication/Authorization
```

**User Stories 與 UI/UX**:
```yaml
31. docs/user-stories/modules/module-07/US-7.1-Workflow-Editor-Backend-API.md
    - US 7.1 完整描述
    - 驗收標準

32. docs/ux-design/user-research/personas.md
    - IT Developer, Business Analyst, Enterprise Admin

33. docs/ux-design/user-research/user-journey-maps.md
    - Workflow 創建旅程
```

**Sprint 文檔**:
```yaml
34. claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md
    - Phase 1B 完成驗收
    - Phase 1C 準備度評估

35. claudedocs/2-sprints/sprint-10/ (本 Sprint 文檔)
    - SPRINT-10-1-OVERVIEW.md
    - SPRINT-10-2-PLAN.md (本文檔)
    - SPRINT-10-3-CONTEXT.md
    - SPRINT-10-4-CHECKLIST.md
    - SPRINT-10-5-DEV-LOG.md
    - SPRINT-10-6-ISSUES.md
    - SPRINT-10-7-RETROSPECTIVE.md
```

---

**文檔所有者**: AI Agent Platform Development Team
**審閱者**: Tech Lead, Project Manager
**批准者**: System Architect
**下次審閱日期**: 2026-05-11 (Sprint 10 結束時)

---

*本文檔是 AI Agent Platform 項目 Sprint 10 的官方實施計劃,提供詳細的技術實作指南、編碼規範、測試策略和質量保證標準。*

*最後更新: 2025-11-14*
