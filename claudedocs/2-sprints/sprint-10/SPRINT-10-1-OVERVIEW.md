# SPRINT-10-1-OVERVIEW.md - Sprint 10 概覽:工作流編輯器 Backend API (Phase 1C 啟動)

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

1. [Sprint 定位與參考](#sprint-定位與參考)
2. [Phase 1C 啟動說明](#phase-1c-啟動說明)
3. [Sprint 目標](#sprint-目標)
4. [User Stories 分配](#user-stories-分配)
5. [技術範圍](#技術範圍)
6. [預期交付](#預期交付)
7. [風險與依賴](#風險與依賴)
8. [成功指標](#成功指標)
9. [相關文檔](#相關文檔)
10. [完整參考文獻索引](#完整參考文獻索引)
11. [使用指南](#使用指南)
12. [版本歷史](#版本歷史)

---

## Sprint 定位與參考

**🎉 Phase 轉換重要里程碑**:
Sprint 10 標誌著從 **Phase 1B (工作流引擎核心)** 成功轉換到 **Phase 1C (工作流編輯器 Backend)**。關鍵路徑 (Critical Path) 已在 Sprint 7-9 完成,後續 Sprint 不再有阻斷風險。

**Sprint 定位參考**:
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1C: 工作流編輯器 Backend
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 10-11 詳細分析 (16 SP)
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - 工作流編輯器開發策略
- 🏛️ [ADR-012](../../docs/architecture/ADR-012-workflow-editor-technology.md) - 工作流編輯器技術選型
- 🗄️ [Workflow Schema](../../docs/database/workflow-schema.md) - Workflow Definition 數據模型
- 🔌 [Workflow API Design](../../docs/api/workflow-api-design.md) - API 設計規範

**Sprint 7-8-9-10 連貫性**:
```yaml
Sprint 7 (✅ 已完成):
  核心: Task Generator + Multi-Agent Coordination Layer
  交付:
    - ✅ Task Generator (LLM 驅動任務規劃)
    - ✅ Sequential Coordination (順序執行)
    - ✅ Parallel Coordination (並行執行初步)
    - ✅ Workflow API (CRUD)
  Story Points: 13 SP
  狀態: Phase 1B 第一階段完成 (38%)

Sprint 8 (✅ 已完成):
  核心: Workflow Execution Engine + Agent Messaging
  交付:
    - ✅ Parallel Coordination 完成 (race condition, result aggregation)
    - ✅ Workflow Execution Engine 基礎實現
    - ✅ Agent Messaging 系統建立
    - ✅ State Management 強化
  Story Points: 13 SP
  狀態: Phase 1B 第二階段完成 (76%)

Sprint 9 (✅ 已完成):
  核心: Framework Abstraction Layer
  交付:
    - ✅ 5 個核心介面實現 (IWorkflowEngine, ITaskGenerator, etc.)
    - ✅ SemanticKernelAdapter 完整實作
    - ✅ 可替換性驗證與測試
    - ✅ Phase 1B 完成驗收與移交
  Story Points: 9 SP
  狀態: Phase 1B 100% 完成

Sprint 10 (本 Sprint - Phase 1C 啟動):
  核心: Workflow Editor Backend API
  重點:
    - Workflow Definition CRUD API
    - Node Registry 註冊機制
    - Edge Validation 驗證規則
    - API 設計與文檔
  Story Points: 8 SP
  關鍵性: **Phase 1C 開始,為 Phase 1D (Frontend) 奠定基礎**
```

---

## Phase 1C 啟動說明

### Phase 1B 完成總結

**Phase 1B (Sprint 7-9) 完成驗收**:
```yaml
總計: 35 Story Points, 9 週, 100% 完成

核心交付:
  ✅ Task Generator:
     - LLM 驅動任務分解 (GPT-4 / Claude 3)
     - 任務依賴分析
     - 執行計劃生成
     - 參考: Sprint 7 交付

  ✅ Multi-Agent Coordination:
     - Sequential 協調模式 (順序執行)
     - Parallel 協調模式 (並行執行)
     - Race condition 處理
     - Result aggregation (結果聚合)
     - 參考: Sprint 7-8 交付

  ✅ Workflow Execution Engine:
     - Workflow 實例管理
     - 狀態機實現 (7 種狀態)
     - 錯誤處理與重試
     - 執行日誌記錄
     - 參考: Sprint 8 交付

  ✅ Agent Messaging:
     - 消息傳遞機制 (Request/Response)
     - 消息路由與分發
     - 消息持久化 (PostgreSQL)
     - 參考: Sprint 8 交付

  ✅ Framework Abstraction Layer:
     - 5 個核心介面定義
     - SemanticKernelAdapter 實作
     - 可替換性驗證
     - 性能基準測試 (開銷 <5%)
     - 參考: Sprint 9 交付

驗收標準:
  ✅ 功能完整性: 100% User Stories 完成
  ✅ 測試覆蓋率: >85% (單元測試 + 整合測試)
  ✅ 性能達標: Agent 執行 P95 <5 秒
  ✅ 安全驗證: 無已知安全漏洞
  ✅ 文檔完整: API 文檔、架構文檔、測試報告

技術債務清單:
  🔧 TD-069: Parallel Coordination 優化 (P2)
  🔧 TD-071: Task Generator 錯誤處理強化 (P2)
  🔧 TD-073: Workflow Execution 監控增強 (P3)
  📋 詳見: Sprint 9 Retrospective

參考文檔:
  - claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md (Phase 1B 完整回顧)
  - docs/architecture/ADR-011-framework-migration-strategy.md (Framework 抽象層決策)
```

### Phase 1C 目標與範圍

**Phase 1C Overview**:
```yaml
名稱: 工作流編輯器 Backend (Workflow Editor Backend)
Sprint: Sprint 10-11 (6 週)
Story Points: 16 SP
完成度: 0% → 100%

核心目標:
  1. Workflow Definition CRUD API
     - WorkflowDefinition 實體與 Repository
     - Workflow metadata 管理
     - Workflow 版本控制基礎
     - 完整 CRUD API 端點

  2. Node Registry
     - Node type 註冊機制
     - Node schema 定義 (JSON Schema)
     - Node validation rules
     - Node compatibility matrix

  3. Edge Validation
     - Edge connection rules
     - Node compatibility 驗證
     - Circular dependency 檢測
     - Data flow 驗證

  4. Workflow Validation API
     - Workflow schema 驗證
     - Node configuration 驗證
     - Edge connectivity 驗證
     - 錯誤訊息與建議

業務價值:
  對 IT 開發者:
    - 可以透過 API 創建和管理 Workflow Definition
    - 支援 Workflow 版本控制和重用
    - 驗證 Workflow 正確性,減少錯誤

  對業務分析師:
    - 為未來視覺化編輯器提供 Backend 支持
    - 確保 Workflow 定義的正確性和一致性

  對企業管理者:
    - 為 Phase 1D (工作流編輯器 Frontend) 奠定基礎
    - 實現 Workflow 集中管理和版本控制

戰略意義:
  - 為視覺化工作流編輯器提供完整 Backend API
  - 支援 Workflow 的 CRUD、驗證、版本控制
  - 與 n8n、AutoGen 競爭的差異化能力基礎

參考文檔:
  - claudedocs/1-planning/MVP-SCOPE-DEFINITION.md (Phase 1C 完整定義)
  - docs/api/workflow-api-design.md (API 設計規範)
  - docs/database/workflow-schema.md (數據模型設計)
```

### Sprint 10-11 計劃概覽

**Sprint 分配策略**:
```yaml
Sprint 10 (本 Sprint):
  重點: Workflow Definition CRUD + Node Registry
  Story Points: 8 SP
  核心交付:
    - Workflow Definition 實體與 API (3-4 SP)
    - Node Registry 註冊機制 (2-3 SP)
    - Edge Validation 基礎 (2-3 SP)
  完成度: Phase 1C 0% → 50%

Sprint 11 (下一個 Sprint):
  重點: Workflow Validation + 整合測試
  Story Points: 8 SP
  核心交付:
    - Workflow Validation API (3-4 SP)
    - Schema Versioning (2-3 SP)
    - 整合測試與文檔 (2-3 SP)
  完成度: Phase 1C 50% → 100%

Phase 1C 完成標準:
  ✅ Workflow Definition CRUD API 100% 實現
  ✅ Node Registry 完整註冊和查詢
  ✅ Edge Validation 規則完整實現
  ✅ Workflow Validation API 正常運作
  ✅ 測試覆蓋率 >85%
  ✅ API 文檔完整 (Swagger)
  ✅ Phase 1D 準備度檢查通過

參考文檔:
  - claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md (Sprint 10-11 詳細分配)
```

### 與 Phase 1D (Frontend) 的銜接

**Phase 1D 準備度評估**:
```yaml
Phase 1D Overview:
  名稱: 工作流編輯器 Frontend (Workflow Editor Frontend)
  Sprint: Sprint 12-14 (9 週)
  技術棧: Vue 3 + VueFlow + Module Federation
  Story Points: 35-45 SP

Phase 1C 為 Phase 1D 奠定的基礎:
  ✅ Workflow Definition API:
     - 完整 CRUD 端點供 Frontend 調用
     - Workflow metadata 和版本管理

  ✅ Node Registry API:
     - Node types 查詢和 schema 獲取
     - Node validation rules 供 Frontend 使用

  ✅ Edge Validation API:
     - Edge connection 驗證
     - Real-time validation feedback

  ✅ Workflow Validation API:
     - 完整 Workflow 驗證
     - 錯誤訊息和修復建議

技術銜接要求:
  🔍 API 穩定性: RESTful API 設計穩定,版本化
  🔍 API 文檔: Swagger/OpenAPI 3.0 完整文檔
  🔍 錯誤處理: 標準化錯誤格式和狀態碼
  🔍 性能基準: API 響應時間 P95 <200ms

Phase 1D 前置條件檢查:
  - ✅ Phase 1C (Sprint 10-11) 100% 完成
  - ✅ Workflow API 穩定並有完整文檔
  - ✅ Node Registry 和 Validation API 驗收通過
  - ✅ 性能和安全測試通過
  - 📋 詳見: Sprint 11 Retrospective (Phase 1D 準備度評估)

參考文檔:
  - claudedocs/1-planning/MVP-SCOPE-DEFINITION.md (Phase 1D 定義)
  - docs/architecture/ADR-012-workflow-editor-technology.md (Frontend 技術選型)
  - docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-index.md (UI 設計)
```

---

## Sprint 目標

### 核心目標

實現 **工作流編輯器 Backend API (Phase 1C Part 1)**,建立支援視覺化工作流編輯器的完整 Backend API,透過 Workflow Definition CRUD + Node Registry + Edge Validation 實現 Workflow 的定義、驗證和管理。

#### 主要目標 (Primary Goals)

1. **Workflow Definition CRUD (3-4 SP)** - 完整 Workflow 定義的創建、讀取、更新、刪除
2. **Node Registry (2-3 SP)** - Node type 註冊機制和 schema 定義
3. **Edge Validation (2-3 SP)** - Edge connection 規則和驗證邏輯

#### 次要目標 (Secondary Goals)

4. **API 設計與文檔** - RESTful API 設計和 Swagger 文檔
5. **Database Schema** - WorkflowDefinition 數據模型設計
6. **基礎測試** - 單元測試和整合測試

### 業務價值 (Business Value)

```yaml
對 IT 開發者 (IT Developer):
  價值: 可以透過 API 創建和管理 Workflow Definition
  影響: 支援 Workflow 的程式化創建和版本控制
  參考: docs/ux-design/user-research/personas.md (Alex - IT Developer)

對業務分析師 (Business Analyst):
  價值: 為未來視覺化編輯器提供 Backend 支持
  影響: 確保 Workflow 定義的正確性和一致性
  參考: docs/ux-design/user-research/personas.md (Emma - Business Analyst)

對企業管理者 (Enterprise Admin):
  價值: Workflow 集中管理和版本控制能力
  影響: 為企業級工作流管理奠定基礎
  參考: docs/ux-design/user-research/personas.md (David - Enterprise Admin)

戰略意義:
  - 為 Phase 1D (工作流編輯器 Frontend) 提供完整 API 支持
  - 實現與 n8n、AutoGen 競爭的差異化編輯器能力
  - 支援 Workflow 的集中管理、版本控制和重用
  - 為企業級工作流管理平台奠定基礎

參考文檔:
  - docs/ux-design/user-research/personas.md (三大用戶角色)
  - docs/ux-design/user-research/user-journey-maps.md (Workflow 創建旅程)
```

### 技術目標 (Technical Goals)

```yaml
API 設計:
  - RESTful API 設計原則
  - 版本化 API (v1)
  - 標準化錯誤處理
  - 完整 Swagger/OpenAPI 文檔
  參考: docs/api/workflow-api-design.md

數據模型:
  - WorkflowDefinition 實體設計
  - NodeDefinition 和 EdgeDefinition 設計
  - 版本控制支持 (Version field)
  - Soft delete 支持
  參考: docs/database/workflow-schema.md

驗證邏輯:
  - Workflow schema 驗證 (JSON Schema)
  - Node configuration 驗證
  - Edge connectivity 驗證
  - Circular dependency 檢測
  參考: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md

性能目標:
  - API 響應時間 P95 <200ms
  - Workflow 驗證延遲 <500ms
  - 支援 Workflow definition >1000 nodes
  參考: docs/performance/performance-targets.md

參考文檔:
  - docs/api/workflow-api-design.md (API 設計規範)
  - docs/database/workflow-schema.md (數據模型設計)
  - docs/performance/performance-targets.md (性能目標)
```

---

## User Stories 分配

### Sprint 10 User Stories

**US 7.1 - Workflow Editor Backend API (8 SP)** - **本 Sprint 開始**

```yaml
User Story:
  作為: IT 開發者
  我想要: 透過 API 創建和管理 Workflow Definition
  以便: 可以程式化地定義工作流並支援版本控制

Story Points: 8 SP (本 Sprint 完成 Part 1)

Sprint 10 範圍 (Part 1 - 50%):
  ✅ Workflow Definition CRUD:
     - WorkflowDefinition 實體與 Repository
     - Workflow metadata 管理 (Name, Description, Version)
     - CRUD API 端點實現
     - Soft delete 支持

  ✅ Node Registry:
     - Node type 註冊機制
     - Node schema 定義 (JSON Schema)
     - Node validation rules
     - Node types 查詢 API

  ✅ Edge Validation:
     - Edge connection rules
     - Node compatibility 驗證
     - Circular dependency 檢測
     - Data flow 驗證

Sprint 11 範圍 (Part 2 - 50%):
  ⏳ Workflow Validation API (3-4 SP)
  ⏳ Schema Versioning (2-3 SP)
  ⏳ 整合測試與文檔 (2-3 SP)

參考文檔:
  - docs/user-stories/modules/module-07/US-7.1-Workflow-Editor-Backend-API.md
  - docs/api/workflow-api-design.md (API 設計)
  - docs/database/workflow-schema.md (數據模型)
```

### Sprint 10 技術任務分解

**Phase 1: Workflow Definition CRUD (3-4 SP)**

```yaml
Task 1.1 - WorkflowDefinition 實體設計 (0.5 SP):
  檔案:
    - src/Core/Domain/Entities/Workflows/WorkflowDefinition.cs
    - src/Core/Domain/Entities/Workflows/NodeDefinition.cs
    - src/Core/Domain/Entities/Workflows/EdgeDefinition.cs
  內容:
    - WorkflowDefinition 實體 (Id, Name, Description, Version, etc.)
    - NodeDefinition 實體 (NodeId, Type, Configuration, etc.)
    - EdgeDefinition 實體 (SourceNodeId, TargetNodeId, Condition, etc.)
    - Domain events (WorkflowDefinitionCreated, etc.)
  驗收標準:
    - 實體設計符合 DDD 原則
    - 包含完整的 validation rules
    - Soft delete 支持
  參考: docs/database/workflow-schema.md

Task 1.2 - Repository 實現 (0.5 SP):
  檔案:
    - src/Infrastructure/Persistence/Repositories/WorkflowDefinitionRepository.cs
  內容:
    - IWorkflowDefinitionRepository 介面
    - WorkflowDefinitionRepository 實現
    - EF Core configuration
    - 包含 Nodes 和 Edges 的完整查詢
  驗收標準:
    - Repository 實現 CRUD 操作
    - 包含單元測試
  參考: docs/architecture/adr/ADR-004-repository-pattern.md

Task 1.3 - CQRS Commands/Queries (1.0 SP):
  檔案:
    - src/Application/UseCases/Workflows/Commands/CreateWorkflowDefinitionCommand.cs
    - src/Application/UseCases/Workflows/Commands/UpdateWorkflowDefinitionCommand.cs
    - src/Application/UseCases/Workflows/Queries/GetWorkflowDefinitionByIdQuery.cs
    - src/Application/UseCases/Workflows/Queries/GetWorkflowDefinitionsQuery.cs
  內容:
    - CreateWorkflowDefinitionCommand + Handler
    - UpdateWorkflowDefinitionCommand + Handler
    - DeleteWorkflowDefinitionCommand + Handler (soft delete)
    - GetWorkflowDefinitionByIdQuery + Handler
    - GetWorkflowDefinitionsQuery + Handler (分頁)
  驗收標準:
    - 完整 CQRS pattern 實現
    - FluentValidation 驗證
    - 包含單元測試
  參考: docs/architecture/adr/ADR-005-cqrs-pattern.md

Task 1.4 - API Controllers (1.0 SP):
  檔案:
    - src/API/Controllers/WorkflowDefinitionsController.cs
  內容:
    - POST /api/v1/workflow-definitions (創建)
    - GET /api/v1/workflow-definitions (列表,分頁)
    - GET /api/v1/workflow-definitions/{id} (取得單一)
    - PUT /api/v1/workflow-definitions/{id} (更新)
    - DELETE /api/v1/workflow-definitions/{id} (軟刪除)
  驗收標準:
    - RESTful API 設計原則
    - 完整 Swagger 註解
    - 錯誤處理和狀態碼標準化
    - 整合測試覆蓋
  參考: docs/api/workflow-api-design.md

Task 1.5 - Database Migration (0.5 SP):
  檔案:
    - src/Infrastructure/Migrations/20260421_AddWorkflowDefinition.cs
  內容:
    - WorkflowDefinitions 表
    - NodeDefinitions 表
    - EdgeDefinitions 表
    - 索引和外鍵約束
  驗收標準:
    - Migration 可以正常 up/down
    - 包含測試數據 seed
  參考: docs/database/workflow-schema.md
```

**Phase 2: Node Registry (2-3 SP)**

```yaml
Task 2.1 - Node Type Registry Service (1.0 SP):
  檔案:
    - src/Application/Services/NodeTypeRegistry.cs
    - src/Core/Domain/Models/NodeTypeDefinition.cs
  內容:
    - INodeTypeRegistry 介面
    - NodeTypeRegistry 實現
    - NodeTypeDefinition 模型 (Type, Schema, ValidationRules, etc.)
    - 內建 Node types 註冊 (Agent, Tool, Condition, etc.)
  驗收標準:
    - 支援動態註冊 Node types
    - 包含 JSON Schema 定義
    - 包含 validation rules
    - 單元測試覆蓋
  參考: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md

Task 2.2 - Node Schema Validation (0.5 SP):
  檔案:
    - src/Application/Validators/NodeConfigurationValidator.cs
  內容:
    - JSON Schema 驗證
    - Node configuration 驗證
    - 錯誤訊息生成
  驗收標準:
    - 支援 JSON Schema Draft 7
    - 詳細錯誤訊息
    - 單元測試覆蓋
  參考: docs/api/workflow-api-design.md (Validation 章節)

Task 2.3 - Node Registry API (0.5 SP):
  檔案:
    - src/API/Controllers/NodeTypesController.cs
  內容:
    - GET /api/v1/node-types (取得所有 Node types)
    - GET /api/v1/node-types/{type} (取得特定 Node type)
    - GET /api/v1/node-types/{type}/schema (取得 JSON Schema)
  驗收標準:
    - RESTful API 設計
    - 完整 Swagger 文檔
    - 整合測試
  參考: docs/api/workflow-api-design.md
```

**Phase 3: Edge Validation (2-3 SP)**

```yaml
Task 3.1 - Edge Validation Service (1.0 SP):
  檔案:
    - src/Application/Services/EdgeValidationService.cs
  內容:
    - IEdgeValidationService 介面
    - EdgeValidationService 實現
    - Edge connection rules (Node compatibility)
    - Circular dependency 檢測 (DFS algorithm)
    - Data flow 驗證
  驗收標準:
    - 完整驗證邏輯
    - 詳細錯誤訊息
    - 單元測試覆蓋 (包含 circular dependency cases)
  參考: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md

Task 3.2 - Node Compatibility Matrix (0.5 SP):
  檔案:
    - src/Core/Domain/Models/NodeCompatibilityMatrix.cs
  內容:
    - Node compatibility 定義
    - Compatibility rules (哪些 Node types 可以連接)
    - Compatibility 查詢 API
  驗收標準:
    - 清晰的 compatibility 定義
    - 易於擴展新 Node types
    - 單元測試覆蓋
  參考: docs/api/workflow-api-design.md

Task 3.3 - Edge Validation API (0.5 SP):
  檔案:
    - src/API/Controllers/WorkflowValidationController.cs
  內容:
    - POST /api/v1/workflow-validation/edge (驗證單一 Edge)
    - POST /api/v1/workflow-validation/workflow (驗證完整 Workflow)
  驗收標準:
    - RESTful API 設計
    - 詳細驗證結果和錯誤訊息
    - 完整 Swagger 文檔
    - 整合測試
  參考: docs/api/workflow-api-design.md
```

**Phase 4: 整合測試與文檔 (1-2 SP)**

```yaml
Task 4.1 - 整合測試 (0.5 SP):
  檔案:
    - tests/IntegrationTests/API/WorkflowDefinitionsControllerTests.cs
    - tests/IntegrationTests/API/NodeTypesControllerTests.cs
    - tests/IntegrationTests/API/WorkflowValidationControllerTests.cs
  內容:
    - Workflow Definition CRUD API 測試
    - Node Registry API 測試
    - Edge Validation API 測試
    - 錯誤處理測試
  驗收標準:
    - 覆蓋所有 API 端點
    - Happy path + Error cases
    - 使用 TestContainers (PostgreSQL)
  參考: docs/testing/integration-testing-guidelines.md

Task 4.2 - API 文檔 (0.5 SP):
  檔案:
    - docs/api/workflow-editor-api-v1.md
  內容:
    - 完整 API 文檔 (Swagger/OpenAPI 3.0)
    - 請求/響應範例
    - 錯誤代碼說明
    - 使用範例 (cURL, C#, TypeScript)
  驗收標準:
    - 文檔清晰易懂
    - 包含完整範例
    - 與實際 API 一致
  參考: docs/api/api-documentation-standards.md

Task 4.3 - Sprint 回顧文檔 (0.5 SP):
  檔案:
    - claudedocs/2-sprints/sprint-10/SPRINT-10-7-RETROSPECTIVE.md
  內容:
    - Sprint 完成總結
    - 技術決策記錄
    - 問題與解決方案
    - Phase 1C Part 1 完成度評估
  驗收標準:
    - 詳細回顧報告
    - 為 Sprint 11 提供基礎
  參考: claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md (格式參考)
```

---

## 技術範圍

### 核心技術領域

```yaml
1. API 設計:
   - RESTful API 原則
   - 版本化 (v1)
   - Swagger/OpenAPI 3.0
   - 標準化錯誤處理
   參考: docs/api/workflow-api-design.md

2. Domain Model:
   - WorkflowDefinition 實體
   - NodeDefinition 實體
   - EdgeDefinition 實體
   - Domain events
   參考: docs/database/workflow-schema.md

3. CQRS Pattern:
   - Commands (Create, Update, Delete)
   - Queries (Get, List)
   - MediatR handlers
   - FluentValidation
   參考: docs/architecture/adr/ADR-005-cqrs-pattern.md

4. Validation Logic:
   - JSON Schema validation
   - Node configuration validation
   - Edge connectivity validation
   - Circular dependency detection
   參考: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md

5. Testing:
   - 單元測試 (Domain, Application)
   - 整合測試 (API, Database)
   - TestContainers (PostgreSQL)
   參考: docs/testing/integration-testing-guidelines.md
```

### 技術棧

```yaml
Backend:
  - .NET 9 (C# 12)
  - ASP.NET Core 9.0
  - Entity Framework Core 9.0
  - MediatR 12.4
  - FluentValidation 11.11
  - Swashbuckle (Swagger)

Database:
  - PostgreSQL 16
  - EF Core Migrations
  - 參考: docs/database/workflow-schema.md

Testing:
  - xUnit 2.6
  - FluentAssertions 6.12
  - Moq 4.20
  - TestContainers 3.6
  - 參考: docs/testing/unit-testing-guidelines.md

Documentation:
  - Swagger/OpenAPI 3.0
  - Markdown
  - 參考: docs/api/api-documentation-standards.md
```

### 不在本 Sprint 範圍內

```yaml
不包含 (延後到 Sprint 11):
  - Workflow Validation 完整實現 (複雜驗證邏輯)
  - Schema Versioning (版本遷移)
  - Workflow Template 系統
  - Performance optimization (caching, indexing)
  - 參考: Sprint 11 計劃

不包含 (延後到 Phase 1D):
  - 工作流編輯器 Frontend (Vue 3 + VueFlow)
  - 視覺化編輯器 UI
  - Real-time collaboration (Yjs CRDT)
  - 參考: Phase 1D 計劃

不包含 (延後到 Phase 1E):
  - 高級編排模式 (Conditional, Loop, Handoff)
  - Supervisor Agent
  - Workflow execution monitoring
  - 參考: Phase 1E 計劃
```

---

## 預期交付

### 核心交付物

**1. Workflow Definition CRUD API**

```yaml
API 端點:
  - POST /api/v1/workflow-definitions
  - GET /api/v1/workflow-definitions
  - GET /api/v1/workflow-definitions/{id}
  - PUT /api/v1/workflow-definitions/{id}
  - DELETE /api/v1/workflow-definitions/{id}

功能:
  - 創建 Workflow Definition (包含 Nodes 和 Edges)
  - 取得 Workflow Definition 列表 (分頁)
  - 取得單一 Workflow Definition (包含完整結構)
  - 更新 Workflow Definition
  - 刪除 Workflow Definition (soft delete)

驗收標準:
  ✅ 所有 API 端點正常運作
  ✅ RESTful 設計原則
  ✅ 標準化錯誤處理
  ✅ 完整 Swagger 文檔
  ✅ 整合測試覆蓋

參考: docs/api/workflow-api-design.md
```

**2. Node Registry**

```yaml
Node Types:
  - Agent (執行 Agent)
  - Tool (調用 Tool/Plugin)
  - Condition (條件判斷 - 基礎)
  - Parallel (並行執行)
  - Sequential (順序執行)

API 端點:
  - GET /api/v1/node-types
  - GET /api/v1/node-types/{type}
  - GET /api/v1/node-types/{type}/schema

功能:
  - 取得所有可用 Node types
  - 取得特定 Node type 詳情
  - 取得 Node type JSON Schema (用於驗證 configuration)

驗收標準:
  ✅ 5 種基礎 Node types 註冊
  ✅ 每種 Node type 有完整 JSON Schema
  ✅ Node configuration 驗證正常
  ✅ API 端點正常運作
  ✅ 單元測試和整合測試覆蓋

參考: docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
```

**3. Edge Validation**

```yaml
驗證規則:
  - Node compatibility (哪些 Node types 可以連接)
  - Circular dependency 檢測 (避免無限循環)
  - Data flow 驗證 (輸出類型匹配輸入類型)

API 端點:
  - POST /api/v1/workflow-validation/edge (驗證單一 Edge)
  - POST /api/v1/workflow-validation/workflow (驗證完整 Workflow)

功能:
  - 驗證 Edge connection 是否合法
  - 檢測 Workflow 是否有 circular dependency
  - 驗證 data flow 是否正確
  - 提供詳細錯誤訊息和修復建議

驗收標準:
  ✅ Edge connection rules 完整實現
  ✅ Circular dependency 檢測正確 (DFS algorithm)
  ✅ 詳細錯誤訊息
  ✅ API 端點正常運作
  ✅ 單元測試和整合測試覆蓋 (包含 edge cases)

參考: docs/api/workflow-api-design.md
```

### 文檔交付物

```yaml
1. API 文檔:
   - docs/api/workflow-editor-api-v1.md
   - Swagger/OpenAPI 3.0 規範
   - 包含請求/響應範例
   - 錯誤代碼說明

2. 數據庫 Schema:
   - docs/database/workflow-schema.md (更新)
   - EF Core Migrations
   - 包含索引和約束

3. 技術文檔:
   - Node Registry 設計文檔
   - Edge Validation 設計文檔
   - Validation 錯誤訊息規範

4. 測試報告:
   - 單元測試報告 (覆蓋率 >85%)
   - 整合測試報告
   - 測試案例列表

5. Sprint 回顧:
   - SPRINT-10-7-RETROSPECTIVE.md
   - 技術決策記錄
   - 問題與解決方案
   - Phase 1C Part 1 完成度評估

參考: docs/api/api-documentation-standards.md
```

### 代碼交付物

```yaml
Domain Layer:
  - WorkflowDefinition.cs (實體)
  - NodeDefinition.cs (實體)
  - EdgeDefinition.cs (實體)
  - Domain events

Application Layer:
  - Workflow Definition Commands/Queries (CQRS)
  - NodeTypeRegistry.cs (Node Registry Service)
  - EdgeValidationService.cs (Edge Validation Service)
  - Validators (FluentValidation)

Infrastructure Layer:
  - WorkflowDefinitionRepository.cs (Repository)
  - EF Core configurations
  - Migrations

API Layer:
  - WorkflowDefinitionsController.cs
  - NodeTypesController.cs
  - WorkflowValidationController.cs

Tests:
  - 單元測試 (Domain, Application)
  - 整合測試 (API, Database)

參考: docs/architecture/architecture-design-document.md
```

---

## 風險與依賴

### 依賴項

**前置依賴 (Sprint 1-9)**:
```yaml
✅ Phase 1A (Sprint 1-6) 必須 100% 完成:
   - Agent Management (US 1.1-1.3)
   - Plugin System (US 2.1-2.2)
   - Persona Framework (US 3.1)
   - Knowledge Base (US 4.1)
   - Code Interpreter (US 5.1)
   - Text-to-SQL (US 5.2)

✅ Phase 1B (Sprint 7-9) 必須 100% 完成:
   - Task Generator (US 6.1 Part 1)
   - Multi-Agent Coordination (US 6.1 Part 2)
   - Workflow Execution Engine (US 6.1 Part 3)
   - Framework Abstraction Layer (US 6.1 Part 4)

驗收標準:
  ✅ Sprint 9 Retrospective 驗收通過
  ✅ Phase 1B 功能測試 100% 通過
  ✅ 無已知 P0/P1 bugs
  ✅ 技術債務清單整理完成

參考文檔:
  - claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md (Phase 1B 完成驗收)
```

**平行依賴 (Sprint 10 內部)**:
```yaml
Task 依賴關係:
  Phase 1 (Workflow Definition CRUD):
    - Task 1.1 (實體設計) → Task 1.2 (Repository)
    - Task 1.2 (Repository) → Task 1.3 (CQRS)
    - Task 1.3 (CQRS) → Task 1.4 (API Controllers)
    - Task 1.4 (API Controllers) ← Task 1.5 (Migrations)

  Phase 2 (Node Registry):
    - Task 2.1 (Registry Service) → Task 2.2 (Schema Validation)
    - Task 2.2 (Schema Validation) → Task 2.3 (API)

  Phase 3 (Edge Validation):
    - Task 3.1 (Validation Service) → Task 3.2 (Compatibility Matrix)
    - Task 3.2 (Compatibility Matrix) → Task 3.3 (API)

  Phase 4 (整合測試):
    - All phases must complete → Task 4.1 (Integration Tests)
    - Task 4.1 → Task 4.2 (Documentation)
    - Task 4.2 → Task 4.3 (Retrospective)

關鍵路徑: Phase 1 → Phase 2/3 (並行) → Phase 4
```

**後續依賴 (Sprint 11)**:
```yaml
Sprint 11 依賴 Sprint 10 交付:
  ⏳ Workflow Definition API 穩定並有文檔
  ⏳ Node Registry 完整實現
  ⏳ Edge Validation 基礎實現
  ⏳ Database schema 建立完成

Sprint 11 將建立在以上基礎上:
  - Workflow Validation 完整實現
  - Schema Versioning
  - Performance optimization
  - 完整整合測試

參考文檔:
  - claudedocs/1-planning/DEPENDENCY-MATRIX.md (Sprint 10-11 依賴分析)
```

### 風險識別

**RISK-027: Node Registry 可擴展性設計 (中等風險)**

```yaml
風險描述:
  Node Registry 設計可能不夠靈活
  未來新增 Node types 可能需要大量修改
  第三方 Node types 整合可能困難

影響:
  - 開發進度延遲
  - 代碼耦合度高
  - 難以支援第三方擴展

發生概率: 35%

緩解措施:
  ✅ 參考 n8n Node Registry 設計 (成熟架構)
  ✅ 使用 JSON Schema 定義 Node configuration (靈活)
  ✅ Plugin 架構支援動態載入 Node types
  ✅ 充分的單元測試覆蓋

殘餘風險: 15%

參考文檔:
  - docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
  - docs/architecture/adr/ADR-012-workflow-editor-technology.md
```

**RISK-028: Edge Validation 複雜度 (中等風險)**

```yaml
風險描述:
  Edge validation 邏輯可能比預期複雜
  Circular dependency 檢測可能有 edge cases
  Data flow validation 可能不夠準確

影響:
  - 開發時間增加
  - Bug 密度可能較高
  - 用戶體驗受影響 (錯誤的驗證結果)

發生概率: 30%

緩解措施:
  ✅ 使用標準 DFS 算法檢測 circular dependency
  ✅ 充分的測試案例 (包含複雜 Workflow)
  ✅ 參考 n8n, AutoGen 的驗證邏輯
  ✅ 詳細的錯誤訊息和修復建議

殘餘風險: 10%

參考文檔:
  - docs/api/workflow-api-design.md (Validation 章節)
```

**RISK-029: Workflow Schema 演進策略 (低風險)**

```yaml
風險描述:
  Workflow schema 未來可能需要變更
  缺乏版本遷移機制可能導致問題

影響:
  - 舊 Workflow definition 無法使用
  - 需要手動遷移數據

發生概率: 25%

緩解措施:
  ✅ Workflow Definition 包含 Version 欄位
  ✅ Schema versioning 將在 Sprint 11 實現
  ✅ 設計預留擴展空間
  ✅ 充分的文檔記錄 schema 變更

殘餘風險: 5%

參考文檔:
  - docs/database/workflow-schema.md (Versioning 章節)
```

### 風險總結

| 風險 | 優先級 | 概率 | 影響 | 殘餘風險 | 狀態 | 參考 |
|------|--------|------|------|---------|------|------|
| Node Registry 可擴展性 | 中 | 35% | 中 | 15% | 🟡 設計審查 | RISK-027 |
| Edge Validation 複雜度 | 中 | 30% | 中 | 10% | 🟡 充分測試 | RISK-028 |
| Workflow Schema 演進 | 低 | 25% | 低 | 5% | ✅ 版本控制 | RISK-029 |

**完整風險分析**: 參考 [claudedocs/1-planning/RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md) (30+ risks tracked)

---

## 成功指標

### 功能完整性

```yaml
Workflow Definition CRUD:
  ✅ 5 個 API 端點 100% 實現
  ✅ CRUD 操作正常運作
  ✅ 包含 Nodes 和 Edges 的完整結構
  ✅ Soft delete 支持
  ✅ 整合測試覆蓋

Node Registry:
  ✅ 5 種基礎 Node types 註冊
  ✅ JSON Schema 定義完整
  ✅ Node configuration 驗證正常
  ✅ 3 個 API 端點正常運作
  ✅ 單元測試覆蓋

Edge Validation:
  ✅ Edge connection rules 實現
  ✅ Circular dependency 檢測正確
  ✅ Data flow 驗證基礎實現
  ✅ 2 個 API 端點正常運作
  ✅ 單元測試覆蓋 (包含 edge cases)

參考: docs/development-standards/definition-of-done.md
```

### 質量指標

```yaml
測試覆蓋率:
  - 單元測試: ≥85% (Domain + Application Layer)
  - 整合測試: 覆蓋所有 API 端點
  - 測試通過率: 100%
  參考: docs/testing/unit-testing-guidelines.md

代碼質量:
  - 無 P0/P1 Bugs
  - Code Review 100% 通過
  - SonarQube Quality Gate 通過
  - 技術債務 ≤5 個 (P2/P3)
  參考: docs/development-standards/code-quality-standards.md

文檔完整性:
  - API 文檔 100% 完整 (Swagger)
  - 設計文檔更新
  - 測試報告完整
  - Sprint 回顧文檔完成
  參考: docs/api/api-documentation-standards.md
```

### 性能指標

```yaml
API 響應時間:
  - GET /api/v1/workflow-definitions (列表): P95 <200ms
  - GET /api/v1/workflow-definitions/{id}: P95 <150ms
  - POST /api/v1/workflow-definitions: P95 <300ms
  - PUT /api/v1/workflow-definitions/{id}: P95 <300ms
  參考: docs/performance/performance-targets.md

Validation 性能:
  - Edge validation: <100ms per edge
  - Workflow validation: <500ms per workflow (up to 100 nodes)
  - Circular dependency detection: <200ms (up to 100 nodes)
  參考: docs/performance/performance-targets.md

Database 性能:
  - Workflow Definition 查詢: <100ms
  - 包含 Nodes/Edges 的完整查詢: <200ms
  - 分頁查詢: <150ms
  參考: docs/database/performance-optimization.md
```

### Phase 1C Part 1 完成度

```yaml
Sprint 10 完成標準:
  ✅ US 7.1 Part 1 (50%) 完成
  ✅ Workflow Definition CRUD API 100% 實現
  ✅ Node Registry 100% 實現
  ✅ Edge Validation 基礎實現
  ✅ 測試覆蓋率 >85%
  ✅ API 文檔完整
  ✅ Sprint 回顧文檔完成

Phase 1C 整體進度:
  - 完成度: 0% → 50%
  - 下一步: Sprint 11 (Workflow Validation + Schema Versioning)
  - 預期完成: Sprint 11 結束

參考文檔:
  - claudedocs/1-planning/MVP-SCOPE-DEFINITION.md (Phase 1C 完整定義)
```

---

## 相關文檔

### 核心文檔

```yaml
規劃文檔:
  - claudedocs/1-planning/MVP-SCOPE-DEFINITION.md (Phase 1C 定義)
  - claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md (Sprint 10-11 分析)
  - claudedocs/1-planning/DEVELOPMENT-STRATEGY.md (開發策略)
  - claudedocs/1-planning/DEPENDENCY-MATRIX.md (依賴分析)

架構文檔:
  - docs/architecture/Architecture-Design-Document.md (系統架構)
  - docs/architecture/ADR-012-workflow-editor-technology.md (技術選型)
  - docs/architecture/C4-architecture-diagrams.md (C4 架構圖)

API 文檔:
  - docs/api/workflow-api-design.md (API 設計規範)
  - docs/api/api-documentation-standards.md (文檔標準)

Database 文檔:
  - docs/database/workflow-schema.md (Workflow Schema 設計)
  - docs/database/database-schema.md (完整 Schema)
```

### 技術實施文檔

```yaml
Backend 實施:
  - docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
  - docs/development-standards/coding-standards.md (編碼標準)
  - docs/development-standards/code-quality-standards.md (質量標準)

測試文檔:
  - docs/testing/unit-testing-guidelines.md (單元測試)
  - docs/testing/integration-testing-guidelines.md (整合測試)
  - docs/testing/test-strategy.md (測試策略)

性能文檔:
  - docs/performance/performance-targets.md (性能目標)
  - docs/performance/performance-testing.md (性能測試)
```

### User Stories 與 UI/UX

```yaml
User Stories:
  - docs/user-stories/modules/module-07/US-7.1-Workflow-Editor-Backend-API.md
  - docs/user-stories/implementation-strategy.md (實施策略)

UI/UX 文檔:
  - docs/ux-design/user-research/personas.md (用戶角色)
  - docs/ux-design/user-research/user-journey-maps.md (用戶旅程)
  - docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-index.md (編輯器設計)
```

### Sprint 文檔

```yaml
前置 Sprint:
  - claudedocs/2-sprints/sprint-7/ (Task Generator + Coordination)
  - claudedocs/2-sprints/sprint-8/ (Execution Engine + Messaging)
  - claudedocs/2-sprints/sprint-9/ (Framework Abstraction Layer)

本 Sprint (Sprint 10):
  - claudedocs/2-sprints/sprint-10/SPRINT-10-1-OVERVIEW.md (本文檔)
  - claudedocs/2-sprints/sprint-10/SPRINT-10-2-PLAN.md (實施計劃)
  - claudedocs/2-sprints/sprint-10/SPRINT-10-3-CONTEXT.md (技術上下文)
  - claudedocs/2-sprints/sprint-10/SPRINT-10-4-CHECKLIST.md (任務清單)
  - claudedocs/2-sprints/sprint-10/SPRINT-10-5-DEV-LOG.md (開發日誌)
  - claudedocs/2-sprints/sprint-10/SPRINT-10-6-ISSUES.md (問題追蹤)
  - claudedocs/2-sprints/sprint-10/SPRINT-10-7-RETROSPECTIVE.md (回顧)

下一個 Sprint:
  - claudedocs/2-sprints/sprint-11/ (Workflow Validation + Schema Versioning)
```

---

## 完整參考文獻索引

### 項目管理文檔 (Project Management)

```yaml
1. claudedocs/1-planning/MVP-SCOPE-DEFINITION.md
   - Phase 1C 完整定義
   - Sprint 10-11 範圍
   - 驗收標準

2. claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md
   - Sprint 10-11 Story Points 分析
   - 任務分解
   - 時間估算

3. claudedocs/1-planning/DEVELOPMENT-STRATEGY.md
   - 工作流編輯器開發策略
   - Phase 1C-1D 銜接
   - 技術路線圖

4. claudedocs/1-planning/DEPENDENCY-MATRIX.md
   - Sprint 10 依賴分析
   - 關鍵路徑追蹤
   - 風險評估

5. claudedocs/1-planning/RISK-REGISTER.md
   - RISK-027: Node Registry 可擴展性
   - RISK-028: Edge Validation 複雜度
   - RISK-029: Workflow Schema 演進

6. claudedocs/1-planning/TECHNICAL-DECISIONS-LOG.md
   - TD-076+: Sprint 10 技術決策
   - API 設計決策
   - Validation 策略決策

7. docs/project-management/Project-Management-Plan.md
   - 項目時間線
   - 資源分配
   - 溝通計劃

8. docs/project-management/sprint-planning.md
   - Sprint 規劃流程
   - 估算方法
   - 回顧機制
```

### 架構與設計文檔 (Architecture & Design)

```yaml
9. docs/architecture/Architecture-Design-Document.md
   - 系統整體架構
   - 工作流編輯器架構
   - 技術棧選型

10. docs/architecture/ADR-012-workflow-editor-technology.md
    - 工作流編輯器技術選型
    - Vue 3 + VueFlow 決策
    - Module Federation 決策

11. docs/architecture/ADR-005-cqrs-pattern.md
    - CQRS pattern 應用
    - Command/Query 分離
    - MediatR 使用指南

12. docs/architecture/ADR-004-repository-pattern.md
    - Repository pattern 設計
    - EF Core 配置
    - Transaction 管理

13. docs/architecture/C4-architecture-diagrams.md
    - C4 Context Diagram
    - C4 Container Diagram
    - C4 Component Diagram (Workflow Editor)

14. docs/architecture/performance-scalability-strategy.md
    - 性能策略
    - 擴展性設計
    - 優化方向
```

### API 與數據庫文檔 (API & Database)

```yaml
15. docs/api/workflow-api-design.md
    - Workflow Definition API 設計
    - Node Registry API 設計
    - Edge Validation API 設計
    - 請求/響應格式
    - 錯誤處理規範

16. docs/api/api-documentation-standards.md
    - API 文檔標準
    - Swagger/OpenAPI 規範
    - 範例編寫指南

17. docs/database/workflow-schema.md
    - WorkflowDefinition 表設計
    - NodeDefinition 表設計
    - EdgeDefinition 表設計
    - 索引和約束
    - Versioning 策略

18. docs/database/database-schema.md
    - 完整數據庫 Schema
    - ER Diagram
    - 數據字典

19. docs/database/performance-optimization.md
    - 查詢優化
    - 索引策略
    - Caching 策略
```

### 技術實施文檔 (Technical Implementation)

```yaml
20. docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md
    - 工作流編排實施指南
    - Node Registry 設計
    - Edge Validation 設計
    - Circular dependency 檢測算法

21. docs/technical-implementation/01-backend-net9/02-domain-layer-implementation.md
    - Domain 實體設計
    - Domain events
    - Aggregate 設計

22. docs/technical-implementation/01-backend-net9/03-application-layer-implementation.md
    - CQRS 實施
    - Use Cases 實現
    - Validators 設計

23. docs/technical-implementation/01-backend-net9/04-infrastructure-layer-implementation.md
    - Repository 實現
    - EF Core configuration
    - Migrations 管理

24. docs/technical-implementation/01-backend-net9/05-api-layer-implementation.md
    - Controllers 實現
    - Middleware 配置
    - Error handling
```

### 開發標準文檔 (Development Standards)

```yaml
25. docs/development-standards/coding-standards.md
    - C# coding standards
    - Naming conventions
    - Code organization

26. docs/development-standards/code-quality-standards.md
    - Quality gates
    - SonarQube rules
    - Code review guidelines

27. docs/development-standards/definition-of-done.md
    - Sprint DoD
    - Feature DoD
    - Release DoD

28. docs/development-standards/Non-Functional-Requirements.md
    - Performance requirements
    - Security requirements
    - Reliability requirements
```

### 測試文檔 (Testing)

```yaml
29. docs/testing/unit-testing-guidelines.md
    - Unit test 編寫規範
    - Test naming conventions
    - Mocking strategies

30. docs/testing/integration-testing-guidelines.md
    - Integration test 編寫規範
    - TestContainers 使用
    - API testing strategies

31. docs/testing/test-strategy.md
    - 測試策略
    - 測試金字塔
    - Coverage targets

32. docs/testing/performance-testing.md
    - Performance testing guidelines
    - Load testing strategies
    - Benchmarking methods
```

### 性能與安全文檔 (Performance & Security)

```yaml
33. docs/performance/performance-targets.md
    - API 響應時間目標
    - Database 查詢性能目標
    - Validation 性能目標

34. docs/security/Security-Requirements.md
    - API 安全要求
    - Authentication/Authorization
    - Input validation
```

### User Stories 與 UI/UX 文檔

```yaml
35. docs/user-stories/modules/module-07/US-7.1-Workflow-Editor-Backend-API.md
    - US 7.1 完整描述
    - 驗收標準
    - 技術實施指南

36. docs/user-stories/implementation-strategy.md
    - 實施策略
    - 技術難點
    - 解決方案

37. docs/ux-design/user-research/personas.md
    - Alex (IT Developer)
    - Emma (Business Analyst)
    - David (Enterprise Admin)

38. docs/ux-design/user-research/user-journey-maps.md
    - Workflow 創建旅程
    - Workflow 編輯旅程
    - Pain points 分析

39. docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-index.md
    - 工作流編輯器 UI 設計
    - 節點面板設計
    - Canvas 設計
```

### Sprint 文檔 (Sprint Documentation)

```yaml
40. claudedocs/2-sprints/sprint-7/SPRINT-7-1-OVERVIEW.md
    - Sprint 7 概覽
    - Task Generator + Coordination Layer
    - Phase 1B 啟動

41. claudedocs/2-sprints/sprint-7/SPRINT-7-7-RETROSPECTIVE.md
    - Sprint 7 回顧
    - 技術決策
    - 經驗教訓

42. claudedocs/2-sprints/sprint-8/SPRINT-8-1-OVERVIEW.md
    - Sprint 8 概覽
    - Execution Engine + Messaging

43. claudedocs/2-sprints/sprint-8/SPRINT-8-7-RETROSPECTIVE.md
    - Sprint 8 回顧
    - 技術決策
    - 經驗教訓

44. claudedocs/2-sprints/sprint-9/SPRINT-9-1-OVERVIEW.md
    - Sprint 9 概覽
    - Framework Abstraction Layer
    - Phase 1B 完成

45. claudedocs/2-sprints/sprint-9/SPRINT-9-7-RETROSPECTIVE.md
    - Sprint 9 回顧
    - Phase 1B 完成驗收
    - Phase 1C 準備度評估
```

### 其他重要文檔

```yaml
46. docs/brief.md
    - 項目背景
    - 業務需求
    - 競爭分析

47. CLAUDE.md
    - 項目概述
    - 開發環境設置
    - 常用命令

48. README.md
    - 項目簡介
    - 快速開始
    - 貢獻指南
```

---

## 使用指南

### 如何使用本文檔

**開發人員視角**:
```yaml
Sprint 開始前:
  1. 閱讀 "Phase 1C 啟動說明" → 理解 Phase 轉換背景
  2. 閱讀 "Sprint 目標" → 理解本 Sprint 要實現什麼
  3. 閱讀 "User Stories 分配" → 理解任務分解和優先級
  4. 查看 "技術範圍" → 理解使用的技術和框架

開發過程中:
  5. 參考 "預期交付" → 理解具體交付物要求
  6. 查看 "SPRINT-10-2-PLAN.md" → 獲取詳細實施指南
  7. 使用 "SPRINT-10-4-CHECKLIST.md" → 追蹤任務完成進度
  8. 記錄 "SPRINT-10-5-DEV-LOG.md" → 記錄開發日誌和決策

遇到問題時:
  9. 查看 "風險與依賴" → 確認是否為已知風險
  10. 記錄 "SPRINT-10-6-ISSUES.md" → 追蹤問題和解決方案
  11. 參考 "完整參考文獻索引" → 查找相關文檔

Sprint 結束時:
  12. 檢查 "成功指標" → 驗證是否達成目標
  13. 完成 "SPRINT-10-7-RETROSPECTIVE.md" → 總結經驗教訓
```

**項目經理視角**:
```yaml
Sprint 規劃:
  1. 閱讀 "Sprint 定位與參考" → 理解 Sprint 在整體計劃中的位置
  2. 查看 "User Stories 分配" → 確認 Story Points 和任務分解
  3. 檢查 "風險與依賴" → 識別潛在阻塞和風險

Sprint 追蹤:
  4. 監控 "SPRINT-10-4-CHECKLIST.md" → 追蹤任務進度
  5. 查看 "SPRINT-10-6-ISSUES.md" → 了解當前問題
  6. 檢查 "成功指標" → 評估 Sprint 進度

Sprint 回顧:
  7. 審閱 "SPRINT-10-7-RETROSPECTIVE.md" → 評估 Sprint 成果
  8. 確認 "預期交付" → 驗證所有交付物已完成
```

### 文檔導航指南

```yaml
快速導航:
  - 想了解 Phase 1C 啟動背景? → 查看 "Phase 1C 啟動說明"
  - 想了解具體做什麼? → 查看 "Sprint 目標" 和 "User Stories 分配"
  - 想了解怎麼做? → 查看 "SPRINT-10-2-PLAN.md"
  - 想了解技術細節? → 查看 "SPRINT-10-3-CONTEXT.md"
  - 想追蹤任務進度? → 查看 "SPRINT-10-4-CHECKLIST.md"
  - 想查看開發日誌? → 查看 "SPRINT-10-5-DEV-LOG.md"
  - 想了解問題和解決方案? → 查看 "SPRINT-10-6-ISSUES.md"
  - 想了解 Sprint 結果? → 查看 "SPRINT-10-7-RETROSPECTIVE.md"

參考文檔查找:
  - API 設計規範: docs/api/workflow-api-design.md
  - 數據庫 Schema: docs/database/workflow-schema.md
  - 架構決策: docs/architecture/ADR-012-workflow-editor-technology.md
  - 測試指南: docs/testing/integration-testing-guidelines.md
  - 性能目標: docs/performance/performance-targets.md
```

### 最佳實踐

```yaml
文檔維護:
  - 每日更新 DEV-LOG (記錄進度和決策)
  - 及時記錄 ISSUES (問題和解決方案)
  - 每週更新 CHECKLIST (任務完成狀態)
  - Sprint 結束前完成 RETROSPECTIVE

協作溝通:
  - 使用文檔中的參考鏈接快速查找信息
  - 在 DEV-LOG 中記錄重要的技術決策
  - 在 ISSUES 中追蹤阻塞問題
  - 在 RETROSPECTIVE 中分享經驗教訓

質量保證:
  - 對照 "成功指標" 自我檢查
  - 參考 "預期交付" 確認交付物完整性
  - 使用 "CHECKLIST" 確保沒有遺漏任務
  - 查看 "風險與依賴" 確認沒有未處理的風險
```

---

## 版本歷史

### v2.1 (2025-11-14) - Initial Version

```yaml
創建人: Claude (AI Assistant)
創建日期: 2025-11-14

內容:
  - 建立 Sprint 10 完整 Overview 文檔
  - Phase 1C 啟動說明 (Phase 1B → Phase 1C 轉換)
  - Sprint 目標與範圍定義
  - User Stories 分配與任務分解
  - 風險識別與緩解措施
  - 完整參考文獻索引 (48 個文檔)

文檔結構:
  - 遵循 v2.1 標準 8 欄位 header
  - 格式 100% 與 Sprint 5-9 一致
  - 包含 Phase 轉換特殊章節
  - 包含完整參考索引和使用指南

參考文件:
  - claudedocs/2-sprints/sprint-7/SPRINT-7-1-OVERVIEW.md (格式參考)
  - claudedocs/2-sprints/sprint-9/SPRINT-9-1-OVERVIEW.md (格式參考)
  - claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md (內容來源)
  - claudedocs/1-planning/MVP-SCOPE-DEFINITION.md (Phase 定義)

變更記錄:
  - [2025-11-14] Initial creation
  - 新增 "Phase 1C 啟動說明" 章節
  - 新增 Sprint 10-11 連貫性說明
  - 新增與 Phase 1D 銜接說明
  - 新增 RISK-027, RISK-028, RISK-029
  - 完整參考文獻索引 (48 個文檔)
```

---

**文檔所有者**: AI Agent Platform Development Team
**審閱者**: Tech Lead, Project Manager
**批准者**: System Architect
**下次審閱日期**: 2026-05-11 (Sprint 10 結束時)

---

**相關 Sprint 文檔**:
- [Sprint 10 實施計劃](./SPRINT-10-2-PLAN.md)
- [Sprint 10 技術上下文](./SPRINT-10-3-CONTEXT.md)
- [Sprint 10 任務清單](./SPRINT-10-4-CHECKLIST.md)
- [Sprint 10 開發日誌](./SPRINT-10-5-DEV-LOG.md)
- [Sprint 10 問題追蹤](./SPRINT-10-6-ISSUES.md)
- [Sprint 10 回顧](./SPRINT-10-7-RETROSPECTIVE.md)

**Phase 文檔連結**:
- [Phase 1B 完成回顧](../sprint-9/SPRINT-9-7-RETROSPECTIVE.md)
- [Phase 1C 定義](../../1-planning/MVP-SCOPE-DEFINITION.md)
- [Phase 1D 準備](../../1-planning/MVP-SCOPE-DEFINITION.md)

---

*本文檔是 AI Agent Platform 項目 Sprint 10 的官方 Overview,提供 Sprint 定位、Phase 轉換說明、目標範圍、風險管理和完整參考索引。*

*最後更新: 2025-11-14*
