# SPRINT-9-3-CONTEXT.md - Sprint 9 開發上下文：Framework Abstraction Layer 詳細技術上下文

**版本**: v2.1
**Sprint 編號**: Sprint 9
**Sprint 週期**: Week 25-27 (3 週)
**Phase**: Phase 1B - 工作流引擎核心 (Workflow Engine Core)
**計劃日期**: 2026-03-31 ~ 2026-04-20
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
  - [1.4 Sprint 7-8 延續性](#14-sprint-7-8-延續性)
- [二、Framework Abstraction Layer - 詳細技術上下文](#二framework-abstraction-layer---詳細技術上下文)
  - [2.1 核心接口設計詳解](#21-核心接口設計詳解)
  - [2.2 SemanticKernelAdapter 實作細節](#22-semantickerneladapter-實作細節)
  - [2.3 可替換性驗證機制](#23-可替換性驗證機制)
  - [2.4 Phase 1B 完成與移交](#24-phase-1b-完成與移交)
- [三、完整文檔引用清單 (按功能分類)](#三完整文檔引用清單-按功能分類)
- [使用指南](#-使用指南)
- [更新日誌](#-更新日誌)

---

## 📖 使用說明 (How to Use This Document)

**本文檔定位**: "Just-in-Time" 技術參考手冊

**適用場景**:
- ✅ 開發 Framework Abstraction Layer 時,需要查詢接口設計原則 → 第二章 2.1
- ✅ 開發 SemanticKernelAdapter 時,需要查詢 Adapter Pattern 實作細節 → 第二章 2.2
- ✅ 需要快速找到可替換性驗證機制 → 2.3 章節
- ✅ 需要理解 Phase 1B 完成標準 → 2.4 章節
- ✅ 需要快速找到架構設計文檔 → 第一章 Reference Layer
- ✅ 需要理解 Sprint 7-8 → Sprint 9 延續性 → 1.4 章節

**不適用場景**:
- ❌ 尋找 Sprint 整體進度與成果 → 使用 [SPRINT-9-1-OVERVIEW.md](./SPRINT-9-1-OVERVIEW.md)
- ❌ 尋找詳細開發計劃與任務清單 → 使用 [SPRINT-9-2-PLAN.md](./SPRINT-9-2-PLAN.md)
- ❌ 追蹤開發進度與 Checklist → 使用 [SPRINT-9-4-CHECKLIST.md](./SPRINT-9-4-CHECKLIST.md)

---

## 一、核心技術參考層 (Reference Layer)

### 1.1 Module & Epic 引用

**Framework Abstraction Layer 完整規格**:
- 📖 [ADR-011: Framework Abstraction Layer](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md) - Framework Abstraction Layer 完整架構決策
  - **Section**: [Core Interfaces Design](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md#core-interfaces-design) - 5 個核心接口設計
  - **Section**: [Adapter Pattern Implementation](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md#adapter-pattern-implementation) - Adapter Pattern 實作細節
  - **關鍵內容**: IWorkflowEngine, ITaskGenerator, ICoordinationLayer, IExecutionEngine, IStateManager

**Epic 文檔**:
- 📖 [Epic 07: Workflow Engine](../../../docs/user-stories/epics/epic-07-workflow-engine.md) - Workflow Engine 功能完整策略
  - **用途**: 理解 Framework Abstraction 在整體系統中的定位
  - **Sprint 9 範圍**: Framework Abstraction Layer 完整實作
  - **Phase 2 演進**: 支援其他框架 (Autogen, LangChain)

### 1.2 架構設計文檔

**核心架構決策記錄 (ADR)**:

**1. Framework Abstraction Layer** - [ADR-011](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md)
- **決策**: 建立 Framework Abstraction Layer,支援未來框架遷移
- **關鍵原則**: Interface Segregation, Adapter Pattern, Framework-agnostic
- **Section**: [Core Interfaces Design](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md#core-interfaces-design) - 5 個核心接口設計
- **Section**: [SemanticKernelAdapter](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md#semantickernel-adapter) - Semantic Kernel 適配器設計
- **Section**: [Replaceability Verification](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md#replaceability-verification) - 可替換性驗證機制
- **實施影響**: Sprint 9 完整實作,Phase 2 支援其他框架

**2. Workflow Orchestration Strategy** - [ADR-009](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- **決策**: Task-based Orchestration + Event-driven Coordination
- **關鍵設計**: Framework Abstraction Layer 基於此決策
- **Section**: [Execution Engine Design](../../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md#execution-engine-design) - Execution Engine 架構
- **實施影響**: IWorkflowEngine, IExecutionEngine 接口設計

**完整架構設計文檔**:
- 🏗️ [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 系統架構總覽
  - **Section**: [Framework Abstraction Layer Architecture](../../../docs/architecture/Architecture-Design-Document.md#framework-abstraction-layer-architecture) - Framework Abstraction 系統架構圖

### 1.3 技術決策記錄 (ADR)

**Sprint 9 相關 ADR**:

**ADR-011: Framework Abstraction Layer**
- **決策日期**: 2025-10-28
- **決策內容**: 建立 5 個核心接口,實作 SemanticKernelAdapter,支援框架可替換性
- **關鍵影響**:
  - 5 個核心接口: IWorkflowEngine, ITaskGenerator, ICoordinationLayer, IExecutionEngine, IStateManager
  - SemanticKernelAdapter 完整封裝 Semantic Kernel
  - Mock implementations 用於測試與驗證
  - Feature Flag 機制支援動態切換
- **參考**: [ADR-011](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md)

### 1.4 Sprint 7-8 延續性

**Sprint 7-8 已完成組件**:
```yaml
Sprint 7 成果: ✅ 完成
  Task Generator (5-8 SP):
    - ITaskGenerator 介面 (基礎版本)
    - LLMTaskGenerator 實現
    - Prompt Templates 設計
    - 準確率 ≥70% 驗證通過

  Sequential Coordination (2-3 SP):
    - ICoordinationStrategy 介面
    - SequentialCoordination 實現

  Parallel Coordination (基礎版) (2-3 SP):
    - ParallelCoordination 基礎實現

  狀態: 穩定,可供 Sprint 9 抽象化

Sprint 8 成果: ✅ 完成
  Parallel Coordination 完成 (3-5 SP):
    - Race condition 處理
    - Result aggregation
    - 性能優化

  Workflow Execution Engine (5 SP):
    - IWorkflowExecutor 介面 (基礎版本)
    - WorkflowExecutor 核心邏輯
    - ExecutionContext 管理

  Agent Messaging 系統 (5 SP):
    - IMessageBus 介面
    - InMemoryMessageBus 實現

  State Management 強化 (1 SP):
    - IStateStore 介面 (基礎版本)
    - RedisStateStore 實現

  狀態: 穩定,可供 Sprint 9 抽象化
```

**Sprint 9 抽象化策略**:
```yaml
Interface 抽象化:
  現有接口 (Sprint 7-8):
    - ITaskGenerator (基礎版本)
    - IWorkflowExecutor (基礎版本)
    - IMessageBus
    - IStateStore (基礎版本)

  Sprint 9 增強:
    - 重新設計為 Framework-agnostic
    - 添加完整 XML 註釋
    - 設計 Adapter Pattern
    - 建立 Mock implementations

Adapter 實作:
  SemanticKernelAdapter:
    - 封裝 Sprint 7-8 的具體實現
    - 實作 5 個核心接口
    - 隔離 Semantic Kernel 依賴

  MockAdapter:
    - 用於測試與驗證
    - 不依賴 Semantic Kernel
    - 驗證接口可替換性
```

---

## 二、Framework Abstraction Layer - 詳細技術上下文

### 2.1 核心接口設計詳解

**為什麼需要 5 個核心接口?**
- **IWorkflowEngine**: 工作流引擎入口 (Facade Pattern)
- **ITaskGenerator**: 任務生成器 (LLM 驅動)
- **ICoordinationLayer**: Multi-Agent 協調層
- **IExecutionEngine**: 工作流執行引擎
- **IStateManager**: 狀態管理器 (Redis + PostgreSQL)

**設計原則 (SOLID)**:
```yaml
Interface Segregation Principle (ISP):
  - 每個接口職責單一,專注於特定功能
  - 避免單一巨大接口 (God Interface)
  - Client 只依賴需要的接口

Dependency Inversion Principle (DIP):
  - 高層模組依賴接口,不依賴具體實現
  - 低層模組實作接口
  - 接口由高層模組定義 (Application Layer)

Single Responsibility Principle (SRP):
  - 每個接口有且僅有一個變更的理由
  - IWorkflowEngine: 工作流執行
  - ITaskGenerator: 任務規劃
  - ICoordinationLayer: Agent 協調
  - IExecutionEngine: 節點執行
  - IStateManager: 狀態管理
```

#### IWorkflowEngine 詳解

**設計決策**:
```yaml
職責:
  - 工作流引擎入口 (Facade Pattern)
  - 整合 TaskGenerator + Coordination + Execution + State
  - 提供高層抽象,隱藏內部複雜度

設計原則:
  - Framework-agnostic (不暴露框架細節)
  - 完整的工作流生命週期管理
  - 支援 Pause/Resume/Cancel

參考: ADR-011 Framework Abstraction Layer
```

**核心方法設計**:
```csharp
/// <summary>
/// 執行工作流
/// </summary>
/// <param name="workflowId">工作流 ID</param>
/// <param name="userInput">使用者輸入</param>
/// <param name="parameters">執行參數</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>執行結果</returns>
Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
    Guid workflowId,
    string userInput,
    Dictionary<string, object>? parameters = null,
    CancellationToken cancellationToken = default);
```

**關鍵設計考量**:
```yaml
異步設計:
  - 所有方法返回 Task 或 Task<T>
  - 支援 CancellationToken
  - 使用 async/await pattern

錯誤處理:
  - 不拋出異常,返回 Result<T> (Result Pattern)
  - ErrorMessage 屬性記錄錯誤
  - Success 屬性指示成功/失敗

生命週期管理:
  - ExecuteWorkflowAsync: 啟動執行
  - PauseExecutionAsync: 暫停執行
  - ResumeExecutionAsync: 恢復執行
  - CancelExecutionAsync: 取消執行
  - GetExecutionStatusAsync: 查詢狀態
```

#### ITaskGenerator 詳解

**設計決策**:
```yaml
職責:
  - LLM 驅動任務規劃
  - 任務分解與依賴分析
  - Framework-agnostic (不依賴 Semantic Kernel)

設計原則:
  - 抽象 LLM 能力 (不暴露 Semantic Kernel Plugins)
  - 支援多種 LLM (OpenAI, Azure OpenAI, Anthropic)
  - 任務依賴關係建模 (DAG)

參考: ADR-011 Framework Abstraction Layer
```

**核心方法設計**:
```csharp
/// <summary>
/// 生成任務計劃
/// </summary>
/// <param name="goal">工作流目標</param>
/// <param name="workflow">工作流定義</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>任務計劃</returns>
Task<TaskPlan> GenerateTaskPlanAsync(
    string goal,
    WorkflowDefinition workflow,
    CancellationToken cancellationToken = default);

/// <summary>
/// 分析任務依賴關係
/// </summary>
/// <param name="tasks">任務列表</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>依賴圖</returns>
Task<TaskDependencyGraph> AnalyzeDependenciesAsync(
    IEnumerable<WorkflowTask> tasks,
    CancellationToken cancellationToken = default);
```

**關鍵設計考量**:
```yaml
任務依賴建模:
  - TaskDependencyGraph: DAG 表示
  - GetExecutionOrder(): 拓撲排序
  - ValidateAcyclicDependencies(): 循環檢測

LLM 抽象化:
  - 不暴露 Semantic Kernel KernelFunction
  - 不暴露 Prompt Templates
  - 僅返回 TaskPlan (Framework-agnostic DTO)

驗證機制:
  - ValidatePlanAsync(): 驗證任務計劃合法性
  - 檢查循環依賴
  - 檢查任務數量 (<100)
```

#### ICoordinationLayer 詳解

**設計決策**:
```yaml
職責:
  - Multi-Agent 協調
  - Sequential/Parallel 執行策略
  - Framework-agnostic (不依賴 Semantic Kernel Agents)

設計原則:
  - Strategy Pattern (協調策略)
  - 支援多種協調模式 (Sequential, Parallel, Pipeline)

參考: ADR-009 Workflow Orchestration Strategy
```

**核心方法設計**:
```csharp
/// <summary>
/// 協調執行任務
/// </summary>
/// <param name="tasks">任務列表</param>
/// <param name="strategy">協調策略</param>
/// <param name="context">執行上下文</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>協調結果</returns>
Task<CoordinationResult> CoordinateAsync(
    IEnumerable<WorkflowTask> tasks,
    CoordinationStrategy strategy,
    ExecutionContext context,
    CancellationToken cancellationToken = default);
```

**協調策略設計**:
```yaml
Sequential (順序執行):
  - 任務按順序執行
  - 前一個任務完成後才開始下一個
  - 任一失敗則停止執行

Parallel (並行執行):
  - 任務並行執行
  - 使用 SemaphoreSlim 控制並發度
  - All-or-Nothing 策略

Pipeline (管道執行):
  - 任務順序執行
  - 上一個任務的輸出作為下一個任務的輸入
  - 數據流傳遞
```

#### IExecutionEngine 詳解

**設計決策**:
```yaml
職責:
  - 工作流節點執行
  - ExecutionContext 管理
  - Checkpoint/Resume 支援

設計原則:
  - Command Pattern
  - State Machine Pattern

參考: ADR-009 Workflow Orchestration Strategy
```

**核心方法設計**:
```csharp
/// <summary>
/// 執行工作流節點
/// </summary>
/// <param name="node">工作流節點</param>
/// <param name="tasks">節點任務</param>
/// <param name="context">執行上下文</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>節點執行結果</returns>
Task<NodeExecutionResult> ExecuteNodeAsync(
    WorkflowNode node,
    IEnumerable<WorkflowTask> tasks,
    ExecutionContext context,
    CancellationToken cancellationToken = default);

/// <summary>
/// 創建執行上下文
/// </summary>
/// <param name="workflowId">工作流 ID</param>
/// <param name="userInput">使用者輸入</param>
/// <param name="parameters">執行參數</param>
/// <returns>執行上下文</returns>
ExecutionContext CreateExecutionContext(
    Guid workflowId,
    string userInput,
    Dictionary<string, object>? parameters = null);
```

**執行流程設計**:
```yaml
ExecuteNodeAsync 流程:
  1. 驗證執行條件 (CanExecuteNodeAsync)
  2. 選擇協調策略 (Sequential/Parallel)
  3. 執行節點任務 (ICoordinationLayer.CoordinateAsync)
  4. 更新執行上下文 (UpdateExecutionContext)
  5. 保存 Checkpoint (Phase 1B 簡化版本)

ExecutionContext 管理:
  - CreateExecutionContext: 創建新上下文
  - UpdateExecutionContext: 更新上下文 (節點完成後)
  - Variables: 全局變數 (Workflow-level)
```

#### IStateManager 詳解

**設計決策**:
```yaml
職責:
  - 執行狀態管理 (Redis + PostgreSQL)
  - Checkpoint/Resume 支援
  - 執行歷史查詢

設計原則:
  - Repository Pattern
  - 混合存儲策略 (Cache + Persistence)

參考: ADR-006 Hybrid State Management
```

**核心方法設計**:
```csharp
/// <summary>
/// 保存執行狀態
/// </summary>
/// <param name="context">執行上下文</param>
/// <param name="cancellationToken">取消令牌</param>
Task SaveStateAsync(
    ExecutionContext context,
    CancellationToken cancellationToken = default);

/// <summary>
/// 載入執行狀態
/// </summary>
/// <param name="executionId">執行 ID</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>執行上下文</returns>
Task<ExecutionContext?> LoadStateAsync(
    Guid executionId,
    CancellationToken cancellationToken = default);

/// <summary>
/// 保存 Checkpoint
/// </summary>
/// <param name="context">執行上下文</param>
/// <param name="checkpoint">Checkpoint 名稱</param>
/// <param name="cancellationToken">取消令牌</param>
Task SaveCheckpointAsync(
    ExecutionContext context,
    string checkpoint,
    CancellationToken cancellationToken = default);
```

**混合存儲策略**:
```yaml
SaveStateAsync:
  1. 寫入 Redis (快取,TTL 24 小時)
  2. 寫入 PostgreSQL (持久化)
  3. 同步寫入,確保一致性

LoadStateAsync:
  1. 嘗試從 Redis 讀取 (快速路徑)
  2. Redis 沒有,從 PostgreSQL 讀取 (慢速路徑)
  3. 重新放入 Redis 快取

SaveCheckpointAsync:
  1. 同時寫入 Redis 和 PostgreSQL
  2. Checkpoint 名稱: node-{nodeId}
  3. 支援 Resume 恢復執行
```

---

### 2.2 SemanticKernelAdapter 實作細節

**為什麼需要 SemanticKernelAdapter?**
- 將 Semantic Kernel SDK 封裝到 Framework Abstraction Layer 接口
- 隔離框架特定實作 (Plugins, Functions, Agents)
- 為未來框架遷移做準備

**Adapter Pattern 設計原則**:
```yaml
完全封裝 Semantic Kernel:
  - 業務邏輯不直接依賴 Semantic Kernel
  - 所有調用通過 Adapter Interface
  - 參考: src/AIAgentPlatform.Core/Abstractions/*.cs

可替換性驗證:
  - 創建 MockAdapter 用於測試
  - 使用 Feature Flag 切換 Adapter
  - A/B Testing 機制
  - 參考: src/AIAgentPlatform.Infrastructure/Adapters/MockWorkflowEngine.cs

性能損耗 <5%:
  - Adapter 薄封裝,避免過度抽象
  - 使用 ValueTask 減少分配
  - 緩存常用對象
  - 參考: docs/technical-implementation/08-performance-optimization/01-performance-requirements.md

可觀察性:
  - 所有 Adapter 方法添加 Telemetry
  - 使用 OpenTelemetry 標準
  - 記錄 Adapter 切換事件
  - 參考: docs/technical-implementation/07-monitoring-logging/01-telemetry-strategy.md
```

#### SemanticKernelWorkflowEngine 詳解

**技術決策 TD-070**:
```yaml
決策: SemanticKernelWorkflowEngine 作為 Facade
背景:
  - 需要整合多個 Adapter (TaskGenerator, Coordination, Execution, State)
  - 提供統一的工作流執行入口
  - 隱藏內部複雜度

選項:
  1. Monolithic Engine (單一類別)
     優點: 簡單直接
     缺點: 職責過多,難以維護
  2. Facade Pattern (整合多個 Adapter)
     優點: 職責清晰,易於測試
     缺點: 複雜度略高

決策: 選擇選項 2 (Facade Pattern)
  - WorkflowEngine 作為 Facade
  - 依賴注入其他 4 個 Adapter
  - 協調執行流程

實施:
  - 構造器注入: ITaskGenerator, ICoordinationLayer, IExecutionEngine, IStateManager
  - ExecuteWorkflowAsync: 協調執行流程
  - 錯誤處理與日誌記錄

參考:
  - src/AIAgentPlatform.Infrastructure/Adapters/SemanticKernel/SemanticKernelWorkflowEngine.cs
  - SPRINT-9-2-PLAN.md Phase 2
```

**關鍵實作細節**:
```csharp
public class SemanticKernelWorkflowEngine : IWorkflowEngine
{
    private readonly ITaskGenerator _taskGenerator;
    private readonly ICoordinationLayer _coordinationLayer;
    private readonly IExecutionEngine _executionEngine;
    private readonly IStateManager _stateManager;
    private readonly IWorkflowRepository _workflowRepository;
    private readonly ILogger<SemanticKernelWorkflowEngine> _logger;

    public async Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
        Guid workflowId,
        string userInput,
        Dictionary<string, object>? parameters = null,
        CancellationToken cancellationToken = default)
    {
        // 1. 載入工作流定義 (IWorkflowRepository)
        // 2. 創建執行上下文 (IExecutionEngine.CreateExecutionContext)
        // 3. 生成任務計劃 (ITaskGenerator.GenerateTaskPlanAsync)
        // 4. 執行工作流節點 (循環處理)
        //    - 驗證執行條件
        //    - 獲取節點任務
        //    - 執行節點 (IExecutionEngine.ExecuteNodeAsync)
        //    - 更新上下文
        //    - 保存 Checkpoint (IStateManager.SaveCheckpointAsync)
        // 5. 錯誤處理
        // 6. 返回結果
    }
}
```

#### SemanticKernelTaskGenerator 詳解

**技術決策 TD-071**:
```yaml
決策: 使用 Semantic Kernel KernelFunction 生成任務
背景:
  - 需要 LLM 驅動任務規劃
  - Prompt Engineering 複雜
  - JSON 解析容錯性

選項:
  1. 直接調用 OpenAI API
     優點: 簡單直接
     缺點: 缺乏 Semantic Kernel 抽象
  2. 使用 Semantic Kernel KernelFunction
     優點: 完整的 Prompt 管理,Plugin 支援
     缺點: 依賴 Semantic Kernel

決策: 選擇選項 2 (Semantic Kernel KernelFunction)
  - 使用 KernelFunction 封裝 Prompt
  - Prompt Template 管理
  - JSON 解析與容錯

實施:
  - BuildTaskDecompositionPrompt(): 構建 LLM Prompt
  - ParseTaskListFromLLM(): 解析 LLM 返回的 JSON
  - ValidateAcyclicDependencies(): 檢測循環依賴

參考:
  - src/AIAgentPlatform.Infrastructure/Adapters/SemanticKernel/SemanticKernelTaskGenerator.cs
  - SPRINT-9-2-PLAN.md Phase 2
```

**Prompt Engineering 策略**:
```csharp
private string BuildTaskDecompositionPrompt(
    string goal, WorkflowDefinition workflow)
{
    return $@"
你是一個專業的任務規劃專家。請將以下工作流目標分解為具體的執行任務。

工作流目標: {goal}

工作流定義:
- 節點數: {workflow.Nodes.Count}
- 節點列表: {string.Join(", ", workflow.Nodes.Select(n => n.Name))}

請返回 JSON 格式的任務列表,每個任務包含:
- id: 唯一標識符 (GUID)
- nodeId: 所屬節點 ID
- name: 任務名稱
- description: 任務描述
- agentType: 執行此任務的 Agent 類型
- dependencies: 依賴的任務 ID 列表 (如果沒有依賴則為空數組)
- estimatedDuration: 預估執行時間 (ISO 8601 duration format)

請確保:
1. 任務之間的依賴關係正確
2. 沒有循環依賴
3. 任務粒度適中 (每個任務 5-30 分鐘)
4. Agent 類型選擇合理
";
}
```

#### SemanticKernelCoordination 詳解

**技術決策 TD-072**:
```yaml
決策: 整合 Sprint 7-8 的 Coordination 實作
背景:
  - Sprint 7-8 已完成 Sequential/Parallel Coordination
  - 需要封裝到 ICoordinationLayer 接口

選項:
  1. 重寫 Coordination 邏輯
     優點: 完全 Framework-agnostic
     缺點: 工作量大,風險高
  2. 整合現有實作
     優點: 快速,穩定
     缺點: 依賴 Semantic Kernel

決策: 選擇選項 2 (整合現有實作)
  - 封裝 Sprint 7-8 的 Coordination 實作
  - 實作 ICoordinationLayer 接口
  - 保持 Sequential/Parallel/Pipeline 策略

實施:
  - ExecuteSequentialAsync(): 順序執行
  - ExecuteParallelAsync(): 並行執行 (SemaphoreSlim)
  - ExecutePipelineAsync(): 管道執行

參考:
  - src/AIAgentPlatform.Infrastructure/Adapters/SemanticKernel/SemanticKernelCoordination.cs
  - SPRINT-9-2-PLAN.md Phase 2
```

---

### 2.3 可替換性驗證機制

**為什麼需要可替換性驗證?**
- 確保 Framework Abstraction Layer 設計正確
- 驗證接口隔離性 (不依賴 Semantic Kernel)
- 為未來框架遷移提供信心

**技術挑戰**:
- Mock Framework 實作 (用於測試)
- Integration Tests 設計
- 接口合規性驗證

#### MockWorkflowEngine 詳解

**技術決策 TD-073**:
```yaml
決策: 實作 MockWorkflowEngine 用於可替換性驗證
背景:
  - 需要驗證 IWorkflowEngine 接口可替換性
  - 不依賴 Semantic Kernel
  - 簡化版本實作

選項:
  1. 使用 Moq 框架
     優點: 簡單,無需實作
     缺點: 無法驗證完整邏輯
  2. 實作 Mock 版本
     優點: 驗證完整,可測試
     缺點: 工作量略高

決策: 選擇選項 2 (實作 Mock 版本)
  - MockWorkflowEngine: 簡化版本實作
  - 不依賴 Semantic Kernel
  - 驗證接口設計正確

實施:
  - ExecuteWorkflowAsync: 直接返回成功結果
  - GetExecutionStatusAsync: 返回 Mock 狀態
  - PauseAsync/CancelAsync: 更新 Mock 狀態

參考:
  - tests/AIAgentPlatform.IntegrationTests/Mocks/MockWorkflowEngine.cs
  - SPRINT-9-2-PLAN.md Phase 3
```

**Mock Implementation 示例**:
```csharp
public class MockWorkflowEngine : IWorkflowEngine
{
    private readonly Dictionary<Guid, ExecutionContext> _executions = new();

    public Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
        Guid workflowId,
        string userInput,
        Dictionary<string, object>? parameters = null,
        CancellationToken cancellationToken = default)
    {
        var executionId = Guid.NewGuid();

        // Mock 實作: 直接返回成功結果
        var result = new WorkflowExecutionResult
        {
            ExecutionId = executionId,
            Success = true,
            Output = new Dictionary<string, object>
            {
                ["mockOutput"] = "Mock execution completed"
            },
            Duration = TimeSpan.FromSeconds(1),
            CompletedAt = DateTime.UtcNow
        };

        // 記錄執行狀態
        _executions[executionId] = new ExecutionContext
        {
            ExecutionId = executionId,
            WorkflowId = workflowId,
            UserInput = userInput,
            State = WorkflowState.Completed,
            StartTime = DateTime.UtcNow,
            EndTime = DateTime.UtcNow
        };

        return Task.FromResult(result);
    }
}
```

#### Feature Flag 機制詳解

**技術決策 TD-074**:
```yaml
決策: 使用 Microsoft.FeatureManagement 實作 Feature Flag
背景:
  - 需要動態切換 Adapter (SemanticKernel ↔ Mock)
  - A/B Testing 支援
  - 運行時配置

選項:
  1. 環境變數
     優點: 簡單
     缺點: 需要重啟應用
  2. Microsoft.FeatureManagement
     優點: 動態切換,A/B Testing
     缺點: 複雜度略高

決策: 選擇選項 2 (Microsoft.FeatureManagement)
  - 支援動態切換 Adapter
  - A/B Testing 機制
  - 配置管理 (appsettings.json)

實施:
  - FeatureFlagManager: Feature Flag 管理
  - AdapterFactory: Adapter 工廠 (根據 Feature Flag)
  - ABTestingService: A/B Testing 服務

參考:
  - src/AIAgentPlatform.API/Configuration/FeatureFlags.cs
  - docs/technical-implementation/06-configuration-management/01-feature-flags.md
```

#### Replaceability Tests 詳解

**測試策略**:
```csharp
public class ReplaceabilityTests
{
    [Fact]
    public async Task ShouldSupportWorkflowEngineReplacement()
    {
        // Arrange: 使用 MockWorkflowEngine 替換 SemanticKernelWorkflowEngine
        var mockEngine = new MockWorkflowEngine();

        var workflowId = Guid.NewGuid();
        var userInput = "Test workflow execution";

        // Act: 執行工作流
        var result = await mockEngine.ExecuteWorkflowAsync(
            workflowId, userInput);

        // Assert: 驗證結果
        result.Should().NotBeNull();
        result.Success.Should().BeTrue();
        result.ExecutionId.Should().NotBeEmpty();

        // 驗證可以獲取執行狀態
        var status = await mockEngine.GetExecutionStatusAsync(
            result.ExecutionId);

        status.Should().NotBeNull();
        status.State.Should().Be(WorkflowState.Completed);
    }
}
```

---

### 2.4 Phase 1B 完成與移交

**Phase 1B 完成標準**:
```yaml
Sprint 7-9 完成度:
  Sprint 7: Task Generator + Coordination Layer (13 SP) ✅
  Sprint 8: Execution Engine + Messaging (13 SP) ✅
  Sprint 9: Framework Abstraction Layer (13 SP) 🎯
  總計: 35 SP, 9 週, 100%

核心交付:
  ✅ LLM 驅動任務規劃 (Task Generator)
  ✅ Multi-Agent 協調 (Sequential + Parallel)
  ✅ 工作流執行引擎 (Execution Engine)
  ✅ Agent 間消息通訊 (Messaging)
  ✅ 混合狀態管理 (Redis + PostgreSQL)
  ✅ Framework 抽象層 (Abstraction Layer) 🎯

技術債務:
  - Checkpoint/Resume 完整實現 (延後到 Phase 1E)
  - 完整錯誤處理與重試 (延後到 Phase 1D)

Critical Path 解除:
  ✅ Phase 1B 100% 完成
  ✅ 解除後續 10 個 Sprint 阻斷
```

**Phase 1C 準備度評估**:
```yaml
Phase 1C 範圍:
  - Sprint 10-12: Workflow Editor Backend
  - 3 個 Sprint, 9 週, 39 SP

前置依賴檢查:
  ✅ Phase 1B 完成 (100%)
  ✅ Framework Abstraction Layer 完成
  ✅ Workflow Execution Engine 完成
  ✅ State Management 完成

技術準備度:
  ✅ Workflow Definition Storage (準備就緒)
  ✅ Workflow Versioning (準備就緒)
  ✅ Workflow Editor Backend API (準備就緒)

風險評估:
  🟡 Workflow Versioning 複雜度 (中風險)
  🟢 Workflow Definition Storage (低風險)
  🟢 Workflow Editor Backend API (低風險)
```

**Sprint 10 移交 Checklist**:
```yaml
Sprint 9 完成驗收:
  - [ ] Framework Abstraction Layer 完成
  - [ ] SemanticKernelAdapter 完成
  - [ ] Replaceability Verification 完成
  - [ ] 所有測試通過 (單元測試 + 整合測試)
  - [ ] Code Review 完成
  - [ ] 文檔更新完成

Sprint 10 準備:
  - [ ] Sprint 10 計劃文檔準備
  - [ ] Sprint 10 開發環境準備
  - [ ] Sprint 10 技術調研完成
  - [ ] Sprint 10 風險評估完成

移交資料:
  - [ ] Phase 1B 完成文檔
  - [ ] Phase 1C 準備度評估
  - [ ] 技術債務清單
  - [ ] 風險登記簿
```

---

## 三、完整文檔引用清單 (按功能分類)

### 規劃文檔 (8 refs)
1. [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md)
2. [SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md)
3. [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md)
4. [DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md)
5. [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md)
6. [TECHNICAL-DECISIONS-LOG.md](../../1-planning/TECHNICAL-DECISIONS-LOG.md)
7. [ARCHITECTURE-EVOLUTION-ROADMAP.md](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md)
8. [DOCS-REBUILD-EXECUTION-PLAN.md](../../1-planning/DOCS-REBUILD-EXECUTION-PLAN.md)

### Sprint 9 文檔 (7 refs)
9. [SPRINT-9-1-OVERVIEW.md](./SPRINT-9-1-OVERVIEW.md)
10. [SPRINT-9-2-PLAN.md](./SPRINT-9-2-PLAN.md)
11. [SPRINT-9-3-CONTEXT.md](./SPRINT-9-3-CONTEXT.md) (本文件)
12. [SPRINT-9-4-CHECKLIST.md](./SPRINT-9-4-CHECKLIST.md)
13. [SPRINT-9-5-DEV-LOG.md](./SPRINT-9-5-DEV-LOG.md)
14. [SPRINT-9-6-ISSUES.md](./SPRINT-9-6-ISSUES.md)
15. [SPRINT-9-7-RETROSPECTIVE.md](./SPRINT-9-7-RETROSPECTIVE.md)

### Sprint 7-8 參考 (14 refs)
16-22. [SPRINT-7-*.md](../sprint-7/)
23-29. [SPRINT-8-*.md](../sprint-8/)

### 架構設計文檔 (5 refs)
30. [Architecture-Design-Document.md](../../docs/architecture/Architecture-Design-Document.md)
31. [ADR-011: Framework Abstraction Layer](../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md) 🔥 核心
32. [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
33. [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
34. [ADR-007: Internal Communication Strategy](../../docs/architecture/adr/ADR-007-internal-communication-strategy.md)

### 技術實作文檔 (4 refs)
35. [02-semantic-kernel-integration.md](../../docs/technical-implementation/01-backend-net9/02-semantic-kernel-integration.md)
36. [12-workflow-orchestration-implementation.md](../../docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md)
37. [01-feature-flags.md](../../docs/technical-implementation/06-configuration-management/01-feature-flags.md)
38. [01-telemetry-strategy.md](../../docs/technical-implementation/07-monitoring-logging/01-telemetry-strategy.md)

---

## 📚 使用指南

### 目標讀者
- 👨‍💻 **開發人員**: 實作 Framework Abstraction Layer 時的技術參考
- 🏗️ **架構師**: 理解技術決策背景和架構設計
- 🎯 **Tech Lead**: 評估技術風險和實施策略

### 使用場景

**場景 1: 實作 Core Interfaces**
```
問題: 如何設計 IWorkflowEngine 接口?
  → 查看 2.1 核心接口設計詳解
  → 參考 TD-070 設計原則
  → 參考 ADR-011 Framework Abstraction Layer
```

**場景 2: 實作 SemanticKernelAdapter**
```
問題: 如何封裝 Semantic Kernel?
  → 查看 2.2 SemanticKernelAdapter 實作細節
  → 參考 TD-071 Adapter Pattern 策略
  → 參考 SPRINT-9-2-PLAN.md Phase 2
```

**場景 3: 驗證可替換性**
```
問題: 如何驗證接口可替換性?
  → 查看 2.3 可替換性驗證機制
  → 參考 TD-073 Mock Implementation 策略
  → 參考 SPRINT-9-2-PLAN.md Phase 3
```

---

## 📝 更新日誌

### v2.1 (2025-11-14) - 當前版本
- ✅ 建立 Sprint 9 CONTEXT 文件
- ✅ 遵循 v2.1 標準格式 (8 欄位 Header)
- ✅ 詳細的技術決策記錄 (TD-070 到 TD-074)
- ✅ 5 個核心接口設計詳解
- ✅ SemanticKernelAdapter 實作細節
- ✅ 可替換性驗證機制
- ✅ Phase 1B 完成與移交
- ✅ 完整參考文檔列表 (38 refs)
- ✅ 與 Sprint 8 格式 100% 一致

**文件統計** (v2.1):
- 總行數: ~1,500 行
- 技術決策: 5 個 (TD-070 to TD-074)
- 參考文檔: 38 個
- 接口詳解: 5 個

**品質指標**:
- 與 Sprint 8 格式一致性: ✅ 100%
- v2.1 標準符合度: ✅ 100%
- 技術決策完整性: ✅ 充分
- Phase 1B 完成說明: ✅ 完整

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 9 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-9/SPRINT-9-3-CONTEXT.md`
