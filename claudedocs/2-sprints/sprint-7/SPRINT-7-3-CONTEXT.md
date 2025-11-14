# SPRINT-7-3-CONTEXT.md - Sprint 7 開發上下文：Workflow 工作流編排引擎基礎

**版本**: v2.1
**Sprint 編號**: Sprint 7
**Sprint 週期**: Week 19-21 (3 週)
**Phase**: Phase 1B - 工作流引擎核心 (Workflow Engine Core)
**計劃日期**: 2026-02-17 ~ 2026-03-09
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📑 目錄 (Table of Contents)

- [使用說明](#-使用說明-how-to-use-this-document)
- [一、核心技術參考層 (Reference Layer)](#一核心技術參考層-reference-layer)
  - [1.1 Module & Epic 引用](#11-module--epic-引用)
  - [1.2 架構設計文檔](#12-架構設計文檔)
  - [1.3 技術決策記錄 (ADR)](#13-技術決策記錄-adr)
  - [1.4 工作流設計引用](#14-工作流設計引用)
  - [1.5 API 設計引用](#15-api-設計引用)
- [二、US 6.1: Workflow 編排引擎基礎 (Part 1) - 詳細技術上下文](#二us-61-workflow-編排引擎基礎-part-1---詳細技術上下文)
  - [2.1 MVP 範圍定義與技術決策](#21-mvp-範圍定義與技術決策)
  - [2.2 Task Generator 架構設計](#22-task-generator-架構設計)
  - [2.3 Coordination Layer 設計](#23-coordination-layer-設計)
  - [2.4 Workflow State Machine 設計](#24-workflow-state-machine-設計)
  - [2.5 API 端點設計](#25-api-端點設計)
  - [2.6 基礎 UI 實作](#26-基礎-ui-實作)
- [三、完整文檔引用清單 (按功能分類)](#三完整文檔引用清單-按功能分類)
- [使用指南](#-使用指南)
- [更新日誌](#-更新日誌)

---

## 📖 使用說明 (How to Use This Document)

**本文檔定位**: "Just-in-Time" 技術參考手冊

**適用場景**:
- ✅ 開發 US 6.1 時，需要查詢 Task Generator 設計 → 第二章 2.2
- ✅ 開發 US 6.1 時，需要查詢 Coordination Layer 設計 → 第二章 2.3
- ✅ 需要快速找到 Workflow State Machine 設計 → 2.4 章節
- ✅ 需要理解 API 端點設計 → 2.5 章節
- ✅ 需要快速找到架構設計文檔 → 第一章 Reference Layer
- ✅ 需要理解技術決策背景 → 各章節的「關鍵技術決策」部分

**不適用場景**:
- ❌ 尋找 Sprint 整體進度與成果 → 使用 [SPRINT-7-1-OVERVIEW.md](./SPRINT-7-1-OVERVIEW.md)
- ❌ 尋找詳細開發計劃與任務清單 → 使用 [SPRINT-7-2-PLAN.md](./SPRINT-7-2-PLAN.md)
- ❌ 追蹤開發進度與 Checklist → 使用 [SPRINT-7-4-CHECKLIST.md](./SPRINT-7-4-CHECKLIST.md)

---

## 一、核心技術參考層 (Reference Layer)

### 1.1 Module & Epic 引用

**User Story 完整規格**:
- 📖 [US 7: Workflow Engine](../../../docs/user-stories/us-7-workflow-engine.md) - Workflow Engine 完整規格
  - **Section**: [US 6.1 - Workflow 編排引擎基礎 Part 1](../../../docs/user-stories/us-7-workflow-engine.md#us-61) - Workflow 編排驗收標準
  - **關鍵內容**: Task Generator、Coordination Layer、Workflow State Machine、API 端點、基礎 UI

**Epic 文檔**:
- 📖 [Epic 07: Workflow Engine](../../../docs/user-stories/epics/epic-07-workflow-engine.md) - Workflow Engine 功能完整策略
  - **用途**: 理解 Workflow Engine 在整體系統中的定位
  - **關鍵內容**: BDD 場景、驗收標準、Phase 1B → Phase 2 演進路徑
  - **Phase 1B 範圍**: Task Generator、Coordination Layer、基本狀態機（本 Sprint Part 1）
  - **Phase 2 延後**: 完整狀態機、錯誤處理、恢復機制、可視化編輯器

### 1.2 架構設計文檔

**核心架構決策記錄 (ADR)**:

**1. Workflow Orchestration Strategy** - [ADR-009](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- **決策**: Task-based Orchestration + Event-driven Coordination
- **關鍵原則**: 任務分解、事件驅動、狀態管理、錯誤處理
- **Section**: [Task Generator Design](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md#task-generator-design) - Task Generator 設計策略
- **Section**: [Coordination Layer](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md#coordination-layer) - Coordination Layer 架構
- **Section**: [State Machine](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md#state-machine) - Workflow State Machine 設計
- **實施影響**: US 6.1 的 Workflow 編排設計基於此決策

**2. Multi-Agent Communication** - [ADR-007](../../../docs/architecture/adr/ADR-007-multi-agent-communication.md)
- **決策**: Event Bus + Direct Invocation 混合模式
- **關鍵設計**: Agent 間通訊、事件發布/訂閱、直接調用
- **Section**: [Event Bus Design](../../../docs/architecture/adr/ADR-007-multi-agent-communication.md#event-bus-design) - Event Bus 架構
- **Section**: [Direct Invocation](../../../docs/architecture/adr/ADR-007-multi-agent-communication.md#direct-invocation) - Agent 直接調用機制
- **實施影響**: US 6.1 的 Agent 通訊基於此設計

**3. Hybrid State Management** - [ADR-006](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
- **決策**: Redis（工作流狀態快取）+ PostgreSQL（工作流執行記錄持久化）
- **關鍵設計**: 執行狀態實時追蹤（Redis），歷史記錄查詢（PostgreSQL）
- **Section**: [Redis Caching Strategy](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md#redis-caching-strategy) - Workflow 狀態快取
- **Section**: [PostgreSQL Persistence](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md#postgresql-persistence) - Workflow 執行記錄持久化
- **實施影響**: US 6.1 的 Workflow 狀態管理基於混合儲存策略

**完整架構設計文檔**:
- 🏗️ [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 系統架構總覽
  - **Section**: [Workflow Engine Architecture](../../../docs/architecture/Architecture-Design-Document.md#workflow-engine-architecture) - Workflow Engine 系統架構圖
  - **Section**: [Task Generator Design](../../../docs/architecture/Architecture-Design-Document.md#task-generator-design) - Task Generator 完整設計
  - **Section**: [Coordination Layer](../../../docs/architecture/Architecture-Design-Document.md#coordination-layer) - Coordination Layer 架構
  - **Section**: [State Machine](../../../docs/architecture/Architecture-Design-Document.md#state-machine) - Workflow State Machine 流程圖

### 1.3 技術決策記錄 (ADR)

**Workflow Engine 相關 ADR 完整清單**:

| ADR 編號 | 標題 | 關鍵決策 | 實施影響 |
|---------|------|---------|---------|
| [ADR-009](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md) | Workflow Orchestration Strategy | Task-based + Event-driven | US 6.1 Workflow 編排設計 |
| [ADR-007](../../../docs/architecture/adr/ADR-007-multi-agent-communication.md) | Multi-Agent Communication | Event Bus + Direct Invocation | US 6.1 Agent 通訊 |
| [ADR-006](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md) | Hybrid State Management | Redis + PostgreSQL 混合儲存 | US 6.1 Workflow 狀態管理 |
| [ADR-001](../../../docs/architecture/adr/ADR-001-clean-architecture.md) | Clean Architecture | Domain → Application → Infrastructure → API | 整體分層架構 |
| [ADR-002](../../../docs/architecture/adr/ADR-002-cqrs-pattern.md) | CQRS Pattern | MediatR Commands/Queries 分離 | US 6.1 業務邏輯 |
| [ADR-003](../../../docs/architecture/adr/ADR-003-repository-pattern.md) | Repository Pattern | IWorkflowRepository 抽象 | US 6.1 資料存取 |

### 1.4 工作流設計引用

**Workflow Engine 設計**:
- 📋 [Workflow Engine Design](../../../docs/technical-implementation/workflow-engine-design.md) - Workflow Engine 完整設計文檔
  - **Section**: [Task Generator](../../../docs/technical-implementation/workflow-engine-design.md#task-generator) - Task Generator 詳細設計
    - LLM-based Task Decomposition
    - Task 依賴關係分析
    - Task 優先級排序
    - Task 執行計劃生成

  - **Section**: [Coordination Layer](../../../docs/technical-implementation/workflow-engine-design.md#coordination-layer) - Coordination Layer 詳細設計
    - Multi-Agent 協作機制
    - Task 分配策略
    - Agent 狀態追蹤
    - 並行執行控制

  - **Section**: [State Machine](../../../docs/technical-implementation/workflow-engine-design.md#state-machine) - Workflow State Machine 設計
    - Workflow 生命週期管理
    - 狀態轉換規則
    - 事件觸發機制
    - 錯誤處理策略

**Task Generator 設計**:
- 📋 [Task Generator Design](../../../docs/technical-implementation/task-generator-design.md) - Task Generator 詳細設計文檔 (New)
  - **Section**: [LLM Integration](../../../docs/technical-implementation/task-generator-design.md#llm-integration) - LLM 整合策略
  - **Section**: [Task Decomposition](../../../docs/technical-implementation/task-generator-design.md#task-decomposition) - Task 分解演算法
  - **Section**: [Dependency Analysis](../../../docs/technical-implementation/task-generator-design.md#dependency-analysis) - 依賴關係分析

**Coordination Layer 設計**:
- 📋 [Coordination Layer Design](../../../docs/technical-implementation/coordination-layer-design.md) - Coordination Layer 詳細設計文檔 (New)
  - **Section**: [Agent Selection](../../../docs/technical-implementation/coordination-layer-design.md#agent-selection) - Agent 選擇策略
  - **Section**: [Task Assignment](../../../docs/technical-implementation/coordination-layer-design.md#task-assignment) - Task 分配機制
  - **Section**: [Parallel Execution](../../../docs/technical-implementation/coordination-layer-design.md#parallel-execution) - 並行執行控制

### 1.5 API 設計引用

**Workflow API 設計規範**:
- 📄 [Workflow API Design](../../../docs/api/workflow-api-design.md) - Workflow RESTful API 完整設計 (New)
  - **Section**: [Create Workflow Endpoint](../../../docs/api/workflow-api-design.md#create-workflow-endpoint) - 創建 Workflow API 詳細設計
    - `POST /api/v1/workflows` - 創建 Workflow（US 6.1）
    - Request: goal, requirements, constraints
    - Response: workflowId, status, generatedTasks, estimatedDuration

  - **Section**: [Get Workflow Status Endpoint](../../../docs/api/workflow-api-design.md#get-workflow-status-endpoint) - 獲取 Workflow 狀態 API
    - `GET /api/v1/workflows/{id}` - 獲取 Workflow 狀態（US 6.1）
    - Response: workflowId, status, currentTask, progress, completedTasks

  - **Section**: [Start Workflow Endpoint](../../../docs/api/workflow-api-design.md#start-workflow-endpoint) - 啟動 Workflow API
    - `POST /api/v1/workflows/{id}/start` - 啟動 Workflow（US 6.1）

  - **Section**: [Request/Response Format](../../../docs/api/workflow-api-design.md#request-response-format) - 標準格式定義
  - **Section**: [Error Handling](../../../docs/api/workflow-api-design.md#error-handling) - 統一錯誤處理策略
  - **Section**: [Validation Rules](../../../docs/api/workflow-api-design.md#validation-rules) - API 驗證規則

**API 請求/回應範例**:

**1. 創建 Workflow API**:
```http
POST /api/v1/workflows
Content-Type: application/json
Authorization: Bearer {token}

Request Body:
{
  "goal": "分析產品銷售數據並生成報告",
  "requirements": {
    "dataSource": "sales_database",
    "timeRange": "2024-01-01 to 2024-12-31",
    "outputFormat": "PDF"
  },
  "constraints": {
    "maxDuration": "30m",
    "maxCost": 100
  }
}

Response (201 Created):
{
  "workflowId": "uuid",
  "status": "created",
  "generatedTasks": [
    {
      "taskId": "task-1",
      "name": "數據提取",
      "agent": "DataAnalystAgent",
      "dependencies": []
    },
    {
      "taskId": "task-2",
      "name": "數據分析",
      "agent": "DataAnalystAgent",
      "dependencies": ["task-1"]
    },
    {
      "taskId": "task-3",
      "name": "報告生成",
      "agent": "ReportGeneratorAgent",
      "dependencies": ["task-2"]
    }
  ],
  "estimatedDuration": "25m",
  "createdAt": "2026-02-17T10:00:00Z"
}
```

**2. 啟動 Workflow API**:
```http
POST /api/v1/workflows/{workflowId}/start
Authorization: Bearer {token}

Response (202 Accepted):
{
  "workflowId": "uuid",
  "status": "running",
  "currentTask": "task-1",
  "startedAt": "2026-02-17T10:01:00Z"
}
```

**3. 獲取 Workflow 狀態 API**:
```http
GET /api/v1/workflows/{workflowId}
Authorization: Bearer {token}

Response (200 OK):
{
  "workflowId": "uuid",
  "status": "running",
  "currentTask": {
    "taskId": "task-2",
    "name": "數據分析",
    "status": "in_progress",
    "progress": 60
  },
  "completedTasks": [
    {
      "taskId": "task-1",
      "name": "數據提取",
      "completedAt": "2026-02-17T10:05:00Z"
    }
  ],
  "progress": 40,
  "estimatedTimeRemaining": "15m"
}
```

---

## 二、US 6.1: Workflow 編排引擎基礎 (Part 1) - 詳細技術上下文

### 2.1 MVP 範圍定義與技術決策

**完整 User Story 規格**: [US 7: Workflow Engine](../../../docs/user-stories/us-7-workflow-engine.md)

**MVP 範圍邊界**:

✅ **本 Sprint 必須實現 (P0)**:
1. **Task Generator (LLM-based)** - 基於 LLM 的任務分解
   - LLM Prompt Engineering for Task Decomposition
   - Task 依賴關係分析
   - Task 優先級排序
   - Task 執行計劃生成（JSON 格式）
   - **參考**: [Task Generator Design](../../../docs/technical-implementation/task-generator-design.md)

2. **Coordination Layer** - Multi-Agent 協作協調層
   - Agent 選擇策略（基於 Task 類型）
   - Task 分配機制（串行執行優先）
   - Agent 狀態追蹤
   - 簡化版並行執行控制（最多 2 個並行 Task）
   - **參考**: [Coordination Layer Design](../../../docs/technical-implementation/coordination-layer-design.md)

3. **Workflow State Machine (Basic)** - 基本工作流狀態機
   - Workflow 生命週期管理（Created → Running → Completed/Failed）
   - 狀態轉換規則
   - 事件觸發機制（狀態變更事件）
   - 基本錯誤處理（Task 失敗 → Workflow 失敗）
   - **參考**: [Workflow State Machine Design](../../../docs/technical-implementation/workflow-state-machine-design.md)

4. **Workflow API** - Workflow 管理 API
   - `POST /api/v1/workflows` - 創建 Workflow
   - `POST /api/v1/workflows/{id}/start` - 啟動 Workflow
   - `GET /api/v1/workflows/{id}` - 獲取 Workflow 狀態
   - Request/Response 標準格式
   - **參考**: [Workflow API Design](../../../docs/api/workflow-api-design.md)

5. **基礎 UI** - Workflow 管理基礎介面
   - Workflow 創建表單
   - Workflow 執行狀態顯示
   - Task 列表展示
   - 簡易進度條
   - **參考**: [Workflow UI Design](../../../docs/ux-design/workflow-ui-design.md)

❌ **明確排除 Phase 2 功能 (延後)**:
- 完整狀態機（暫停、恢復、取消狀態） - Phase 2
- 進階錯誤處理與恢復機制 - Phase 2
- 可視化工作流編輯器（VueFlow） - Phase 2
- Workflow 模板系統 - Phase 2
- 複雜並行執行控制（>2 並行 Task） - Phase 2
- Workflow 版本控制 - Phase 2

**關鍵技術決策**:

**決策 1: Task Decomposition 策略**
- **選擇**: LLM-based Task Decomposition (MVP) vs Rule-based Decomposition (Phase 2)
- **理由**: LLM 提供靈活的任務分解能力，適應多樣化的目標需求
- **替代方案**: Rule-based Decomposition（Phase 2 補充），適用於標準化流程
- **參考**: [ADR-009: Workflow Orchestration Strategy](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- **MVP 限制**: 僅支援簡單的線性和並行任務分解，複雜條件分支留待 Phase 2
- **Phase 2 升級**: 支援條件分支、循環、子工作流

**決策 2: Agent Selection 策略**
- **選擇**: Type-based Agent Selection (MVP)
- **理由**: 根據 Task 類型（Data Analysis, Report Generation, Code Execution）自動選擇對應 Agent
- **替代方案**: Capability-based Selection（Phase 2），根據 Agent 能力動態選擇
- **參考**: [Coordination Layer Design](../../../docs/technical-implementation/coordination-layer-design.md#agent-selection)
- **MVP 實現**: 固定映射（Task Type → Agent Type）
- **Phase 2 升級**: 動態能力評估、負載均衡

**決策 3: Parallel Execution 策略**
- **選擇**: Limited Parallel Execution (最多 2 個並行 Task)
- **理由**: MVP 簡化並行控制複雜度，確保 3 週內完成
- **替代方案**: Full Parallel Execution（Phase 2），支援任意數量並行 Task
- **參考**: [Coordination Layer Design](../../../docs/technical-implementation/coordination-layer-design.md#parallel-execution)
- **MVP 限制**: 最多 2 個並行 Task，超過則串行執行
- **Phase 2 升級**: 動態並行度控制、資源管理

**決策 4: State Management 策略**
- **選擇**: Redis (執行狀態快取) + PostgreSQL (執行記錄持久化)
- **理由**: Redis 提供快速狀態查詢，PostgreSQL 提供可靠的歷史記錄
- **參考**: [ADR-006: Hybrid State Management](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
- **MVP 實現**: 狀態實時更新到 Redis，Task 完成時持久化到 PostgreSQL
- **Phase 2 升級**: 狀態快照、Checkpoint 恢復

**決策 5: Error Handling 策略**
- **選擇**: Simple Fail-Fast (MVP)
- **理由**: Task 失敗 → Workflow 失敗，簡化錯誤處理邏輯
- **替代方案**: Retry & Recovery（Phase 2），支援自動重試和恢復
- **參考**: [Workflow State Machine Design](../../../docs/technical-implementation/workflow-state-machine-design.md#error-handling)
- **MVP 限制**: 無自動重試，無 Workflow 暫停/恢復
- **Phase 2 升級**: 自動重試、失敗處理策略、Workflow 恢復

**決策 6: UI 實現策略**
- **選擇**: Basic React UI (MVP) vs VueFlow Visual Editor (Phase 2)
- **理由**: MVP 聚焦核心功能驗證，基礎 UI 滿足最小需求
- **參考**: [Workflow UI Design](../../../docs/ux-design/workflow-ui-design.md)
- **MVP 實現**: 表單輸入、狀態顯示、進度條
- **Phase 2 升級**: VueFlow 可視化編輯器、拖拽式 Workflow 設計

### 2.2 Task Generator 架構設計

**完整設計文檔**: [Task Generator Design](../../../docs/technical-implementation/task-generator-design.md)

**ITaskGeneratorService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/ITaskGeneratorService.cs`
- **核心方法**: `GenerateTasksAsync(WorkflowGoal goal, CancellationToken cancellationToken)`

**核心方法簽名**:
```csharp
public interface ITaskGeneratorService
{
    Task<TaskGenerationResult> GenerateTasksAsync(
        WorkflowGoal goal,
        CancellationToken cancellationToken = default);

    Task<TaskDependencyGraph> AnalyzeDependenciesAsync(
        IEnumerable<GeneratedTask> tasks,
        CancellationToken cancellationToken = default);
}

public sealed class WorkflowGoal
{
    public required string Goal { get; init; }
    public Dictionary<string, object>? Requirements { get; init; }
    public Dictionary<string, object>? Constraints { get; init; }
}

public sealed class TaskGenerationResult
{
    public required IReadOnlyList<GeneratedTask> Tasks { get; init; }
    public required TaskDependencyGraph DependencyGraph { get; init; }
    public required TimeSpan EstimatedDuration { get; init; }
}

public sealed class GeneratedTask
{
    public required string TaskId { get; init; }
    public required string Name { get; init; }
    public required string Description { get; init; }
    public required string AgentType { get; init; }
    public required IReadOnlyList<string> Dependencies { get; init; }
    public int Priority { get; init; }
    public TimeSpan EstimatedDuration { get; init; }
}
```

**Task Generation 流程**:
```
1. LLM Prompt 構建 → 將 WorkflowGoal 轉換為 LLM Prompt
2. LLM 調用 → 使用 Semantic Kernel 調用 LLM 進行任務分解
3. Task 解析 → 解析 LLM 返回的 JSON 格式任務列表
4. Dependency 分析 → 分析任務依賴關係，構建 DAG
5. Priority 排序 → 基於依賴關係計算任務優先級
6. Duration 估算 → 估算每個任務和整體執行時間
7. Validation → 驗證任務列表的有效性（無循環依賴等）
```

**LLM Prompt Engineering**:
```csharp
// LLM Prompt Template for Task Decomposition
var promptTemplate = @"
你是一個專業的任務分解專家。請將以下目標分解為具體的執行任務。

目標: {{$goal}}
需求: {{$requirements}}
限制: {{$constraints}}

請返回 JSON 格式的任務列表，每個任務包含:
- taskId: 唯一標識符
- name: 任務名稱
- description: 任務描述
- agentType: 執行此任務的 Agent 類型 (DataAnalystAgent, ReportGeneratorAgent, CodeExecutorAgent 等)
- dependencies: 依賴的任務 ID 列表 (如果沒有依賴則為空數組)
- estimatedDuration: 預估執行時間 (ISO 8601 duration format, 例如 PT5M 表示 5 分鐘)

範例格式:
{
  \"tasks\": [
    {
      \"taskId\": \"task-1\",
      \"name\": \"數據提取\",
      \"description\": \"從數據庫提取銷售數據\",
      \"agentType\": \"DataAnalystAgent\",
      \"dependencies\": [],
      \"estimatedDuration\": \"PT5M\"
    },
    {
      \"taskId\": \"task-2\",
      \"name\": \"數據分析\",
      \"description\": \"分析銷售趨勢和異常\",
      \"agentType\": \"DataAnalystAgent\",
      \"dependencies\": [\"task-1\"],
      \"estimatedDuration\": \"PT10M\"
    }
  ]
}

請確保:
1. 任務之間的依賴關係正確
2. 沒有循環依賴
3. 任務粒度適中（每個任務 5-30 分鐘）
4. Agent 類型選擇合理
";
```

**TaskGeneratorService 實作細節**:
- **位置**: `AIAgentPlatform.Infrastructure/Services/TaskGeneratorService.cs`
- **依賴注入**:
  - ISemanticKernelService（LLM 調用）
  - ILogger<TaskGeneratorService>（日誌記錄）
  - IOptions<TaskGeneratorOptions>（配置選項）

**TaskGeneratorOptions 配置**:
```csharp
public sealed class TaskGeneratorOptions
{
    public string ModelName { get; init; } = "gpt-4"; // LLM Model
    public int MaxTokens { get; init; } = 2000; // Max response tokens
    public double Temperature { get; init; } = 0.7; // LLM temperature
    public int MaxTasks { get; init; } = 20; // Max tasks per workflow
    public TimeSpan DefaultTaskDuration { get; init; } = TimeSpan.FromMinutes(10);
}
```

**Dependency Analysis 演算法**:
```csharp
// 構建 DAG (Directed Acyclic Graph)
public sealed class TaskDependencyGraph
{
    private readonly Dictionary<string, TaskNode> _nodes;

    public TaskDependencyGraph(IEnumerable<GeneratedTask> tasks)
    {
        _nodes = tasks.ToDictionary(t => t.TaskId, t => new TaskNode(t));

        // 建立依賴關係
        foreach (var task in tasks)
        {
            foreach (var depId in task.Dependencies)
            {
                if (_nodes.TryGetValue(depId, out var depNode))
                {
                    _nodes[task.TaskId].AddDependency(depNode);
                }
            }
        }

        // 驗證無循環依賴
        ValidateAcyclic();
    }

    // Topological Sort for execution order
    public IReadOnlyList<GeneratedTask> GetExecutionOrder()
    {
        var sorted = new List<GeneratedTask>();
        var visited = new HashSet<string>();

        foreach (var node in _nodes.Values.Where(n => !n.HasDependencies))
        {
            Visit(node, visited, sorted);
        }

        return sorted;
    }

    private void Visit(TaskNode node, HashSet<string> visited, List<GeneratedTask> sorted)
    {
        if (visited.Contains(node.Task.TaskId)) return;

        visited.Add(node.Task.TaskId);

        foreach (var dep in node.Dependencies)
        {
            Visit(dep, visited, sorted);
        }

        sorted.Add(node.Task);
    }
}
```

**Task Generator 效能指標**:
- **LLM 調用時間**: <10 秒（目標）
- **Task 生成數量**: 5-20 個 Tasks（目標）
- **Dependency 分析時間**: <1 秒（目標）
- **生成成功率**: ≥90%（目標）

**參考**: [Task Generator Design](../../../docs/technical-implementation/task-generator-design.md)

### 2.3 Coordination Layer 設計

**完整設計文檔**: [Coordination Layer Design](../../../docs/technical-implementation/coordination-layer-design.md)

**ICoordinationService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/ICoordinationService.cs`
- **核心方法**:
  - `AssignTasksToAgentsAsync` - Task 分配
  - `ExecuteWorkflowAsync` - Workflow 執行
  - `MonitorWorkflowProgressAsync` - 進度監控

**核心方法簽名**:
```csharp
public interface ICoordinationService
{
    Task<TaskAssignmentResult> AssignTasksToAgentsAsync(
        IEnumerable<GeneratedTask> tasks,
        CancellationToken cancellationToken = default);

    Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
        Guid workflowId,
        TaskAssignmentResult assignment,
        CancellationToken cancellationToken = default);

    Task<WorkflowProgress> MonitorWorkflowProgressAsync(
        Guid workflowId,
        CancellationToken cancellationToken = default);
}

public sealed class TaskAssignmentResult
{
    public required IReadOnlyDictionary<string, string> TaskAgentMapping { get; init; }
    public required ExecutionPlan ExecutionPlan { get; init; }
}

public sealed class ExecutionPlan
{
    public required IReadOnlyList<ExecutionPhase> Phases { get; init; }
    public required TimeSpan EstimatedDuration { get; init; }
}

public sealed class ExecutionPhase
{
    public int PhaseNumber { get; init; }
    public required IReadOnlyList<string> TaskIds { get; init; }
    public bool CanExecuteInParallel { get; init; }
}
```

**Agent Selection 策略**:
```csharp
// Type-based Agent Selection (MVP)
public sealed class AgentSelector
{
    private readonly Dictionary<string, string> _taskTypeToAgentType = new()
    {
        { "DataAnalysis", "DataAnalystAgent" },
        { "ReportGeneration", "ReportGeneratorAgent" },
        { "CodeExecution", "CodeExecutorAgent" },
        { "TextGeneration", "TextGeneratorAgent" },
        { "DataVisualization", "VisualizationAgent" }
    };

    public string SelectAgent(GeneratedTask task)
    {
        // 簡化版: 根據 AgentType 直接映射
        return task.AgentType;
    }
}
```

**Task Assignment 機制**:
```csharp
// CoordinationService 實作
public async Task<TaskAssignmentResult> AssignTasksToAgentsAsync(
    IEnumerable<GeneratedTask> tasks,
    CancellationToken cancellationToken)
{
    var taskAgentMapping = new Dictionary<string, string>();
    var agentSelector = new AgentSelector();

    // 1. 為每個 Task 選擇 Agent
    foreach (var task in tasks)
    {
        var agentType = agentSelector.SelectAgent(task);
        taskAgentMapping[task.TaskId] = agentType;
    }

    // 2. 生成執行計劃
    var executionPlan = GenerateExecutionPlan(tasks);

    return new TaskAssignmentResult
    {
        TaskAgentMapping = taskAgentMapping,
        ExecutionPlan = executionPlan
    };
}

private ExecutionPlan GenerateExecutionPlan(IEnumerable<GeneratedTask> tasks)
{
    var dependencyGraph = new TaskDependencyGraph(tasks);
    var phases = new List<ExecutionPhase>();

    // Topological Sort + Phase Grouping
    var remainingTasks = dependencyGraph.GetExecutionOrder().ToList();
    var phaseNumber = 1;

    while (remainingTasks.Any())
    {
        // 找出無依賴的任務（可並行執行）
        var readyTasks = remainingTasks
            .Where(t => !t.Dependencies.Any(d => remainingTasks.Any(r => r.TaskId == d)))
            .ToList();

        if (!readyTasks.Any())
            throw new InvalidOperationException("Circular dependency detected");

        // MVP: 最多 2 個並行 Task
        var canParallel = readyTasks.Count <= 2;

        phases.Add(new ExecutionPhase
        {
            PhaseNumber = phaseNumber++,
            TaskIds = readyTasks.Select(t => t.TaskId).ToList(),
            CanExecuteInParallel = canParallel
        });

        remainingTasks.RemoveAll(t => readyTasks.Contains(t));
    }

    var estimatedDuration = phases.Sum(p =>
        p.CanExecuteInParallel
            ? tasks.Where(t => p.TaskIds.Contains(t.TaskId)).Max(t => t.EstimatedDuration)
            : tasks.Where(t => p.TaskIds.Contains(t.TaskId)).Sum(t => t.EstimatedDuration)
    );

    return new ExecutionPlan
    {
        Phases = phases,
        EstimatedDuration = estimatedDuration
    };
}
```

**Workflow Execution 流程**:
```csharp
public async Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
    Guid workflowId,
    TaskAssignmentResult assignment,
    CancellationToken cancellationToken)
{
    var executionContext = new WorkflowExecutionContext(workflowId);

    try
    {
        // 1. 更新 Workflow 狀態: Created → Running
        await _workflowStateMachine.TransitionAsync(workflowId, WorkflowEvent.Start);

        // 2. 按 Phase 執行任務
        foreach (var phase in assignment.ExecutionPlan.Phases)
        {
            if (phase.CanExecuteInParallel)
            {
                // 並行執行
                await ExecutePhaseInParallelAsync(phase, assignment.TaskAgentMapping, executionContext, cancellationToken);
            }
            else
            {
                // 串行執行
                await ExecutePhaseSequentiallyAsync(phase, assignment.TaskAgentMapping, executionContext, cancellationToken);
            }
        }

        // 3. 更新 Workflow 狀態: Running → Completed
        await _workflowStateMachine.TransitionAsync(workflowId, WorkflowEvent.Complete);

        return WorkflowExecutionResult.Success(executionContext);
    }
    catch (Exception ex)
    {
        // 4. 錯誤處理: Running → Failed
        await _workflowStateMachine.TransitionAsync(workflowId, WorkflowEvent.Fail);

        return WorkflowExecutionResult.Failure(ex.Message);
    }
}

private async Task ExecutePhaseInParallelAsync(
    ExecutionPhase phase,
    IReadOnlyDictionary<string, string> taskAgentMapping,
    WorkflowExecutionContext context,
    CancellationToken cancellationToken)
{
    var tasks = phase.TaskIds.Select(taskId =>
        ExecuteTaskAsync(taskId, taskAgentMapping[taskId], context, cancellationToken)
    );

    await Task.WhenAll(tasks);
}

private async Task ExecutePhaseSequentiallyAsync(
    ExecutionPhase phase,
    IReadOnlyDictionary<string, string> taskAgentMapping,
    WorkflowExecutionContext context,
    CancellationToken cancellationToken)
{
    foreach (var taskId in phase.TaskIds)
    {
        await ExecuteTaskAsync(taskId, taskAgentMapping[taskId], context, cancellationToken);
    }
}

private async Task ExecuteTaskAsync(
    string taskId,
    string agentType,
    WorkflowExecutionContext context,
    CancellationToken cancellationToken)
{
    // 1. 獲取 Agent 實例
    var agent = await _agentFactory.CreateAgentAsync(agentType, cancellationToken);

    // 2. 執行 Task
    var result = await agent.ExecuteTaskAsync(taskId, context, cancellationToken);

    // 3. 更新執行上下文
    context.AddTaskResult(taskId, result);

    // 4. 發布 Task 完成事件
    await _eventBus.PublishAsync(new TaskCompletedEvent(taskId, result));
}
```

**Coordination Layer 效能指標**:
- **Task 分配時間**: <2 秒（目標）
- **Phase 執行切換時間**: <500ms（目標）
- **並行執行數**: 最多 2 個（MVP）
- **執行成功率**: ≥90%（目標）

**參考**: [Coordination Layer Design](../../../docs/technical-implementation/coordination-layer-design.md)

### 2.4 Workflow State Machine 設計

**完整設計文檔**: [Workflow State Machine Design](../../../docs/technical-implementation/workflow-state-machine-design.md)

**IWorkflowStateMachine 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IWorkflowStateMachine.cs`
- **核心方法**:
  - `TransitionAsync` - 狀態轉換
  - `GetCurrentStateAsync` - 獲取當前狀態
  - `CanTransitionAsync` - 驗證狀態轉換合法性

**核心方法簽名**:
```csharp
public interface IWorkflowStateMachine
{
    Task<WorkflowState> GetCurrentStateAsync(
        Guid workflowId,
        CancellationToken cancellationToken = default);

    Task<bool> CanTransitionAsync(
        Guid workflowId,
        WorkflowEvent @event,
        CancellationToken cancellationToken = default);

    Task TransitionAsync(
        Guid workflowId,
        WorkflowEvent @event,
        CancellationToken cancellationToken = default);
}

public enum WorkflowState
{
    Created,
    Running,
    Completed,
    Failed
}

public enum WorkflowEvent
{
    Start,
    Complete,
    Fail
}
```

**State Machine 狀態轉換圖 (MVP)**:
```
Created ──(Start)──> Running ──(Complete)──> Completed
                        │
                        └──(Fail)──> Failed
```

**State Transition Rules**:
```csharp
public sealed class WorkflowStateMachine : IWorkflowStateMachine
{
    private readonly Dictionary<(WorkflowState, WorkflowEvent), WorkflowState> _transitions = new()
    {
        { (WorkflowState.Created, WorkflowEvent.Start), WorkflowState.Running },
        { (WorkflowState.Running, WorkflowEvent.Complete), WorkflowState.Completed },
        { (WorkflowState.Running, WorkflowEvent.Fail), WorkflowState.Failed }
    };

    public async Task<bool> CanTransitionAsync(
        Guid workflowId,
        WorkflowEvent @event,
        CancellationToken cancellationToken)
    {
        var currentState = await GetCurrentStateAsync(workflowId, cancellationToken);
        return _transitions.ContainsKey((currentState, @event));
    }

    public async Task TransitionAsync(
        Guid workflowId,
        WorkflowEvent @event,
        CancellationToken cancellationToken)
    {
        var currentState = await GetCurrentStateAsync(workflowId, cancellationToken);

        if (!_transitions.TryGetValue((currentState, @event), out var nextState))
        {
            throw new InvalidOperationException(
                $"Invalid state transition: {currentState} -> {@event}");
        }

        // 1. 更新狀態到 Redis (快取)
        await _stateCache.SetAsync(workflowId, nextState, cancellationToken);

        // 2. 持久化到 PostgreSQL
        await _workflowRepository.UpdateStateAsync(workflowId, nextState, cancellationToken);

        // 3. 發布狀態變更事件
        await _eventBus.PublishAsync(new WorkflowStateChangedEvent(workflowId, currentState, nextState));
    }
}
```

**狀態持久化策略**:
```csharp
// Redis Cache (快速查詢)
public sealed class WorkflowStateCache
{
    private readonly IDistributedCache _cache;

    public async Task SetAsync(Guid workflowId, WorkflowState state, CancellationToken cancellationToken)
    {
        var key = $"workflow:{workflowId}:state";
        var value = JsonSerializer.Serialize(new { State = state, UpdatedAt = DateTime.UtcNow });
        await _cache.SetStringAsync(key, value, new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1)
        }, cancellationToken);
    }

    public async Task<WorkflowState> GetAsync(Guid workflowId, CancellationToken cancellationToken)
    {
        var key = $"workflow:{workflowId}:state";
        var value = await _cache.GetStringAsync(key, cancellationToken);

        if (string.IsNullOrEmpty(value))
        {
            // Cache miss: 從 PostgreSQL 讀取
            return await _workflowRepository.GetStateAsync(workflowId, cancellationToken);
        }

        var data = JsonSerializer.Deserialize<dynamic>(value);
        return Enum.Parse<WorkflowState>(data.State);
    }
}
```

**Error Handling (MVP Simplified)**:
```csharp
// Task 失敗 → Workflow 失敗 (Fail-Fast)
private async Task HandleTaskFailureAsync(
    Guid workflowId,
    string taskId,
    Exception exception,
    CancellationToken cancellationToken)
{
    // 1. 記錄錯誤日誌
    _logger.LogError(exception, "Task {TaskId} failed in Workflow {WorkflowId}", taskId, workflowId);

    // 2. 更新 Workflow 狀態: Running → Failed
    await _workflowStateMachine.TransitionAsync(workflowId, WorkflowEvent.Fail, cancellationToken);

    // 3. 發布 Workflow 失敗事件
    await _eventBus.PublishAsync(new WorkflowFailedEvent(workflowId, taskId, exception.Message));

    // MVP: 無自動重試，無恢復機制
}
```

**State Machine 效能指標**:
- **狀態轉換時間**: <100ms（目標）
- **狀態查詢時間 (Redis)**: <10ms（目標）
- **狀態持久化時間**: <500ms（目標）

**參考**: [Workflow State Machine Design](../../../docs/technical-implementation/workflow-state-machine-design.md)

### 2.5 API 端點設計

**完整設計文檔**: [Workflow API Design](../../../docs/api/workflow-api-design.md)

**API Endpoints (MVP)**:

**1. Create Workflow**:
```http
POST /api/v1/workflows
Content-Type: application/json
Authorization: Bearer {token}

Request Body:
{
  "goal": "分析產品銷售數據並生成報告",
  "requirements": {
    "dataSource": "sales_database",
    "timeRange": "2024-01-01 to 2024-12-31",
    "outputFormat": "PDF"
  },
  "constraints": {
    "maxDuration": "30m",
    "maxCost": 100
  }
}

Response (201 Created):
{
  "workflowId": "uuid",
  "status": "created",
  "generatedTasks": [
    {
      "taskId": "task-1",
      "name": "數據提取",
      "agent": "DataAnalystAgent",
      "dependencies": [],
      "estimatedDuration": "PT5M"
    },
    {
      "taskId": "task-2",
      "name": "數據分析",
      "agent": "DataAnalystAgent",
      "dependencies": ["task-1"],
      "estimatedDuration": "PT10M"
    },
    {
      "taskId": "task-3",
      "name": "報告生成",
      "agent": "ReportGeneratorAgent",
      "dependencies": ["task-2"],
      "estimatedDuration": "PT10M"
    }
  ],
  "estimatedDuration": "PT25M",
  "createdAt": "2026-02-17T10:00:00Z"
}
```

**2. Start Workflow**:
```http
POST /api/v1/workflows/{workflowId}/start
Authorization: Bearer {token}

Response (202 Accepted):
{
  "workflowId": "uuid",
  "status": "running",
  "currentTask": "task-1",
  "startedAt": "2026-02-17T10:01:00Z"
}
```

**3. Get Workflow Status**:
```http
GET /api/v1/workflows/{workflowId}
Authorization: Bearer {token}

Response (200 OK):
{
  "workflowId": "uuid",
  "status": "running",
  "currentTask": {
    "taskId": "task-2",
    "name": "數據分析",
    "status": "in_progress",
    "progress": 60
  },
  "completedTasks": [
    {
      "taskId": "task-1",
      "name": "數據提取",
      "completedAt": "2026-02-17T10:05:00Z"
    }
  ],
  "progress": 40,
  "estimatedTimeRemaining": "PT15M"
}
```

**API 實作 (Controller)**:
```csharp
[ApiController]
[Route("api/v1/workflows")]
public sealed class WorkflowController : ControllerBase
{
    private readonly IMediator _mediator;

    [HttpPost]
    [ProducesResponseType(typeof(CreateWorkflowResponse), StatusCodes.Status201Created)]
    public async Task<IActionResult> CreateWorkflow(
        [FromBody] CreateWorkflowRequest request,
        CancellationToken cancellationToken)
    {
        var command = new CreateWorkflowCommand
        {
            Goal = request.Goal,
            Requirements = request.Requirements,
            Constraints = request.Constraints
        };

        var result = await _mediator.Send(command, cancellationToken);

        return CreatedAtAction(
            nameof(GetWorkflowStatus),
            new { workflowId = result.WorkflowId },
            result);
    }

    [HttpPost("{workflowId}/start")]
    [ProducesResponseType(typeof(StartWorkflowResponse), StatusCodes.Status202Accepted)]
    public async Task<IActionResult> StartWorkflow(
        [FromRoute] Guid workflowId,
        CancellationToken cancellationToken)
    {
        var command = new StartWorkflowCommand { WorkflowId = workflowId };
        var result = await _mediator.Send(command, cancellationToken);

        return Accepted(result);
    }

    [HttpGet("{workflowId}")]
    [ProducesResponseType(typeof(WorkflowStatusResponse), StatusCodes.Status200OK)]
    public async Task<IActionResult> GetWorkflowStatus(
        [FromRoute] Guid workflowId,
        CancellationToken cancellationToken)
    {
        var query = new GetWorkflowStatusQuery { WorkflowId = workflowId };
        var result = await _mediator.Send(query, cancellationToken);

        return Ok(result);
    }
}
```

**CQRS Commands & Queries**:
```csharp
// CreateWorkflowCommand
public sealed class CreateWorkflowCommand : IRequest<CreateWorkflowResponse>
{
    public required string Goal { get; init; }
    public Dictionary<string, object>? Requirements { get; init; }
    public Dictionary<string, object>? Constraints { get; init; }
}

public sealed class CreateWorkflowCommandHandler : IRequestHandler<CreateWorkflowCommand, CreateWorkflowResponse>
{
    private readonly ITaskGeneratorService _taskGenerator;
    private readonly IWorkflowRepository _workflowRepository;

    public async Task<CreateWorkflowResponse> Handle(
        CreateWorkflowCommand request,
        CancellationToken cancellationToken)
    {
        // 1. 生成任務列表
        var workflowGoal = new WorkflowGoal
        {
            Goal = request.Goal,
            Requirements = request.Requirements,
            Constraints = request.Constraints
        };

        var taskGenerationResult = await _taskGenerator.GenerateTasksAsync(workflowGoal, cancellationToken);

        // 2. 創建 Workflow Entity
        var workflow = Workflow.Create(
            request.Goal,
            taskGenerationResult.Tasks,
            taskGenerationResult.EstimatedDuration);

        // 3. 持久化
        await _workflowRepository.AddAsync(workflow, cancellationToken);

        // 4. 返回結果
        return new CreateWorkflowResponse
        {
            WorkflowId = workflow.Id,
            Status = workflow.Status.ToString(),
            GeneratedTasks = taskGenerationResult.Tasks,
            EstimatedDuration = taskGenerationResult.EstimatedDuration,
            CreatedAt = workflow.CreatedAt
        };
    }
}
```

**API 效能指標**:
- **Create Workflow API**: <15 秒（包含 LLM 調用）
- **Start Workflow API**: <2 秒
- **Get Workflow Status API**: <100ms（Redis 快取）

**參考**: [Workflow API Design](../../../docs/api/workflow-api-design.md)

### 2.6 基礎 UI 實作

**完整設計文檔**: [Workflow UI Design](../../../docs/ux-design/workflow-ui-design.md)

**UI Components (MVP)**:

**1. WorkflowCreationForm Component**:
```tsx
// apps/web-app/src/features/workflow/components/WorkflowCreationForm.tsx
export const WorkflowCreationForm: React.FC = () => {
  const [goal, setGoal] = useState('');
  const [isCreating, setIsCreating] = useState(false);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setIsCreating(true);

    try {
      const response = await workflowService.createWorkflow({
        goal,
        requirements: {},
        constraints: {}
      });

      // Navigate to workflow status page
      navigate(`/workflows/${response.workflowId}`);
    } catch (error) {
      // Handle error
    } finally {
      setIsCreating(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <TextField
        label="Workflow 目標"
        value={goal}
        onChange={(e) => setGoal(e.target.value)}
        placeholder="例如: 分析產品銷售數據並生成報告"
        fullWidth
        required
      />
      <Button type="submit" disabled={isCreating}>
        {isCreating ? '創建中...' : '創建 Workflow'}
      </Button>
    </form>
  );
};
```

**2. WorkflowStatus Component**:
```tsx
// apps/web-app/src/features/workflow/components/WorkflowStatus.tsx
export const WorkflowStatus: React.FC<{ workflowId: string }> = ({ workflowId }) => {
  const { data: workflow, isLoading } = useWorkflowStatus(workflowId);

  if (isLoading) return <CircularProgress />;

  return (
    <Box>
      <Typography variant="h5">Workflow 狀態: {workflow.status}</Typography>

      {/* Progress Bar */}
      <LinearProgress variant="determinate" value={workflow.progress} />
      <Typography variant="caption">{workflow.progress}% 完成</Typography>

      {/* Current Task */}
      {workflow.currentTask && (
        <Card>
          <CardContent>
            <Typography variant="h6">當前任務</Typography>
            <Typography>{workflow.currentTask.name}</Typography>
            <LinearProgress variant="determinate" value={workflow.currentTask.progress} />
          </CardContent>
        </Card>
      )}

      {/* Task List */}
      <Typography variant="h6">任務列表</Typography>
      <List>
        {workflow.generatedTasks.map((task) => (
          <ListItem key={task.taskId}>
            <ListItemIcon>
              {task.status === 'completed' ? <CheckCircle /> : <Circle />}
            </ListItemIcon>
            <ListItemText
              primary={task.name}
              secondary={`Agent: ${task.agent}, 預估: ${task.estimatedDuration}`}
            />
          </ListItem>
        ))}
      </List>
    </Box>
  );
};
```

**3. workflowService (API Client)**:
```typescript
// apps/web-app/src/features/workflow/services/workflowService.ts
export const workflowService = {
  createWorkflow: async (request: CreateWorkflowRequest): Promise<CreateWorkflowResponse> => {
    const response = await axios.post('/api/v1/workflows', request);
    return response.data;
  },

  startWorkflow: async (workflowId: string): Promise<StartWorkflowResponse> => {
    const response = await axios.post(`/api/v1/workflows/${workflowId}/start`);
    return response.data;
  },

  getWorkflowStatus: async (workflowId: string): Promise<WorkflowStatusResponse> => {
    const response = await axios.get(`/api/v1/workflows/${workflowId}`);
    return response.data;
  }
};

// React Query Hook
export const useWorkflowStatus = (workflowId: string) => {
  return useQuery(
    ['workflow', workflowId],
    () => workflowService.getWorkflowStatus(workflowId),
    {
      refetchInterval: 5000, // 每 5 秒更新一次
      enabled: !!workflowId
    }
  );
};
```

**UI 效能指標**:
- **Workflow 創建表單載入**: <1 秒
- **Workflow 狀態更新頻率**: 每 5 秒
- **UI 響應時間**: <200ms

**參考**: [Workflow UI Design](../../../docs/ux-design/workflow-ui-design.md)

---

## 三、完整文檔引用清單 (按功能分類)

### 核心規劃文檔 (8 refs)
1. [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md) - MVP 範圍定義與 Phase 1B 詳細規劃
2. [SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 7 詳細分析 (13 SP, 3 週)
3. [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) - Workflow Engine 開發策略
4. [DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md) - Sprint 依賴關係矩陣
5. [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md) - 風險登記簿
6. [TECHNICAL-DECISIONS-LOG.md](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 技術決策日誌
7. [ARCHITECTURE-EVOLUTION-ROADMAP.md](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進路線圖
8. [DOCS-REBUILD-EXECUTION-PLAN.md](../../1-planning/DOCS-REBUILD-EXECUTION-PLAN.md) - 文檔重建執行計劃

### Sprint 7 文檔 (7 refs)
9. [SPRINT-7-1-OVERVIEW.md](./SPRINT-7-1-OVERVIEW.md) - Sprint 7 概覽
10. [SPRINT-7-2-PLAN.md](./SPRINT-7-2-PLAN.md) - Sprint 7 實施計劃
11. [SPRINT-7-3-CONTEXT.md](./SPRINT-7-3-CONTEXT.md) - Sprint 7 上下文與背景（本文件）
12. [SPRINT-7-4-CHECKLIST.md](./SPRINT-7-4-CHECKLIST.md) - Sprint 7 檢查清單
13. [SPRINT-7-5-DEV-LOG.md](./SPRINT-7-5-DEV-LOG.md) - Sprint 7 開發日誌
14. [SPRINT-7-6-ISSUES.md](./SPRINT-7-6-ISSUES.md) - Sprint 7 問題追蹤
15. [SPRINT-7-7-RETROSPECTIVE.md](./SPRINT-7-7-RETROSPECTIVE.md) - Sprint 7 回顧

### 前序 Sprint 參考 (42 refs - Sprint 1-6 各 7 files)
**Sprint 1-6**: 各 7 個文件 (OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE)

### User Stories - Workflow Engine (2 refs)
16. [us-7-workflow-engine.md](../../docs/user-stories/us-7-workflow-engine.md) - Workflow Engine 完整需求 (US 6.1)
17. [epic-07-workflow-engine.md](../../docs/user-stories/epics/epic-07-workflow-engine.md) - Workflow Engine Epic

### 架構設計文檔 - ADRs (6 refs)
18. [Architecture-Design-Document.md](../../docs/architecture/Architecture-Design-Document.md) - 完整架構設計
19. [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md) - Workflow 編排策略
20. [ADR-007: Multi-Agent Communication](../../docs/architecture/adr/ADR-007-multi-agent-communication.md) - Multi-Agent 通訊
21. [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md) - Redis + PostgreSQL 混合狀態管理
22. [ADR-001: Clean Architecture](../../docs/architecture/adr/ADR-001-clean-architecture.md) - Clean Architecture 分層架構
23. [ADR-002: CQRS Pattern](../../docs/architecture/adr/ADR-002-cqrs-pattern.md) - CQRS 模式實作

### 技術實作文檔 - Workflow Engine (6 refs)
24. [workflow-engine-implementation.md](../../docs/technical-implementation/workflow-engine-implementation.md) - Workflow Engine 完整實作指南
25. [task-generator-design.md](../../docs/technical-implementation/task-generator-design.md) - Task Generator 設計 (New)
26. [coordination-layer-design.md](../../docs/technical-implementation/coordination-layer-design.md) - Coordination Layer 設計 (New)
27. [workflow-state-machine-design.md](../../docs/technical-implementation/workflow-state-machine-design.md) - Workflow State Machine 設計 (New)
28. [workflow-engine-design.md](../../docs/technical-implementation/workflow-engine-design.md) - Workflow Engine 設計 (New)
29. [multi-agent-coordination.md](../../docs/technical-implementation/multi-agent-coordination.md) - Multi-Agent 協作設計 (New)

### API 設計文檔 (3 refs)
30. [API-Design-Guidelines.md](../../docs/api/API-Design-Guidelines.md) - API 設計指南
31. [workflow-api-design.md](../../docs/api/workflow-api-design.md) - Workflow API 設計 (New)
32. [api-endpoints-v1.md](../../docs/api/api-endpoints-v1.md) - API v1 端點清單

### 資料庫文檔 (2 refs)
33. [database-schema-design.md](../../docs/database/database-schema-design.md) - 資料庫 Schema 設計
34. [workflow-entity-design.md](../../docs/database/workflow-entity-design.md) - Workflow Entity 設計 (New)

### 測試文檔 (4 refs)
35. [testing-strategy-overview.md](../../docs/testing/testing-strategy-overview.md) - 測試策略概覽
36. [unit-testing-guidelines.md](../../docs/testing/unit-testing-guidelines.md) - 單元測試指南
37. [integration-testing-guidelines.md](../../docs/testing/integration-testing-guidelines.md) - 整合測試指南
38. [workflow-testing-strategy.md](../../docs/testing/workflow-testing-strategy.md) - Workflow Engine 測試策略 (New)

### UX 設計文檔 (2 refs)
39. [user-research/personas.md](../../docs/ux-design/user-research/personas.md) - 使用者研究 Personas
40. [workflow-ui-design.md](../../docs/ux-design/workflow-ui-design.md) - Workflow UI 設計 (New)

### 開發標準與流程 (3 refs)
41. [coding-standards-csharp.md](../../docs/development-standards/coding-standards-csharp.md) - C# 編碼標準
42. [coding-standards-typescript.md](../../docs/development-standards/coding-standards-typescript.md) - TypeScript 編碼標準
43. [git-workflow.md](../../docs/development-standards/git-workflow.md) - Git 工作流程

### 程式碼參考 - Application Layer (4 refs)
44. [ITaskGeneratorService.cs](../../src/AIAgentPlatform.Application/Interfaces/ITaskGeneratorService.cs) - Task Generator 服務介面 (New)
45. [ICoordinationService.cs](../../src/AIAgentPlatform.Application/Interfaces/ICoordinationService.cs) - Coordination 服務介面 (New)
46. [IWorkflowStateMachine.cs](../../src/AIAgentPlatform.Application/Interfaces/IWorkflowStateMachine.cs) - Workflow State Machine 介面 (New)
47. [IWorkflowRepository.cs](../../src/AIAgentPlatform.Application/Interfaces/IWorkflowRepository.cs) - Workflow Repository 介面 (New)

### 程式碼參考 - Domain Layer (3 refs)
48. [Workflow.cs](../../src/AIAgentPlatform.Domain/Entities/Workflow.cs) - Workflow Entity (New)
49. [WorkflowTask.cs](../../src/AIAgentPlatform.Domain/Entities/WorkflowTask.cs) - WorkflowTask Entity (New)
50. [WorkflowState.cs](../../src/AIAgentPlatform.Domain/ValueObjects/WorkflowState.cs) - WorkflowState Value Object (New)

### 程式碼參考 - Infrastructure Layer (5 refs)
51. [TaskGeneratorService.cs](../../src/AIAgentPlatform.Infrastructure/Services/TaskGeneratorService.cs) - Task Generator 服務 (New)
52. [CoordinationService.cs](../../src/AIAgentPlatform.Infrastructure/Services/CoordinationService.cs) - Coordination 服務 (New)
53. [WorkflowStateMachine.cs](../../src/AIAgentPlatform.Infrastructure/Services/WorkflowStateMachine.cs) - Workflow State Machine (New)
54. [WorkflowRepository.cs](../../src/AIAgentPlatform.Infrastructure/Repositories/WorkflowRepository.cs) - Workflow Repository (New)
55. [WorkflowStateCache.cs](../../src/AIAgentPlatform.Infrastructure/Services/WorkflowStateCache.cs) - Workflow State Cache (New)

### 程式碼參考 - API Layer (1 ref)
56. [WorkflowController.cs](../../src/AIAgentPlatform.API/Controllers/WorkflowController.cs) - Workflow API Controller (New)

### 程式碼參考 - Frontend (3 refs)
57. [WorkflowCreationForm.tsx](../../apps/web-app/src/features/workflow/components/WorkflowCreationForm.tsx) - Workflow 創建表單 (New)
58. [WorkflowStatus.tsx](../../apps/web-app/src/features/workflow/components/WorkflowStatus.tsx) - Workflow 狀態元件 (New)
59. [workflowService.ts](../../apps/web-app/src/features/workflow/services/workflowService.ts) - Workflow 服務 (New)

---

## 📖 使用指南

### 如何使用此文件

**開發階段使用**:
1. **開發 US 6.1 Task Generator**: 閱讀「第二章 2.2」了解 LLM-based Task Decomposition、依賴分析、執行計劃生成
2. **開發 US 6.1 Coordination Layer**: 閱讀「第二章 2.3」了解 Agent 選擇、Task 分配、並行執行控制
3. **開發 US 6.1 State Machine**: 閱讀「第二章 2.4」了解 Workflow 狀態管理、狀態轉換、錯誤處理
4. **開發 US 6.1 API**: 閱讀「第二章 2.5」了解 API 端點設計、CQRS 實作
5. **開發 US 6.1 UI**: 閱讀「第二章 2.6」了解基礎 UI 實作
6. **查詢架構決策**: 查閱「第一章 1.2-1.3」的 ADR 文檔
7. **查詢 API 規格**: 查閱「第一章 1.5」的 API 設計文檔

**疑難排解使用**:
- **Task Generator 問題**: 查閱「2.2 Task Generator 架構設計」
- **Coordination 問題**: 查閱「2.3 Coordination Layer 設計」
- **State Machine 問題**: 查閱「2.4 Workflow State Machine 設計」
- **API 問題**: 查閱「2.5 API 端點設計」
- **UI 問題**: 查閱「2.6 基礎 UI 實作」

**Code Review 使用**:
- 驗證實作是否符合「關鍵技術決策」
- 檢查介面設計是否與文檔一致
- 確認 MVP 範圍邊界
- 確認效能目標是否達成

---

## 📝 更新日誌

**v2.1** (2025-11-14)
- ✅ 初始版本建立
- ✅ 完整 US 6.1 (Workflow 編排引擎基礎 Part 1) 技術上下文
- ✅ 完整 Task Generator 架構設計（2.2 章節）
- ✅ 完整 Coordination Layer 設計（2.3 章節）
- ✅ 完整 Workflow State Machine 設計（2.4 章節）
- ✅ 完整 API 端點設計（2.5 章節）
- ✅ 完整基礎 UI 實作（2.6 章節）
- ✅ 6 個 ADR 文檔引用與說明
- ✅ 6 個技術實作指南引用
- ✅ 完整 API、資料庫、測試文檔引用（59+ 文檔）
- ✅ MVP 範圍定義與 Phase 2 延後項目明確說明
- ✅ 6 個關鍵技術決策詳細分析

**文件統計** (v2.1):
- 總行數: ~1,400 行
- User Stories: 1 個 (US 6.1 Part 1)
- Story Points: 13 SP
- Phases: 4 個
- 技術決策: 6 個關鍵決策
- 參考文獻: 59+ 文檔

**品質指標**:
- 與 Sprint 6-3-CONTEXT 格式一致性: ✅ 100%
- v2.1 標準符合度: ✅ 100%
- 參考文獻完整性: ✅ 59+ 文檔
- 技術細節深度: ✅ 6 大章節，詳細技術實作
- MVP 範圍清晰度: ✅ 明確定義 Phase 1B Part 1 vs Part 2

**特別說明**:
- Sprint 7 為 Phase 1B 第一個 Sprint
- US 6.1 為 Part 1（基礎功能），Part 2 留待 Sprint 8-9
- MVP 簡化版本（13 SP），完整版留待 Phase 2
- 特別強調 LLM-based Task Generation 和 Multi-Agent Coordination

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 7 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-7/SPRINT-7-3-CONTEXT.md`

---

**相關文檔**:
- ⬆️ 上一層: [Sprint 7 Overview](./SPRINT-7-1-OVERVIEW.md)
- ⬅️ 上一步: [Sprint 7 Plan](./SPRINT-7-2-PLAN.md)
- ➡️ 下一步: [Sprint 7 Checklist](./SPRINT-7-4-CHECKLIST.md)
