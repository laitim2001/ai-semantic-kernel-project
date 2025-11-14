# SPRINT-7-4-CHECKLIST.md - Sprint 7 任務清單：工作流編排引擎基礎

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

### 核心章節
1. [總體進度統計](#總體進度統計)
2. [項目準備](#項目準備)
3. [US 6.1 - 工作流編排引擎基礎 (13 SP, 5 Phases)](#us-61---工作流編排引擎基礎-13-sp-5-phases)
   - 3.1 [Phase 1: 工作流定義與解析 (Part 1)](#phase-1-工作流定義與解析-part-1-3-sp-)
   - 3.2 [Phase 2: Task Generator 實作 (Part 1)](#phase-2-task-generator-實作-part-1-3-sp-)
   - 3.3 [Phase 3: Coordination Layer 基礎](#phase-3-coordination-layer-基礎-3-sp-)
   - 3.4 [Phase 4: Workflow State Machine](#phase-4-workflow-state-machine-2-sp-)
   - 3.5 [Phase 5: Workflow Execution API](#phase-5-workflow-execution-api-2-sp-)
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
本文件為 Sprint 7 的詳細任務檢查清單，按 **User Story (US)** 組織，追蹤所有待辦事項的執行狀態和完成進度。

### 目標讀者
- **開發團隊**：日常開發任務的核心參考文件，追蹤工作進度
- **Scrum Master / PM**：Sprint 進度追蹤與風險識別
- **QA 團隊**：測試範圍與驗收標準的參考
- **Tech Lead**：技術決策與架構實施監控
- **AI Assistant**：任務狀態查詢與進度更新

### 使用方式
1. **每日開發**：查看對應 User Story 的 Phase 任務清單，更新完成狀態
2. **進度追蹤**：查看總體進度統計表，了解 Sprint 整體進度
3. **任務分配**：依據 Phase 劃分合理分配開發任務
4. **完成驗證**：使用 Definition of Done 驗證章節確保質量標準
5. **更新文件**：完成任務後立即更新檢查框狀態 [x]
6. **風險關注**：RISK-020 (工作流引擎複雜度) 為本 Sprint Critical Path 風險

### 快速導航
- **查看 Sprint 7 整體概覽** → [SPRINT-7-1-OVERVIEW.md](./SPRINT-7-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-7-2-PLAN.md](./SPRINT-7-2-PLAN.md)
- **查看技術上下文** → [SPRINT-7-3-CONTEXT.md](./SPRINT-7-3-CONTEXT.md)
- **查看開發日誌** → [SPRINT-7-5-DEV-LOG.md](./SPRINT-7-5-DEV-LOG.md)
- **查看問題追蹤** → [SPRINT-7-6-ISSUES.md](./SPRINT-7-6-ISSUES.md)

### 狀態標記說明
- ✅ **已完成** - 項目已完成並通過驗證
- 🔄 **進行中** - 項目正在執行中
- ⏳ **待開始** - 項目已計劃但尚未開始
- ⚠️ **有問題** - 項目遇到阻礙需要關注
- ❌ **失敗** - 項目未通過驗證需要重做
- 🎯 **Critical Path** - 關鍵路徑任務，影響 Sprint 交付

### 優先級標記
- **P0** - 必須完成的項目，影響 Sprint 交付
- **P1** - 重要但非阻塞的項目，建議完成
- **CRITICAL PATH** - 關鍵路徑任務，延遲影響整體進度

---

## 📊 總體進度統計

| 類別 | 已完成 / 總數 | 進度 | 狀態 |
|------|------------|------|------|
| 項目準備 | 0/12 | 0% | ⏳ |
| US 6.1 - 工作流編排引擎基礎 | 0/95 | 0% | ⏳ |
| 測試 | 0/30 | 0% | ⏳ |
| 文檔 | 0/10 | 0% | ⏳ |
| 部署 | 0/8 | 0% | ⏳ |
| **總計** | **0/155** | **0%** | ⏳ |

**圖例**: ✅ 已完成 | 🔄 進行中 | ⏳ 待開始 | ❌ 阻塞 | 🎯 Critical Path

**Story Points 分配**:
- US 6.1: 工作流編排引擎基礎 (13 SP, Part 1)
- **總計**: 13 SP

**重要里程碑**:
- Week 1 完成：工作流定義與解析 + Task Generator 基礎
- Week 2 完成：Coordination Layer 實作 + State Machine 🎯
- Week 3 完成：Workflow Execution API + 整合測試

---

## 項目準備

### 環境設置 (P0)
- [ ] 更新開發環境 (`git pull origin main`)
- [ ] 創建 Feature Branch (`git checkout -b feature/us-6.1-workflow-engine`)
- [ ] 安裝依賴 (`dotnet restore`, `pnpm install`)
- [ ] 驗證資料庫連接正常 (PostgreSQL, Redis)
- [ ] 檢查 Sprint 7 所有文檔
- [ ] 驗證 Phase 1A 所有功能正常運作
- [ ] 準備工作流測試數據
- [ ] 配置 Message Queue (RabbitMQ or Azure Service Bus)
- [ ] 檢查 Semantic Kernel 版本 (≥1.66.0)
- [ ] 驗證 Agent 引擎正常運作 (Sprint 2 交付)

### Sprint Planning (P0)
- [ ] 閱讀 [US 6.1 規格](../../docs/user-stories/modules/module-05-workflow-orchestration.md) 與 MVP 範圍
- [ ] 閱讀 [ADR-007: Multi-Agent Communication](../../docs/architecture/ADR-007-multi-agent-communication.md)
- [ ] 閱讀 [Workflow Engine Architecture](../../docs/architecture/workflow-engine-architecture.md)
- [ ] 確認 Phase 實施順序
- [ ] 規劃工作流 DSL 設計 (YAML-based)
- [ ] 規劃 Task Generator 策略
- [ ] 規劃 Coordination Layer 通訊機制
- [ ] 規劃 State Machine 設計
- [ ] 規劃測試策略 (Unit, Integration, E2E Tests)
- [ ] 確認 RISK-020 緩解措施

**驗收標準**:
- ✅ 開發環境就緒
- ✅ Feature Branch 創建成功
- ✅ 所有 Sprint 7 規格文檔已閱讀
- ✅ Message Queue 配置完成
- ✅ Phase 1A 功能驗證通過

---

## US 6.1: 工作流編排引擎基礎 (13 SP, 5 Phases)

### Phase 1: 工作流定義與解析 (Part 1) (3 SP) ⏳ 待開始

#### 工作流 DSL 設計 (P0) 🎯
- [ ] **設計工作流 YAML Schema**:
  - 工作流定義結構 (name, version, description)
  - 輸入/輸出參數定義
  - Steps 定義 (id, type, agent, inputs, outputs)
  - 依賴關係定義 (dependencies)
  - 條件邏輯 (conditions, branches)
  - 參考: [Workflow DSL Specification](../../docs/architecture/workflow-dsl-spec.md)

- [ ] **創建 Workflow Schema Validator**:
  - JSON Schema 驗證
  - 語法正確性檢查
  - 依賴關係驗證 (無循環依賴)
  - 參數類型驗證
  - Agent 存在性驗證

- [ ] **範例工作流定義**:
  ```yaml
  workflow:
    name: "customer-support-workflow"
    version: "1.0"
    description: "Customer support ticket processing"
    inputs:
      - name: "ticket_id"
        type: "string"
        required: true
    steps:
      - id: "analyze-ticket"
        type: "agent"
        agent: "ticket-analyzer"
        inputs:
          ticket: "${inputs.ticket_id}"
        outputs:
          - "category"
          - "priority"
      - id: "route-ticket"
        type: "agent"
        agent: "ticket-router"
        dependencies: ["analyze-ticket"]
        inputs:
          category: "${steps.analyze-ticket.outputs.category}"
  ```

#### Domain Layer - Workflow Entities (P0)
- [ ] **創建 Workflow Entity**: `src/AIAgentPlatform.Domain/Entities/Workflow.cs`
  - Properties: Id, Name, Version, Description, Definition, Status, CreatedAt
  - Methods: Validate(), GetSteps(), GetDependencies()
  - 參考: [Domain Model Design](../../docs/architecture/domain-model.md)

- [ ] **創建 WorkflowStep Entity**: `WorkflowStep.cs`
  - Properties: Id, WorkflowId, StepId, Type, AgentId, Inputs, Outputs, Dependencies
  - Methods: CanExecute(), GetDependencies()

- [ ] **創建 WorkflowExecution Entity**: `WorkflowExecution.cs`
  - Properties: Id, WorkflowId, Status, StartedAt, CompletedAt, CurrentStep, Context
  - Methods: Start(), Complete(), Fail(), UpdateProgress()

- [ ] **創建 WorkflowDefinition VO**: `WorkflowDefinition.cs`
  - Properties: Name, Version, Steps, Inputs, Outputs
  - Methods: Parse(), Validate(), GetExecutionPlan()

#### Application Layer - Interfaces (P0)
- [ ] **創建 IWorkflowParser Interface**: `src/AIAgentPlatform.Application/Interfaces/IWorkflowParser.cs`
  - Method: `ParseWorkflowAsync(string yamlDefinition)`
  - Return Type: `WorkflowDefinition`

- [ ] **創建 IWorkflowValidator Interface**: `IWorkflowValidator.cs`
  - Method: `ValidateWorkflowAsync(WorkflowDefinition workflow)`
  - Return Type: `ValidationResult`

- [ ] **創建 IWorkflowRepository Interface**: `IWorkflowRepository.cs`
  - Methods: CRUD operations for Workflow entities
  - Query methods: GetByName(), GetByVersion()

#### Infrastructure Layer - Parser Implementation (P0)
- [ ] **實作 YamlWorkflowParser**: `src/AIAgentPlatform.Infrastructure/Services/YamlWorkflowParser.cs`
  - 使用 YamlDotNet 解析 YAML
  - 轉換為 WorkflowDefinition 對象
  - 錯誤處理與驗證

- [ ] **實作 WorkflowValidator**: `WorkflowValidator.cs`
  - 語法驗證
  - 依賴關係驗證 (DAG 檢查)
  - 參數類型驗證
  - Agent 存在性驗證

- [ ] **實作 WorkflowRepository**: `WorkflowRepository.cs`
  - EF Core 實作
  - CRUD operations
  - 查詢優化

#### Unit Tests (P0)
- [ ] **YamlWorkflowParser 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/YamlWorkflowParserTests.cs`
  - `ParseWorkflow_ValidYaml_Success`
  - `ParseWorkflow_InvalidYaml_ThrowsException`
  - `ParseWorkflow_ComplexWorkflow_Success`
  - 測試覆蓋率: ≥85%

- [ ] **WorkflowValidator 單元測試**: `WorkflowValidatorTests.cs`
  - `ValidateWorkflow_Valid_Success`
  - `ValidateWorkflow_CircularDependency_Fails`
  - `ValidateWorkflow_InvalidAgent_Fails`
  - 測試覆蓋率: ≥85%

**驗收標準 (Phase 1)**:
- ✅ Workflow YAML Schema 設計完成
- ✅ 工作流解析器實作完成
- ✅ 工作流驗證器實作完成
- ✅ 範例工作流可成功解析
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 2: Task Generator 實作 (Part 1) (3 SP) ⏳ 待開始 🎯

#### Application Layer - Task Generator Interfaces (P0)
- [ ] **創建 ITaskGenerator Interface**: `src/AIAgentPlatform.Application/Interfaces/ITaskGenerator.cs`
  - Method: `GenerateTasksAsync(WorkflowDefinition workflow, WorkflowContext context)`
  - Return Type: `List<WorkflowTask>`

- [ ] **創建 IExecutionPlanner Interface**: `IExecutionPlanner.cs`
  - Method: `PlanExecutionAsync(WorkflowDefinition workflow)`
  - Return Type: `ExecutionPlan`

- [ ] **創建 WorkflowTask DTO**: `WorkflowTask.cs`
  - Properties: TaskId, StepId, AgentId, Inputs, Dependencies, Status
  - Methods: CanExecute(), MarkCompleted()

- [ ] **創建 ExecutionPlan VO**: `ExecutionPlan.cs`
  - Properties: Tasks, ExecutionOrder, ParallelGroups
  - Methods: GetNextTasks(), IsComplete()

#### Infrastructure Layer - Task Generator Implementation (P0)
- [ ] **實作 TaskGenerator**: `src/AIAgentPlatform.Infrastructure/Services/TaskGenerator.cs`
  - 從 WorkflowDefinition 生成 Tasks
  - 解析依賴關係
  - 參數綁定與解析
  - 條件邏輯處理

- [ ] **實作 ExecutionPlanner**: `ExecutionPlanner.cs`
  - DAG 拓撲排序
  - 識別可並行執行的 Tasks
  - 生成執行計劃
  - 優化執行順序

- [ ] **實作 DependencyResolver**: `DependencyResolver.cs`
  - 依賴關係解析
  - 循環依賴檢測
  - 參數傳遞驗證

#### Task Scheduling Logic (P0)
- [ ] **實作任務調度邏輯**:
  - 識別可執行的 Tasks (所有依賴已完成)
  - 並行執行組識別
  - 任務優先級排序
  - 資源限制考慮

- [ ] **實作參數綁定**:
  - 輸入參數綁定 (`${inputs.param}`)
  - Step 輸出綁定 (`${steps.step-id.outputs.result}`)
  - 上下文變量綁定 (`${context.user_id}`)
  - 表達式求值

- [ ] **實作條件邏輯**:
  - 條件表達式求值
  - 分支選擇邏輯
  - 動態任務生成

#### Unit Tests (P0)
- [ ] **TaskGenerator 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/TaskGeneratorTests.cs`
  - `GenerateTasks_SimpleWorkflow_Success`
  - `GenerateTasks_WithDependencies_Success`
  - `GenerateTasks_WithConditions_Success`
  - 測試覆蓋率: ≥85%

- [ ] **ExecutionPlanner 單元測試**: `ExecutionPlannerTests.cs`
  - `PlanExecution_LinearWorkflow_CorrectOrder`
  - `PlanExecution_ParallelSteps_IdentifiedCorrectly`
  - `PlanExecution_ComplexDAG_OptimalOrder`
  - 測試覆蓋率: ≥85%

- [ ] **DependencyResolver 單元測試**: `DependencyResolverTests.cs`
  - `ResolveDependencies_ValidDAG_Success`
  - `ResolveDependencies_CircularDependency_Fails`
  - `ResolveDependencies_ParameterBinding_Success`
  - 測試覆蓋率: ≥85%

**驗收標準 (Phase 2)** 🎯:
- ✅ Task Generator 實作完成
- ✅ Execution Planner 實作完成
- ✅ 依賴關係解析正確
- ✅ 參數綁定機制運作正常
- ✅ 並行執行識別正確
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 3: Coordination Layer 基礎 (3 SP) ⏳ 待開始 🎯

#### Application Layer - Coordination Interfaces (P0)
- [ ] **創建 IWorkflowCoordinator Interface**: `src/AIAgentPlatform.Application/Interfaces/IWorkflowCoordinator.cs`
  - Methods: `StartWorkflowAsync`, `ExecuteStepAsync`, `HandleStepCompletionAsync`
  - Event Handlers: `OnStepCompleted`, `OnStepFailed`, `OnWorkflowCompleted`

- [ ] **創建 ITaskScheduler Interface**: `ITaskScheduler.cs`
  - Methods: `ScheduleTaskAsync`, `GetNextTasksAsync`, `UpdateTaskStatusAsync`
  - Return Type: Task scheduling decisions

- [ ] **創建 IAgentInvoker Interface**: `IAgentInvoker.cs`
  - Method: `InvokeAgentAsync(AgentId, Inputs)`
  - Return Type: `AgentExecutionResult`

#### Infrastructure Layer - Coordination Implementation (P0)
- [ ] **實作 WorkflowCoordinator**: `src/AIAgentPlatform.Infrastructure/Services/WorkflowCoordinator.cs`
  - 工作流執行協調
  - Step 執行管理
  - 依賴關係追蹤
  - 狀態同步 (Redis + PostgreSQL)
  - 錯誤處理與重試機制

- [ ] **實作 TaskScheduler**: `TaskScheduler.cs`
  - 任務調度邏輯
  - 並行執行控制
  - 資源限制管理
  - 優先級排序

- [ ] **實作 AgentInvoker**: `AgentInvoker.cs`
  - Agent 調用邏輯
  - 參數序列化/反序列化
  - 結果收集
  - 超時控制

#### Message Queue Integration (P0)
- [ ] **配置 Message Queue**:
  - 選擇: RabbitMQ (開發) / Azure Service Bus (生產)
  - Exchange/Topic 設計
  - 消息格式定義
  - 錯誤佇列配置

- [ ] **實作 IMessagePublisher Interface**: `IMessagePublisher.cs`
  - Method: `PublishAsync(message, routingKey)`
  - Reliability: At-least-once delivery

- [ ] **實作 IMessageConsumer Interface**: `IMessageConsumer.cs`
  - Method: `ConsumeAsync(queueName, handler)`
  - Error handling: Dead-letter queue

- [ ] **實作 WorkflowEventPublisher**: `WorkflowEventPublisher.cs`
  - 發布工作流事件 (Started, StepCompleted, Completed, Failed)
  - Event schema 設計
  - Message serialization

- [ ] **實作 WorkflowEventConsumer**: `WorkflowEventConsumer.cs`
  - 訂閱工作流事件
  - 事件處理邏輯
  - 冪等性保證

#### Agent Communication (P0)
- [ ] **實作 Agent-to-Coordinator 通訊**:
  - Agent 完成通知
  - 結果回傳機制
  - 錯誤報告

- [ ] **實作 Coordinator-to-Agent 通訊**:
  - 任務分配
  - 參數傳遞
  - 取消通知

- [ ] **實作通訊協議**:
  - Message format (JSON)
  - Correlation ID 追蹤
  - Timeout handling

#### Unit Tests (P0)
- [ ] **WorkflowCoordinator 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/WorkflowCoordinatorTests.cs`
  - `StartWorkflow_ValidWorkflow_Success`
  - `ExecuteStep_ValidStep_InvokesAgent`
  - `HandleStepCompletion_TriggersNextSteps`
  - 測試覆蓋率: ≥85%

- [ ] **TaskScheduler 單元測試**: `TaskSchedulerTests.cs`
  - `ScheduleTask_ReadyTask_SchedulesImmediately`
  - `GetNextTasks_ParallelSteps_ReturnsMultiple`
  - `UpdateTaskStatus_Completed_UpdatesCorrectly`
  - 測試覆蓋率: ≥85%

- [ ] **AgentInvoker 單元測試**: `AgentInvokerTests.cs`
  - `InvokeAgent_ValidAgent_ReturnsResult`
  - `InvokeAgent_AgentNotFound_ThrowsException`
  - `InvokeAgent_Timeout_ReturnsError`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Coordination Layer 整合測試**: `tests/AIAgentPlatform.IntegrationTests/Workflow/CoordinationTests.cs`
  - `Workflow_LinearExecution_Success`
  - `Workflow_ParallelExecution_Success`
  - `Workflow_MessageQueue_EventsPublished`
  - 使用真實 Message Queue
  - 測試覆蓋率: 100%

**驗收標準 (Phase 3)** 🎯:
- ✅ Workflow Coordinator 實作完成
- ✅ Task Scheduler 運作正常
- ✅ Agent Invoker 可調用 Agent
- ✅ Message Queue 整合完成
- ✅ 事件發布/訂閱機制運作正常
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 4: Workflow State Machine (2 SP) ⏳ 待開始

#### Domain Layer - State Machine Design (P0)
- [ ] **設計 Workflow States**:
  - Pending (待執行)
  - Running (執行中)
  - Paused (暫停)
  - Completed (完成)
  - Failed (失敗)
  - Cancelled (取消)

- [ ] **設計 State Transitions**:
  - Pending → Running (開始執行)
  - Running → Paused (暫停)
  - Paused → Running (恢復)
  - Running → Completed (成功完成)
  - Running → Failed (執行失敗)
  - Running → Cancelled (取消執行)

- [ ] **創建 WorkflowState Enum**: `src/AIAgentPlatform.Domain/Enums/WorkflowState.cs`

- [ ] **創建 WorkflowStateMachine**: `src/AIAgentPlatform.Domain/StateMachines/WorkflowStateMachine.cs`
  - Methods: `TransitionTo(newState)`, `CanTransitionTo(newState)`, `GetValidTransitions()`
  - Event Triggers: `OnStateChanged`

#### Application Layer - State Management (P0)
- [ ] **創建 IWorkflowStateManager Interface**: `src/AIAgentPlatform.Application/Interfaces/IWorkflowStateManager.cs`
  - Methods: `GetStateAsync`, `UpdateStateAsync`, `CanTransitionAsync`
  - State persistence management

- [ ] **創建 StateTransitionEvent**: `StateTransitionEvent.cs`
  - Properties: WorkflowId, FromState, ToState, Timestamp, Reason
  - Event notification

#### Infrastructure Layer - State Persistence (P0)
- [ ] **實作 WorkflowStateManager**: `src/AIAgentPlatform.Infrastructure/Services/WorkflowStateManager.cs`
  - 狀態查詢與更新
  - 狀態轉換驗證
  - 狀態歷史記錄
  - Redis 緩存 + PostgreSQL 持久化

- [ ] **實作 State Persistence**:
  - 當前狀態: Redis (TTL: 24 hours)
  - 狀態歷史: PostgreSQL (WorkflowExecutionHistory table)
  - 狀態同步機制

- [ ] **實作 State Recovery**:
  - 從 Redis 恢復狀態
  - Redis 失效時從 PostgreSQL 恢復
  - 狀態一致性保證

#### Workflow Context Management (P0)
- [ ] **創建 WorkflowContext VO**: `WorkflowContext.cs`
  - Properties: ExecutionId, Inputs, StepOutputs, Variables, Metadata
  - Methods: GetInput(), SetOutput(), UpdateVariable()

- [ ] **實作 Context Persistence**:
  - Context 序列化/反序列化
  - Redis 緩存 (執行期間)
  - PostgreSQL 持久化 (完成後)

- [ ] **實作 Context Isolation**:
  - 每個執行獨立 Context
  - 參數隔離
  - 並發安全

#### Unit Tests (P0)
- [ ] **WorkflowStateMachine 單元測試**: `tests/AIAgentPlatform.UnitTests/Domain/StateMachines/WorkflowStateMachineTests.cs`
  - `TransitionTo_ValidTransition_Success`
  - `TransitionTo_InvalidTransition_Fails`
  - `GetValidTransitions_ReturnsCorrectStates`
  - 測試覆蓋率: ≥85%

- [ ] **WorkflowStateManager 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/WorkflowStateManagerTests.cs`
  - `UpdateState_ValidTransition_Success`
  - `GetState_FromRedis_Success`
  - `RecoverState_FromPostgreSQL_Success`
  - 測試覆蓋率: ≥85%

**驗收標準 (Phase 4)**:
- ✅ Workflow State Machine 設計完成
- ✅ 狀態轉換邏輯正確
- ✅ 狀態持久化機制運作正常
- ✅ 狀態恢復機制有效
- ✅ Context 管理機制完整
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 5: Workflow Execution API (2 SP) ⏳ 待開始

#### API Layer - Controllers (P0)
- [ ] **創建 WorkflowController**: `src/AIAgentPlatform.API/Controllers/WorkflowController.cs`
  - `POST /api/v1/workflows` - 創建工作流
  - `GET /api/v1/workflows` - 獲取工作流列表
  - `GET /api/v1/workflows/{id}` - 獲取工作流詳情
  - `PUT /api/v1/workflows/{id}` - 更新工作流
  - `DELETE /api/v1/workflows/{id}` - 刪除工作流
  - 參考: [API Design Guidelines](../../docs/api/API-Design-Guidelines.md)

- [ ] **創建 WorkflowExecutionController**: `WorkflowExecutionController.cs`
  - `POST /api/v1/workflows/{id}/executions` - 啟動工作流執行
  - `GET /api/v1/workflows/{id}/executions` - 獲取執行列表
  - `GET /api/v1/workflows/{id}/executions/{executionId}` - 獲取執行詳情
  - `POST /api/v1/workflows/{id}/executions/{executionId}/pause` - 暫停執行
  - `POST /api/v1/workflows/{id}/executions/{executionId}/resume` - 恢復執行
  - `POST /api/v1/workflows/{id}/executions/{executionId}/cancel` - 取消執行

#### Application Layer - CQRS Commands (P0)
- [ ] **創建 CreateWorkflowCommand**: `src/AIAgentPlatform.Application/Workflow/Commands/CreateWorkflow/CreateWorkflowCommand.cs`
  - Properties: Name, Version, Definition
  - Validation: YAML 格式驗證, 依賴關係驗證

- [ ] **創建 CreateWorkflowCommandHandler**: `CreateWorkflowCommandHandler.cs`
  - 步驟 1: 驗證工作流定義
  - 步驟 2: 解析工作流
  - 步驟 3: 保存至資料庫
  - 步驟 4: 返回工作流 ID

- [ ] **創建 StartWorkflowExecutionCommand**: `StartWorkflowExecution/StartWorkflowExecutionCommand.cs`
  - Properties: WorkflowId, Inputs
  - Handler: 啟動工作流執行

- [ ] **創建 PauseWorkflowExecutionCommand**: `PauseWorkflowExecution/PauseWorkflowExecutionCommand.cs`
  - Handler: 暫停工作流執行

- [ ] **創建 CancelWorkflowExecutionCommand**: `CancelWorkflowExecution/CancelWorkflowExecutionCommand.cs`
  - Handler: 取消工作流執行

#### Application Layer - CQRS Queries (P0)
- [ ] **創建 GetWorkflowQuery**: `src/AIAgentPlatform.Application/Workflow/Queries/GetWorkflow/GetWorkflowQuery.cs`
  - Properties: WorkflowId
  - Handler: 從資料庫獲取工作流

- [ ] **創建 GetWorkflowExecutionQuery**: `GetWorkflowExecution/GetWorkflowExecutionQuery.cs`
  - Properties: ExecutionId
  - Handler: 獲取執行詳情與狀態

- [ ] **創建 ListWorkflowExecutionsQuery**: `ListWorkflowExecutions/ListWorkflowExecutionsQuery.cs`
  - Properties: WorkflowId, PageNumber, PageSize
  - Handler: 獲取執行列表 (分頁)

#### Request/Response DTOs (P0)
- [ ] **創建 CreateWorkflowRequest**: `CreateWorkflowRequest.cs`
  - Properties: Name, Version, Description, Definition
  - Validation: Required fields, YAML format

- [ ] **創建 WorkflowDto**: `WorkflowDto.cs`
  - Properties: Id, Name, Version, Status, CreatedAt

- [ ] **創建 StartWorkflowExecutionRequest**: `StartWorkflowExecutionRequest.cs`
  - Properties: Inputs (Dictionary<string, object>)

- [ ] **創建 WorkflowExecutionDto**: `WorkflowExecutionDto.cs`
  - Properties: Id, WorkflowId, Status, CurrentStep, StartedAt, CompletedAt

#### Unit Tests (P0)
- [ ] **WorkflowController 單元測試**: `tests/AIAgentPlatform.UnitTests/API/Controllers/WorkflowControllerTests.cs`
  - `CreateWorkflow_ValidRequest_ReturnsCreated`
  - `GetWorkflow_ExistingId_ReturnsWorkflow`
  - `DeleteWorkflow_ExistingId_ReturnsNoContent`
  - 測試覆蓋率: ≥85%

- [ ] **WorkflowExecutionController 單元測試**: `WorkflowExecutionControllerTests.cs`
  - `StartExecution_ValidWorkflow_ReturnsAccepted`
  - `GetExecution_ExistingId_ReturnsExecution`
  - `PauseExecution_RunningExecution_Success`
  - 測試覆蓋率: ≥85%

- [ ] **CreateWorkflowCommandHandler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/Workflow/Commands/CreateWorkflowCommandHandlerTests.cs`
  - `Handle_ValidCommand_CreatesWorkflow`
  - `Handle_InvalidYaml_ThrowsException`
  - 測試覆蓋率: ≥85%

#### E2E Tests (P0)
- [ ] **Workflow Execution E2E Test**: `tests/AIAgentPlatform.E2ETests/Workflow/WorkflowExecutionTests.cs`
  - `ExecuteWorkflow_LinearWorkflow_Success`
  - `ExecuteWorkflow_ParallelSteps_Success`
  - `PauseAndResumeWorkflow_Success`
  - `CancelWorkflow_RunningWorkflow_Success`
  - 使用真實 API 與資料庫
  - 測試覆蓋率: 100%

**驗收標準 (Phase 5)**:
- ✅ Workflow API 完成
- ✅ Workflow Execution API 完成
- ✅ CQRS Commands/Queries 實作完整
- ✅ 單元測試覆蓋率 ≥85%
- ✅ E2E 測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

## 測試

### Unit Tests (P0)
- [ ] **Domain Layer 單元測試**:
  - Workflow Entity Tests (5 tests)
  - WorkflowStateMachine Tests (4 tests)
  - WorkflowDefinition VO Tests (3 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Application Layer 單元測試**:
  - CreateWorkflowCommandHandler Tests (4 tests)
  - StartWorkflowExecutionCommandHandler Tests (4 tests)
  - GetWorkflowQueryHandler Tests (3 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Infrastructure Layer 單元測試**:
  - YamlWorkflowParser Tests (5 tests)
  - WorkflowValidator Tests (5 tests)
  - TaskGenerator Tests (5 tests)
  - ExecutionPlanner Tests (5 tests)
  - WorkflowCoordinator Tests (5 tests)
  - TaskScheduler Tests (4 tests)
  - AgentInvoker Tests (4 tests)
  - WorkflowStateManager Tests (4 tests)
  - 測試覆蓋率: ≥85%

### Integration Tests (P0)
- [ ] **Workflow Execution 整合測試**:
  - ExecuteWorkflow_LinearWorkflow_Success
  - ExecuteWorkflow_ParallelSteps_Success
  - ExecuteWorkflow_WithDependencies_Success
  - ExecuteWorkflow_WithConditions_Success
  - 測試覆蓋率: 100%

- [ ] **Message Queue 整合測試**:
  - PublishEvent_WorkflowStarted_EventPublished
  - ConsumeEvent_StepCompleted_HandlerInvoked
  - 測試覆蓋率: 100%

- [ ] **State Management 整合測試**:
  - StateTransition_ValidTransition_Persisted
  - StateRecovery_AfterRedisFailure_FromPostgreSQL
  - 測試覆蓋率: 100%

### E2E Tests (P0)
- [ ] **Workflow E2E Test**:
  - CreateAndExecuteWorkflow_End2End_Success
  - PauseResumeWorkflow_End2End_Success
  - CancelWorkflow_End2End_Success
  - 測試覆蓋率: 100%

### Performance Tests (P1)
- [ ] **Workflow Performance**:
  - Simple Workflow Execution → <5 秒 (P95)
  - Complex Workflow (10 steps) → <30 秒 (P95)
  - Parallel Execution (5 parallel steps) → <10 秒 (P95)
  - Task Scheduling Latency → <100ms (P95)

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

- [ ] **API Design Document**:
  - `docs/api/workflow-api-design.md`
  - 包含所有端點設計
  - Request/Response 範例
  - 錯誤處理策略

### Architecture Documentation (P0)
- [ ] **ADR-009: Workflow Engine Architecture**:
  - 記錄工作流引擎設計決策
  - Task Generator 策略
  - Coordination Layer 設計
  - State Machine 設計

### Technical Implementation Guides (P1)
- [ ] **Workflow Engine Implementation Guide**:
  - `docs/technical-implementation/workflow-engine-implementation.md`
  - 如何創建自定義工作流
  - 如何擴展 Task Generator
  - 如何實作自定義 Coordinator

### User Documentation (P1)
- [ ] **Workflow User Guide**:
  - `docs/user-guides/workflow-guide.md`
  - 如何定義工作流 (YAML)
  - 如何啟動工作流執行
  - 常見問題與解決方法

**文檔驗收標準**:
- ✅ API Documentation 完整
- ✅ ADR-009 創建完成
- ✅ Implementation Guide 完成
- ✅ User Guide 完成
- ✅ 所有文檔 Review 通過

---

## 部署與DevOps

### Infrastructure Setup (P0)
- [ ] **Message Queue**:
  - 配置 RabbitMQ (開發環境)
  - 配置 Azure Service Bus (生產環境)
  - 配置 Dead-letter Queue
  - 配置 Retry Policy

- [ ] **Redis**:
  - 配置工作流狀態緩存 (TTL: 24 hours)
  - 配置 Context 緩存 (TTL: 1 hour)

- [ ] **PostgreSQL**:
  - 創建 Workflow tables (Workflows, WorkflowExecutions, WorkflowSteps)
  - 配置 Indexes (WorkflowId, ExecutionId, Status)
  - 配置資料保留政策 (保留 90 天)

### Environment Variables (P0)
- [ ] **Workflow Configuration**:
  - `WORKFLOW_MAX_PARALLEL_STEPS` (5)
  - `WORKFLOW_EXECUTION_TIMEOUT` (3600)
  - `WORKFLOW_STATE_CACHE_TTL` (86400)

- [ ] **Message Queue Configuration**:
  - `MESSAGE_QUEUE_CONNECTION_STRING`
  - `MESSAGE_QUEUE_EXCHANGE_NAME`
  - `MESSAGE_QUEUE_RETRY_COUNT` (3)

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
  - Log Task Scheduling Metrics (latency, throughput)
  - Log Message Queue Metrics (message count, latency)

- [ ] **Health Checks**:
  - Workflow Engine Health Check
  - Message Queue Health Check
  - Redis Health Check

**部署驗收標準**:
- ✅ Infrastructure 設置完成
- ✅ Environment Variables 配置完成
- ✅ CI/CD Pipeline 運作正常
- ✅ Monitoring & Alerts 配置完成
- ✅ Health Checks 全部通過

---

## Definition of Done 驗證

### Sprint 7 Definition of Done

#### Functionality (P0)
- [ ] ✅ **US 6.1 Part 1 完成**:
  - 所有 5 個 Phases 完成
  - Workflow Execution API 正常運作
  - 工作流可成功執行 (Linear + Parallel)
  - 執行成功率 ≥90%

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
- [ ] ✅ **Workflow Execution Performance**:
  - Simple Workflow <5 秒 (P95)
  - Complex Workflow <30 秒 (P95)
  - Task Scheduling Latency <100ms (P95)

- [ ] ✅ **Message Queue Performance**:
  - Event Publish Latency <50ms (P95)
  - Event Consume Latency <100ms (P95)

#### Documentation (P0)
- [ ] ✅ **API Documentation**:
  - Swagger 完整
  - API Design Document 完成

- [ ] ✅ **Architecture Documentation**:
  - ADR-009 創建完成

#### Deployment (P0)
- [ ] ✅ **Infrastructure Ready**:
  - Message Queue 配置完成
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

### Sprint 7 核心文檔 (7 refs)
1. **SPRINT-7-1-OVERVIEW.md** - Sprint 7 概覽
2. **SPRINT-7-2-PLAN.md** - Sprint 7 實施計劃
3. **SPRINT-7-3-CONTEXT.md** - Sprint 7 技術上下文
4. **SPRINT-7-4-CHECKLIST.md** - Sprint 7 檢查清單 (本文件)
5. **SPRINT-7-5-DEV-LOG.md** - Sprint 7 開發日誌
6. **SPRINT-7-6-ISSUES.md** - Sprint 7 問題追蹤
7. **SPRINT-7-7-RETROSPECTIVE.md** - Sprint 7 回顧

### User Story 規格文檔 (2 refs)
8. **module-05-workflow-orchestration.md** - US 6.1-6.3 詳細規格
9. **epic-05-workflow-orchestration.md** - Workflow Orchestration Epic

### Architecture Design Documents - ADRs (3 refs)
10. **Architecture-Design-Document.md** - 完整架構設計
11. **ADR-007: Multi-Agent Communication** - Multi-Agent 通訊決策
12. **ADR-009: Workflow Engine Architecture** - 工作流引擎架構決策 (New)

### Technical Implementation Documents (2 refs)
13. **workflow-engine-architecture.md** - 工作流引擎架構設計
14. **workflow-dsl-spec.md** - 工作流 DSL 規範

### API Design Documents (2 refs)
15. **API-Design-Guidelines.md** - API 設計指南
16. **workflow-api-design.md** - Workflow API 設計 (New)

### Planning Documents (5 refs)
17. **MVP-SCOPE-DEFINITION.md** - MVP 範圍定義與 Phase 1B 詳細規劃
18. **SPRINT-ALLOCATION-ANALYSIS.md** - Sprint 7 詳細分析 (13 SP, 3 週)
19. **RISK-REGISTER.md** - 風險登記簿 (RISK-020: 工作流引擎複雜度)
20. **TECHNICAL-DECISIONS-LOG.md** - 技術決策日誌
21. **PHASE-1B-ROADMAP.md** - Phase 1B 路線圖

### Previous Sprint References (7 refs - Sprint 6)
22. **SPRINT-6-1-OVERVIEW.md** - Sprint 6 概覽
23. **SPRINT-6-7-RETROSPECTIVE.md** - Sprint 6 回顧 (Phase 1A 完成評估)

---

## 版本歷史

| 版本 | 日期 | 作者 | 變更說明 |
|------|------|------|----------|
| v2.1 | 2025-11-14 | AI Assistant | 初始版本建立，完整 Sprint 7 任務清單 (155 項任務) |
|      |            |              | - US 6.1: 95 項任務 (5 Phases) |
|      |            |              | - 測試: 30 項任務 |
|      |            |              | - 文檔: 10 項任務 |
|      |            |              | - 部署: 8 項任務 |
|      |            |              | - 項目準備: 12 項任務 |
|      |            |              | 遵循 v2.1 標準 (8 欄位 header, 完整 ToC, 參考索引) |
|      |            |              | 特別標註 Critical Path 任務 (Phase 2-3) 🎯 |
|      |            |              | Phase 1B 啟動 Sprint (工作流引擎核心) |

---

**文件結束** - Sprint 7 任務清單已完整建立 ✅

**重要提醒** 🎯:
- Phase 2-3 為 Critical Path，必須優先完成
- RISK-020 (工作流引擎複雜度) 需持續監控
- Message Queue 整合為關鍵技術挑戰
- 並行執行邏輯需充分測試
