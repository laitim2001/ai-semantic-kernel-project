# SPRINT-8-2-PLAN.md - Sprint 8 實施計劃：工作流執行引擎與消息系統

**版本**: v2.1
**Sprint 編號**: Sprint 8
**Sprint 週期**: Week 22-24 (3 週)
**Phase**: Phase 1B - 工作流引擎核心 (Workflow Engine Core)
**計劃日期**: 2026-03-10 ~ 2026-03-30
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 目錄 (Table of Contents)

### 第一部分: 本 Sprint 要建立什麼
1. [Sprint 8 實施範圍概述](#第一部分-本-sprint-要建立什麼)
2. [Phase 1: Parallel Coordination 完成](#phase-1-parallel-coordination-完成-3-5-sp)
3. [Phase 2: Workflow Execution Engine](#phase-2-workflow-execution-engine-基礎實現-5-sp)
4. [Phase 3: Agent Messaging 系統](#phase-3-agent-messaging-系統-5-sp)
5. [Phase 4: State Management 強化](#phase-4-state-management-強化與整合-1-sp)

### 第二部分: 如何實施
6. [Parallel Coordination 詳細實作指南](#詳細實作指南-parallel-coordination)
7. [Workflow Execution Engine 詳細實作指南](#詳細實作指南-workflow-execution-engine)
8. [Agent Messaging 詳細實作指南](#詳細實作指南-agent-messaging)
9. [State Management 詳細實作指南](#詳細實作指南-state-management)

### 第三部分: 編碼規範與最佳實踐
10. [編碼規範](#第三部分-編碼規範與最佳實踐)
11. [測試策略](#測試策略)
12. [性能優化指南](#性能優化指南)

### 第四部分: 質量保證
13. [質量檢查清單](#第四部分-質量保證)
14. [驗收測試](#驗收測試)

### 第五部分: 參考文檔
15. [完整參考文檔列表](#第五部分-參考文檔)

---

## 第一部分: 本 Sprint 要建立什麼

### Sprint 8 實施範圍概述

**核心交付物** (13 SP):
```yaml
Phase 1: Parallel Coordination 完成 (3-5 SP)
  目標: 完成 Parallel 協調模式的所有細節與性能優化
  組件:
    - Parallel Coordination 完整實現
    - Race condition 處理
    - Result aggregation
    - 性能優化 (並發度控制)

Phase 2: Workflow Execution Engine (5 SP)
  目標: 建立工作流執行引擎核心能力
  組件:
    - IWorkflowExecutor 介面
    - WorkflowExecutor 實現
    - ExecutionContext 管理
    - 錯誤處理與 Retry
    - 生命週期管理

Phase 3: Agent Messaging 系統 (5 SP)
  目標: 建立 Agent 間消息通訊機制
  組件:
    - IMessageBus 介面
    - InMemoryMessageBus 實現
    - Agent 間消息協議
    - Event-driven communication
    - Message delivery 保證

Phase 4: State Management 強化 (1 SP)
  目標: 強化工作流執行狀態管理
  組件:
    - Execution State 持久化
    - State Recovery 機制
    - Domain Events
    - MediatR Integration
```

**架構依賴關係**:
```
Sprint 7 成果 (前置依賴):
├─ Task Generator (LLM 驅動任務規劃) ✅
├─ Sequential Coordination ✅
├─ Parallel Coordination (基礎版) ✅
└─ Workflow API (CRUD) ✅

Sprint 8 新建 (本 Sprint):
├─ Parallel Coordination (完整版) 🔥
├─ Workflow Execution Engine 🔥
├─ Agent Messaging 系統 🔥
└─ State Management 強化 🔥

Sprint 9 依賴 (後續):
├─ Framework Abstraction Layer
└─ SemanticKernelAdapter
```

---

## Phase 1: Parallel Coordination 完成 (3-5 SP)

### 1.1 目標與背景

**為什麼要完成 Parallel Coordination?**
- Sprint 7 已實現基礎 Parallel 執行
- Sprint 8 需完成所有細節 (race condition, result aggregation)
- 為 Workflow Execution Engine 提供穩定的並行執行能力

**技術挑戰**:
- Race condition 正確處理
- Task cancellation propagation
- Result aggregation 與錯誤聚合
- 性能優化 (並發度控制)

**參考文檔**:
- [US 7.4: Multi-Agent Coordination](../../docs/user-stories/module-04-workflow-orchestration/US-7.4-Multi-Agent-Coordination.md)
- [Workflow Orchestration Implementation](../../docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md)
- [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) Lines 250-313

### 1.2 核心組件

#### ParallelCoordination 完整實現

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Services/ParallelCoordination.cs`

**設計決策**:
```yaml
實施策略:
  - 使用 Task.WhenAll 實現並行執行
  - 使用 CancellationTokenSource 實現取消傳播
  - 使用 SemaphoreSlim 實現並發度控制
  - 使用 ConcurrentBag 實現結果聚合

錯誤處理策略:
  - All-or-Nothing: 任一失敗則全部失敗 (預設)
  - Best-Effort: 部分成功也算成功 (選用)
  - 錯誤聚合: 收集所有錯誤並報告

參考: ADR-009 Workflow Orchestration Strategy
```

**核心邏輯**:
```csharp
public class ParallelCoordination : ICoordinationStrategy
{
    private readonly ILogger<ParallelCoordination> _logger;
    private readonly int _maxDegreeOfParallelism;

    public ParallelCoordination(
        ILogger<ParallelCoordination> logger,
        int maxDegreeOfParallelism = 10)
    {
        _logger = logger;
        _maxDegreeOfParallelism = maxDegreeOfParallelism;
    }

    public async Task<CoordinationResult> ExecuteAsync(
        List<WorkflowTask> tasks,
        ExecutionContext context,
        CancellationToken cancellationToken)
    {
        _logger.LogInformation(
            "開始並行執行 {TaskCount} 個任務 (MaxDegreeOfParallelism: {MaxDOP})",
            tasks.Count, _maxDegreeOfParallelism);

        // 1. 創建並發控制 Semaphore
        using var semaphore = new SemaphoreSlim(_maxDegreeOfParallelism);

        // 2. 創建取消令牌源 (用於提前取消)
        using var cts = CancellationTokenSource.CreateLinkedTokenSource(cancellationToken);

        // 3. 創建結果集合 (線程安全)
        var results = new ConcurrentBag<TaskResult>();
        var errors = new ConcurrentBag<Exception>();

        // 4. 並行執行所有任務
        var executionTasks = tasks.Select(async task =>
        {
            await semaphore.WaitAsync(cts.Token);
            try
            {
                var result = await ExecuteSingleTaskAsync(
                    task, context, cts.Token);

                results.Add(result);

                // 如果失敗且為 All-or-Nothing 策略,觸發取消
                if (!result.Success && !context.IsBestEffort)
                {
                    _logger.LogWarning(
                        "任務 {TaskId} 失敗,觸發取消其他任務", task.Id);
                    cts.Cancel();
                }
            }
            catch (OperationCanceledException)
            {
                _logger.LogWarning("任務 {TaskId} 被取消", task.Id);
                // 取消不視為錯誤
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "任務 {TaskId} 執行異常", task.Id);
                errors.Add(ex);

                // All-or-Nothing: 任一異常則取消其他任務
                if (!context.IsBestEffort)
                    cts.Cancel();
            }
            finally
            {
                semaphore.Release();
            }
        });

        // 5. 等待所有任務完成 (或被取消)
        try
        {
            await Task.WhenAll(executionTasks);
        }
        catch (OperationCanceledException)
        {
            _logger.LogWarning("並行執行被取消");
        }

        // 6. 聚合結果
        return AggregateResults(results, errors, context);
    }

    private async Task<TaskResult> ExecuteSingleTaskAsync(
        WorkflowTask task,
        ExecutionContext context,
        CancellationToken cancellationToken)
    {
        var stopwatch = Stopwatch.StartNew();

        try
        {
            // 調用 Agent 執行任務
            var agent = context.GetAgent(task.AgentId);
            var result = await agent.ExecuteAsync(
                task.Input, cancellationToken);

            stopwatch.Stop();

            _logger.LogInformation(
                "任務 {TaskId} 執行成功 (耗時: {Duration}ms)",
                task.Id, stopwatch.ElapsedMilliseconds);

            return TaskResult.Success(result, stopwatch.Elapsed);
        }
        catch (Exception ex)
        {
            stopwatch.Stop();

            _logger.LogError(ex,
                "任務 {TaskId} 執行失敗 (耗時: {Duration}ms)",
                task.Id, stopwatch.ElapsedMilliseconds);

            return TaskResult.Failure(ex, stopwatch.Elapsed);
        }
    }

    private CoordinationResult AggregateResults(
        ConcurrentBag<TaskResult> results,
        ConcurrentBag<Exception> errors,
        ExecutionContext context)
    {
        var successCount = results.Count(r => r.Success);
        var failureCount = results.Count - successCount;

        _logger.LogInformation(
            "並行執行完成: 成功 {Success}, 失敗 {Failure}",
            successCount, failureCount);

        // All-or-Nothing: 任一失敗則整體失敗
        if (!context.IsBestEffort && failureCount > 0)
        {
            var aggregateException = new AggregateException(
                "並行執行失敗", errors);

            return CoordinationResult.Failure(aggregateException);
        }

        // Best-Effort: 部分成功也算成功
        return CoordinationResult.Success(results.ToList());
    }
}
```

**關鍵實作細節**:
1. **並發度控制**: 使用 `SemaphoreSlim` 限制同時執行的任務數 (避免資源耗盡)
2. **取消傳播**: 使用 `CancellationTokenSource` 實現提前取消
3. **結果聚合**: 使用 `ConcurrentBag` 線程安全地收集結果
4. **錯誤策略**: 支援 All-or-Nothing 和 Best-Effort 兩種策略
5. **性能監控**: 記錄每個任務的執行時間

---

## Phase 2: Workflow Execution Engine 基礎實現 (5 SP)

### 2.1 目標與背景

**為什麼要建立 Execution Engine?**
- 整合 Task Generator + Coordination Layer + Messaging
- 提供完整的工作流執行生命週期管理
- 為工作流編輯器提供執行引擎 API

**技術挑戰**:
- 執行上下文管理 (變數作用域、狀態持久化)
- 錯誤處理與 Retry 機制
- Checkpoint/Resume 實現
- 生命週期狀態轉換

**參考文檔**:
- [US 7.2: Workflow Execution Engine](../../docs/user-stories/module-04-workflow-orchestration/US-7.2-Workflow-Execution-Engine.md)
- [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) Lines 316-411

### 2.2 核心組件

#### IWorkflowExecutor 介面

**檔案位置**: `src/AIAgentPlatform.Application/Interfaces/IWorkflowExecutor.cs`

```csharp
namespace AIAgentPlatform.Application.Interfaces;

/// <summary>
/// 工作流執行器介面
/// </summary>
public interface IWorkflowExecutor
{
    /// <summary>
    /// 執行工作流
    /// </summary>
    /// <param name="workflowId">工作流 ID</param>
    /// <param name="userInput">使用者輸入</param>
    /// <param name="parameters">執行參數</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>執行結果</returns>
    Task<ExecutionResult> ExecuteAsync(
        Guid workflowId,
        string userInput,
        Dictionary<string, object>? parameters = null,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 恢復執行 (從 Checkpoint)
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <param name="cancellationToken">取消令牌</param>
    /// <returns>執行結果</returns>
    Task<ExecutionResult> ResumeAsync(
        Guid executionId,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 暫停執行
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    Task PauseAsync(Guid executionId);

    /// <summary>
    /// 取消執行
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    Task CancelAsync(Guid executionId);

    /// <summary>
    /// 獲取執行狀態
    /// </summary>
    /// <param name="executionId">執行 ID</param>
    /// <returns>執行狀態</returns>
    Task<ExecutionStatus> GetStatusAsync(Guid executionId);
}

/// <summary>
/// 執行結果
/// </summary>
public class ExecutionResult
{
    public Guid ExecutionId { get; set; }
    public bool Success { get; set; }
    public Dictionary<string, object> Output { get; set; } = new();
    public string? ErrorMessage { get; set; }
    public TimeSpan Duration { get; set; }
}

/// <summary>
/// 執行狀態
/// </summary>
public class ExecutionStatus
{
    public Guid ExecutionId { get; set; }
    public WorkflowExecutionState State { get; set; }
    public int TotalTasks { get; set; }
    public int CompletedTasks { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime? EndTime { get; set; }
}

public enum WorkflowExecutionState
{
    Pending,
    Running,
    Paused,
    Completed,
    Failed,
    Cancelled
}
```

#### WorkflowExecutor 實現

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Services/WorkflowExecutor.cs`

```csharp
public class WorkflowExecutor : IWorkflowExecutor
{
    private readonly IWorkflowRepository _workflowRepository;
    private readonly ITaskGenerator _taskGenerator;
    private readonly ICoordinationStrategyFactory _strategyFactory;
    private readonly IStateStore _stateStore;
    private readonly IMessageBus _messageBus;
    private readonly ILogger<WorkflowExecutor> _logger;

    public WorkflowExecutor(
        IWorkflowRepository workflowRepository,
        ITaskGenerator taskGenerator,
        ICoordinationStrategyFactory strategyFactory,
        IStateStore stateStore,
        IMessageBus messageBus,
        ILogger<WorkflowExecutor> logger)
    {
        _workflowRepository = workflowRepository;
        _taskGenerator = taskGenerator;
        _strategyFactory = strategyFactory;
        _stateStore = stateStore;
        _messageBus = messageBus;
        _logger = logger;
    }

    public async Task<ExecutionResult> ExecuteAsync(
        Guid workflowId,
        string userInput,
        Dictionary<string, object>? parameters = null,
        CancellationToken cancellationToken = default)
    {
        var stopwatch = Stopwatch.StartNew();
        var executionId = Guid.NewGuid();

        try
        {
            _logger.LogInformation(
                "開始執行工作流 {WorkflowId} (ExecutionId: {ExecutionId})",
                workflowId, executionId);

            // 1. 載入工作流定義
            var workflow = await _workflowRepository.GetByIdAsync(
                workflowId, cancellationToken);

            if (workflow == null)
                throw new WorkflowNotFoundException(workflowId);

            // 2. 創建執行上下文
            var context = new ExecutionContext
            {
                ExecutionId = executionId,
                WorkflowId = workflowId,
                UserInput = userInput,
                Parameters = parameters ?? new(),
                StartTime = DateTime.UtcNow,
                State = WorkflowExecutionState.Running,
                Variables = new Dictionary<string, object>()
            };

            // 發布執行開始事件
            await _messageBus.PublishAsync(
                new WorkflowExecutionStartedEvent(executionId, workflowId));

            // 3. 生成任務計劃 (LLM 驅動)
            var tasks = await _taskGenerator.GenerateTasksAsync(
                userInput, workflow, cancellationToken);

            context.TotalTasks = tasks.Count;

            _logger.LogInformation(
                "生成 {TaskCount} 個任務", tasks.Count);

            // 4. 執行工作流節點
            foreach (var node in workflow.Nodes.OrderBy(n => n.Order))
            {
                if (cancellationToken.IsCancellationRequested)
                {
                    _logger.LogWarning("工作流執行被取消");
                    context.State = WorkflowExecutionState.Cancelled;
                    break;
                }

                // 4.1 獲取該節點的任務
                var nodeTasks = tasks
                    .Where(t => t.NodeId == node.Id)
                    .ToList();

                if (!nodeTasks.Any())
                {
                    _logger.LogWarning(
                        "節點 {NodeId} 沒有對應的任務", node.Id);
                    continue;
                }

                // 4.2 選擇協調策略 (Sequential/Parallel)
                var strategy = _strategyFactory.Create(
                    node.CoordinationType);

                _logger.LogInformation(
                    "執行節點 {NodeId} (策略: {Strategy}, 任務數: {TaskCount})",
                    node.Id, node.CoordinationType, nodeTasks.Count);

                // 4.3 執行節點任務
                var result = await strategy.ExecuteAsync(
                    nodeTasks, context, cancellationToken);

                // 4.4 更新上下文
                context.CompletedTasks += nodeTasks.Count;
                context.LastExecutedNodeId = node.Id;

                // 4.5 Checkpoint (每個節點完成後)
                await _stateStore.SaveCheckpointAsync(context);

                // 4.6 發布任務完成事件
                await _messageBus.PublishAsync(
                    new TaskCompletedEvent(node.Id, result));

                // 4.7 錯誤處理
                if (!result.Success)
                {
                    if (node.ContinueOnError)
                    {
                        _logger.LogWarning(
                            "節點 {NodeId} 執行失敗,但設定為繼續執行",
                            node.Id);
                    }
                    else
                    {
                        _logger.LogError(
                            "節點 {NodeId} 執行失敗,停止工作流執行",
                            node.Id);

                        context.State = WorkflowExecutionState.Failed;
                        context.ErrorMessage = result.ErrorMessage;

                        stopwatch.Stop();

                        // 發布執行失敗事件
                        await _messageBus.PublishAsync(
                            new WorkflowExecutionFailedEvent(
                                executionId, result.ErrorMessage));

                        return ExecutionResult.Failure(
                            executionId, result.ErrorMessage, stopwatch.Elapsed);
                    }
                }

                // 4.8 更新節點輸出到上下文變數
                UpdateContextVariables(context, node, result);
            }

            // 5. 執行完成
            stopwatch.Stop();
            context.State = WorkflowExecutionState.Completed;
            context.EndTime = DateTime.UtcNow;
            context.Duration = stopwatch.Elapsed;

            // 最終 Checkpoint
            await _stateStore.SaveCheckpointAsync(context);

            // 發布執行完成事件
            await _messageBus.PublishAsync(
                new WorkflowExecutionCompletedEvent(executionId));

            _logger.LogInformation(
                "工作流執行完成 (ExecutionId: {ExecutionId}, 耗時: {Duration}ms)",
                executionId, stopwatch.ElapsedMilliseconds);

            return ExecutionResult.Success(
                executionId, context.Variables, stopwatch.Elapsed);
        }
        catch (Exception ex)
        {
            stopwatch.Stop();

            _logger.LogError(ex,
                "工作流執行異常 (ExecutionId: {ExecutionId})",
                executionId);

            // 發布執行失敗事件
            await _messageBus.PublishAsync(
                new WorkflowExecutionFailedEvent(executionId, ex.Message));

            return ExecutionResult.Failure(
                executionId, ex.Message, stopwatch.Elapsed);
        }
    }

    public async Task<ExecutionResult> ResumeAsync(
        Guid executionId,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation(
            "恢復工作流執行 (ExecutionId: {ExecutionId})", executionId);

        // 1. 從 Checkpoint 恢復上下文
        var context = await _stateStore.LoadCheckpointAsync(executionId);

        if (context == null)
            throw new CheckpointNotFoundException(executionId);

        // 2. 載入工作流定義
        var workflow = await _workflowRepository.GetByIdAsync(
            context.WorkflowId, cancellationToken);

        // 3. 找到上次執行的節點
        var lastNodeIndex = workflow.Nodes
            .OrderBy(n => n.Order)
            .ToList()
            .FindIndex(n => n.Id == context.LastExecutedNodeId);

        // 4. 從下一個節點繼續執行
        // (實作邏輯與 ExecuteAsync 類似,從 lastNodeIndex + 1 開始)

        // 簡化版本: 暫時不實現完整的 Resume 邏輯
        throw new NotImplementedException(
            "Resume 功能將在 Phase 1E (Sprint 15) 完整實現");
    }

    public async Task PauseAsync(Guid executionId)
    {
        // 基礎版本: 僅更新狀態
        var context = await _stateStore.LoadCheckpointAsync(executionId);
        if (context != null)
        {
            context.State = WorkflowExecutionState.Paused;
            await _stateStore.SaveCheckpointAsync(context);
        }
    }

    public async Task CancelAsync(Guid executionId)
    {
        // 基礎版本: 僅更新狀態
        var context = await _stateStore.LoadCheckpointAsync(executionId);
        if (context != null)
        {
            context.State = WorkflowExecutionState.Cancelled;
            await _stateStore.SaveCheckpointAsync(context);
        }
    }

    public async Task<ExecutionStatus> GetStatusAsync(Guid executionId)
    {
        var context = await _stateStore.LoadCheckpointAsync(executionId);
        if (context == null)
            return null;

        return new ExecutionStatus
        {
            ExecutionId = executionId,
            State = context.State,
            TotalTasks = context.TotalTasks,
            CompletedTasks = context.CompletedTasks,
            StartTime = context.StartTime,
            EndTime = context.EndTime
        };
    }

    private void UpdateContextVariables(
        ExecutionContext context,
        WorkflowNode node,
        CoordinationResult result)
    {
        // 將節點輸出存儲到上下文變數
        // 使用 node.OutputVariable 作為變數名稱
        if (!string.IsNullOrEmpty(node.OutputVariable) && result.Success)
        {
            context.Variables[node.OutputVariable] = result.Output;
        }
    }
}
```

**關鍵實作細節**:
1. **執行生命週期**: Pending → Running → Completed/Failed/Cancelled
2. **Checkpoint 機制**: 每個節點完成後保存 Checkpoint
3. **Event Publishing**: 發布執行開始、任務完成、執行完成/失敗事件
4. **錯誤處理**: 支援 ContinueOnError 設定
5. **變數管理**: 節點輸出存儲到上下文變數,供後續節點使用

---

## Phase 3: Agent Messaging 系統 (5 SP)

### 3.1 目標與背景

**為什麼要建立 Messaging 系統?**
- Agent 之間需要異步消息通訊
- 支援 Event-driven architecture
- 為未來分布式執行做準備

**技術挑戰**:
- Message delivery 保證 (at-least-once)
- Message ordering
- Topic-based routing
- 性能優化 (低延遲)

**參考文檔**:
- [ADR-007: Internal Communication Strategy](../../docs/architecture/adr/ADR-007-internal-communication-strategy.md)
- [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) Lines 465-499

### 3.2 核心組件

#### IMessageBus 介面

**檔案位置**: `src/AIAgentPlatform.Application/Interfaces/IMessageBus.cs`

```csharp
namespace AIAgentPlatform.Application.Interfaces;

/// <summary>
/// 消息總線介面
/// </summary>
public interface IMessageBus
{
    /// <summary>
    /// 發布消息
    /// </summary>
    Task PublishAsync<T>(T message) where T : IMessage;

    /// <summary>
    /// 訂閱消息
    /// </summary>
    Task SubscribeAsync<T>(Func<T, Task> handler) where T : IMessage;

    /// <summary>
    /// 取消訂閱
    /// </summary>
    Task UnsubscribeAsync<T>(Func<T, Task> handler) where T : IMessage;
}

/// <summary>
/// 消息基礎介面
/// </summary>
public interface IMessage
{
    /// <summary>
    /// 消息 ID
    /// </summary>
    Guid MessageId { get; }

    /// <summary>
    /// 發送者 ID
    /// </summary>
    string SenderId { get; }

    /// <summary>
    /// 接收者 ID (可選,用於點對點消息)
    /// </summary>
    string? ReceiverId { get; }

    /// <summary>
    /// 時間戳
    /// </summary>
    DateTime Timestamp { get; }

    /// <summary>
    /// 關聯 ID (用於追蹤消息鏈)
    /// </summary>
    string? CorrelationId { get; }
}
```

#### Agent 間消息協議

**檔案位置**: `src/AIAgentPlatform.Domain/Messages/`

```csharp
// WorkflowExecutionStartedEvent.cs
public class WorkflowExecutionStartedEvent : IMessage
{
    public Guid MessageId { get; } = Guid.NewGuid();
    public string SenderId { get; } = "WorkflowExecutor";
    public string? ReceiverId { get; }
    public DateTime Timestamp { get; } = DateTime.UtcNow;
    public string? CorrelationId { get; }

    public Guid ExecutionId { get; }
    public Guid WorkflowId { get; }

    public WorkflowExecutionStartedEvent(Guid executionId, Guid workflowId)
    {
        ExecutionId = executionId;
        WorkflowId = workflowId;
        CorrelationId = executionId.ToString();
    }
}

// TaskCompletedEvent.cs
public class TaskCompletedEvent : IMessage
{
    public Guid MessageId { get; } = Guid.NewGuid();
    public string SenderId { get; }
    public string? ReceiverId { get; }
    public DateTime Timestamp { get; } = DateTime.UtcNow;
    public string? CorrelationId { get; }

    public Guid NodeId { get; }
    public CoordinationResult Result { get; }

    public TaskCompletedEvent(Guid nodeId, CoordinationResult result)
    {
        NodeId = nodeId;
        Result = result;
        SenderId = "CoordinationStrategy";
    }
}

// WorkflowExecutionCompletedEvent.cs
public class WorkflowExecutionCompletedEvent : IMessage
{
    public Guid MessageId { get; } = Guid.NewGuid();
    public string SenderId { get; } = "WorkflowExecutor";
    public string? ReceiverId { get; }
    public DateTime Timestamp { get; } = DateTime.UtcNow;
    public string? CorrelationId { get; }

    public Guid ExecutionId { get; }

    public WorkflowExecutionCompletedEvent(Guid executionId)
    {
        ExecutionId = executionId;
        CorrelationId = executionId.ToString();
    }
}

// WorkflowExecutionFailedEvent.cs
public class WorkflowExecutionFailedEvent : IMessage
{
    public Guid MessageId { get; } = Guid.NewGuid();
    public string SenderId { get; } = "WorkflowExecutor";
    public string? ReceiverId { get; }
    public DateTime Timestamp { get; } = DateTime.UtcNow;
    public string? CorrelationId { get; }

    public Guid ExecutionId { get; }
    public string ErrorMessage { get; }

    public WorkflowExecutionFailedEvent(
        Guid executionId, string errorMessage)
    {
        ExecutionId = executionId;
        ErrorMessage = errorMessage;
        CorrelationId = executionId.ToString();
    }
}
```

#### InMemoryMessageBus 實現

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Services/InMemoryMessageBus.cs`

```csharp
public class InMemoryMessageBus : IMessageBus
{
    private readonly ConcurrentDictionary<Type, List<Delegate>> _handlers;
    private readonly ILogger<InMemoryMessageBus> _logger;

    public InMemoryMessageBus(ILogger<InMemoryMessageBus> logger)
    {
        _handlers = new ConcurrentDictionary<Type, List<Delegate>>();
        _logger = logger;
    }

    public async Task PublishAsync<T>(T message) where T : IMessage
    {
        var messageType = typeof(T);

        _logger.LogDebug(
            "發布消息: {MessageType} (MessageId: {MessageId}, SenderId: {SenderId})",
            messageType.Name, message.MessageId, message.SenderId);

        if (_handlers.TryGetValue(messageType, out var handlers))
        {
            // 複製 handlers 列表 (避免在迭代時修改)
            var handlersCopy = handlers.ToList();

            _logger.LogDebug(
                "找到 {HandlerCount} 個訂閱者", handlersCopy.Count);

            // 並行調用所有 handlers
            var tasks = handlersCopy
                .Cast<Func<T, Task>>()
                .Select(handler => InvokeHandlerAsync(handler, message));

            await Task.WhenAll(tasks);
        }
        else
        {
            _logger.LogDebug(
                "消息 {MessageType} 沒有訂閱者", messageType.Name);
        }
    }

    public Task SubscribeAsync<T>(Func<T, Task> handler) where T : IMessage
    {
        var messageType = typeof(T);

        _handlers.AddOrUpdate(
            messageType,
            _ =>
            {
                _logger.LogInformation(
                    "創建新的訂閱者列表: {MessageType}", messageType.Name);
                return new List<Delegate> { handler };
            },
            (_, existingHandlers) =>
            {
                lock (existingHandlers)
                {
                    existingHandlers.Add(handler);
                    _logger.LogInformation(
                        "添加訂閱者: {MessageType} (總數: {Count})",
                        messageType.Name, existingHandlers.Count);
                }
                return existingHandlers;
            });

        return Task.CompletedTask;
    }

    public Task UnsubscribeAsync<T>(Func<T, Task> handler) where T : IMessage
    {
        var messageType = typeof(T);

        if (_handlers.TryGetValue(messageType, out var handlers))
        {
            lock (handlers)
            {
                handlers.Remove(handler);
                _logger.LogInformation(
                    "移除訂閱者: {MessageType} (剩餘: {Count})",
                    messageType.Name, handlers.Count);
            }
        }

        return Task.CompletedTask;
    }

    private async Task InvokeHandlerAsync<T>(
        Func<T, Task> handler, T message) where T : IMessage
    {
        try
        {
            await handler(message);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex,
                "Handler 執行異常: {MessageType} (MessageId: {MessageId})",
                typeof(T).Name, message.MessageId);

            // 不拋出異常,避免影響其他 handlers
        }
    }
}
```

**關鍵實作細節**:
1. **Topic-based Routing**: 使用 Type 作為 Topic
2. **At-least-once Delivery**: 確保消息至少被傳遞一次
3. **Async Delivery**: 並行調用所有 handlers (提高性能)
4. **Error Isolation**: Handler 異常不影響其他 handlers
5. **Thread Safety**: 使用 `ConcurrentDictionary` 和 `lock`

---

## Phase 4: State Management 強化與整合 (1 SP)

### 4.1 目標與背景

**為什麼要強化 State Management?**
- 執行狀態需要持久化 (Redis + PostgreSQL)
- 支援 Checkpoint/Resume
- 支援執行歷史查詢

**技術挑戰**:
- 混合存儲策略 (Redis + PostgreSQL)
- 狀態一致性保證
- 性能優化 (減少寫入開銷)

**參考文檔**:
- [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
- [Redis Configuration](../../docs/technical-implementation/03-data-layer/redis-configuration.md)

### 4.2 核心組件

#### IStateStore 介面

**檔案位置**: `src/AIAgentPlatform.Application/Interfaces/IStateStore.cs`

```csharp
namespace AIAgentPlatform.Application.Interfaces;

/// <summary>
/// 狀態存儲介面
/// </summary>
public interface IStateStore
{
    /// <summary>
    /// 保存 Checkpoint
    /// </summary>
    Task SaveCheckpointAsync(ExecutionContext context);

    /// <summary>
    /// 載入 Checkpoint
    /// </summary>
    Task<ExecutionContext?> LoadCheckpointAsync(Guid executionId);

    /// <summary>
    /// 刪除 Checkpoint
    /// </summary>
    Task DeleteCheckpointAsync(Guid executionId);

    /// <summary>
    /// 獲取所有執行記錄 (分頁)
    /// </summary>
    Task<PagedResult<ExecutionContext>> GetExecutionHistoryAsync(
        int pageIndex, int pageSize);
}
```

#### RedisStateStore 實現

**檔案位置**: `src/AIAgentPlatform.Infrastructure/Services/RedisStateStore.cs`

```csharp
public class RedisStateStore : IStateStore
{
    private readonly IConnectionMultiplexer _redis;
    private readonly IWorkflowExecutionRepository _executionRepository;
    private readonly ILogger<RedisStateStore> _logger;

    private const string CheckpointKeyPrefix = "checkpoint:";
    private static readonly TimeSpan CheckpointTTL = TimeSpan.FromHours(24);

    public RedisStateStore(
        IConnectionMultiplexer redis,
        IWorkflowExecutionRepository executionRepository,
        ILogger<RedisStateStore> logger)
    {
        _redis = redis;
        _executionRepository = executionRepository;
        _logger = logger;
    }

    public async Task SaveCheckpointAsync(ExecutionContext context)
    {
        var db = _redis.GetDatabase();
        var key = $"{CheckpointKeyPrefix}{context.ExecutionId}";
        var value = JsonSerializer.Serialize(context);

        // 1. 保存到 Redis (快取)
        await db.StringSetAsync(key, value, CheckpointTTL);

        _logger.LogDebug(
            "Checkpoint 已保存到 Redis (ExecutionId: {ExecutionId})",
            context.ExecutionId);

        // 2. 保存到 PostgreSQL (持久化)
        await _executionRepository.UpsertAsync(new WorkflowExecution
        {
            Id = context.ExecutionId,
            WorkflowId = context.WorkflowId,
            State = context.State,
            TotalTasks = context.TotalTasks,
            CompletedTasks = context.CompletedTasks,
            StartTime = context.StartTime,
            EndTime = context.EndTime,
            Duration = context.Duration,
            ErrorMessage = context.ErrorMessage,
            ContextJson = value
        });

        _logger.LogDebug(
            "Checkpoint 已保存到 PostgreSQL (ExecutionId: {ExecutionId})",
            context.ExecutionId);
    }

    public async Task<ExecutionContext?> LoadCheckpointAsync(
        Guid executionId)
    {
        var db = _redis.GetDatabase();
        var key = $"{CheckpointKeyPrefix}{executionId}";

        // 1. 嘗試從 Redis 載入 (快速路徑)
        var value = await db.StringGetAsync(key);

        if (!value.IsNullOrEmpty)
        {
            _logger.LogDebug(
                "從 Redis 載入 Checkpoint (ExecutionId: {ExecutionId})",
                executionId);

            return JsonSerializer.Deserialize<ExecutionContext>(value);
        }

        // 2. Redis 沒有,從 PostgreSQL 載入 (慢速路徑)
        _logger.LogDebug(
            "Redis 沒有 Checkpoint,從 PostgreSQL 載入 (ExecutionId: {ExecutionId})",
            executionId);

        var execution = await _executionRepository.GetByIdAsync(executionId);

        if (execution == null)
            return null;

        var context = JsonSerializer.Deserialize<ExecutionContext>(
            execution.ContextJson);

        // 3. 重新放入 Redis 快取
        if (context != null)
        {
            await db.StringSetAsync(
                key,
                execution.ContextJson,
                CheckpointTTL);
        }

        return context;
    }

    public async Task DeleteCheckpointAsync(Guid executionId)
    {
        var db = _redis.GetDatabase();
        var key = $"{CheckpointKeyPrefix}{executionId}";

        // 1. 從 Redis 刪除
        await db.KeyDeleteAsync(key);

        // 2. PostgreSQL 保留 (用於歷史查詢)
        _logger.LogDebug(
            "Checkpoint 已從 Redis 刪除 (ExecutionId: {ExecutionId})",
            executionId);
    }

    public async Task<PagedResult<ExecutionContext>> GetExecutionHistoryAsync(
        int pageIndex, int pageSize)
    {
        // 從 PostgreSQL 查詢歷史記錄
        var executions = await _executionRepository.GetPagedAsync(
            pageIndex, pageSize);

        var contexts = executions.Items
            .Select(e => JsonSerializer.Deserialize<ExecutionContext>(
                e.ContextJson))
            .Where(c => c != null)
            .ToList();

        return new PagedResult<ExecutionContext>
        {
            Items = contexts,
            TotalCount = executions.TotalCount,
            PageIndex = pageIndex,
            PageSize = pageSize
        };
    }
}
```

**關鍵實作細節**:
1. **混合存儲**: Redis (快取) + PostgreSQL (持久化)
2. **兩層讀取**: Redis 優先,不存在則從 PostgreSQL 讀取並回填 Redis
3. **TTL 機制**: Redis Checkpoint 24 小時後自動過期
4. **異步寫入**: 同時寫入 Redis 和 PostgreSQL (可考慮優化為異步)

---

## 第三部分: 編碼規範與最佳實踐

### 編碼規範

**C# 編碼標準**:
```yaml
命名規範:
  - 類別: PascalCase (WorkflowExecutor)
  - 方法: PascalCase (ExecuteAsync)
  - 參數: camelCase (workflowId)
  - 私有欄位: _camelCase (_logger)

Async 方法:
  - 後綴 Async (ExecuteAsync)
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
  - ParallelCoordination 所有公開方法
  - WorkflowExecutor 所有執行路徑
  - InMemoryMessageBus 所有訂閱/發布場景
  - RedisStateStore 所有存儲路徑
```

**整合測試**:
```yaml
覆蓋率目標: ≥80%

測試場景:
  - 端到端工作流執行
  - Checkpoint/Resume 流程
  - 錯誤處理與 Retry
  - 並發執行穩定性

測試環境:
  - 使用 Testcontainers (Redis, PostgreSQL)
  - 使用 WebApplicationFactory (API 測試)
```

### 性能優化指南

**性能目標**:
```yaml
Parallel Coordination:
  - 執行延遲: <5 秒 (10 個並行任務, P95)
  - 並發度: 支援 100+ 並行任務

Workflow Execution Engine:
  - 執行延遲: <5 秒 (簡單工作流, P95)
  - Checkpoint 開銷: <10% (執行時間)

Agent Messaging:
  - Message delivery 延遲: <100ms (P95)
  - Throughput: >1000 msg/sec

State Management:
  - Redis 讀取延遲: <10ms (P95)
  - PostgreSQL 寫入延遲: <50ms (P95)
```

**優化策略**:
```yaml
並發控制:
  - 使用 SemaphoreSlim 限制並發度
  - 避免過度並發導致資源耗盡

內存管理:
  - 及時釋放大對象
  - 使用 ArrayPool 重用陣列
  - 避免閉包捕獲大對象

I/O 優化:
  - 異步 I/O (避免阻塞線程)
  - 批次處理 (減少 I/O 次數)
  - 連接池 (Redis, PostgreSQL)

日誌優化:
  - 使用結構化日誌 (Serilog)
  - 避免過度日誌 (僅 Debug 級別)
  - 使用 LoggerMessage (避免裝箱)
```

---

## 第四部分: 質量保證

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
- [ ] 並發測試通過

**文檔完整性**:
- [ ] README 更新
- [ ] API 文檔更新 (Swagger)
- [ ] 架構圖更新
- [ ] 使用範例提供

### 驗收測試

**功能驗收**:
```yaml
Parallel Coordination:
  - [ ] 並行執行 10 個任務成功
  - [ ] Race condition 正確處理
  - [ ] Result aggregation 正確
  - [ ] Cancellation 正確傳播

Workflow Execution Engine:
  - [ ] 執行簡單工作流成功
  - [ ] 執行複雜工作流成功 (Sequential + Parallel)
  - [ ] 錯誤處理正確 (ContinueOnError)
  - [ ] Checkpoint 正確保存

Agent Messaging:
  - [ ] 消息發布成功
  - [ ] 消息訂閱正確
  - [ ] 事件傳遞正確
  - [ ] 多個訂閱者正確處理

State Management:
  - [ ] Checkpoint 保存到 Redis 和 PostgreSQL
  - [ ] Checkpoint 從 Redis 載入成功
  - [ ] Checkpoint 從 PostgreSQL 載入成功 (Redis 不存在時)
  - [ ] 執行歷史查詢正確
```

**性能驗收**:
```yaml
Parallel Coordination:
  - [ ] 並行執行延遲 <5 秒 (P95)
  - [ ] 支援 100+ 並行任務

Workflow Execution Engine:
  - [ ] 執行延遲 <5 秒 (P95)
  - [ ] Checkpoint 開銷 <10%

Agent Messaging:
  - [ ] Message delivery 延遲 <100ms (P95)
  - [ ] Throughput >1000 msg/sec

State Management:
  - [ ] Redis 讀取延遲 <10ms (P95)
  - [ ] PostgreSQL 寫入延遲 <50ms (P95)
```

---

## 第五部分: 參考文檔

### 核心規劃文檔
1. [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md)
2. [SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md)
3. [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md)
4. [DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md)
5. [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md)

### User Stories
6. [us-4-multi-agent-orchestration.md](../../docs/user-stories/us-4-multi-agent-orchestration.md)
7. [US-7.2-Workflow-Execution-Engine.md](../../docs/user-stories/module-04-workflow-orchestration/US-7.2-Workflow-Execution-Engine.md)
8. [US-7.4-Multi-Agent-Coordination.md](../../docs/user-stories/module-04-workflow-orchestration/US-7.4-Multi-Agent-Coordination.md)

### 架構設計文檔
9. [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
10. [ADR-007: Internal Communication Strategy](../../docs/architecture/adr/ADR-007-internal-communication-strategy.md)
11. [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)

### 技術實作文檔
12. [12-workflow-orchestration-implementation.md](../../docs/technical-implementation/01-backend-net9/12-workflow-orchestration-implementation.md)
13. [redis-configuration.md](../../docs/technical-implementation/03-data-layer/redis-configuration.md)
14. [postgresql-configuration.md](../../docs/technical-implementation/03-data-layer/postgresql-configuration.md)

### 開發標準
15. [csharp-coding-standards.md](../../docs/development-standards/csharp-coding-standards.md)
16. [dotnet9-best-practices.md](../../docs/development-standards/dotnet9-best-practices.md)
17. [test-strategy.md](../../docs/testing/test-strategy.md)

### Sprint 文檔
18. [SPRINT-8-1-OVERVIEW.md](./SPRINT-8-1-OVERVIEW.md)
19. [SPRINT-8-3-CONTEXT.md](./SPRINT-8-3-CONTEXT.md)
20. [SPRINT-8-4-CHECKLIST.md](./SPRINT-8-4-CHECKLIST.md)

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 8 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-8/SPRINT-8-2-PLAN.md`
