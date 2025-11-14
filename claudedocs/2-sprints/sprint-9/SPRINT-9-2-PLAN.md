# SPRINT-9-2-PLAN.md - Sprint 9 實施計劃：Framework Abstraction Layer 與 Phase 1B 完成

**版本**: v2.1
**Sprint 編號**: Sprint 9
**Sprint 週期**: Week 25-27 (3 週)
**Phase**: Phase 1B - 工作流引擎核心 (Workflow Engine Core)
**計劃日期**: 2026-03-31 ~ 2026-04-20
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 目錄 (Table of Contents)

### 第一部分: 規劃文檔參考
1. [Sprint 9 實施範圍概述](#第一部分-規劃文檔參考)
2. [架構決策引用](#架構決策引用)
3. [Phase 1B 完成里程碑](#phase-1b-完成里程碑)

### 第二部分: 本 Sprint 要建立什麼
4. [Framework Abstraction Layer 核心](#第二部分-本-sprint-要建立什麼)
5. [Phase 1: 核心接口設計](#phase-1-核心接口設計-5-sp)
6. [Phase 2: SemanticKernelAdapter 實作](#phase-2-semantickerneladapter-實作-5-sp)
7. [Phase 3: 可替換性驗證](#phase-3-可替換性驗證-2-sp)
8. [Phase 4: Phase 1B 總結與移交](#phase-4-phase-1b-總結與移交-1-sp)

### 第三部分: 如何實施
9. [詳細實作指南 - Core Interfaces](#詳細實作指南-core-interfaces)
10. [詳細實作指南 - SemanticKernelAdapter](#詳細實作指南-semantickerneladapter)
11. [詳細實作指南 - Replaceability Verification](#詳細實作指南-replaceability-verification)
12. [詳細實作指南 - Phase 1B Handover](#詳細實作指南-phase-1b-handover)

### 第四部分: 編碼規範與最佳實踐
13. [編碼規範](#第四部分-編碼規範與最佳實踐)
14. [測試策略](#測試策略)
15. [性能優化指南](#性能優化指南)

### 第五部分: 質量保證
16. [質量檢查清單](#第五部分-質量保證)
17. [驗收測試](#驗收測試)
18. [Phase 1B 完成驗收](#phase-1b-完成驗收)

### 第六部分: 參考文檔
19. [完整參考文檔列表](#第六部分-參考文檔)

---

## 第一部分: 規劃文檔參考

### Sprint 9 實施範圍概述

**核心交付物** (13 SP):
```yaml
Phase 1: 核心接口設計 (5 SP)
  目標: 建立 Framework Abstraction Layer 核心接口
  組件:
    - IWorkflowEngine (工作流引擎介面)
    - ITaskGenerator (任務生成器介面)
    - ICoordinationLayer (協調層介面)
    - IExecutionEngine (執行引擎介面)
    - IStateManager (狀態管理器介面)
  關鍵決策:
    - Interface Segregation Principle (ISP)
    - Dependency Inversion Principle (DIP)
    - Framework-agnostic 設計

Phase 2: SemanticKernelAdapter 實作 (5 SP)
  目標: 實作 Semantic Kernel 具體適配器
  組件:
    - SemanticKernelWorkflowEngine
    - SemanticKernelTaskGenerator
    - SemanticKernelCoordination
    - SemanticKernelExecutor
    - SemanticKernelStateManager
  關鍵決策:
    - Adapter Pattern 實作
    - Semantic Kernel SDK 封裝
    - 錯誤處理與重試

Phase 3: 可替換性驗證 (2 SP)
  目標: 驗證 Framework Abstraction Layer 可替換性
  組件:
    - MockWorkflowEngine (測試用)
    - Integration Tests
    - Replaceability Verification
  關鍵決策:
    - 測試策略
    - 接口合規性驗證

Phase 4: Phase 1B 總結與移交 (1 SP)
  目標: 完成 Phase 1B,準備 Phase 1C
  組件:
    - Phase 1B 完成文檔
    - Phase 1C 準備度評估
    - 技術債務清單
    - Sprint 10 移交 Checklist
```

**架構依賴關係**:
```
Sprint 7 成果 (前置依賴):
├─ Task Generator (LLM 驅動) ✅
├─ Sequential Coordination ✅
├─ Parallel Coordination (基礎) ✅
└─ Workflow State Machine ✅

Sprint 8 成果 (前置依賴):
├─ Parallel Coordination (完整) ✅
├─ Workflow Execution Engine ✅
├─ Agent Messaging 系統 ✅
└─ State Management 強化 ✅

Sprint 9 新建 (本 Sprint):
├─ Framework Abstraction Layer 🎯
├─ SemanticKernelAdapter 🎯
├─ Replaceability Verification 🎯
└─ Phase 1B 完成 🎉

Phase 1C 依賴 (後續):
├─ Workflow Editor Backend API
├─ Workflow Definition Storage
└─ Workflow Versioning
```

### 架構決策引用

**核心 ADR**:
- [ADR-011: Framework Abstraction Layer](../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md)
  - **決策**: 建立 Framework Abstraction Layer 解耦 Semantic Kernel
  - **理由**: 支援未來遷移到 Autogen, LangChain 等框架
  - **實施影響**: Sprint 9 完整實作

**相關 ADR**:
- [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- [ADR-007: Internal Communication Strategy](../../docs/architecture/adr/ADR-007-internal-communication-strategy.md)
- [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md)

### Phase 1B 完成里程碑

**🎉 Phase 1B 完成標誌**:
```yaml
Sprint 7: Task Generator + Sequential (9 SP) ✅
  - Task Generator (LLM 驅動) ✅
  - Sequential Coordination ✅
  - Workflow State Machine ✅

Sprint 8: Parallel + Messaging (13 SP) ✅
  - Parallel Coordination ✅
  - Workflow Execution Engine ✅
  - Agent Messaging 系統 ✅
  - State Management 強化 ✅

Sprint 9: Framework Abstraction (13 SP) 🎯
  - Framework Abstraction Layer 🎯
  - SemanticKernelAdapter 🎯
  - Replaceability Verification 🎯
  - Phase 1B 完成 🎉

Phase 1B 總計:
  - 總 Story Points: 35 SP
  - 總週期: 9 週 (Week 19-27)
  - 完成度: 100%
  - Critical Path: 解除 ✅
```

**Phase 1B 核心成果**:
```yaml
工作流引擎核心能力:
  ✅ LLM 驅動任務規劃 (Task Generator)
  ✅ Multi-Agent 協調 (Sequential + Parallel)
  ✅ 工作流執行引擎 (Execution Engine)
  ✅ Agent 間消息通訊 (Messaging)
  ✅ 混合狀態管理 (Redis + PostgreSQL)
  ✅ Framework 抽象層 (Abstraction Layer)

技術債務:
  - Checkpoint/Resume 完整實現 (延後到 Phase 1E)
  - 完整錯誤處理與重試 (延後到 Phase 1D)
  - 可視化工作流編輯器 (Phase 2)
  - Workflow 模板系統 (Phase 2)
```

---

## 第二部分: 本 Sprint 要建立什麼

### Framework Abstraction Layer 核心

**為什麼要建立 Framework Abstraction Layer?**
- **問題**: 當前實作與 Semantic Kernel 強耦合
- **風險**: 未來無法遷移到其他 Agent 框架 (Autogen, LangChain)
- **解決方案**: 建立 Framework Abstraction Layer,實作 Adapter Pattern
- **預期收益**:
  - Framework 可替換性 (Semantic Kernel ↔ Autogen ↔ LangChain)
  - 降低框架依賴風險
  - 提升系統可測試性 (Mock Framework)

**技術挑戰**:
- Interface Segregation (避免接口過大)
- Adapter Pattern 性能開銷
- Semantic Kernel 特性封裝 (Plugins, Functions)
- 測試可替換性 (Integration Tests)

**參考文檔**:
- [ADR-011: Framework Abstraction Layer](../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md)
- [Semantic Kernel Integration](../../docs/technical-implementation/01-backend-net9/02-semantic-kernel-integration.md)
- [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) Lines 500-600

---

## Phase 1: 核心接口設計 (5 SP)

### 1.1 目標與背景

**為什麼需要 5 個核心接口?**
- **IWorkflowEngine**: 工作流引擎入口 (高層接口)
- **ITaskGenerator**: 任務生成器 (LLM 驅動)
- **ICoordinationLayer**: Multi-Agent 協調層
- **IExecutionEngine**: 工作流執行引擎
- **IStateManager**: 狀態管理器 (Redis + PostgreSQL)

**設計原則**:
- **Interface Segregation Principle (ISP)**: 接口應小而專注
- **Dependency Inversion Principle (DIP)**: 依賴接口而非實作
- **Single Responsibility Principle (SRP)**: 每個接口職責單一

### 1.2 核心組件

#### IWorkflowEngine 介面

**檔案位置**: `src/AIAgentPlatform.Application/Abstractions/IWorkflowEngine.cs`

**設計決策**:
```yaml
職責:
  - 工作流引擎入口 (Facade Pattern)
  - 整合 TaskGenerator + Coordination + Execution + State

設計原則:
  - 高層抽象 (不暴露框架細節)
  - Framework-agnostic (可替換 Semantic Kernel)

參考: ADR-011 Framework Abstraction Layer
```

**介面定義**:
```csharp
namespace AIAgentPlatform.Application.Abstractions;

/// <summary>
/// 工作流引擎核心介面 (Framework Abstraction)
/// </summary>
public interface IWorkflowEngine
{
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

    /// <summary>
    /// 獲取工作流執行狀態
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>執行狀態</returns>
    Task<WorkflowExecutionStatus> GetExecutionStatusAsync(
        Guid executionId,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 暫停工作流執行
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    Task PauseExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 恢復工作流執行
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>執行結果</returns>
    Task<WorkflowExecutionResult> ResumeExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 取消工作流執行
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    Task CancelExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default);
}

/// <summary>
/// 工作流執行結果
/// </summary>
public class WorkflowExecutionResult
{
    public Guid ExecutionId { get; set; }
    public bool Success { get; set; }
    public Dictionary<string, object> Output { get; set; } = new();
    public string? ErrorMessage { get; set; }
    public TimeSpan Duration { get; set; }
    public DateTime CompletedAt { get; set; }
}

/// <summary>
/// 工作流執行狀態
/// </summary>
public class WorkflowExecutionStatus
{
    public Guid ExecutionId { get; set; }
    public WorkflowState State { get; set; }
    public int TotalTasks { get; set; }
    public int CompletedTasks { get; set; }
    public int ProgressPercentage => TotalTasks > 0
        ? (int)((double)CompletedTasks / TotalTasks * 100)
        : 0;
    public DateTime StartTime { get; set; }
    public DateTime? EndTime { get; set; }
    public string? CurrentTaskName { get; set; }
}

public enum WorkflowState
{
    Pending,
    Running,
    Paused,
    Completed,
    Failed,
    Cancelled
}
```

#### ITaskGenerator 介面

**檔案位置**: `src/AIAgentPlatform.Application/Abstractions/ITaskGenerator.cs`

**設計決策**:
```yaml
職責:
  - LLM 驅動任務規劃
  - 任務分解與依賴分析
  - Framework-agnostic (不依賴 Semantic Kernel)

設計原則:
  - 抽象 LLM 能力 (不暴露 Semantic Kernel Plugins)
  - 支援多種 LLM (OpenAI, Azure OpenAI, Anthropic)

參考: ADR-011 Framework Abstraction Layer
```

**介面定義**:
```csharp
namespace AIAgentPlatform.Application.Abstractions;

/// <summary>
/// 任務生成器介面 (Framework Abstraction)
/// </summary>
public interface ITaskGenerator
{
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

    /// <summary>
    /// 驗證任務計劃
    /// </summary>
    /// <param name="plan">任務計劃</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>驗證結果</returns>
    Task<TaskPlanValidationResult> ValidatePlanAsync(
        TaskPlan plan,
        CancellationToken cancellationToken = default);
}

/// <summary>
/// 任務計劃
/// </summary>
public class TaskPlan
{
    public required IReadOnlyList<WorkflowTask> Tasks { get; init; }
    public required TaskDependencyGraph DependencyGraph { get; init; }
    public TimeSpan EstimatedDuration { get; init; }
}

/// <summary>
/// 任務依賴圖
/// </summary>
public class TaskDependencyGraph
{
    public required IReadOnlyDictionary<Guid, WorkflowTask> Tasks { get; init; }
    public required IReadOnlyDictionary<Guid, IReadOnlyList<Guid>> Dependencies { get; init; }

    /// <summary>
    /// 獲取拓撲排序後的任務執行順序
    /// </summary>
    public IReadOnlyList<WorkflowTask> GetExecutionOrder()
    {
        // Topological Sort 實作
        var sorted = new List<WorkflowTask>();
        var visited = new HashSet<Guid>();

        foreach (var taskId in Tasks.Keys.Where(id => !Dependencies[id].Any()))
        {
            Visit(taskId, visited, sorted);
        }

        return sorted;
    }

    private void Visit(Guid taskId, HashSet<Guid> visited, List<WorkflowTask> sorted)
    {
        if (visited.Contains(taskId)) return;
        visited.Add(taskId);

        foreach (var depId in Dependencies[taskId])
        {
            Visit(depId, visited, sorted);
        }

        sorted.Add(Tasks[taskId]);
    }
}

/// <summary>
/// 任務計劃驗證結果
/// </summary>
public class TaskPlanValidationResult
{
    public bool IsValid { get; set; }
    public List<string> Errors { get; set; } = new();
    public List<string> Warnings { get; set; } = new();
}
```

#### ICoordinationLayer 介面

**檔案位置**: `src/AIAgentPlatform.Application/Abstractions/ICoordinationLayer.cs`

**設計決策**:
```yaml
職責:
  - Multi-Agent 協調
  - Sequential/Parallel 執行策略
  - Framework-agnostic (不依賴 Semantic Kernel Agents)

設計原則:
  - 策略模式 (Strategy Pattern)
  - 支援多種協調策略 (Sequential, Parallel, Pipeline)

參考: ADR-009 Workflow Orchestration Strategy
```

**介面定義**:
```csharp
namespace AIAgentPlatform.Application.Abstractions;

/// <summary>
/// 協調層介面 (Framework Abstraction)
/// </summary>
public interface ICoordinationLayer
{
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

    /// <summary>
    /// 選擇 Agent 執行任務
    /// </summary>
    /// <param name="task">任務</param>
    /// <param name="context">執行上下文</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>Agent ID</returns>
    Task<string> SelectAgentAsync(
        WorkflowTask task,
        ExecutionContext context,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 監控任務執行進度
    /// </summary>
    /// <param name="taskId">任務 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>任務狀態</returns>
    Task<TaskExecutionStatus> MonitorTaskAsync(
        Guid taskId,
        CancellationToken cancellationToken = default);
}

/// <summary>
/// 協調策略
/// </summary>
public enum CoordinationStrategy
{
    /// <summary>
    /// 順序執行
    /// </summary>
    Sequential,

    /// <summary>
    /// 並行執行
    /// </summary>
    Parallel,

    /// <summary>
    /// 管道執行 (輸出傳遞)
    /// </summary>
    Pipeline
}

/// <summary>
/// 協調結果
/// </summary>
public class CoordinationResult
{
    public bool Success { get; set; }
    public Dictionary<Guid, TaskResult> TaskResults { get; set; } = new();
    public string? ErrorMessage { get; set; }
    public TimeSpan Duration { get; set; }
}

/// <summary>
/// 任務結果
/// </summary>
public class TaskResult
{
    public Guid TaskId { get; set; }
    public bool Success { get; set; }
    public Dictionary<string, object> Output { get; set; } = new();
    public string? ErrorMessage { get; set; }
    public TimeSpan Duration { get; set; }
}

/// <summary>
/// 任務執行狀態
/// </summary>
public class TaskExecutionStatus
{
    public Guid TaskId { get; set; }
    public TaskState State { get; set; }
    public int ProgressPercentage { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime? EndTime { get; set; }
}

public enum TaskState
{
    Pending,
    Running,
    Completed,
    Failed,
    Cancelled
}
```

#### IExecutionEngine 介面

**檔案位置**: `src/AIAgentPlatform.Application/Abstractions/IExecutionEngine.cs`

**設計決策**:
```yaml
職責:
  - 工作流執行引擎核心邏輯
  - Checkpoint/Resume 支援
  - Framework-agnostic (不依賴 Semantic Kernel)

設計原則:
  - 命令模式 (Command Pattern)
  - 狀態機 (State Machine)

參考: ADR-009 Workflow Orchestration Strategy
```

**介面定義**:
```csharp
namespace AIAgentPlatform.Application.Abstractions;

/// <summary>
/// 執行引擎介面 (Framework Abstraction)
/// </summary>
public interface IExecutionEngine
{
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

    /// <summary>
    /// 更新執行上下文
    /// </summary>
    /// <param name="context">執行上下文</param>
    /// <param name="nodeId">節點 ID</param>
    /// <param name="result">節點結果</param>
    void UpdateExecutionContext(
        ExecutionContext context,
        Guid nodeId,
        NodeExecutionResult result);

    /// <summary>
    /// 驗證執行條件
    /// </summary>
    /// <param name="node">工作流節點</param>
    /// <param name="context">執行上下文</param>
    /// <returns>是否可執行</returns>
    Task<bool> CanExecuteNodeAsync(
        WorkflowNode node,
        ExecutionContext context);
}

/// <summary>
/// 執行上下文
/// </summary>
public class ExecutionContext
{
    public Guid ExecutionId { get; set; }
    public Guid WorkflowId { get; set; }
    public string UserInput { get; set; } = string.Empty;
    public Dictionary<string, object> Parameters { get; set; } = new();
    public Dictionary<string, object> Variables { get; set; } = new();
    public WorkflowState State { get; set; }
    public int TotalTasks { get; set; }
    public int CompletedTasks { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime? EndTime { get; set; }
    public TimeSpan Duration { get; set; }
    public string? ErrorMessage { get; set; }
    public Guid? LastExecutedNodeId { get; set; }
}

/// <summary>
/// 節點執行結果
/// </summary>
public class NodeExecutionResult
{
    public Guid NodeId { get; set; }
    public bool Success { get; set; }
    public Dictionary<string, object> Output { get; set; } = new();
    public string? ErrorMessage { get; set; }
    public TimeSpan Duration { get; set; }
}
```

#### IStateManager 介面

**檔案位置**: `src/AIAgentPlatform.Application/Abstractions/IStateManager.cs`

**設計決策**:
```yaml
職責:
  - 執行狀態管理 (Redis + PostgreSQL)
  - Checkpoint/Resume 支援
  - Framework-agnostic (不依賴 Semantic Kernel)

設計原則:
  - Repository Pattern
  - 混合存儲策略 (Cache + Persistence)

參考: ADR-006 Hybrid State Management
```

**介面定義**:
```csharp
namespace AIAgentPlatform.Application.Abstractions;

/// <summary>
/// 狀態管理器介面 (Framework Abstraction)
/// </summary>
public interface IStateManager
{
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
    /// 刪除執行狀態
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    Task DeleteStateAsync(
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

    /// <summary>
    /// 載入 Checkpoint
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="checkpoint">Checkpoint 名稱</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>執行上下文</returns>
    Task<ExecutionContext?> LoadCheckpointAsync(
        Guid executionId,
        string checkpoint,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 獲取執行歷史 (分頁)
    /// </summary>
    /// <param name="workflowId">工作流 ID (可選)</param>
    /// <param name="pageIndex">頁索引</param>
    /// <param name="pageSize">頁大小</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>執行歷史分頁結果</returns>
    Task<PagedResult<ExecutionContext>> GetExecutionHistoryAsync(
        Guid? workflowId = null,
        int pageIndex = 1,
        int pageSize = 20,
        CancellationToken cancellationToken = default);
}

/// <summary>
/// 分頁結果
/// </summary>
public class PagedResult<T>
{
    public required IReadOnlyList<T> Items { get; init; }
    public int TotalCount { get; init; }
    public int PageIndex { get; init; }
    public int PageSize { get; init; }
    public int TotalPages => (int)Math.Ceiling((double)TotalCount / PageSize);
}
```

**關鍵實作細節 (Phase 1)**:
1. **Interface Segregation**: 5 個專注的接口,避免單一巨大接口
2. **Framework-agnostic**: 所有接口不依賴 Semantic Kernel
3. **Dependency Inversion**: 高層模組依賴接口,低層模組實作接口
4. **Testability**: 可輕易 Mock 接口進行單元測試
5. **Replaceability**: 可替換為其他 Agent 框架 (Autogen, LangChain)

---

## Phase 2: SemanticKernelAdapter 實作 (5 SP)

### 2.1 目標與背景

**為什麼要實作 SemanticKernelAdapter?**
- 將 Semantic Kernel SDK 封裝到 Framework Abstraction Layer 接口
- 隔離框架特定實作 (Plugins, Functions, Agents)
- 為未來框架遷移做準備

**技術挑戰**:
- Semantic Kernel Plugins 封裝
- Semantic Kernel Agents 生命週期管理
- 錯誤處理與重試
- 性能開銷 (Adapter Pattern)

### 2.2 核心組件

#### SemanticKernelWorkflowEngine

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Adapters/SemanticKernel/SemanticKernelWorkflowEngine.cs`

**設計決策**:
```yaml
職責:
  - 實作 IWorkflowEngine 接口
  - 整合 Semantic Kernel SDK
  - 協調其他 4 個 Adapter

實施策略:
  - Facade Pattern (整合多個 Adapter)
  - Dependency Injection (注入其他 Adapter)

參考: ADR-011 Framework Abstraction Layer
```

**核心邏輯**:
```csharp
namespace AIAgentPlatform.Infrastructure.Adapters.SemanticKernel;

/// <summary>
/// Semantic Kernel 工作流引擎適配器
/// </summary>
public class SemanticKernelWorkflowEngine : IWorkflowEngine
{
    private readonly ITaskGenerator _taskGenerator;
    private readonly ICoordinationLayer _coordinationLayer;
    private readonly IExecutionEngine _executionEngine;
    private readonly IStateManager _stateManager;
    private readonly IWorkflowRepository _workflowRepository;
    private readonly ILogger<SemanticKernelWorkflowEngine> _logger;

    public SemanticKernelWorkflowEngine(
        ITaskGenerator taskGenerator,
        ICoordinationLayer coordinationLayer,
        IExecutionEngine executionEngine,
        IStateManager stateManager,
        IWorkflowRepository workflowRepository,
        ILogger<SemanticKernelWorkflowEngine> logger)
    {
        _taskGenerator = taskGenerator;
        _coordinationLayer = coordinationLayer;
        _executionEngine = executionEngine;
        _stateManager = stateManager;
        _workflowRepository = workflowRepository;
        _logger = logger;
    }

    public async Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
        Guid workflowId,
        string userInput,
        Dictionary<string, object>? parameters = null,
        CancellationToken cancellationToken = default)
    {
        var stopwatch = Stopwatch.StartNew();
        var executionId = Guid.NewGuid();

        _logger.LogInformation(
            "開始執行工作流 {WorkflowId} (ExecutionId: {ExecutionId})",
            workflowId, executionId);

        try
        {
            // 1. 載入工作流定義
            var workflow = await _workflowRepository.GetByIdAsync(
                workflowId, cancellationToken);

            if (workflow == null)
                throw new WorkflowNotFoundException(workflowId);

            // 2. 創建執行上下文
            var context = _executionEngine.CreateExecutionContext(
                workflowId, userInput, parameters);

            context.ExecutionId = executionId;

            // 3. 生成任務計劃 (LLM 驅動)
            var taskPlan = await _taskGenerator.GenerateTaskPlanAsync(
                userInput, workflow, cancellationToken);

            context.TotalTasks = taskPlan.Tasks.Count;

            // 4. 保存初始狀態
            await _stateManager.SaveStateAsync(context, cancellationToken);

            // 5. 執行工作流節點
            foreach (var node in workflow.Nodes.OrderBy(n => n.Order))
            {
                if (cancellationToken.IsCancellationRequested)
                {
                    _logger.LogWarning("工作流執行被取消");
                    context.State = WorkflowState.Cancelled;
                    break;
                }

                // 5.1 驗證執行條件
                if (!await _executionEngine.CanExecuteNodeAsync(node, context))
                {
                    _logger.LogWarning(
                        "節點 {NodeId} 執行條件不滿足,跳過", node.Id);
                    continue;
                }

                // 5.2 獲取節點任務
                var nodeTasks = taskPlan.Tasks
                    .Where(t => t.NodeId == node.Id)
                    .ToList();

                // 5.3 執行節點
                var nodeResult = await _executionEngine.ExecuteNodeAsync(
                    node, nodeTasks, context, cancellationToken);

                // 5.4 更新上下文
                _executionEngine.UpdateExecutionContext(
                    context, node.Id, nodeResult);

                // 5.5 保存 Checkpoint
                await _stateManager.SaveCheckpointAsync(
                    context, $"node-{node.Id}", cancellationToken);

                // 5.6 錯誤處理
                if (!nodeResult.Success && !node.ContinueOnError)
                {
                    _logger.LogError(
                        "節點 {NodeId} 執行失敗,停止工作流", node.Id);

                    context.State = WorkflowState.Failed;
                    context.ErrorMessage = nodeResult.ErrorMessage;

                    stopwatch.Stop();
                    context.Duration = stopwatch.Elapsed;

                    await _stateManager.SaveStateAsync(
                        context, cancellationToken);

                    return new WorkflowExecutionResult
                    {
                        ExecutionId = executionId,
                        Success = false,
                        ErrorMessage = nodeResult.ErrorMessage,
                        Duration = stopwatch.Elapsed,
                        CompletedAt = DateTime.UtcNow
                    };
                }
            }

            // 6. 執行完成
            stopwatch.Stop();
            context.State = WorkflowState.Completed;
            context.EndTime = DateTime.UtcNow;
            context.Duration = stopwatch.Elapsed;

            await _stateManager.SaveStateAsync(context, cancellationToken);

            _logger.LogInformation(
                "工作流執行完成 (ExecutionId: {ExecutionId}, 耗時: {Duration}ms)",
                executionId, stopwatch.ElapsedMilliseconds);

            return new WorkflowExecutionResult
            {
                ExecutionId = executionId,
                Success = true,
                Output = context.Variables,
                Duration = stopwatch.Elapsed,
                CompletedAt = DateTime.UtcNow
            };
        }
        catch (Exception ex)
        {
            stopwatch.Stop();

            _logger.LogError(ex,
                "工作流執行異常 (ExecutionId: {ExecutionId})",
                executionId);

            return new WorkflowExecutionResult
            {
                ExecutionId = executionId,
                Success = false,
                ErrorMessage = ex.Message,
                Duration = stopwatch.Elapsed,
                CompletedAt = DateTime.UtcNow
            };
        }
    }

    public async Task<WorkflowExecutionStatus> GetExecutionStatusAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        var context = await _stateManager.LoadStateAsync(
            executionId, cancellationToken);

        if (context == null)
            throw new ExecutionNotFoundException(executionId);

        return new WorkflowExecutionStatus
        {
            ExecutionId = executionId,
            State = context.State,
            TotalTasks = context.TotalTasks,
            CompletedTasks = context.CompletedTasks,
            StartTime = context.StartTime,
            EndTime = context.EndTime
        };
    }

    public async Task PauseExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        var context = await _stateManager.LoadStateAsync(
            executionId, cancellationToken);

        if (context == null)
            throw new ExecutionNotFoundException(executionId);

        context.State = WorkflowState.Paused;

        await _stateManager.SaveStateAsync(context, cancellationToken);

        _logger.LogInformation(
            "工作流執行已暫停 (ExecutionId: {ExecutionId})", executionId);
    }

    public async Task<WorkflowExecutionResult> ResumeExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        // 簡化版本: 暫時不實現完整的 Resume 邏輯
        throw new NotImplementedException(
            "Resume 功能將在 Phase 1E (Sprint 15) 完整實現");
    }

    public async Task CancelExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        var context = await _stateManager.LoadStateAsync(
            executionId, cancellationToken);

        if (context == null)
            throw new ExecutionNotFoundException(executionId);

        context.State = WorkflowState.Cancelled;

        await _stateManager.SaveStateAsync(context, cancellationToken);

        _logger.LogInformation(
            "工作流執行已取消 (ExecutionId: {ExecutionId})", executionId);
    }
}
```

#### SemanticKernelTaskGenerator

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Adapters/SemanticKernel/SemanticKernelTaskGenerator.cs`

**設計決策**:
```yaml
職責:
  - 實作 ITaskGenerator 接口
  - 封裝 Semantic Kernel LLM 調用
  - Prompt Engineering for Task Decomposition

實施策略:
  - 使用 Semantic Kernel KernelFunction
  - 封裝 LLM Prompt Templates
  - 處理 LLM 返回的 JSON 解析

參考: ADR-011 Framework Abstraction Layer
```

**核心邏輯**:
```csharp
namespace AIAgentPlatform.Infrastructure.Adapters.SemanticKernel;

/// <summary>
/// Semantic Kernel 任務生成器適配器
/// </summary>
public class SemanticKernelTaskGenerator : ITaskGenerator
{
    private readonly Kernel _kernel;
    private readonly ILogger<SemanticKernelTaskGenerator> _logger;

    public SemanticKernelTaskGenerator(
        Kernel kernel,
        ILogger<SemanticKernelTaskGenerator> logger)
    {
        _kernel = kernel;
        _logger = logger;
    }

    public async Task<TaskPlan> GenerateTaskPlanAsync(
        string goal,
        WorkflowDefinition workflow,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation(
            "開始生成任務計劃 (Goal: {Goal})", goal);

        var stopwatch = Stopwatch.StartNew();

        // 1. 構建 LLM Prompt
        var prompt = BuildTaskDecompositionPrompt(goal, workflow);

        // 2. 調用 Semantic Kernel LLM
        var kernelArguments = new KernelArguments
        {
            ["goal"] = goal,
            ["workflow"] = JsonSerializer.Serialize(workflow),
            ["prompt"] = prompt
        };

        var function = _kernel.CreateFunctionFromPrompt(
            prompt,
            new OpenAIPromptExecutionSettings
            {
                Temperature = 0.7,
                MaxTokens = 2000
            });

        var result = await _kernel.InvokeAsync(
            function,
            kernelArguments,
            cancellationToken);

        // 3. 解析 LLM 返回的任務列表
        var taskListJson = result.ToString();
        var taskList = ParseTaskListFromLLM(taskListJson, workflow);

        stopwatch.Stop();

        _logger.LogInformation(
            "任務計劃生成完成 (任務數: {TaskCount}, 耗時: {Duration}ms)",
            taskList.Count, stopwatch.ElapsedMilliseconds);

        // 4. 分析依賴關係
        var dependencyGraph = await AnalyzeDependenciesAsync(
            taskList, cancellationToken);

        // 5. 估算執行時間
        var estimatedDuration = EstimateDuration(taskList, dependencyGraph);

        return new TaskPlan
        {
            Tasks = taskList,
            DependencyGraph = dependencyGraph,
            EstimatedDuration = estimatedDuration
        };
    }

    public async Task<TaskDependencyGraph> AnalyzeDependenciesAsync(
        IEnumerable<WorkflowTask> tasks,
        CancellationToken cancellationToken = default)
    {
        var taskDict = tasks.ToDictionary(t => t.Id, t => t);
        var depDict = tasks.ToDictionary(
            t => t.Id,
            t => (IReadOnlyList<Guid>)t.Dependencies.ToList());

        // 驗證無循環依賴
        ValidateAcyclicDependencies(taskDict, depDict);

        return new TaskDependencyGraph
        {
            Tasks = taskDict,
            Dependencies = depDict
        };
    }

    public async Task<TaskPlanValidationResult> ValidatePlanAsync(
        TaskPlan plan,
        CancellationToken cancellationToken = default)
    {
        var result = new TaskPlanValidationResult { IsValid = true };

        // 1. 驗證任務數量
        if (!plan.Tasks.Any())
        {
            result.IsValid = false;
            result.Errors.Add("任務列表為空");
        }

        // 2. 驗證依賴關係
        try
        {
            var taskDict = plan.Tasks.ToDictionary(t => t.Id, t => t);
            var depDict = plan.DependencyGraph.Dependencies;

            ValidateAcyclicDependencies(taskDict, depDict);
        }
        catch (Exception ex)
        {
            result.IsValid = false;
            result.Errors.Add($"依賴關係驗證失敗: {ex.Message}");
        }

        // 3. 驗證預估時間
        if (plan.EstimatedDuration == TimeSpan.Zero)
        {
            result.Warnings.Add("預估執行時間為 0");
        }

        return result;
    }

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

    private List<WorkflowTask> ParseTaskListFromLLM(
        string taskListJson, WorkflowDefinition workflow)
    {
        try
        {
            var jsonDoc = JsonDocument.Parse(taskListJson);
            var tasksArray = jsonDoc.RootElement.GetProperty("tasks");

            var tasks = new List<WorkflowTask>();

            foreach (var taskElement in tasksArray.EnumerateArray())
            {
                var task = new WorkflowTask
                {
                    Id = Guid.Parse(taskElement.GetProperty("id").GetString()!),
                    NodeId = Guid.Parse(taskElement.GetProperty("nodeId").GetString()!),
                    Name = taskElement.GetProperty("name").GetString()!,
                    Description = taskElement.GetProperty("description").GetString()!,
                    AgentType = taskElement.GetProperty("agentType").GetString()!,
                    Dependencies = taskElement.GetProperty("dependencies")
                        .EnumerateArray()
                        .Select(e => Guid.Parse(e.GetString()!))
                        .ToList(),
                    EstimatedDuration = XmlConvert.ToTimeSpan(
                        taskElement.GetProperty("estimatedDuration").GetString()!)
                };

                tasks.Add(task);
            }

            return tasks;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "解析 LLM 返回的任務列表失敗");
            throw new TaskGenerationException(
                "LLM 返回格式錯誤,無法解析任務列表", ex);
        }
    }

    private void ValidateAcyclicDependencies(
        Dictionary<Guid, WorkflowTask> taskDict,
        IReadOnlyDictionary<Guid, IReadOnlyList<Guid>> depDict)
    {
        var visited = new HashSet<Guid>();
        var recStack = new HashSet<Guid>();

        foreach (var taskId in taskDict.Keys)
        {
            if (HasCycle(taskId, visited, recStack, depDict))
            {
                throw new InvalidOperationException(
                    $"檢測到循環依賴: Task {taskId}");
            }
        }
    }

    private bool HasCycle(
        Guid taskId,
        HashSet<Guid> visited,
        HashSet<Guid> recStack,
        IReadOnlyDictionary<Guid, IReadOnlyList<Guid>> depDict)
    {
        if (recStack.Contains(taskId))
            return true;

        if (visited.Contains(taskId))
            return false;

        visited.Add(taskId);
        recStack.Add(taskId);

        if (depDict.TryGetValue(taskId, out var deps))
        {
            foreach (var depId in deps)
            {
                if (HasCycle(depId, visited, recStack, depDict))
                    return true;
            }
        }

        recStack.Remove(taskId);
        return false;
    }

    private TimeSpan EstimateDuration(
        List<WorkflowTask> tasks,
        TaskDependencyGraph dependencyGraph)
    {
        // 簡化版本: 計算關鍵路徑 (Critical Path)
        var executionOrder = dependencyGraph.GetExecutionOrder();
        return executionOrder.Sum(t => t.EstimatedDuration);
    }
}
```

#### SemanticKernelCoordination

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Adapters/SemanticKernel/SemanticKernelCoordination.cs`

**設計決策**:
```yaml
職責:
  - 實作 ICoordinationLayer 接口
  - 封裝 Semantic Kernel Agents
  - Sequential/Parallel 協調策略

實施策略:
  - 使用 Semantic Kernel ChatCompletionAgent
  - 整合 Sprint 7-8 的 Coordination 實作

參考: ADR-009 Workflow Orchestration Strategy
```

**核心邏輯**:
```csharp
namespace AIAgentPlatform.Infrastructure.Adapters.SemanticKernel;

/// <summary>
/// Semantic Kernel 協調層適配器
/// </summary>
public class SemanticKernelCoordination : ICoordinationLayer
{
    private readonly Kernel _kernel;
    private readonly IAgentFactory _agentFactory;
    private readonly ILogger<SemanticKernelCoordination> _logger;

    public SemanticKernelCoordination(
        Kernel kernel,
        IAgentFactory agentFactory,
        ILogger<SemanticKernelCoordination> logger)
    {
        _kernel = kernel;
        _agentFactory = agentFactory;
        _logger = logger;
    }

    public async Task<CoordinationResult> CoordinateAsync(
        IEnumerable<WorkflowTask> tasks,
        CoordinationStrategy strategy,
        ExecutionContext context,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation(
            "開始協調執行 {TaskCount} 個任務 (策略: {Strategy})",
            tasks.Count(), strategy);

        return strategy switch
        {
            CoordinationStrategy.Sequential =>
                await ExecuteSequentialAsync(tasks, context, cancellationToken),

            CoordinationStrategy.Parallel =>
                await ExecuteParallelAsync(tasks, context, cancellationToken),

            CoordinationStrategy.Pipeline =>
                await ExecutePipelineAsync(tasks, context, cancellationToken),

            _ => throw new NotSupportedException(
                $"協調策略 {strategy} 不支援")
        };
    }

    public async Task<string> SelectAgentAsync(
        WorkflowTask task,
        ExecutionContext context,
        CancellationToken cancellationToken = default)
    {
        // 簡化版本: 基於任務類型選擇 Agent
        return task.AgentType;
    }

    public async Task<TaskExecutionStatus> MonitorTaskAsync(
        Guid taskId,
        CancellationToken cancellationToken = default)
    {
        // 簡化版本: 暫時返回預設狀態
        return new TaskExecutionStatus
        {
            TaskId = taskId,
            State = TaskState.Running,
            ProgressPercentage = 0,
            StartTime = DateTime.UtcNow
        };
    }

    private async Task<CoordinationResult> ExecuteSequentialAsync(
        IEnumerable<WorkflowTask> tasks,
        ExecutionContext context,
        CancellationToken cancellationToken)
    {
        var stopwatch = Stopwatch.StartNew();
        var taskResults = new Dictionary<Guid, TaskResult>();

        foreach (var task in tasks)
        {
            if (cancellationToken.IsCancellationRequested)
            {
                _logger.LogWarning("Sequential 執行被取消");
                break;
            }

            var taskResult = await ExecuteSingleTaskAsync(
                task, context, cancellationToken);

            taskResults[task.Id] = taskResult;

            if (!taskResult.Success)
            {
                _logger.LogError(
                    "任務 {TaskId} 執行失敗,停止 Sequential 執行", task.Id);

                stopwatch.Stop();

                return new CoordinationResult
                {
                    Success = false,
                    TaskResults = taskResults,
                    ErrorMessage = taskResult.ErrorMessage,
                    Duration = stopwatch.Elapsed
                };
            }
        }

        stopwatch.Stop();

        _logger.LogInformation(
            "Sequential 執行完成 (耗時: {Duration}ms)",
            stopwatch.ElapsedMilliseconds);

        return new CoordinationResult
        {
            Success = true,
            TaskResults = taskResults,
            Duration = stopwatch.Elapsed
        };
    }

    private async Task<CoordinationResult> ExecuteParallelAsync(
        IEnumerable<WorkflowTask> tasks,
        ExecutionContext context,
        CancellationToken cancellationToken)
    {
        var stopwatch = Stopwatch.StartNew();

        // 使用 SemaphoreSlim 限制並發度
        using var semaphore = new SemaphoreSlim(10);
        using var cts = CancellationTokenSource.CreateLinkedTokenSource(
            cancellationToken);

        var taskResults = new ConcurrentDictionary<Guid, TaskResult>();

        var executionTasks = tasks.Select(async task =>
        {
            await semaphore.WaitAsync(cts.Token);
            try
            {
                var taskResult = await ExecuteSingleTaskAsync(
                    task, context, cts.Token);

                taskResults[task.Id] = taskResult;

                if (!taskResult.Success)
                {
                    _logger.LogWarning(
                        "任務 {TaskId} 執行失敗", task.Id);
                    cts.Cancel(); // 任一失敗則取消其他任務
                }
            }
            catch (OperationCanceledException)
            {
                _logger.LogWarning("任務 {TaskId} 被取消", task.Id);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex,
                    "任務 {TaskId} 執行異常", task.Id);

                taskResults[task.Id] = new TaskResult
                {
                    TaskId = task.Id,
                    Success = false,
                    ErrorMessage = ex.Message
                };

                cts.Cancel();
            }
            finally
            {
                semaphore.Release();
            }
        });

        try
        {
            await Task.WhenAll(executionTasks);
        }
        catch (OperationCanceledException)
        {
            _logger.LogWarning("Parallel 執行被取消");
        }

        stopwatch.Stop();

        var success = taskResults.Values.All(r => r.Success);

        _logger.LogInformation(
            "Parallel 執行完成 (成功: {Success}, 耗時: {Duration}ms)",
            success, stopwatch.ElapsedMilliseconds);

        return new CoordinationResult
        {
            Success = success,
            TaskResults = taskResults.ToDictionary(
                kvp => kvp.Key, kvp => kvp.Value),
            ErrorMessage = success ? null : "部分任務執行失敗",
            Duration = stopwatch.Elapsed
        };
    }

    private async Task<CoordinationResult> ExecutePipelineAsync(
        IEnumerable<WorkflowTask> tasks,
        ExecutionContext context,
        CancellationToken cancellationToken)
    {
        // Pipeline: 將上一個任務的輸出作為下一個任務的輸入
        var stopwatch = Stopwatch.StartNew();
        var taskResults = new Dictionary<Guid, TaskResult>();

        object? pipelineData = null;

        foreach (var task in tasks)
        {
            if (cancellationToken.IsCancellationRequested)
            {
                _logger.LogWarning("Pipeline 執行被取消");
                break;
            }

            // 將 pipeline data 注入到任務輸入
            if (pipelineData != null)
            {
                task.Input["pipelineData"] = pipelineData;
            }

            var taskResult = await ExecuteSingleTaskAsync(
                task, context, cancellationToken);

            taskResults[task.Id] = taskResult;

            if (!taskResult.Success)
            {
                _logger.LogError(
                    "任務 {TaskId} 執行失敗,停止 Pipeline 執行", task.Id);

                stopwatch.Stop();

                return new CoordinationResult
                {
                    Success = false,
                    TaskResults = taskResults,
                    ErrorMessage = taskResult.ErrorMessage,
                    Duration = stopwatch.Elapsed
                };
            }

            // 將輸出作為下一個任務的輸入
            pipelineData = taskResult.Output;
        }

        stopwatch.Stop();

        _logger.LogInformation(
            "Pipeline 執行完成 (耗時: {Duration}ms)",
            stopwatch.ElapsedMilliseconds);

        return new CoordinationResult
        {
            Success = true,
            TaskResults = taskResults,
            Duration = stopwatch.Elapsed
        };
    }

    private async Task<TaskResult> ExecuteSingleTaskAsync(
        WorkflowTask task,
        ExecutionContext context,
        CancellationToken cancellationToken)
    {
        var stopwatch = Stopwatch.StartNew();

        try
        {
            // 1. 獲取或創建 Agent
            var agent = await _agentFactory.GetOrCreateAgentAsync(
                task.AgentType, cancellationToken);

            // 2. 執行任務
            var input = JsonSerializer.Serialize(task.Input);
            var result = await agent.InvokeAsync(input, cancellationToken);

            stopwatch.Stop();

            _logger.LogInformation(
                "任務 {TaskId} 執行成功 (耗時: {Duration}ms)",
                task.Id, stopwatch.ElapsedMilliseconds);

            // 3. 解析輸出
            var output = JsonSerializer.Deserialize<Dictionary<string, object>>(
                result) ?? new Dictionary<string, object>();

            return new TaskResult
            {
                TaskId = task.Id,
                Success = true,
                Output = output,
                Duration = stopwatch.Elapsed
            };
        }
        catch (Exception ex)
        {
            stopwatch.Stop();

            _logger.LogError(ex,
                "任務 {TaskId} 執行失敗 (耗時: {Duration}ms)",
                task.Id, stopwatch.ElapsedMilliseconds);

            return new TaskResult
            {
                TaskId = task.Id,
                Success = false,
                ErrorMessage = ex.Message,
                Duration = stopwatch.Elapsed
            };
        }
    }
}
```

#### SemanticKernelExecutor + SemanticKernelStateManager

**簡化說明**: 這兩個組件的實作與 Sprint 8 的 `WorkflowExecutor` 和 `RedisStateStore` 類似,主要是實作對應的接口。

**關鍵實作細節 (Phase 2)**:
1. **Adapter Pattern**: 封裝 Semantic Kernel SDK
2. **Error Handling**: 統一錯誤處理與日誌記錄
3. **Performance**: 減少 Adapter Pattern 性能開銷
4. **Testability**: 可輕易 Mock Semantic Kernel Kernel

---

## Phase 3: 可替換性驗證 (2 SP)

### 3.1 目標與背景

**為什麼要驗證可替換性?**
- 確保 Framework Abstraction Layer 設計正確
- 驗證接口隔離性 (不依賴 Semantic Kernel)
- 為未來框架遷移提供信心

**技術挑戰**:
- Mock Framework 實作 (用於測試)
- Integration Tests 設計
- 接口合規性驗證

### 3.2 核心組件

#### MockWorkflowEngine

**檔案位置**: `tests/AIAgentPlatform.IntegrationTests/Mocks/MockWorkflowEngine.cs`

**設計決策**:
```yaml
職責:
  - 實作 IWorkflowEngine 接口 (Mock 版本)
  - 驗證接口隔離性
  - 用於 Integration Tests

實施策略:
  - 不依賴 Semantic Kernel
  - 簡化版本實作
  - 驗證接口設計

參考: ADR-011 Framework Abstraction Layer
```

**核心邏輯**:
```csharp
namespace AIAgentPlatform.IntegrationTests.Mocks;

/// <summary>
/// Mock 工作流引擎 (用於驗證可替換性)
/// </summary>
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

    public Task<WorkflowExecutionStatus> GetExecutionStatusAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        if (!_executions.TryGetValue(executionId, out var context))
        {
            throw new ExecutionNotFoundException(executionId);
        }

        var status = new WorkflowExecutionStatus
        {
            ExecutionId = executionId,
            State = context.State,
            TotalTasks = 1,
            CompletedTasks = 1,
            StartTime = context.StartTime,
            EndTime = context.EndTime
        };

        return Task.FromResult(status);
    }

    public Task PauseExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        if (_executions.TryGetValue(executionId, out var context))
        {
            context.State = WorkflowState.Paused;
        }

        return Task.CompletedTask;
    }

    public Task<WorkflowExecutionResult> ResumeExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        throw new NotImplementedException("Mock Resume not implemented");
    }

    public Task CancelExecutionAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        if (_executions.TryGetValue(executionId, out var context))
        {
            context.State = WorkflowState.Cancelled;
        }

        return Task.CompletedTask;
    }
}
```

#### ReplaceabilityTests

**檔案位置**: `tests/AIAgentPlatform.IntegrationTests/ReplaceabilityTests.cs`

**測試策略**:
```csharp
namespace AIAgentPlatform.IntegrationTests;

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

    [Fact]
    public async Task ShouldSupportTaskGeneratorReplacement()
    {
        // Arrange: 使用 MockTaskGenerator
        var mockGenerator = new MockTaskGenerator();

        var goal = "Test task generation";
        var workflow = new WorkflowDefinition
        {
            Id = Guid.NewGuid(),
            Name = "Test Workflow",
            Nodes = new List<WorkflowNode>
            {
                new WorkflowNode
                {
                    Id = Guid.NewGuid(),
                    Name = "Test Node",
                    Type = NodeType.Action
                }
            }
        };

        // Act: 生成任務計劃
        var taskPlan = await mockGenerator.GenerateTaskPlanAsync(
            goal, workflow);

        // Assert: 驗證任務計劃
        taskPlan.Should().NotBeNull();
        taskPlan.Tasks.Should().NotBeEmpty();
    }

    [Fact]
    public async Task ShouldSupportCoordinationLayerReplacement()
    {
        // Arrange: 使用 MockCoordinationLayer
        var mockCoordination = new MockCoordinationLayer();

        var tasks = new List<WorkflowTask>
        {
            new WorkflowTask
            {
                Id = Guid.NewGuid(),
                Name = "Task 1",
                AgentType = "MockAgent"
            }
        };

        var context = new ExecutionContext
        {
            ExecutionId = Guid.NewGuid(),
            WorkflowId = Guid.NewGuid()
        };

        // Act: 協調執行
        var result = await mockCoordination.CoordinateAsync(
            tasks, CoordinationStrategy.Sequential, context);

        // Assert: 驗證結果
        result.Should().NotBeNull();
        result.Success.Should().BeTrue();
    }
}
```

**關鍵實作細節 (Phase 3)**:
1. **Mock Implementations**: 實作 Mock 版本的所有核心接口
2. **Integration Tests**: 驗證接口可替換性
3. **Interface Compliance**: 確保接口設計正確
4. **Future Migration**: 為未來框架遷移提供信心

---

## Phase 4: Phase 1B 總結與移交 (1 SP)

### 4.1 目標與背景

**為什麼需要 Phase 1B 總結?**
- Sprint 9 是 Phase 1B 的最後一個 Sprint
- 需要完成 Phase 1B 的所有交付物
- 為 Phase 1C (Workflow Editor Backend) 做準備

**交付物**:
- Phase 1B 完成文檔
- Phase 1C 準備度評估
- 技術債務清單
- Sprint 10 移交 Checklist

### 4.2 核心組件

#### Phase 1B 完成文檔

**檔案位置**: `claudedocs/2-sprints/sprint-9/PHASE-1B-COMPLETION.md`

**內容大綱**:
```markdown
# Phase 1B 完成文檔

## 完成總結
- Sprint 7: Task Generator + Sequential (9 SP) ✅
- Sprint 8: Parallel + Messaging (13 SP) ✅
- Sprint 9: Framework Abstraction (13 SP) ✅
- 總計: 35 SP, 9 週, 100% 完成

## 核心成果
1. ✅ LLM 驅動任務規劃 (Task Generator)
2. ✅ Multi-Agent 協調 (Sequential + Parallel)
3. ✅ 工作流執行引擎 (Execution Engine)
4. ✅ Agent 間消息通訊 (Messaging)
5. ✅ 混合狀態管理 (Redis + PostgreSQL)
6. ✅ Framework 抽象層 (Abstraction Layer)

## 技術債務
1. Checkpoint/Resume 完整實現 (延後到 Phase 1E)
2. 完整錯誤處理與重試 (延後到 Phase 1D)
3. 可視化工作流編輯器 (Phase 2)
4. Workflow 模板系統 (Phase 2)

## Critical Path 解除
✅ Phase 1B 完成,解除後續 10 個 Sprint 阻斷
```

#### Phase 1C 準備度評估

**檔案位置**: `claudedocs/2-sprints/sprint-9/PHASE-1C-READINESS.md`

**內容大綱**:
```markdown
# Phase 1C 準備度評估

## Phase 1C 範圍
- Sprint 10-12: Workflow Editor Backend
- 3 個 Sprint, 9 週, 39 SP

## 前置依賴檢查
- ✅ Phase 1B 完成 (100%)
- ✅ Framework Abstraction Layer 完成
- ✅ Workflow Execution Engine 完成
- ✅ State Management 完成

## 技術準備度
- ✅ Workflow Definition Storage (準備就緒)
- ✅ Workflow Versioning (準備就緒)
- ✅ Workflow Editor Backend API (準備就緒)

## 風險評估
- 🟡 Workflow Versioning 複雜度 (中風險)
- 🟢 Workflow Definition Storage (低風險)
- 🟢 Workflow Editor Backend API (低風險)

## 移交 Checklist
- [ ] Phase 1B 完成文檔
- [ ] Sprint 10 準備就緒
- [ ] 技術債務清單
- [ ] 風險登記簿更新
```

#### Sprint 10 移交 Checklist

**檔案位置**: `claudedocs/2-sprints/sprint-9/SPRINT-10-HANDOVER.md`

**內容大綱**:
```markdown
# Sprint 10 移交 Checklist

## Sprint 9 完成驗收
- [ ] Framework Abstraction Layer 完成
- [ ] SemanticKernelAdapter 完成
- [ ] Replaceability Verification 完成
- [ ] 所有測試通過 (單元測試 + 整合測試)
- [ ] Code Review 完成
- [ ] 文檔更新完成

## Sprint 10 準備
- [ ] Sprint 10 計劃文檔準備
- [ ] Sprint 10 開發環境準備
- [ ] Sprint 10 技術調研完成
- [ ] Sprint 10 風險評估完成

## 移交資料
- [ ] Phase 1B 完成文檔
- [ ] Phase 1C 準備度評估
- [ ] 技術債務清單
- [ ] 風險登記簿
```

**關鍵實作細節 (Phase 4)**:
1. **Complete Documentation**: 完整的 Phase 1B 完成文檔
2. **Readiness Assessment**: Phase 1C 準備度評估
3. **Technical Debt**: 技術債務清單
4. **Handover**: Sprint 10 移交 Checklist

---

## 第三部分: 如何實施

### 詳細實作指南 - Core Interfaces

**Week 25 (2026-03-31 ~ 2026-04-06)**:

**Day 1-2: IWorkflowEngine + ITaskGenerator**
```yaml
任務:
  - 定義 IWorkflowEngine 介面
  - 定義 ITaskGenerator 介面
  - 定義相關 DTO (WorkflowExecutionResult, TaskPlan, etc.)

驗收標準:
  - 介面定義完整
  - XML 文檔註釋完整
  - 編譯通過
```

**Day 3-4: ICoordinationLayer + IExecutionEngine**
```yaml
任務:
  - 定義 ICoordinationLayer 介面
  - 定義 IExecutionEngine 介面
  - 定義相關 DTO (CoordinationResult, NodeExecutionResult, etc.)

驗收標準:
  - 介面定義完整
  - XML 文檔註釋完整
  - 編譯通過
```

**Day 5: IStateManager**
```yaml
任務:
  - 定義 IStateManager 介面
  - 定義相關 DTO (ExecutionContext, PagedResult, etc.)

驗收標準:
  - 介面定義完整
  - XML 文檔註釋完整
  - 編譯通過
```

### 詳細實作指南 - SemanticKernelAdapter

**Week 26 (2026-04-07 ~ 2026-04-13)**:

**Day 1-2: SemanticKernelWorkflowEngine**
```yaml
任務:
  - 實作 SemanticKernelWorkflowEngine
  - 整合 TaskGenerator + Coordination + Execution + State
  - 錯誤處理與日誌記錄

驗收標準:
  - 實作完整
  - 單元測試通過
  - 整合測試通過
```

**Day 3-4: SemanticKernelTaskGenerator**
```yaml
任務:
  - 實作 SemanticKernelTaskGenerator
  - LLM Prompt Engineering
  - 任務列表解析

驗收標準:
  - 實作完整
  - LLM 調用成功
  - 任務生成成功
```

**Day 5: SemanticKernelCoordination**
```yaml
任務:
  - 實作 SemanticKernelCoordination
  - Sequential/Parallel/Pipeline 策略
  - Agent 選擇與執行

驗收標準:
  - 實作完整
  - 協調策略正確
  - 測試通過
```

### 詳細實作指南 - Replaceability Verification

**Week 27 (2026-04-14 ~ 2026-04-20)**:

**Day 1-2: Mock Implementations**
```yaml
任務:
  - 實作 MockWorkflowEngine
  - 實作 MockTaskGenerator
  - 實作 MockCoordinationLayer

驗收標準:
  - Mock 實作完整
  - 不依賴 Semantic Kernel
  - 測試通過
```

**Day 3: Replaceability Tests**
```yaml
任務:
  - 撰寫 Replaceability Integration Tests
  - 驗證接口可替換性
  - 驗證接口合規性

驗收標準:
  - 測試覆蓋所有核心接口
  - 測試通過
  - 可替換性驗證成功
```

**Day 4-5: Phase 1B Handover**
```yaml
任務:
  - 撰寫 Phase 1B 完成文檔
  - 撰寫 Phase 1C 準備度評估
  - 撰寫 Sprint 10 移交 Checklist

驗收標準:
  - 文檔完整
  - 準備度評估完成
  - 移交 Checklist 完成
```

### 詳細實作指南 - Phase 1B Handover

**文檔撰寫指南**:
```yaml
Phase 1B 完成文檔:
  - 完成總結
  - 核心成果
  - 技術債務
  - Critical Path 解除

Phase 1C 準備度評估:
  - Phase 1C 範圍
  - 前置依賴檢查
  - 技術準備度
  - 風險評估

Sprint 10 移交 Checklist:
  - Sprint 9 完成驗收
  - Sprint 10 準備
  - 移交資料
```

---

## 第四部分: 編碼規範與最佳實踐

### 編碼規範

**C# 編碼標準**:
```yaml
命名規範:
  - 介面: IPascalCase (IWorkflowEngine)
  - 類別: PascalCase (SemanticKernelWorkflowEngine)
  - 方法: PascalCase (ExecuteWorkflowAsync)
  - 參數: camelCase (workflowId)
  - 私有欄位: _camelCase (_kernel)

Async 方法:
  - 後綴 Async (ExecuteWorkflowAsync)
  - 返回 Task 或 Task<T>
  - 接受 CancellationToken 參數

Null 處理:
  - 使用 Nullable Reference Types (C# 8+)
  - 使用 ? 標記可 null 參數
  - 使用 ?? 和 ?. 操作符

LINQ:
  - 優先使用 LINQ 而非迴圈
  - 使用 method syntax (避免 query syntax)

依賴注入:
  - 建構子注入 (避免 Service Locator)
  - 明確的依賴聲明
```

**參考文檔**:
- [C# Coding Standards](../../docs/development-standards/csharp-coding-standards.md)
- [.NET 9 Best Practices](../../docs/development-standards/dotnet9-best-practices.md)

### 測試策略

**單元測試**:
```yaml
覆蓋率目標: ≥85%

測試框架: xUnit + Moq + FluentAssertions

測試組織:
  - 每個類別對應一個測試類別
  - 使用 AAA 模式 (Arrange, Act, Assert)
  - 使用描述性測試名稱

測試範圍:
  - 所有核心接口的所有方法
  - SemanticKernelAdapter 所有公開方法
  - Mock 實作所有方法
```

**整合測試**:
```yaml
覆蓋率目標: ≥80%

測試場景:
  - Framework Abstraction Layer 可替換性
  - SemanticKernelAdapter 整合
  - 端到端工作流執行
  - 錯誤處理與 Retry
```

### 性能優化指南

**性能目標**:
```yaml
Framework Abstraction Layer:
  - Adapter Pattern 開銷: <5% (相比直接調用)
  - 介面調用延遲: <1ms (P95)

SemanticKernelAdapter:
  - LLM 調用延遲: <10 秒 (P95)
  - Task 生成延遲: <15 秒 (P95)
  - 協調執行延遲: <5 秒 (P95)
```

**優化策略**:
```yaml
Adapter Pattern:
  - 避免不必要的對象創建
  - 使用對象池 (Object Pool)
  - 減少反射調用

LLM 調用:
  - 使用 Prompt 緩存
  - 並行 LLM 調用 (可能)
  - 超時控制
```

---

## 第五部分: 質量保證

### 質量檢查清單

**代碼質量**:
- [ ] 所有 public API 有 XML 文檔註釋
- [ ] 所有異步方法使用 async/await
- [ ] 所有資源正確釋放 (using, Dispose)
- [ ] 無明顯的性能問題 (使用 profiler 驗證)
- [ ] Code complexity ≤15 (cyclomatic complexity)

**測試覆蓋率**:
- [ ] 單元測試覆蓋率 ≥85%
- [ ] 整合測試覆蓋核心流程
- [ ] 所有錯誤路徑有測試
- [ ] Replaceability Tests 通過

**文檔完整性**:
- [ ] README 更新
- [ ] API 文檔更新 (Swagger)
- [ ] 架構圖更新
- [ ] Phase 1B 完成文檔

### 驗收測試

**功能驗收**:
```yaml
Core Interfaces:
  - [ ] IWorkflowEngine 定義完整
  - [ ] ITaskGenerator 定義完整
  - [ ] ICoordinationLayer 定義完整
  - [ ] IExecutionEngine 定義完整
  - [ ] IStateManager 定義完整

SemanticKernelAdapter:
  - [ ] SemanticKernelWorkflowEngine 實作完整
  - [ ] SemanticKernelTaskGenerator 實作完整
  - [ ] SemanticKernelCoordination 實作完整
  - [ ] SemanticKernelExecutor 實作完整
  - [ ] SemanticKernelStateManager 實作完整

Replaceability:
  - [ ] MockWorkflowEngine 實作完整
  - [ ] Replaceability Tests 通過
  - [ ] 接口可替換性驗證成功
```

**性能驗收**:
```yaml
Framework Abstraction Layer:
  - [ ] Adapter Pattern 開銷 <5%
  - [ ] 介面調用延遲 <1ms (P95)

SemanticKernelAdapter:
  - [ ] LLM 調用延遲 <10 秒 (P95)
  - [ ] Task 生成延遲 <15 秒 (P95)
  - [ ] 協調執行延遲 <5 秒 (P95)
```

### Phase 1B 完成驗收

**🎉 Phase 1B 完成標準**:
```yaml
Sprint 7 驗收:
  - [✅] Task Generator (LLM 驅動) 完成
  - [✅] Sequential Coordination 完成
  - [✅] Workflow State Machine 完成

Sprint 8 驗收:
  - [✅] Parallel Coordination 完成
  - [✅] Workflow Execution Engine 完成
  - [✅] Agent Messaging 系統完成
  - [✅] State Management 強化完成

Sprint 9 驗收:
  - [ ] Framework Abstraction Layer 完成
  - [ ] SemanticKernelAdapter 完成
  - [ ] Replaceability Verification 完成
  - [ ] Phase 1B 完成文檔完成

Phase 1B 整體驗收:
  - [ ] 總 Story Points: 35 SP 完成
  - [ ] 總週期: 9 週完成
  - [ ] 完成度: 100%
  - [ ] Critical Path: 解除
  - [ ] 技術債務: 清單完成
  - [ ] Phase 1C 準備: 就緒
```

---

## 第六部分: 參考文檔

### 核心規劃文檔
1. [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md)
2. [SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md)
3. [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md)
4. [DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md)
5. [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md)

### User Stories
6. [us-4-multi-agent-orchestration.md](../../docs/user-stories/us-4-multi-agent-orchestration.md)
7. [US-7.1-Workflow-Definition.md](../../docs/user-stories/module-04-workflow-orchestration/US-7.1-Workflow-Definition.md)
8. [US-7.2-Workflow-Execution-Engine.md](../../docs/user-stories/module-04-workflow-orchestration/US-7.2-Workflow-Execution-Engine.md)

### 架構設計文檔
9. [ADR-011: Framework Abstraction Layer](../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md)
10. [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
11. [ADR-007: Internal Communication Strategy](../../docs/architecture/adr/ADR-007-internal-communication-strategy.md)
12. [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md)

### 技術實作文檔
13. [02-semantic-kernel-integration.md](../../docs/technical-implementation/01-backend-net9/02-semantic-kernel-integration.md)
14. [12-workflow-orchestration-implementation.md](../../docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md)

### 開發標準
15. [csharp-coding-standards.md](../../docs/development-standards/csharp-coding-standards.md)
16. [dotnet9-best-practices.md](../../docs/development-standards/dotnet9-best-practices.md)
17. [test-strategy.md](../../docs/testing/test-strategy.md)

### Sprint 文檔
18. [SPRINT-9-1-OVERVIEW.md](./SPRINT-9-1-OVERVIEW.md)
19. [SPRINT-9-3-CONTEXT.md](./SPRINT-9-3-CONTEXT.md)
20. [SPRINT-9-4-CHECKLIST.md](./SPRINT-9-4-CHECKLIST.md)

### Phase 1B 文檔
21. [SPRINT-7-1-OVERVIEW.md](../sprint-7/SPRINT-7-1-OVERVIEW.md)
22. [SPRINT-8-1-OVERVIEW.md](../sprint-8/SPRINT-8-1-OVERVIEW.md)
23. [PHASE-1B-COMPLETION.md](./PHASE-1B-COMPLETION.md)
24. [PHASE-1C-READINESS.md](./PHASE-1C-READINESS.md)

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 9 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-9/SPRINT-9-2-PLAN.md`

---

**相關文檔**:
- ⬆️ 上一層: [Sprint 9 Overview](./SPRINT-9-1-OVERVIEW.md)
- ⬅️ 上一步: [Sprint 9 Overview](./SPRINT-9-1-OVERVIEW.md)
- ➡️ 下一步: [Sprint 9 Context](./SPRINT-9-3-CONTEXT.md)
