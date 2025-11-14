# SPRINT 10 - 任務檢查清單 (Task Checklist)

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

1. [總體進度追蹤 (Overall Progress)](#總體進度追蹤-overall-progress)
2. [Phase 1: Workflow Definition CRUD (50+ tasks)](#phase-1-workflow-definition-crud-50-tasks)
3. [Phase 2: Node Registry System (40+ tasks)](#phase-2-node-registry-system-40-tasks)
4. [Phase 3: Edge Validation Engine (35+ tasks)](#phase-3-edge-validation-engine-35-tasks)
5. [Phase 4: Testing & Documentation (35+ tasks)](#phase-4-testing--documentation-35-tasks)
6. [Phase 1C 啟動 Checklist](#phase-1c-啟動-checklist)
7. [Definition of Done](#definition-of-done)
8. [Quality Gates](#quality-gates)

---

## 總體進度追蹤 (Overall Progress)

### Sprint 10 整體完成度

| Phase | 任務數 | 已完成 | 進行中 | 待開始 | 完成率 |
|-------|--------|--------|--------|--------|--------|
| Phase 1: Workflow Definition CRUD | 52 | 0 | 0 | 52 | 0% |
| Phase 2: Node Registry System | 42 | 0 | 0 | 42 | 0% |
| Phase 3: Edge Validation Engine | 38 | 0 | 0 | 38 | 0% |
| Phase 4: Testing & Documentation | 35 | 0 | 0 | 35 | 0% |
| **總計** | **167** | **0** | **0** | **167** | **0%** |

### 關鍵里程碑追蹤

| 里程碑 | 目標日期 | 狀態 | 備註 |
|--------|----------|------|------|
| M10.1: Workflow Definition CRUD 完成 | Week 28 End | ⏳ 待開始 | 5 個 API 端點 + Repository |
| M10.2: Node Registry 完成 | Week 29 End | ⏳ 待開始 | 5 種 Node types + Validation |
| M10.3: Edge Validation 完成 | Week 30 Mid | ⏳ 待開始 | Circular dependency + Compatibility |
| M10.4: Phase 1C Part 1 完成移交 | Week 30 End | ⏳ 待開始 | Documentation + Sprint 回顧 |

### 風險與阻礙追蹤

| ID | 風險/阻礙 | 狀態 | 優先級 | 負責人 | 解決方案 |
|----|----------|------|--------|--------|----------|
| R10.1 | Node Registry 可擴展性設計 | 🟡 監控 | P1 | Architect | 參考 n8n 設計 + JSON Schema |
| R10.2 | Edge Validation 複雜度 | 🟡 監控 | P1 | Dev Lead | DFS 算法 + 充分測試 |
| R10.3 | Workflow Schema 演進策略 | 🟢 低風險 | P2 | Architect | Version 欄位 + Sprint 11 實現 |

---

## Phase 1: Workflow Definition CRUD (50+ tasks)

**目標**: 實現 Workflow Definition 的完整 CRUD API
**工期**: Week 28 (Day 1-5)
**總任務數**: 52 tasks
**預計完成度**: Week 28 結束前 100%

### 1.1 Domain Model 設計 (12 tasks)

#### 1.1.1 WorkflowDefinition 實體
- [ ] **T10.001** - 創建 WorkflowDefinition.cs 實體
  - [ ] 定義基本屬性 (Id, Name, Description, Version)
  - [ ] 定義狀態枚舉 (WorkflowStatus: Draft/Active/Archived)
  - [ ] 定義 DefinitionJson 欄位 (JSONB)
  - [ ] 實現 Soft Delete (IsDeleted 欄位)

- [ ] **T10.002** - 實作 WorkflowDefinition Factory Methods
  - [ ] Create() 方法 (創建新 Workflow)
  - [ ] Update() 方法 (更新 Workflow)
  - [ ] Activate() 方法 (啟用 Workflow)
  - [ ] Archive() 方法 (封存 Workflow)
  - [ ] SoftDelete() 方法 (軟刪除)

- [ ] **T10.003** - 實作 WorkflowDefinition Navigation Properties
  - [ ] Nodes 集合 (List<NodeDefinition>)
  - [ ] Edges 集合 (List<EdgeDefinition>)
  - [ ] AddNode() 方法
  - [ ] AddEdge() 方法

- [ ] **T10.004** - 實作 Domain Events
  - [ ] WorkflowDefinitionCreatedEvent
  - [ ] WorkflowDefinitionUpdatedEvent
  - [ ] WorkflowDefinitionActivatedEvent
  - [ ] WorkflowDefinitionArchivedEvent
  - [ ] WorkflowDefinitionDeletedEvent

#### 1.1.2 NodeDefinition 實體
- [ ] **T10.005** - 創建 NodeDefinition.cs 實體
  - [ ] 定義基本屬性 (Id, NodeId, NodeType)
  - [ ] 定義 ConfigurationJson 欄位 (JSONB)
  - [ ] 定義 Position 屬性 (X, Y 座標)
  - [ ] 外鍵 WorkflowDefinitionId

- [ ] **T10.006** - 實作 NodeDefinition Factory Methods
  - [ ] Create() 方法 (創建新 Node)
  - [ ] Update() 方法 (更新 Node configuration)
  - [ ] Validate() 方法 (驗證 Node configuration)

- [ ] **T10.007** - 實作 NodeDefinition Value Object
  - [ ] Position record (X, Y)
  - [ ] GetEqualityComponents() 方法
  - [ ] ToString() override

#### 1.1.3 EdgeDefinition 實體
- [ ] **T10.008** - 創建 EdgeDefinition.cs 實體
  - [ ] 定義基本屬性 (Id, SourceNodeId, TargetNodeId)
  - [ ] 定義 Condition 欄位 (可選的條件表達式)
  - [ ] 外鍵 WorkflowDefinitionId

- [ ] **T10.009** - 實作 EdgeDefinition Factory Methods
  - [ ] Create() 方法 (創建新 Edge)
  - [ ] Update() 方法 (更新 Condition)
  - [ ] Validate() 方法 (驗證 Edge)

- [ ] **T10.010** - 實作 EdgeDefinition Value Object
  - [ ] GetEqualityComponents() 方法
  - [ ] ToString() override

#### 1.1.4 Unit Tests (Domain)
- [ ] **T10.011** - 編寫 WorkflowDefinition 單元測試
  - [ ] Create() 測試 (成功創建)
  - [ ] Update() 測試 (成功更新)
  - [ ] Activate() 測試 (狀態轉換)
  - [ ] SoftDelete() 測試 (IsDeleted 標記)
  - [ ] Domain Events 測試 (事件觸發)

- [ ] **T10.012** - 編寫 NodeDefinition 單元測試
  - [ ] Create() 測試
  - [ ] Validate() 測試 (valid/invalid configuration)
  - [ ] Equality 測試

---

### 1.2 Repository 實作 (10 tasks)

#### 1.2.1 Repository 介面
- [ ] **T10.013** - 定義 IWorkflowDefinitionRepository 介面
  - [ ] GetByIdAsync() 方法
  - [ ] GetByIdWithDetailsAsync() 方法 (包含 Nodes/Edges)
  - [ ] AddAsync() 方法
  - [ ] Update() 方法
  - [ ] Delete() 方法 (Soft Delete)

- [ ] **T10.014** - 定義 Repository 查詢方法
  - [ ] GetPagedAsync() 方法 (分頁查詢)
  - [ ] GetByStatusAsync() 方法 (按狀態查詢)
  - [ ] ExistsAsync() 方法 (檢查存在)
  - [ ] GetRecentAsync() 方法 (最近的 Workflows)

#### 1.2.2 Repository 實作
- [ ] **T10.015** - 創建 WorkflowDefinitionRepository.cs
  - [ ] 實作 GetByIdAsync() 方法
  - [ ] 實作 GetByIdWithDetailsAsync() 方法 (Include Nodes/Edges)
  - [ ] 實作 AddAsync() 方法
  - [ ] 實作 Update() 方法
  - [ ] 實作 Delete() 方法 (設置 IsDeleted)

- [ ] **T10.016** - 實作 Repository 查詢方法
  - [ ] GetPagedAsync() 實作 (分頁 + 搜尋 + 過濾)
  - [ ] GetByStatusAsync() 實作
  - [ ] ExistsAsync() 實作
  - [ ] GetRecentAsync() 實作 (OrderBy CreatedAt DESC)

#### 1.2.3 EF Core 配置
- [ ] **T10.017** - 創建 WorkflowDefinitionConfiguration.cs
  - [ ] 配置 Table name
  - [ ] 配置 Primary Key
  - [ ] 配置 Properties (Name, Description, Version, etc.)
  - [ ] 配置 Indexes (Name, Status, CreatedAt, IsDeleted)
  - [ ] 配置 JSONB 欄位 (DefinitionJson)

- [ ] **T10.018** - 創建 NodeDefinitionConfiguration.cs
  - [ ] 配置 Table name
  - [ ] 配置 Foreign Key (WorkflowDefinitionId)
  - [ ] 配置 Unique Constraint (WorkflowDefinitionId, NodeId)
  - [ ] 配置 JSONB 欄位 (ConfigurationJson)
  - [ ] 配置 Cascade Delete

- [ ] **T10.019** - 創建 EdgeDefinitionConfiguration.cs
  - [ ] 配置 Table name
  - [ ] 配置 Foreign Key (WorkflowDefinitionId)
  - [ ] 配置 Unique Constraint (WorkflowDefinitionId, SourceNodeId, TargetNodeId)
  - [ ] 配置 Cascade Delete

#### 1.2.4 Database Migration
- [ ] **T10.020** - 創建 EF Core Migration
  - [ ] 運行 dotnet ef migrations add AddWorkflowDefinition
  - [ ] 檢查生成的 Migration 代碼
  - [ ] 測試 Migration Up (創建 tables)
  - [ ] 測試 Migration Down (刪除 tables)

#### 1.2.5 Unit Tests (Repository)
- [ ] **T10.021** - 編寫 Repository 單元測試
  - [ ] GetByIdAsync() 測試 (存在/不存在)
  - [ ] AddAsync() 測試 (成功添加)
  - [ ] Update() 測試 (成功更新)
  - [ ] Delete() 測試 (Soft Delete 驗證)
  - [ ] GetPagedAsync() 測試 (分頁正確)

- [ ] **T10.022** - 編寫 Repository 整合測試
  - [ ] 使用 TestContainers (PostgreSQL)
  - [ ] 測試完整 CRUD 流程
  - [ ] 測試 Cascade Delete (Nodes/Edges)
  - [ ] 測試 JSONB 欄位讀寫

---

### 1.3 CQRS Commands/Queries (12 tasks)

#### 1.3.1 Commands
- [ ] **T10.023** - 創建 CreateWorkflowDefinitionCommand
  - [ ] 定義 Command 結構 (Name, Description, DefinitionJson, Nodes, Edges)
  - [ ] 創建 CreateWorkflowDefinitionCommandHandler
  - [ ] 實作 Handler 邏輯 (Domain Model → Repository)
  - [ ] 觸發 Domain Events

- [ ] **T10.024** - 創建 UpdateWorkflowDefinitionCommand
  - [ ] 定義 Command 結構 (Id, Name, Description, DefinitionJson, Nodes, Edges)
  - [ ] 創建 UpdateWorkflowDefinitionCommandHandler
  - [ ] 實作 Handler 邏輯 (更新 + 觸發 Events)
  - [ ] 處理 Not Found 錯誤

- [ ] **T10.025** - 創建 DeleteWorkflowDefinitionCommand
  - [ ] 定義 Command 結構 (Id)
  - [ ] 創建 DeleteWorkflowDefinitionCommandHandler
  - [ ] 實作 Soft Delete 邏輯
  - [ ] 觸發 WorkflowDefinitionDeletedEvent

- [ ] **T10.026** - 創建 ActivateWorkflowDefinitionCommand
  - [ ] 定義 Command 結構 (Id)
  - [ ] 創建 ActivateWorkflowDefinitionCommandHandler
  - [ ] 實作狀態轉換邏輯 (Draft → Active)
  - [ ] 驗證狀態轉換合法性

#### 1.3.2 Queries
- [ ] **T10.027** - 創建 GetWorkflowDefinitionByIdQuery
  - [ ] 定義 Query 結構 (Id)
  - [ ] 創建 GetWorkflowDefinitionByIdQueryHandler
  - [ ] 實作查詢邏輯 (包含 Nodes/Edges)
  - [ ] 映射到 DTO

- [ ] **T10.028** - 創建 GetWorkflowDefinitionsQuery
  - [ ] 定義 Query 結構 (PageNumber, PageSize, SearchTerm, Status)
  - [ ] 創建 GetWorkflowDefinitionsQueryHandler
  - [ ] 實作分頁查詢邏輯
  - [ ] 映射到 PagedResult<WorkflowDefinitionDto>

#### 1.3.3 Validators
- [ ] **T10.029** - 創建 CreateWorkflowDefinitionCommandValidator
  - [ ] Name 驗證 (非空, 長度 1-200)
  - [ ] Description 驗證 (最大長度 1000)
  - [ ] DefinitionJson 驗證 (有效 JSON)
  - [ ] Nodes 驗證 (至少 1 個 Node)

- [ ] **T10.030** - 創建 UpdateWorkflowDefinitionCommandValidator
  - [ ] Id 驗證 (非空)
  - [ ] Name 驗證
  - [ ] DefinitionJson 驗證
  - [ ] Nodes 驗證

#### 1.3.4 DTOs
- [ ] **T10.031** - 創建 WorkflowDefinitionDto
  - [ ] 基本屬性 (Id, Name, Description, Version, Status)
  - [ ] Nodes 集合 (List<NodeDefinitionDto>)
  - [ ] Edges 集合 (List<EdgeDefinitionDto>)
  - [ ] Audit 欄位 (CreatedAt, UpdatedAt)

- [ ] **T10.032** - 創建 NodeDefinitionDto 和 EdgeDefinitionDto
  - [ ] NodeDefinitionDto (Id, NodeId, NodeType, ConfigurationJson, Position)
  - [ ] EdgeDefinitionDto (Id, SourceNodeId, TargetNodeId, Condition)

#### 1.3.5 Unit Tests (CQRS)
- [ ] **T10.033** - 編寫 Commands 單元測試
  - [ ] CreateWorkflowDefinitionCommand 測試 (成功創建)
  - [ ] UpdateWorkflowDefinitionCommand 測試 (成功更新)
  - [ ] DeleteWorkflowDefinitionCommand 測試 (Soft Delete)
  - [ ] ActivateWorkflowDefinitionCommand 測試 (狀態轉換)

- [ ] **T10.034** - 編寫 Queries 單元測試
  - [ ] GetWorkflowDefinitionByIdQuery 測試
  - [ ] GetWorkflowDefinitionsQuery 測試 (分頁)
  - [ ] Validators 測試 (valid/invalid input)

---

### 1.4 API Controllers (10 tasks)

#### 1.4.1 Controller 實作
- [ ] **T10.035** - 創建 WorkflowDefinitionsController.cs
  - [ ] 註入 IMediator
  - [ ] 設置 Route [api/v1/workflow-definitions]
  - [ ] 設置 ApiController 和 Authorize attributes

- [ ] **T10.036** - 實作 POST /api/v1/workflow-definitions
  - [ ] 接收 CreateWorkflowDefinitionRequest
  - [ ] 發送 CreateWorkflowDefinitionCommand
  - [ ] 返回 201 Created + WorkflowDefinitionDto
  - [ ] 錯誤處理 (400 Bad Request, 500 Internal Server Error)

- [ ] **T10.037** - 實作 GET /api/v1/workflow-definitions
  - [ ] 接收分頁參數 (pageNumber, pageSize, searchTerm, status)
  - [ ] 發送 GetWorkflowDefinitionsQuery
  - [ ] 返回 200 OK + PagedResult<WorkflowDefinitionDto>
  - [ ] 錯誤處理

- [ ] **T10.038** - 實作 GET /api/v1/workflow-definitions/{id}
  - [ ] 接收 id 參數
  - [ ] 發送 GetWorkflowDefinitionByIdQuery
  - [ ] 返回 200 OK + WorkflowDefinitionDto
  - [ ] 錯誤處理 (404 Not Found)

- [ ] **T10.039** - 實作 PUT /api/v1/workflow-definitions/{id}
  - [ ] 接收 UpdateWorkflowDefinitionRequest
  - [ ] 發送 UpdateWorkflowDefinitionCommand
  - [ ] 返回 200 OK + WorkflowDefinitionDto
  - [ ] 錯誤處理 (400, 404, 500)

- [ ] **T10.040** - 實作 DELETE /api/v1/workflow-definitions/{id}
  - [ ] 接收 id 參數
  - [ ] 發送 DeleteWorkflowDefinitionCommand
  - [ ] 返回 204 No Content
  - [ ] 錯誤處理 (404, 500)

#### 1.4.2 Request/Response Models
- [ ] **T10.041** - 創建 Request Models
  - [ ] CreateWorkflowDefinitionRequest
  - [ ] UpdateWorkflowDefinitionRequest
  - [ ] 包含 Data Annotations 驗證

- [ ] **T10.042** - 創建 Response Models
  - [ ] WorkflowDefinitionResponse (繼承 WorkflowDefinitionDto)
  - [ ] PagedWorkflowDefinitionsResponse
  - [ ] Error Response Models

#### 1.4.3 Swagger 註解
- [ ] **T10.043** - 添加 Swagger 註解
  - [ ] Controller 層級註解 (Tags, Description)
  - [ ] 每個 Action 的註解 (Summary, Description)
  - [ ] Request/Response 範例
  - [ ] Error codes 說明

#### 1.4.4 Integration Tests (API)
- [ ] **T10.044** - 編寫 API 整合測試
  - [ ] POST /api/v1/workflow-definitions 測試 (201 Created)
  - [ ] GET /api/v1/workflow-definitions 測試 (分頁)
  - [ ] GET /api/v1/workflow-definitions/{id} 測試 (200 OK, 404 Not Found)
  - [ ] PUT /api/v1/workflow-definitions/{id} 測試 (200 OK, 404)
  - [ ] DELETE /api/v1/workflow-definitions/{id} 測試 (204 No Content, 404)

---

### 1.5 Phase 1 總結與驗收 (8 tasks)

#### 1.5.1 代碼審查
- [ ] **T10.045** - 進行 Code Review
  - [ ] Domain Model 審查
  - [ ] Repository 審查
  - [ ] CQRS 審查
  - [ ] API 審查

#### 1.5.2 測試覆蓋率
- [ ] **T10.046** - 檢查測試覆蓋率
  - [ ] Domain Layer ≥90%
  - [ ] Application Layer ≥85%
  - [ ] Infrastructure Layer ≥80%
  - [ ] API Layer ≥80%

#### 1.5.3 性能測試
- [ ] **T10.047** - 進行性能測試
  - [ ] GET /api/v1/workflow-definitions/{id} (P95 <150ms)
  - [ ] GET /api/v1/workflow-definitions (分頁) (P95 <200ms)
  - [ ] POST /api/v1/workflow-definitions (P95 <300ms)

#### 1.5.4 文檔更新
- [ ] **T10.048** - 更新 API 文檔
  - [ ] Swagger UI 驗證
  - [ ] API 範例更新
  - [ ] 錯誤代碼文檔

#### 1.5.5 部署測試
- [ ] **T10.049** - 部署到 Development 環境
  - [ ] 運行 Migration
  - [ ] 測試 API 端點
  - [ ] 驗證 Swagger 文檔

#### 1.5.6 驗收檢查
- [ ] **T10.050** - Phase 1 驗收 Checklist
  - [ ] ✅ 5 個 API 端點正常運作
  - [ ] ✅ CRUD 操作完整
  - [ ] ✅ Soft Delete 支持
  - [ ] ✅ 測試覆蓋率 >85%
  - [ ] ✅ Swagger 文檔完整

#### 1.5.7 問題追蹤
- [ ] **T10.051** - 記錄技術債務
  - [ ] 識別需要改進的地方
  - [ ] 記錄到 ISSUES.md
  - [ ] 優先級排序

#### 1.5.8 技術決策
- [ ] **T10.052** - 記錄技術決策
  - [ ] TD-076: Workflow Schema 存儲格式
  - [ ] 更新 DEV-LOG.md
  - [ ] 更新 TECHNICAL-DECISIONS-LOG.md

---

## Phase 2: Node Registry System (40+ tasks)

**目標**: 實現 Node Registry 註冊機制和 schema 驗證
**工期**: Week 29 (Day 6-10)
**總任務數**: 42 tasks
**預計完成度**: Week 29 結束前 100%

### 2.1 Node Type Definition (10 tasks)

#### 2.1.1 數據結構設計
- [ ] **T10.053** - 創建 NodeTypeDefinition.cs
  - [ ] 定義基本屬性 (Type, DisplayName, Description)
  - [ ] 定義 Category 和 IconUrl
  - [ ] 定義 ConfigurationSchema (JSON Schema)
  - [ ] 定義 CompatibleSourceTypes 和 CompatibleTargetTypes
  - [ ] 定義 Metadata Dictionary

- [ ] **T10.054** - 創建內建 Node Types
  - [ ] BuiltInNodeTypes.Agent
  - [ ] BuiltInNodeTypes.Tool
  - [ ] BuiltInNodeTypes.Condition
  - [ ] BuiltInNodeTypes.Parallel
  - [ ] BuiltInNodeTypes.Sequential

#### 2.1.2 JSON Schema 定義
- [ ] **T10.055** - 定義 Agent Node Schema
  - [ ] agentId 屬性 (required, uuid format)
  - [ ] parameters 屬性 (object)
  - [ ] timeout 屬性 (integer, 1-3600)
  - [ ] JSON Schema 驗證測試

- [ ] **T10.056** - 定義 Tool Node Schema
  - [ ] toolName 屬性 (required, string)
  - [ ] parameters 屬性 (object)
  - [ ] timeout 屬性 (integer)
  - [ ] JSON Schema 驗證測試

- [ ] **T10.057** - 定義 Condition Node Schema
  - [ ] condition 屬性 (required, string expression)
  - [ ] trueBranch 屬性 (string)
  - [ ] falseBranch 屬性 (string)
  - [ ] JSON Schema 驗證測試

- [ ] **T10.058** - 定義 Parallel Node Schema
  - [ ] branches 屬性 (array of branch definitions)
  - [ ] maxConcurrency 屬性 (integer, default unlimited)
  - [ ] JSON Schema 驗證測試

- [ ] **T10.059** - 定義 Sequential Node Schema
  - [ ] steps 屬性 (array of step definitions)
  - [ ] stopOnError 屬性 (boolean, default true)
  - [ ] JSON Schema 驗證測試

#### 2.1.3 Compatibility Rules
- [ ] **T10.060** - 定義 Node Compatibility Matrix
  - [ ] Agent compatibility rules
  - [ ] Tool compatibility rules
  - [ ] Condition compatibility rules
  - [ ] Parallel compatibility rules
  - [ ] Sequential compatibility rules

#### 2.1.4 Unit Tests (Node Types)
- [ ] **T10.061** - 編寫 Node Type 單元測試
  - [ ] NodeTypeDefinition 創建測試
  - [ ] JSON Schema 載入測試
  - [ ] Compatibility rules 測試

- [ ] **T10.062** - 編寫 JSON Schema 驗證測試
  - [ ] Agent schema 驗證 (valid/invalid configurations)
  - [ ] Tool schema 驗證
  - [ ] Condition schema 驗證
  - [ ] Parallel schema 驗證
  - [ ] Sequential schema 驗證

---

### 2.2 Node Registry Service (12 tasks)

#### 2.2.1 Registry 介面
- [ ] **T10.063** - 定義 INodeTypeRegistry 介面
  - [ ] RegisterNodeType() 方法
  - [ ] UnregisterNodeType() 方法
  - [ ] GetNodeType() 方法
  - [ ] GetAllNodeTypes() 方法
  - [ ] GetNodeTypesByCategory() 方法

- [ ] **T10.064** - 定義 Registry 驗證方法
  - [ ] IsValidNodeType() 方法
  - [ ] ValidateConfiguration() 方法
  - [ ] ValidateConfigurationDetailed() 方法
  - [ ] AreCompatible() 方法
  - [ ] GetCompatibleTargetTypes() 方法

#### 2.2.2 Registry 實作
- [ ] **T10.065** - 創建 NodeTypeRegistry.cs
  - [ ] ConcurrentDictionary 存儲 Node Types
  - [ ] 註冊內建 Node Types
  - [ ] RegisterNodeType() 實作 (線程安全)
  - [ ] GetNodeType() 實作

- [ ] **T10.066** - 實作 Node Type 查詢
  - [ ] GetAllNodeTypes() 實作
  - [ ] GetNodeTypesByCategory() 實作
  - [ ] IsValidNodeType() 實作

- [ ] **T10.067** - 實作 JSON Schema 驗證
  - [ ] ValidateConfiguration() 實作 (使用 NJsonSchema)
  - [ ] ValidateConfigurationDetailed() 實作 (詳細錯誤)
  - [ ] 錯誤訊息生成

- [ ] **T10.068** - 實作 Compatibility 驗證
  - [ ] AreCompatible() 實作
  - [ ] GetCompatibleTargetTypes() 實作
  - [ ] Compatibility 查詢優化

#### 2.2.3 Node Configuration Validator
- [ ] **T10.069** - 創建 NodeConfigurationValidator.cs
  - [ ] 整合 INodeTypeRegistry
  - [ ] Validate() 方法 (驗證 Node configuration)
  - [ ] GetValidationErrors() 方法 (詳細錯誤列表)

#### 2.2.4 Unit Tests (Registry Service)
- [ ] **T10.070** - 編寫 Registry 單元測試
  - [ ] RegisterNodeType() 測試 (成功註冊)
  - [ ] GetNodeType() 測試 (存在/不存在)
  - [ ] GetAllNodeTypes() 測試 (返回 5 個內建 types)
  - [ ] IsValidNodeType() 測試

- [ ] **T10.071** - 編寫 Validation 單元測試
  - [ ] ValidateConfiguration() 測試 (valid configuration)
  - [ ] ValidateConfiguration() 測試 (invalid configuration)
  - [ ] ValidateConfigurationDetailed() 測試 (錯誤訊息)

- [ ] **T10.072** - 編寫 Compatibility 單元測試
  - [ ] AreCompatible() 測試 (compatible pairs)
  - [ ] AreCompatible() 測試 (incompatible pairs)
  - [ ] GetCompatibleTargetTypes() 測試

#### 2.2.5 Integration Tests (Registry Service)
- [ ] **T10.073** - 編寫 Registry 整合測試
  - [ ] 註冊自定義 Node Type
  - [ ] 驗證自定義 Node configuration
  - [ ] Compatibility 整合測試

- [ ] **T10.074** - 編寫 Thread Safety 測試
  - [ ] 並發註冊測試
  - [ ] 並發查詢測試
  - [ ] 線程安全驗證

---

### 2.3 Node Registry API (10 tasks)

#### 2.3.1 Controller 實作
- [ ] **T10.075** - 創建 NodeTypesController.cs
  - [ ] 註入 INodeTypeRegistry
  - [ ] 設置 Route [api/v1/node-types]
  - [ ] 設置 ApiController attribute

- [ ] **T10.076** - 實作 GET /api/v1/node-types
  - [ ] 調用 GetAllNodeTypes()
  - [ ] 映射到 NodeTypeDto 列表
  - [ ] 返回 200 OK + List<NodeTypeDto>
  - [ ] 錯誤處理

- [ ] **T10.077** - 實作 GET /api/v1/node-types/{type}
  - [ ] 接收 type 參數
  - [ ] 調用 GetNodeType()
  - [ ] 返回 200 OK + NodeTypeDto
  - [ ] 錯誤處理 (404 Not Found)

- [ ] **T10.078** - 實作 GET /api/v1/node-types/{type}/schema
  - [ ] 接收 type 參數
  - [ ] 獲取 ConfigurationSchema
  - [ ] 返回 200 OK + JSON Schema
  - [ ] 錯誤處理 (404)

#### 2.3.2 DTOs
- [ ] **T10.079** - 創建 NodeTypeDto
  - [ ] Type, DisplayName, Description
  - [ ] Category, IconUrl
  - [ ] CompatibleSourceTypes, CompatibleTargetTypes
  - [ ] HasConfigurationSchema (boolean)

- [ ] **T10.080** - 創建 ConfigurationSchemaDto
  - [ ] Schema (JSON Schema object)
  - [ ] Examples (配置範例列表)

#### 2.3.3 Swagger 註解
- [ ] **T10.081** - 添加 Swagger 註解
  - [ ] Controller 註解
  - [ ] Action 註解 (Summary, Description)
  - [ ] Response 範例
  - [ ] Error codes 說明

#### 2.3.4 Integration Tests (API)
- [ ] **T10.082** - 編寫 API 整合測試
  - [ ] GET /api/v1/node-types 測試 (返回 5 個 types)
  - [ ] GET /api/v1/node-types/{type} 測試 (200 OK, 404)
  - [ ] GET /api/v1/node-types/{type}/schema 測試 (返回 JSON Schema)

#### 2.3.5 Validation API
- [ ] **T10.083** - 實作 POST /api/v1/node-types/{type}/validate
  - [ ] 接收 Configuration JSON
  - [ ] 調用 ValidateConfigurationDetailed()
  - [ ] 返回 ValidationResultDto
  - [ ] 詳細錯誤訊息

- [ ] **T10.084** - 編寫 Validation API 測試
  - [ ] Valid configuration 測試 (200 OK, isValid: true)
  - [ ] Invalid configuration 測試 (200 OK, isValid: false, errors)

---

### 2.4 Phase 2 總結與驗收 (10 tasks)

#### 2.4.1 代碼審查
- [ ] **T10.085** - 進行 Code Review
  - [ ] Node Type Definition 審查
  - [ ] Registry Service 審查
  - [ ] API 審查

#### 2.4.2 測試覆蓋率
- [ ] **T10.086** - 檢查測試覆蓋率
  - [ ] Node Type 單元測試 ≥90%
  - [ ] Registry Service 單元測試 ≥85%
  - [ ] API 整合測試覆蓋

#### 2.4.3 性能測試
- [ ] **T10.087** - 進行性能測試
  - [ ] GET /api/v1/node-types (P95 <100ms)
  - [ ] ValidateConfiguration() (P95 <50ms)

#### 2.4.4 文檔更新
- [ ] **T10.088** - 更新文檔
  - [ ] Node Registry 設計文檔
  - [ ] JSON Schema 文檔
  - [ ] API 文檔

#### 2.4.5 部署測試
- [ ] **T10.089** - 部署到 Development 環境
  - [ ] 測試 Node Types API
  - [ ] 驗證 JSON Schema validation

#### 2.4.6 驗收檢查
- [ ] **T10.090** - Phase 2 驗收 Checklist
  - [ ] ✅ 5 種 Node Types 註冊
  - [ ] ✅ JSON Schema 驗證正常
  - [ ] ✅ Node compatibility 查詢正常
  - [ ] ✅ 3 個 API 端點正常運作
  - [ ] ✅ 測試覆蓋率 >85%

#### 2.4.7 問題追蹤
- [ ] **T10.091** - 記錄技術債務
  - [ ] 識別需要改進的地方
  - [ ] 記錄到 ISSUES.md

#### 2.4.8 技術決策
- [ ] **T10.092** - 記錄技術決策
  - [ ] TD-077: Node Registry 實作策略
  - [ ] 更新 DEV-LOG.md

#### 2.4.9 擴展性評估
- [ ] **T10.093** - 評估 Registry 可擴展性
  - [ ] 測試自定義 Node Type 註冊
  - [ ] 驗證動態擴展能力
  - [ ] 文檔擴展指南

- [ ] **T10.094** - 性能優化評估
  - [ ] Caching 需求分析
  - [ ] Query 優化機會
  - [ ] 延後到 Sprint 11 (如需要)

---

## Phase 3: Edge Validation Engine (35+ tasks)

**目標**: 實現 Edge connection 驗證和 Circular dependency 檢測
**工期**: Week 30 (Day 11-13)
**總任務數**: 38 tasks
**預計完成度**: Week 30 中期前 100%

### 3.1 Node Compatibility Matrix (8 tasks)

#### 3.1.1 Compatibility 數據結構
- [ ] **T10.095** - 創建 NodeCompatibilityMatrix.cs
  - [ ] 定義 Compatibility rules 數據結構
  - [ ] 載入 Node Types compatibility
  - [ ] 提供 Compatibility 查詢方法

- [ ] **T10.096** - 實作 Compatibility 查詢
  - [ ] IsCompatible(sourceType, targetType) 方法
  - [ ] GetCompatibleSources(targetType) 方法
  - [ ] GetCompatibleTargets(sourceType) 方法

#### 3.1.2 Compatibility Rules
- [ ] **T10.097** - 定義詳細 Compatibility Rules
  - [ ] Agent → (Tool, Condition, Agent, End)
  - [ ] Tool → (Agent, Tool, Condition, End)
  - [ ] Condition → (Agent, Tool, End)
  - [ ] Parallel → (Agent, Tool, End)
  - [ ] Sequential → (Agent, Tool, End)

#### 3.1.3 Unit Tests (Compatibility)
- [ ] **T10.098** - 編寫 Compatibility 單元測試
  - [ ] IsCompatible() 測試 (所有 compatible pairs)
  - [ ] IsCompatible() 測試 (所有 incompatible pairs)
  - [ ] GetCompatibleTargets() 測試

- [ ] **T10.099** - 編寫 Edge Cases 測試
  - [ ] Unknown node types
  - [ ] Null 參數
  - [ ] 邊界情況

#### 3.1.4 Integration with Registry
- [ ] **T10.100** - 整合 Node Compatibility Matrix 與 Registry
  - [ ] 從 NodeTypeDefinition 載入 compatibility
  - [ ] 動態更新 compatibility matrix
  - [ ] 驗證整合正確性

- [ ] **T10.101** - 編寫整合測試
  - [ ] Registry + Compatibility Matrix 整合測試
  - [ ] 動態註冊 Node Type 更新 Matrix

- [ ] **T10.102** - 性能優化
  - [ ] Caching compatibility queries
  - [ ] 預計算 compatibility matrix

---

### 3.2 Circular Dependency Detection (10 tasks)

#### 3.2.1 DFS 算法實作
- [ ] **T10.103** - 創建 CircularDependencyDetector.cs
  - [ ] 實作 BuildAdjacencyList() 方法
  - [ ] 實作 DFS() 遞迴方法
  - [ ] 實作 HasCircularDependency() 方法

- [ ] **T10.104** - 實作 Circular Path 追蹤
  - [ ] FindCircularPath() 方法 (返回循環路徑)
  - [ ] 使用 recursion stack 追蹤路徑
  - [ ] 格式化輸出 (node1 → node2 → node3 → node1)

#### 3.2.2 算法優化
- [ ] **T10.105** - 優化 DFS 算法
  - [ ] Early termination (發現循環即停止)
  - [ ] Visited set 優化
  - [ ] 空間複雜度優化

#### 3.2.3 Unit Tests (DFS)
- [ ] **T10.106** - 編寫 DFS 單元測試 (無循環)
  - [ ] 線性 graph (A → B → C)
  - [ ] 樹狀 graph (A → B/C, B → D/E)
  - [ ] DAG (有向無環圖)

- [ ] **T10.107** - 編寫 DFS 單元測試 (有循環)
  - [ ] 簡單循環 (A → B → A)
  - [ ] 複雜循環 (A → B → C → A)
  - [ ] 多個循環

- [ ] **T10.108** - 編寫 FindCircularPath 測試
  - [ ] 驗證返回的路徑正確
  - [ ] 驗證路徑格式化正確

#### 3.2.4 Performance Tests
- [ ] **T10.109** - 編寫性能測試
  - [ ] 10 nodes, 15 edges (P95 <10ms)
  - [ ] 100 nodes, 200 edges (P95 <100ms)
  - [ ] 1000 nodes, 2000 edges (P95 <500ms)

#### 3.2.5 Edge Cases
- [ ] **T10.110** - 編寫 Edge Cases 測試
  - [ ] 空 graph
  - [ ] 單一 node
  - [ ] 自循環 (A → A)
  - [ ] 斷開的 graph

- [ ] **T10.111** - 編寫錯誤處理測試
  - [ ] Null edges
  - [ ] Invalid node IDs
  - [ ] 異常處理

- [ ] **T10.112** - 代碼審查與優化
  - [ ] Code Review
  - [ ] 性能分析
  - [ ] 優化建議實施

---

### 3.3 Edge Validation Service (10 tasks)

#### 3.3.1 Service 介面
- [ ] **T10.113** - 定義 IEdgeValidationService 介面
  - [ ] ValidateEdgeAsync() 方法
  - [ ] ValidateWorkflowAsync() 方法
  - [ ] DetectCircularDependencyAsync() 方法
  - [ ] ValidateDataFlowAsync() 方法

#### 3.3.2 Service 實作
- [ ] **T10.114** - 創建 EdgeValidationService.cs
  - [ ] 註入 INodeTypeRegistry
  - [ ] 註入 ILogger
  - [ ] 實作建構子

- [ ] **T10.115** - 實作 ValidateEdgeAsync()
  - [ ] 驗證 node types 存在
  - [ ] 驗證 node compatibility
  - [ ] 生成詳細錯誤訊息
  - [ ] 提供修復建議

- [ ] **T10.116** - 實作 ValidateWorkflowAsync()
  - [ ] 驗證所有 edges
  - [ ] 檢測 circular dependency
  - [ ] 聚合所有錯誤
  - [ ] 返回 ValidationResult

- [ ] **T10.117** - 實作 DetectCircularDependencyAsync()
  - [ ] 創建 CircularDependencyDetector
  - [ ] 調用 HasCircularDependency()
  - [ ] 如有循環,調用 FindCircularPath()
  - [ ] 返回 CircularDependencyResult

#### 3.3.3 Validation Results
- [ ] **T10.118** - 創建 ValidationResult.cs
  - [ ] IsValid (boolean)
  - [ ] Errors (List<string>)
  - [ ] Suggestions (List<string>)

- [ ] **T10.119** - 創建 CircularDependencyResult.cs
  - [ ] HasCircularDependency (boolean)
  - [ ] CircularPath (List<string>)
  - [ ] ErrorMessage (string)

#### 3.3.4 Unit Tests (Service)
- [ ] **T10.120** - 編寫 ValidateEdgeAsync 測試
  - [ ] Valid edge 測試 (compatible nodes)
  - [ ] Invalid edge 測試 (incompatible nodes)
  - [ ] Unknown node types 測試
  - [ ] 錯誤訊息驗證

- [ ] **T10.121** - 編寫 ValidateWorkflowAsync 測試
  - [ ] Valid workflow 測試 (無錯誤)
  - [ ] Invalid workflow 測試 (多個錯誤)
  - [ ] Circular dependency 測試

- [ ] **T10.122** - 編寫 Integration Tests
  - [ ] 完整 workflow 驗證
  - [ ] 複雜場景測試

---

### 3.4 Edge Validation API (10 tasks)

#### 3.4.1 Controller 實作
- [ ] **T10.123** - 創建 WorkflowValidationController.cs
  - [ ] 註入 IEdgeValidationService
  - [ ] 設置 Route [api/v1/workflow-validation]
  - [ ] 設置 ApiController attribute

- [ ] **T10.124** - 實作 POST /api/v1/workflow-validation/edge
  - [ ] 接收 ValidateEdgeRequest
  - [ ] 調用 ValidateEdgeAsync()
  - [ ] 返回 200 OK + ValidationResultDto
  - [ ] 錯誤處理

- [ ] **T10.125** - 實作 POST /api/v1/workflow-validation/workflow
  - [ ] 接收 ValidateWorkflowRequest
  - [ ] 調用 ValidateWorkflowAsync()
  - [ ] 返回 200 OK + WorkflowValidationResultDto
  - [ ] 詳細錯誤列表

#### 3.4.2 Request/Response Models
- [ ] **T10.126** - 創建 Request Models
  - [ ] ValidateEdgeRequest (sourceNodeId, targetNodeId, sourceType, targetType)
  - [ ] ValidateWorkflowRequest (nodes, edges)

- [ ] **T10.127** - 創建 Response Models
  - [ ] ValidationResultDto (isValid, errors, suggestions)
  - [ ] WorkflowValidationResultDto (isValid, edgeErrors, circularDependency)

#### 3.4.3 Swagger 註解
- [ ] **T10.128** - 添加 Swagger 註解
  - [ ] Controller 註解
  - [ ] Action 註解
  - [ ] Request/Response 範例
  - [ ] Error examples

#### 3.4.4 Integration Tests (API)
- [ ] **T10.129** - 編寫 API 整合測試
  - [ ] POST /api/v1/workflow-validation/edge 測試 (valid/invalid)
  - [ ] POST /api/v1/workflow-validation/workflow 測試
  - [ ] Circular dependency 檢測測試

#### 3.4.5 End-to-End Tests
- [ ] **T10.130** - 編寫 E2E 測試場景
  - [ ] 場景 1: 簡單 workflow (3 nodes, no circular)
  - [ ] 場景 2: 複雜 workflow (10 nodes, with branches)
  - [ ] 場景 3: Invalid workflow (circular dependency)
  - [ ] 場景 4: Invalid workflow (incompatible edges)

- [ ] **T10.131** - 編寫錯誤訊息驗證
  - [ ] 驗證錯誤訊息清晰
  - [ ] 驗證修復建議有用
  - [ ] 驗證錯誤格式一致

- [ ] **T10.132** - 性能驗證
  - [ ] Edge validation <100ms per edge
  - [ ] Workflow validation <500ms (up to 100 nodes)
  - [ ] Circular dependency detection <200ms

---

## Phase 4: Testing & Documentation (35+ tasks)

**目標**: 完成整合測試、文檔和 Sprint 回顧
**工期**: Week 30 (Day 14-15)
**總任務數**: 35 tasks
**預計完成度**: Week 30 結束前 100%

### 4.1 整合測試 (12 tasks)

#### 4.1.1 API 整合測試
- [ ] **T10.133** - 編寫 Workflow Definition API 整合測試
  - [ ] 完整 CRUD 流程測試
  - [ ] 分頁查詢測試
  - [ ] 錯誤處理測試 (400, 404, 500)
  - [ ] 使用 TestContainers (PostgreSQL)

- [ ] **T10.134** - 編寫 Node Types API 整合測試
  - [ ] 查詢所有 Node types
  - [ ] 查詢單一 Node type
  - [ ] 查詢 JSON Schema
  - [ ] Configuration 驗證測試

- [ ] **T10.135** - 編寫 Workflow Validation API 整合測試
  - [ ] Edge validation 測試
  - [ ] Workflow validation 測試
  - [ ] Circular dependency 測試
  - [ ] 性能測試

#### 4.1.2 Database 整合測試
- [ ] **T10.136** - 編寫 Repository 整合測試
  - [ ] CRUD 操作測試 (with real database)
  - [ ] Cascade delete 測試
  - [ ] JSONB 欄位測試
  - [ ] Transaction 測試

- [ ] **T10.137** - 編寫 Migration 測試
  - [ ] Migration up 測試
  - [ ] Migration down 測試
  - [ ] Data migration 測試 (如有)

#### 4.1.3 End-to-End Tests
- [ ] **T10.138** - E2E 測試: 創建 Workflow
  - [ ] 創建 Workflow Definition
  - [ ] 添加 Nodes
  - [ ] 添加 Edges
  - [ ] 驗證 Workflow
  - [ ] 查詢 Workflow

- [ ] **T10.139** - E2E 測試: 更新 Workflow
  - [ ] 更新 Workflow metadata
  - [ ] 更新 Nodes
  - [ ] 更新 Edges
  - [ ] 重新驗證

- [ ] **T10.140** - E2E 測試: 刪除 Workflow
  - [ ] Soft delete
  - [ ] 驗證 IsDeleted
  - [ ] 查詢不返回已刪除

#### 4.1.4 Performance Tests
- [ ] **T10.141** - API 性能測試
  - [ ] GET /api/v1/workflow-definitions/{id} (P95 <150ms)
  - [ ] GET /api/v1/workflow-definitions (分頁) (P95 <200ms)
  - [ ] POST /api/v1/workflow-definitions (P95 <300ms)
  - [ ] Validation APIs (P95 <500ms)

- [ ] **T10.142** - Database 性能測試
  - [ ] Workflow 查詢 (with Nodes/Edges) <200ms
  - [ ] 分頁查詢 <150ms
  - [ ] JSONB 查詢 <100ms

#### 4.1.5 測試報告
- [ ] **T10.143** - 生成測試報告
  - [ ] 單元測試報告 (Coverage, Pass rate)
  - [ ] 整合測試報告
  - [ ] E2E 測試報告
  - [ ] 性能測試報告

- [ ] **T10.144** - 測試覆蓋率分析
  - [ ] Domain Layer 覆蓋率
  - [ ] Application Layer 覆蓋率
  - [ ] Infrastructure Layer 覆蓋率
  - [ ] API Layer 覆蓋率
  - [ ] 整體覆蓋率 (目標 >85%)

---

### 4.2 API 文檔 (8 tasks)

#### 4.2.1 Swagger/OpenAPI 文檔
- [ ] **T10.145** - 驗證 Swagger UI
  - [ ] 所有 API 端點可見
  - [ ] Request/Response schemas 正確
  - [ ] 範例完整
  - [ ] 錯誤代碼說明

- [ ] **T10.146** - 生成 OpenAPI 3.0 規範
  - [ ] 匯出 swagger.json
  - [ ] 驗證規範有效性
  - [ ] 版本化 (v1)

#### 4.2.2 API 使用指南
- [ ] **T10.147** - 編寫 API 使用文檔
  - [ ] 快速開始指南
  - [ ] Workflow Definition CRUD 範例
  - [ ] Node Registry 使用範例
  - [ ] Validation API 使用範例

- [ ] **T10.148** - 編寫 API 參考文檔
  - [ ] 完整 endpoint 列表
  - [ ] Request/Response 格式
  - [ ] 錯誤代碼表
  - [ ] 狀態碼說明

#### 4.2.3 Code Examples
- [ ] **T10.149** - 編寫 C# 範例代碼
  - [ ] HttpClient 調用範例
  - [ ] 完整 CRUD 流程
  - [ ] 錯誤處理範例

- [ ] **T10.150** - 編寫 cURL 範例
  - [ ] 每個 API 端點的 cURL 命令
  - [ ] 包含 authentication headers
  - [ ] Response 範例

#### 4.2.4 TypeScript 範例 (為 Frontend 準備)
- [ ] **T10.151** - 編寫 TypeScript 範例
  - [ ] fetch/axios 調用範例
  - [ ] Type definitions
  - [ ] Error handling

- [ ] **T10.152** - 編寫 API Client 範例
  - [ ] WorkflowDefinitionClient
  - [ ] NodeTypesClient
  - [ ] ValidationClient

---

### 4.3 技術文檔 (8 tasks)

#### 4.3.1 設計文檔
- [ ] **T10.153** - 更新 Workflow Schema 文檔
  - [ ] Database schema 更新
  - [ ] ER diagram 更新
  - [ ] Indexes 說明

- [ ] **T10.154** - 編寫 Node Registry 設計文檔
  - [ ] Node Type Definition 設計
  - [ ] JSON Schema 使用
  - [ ] Compatibility Matrix 設計
  - [ ] 擴展指南

- [ ] **T10.155** - 編寫 Edge Validation 設計文檔
  - [ ] DFS 算法說明
  - [ ] Circular dependency 檢測
  - [ ] Validation 規則
  - [ ] Error messages 設計

#### 4.3.2 ADR 更新
- [ ] **T10.156** - 更新 ADR-012 (Workflow Editor Technology)
  - [ ] Backend API 設計決策
  - [ ] Node Registry 決策
  - [ ] Validation 策略決策

- [ ] **T10.157** - 創建 ADR-013 (Workflow Definition Storage)
  - [ ] JSONB vs 關聯式表決策
  - [ ] 混合模式理由
  - [ ] 性能考量

#### 4.3.3 最佳實踐文檔
- [ ] **T10.158** - 編寫 Node Type 開發指南
  - [ ] 如何定義新 Node Type
  - [ ] JSON Schema 最佳實踐
  - [ ] Compatibility rules 定義

- [ ] **T10.159** - 編寫 Workflow Definition 最佳實踐
  - [ ] Workflow 結構設計
  - [ ] Node configuration 建議
  - [ ] 性能優化建議

- [ ] **T10.160** - 編寫故障排除指南
  - [ ] 常見問題 FAQ
  - [ ] 錯誤訊息解釋
  - [ ] 解決方案建議

---

### 4.4 Sprint 回顧與移交 (7 tasks)

#### 4.4.1 Sprint 回顧
- [ ] **T10.161** - 完成 SPRINT-10-7-RETROSPECTIVE.md
  - [ ] Sprint 完成總結
  - [ ] 關鍵指標分析
  - [ ] 技術決策回顧
  - [ ] 經驗教訓

- [ ] **T10.162** - 記錄技術債務
  - [ ] 識別技術債務清單
  - [ ] 優先級排序
  - [ ] 延後到 Sprint 11 的項目

- [ ] **T10.163** - 記錄改進建議
  - [ ] 流程改進
  - [ ] 工具改進
  - [ ] 團隊協作改進

#### 4.4.2 Sprint 11 準備
- [ ] **T10.164** - Sprint 11 Handoff Checklist
  - [ ] 已完成功能清單
  - [ ] API 文檔清單
  - [ ] Database schema 清單
  - [ ] 測試報告清單

- [ ] **T10.165** - Sprint 11 技術資料準備
  - [ ] Workflow Validation 完整實現需求
  - [ ] Schema Versioning 設計文檔
  - [ ] Performance optimization 機會

- [ ] **T10.166** - Sprint 11 風險識別
  - [ ] 技術風險清單
  - [ ] 資源風險清單
  - [ ] 時程風險清單

#### 4.4.3 Phase 1C Part 1 完成驗收
- [ ] **T10.167** - Phase 1C Part 1 驗收
  - [ ] ✅ Workflow Definition CRUD 100% 完成
  - [ ] ✅ Node Registry 100% 完成
  - [ ] ✅ Edge Validation 基礎完成
  - [ ] ✅ 測試覆蓋率 >85%
  - [ ] ✅ API 文檔完整
  - [ ] ✅ 性能目標達成
  - [ ] ✅ Phase 1C 整體進度 50%

---

## Phase 1C 啟動 Checklist

**目標**: 確認 Phase 1C 啟動的所有前置條件已滿足

### Phase 1B 完成驗證

- [ ] **PC.1** - Sprint 9 Retrospective 完成
  - [ ] Phase 1B 回顧文檔完成
  - [ ] 技術債務清單整理
  - [ ] 經驗教訓記錄

- [ ] **PC.2** - Framework Abstraction Layer 驗收
  - [ ] 5 個核心介面驗收通過
  - [ ] SemanticKernelAdapter 驗收通過
  - [ ] 可替換性驗證 100% 通過
  - [ ] 性能基準測試通過 (開銷 <5%)

- [ ] **PC.3** - Workflow Execution Engine 驗收
  - [ ] Workflow 執行狀態管理正常
  - [ ] Multi-Agent 協調正常
  - [ ] Agent Messaging 正常
  - [ ] 執行日誌記錄完整

- [ ] **PC.4** - 質量門檢驗證
  - [ ] 測試覆蓋率 >85%
  - [ ] 無已知 P0/P1 bugs
  - [ ] API 響應時間達標
  - [ ] 安全驗證通過

### Sprint 10 環境準備

- [ ] **PC.5** - 開發環境準備
  - [ ] .NET 9 SDK 安裝
  - [ ] PostgreSQL 16 運行
  - [ ] Redis 運行
  - [ ] IDE 配置 (Visual Studio / Rider)

- [ ] **PC.6** - 依賴套件準備
  - [ ] EF Core 9.0 安裝
  - [ ] MediatR 12.4 安裝
  - [ ] FluentValidation 11.11 安裝
  - [ ] NJsonSchema 安裝

- [ ] **PC.7** - 測試環境準備
  - [ ] xUnit 2.6 安裝
  - [ ] TestContainers 3.6 安裝
  - [ ] FluentAssertions 6.12 安裝

### Sprint 10 文檔準備

- [ ] **PC.8** - 參考文檔準備
  - [ ] 閱讀 docs/api/workflow-api-design.md
  - [ ] 閱讀 docs/database/workflow-schema.md
  - [ ] 閱讀 docs/architecture/ADR-012-workflow-editor-technology.md
  - [ ] 閱讀 Sprint 10 OVERVIEW, PLAN, CONTEXT

- [ ] **PC.9** - 設計文檔準備
  - [ ] Workflow Definition 數據模型設計
  - [ ] Node Registry 設計
  - [ ] Edge Validation 設計

### Sprint 10 團隊準備

- [ ] **PC.10** - 團隊能力評估
  - [ ] .NET 9 技能
  - [ ] EF Core 9 技能
  - [ ] CQRS pattern 理解
  - [ ] JSON Schema 理解

- [ ] **PC.11** - 任務分配
  - [ ] Domain Model 負責人
  - [ ] Repository 負責人
  - [ ] CQRS 負責人
  - [ ] API 負責人

- [ ] **PC.12** - Sprint Planning Meeting
  - [ ] Sprint 目標確認
  - [ ] Story Points 確認
  - [ ] 任務分解確認
  - [ ] 時程確認

---

## Definition of Done

### Sprint 10 Definition of Done

#### 代碼質量
- [ ] 所有代碼已提交到 Git (feature/sprint-10 branch)
- [ ] Code Review 已完成 (至少 2 位 Reviewers)
- [ ] 無 Code Review 未解決的 Blocking Comments
- [ ] 代碼符合 C# Coding Standards (StyleCop, EditorConfig)
- [ ] 無 ReSharper / Rider 警告 (Severity: Warning or higher)

#### 測試要求
- [ ] 單元測試覆蓋率 ≥85% (整體)
- [ ] 核心業務邏輯覆蓋率 ≥90% (Domain Layer)
- [ ] 所有單元測試通過 (0 failures)
- [ ] 所有整合測試通過 (0 failures)
- [ ] 所有 E2E 測試通過 (0 failures)
- [ ] 性能測試通過 (API 響應時間, Validation 延遲)

#### 文檔要求
- [ ] XML 文檔註解完整 (所有 public 方法和類)
- [ ] API 文檔生成 (Swagger / Redoc)
- [ ] README 更新 (Phase 1C Part 1 說明)
- [ ] ADR-012 更新完成
- [ ] ADR-013 創建完成
- [ ] API 使用指南編寫完成

#### 整合要求
- [ ] 與 Sprint 9 Framework Abstraction Layer 整合測試通過
- [ ] Database Migration 成功
- [ ] Swagger UI 正常運作
- [ ] DI Container 註冊正確 (Program.cs)

#### 部署要求
- [ ] Development 環境部署成功
- [ ] Staging 環境部署成功
- [ ] 健康檢查端點正常 (GET /health)
- [ ] 日誌輸出正常 (Serilog to Console + File)
- [ ] 無部署錯誤或警告

#### Phase 1C Part 1 完成要求
- [ ] US 7.1 Part 1 (50%) 完成
- [ ] Workflow Definition CRUD 100% 實現
- [ ] Node Registry 100% 實現
- [ ] Edge Validation 基礎實現
- [ ] Sprint 10 Retrospective 完成
- [ ] Sprint 11 Handoff Checklist 準備完成

---

## Quality Gates

### Code Quality Gates

| 指標 | 目標 | 當前值 | 狀態 |
|------|------|--------|------|
| 代碼覆蓋率 (整體) | ≥85% | 0% | ⏳ 待開始 |
| Domain Layer 覆蓋率 | ≥90% | 0% | ⏳ 待開始 |
| Application Layer 覆蓋率 | ≥85% | 0% | ⏳ 待開始 |
| 單元測試通過率 | 100% | 0% | ⏳ 待開始 |
| 整合測試通過率 | 100% | 0% | ⏳ 待開始 |
| E2E 測試通過率 | 100% | 0% | ⏳ 待開始 |
| StyleCop 違規數 | 0 | 0 | ⏳ 待開始 |
| ReSharper 警告數 | 0 | 0 | ⏳ 待開始 |

### Performance Quality Gates

| 指標 | 目標 | 當前值 | 狀態 |
|------|------|--------|------|
| GET /workflow-definitions/{id} (P95) | <150ms | - | ⏳ 待測試 |
| GET /workflow-definitions (P95) | <200ms | - | ⏳ 待測試 |
| POST /workflow-definitions (P95) | <300ms | - | ⏳ 待測試 |
| Edge Validation (P95) | <100ms | - | ⏳ 待測試 |
| Workflow Validation (P95) | <500ms | - | ⏳ 待測試 |
| Circular Dependency Detection (P95) | <200ms | - | ⏳ 待測試 |

### Documentation Quality Gates

| 指標 | 目標 | 當前值 | 狀態 |
|------|------|--------|------|
| XML 文檔註解完整度 | 100% | 0% | ⏳ 待開始 |
| API 文檔生成 | 完成 | 未開始 | ⏳ 待開始 |
| ADR 文檔更新 | 完成 | 未開始 | ⏳ 待開始 |
| API 使用指南編寫 | 完成 | 未開始 | ⏳ 待開始 |
| Code Examples 完整度 | 100% | 0% | ⏳ 待開始 |

### Integration Quality Gates

| 指標 | 目標 | 當前值 | 狀態 |
|------|------|--------|------|
| Sprint 9 整合測試 | 通過 | 未開始 | ⏳ 待開始 |
| Database Migration 測試 | 通過 | 未開始 | ⏳ 待開始 |
| Swagger UI 功能 | 正常 | 未開始 | ⏳ 待開始 |
| API 契約穩定性 | 穩定 | 未開始 | ⏳ 待開始 |

### Phase 1C Part 1 Completion Gates

| 指標 | 目標 | 當前值 | 狀態 |
|------|------|--------|------|
| Workflow Definition CRUD | 100% | 0% | ⏳ 待開始 |
| Node Registry | 100% | 0% | ⏳ 待開始 |
| Edge Validation | 基礎完成 | 0% | ⏳ 待開始 |
| 測試覆蓋率 | >85% | 0% | ⏳ 待開始 |
| API 文檔 | 完整 | 0% | ⏳ 待開始 |
| Sprint 11 Handoff 準備 | 完成 | 未開始 | ⏳ 待開始 |

---

## 進度追蹤說明

### 任務狀態符號
- [ ] **待開始**: 任務尚未開始
- [🔄] **進行中**: 任務正在執行
- [✅] **已完成**: 任務已完成並通過驗證
- [⏸️] **暫停**: 任務因阻礙而暫停
- [❌] **失敗**: 任務執行失敗需要重新執行

### 優先級標記
- **P0**: Critical (必須在 Sprint 內完成)
- **P1**: High (高優先級, 盡量在 Sprint 內完成)
- **P2**: Medium (中優先級, 可以延後)
- **P3**: Low (低優先級, 可以跳過)

### 檢查清單使用方法
1. **每日站會前**: 更新任務狀態
2. **完成任務時**: 勾選對應 checkbox
3. **遇到阻礙時**: 標記為 [⏸️] 並記錄到 Issues
4. **Sprint 結束時**: 確認所有 P0/P1 任務已完成

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**總任務數**: 167 tasks
**預計完成度**: 0% (0/167 tasks completed)
