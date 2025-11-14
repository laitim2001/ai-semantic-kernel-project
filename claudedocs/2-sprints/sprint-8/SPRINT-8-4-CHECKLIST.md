# SPRINT-8-4-CHECKLIST.md - Sprint 8 任務清單：工作流執行引擎與消息系統

**版本**: v2.1
**Sprint 編號**: Sprint 8
**Sprint 週期**: Week 22-24 (3 週)
**Phase**: Phase 1B - 工作流引擎核心 (Workflow Engine Core)
**計劃日期**: 2026-03-10 ~ 2026-03-30
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📑 目錄 (Table of Contents)

### 核心章節
1. [總體進度統計](#總體進度統計)
2. [項目準備](#項目準備)
3. [US 6.1 - 工作流編排引擎基礎 (Part 2) (13 SP, 4 Phases)](#us-61---工作流編排引擎基礎-part-2-13-sp-4-phases)
   - 3.1 [Phase 1: Parallel Coordination 完成](#phase-1-parallel-coordination-完成-3-5-sp-)
   - 3.2 [Phase 2: Workflow Execution Engine](#phase-2-workflow-execution-engine-基礎實現-5-sp-)
   - 3.3 [Phase 3: Agent Messaging 系統](#phase-3-agent-messaging-系統-5-sp-)
   - 3.4 [Phase 4: State Management 強化](#phase-4-state-management-強化與整合-1-sp-)
4. [測試](#測試)
5. [文檔](#文檔)
6. [部署與DevOps](#部署與devops)
7. [Definition of Done 驗證](#definition-of-done-驗證)

### 輔助章節
- [使用指南](#使用指南)
- [參考文獻索引](#參考文獻索引)
- [版本歷史](#版本歷史)

---

## 📖 使用指南

### 文件目的
本文件為 Sprint 8 的詳細任務檢查清單,按 **User Story (US)** 組織,追蹤所有待辦事項的執行狀態和完成進度。

### 目標讀者
- **開發團隊**:日常開發任務的核心參考文件,追蹤工作進度
- **Scrum Master / PM**:Sprint 進度追蹤與風險識別
- **QA 團隊**:測試範圍與驗收標準的參考
- **Tech Lead**:技術決策與架構實施監控
- **AI Assistant**:任務狀態查詢與進度更新

### 使用方式
1. **每日開發**:查看對應 User Story 的 Phase 任務清單,更新完成狀態
2. **進度追蹤**:查看總體進度統計表,了解 Sprint 整體進度
3. **任務分配**:依據 Phase 劃分合理分配開發任務
4. **完成驗證**:使用 Definition of Done 驗證章節確保質量標準
5. **更新文件**:完成任務後立即更新檢查框狀態 [x]
6. **風險關注**:RISK-020 (工作流引擎複雜度) 持續監控, RISK-023/024 為新增風險

### 快速導航
- **查看 Sprint 8 整體概覽** → [SPRINT-8-1-OVERVIEW.md](./SPRINT-8-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-8-2-PLAN.md](./SPRINT-8-2-PLAN.md)
- **查看技術上下文** → [SPRINT-8-3-CONTEXT.md](./SPRINT-8-3-CONTEXT.md)
- **查看開發日誌** → [SPRINT-8-5-DEV-LOG.md](./SPRINT-8-5-DEV-LOG.md)
- **查看問題追蹤** → [SPRINT-8-6-ISSUES.md](./SPRINT-8-6-ISSUES.md)

### 狀態標記說明
- ✅ **已完成** - 項目已完成並通過驗證
- 🔄 **進行中** - 項目正在執行中
- ⏳ **待開始** - 項目已計劃但尚未開始
- ⚠️ **有問題** - 項目遇到阻礙需要關注
- ❌ **失敗** - 項目未通過驗證需要重做
- 🎯 **Critical Path** - 關鍵路徑任務,影響 Sprint 交付

### 優先級標記
- **P0** - 必須完成的項目,影響 Sprint 交付
- **P1** - 重要但非阻塞的項目,建議完成
- **CRITICAL PATH** - 關鍵路徑任務,延遲影響整體進度

---

## 📊 總體進度統計

| 類別 | 已完成 / 總數 | 進度 | 狀態 |
|------|------------|------|------|
| 項目準備 | 0/14 | 0% | ⏳ |
| US 6.1 Part 2 - 工作流編排引擎基礎 | 0/110 | 0% | ⏳ |
| 測試 | 0/35 | 0% | ⏳ |
| 文檔 | 0/12 | 0% | ⏳ |
| 部署 | 0/10 | 0% | ⏳ |
| **總計** | **0/181** | **0%** | ⏳ |

**圖例**: ✅ 已完成 | 🔄 進行中 | ⏳ 待開始 | ❌ 阻塞 | 🎯 Critical Path

**Story Points 分配**:
- US 6.1 Part 2: 工作流編排引擎基礎 (13 SP)
  - Phase 1: Parallel Coordination 完成 (3-5 SP)
  - Phase 2: Workflow Execution Engine (5 SP)
  - Phase 3: Agent Messaging 系統 (5 SP)
  - Phase 4: State Management 強化 (1 SP)
- **總計**: 13 SP

**重要里程碑**:
- Week 1 完成:Parallel Coordination 完成 + Execution Engine 基礎
- Week 2 完成:Execution Engine 完整實現 + Messaging 系統基礎 🎯
- Week 3 完成:State Management 強化 + 整合測試

---

## 項目準備

### 環境設置 (P0)
- [ ] 更新開發環境 (`git pull origin main`)
- [ ] 創建 Feature Branch (`git checkout -b feature/us-6.1-workflow-engine-part2`)
- [ ] 安裝依賴 (`dotnet restore`, `pnpm install`)
- [ ] 驗證資料庫連接正常 (PostgreSQL, Redis)
- [ ] 檢查 Sprint 8 所有文檔
- [ ] 驗證 Sprint 7 所有功能正常運作
- [ ] 準備工作流測試數據
- [ ] 配置 MediatR (Event-driven architecture)
- [ ] 檢查 Semantic Kernel 版本 (≥1.66.0)
- [ ] 驗證 Task Generator 正常運作 (Sprint 7 交付)
- [ ] 驗證 Coordination Layer 正常運作 (Sprint 7 交付)

### Sprint Planning (P0)
- [ ] 閱讀 [US 6.1 規格](../../docs/user-stories/us-7-workflow-engine.md) 與 MVP 範圍
- [ ] 閱讀 [ADR-009: Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)
- [ ] 閱讀 [ADR-007: Internal Communication Strategy](../../docs/architecture/adr/ADR-007-internal-communication-strategy.md)
- [ ] 閱讀 [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
- [ ] 確認 Phase 實施順序
- [ ] 規劃 Parallel Coordination 完成策略
- [ ] 規劃 Execution Engine 設計
- [ ] 規劃 Agent Messaging 系統設計
- [ ] 規劃 State Management 強化策略
- [ ] 規劃測試策略 (Unit, Integration, E2E Tests)
- [ ] 確認 RISK-020, RISK-023, RISK-024 緩解措施

**驗收標準**:
- ✅ 開發環境就緒
- ✅ Feature Branch 創建成功
- ✅ 所有 Sprint 8 規格文檔已閱讀
- ✅ MediatR 配置完成
- ✅ Sprint 7 功能驗證通過

---

## US 6.1: 工作流編排引擎基礎 (Part 2) (13 SP, 4 Phases)

### Phase 1: Parallel Coordination 完成 (3-5 SP) ⏳ 待開始 🎯

#### Parallel Coordination 完整實現 (P0) 🎯
- [ ] **完成 Race Condition 處理**:
  - 使用 ConcurrentBag 收集結果 (線程安全)
  - 使用 SemaphoreSlim 控制並發度 (避免資源耗盡)
  - 每個任務獨立執行上下文
  - 參考:[SPRINT-8-2-PLAN.md Phase 1](./SPRINT-8-2-PLAN.md)

- [ ] **實現 Cancellation Token Propagation**:
  - 使用 CancellationTokenSource.CreateLinkedTokenSource
  - 任一任務失敗時調用 cts.Cancel() (All-or-Nothing)
  - Catch OperationCanceledException 並記錄日誌
  - 支援優雅取消
  - 參考:[ADR-009 Workflow Orchestration Strategy](../../docs/architecture/adr/ADR-009-workflow-orchestration-strategy.md)

- [ ] **實現 Result Aggregation**:
  - 使用 ConcurrentBag<TaskResult> 收集成功結果
  - 使用 ConcurrentBag<Exception> 收集錯誤
  - 使用 AggregateException 聚合所有錯誤
  - 支援 All-or-Nothing 與 Best-Effort 策略
  - 參考:[SPRINT-8-3-CONTEXT.md TD-064](./SPRINT-8-3-CONTEXT.md)

- [ ] **並發度控制實現**:
  - 使用 SemaphoreSlim(_maxDegreeOfParallelism)
  - 預設並發度: 10
  - 可配置並發度 (Configuration)
  - 性能測試驗證 (支援 100+ 並行任務)
  - 參考:[SPRINT-8-2-PLAN.md Phase 1](./SPRINT-8-2-PLAN.md)

#### 性能優化 (P0)
- [ ] **並發執行性能優化**:
  - 測量並發執行延遲 (目標 <5 秒 P95, 10 個任務)
  - 優化 SemaphoreSlim 使用 (減少鎖競爭)
  - 優化內存使用 (及時釋放大對象)
  - Performance benchmark 測試

- [ ] **錯誤處理優化**:
  - 錯誤日誌結構化 (Serilog)
  - 錯誤聚合優化 (減少內存開銷)
  - 錯誤傳播優化 (快速失敗)

#### Unit Tests (P0)
- [ ] **ParallelCoordination 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/ParallelCoordinationTests.cs`
  - `ExecuteAsync_ParallelTasks_Success`
  - `ExecuteAsync_RaceCondition_HandledCorrectly`
  - `ExecuteAsync_ResultAggregation_Correct`
  - `ExecuteAsync_CancellationPropagation_Success`
  - `ExecuteAsync_AllOrNothing_OneFails_AllCancelled`
  - `ExecuteAsync_BestEffort_OneFails_OthersComplete`
  - `ExecuteAsync_ConcurrencyControl_Respected`
  - `ExecuteAsync_100ParallelTasks_Success` (壓力測試)
  - 測試覆蓋率: ≥85%

- [ ] **Performance Benchmark 測試**:
  - `Benchmark_10ParallelTasks_Latency` (目標 <5 秒 P95)
  - `Benchmark_100ParallelTasks_Latency`
  - `Benchmark_Throughput` (目標 >1000 tasks/sec)

**驗收標準 (Phase 1)** 🎯:
- ✅ Parallel Coordination 完整實現
- ✅ Race condition 正確處理
- ✅ Result aggregation 運作正常
- ✅ Cancellation 正確傳播
- ✅ 並發度控制有效 (支援 100+ 並行任務)
- ✅ 執行成功率 ≥95%
- ✅ 執行延遲 <5 秒 (P95, 10 個任務)
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 2: Workflow Execution Engine 基礎實現 (5 SP) ⏳ 待開始 🎯

#### Application Layer - Interfaces (P0) 🎯
- [ ] **創建 IWorkflowExecutor Interface**: `src/AIAgentPlatform.Application/Interfaces/IWorkflowExecutor.cs`
  - Method: `ExecuteAsync(workflowId, userInput, parameters, cancellationToken)`
  - Method: `ResumeAsync(executionId, cancellationToken)` (基礎版)
  - Method: `PauseAsync(executionId)`
  - Method: `CancelAsync(executionId)`
  - Method: `GetStatusAsync(executionId)`
  - 參考:[SPRINT-8-2-PLAN.md Phase 2](./SPRINT-8-2-PLAN.md)

- [ ] **創建 ExecutionResult DTO**: `ExecutionResult.cs`
  - Properties: ExecutionId, Success, Output, ErrorMessage, Duration
  - Methods: Success(), Failure()

- [ ] **創建 ExecutionStatus DTO**: `ExecutionStatus.cs`
  - Properties: ExecutionId, State, TotalTasks, CompletedTasks, StartTime, EndTime

- [ ] **創建 WorkflowExecutionState Enum**: `WorkflowExecutionState.cs`
  - States: Pending, Running, Paused, Completed, Failed, Cancelled
  - 參考:[SPRINT-8-3-CONTEXT.md TD-065](./SPRINT-8-3-CONTEXT.md)

#### Infrastructure Layer - WorkflowExecutor 實現 (P0) 🎯
- [ ] **實作 WorkflowExecutor**: `src/AIAgentPlatform.Infrastructure/Services/WorkflowExecutor.cs`
  - 依賴注入: IWorkflowRepository, ITaskGenerator, ICoordinationStrategyFactory, IStateStore, IMessageBus
  - 參考:[SPRINT-8-2-PLAN.md Phase 2](./SPRINT-8-2-PLAN.md)

- [ ] **實作 ExecuteAsync 方法**:
  - 步驟 1: 載入工作流定義 (IWorkflowRepository)
  - 步驟 2: 創建執行上下文 (ExecutionContext)
  - 步驟 3: 生成任務計劃 (ITaskGenerator)
  - 步驟 4: 執行工作流節點 (循環處理)
  - 步驟 5: Checkpoint (IStateStore)
  - 步驟 6: 事件發布 (IMessageBus)
  - 步驟 7: 錯誤處理 (ContinueOnError)
  - 參考:[SPRINT-8-2-PLAN.md WorkflowExecutor Implementation](./SPRINT-8-2-PLAN.md)

- [ ] **實作節點執行邏輯**:
  - 獲取節點對應的任務
  - 選擇協調策略 (Sequential/Parallel)
  - 執行節點任務 (ICoordinationStrategy.ExecuteAsync)
  - 更新執行上下文 (CompletedTasks, LastExecutedNodeId)
  - Checkpoint (每個節點完成後)
  - 事件發布 (TaskCompletedEvent)

- [ ] **實作錯誤處理邏輯**:
  - ContinueOnError 處理 (節點設定)
  - 工作流失敗處理 (狀態轉換 Failed)
  - 錯誤日誌記錄 (Serilog)
  - 錯誤事件發布 (WorkflowExecutionFailedEvent)

- [ ] **實作 ResumeAsync 方法 (基礎版)**:
  - 從 Checkpoint 恢復上下文 (IStateStore.LoadCheckpointAsync)
  - 載入工作流定義
  - 找到上次執行的節點 (LastExecutedNodeId)
  - 從下一個節點繼續執行
  - 注意: Phase 1B 實現簡化版本,完整版本延後到 Phase 1E

- [ ] **實作 PauseAsync 方法 (基礎版)**:
  - 更新執行狀態 (Paused)
  - 保存 Checkpoint

- [ ] **實作 CancelAsync 方法**:
  - 更新執行狀態 (Cancelled)
  - 保存 Checkpoint
  - 發布事件 (WorkflowExecutionCancelledEvent)

- [ ] **實作 GetStatusAsync 方法**:
  - 從 Checkpoint 載入上下文
  - 返回執行狀態 (ExecutionStatus)

#### Domain Layer - Execution Context (P0)
- [ ] **創建 ExecutionContext VO**: `src/AIAgentPlatform.Domain/ValueObjects/ExecutionContext.cs`
  - Properties: ExecutionId, WorkflowId, UserInput, Parameters, State, StartTime, EndTime, Duration
  - Properties: TotalTasks, CompletedTasks, LastExecutedNodeId
  - Properties: Variables (Dictionary<string, object>)
  - Properties: ErrorMessage
  - Methods: UpdateProgress(), UpdateState(), UpdateVariables()
  - 參考:[SPRINT-8-3-CONTEXT.md TD-066](./SPRINT-8-3-CONTEXT.md)

- [ ] **創建變數管理邏輯**:
  - 變數存儲 (Variables: Dictionary<string, object>)
  - 節點輸出綁定 (OutputVariable → Variables[key])
  - 變數作用域管理 (Workflow-level 全局變數)

#### 生命週期管理 (P0)
- [ ] **實作狀態轉換邏輯**:
  - Pending → Running (開始執行)
  - Running → Paused (暫停)
  - Paused → Running (恢復)
  - Running → Completed (成功完成)
  - Running → Failed (執行失敗)
  - Running → Cancelled (取消執行)
  - 參考:[SPRINT-8-3-CONTEXT.md TD-065](./SPRINT-8-3-CONTEXT.md)

- [ ] **實作生命週期事件發布**:
  - WorkflowExecutionStartedEvent (開始執行)
  - WorkflowExecutionCompletedEvent (執行完成)
  - WorkflowExecutionFailedEvent (執行失敗)
  - WorkflowExecutionCancelledEvent (取消執行)

#### Unit Tests (P0)
- [ ] **WorkflowExecutor 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/WorkflowExecutorTests.cs`
  - `ExecuteAsync_SimpleWorkflow_Success`
  - `ExecuteAsync_ComplexWorkflow_Success` (Sequential + Parallel)
  - `ExecuteAsync_NodeFails_ContinueOnError_Success`
  - `ExecuteAsync_NodeFails_StopOnError_Failed`
  - `ExecuteAsync_Checkpoint_SavedCorrectly`
  - `ExecuteAsync_EventsPublished_Correctly`
  - `ResumeAsync_FromCheckpoint_Success`
  - `PauseAsync_UpdatesState_Success`
  - `CancelAsync_UpdatesState_Success`
  - `GetStatusAsync_ReturnsCorrectStatus`
  - 測試覆蓋率: ≥85%

- [ ] **ExecutionContext 單元測試**: `ExecutionContextTests.cs`
  - `UpdateProgress_UpdatesCorrectly`
  - `UpdateState_ValidTransition_Success`
  - `UpdateVariables_StoresCorrectly`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Workflow Execution 整合測試**: `tests/AIAgentPlatform.IntegrationTests/Workflow/WorkflowExecutionTests.cs`
  - `ExecuteWorkflow_LinearWorkflow_EndToEnd_Success`
  - `ExecuteWorkflow_ParallelWorkflow_EndToEnd_Success`
  - `ExecuteWorkflow_WithCheckpoint_Resume_Success`
  - 使用真實 Task Generator, Coordination Layer, Message Bus, State Store
  - 測試覆蓋率: 100%

**驗收標準 (Phase 2)** 🎯:
- ✅ IWorkflowExecutor 介面定義完整
- ✅ WorkflowExecutor 核心邏輯實現
- ✅ ExecutionContext 正確管理
- ✅ 節點執行邏輯正確
- ✅ 錯誤處理與 ContinueOnError 運作正常
- ✅ 生命週期狀態轉換正確
- ✅ 事件發布正確
- ✅ 工作流執行成功率 ≥95%
- ✅ 執行延遲 <5 秒 (簡單工作流, P95)
- ✅ Checkpoint 開銷 <10%
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋核心流程
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 3: Agent Messaging 系統 (5 SP) ⏳ 待開始 🎯

#### Application Layer - Message Bus Interfaces (P0) 🎯
- [ ] **創建 IMessageBus Interface**: `src/AIAgentPlatform.Application/Interfaces/IMessageBus.cs`
  - Method: `PublishAsync<T>(message)` where T : IMessage
  - Method: `SubscribeAsync<T>(handler)` where T : IMessage
  - Method: `UnsubscribeAsync<T>(handler)` where T : IMessage
  - 參考:[SPRINT-8-2-PLAN.md Phase 3](./SPRINT-8-2-PLAN.md)

- [ ] **創建 IMessage Interface**: `IMessage.cs`
  - Properties: MessageId, SenderId, ReceiverId, Timestamp, CorrelationId
  - 參考:[SPRINT-8-3-CONTEXT.md Agent Messaging 系統設計](./SPRINT-8-3-CONTEXT.md)

#### Domain Layer - Agent 間消息協議 (P0)
- [ ] **創建 WorkflowExecutionStartedEvent**: `src/AIAgentPlatform.Domain/Messages/WorkflowExecutionStartedEvent.cs`
  - Properties: ExecutionId, WorkflowId
  - CorrelationId: executionId.ToString()

- [ ] **創建 TaskCompletedEvent**: `TaskCompletedEvent.cs`
  - Properties: NodeId, Result (CoordinationResult)
  - SenderId: "CoordinationStrategy"

- [ ] **創建 WorkflowExecutionCompletedEvent**: `WorkflowExecutionCompletedEvent.cs`
  - Properties: ExecutionId
  - CorrelationId: executionId.ToString()

- [ ] **創建 WorkflowExecutionFailedEvent**: `WorkflowExecutionFailedEvent.cs`
  - Properties: ExecutionId, ErrorMessage
  - CorrelationId: executionId.ToString()

- [ ] **創建 WorkflowExecutionCancelledEvent**: `WorkflowExecutionCancelledEvent.cs`
  - Properties: ExecutionId
  - CorrelationId: executionId.ToString()

#### Infrastructure Layer - InMemoryMessageBus 實現 (P0) 🎯
- [ ] **實作 InMemoryMessageBus**: `src/AIAgentPlatform.Infrastructure/Services/InMemoryMessageBus.cs`
  - 使用 ConcurrentDictionary<Type, List<Delegate>> 存儲訂閱者
  - Topic-based routing (基於消息類型)
  - 參考:[SPRINT-8-2-PLAN.md Phase 3](./SPRINT-8-2-PLAN.md)

- [ ] **實作 PublishAsync 方法**:
  - 查找消息類型對應的 handlers
  - 並行調用所有 handlers (Task.WhenAll)
  - 錯誤隔離 (Handler 異常不影響其他 handlers)
  - 日誌記錄 (發布成功/失敗)

- [ ] **實作 SubscribeAsync 方法**:
  - 添加 handler 到訂閱者列表
  - 線程安全 (lock)
  - 日誌記錄 (訂閱成功)

- [ ] **實作 UnsubscribeAsync 方法**:
  - 移除 handler 從訂閱者列表
  - 線程安全 (lock)
  - 日誌記錄 (取消訂閱)

#### Event-Driven Communication 實現 (P0)
- [ ] **實作 Event Handler Registration**:
  - WorkflowExecutionStartedEvent handler
  - TaskCompletedEvent handler
  - WorkflowExecutionCompletedEvent handler
  - WorkflowExecutionFailedEvent handler

- [ ] **實作 Event Delivery**:
  - Async delivery (並行調用)
  - At-least-once delivery (確保傳遞)
  - Message ordering (同一 Topic)
  - 參考:[SPRINT-8-3-CONTEXT.md TD-068](./SPRINT-8-3-CONTEXT.md)

#### Message Delivery 保證 (P0)
- [ ] **實作 At-least-once Delivery**:
  - 同步調用所有 handlers (await Task.WhenAll)
  - Handler 異常不影響消息傳遞 (錯誤日誌)
  - 無 Dead Letter Queue (Phase 1B 簡化)

- [ ] **實作 Message Ordering**:
  - 同一 Topic 的消息按發布順序處理
  - 使用 Task.WhenAll 並行處理不同 Topic

#### Unit Tests (P0)
- [ ] **InMemoryMessageBus 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/InMemoryMessageBusTests.cs`
  - `PublishAsync_NoSubscribers_NoError`
  - `PublishAsync_OneSubscriber_HandlerInvoked`
  - `PublishAsync_MultipleSubscribers_AllHandlersInvoked`
  - `PublishAsync_HandlerException_OtherHandlersStillInvoked`
  - `SubscribeAsync_AddsHandler_Success`
  - `UnsubscribeAsync_RemovesHandler_Success`
  - `PublishAsync_MessageOrdering_Correct` (同一 Topic)
  - 測試覆蓋率: ≥85%

- [ ] **Event Messages 單元測試**: `EventMessagesTests.cs`
  - `WorkflowExecutionStartedEvent_PropertiesSet_Correctly`
  - `TaskCompletedEvent_PropertiesSet_Correctly`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Message Bus 整合測試**: `tests/AIAgentPlatform.IntegrationTests/Messaging/MessageBusTests.cs`
  - `PublishAndSubscribe_EventDelivered_Success`
  - `MultipleSubscribers_AllReceiveEvent`
  - `EventOrdering_PreservedCorrectly`
  - 測試覆蓋率: 100%

#### Performance Tests (P1)
- [ ] **Message Bus Performance 測試**:
  - `Benchmark_MessageDeliveryLatency` (目標 <100ms P95)
  - `Benchmark_Throughput` (目標 >1000 msg/sec)
  - `Benchmark_ConcurrentSubscribers` (支援 100+ 訂閱者)

**驗收標準 (Phase 3)** 🎯:
- ✅ IMessageBus 介面定義完整
- ✅ InMemoryMessageBus 實現完成
- ✅ Agent 間消息協議定義清晰
- ✅ Event-driven communication 運作正常
- ✅ Event delivery 正確 (At-least-once)
- ✅ Message ordering 保證正確 (同一 Topic)
- ✅ Message delivery 延遲 <100ms (P95)
- ✅ Throughput >1000 msg/sec
- ✅ 支援 100+ concurrent subscribers
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋核心流程
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 4: State Management 強化與整合 (1 SP) ⏳ 待開始

#### Application Layer - State Store Interface (P0)
- [ ] **創建 IStateStore Interface**: `src/AIAgentPlatform.Application/Interfaces/IStateStore.cs`
  - Method: `SaveCheckpointAsync(context)`
  - Method: `LoadCheckpointAsync(executionId)`
  - Method: `DeleteCheckpointAsync(executionId)`
  - Method: `GetExecutionHistoryAsync(pageIndex, pageSize)`
  - 參考:[SPRINT-8-2-PLAN.md Phase 4](./SPRINT-8-2-PLAN.md)

#### Infrastructure Layer - RedisStateStore 實現 (P0)
- [ ] **實作 RedisStateStore**: `src/AIAgentPlatform.Infrastructure/Services/RedisStateStore.cs`
  - 依賴注入: IConnectionMultiplexer (Redis), IWorkflowExecutionRepository (PostgreSQL)
  - 參考:[SPRINT-8-2-PLAN.md Phase 4](./SPRINT-8-2-PLAN.md)

- [ ] **實作 SaveCheckpointAsync 方法**:
  - 步驟 1: 序列化 ExecutionContext (JSON)
  - 步驟 2: 保存到 Redis (Key: checkpoint:{executionId}, TTL: 24 hours)
  - 步驟 3: 保存到 PostgreSQL (WorkflowExecution 表)
  - 日誌記錄 (保存成功)
  - 參考:[SPRINT-8-3-CONTEXT.md State Management 混合存儲策略](./SPRINT-8-3-CONTEXT.md)

- [ ] **實作 LoadCheckpointAsync 方法**:
  - 步驟 1: 從 Redis 讀取 (快速路徑)
  - 步驟 2: Redis 沒有,從 PostgreSQL 讀取 (慢速路徑)
  - 步驟 3: 重新放入 Redis 快取
  - 日誌記錄 (載入成功/失敗)

- [ ] **實作 DeleteCheckpointAsync 方法**:
  - 從 Redis 刪除 (立即生效)
  - PostgreSQL 保留 (用於歷史查詢)

- [ ] **實作 GetExecutionHistoryAsync 方法**:
  - 從 PostgreSQL 查詢歷史記錄 (分頁)
  - 反序列化 ExecutionContext
  - 返回 PagedResult<ExecutionContext>

#### Database Schema (P0)
- [ ] **創建 workflow_executions 表**:
  - Columns: id, workflow_id, state, total_tasks, completed_tasks, start_time, end_time, duration, error_message, context_json
  - Indexes: workflow_id, state, start_time
  - 參考:[docs/database/workflow-schema.md](../../docs/database/workflow-schema.md)

- [ ] **創建 EF Core Migration**:
  - Migration: AddWorkflowExecutionsTable
  - Apply migration: `dotnet ef database update`

#### Domain Events 完整實現 (P0)
- [ ] **所有 Domain Events 實現**:
  - WorkflowExecutionStartedEvent ✅ (已在 Phase 3)
  - WorkflowExecutionCompletedEvent ✅ (已在 Phase 3)
  - WorkflowExecutionFailedEvent ✅ (已在 Phase 3)
  - WorkflowExecutionCancelledEvent ✅ (已在 Phase 3)
  - TaskExecutionProgressEvent (新增)

- [ ] **創建 TaskExecutionProgressEvent**: `TaskExecutionProgressEvent.cs`
  - Properties: ExecutionId, NodeId, Progress (%)

#### MediatR Integration 優化 (P0)
- [ ] **Event Handlers 實現**:
  - WorkflowExecutionStartedEventHandler
  - WorkflowExecutionCompletedEventHandler
  - WorkflowExecutionFailedEventHandler
  - TaskExecutionProgressEventHandler

- [ ] **MediatR Notification 發布**:
  - 整合 IMessageBus 與 MediatR
  - 確保所有事件正確發布

#### Unit Tests (P0)
- [ ] **RedisStateStore 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/RedisStateStoreTests.cs`
  - `SaveCheckpointAsync_SavesToRedisAndPostgreSQL`
  - `LoadCheckpointAsync_FromRedis_Success`
  - `LoadCheckpointAsync_FromPostgreSQL_WhenRedisEmpty`
  - `DeleteCheckpointAsync_DeletesFromRedisOnly`
  - `GetExecutionHistoryAsync_Paged_ReturnsCorrectly`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **State Management 整合測試**: `tests/AIAgentPlatform.IntegrationTests/State/StateManagementTests.cs`
  - `SaveAndLoad_Checkpoint_RoundTrip_Success`
  - `LoadFromPostgreSQL_WhenRedisExpired_Success`
  - `GetExecutionHistory_Paged_Correct`
  - 使用真實 Redis 和 PostgreSQL
  - 測試覆蓋率: 100%

**驗收標準 (Phase 4)**:
- ✅ IStateStore 介面定義完整
- ✅ RedisStateStore 實現完成
- ✅ Checkpoint 保存到 Redis 和 PostgreSQL
- ✅ Checkpoint 載入正確 (Redis 優先)
- ✅ Checkpoint 恢復成功率 ≥99%
- ✅ Domain Events 完整實現
- ✅ MediatR Integration 穩定
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋核心流程
- ✅ Code Review 通過 (無 Critical/High Issues)

---

## 測試

### Unit Tests (P0)
- [ ] **Domain Layer 單元測試**:
  - ExecutionContext VO Tests (5 tests)
  - WorkflowExecutionState Enum Tests (3 tests)
  - Message Types Tests (5 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Application Layer 單元測試**:
  - IWorkflowExecutor Interface Tests (3 tests)
  - IMessageBus Interface Tests (3 tests)
  - IStateStore Interface Tests (3 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Infrastructure Layer 單元測試**:
  - ParallelCoordination Tests (8 tests) ✅ (已在 Phase 1)
  - WorkflowExecutor Tests (10 tests) ✅ (已在 Phase 2)
  - InMemoryMessageBus Tests (7 tests) ✅ (已在 Phase 3)
  - RedisStateStore Tests (5 tests) ✅ (已在 Phase 4)
  - 測試覆蓋率: ≥85%

### Integration Tests (P0)
- [ ] **Workflow Execution 整合測試**:
  - ExecuteWorkflow_LinearWorkflow_EndToEnd_Success ✅ (已在 Phase 2)
  - ExecuteWorkflow_ParallelWorkflow_EndToEnd_Success ✅ (已在 Phase 2)
  - ExecuteWorkflow_WithCheckpoint_Resume_Success ✅ (已在 Phase 2)
  - ExecuteWorkflow_WithMessages_EventsPublished
  - ExecuteWorkflow_WithState_CheckpointSaved
  - 測試覆蓋率: 100%

- [ ] **Message Bus 整合測試**:
  - PublishAndSubscribe_EventDelivered_Success ✅ (已在 Phase 3)
  - MultipleSubscribers_AllReceiveEvent ✅ (已在 Phase 3)
  - EventOrdering_PreservedCorrectly ✅ (已在 Phase 3)
  - 測試覆蓋率: 100%

- [ ] **State Management 整合測試**:
  - SaveAndLoad_Checkpoint_RoundTrip_Success ✅ (已在 Phase 4)
  - LoadFromPostgreSQL_WhenRedisExpired_Success ✅ (已在 Phase 4)
  - GetExecutionHistory_Paged_Correct ✅ (已在 Phase 4)
  - 測試覆蓋率: 100%

### E2E Tests (P0)
- [ ] **Workflow E2E Test**: `tests/AIAgentPlatform.E2ETests/Workflow/WorkflowE2ETests.cs`
  - `CreateAndExecuteWorkflow_End2End_Success`
  - `ExecuteWorkflow_WithParallelCoordination_Success`
  - `PauseResumeWorkflow_End2End_Success`
  - `CancelWorkflow_End2End_Success`
  - `WorkflowWithMessages_EventsReceived_Success`
  - `WorkflowWithCheckpoint_ResumeAfterRestart_Success`
  - 測試覆蓋率: 100%

### Performance Tests (P1)
- [ ] **Parallel Coordination Performance**:
  - Simple Workflow Execution → <5 秒 (P95, 10 個任務)
  - Complex Workflow (20 tasks) → <10 秒 (P95)
  - 100 Parallel Tasks → <15 秒 (P95)
  - Throughput → >1000 tasks/sec

- [ ] **Workflow Execution Engine Performance**:
  - Simple Workflow → <5 秒 (P95)
  - Complex Workflow (10 nodes) → <30 秒 (P95)
  - Checkpoint Overhead → <10% (執行時間)

- [ ] **Agent Messaging Performance**:
  - Message Delivery Latency → <100ms (P95)
  - Throughput → >1000 msg/sec
  - Concurrent Subscribers → 支援 100+

- [ ] **State Management Performance**:
  - Redis Read Latency → <10ms (P95)
  - PostgreSQL Write Latency → <50ms (P95)
  - Checkpoint Save Time → <100ms (P95)

**測試驗收標準**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋率 100%
- ✅ E2E 測試覆蓋率 100%
- ✅ 所有測試通過率 100%
- ✅ 性能測試達標

---

## 文檔

### API Documentation (P0)
- [ ] **Swagger Documentation**:
  - 所有 API 端點有完整 XML 註解
  - Request/Response Schema 定義完整
  - Example Requests/Responses
  - Error Codes 說明

- [ ] **Workflow Execution API 文檔**:
  - `POST /api/v1/workflows/{id}/execute` 文檔
  - `POST /api/v1/workflows/{id}/pause` 文檔
  - `POST /api/v1/workflows/{id}/resume` 文檔
  - `POST /api/v1/workflows/{id}/cancel` 文檔
  - `GET /api/v1/workflows/{id}/status` 文檔

### Architecture Documentation (P0)
- [ ] **更新 Architecture Design Document**:
  - Execution Engine 架構圖
  - Message Bus 架構圖
  - State Management 架構圖

- [ ] **技術決策記錄更新**:
  - TD-064: Parallel Coordination 並發度控制策略
  - TD-065: Execution Engine 生命週期狀態轉換策略
  - TD-066: Execution Context 變數作用域策略
  - TD-067: Message Bus 實現策略
  - TD-068: Message Delivery 保證策略
  - TD-069: Checkpoint 頻率策略

### Technical Implementation Guides (P1)
- [ ] **Workflow Execution Engine Implementation Guide**:
  - `docs/technical-implementation/workflow-execution-engine-implementation.md`
  - 如何使用 Workflow Executor
  - 如何擴展 Coordination Strategy
  - 如何實作自定義 Message Handler

### User Documentation (P1)
- [ ] **Workflow Execution User Guide**:
  - `docs/user-guides/workflow-execution-guide.md`
  - 如何執行工作流
  - 如何暫停/恢復工作流
  - 如何監控工作流執行狀態
  - 常見問題與解決方法

**文檔驗收標準**:
- ✅ API Documentation 完整
- ✅ 技術決策記錄更新完成
- ✅ Architecture 文檔更新完成
- ✅ Implementation Guide 完成
- ✅ User Guide 完成
- ✅ 所有文檔 Review 通過

---

## 部署與DevOps

### Infrastructure Setup (P0)
- [ ] **MediatR Configuration**:
  - 配置 MediatR (Event-driven architecture)
  - 配置 Event Handlers
  - 配置 Notification Handlers

- [ ] **Redis Configuration**:
  - 配置工作流執行狀態緩存 (TTL: 24 hours)
  - 配置 Checkpoint 緩存
  - 配置連接池

- [ ] **PostgreSQL Configuration**:
  - 創建 workflow_executions 表
  - 配置 Indexes (workflow_id, state, start_time)
  - 配置資料保留政策 (保留 90 天)

### Environment Variables (P0)
- [ ] **Workflow Execution Configuration**:
  - `WORKFLOW_EXECUTION_TIMEOUT` (3600)
  - `WORKFLOW_CHECKPOINT_INTERVAL` (每個節點完成後)
  - `WORKFLOW_MAX_PARALLEL_TASKS` (10)

- [ ] **Message Bus Configuration**:
  - `MESSAGE_BUS_TYPE` (InMemory)
  - `MESSAGE_BUS_DELIVERY_GUARANTEE` (AtLeastOnce)

- [ ] **State Management Configuration**:
  - `STATE_STORE_TYPE` (Redis)
  - `CHECKPOINT_TTL` (86400 秒 = 24 小時)

### CI/CD Pipeline (P0)
- [ ] **Backend CI Pipeline**:
  - Build .NET Solution
  - Run Unit Tests (≥85% coverage)
  - Run Integration Tests
  - Generate Code Coverage Report
  - Run Static Code Analysis (SonarQube)

- [ ] **CD Pipeline**:
  - Deploy to Staging Environment
  - Run E2E Tests on Staging
  - Deploy to Production (manual approval)
  - Run Smoke Tests on Production

### Monitoring & Alerts (P1)
- [ ] **Application Insights**:
  - Log Workflow Execution Metrics (execution time, success rate)
  - Log Message Bus Metrics (message count, latency)
  - Log State Management Metrics (checkpoint save time, load time)

- [ ] **Health Checks**:
  - Workflow Executor Health Check
  - Message Bus Health Check
  - Redis Health Check
  - PostgreSQL Health Check

**部署驗收標準**:
- ✅ Infrastructure 設置完成
- ✅ Environment Variables 配置完成
- ✅ CI/CD Pipeline 運作正常
- ✅ Monitoring & Alerts 配置完成
- ✅ Health Checks 全部通過

---

## Definition of Done 驗證

### Sprint 8 Definition of Done

#### Functionality (P0)
- [ ] ✅ **US 6.1 Part 2 完成**:
  - 所有 4 個 Phases 完成
  - Parallel Coordination 完整實現
  - Workflow Execution Engine 正常運作
  - Agent Messaging 系統正常運作
  - State Management 強化完成
  - 工作流執行成功率 ≥95%

#### Code Quality (P0)
- [ ] ✅ **測試覆蓋率**:
  - Unit Tests ≥85%
  - Integration Tests 100%
  - E2E Tests 100%

- [ ] ✅ **Code Review**:
  - 所有 PR Review 通過
  - 0 Critical Issues
  - 0 High Issues

- [ ] ✅ **Static Analysis**:
  - SonarQube Quality Gate 通過
  - 0 Code Smells (Major)
  - 0 Security Vulnerabilities

#### Performance (P0)
- [ ] ✅ **Parallel Coordination Performance**:
  - 並行執行延遲 <5 秒 (P95, 10 個任務)
  - 支援 100+ 並行任務

- [ ] ✅ **Workflow Execution Engine Performance**:
  - 執行延遲 <5 秒 (簡單工作流, P95)
  - Checkpoint 開銷 <10%

- [ ] ✅ **Agent Messaging Performance**:
  - Message Delivery Latency <100ms (P95)
  - Throughput >1000 msg/sec

- [ ] ✅ **State Management Performance**:
  - Redis Read Latency <10ms (P95)
  - PostgreSQL Write Latency <50ms (P95)

#### Documentation (P0)
- [ ] ✅ **API Documentation**:
  - Swagger 完整
  - 技術決策記錄更新完成 (TD-064 to TD-069)

- [ ] ✅ **Architecture Documentation**:
  - Architecture Design Document 更新完成

#### Deployment (P0)
- [ ] ✅ **Infrastructure Ready**:
  - MediatR 配置完成
  - Redis 配置完成
  - PostgreSQL 配置完成

- [ ] ✅ **CI/CD Pipeline**:
  - Backend CI Pipeline 通過
  - CD Pipeline 部署成功

- [ ] ✅ **Monitoring**:
  - Application Insights 配置完成
  - Health Checks 全部通過

**最終驗收**:
- ✅ 所有功能開發完成
- ✅ 所有測試通過
- ✅ 所有文檔完成
- ✅ 部署成功
- ✅ Sprint Review 通過
- ✅ Stakeholder Sign-off

---

## 參考文獻索引

### Sprint 8 核心文檔 (7 refs)
1. **SPRINT-8-1-OVERVIEW.md** - Sprint 8 概覽
2. **SPRINT-8-2-PLAN.md** - Sprint 8 實施計劃
3. **SPRINT-8-3-CONTEXT.md** - Sprint 8 技術上下文
4. **SPRINT-8-4-CHECKLIST.md** - Sprint 8 檢查清單 (本文件)
5. **SPRINT-8-5-DEV-LOG.md** - Sprint 8 開發日誌
6. **SPRINT-8-6-ISSUES.md** - Sprint 8 問題追蹤
7. **SPRINT-8-7-RETROSPECTIVE.md** - Sprint 8 回顧

### User Story 規格文檔 (2 refs)
8. **us-7-workflow-engine.md** - US 6.1 Part 2 詳細規格
9. **epic-07-workflow-engine.md** - Workflow Engine Epic

### Architecture Design Documents - ADRs (3 refs)
10. **Architecture-Design-Document.md** - 完整架構設計
11. **ADR-009: Workflow Orchestration Strategy** - 工作流編排策略決策
12. **ADR-007: Internal Communication Strategy** - Agent 間通訊決策
13. **ADR-006: Hybrid State Management** - 混合狀態管理決策

### Technical Implementation Documents (2 refs)
14. **12-workflow-orchestration-implementation.md** - 工作流編排實作指南
15. **redis-configuration.md** - Redis 配置指南
16. **postgresql-configuration.md** - PostgreSQL 配置指南

### API Design Documents (1 ref)
17. **workflow-api-design.md** - Workflow API 設計

### Planning Documents (5 refs)
18. **MVP-SCOPE-DEFINITION.md** - MVP 範圍定義與 Phase 1B 詳細規劃
19. **SPRINT-ALLOCATION-ANALYSIS.md** - Sprint 8 詳細分析 (13 SP, 3 週)
20. **RISK-REGISTER.md** - 風險登記簿 (RISK-020, RISK-023, RISK-024)
21. **TECHNICAL-DECISIONS-LOG.md** - 技術決策日誌 (TD-064 to TD-069)
22. **DEPENDENCY-MATRIX.md** - Sprint 7-9 關鍵路徑依賴分析

### Previous Sprint References (7 refs - Sprint 7)
23. **SPRINT-7-1-OVERVIEW.md** - Sprint 7 概覽
24. **SPRINT-7-7-RETROSPECTIVE.md** - Sprint 7 回顧

---

## 版本歷史

| 版本 | 日期 | 作者 | 變更說明 |
|------|------|------|----------|
| v2.1 | 2025-11-14 | AI Assistant | 初始版本建立,完整 Sprint 8 任務清單 (181 項任務) |
|      |            |              | - US 6.1 Part 2: 110 項任務 (4 Phases) |
|      |            |              | - Phase 1: Parallel Coordination 完成 (25 項) 🎯 |
|      |            |              | - Phase 2: Workflow Execution Engine (40 項) 🎯 |
|      |            |              | - Phase 3: Agent Messaging 系統 (30 項) 🎯 |
|      |            |              | - Phase 4: State Management 強化 (15 項) |
|      |            |              | - 測試: 35 項任務 |
|      |            |              | - 文檔: 12 項任務 |
|      |            |              | - 部署: 10 項任務 |
|      |            |              | - 項目準備: 14 項任務 |
|      |            |              | 遵循 v2.1 標準 (8 欄位 header, 完整 ToC, 參考索引) |
|      |            |              | 特別標註 Critical Path 任務 (Phase 1-3) 🎯 |
|      |            |              | Phase 1B 第二階段 Sprint (工作流執行引擎與消息系統) |

---

**文件結束** - Sprint 8 任務清單已完整建立 ✅

**重要提醒** 🎯:
- Phase 1-3 為 Critical Path,必須優先完成
- RISK-020 (工作流引擎複雜度) 持續監控
- RISK-023 (狀態管理複雜度) 和 RISK-024 (消息性能瓶頸) 需密切關注
- Execution Engine 與 Messaging 系統為核心技術挑戰
- State Management 混合存儲需充分測試
