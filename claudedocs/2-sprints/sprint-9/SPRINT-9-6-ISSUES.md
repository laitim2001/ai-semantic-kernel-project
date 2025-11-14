# SPRINT 9 - 問題追蹤 (Issue Tracking)

---

## 📋 文件資訊 (Document Information)

| 欄位 | 內容 |
|------|------|
| **版本** | v2.1 |
| **Sprint 編號** | Sprint 9 |
| **Sprint 週期** | Week 25-27 (3 週) |
| **Phase** | Phase 1B - 工作流引擎核心 (Workflow Engine Core) |
| **計劃日期** | 2026-03-31 ~ 2026-04-20 |
| **狀態** | 📋 計劃階段 (Planned) |
| **創建日期** | 2025-11-14 |
| **最後更新** | 2025-11-14 |

---

## 📑 目錄 (Table of Contents)

1. [問題追蹤系統說明](#問題追蹤系統說明)
2. [問題統計概覽](#問題統計概覽)
3. [Critical Issues (P0)](#critical-issues-p0)
4. [High Priority Issues (P1)](#high-priority-issues-p1)
5. [Medium Priority Issues (P2)](#medium-priority-issues-p2)
6. [Low Priority Issues (P3)](#low-priority-issues-p3)
7. [已解決問題](#已解決問題)
8. [常見問題與解決方案](#常見問題與解決方案)
9. [技術債務追蹤](#技術債務追蹤)

---

## 問題追蹤系統說明

### 問題分類

Sprint 9 問題追蹤使用以下標籤系統:

| 標籤 | 含義 | 顏色 | 使用場景 |
|------|------|------|----------|
| 🐛 Bug | 程式錯誤 | 紅色 | 功能不正常, 產生錯誤 |
| 🔥 Blocker | 阻礙問題 | 深紅色 | 阻止開發進行, 必須立即解決 |
| ⚠️ Warning | 警告 | 橙色 | 潛在問題, 需要注意 |
| 💡 Enhancement | 改進建議 | 藍色 | 功能增強, 性能優化 |
| 📝 Documentation | 文檔相關 | 綠色 | 文檔缺失, 需要更新 |
| 🧪 Testing | 測試相關 | 紫色 | 測試失敗, 覆蓋率不足 |
| 🏗️ Architecture | 架構相關 | 灰色 | 設計問題, 架構調整 |

### 優先級定義

| 優先級 | 符號 | 定義 | 解決時限 |
|--------|------|------|----------|
| P0 - Critical | 🔴 | 阻止 Sprint 完成, 影響核心功能 | 24 小時內 |
| P1 - High | 🟡 | 影響重要功能, 需盡快解決 | 3 天內 |
| P2 - Medium | 🟢 | 影響次要功能, 可以延後 | 1 週內 |
| P3 - Low | ⚪ | 影響輕微, 可以跳過 | Sprint 結束前 |

### 問題狀態

| 狀態 | 符號 | 定義 |
|------|------|------|
| Open | 🔓 | 問題已記錄, 待處理 |
| In Progress | 🔄 | 問題正在解決中 |
| Blocked | ⏸️ | 問題因其他依賴而暫停 |
| Resolved | ✅ | 問題已解決, 待驗證 |
| Closed | 🔒 | 問題已驗證並關閉 |
| Deferred | 🗓️ | 問題延後到下個 Sprint |

### 問題記錄格式

每個問題包含:
- **ID**: 唯一標識 (I9-XXX)
- **標題**: 簡短描述
- **標籤**: 問題類型標籤
- **優先級**: P0 / P1 / P2 / P3
- **狀態**: Open / In Progress / Resolved / Closed
- **報告人**: 發現問題的人
- **負責人**: 負責解決問題的人
- **創建日期**: 問題創建時間
- **更新日期**: 最後更新時間
- **描述**: 詳細問題描述
- **重現步驟**: 如何重現問題 (Bug)
- **預期行為**: 應該如何運作
- **實際行為**: 當前如何運作
- **影響範圍**: 影響哪些模塊/功能
- **解決方案**: 提議的解決方法
- **驗證標準**: 如何驗證已解決

---

## 問題統計概覽

### 整體統計

| 統計項 | 數量 | 百分比 |
|--------|------|--------|
| 總問題數 | 0 | - |
| 開放問題 | 0 | 0% |
| 進行中問題 | 0 | 0% |
| 已解決問題 | 0 | 0% |
| 已關閉問題 | 0 | 0% |
| 延後問題 | 0 | 0% |

### 按優先級統計

| 優先級 | 開放 | 進行中 | 已解決 | 已關閉 | 總計 |
|--------|------|--------|--------|--------|------|
| P0 - Critical | 0 | 0 | 0 | 0 | 0 |
| P1 - High | 0 | 0 | 0 | 0 | 0 |
| P2 - Medium | 0 | 0 | 0 | 0 | 0 |
| P3 - Low | 0 | 0 | 0 | 0 | 0 |

### 按標籤統計

| 標籤 | 數量 | 百分比 |
|------|------|--------|
| 🐛 Bug | 0 | 0% |
| 🔥 Blocker | 0 | 0% |
| ⚠️ Warning | 0 | 0% |
| 💡 Enhancement | 0 | 0% |
| 📝 Documentation | 0 | 0% |
| 🧪 Testing | 0 | 0% |
| 🏗️ Architecture | 0 | 0% |

### 按負責人統計

| 負責人 | 開放 | 進行中 | 已解決 | 總計 |
|--------|------|--------|--------|------|
| Architect | 0 | 0 | 0 | 0 |
| Dev Lead | 0 | 0 | 0 | 0 |
| Backend Dev | 0 | 0 | 0 | 0 |
| QA Lead | 0 | 0 | 0 | 0 |

---

## Critical Issues (P0)

**定義**: 阻止 Sprint 完成, 影響核心功能, 需要立即解決 (24 小時內)

_目前沒有 P0 Critical Issues_

### P0 問題模板 (供參考)

---

#### I9-P0-001: [模板] Critical Issue 標題

**ID**: I9-P0-001
**標籤**: 🔥 Blocker, 🐛 Bug
**優先級**: 🔴 P0 - Critical
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 重現步驟
1. _待填寫_
2. _待填寫_
3. _待填寫_

##### 預期行為
_待填寫_

##### 實際行為
_待填寫_

##### 影響範圍
- **影響的模塊**: _待填寫_
- **影響的功能**: _待填寫_
- **阻礙的任務**: _待填寫_

##### 解決方案
_待提議_

##### 驗證標準
- [ ] _待填寫_
- [ ] _待填寫_

---

## High Priority Issues (P1)

**定義**: 影響重要功能, 需盡快解決 (3 天內)

_目前沒有 P1 High Priority Issues_

### P1 問題模板 (供參考)

---

#### I9-P1-001: [模板] High Priority Issue 標題

**ID**: I9-P1-001
**標籤**: 🐛 Bug
**優先級**: 🟡 P1 - High
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 重現步驟
1. _待填寫_
2. _待填寫_

##### 預期行為
_待填寫_

##### 實際行為
_待填寫_

##### 影響範圍
- **影響的模塊**: _待填寫_
- **影響的功能**: _待填寫_

##### 解決方案
_待提議_

##### 驗證標準
- [ ] _待填寫_

---

## Medium Priority Issues (P2)

**定義**: 影響次要功能, 可以延後 (1 週內)

_目前沒有 P2 Medium Priority Issues_

### P2 問題模板 (供參考)

---

#### I9-P2-001: [模板] Medium Priority Issue 標題

**ID**: I9-P2-001
**標籤**: 💡 Enhancement
**優先級**: 🟢 P2 - Medium
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 改進建議
_待提議_

##### 預期效益
_待評估_

##### 影響範圍
- **影響的模塊**: _待填寫_

##### 實施計劃
_待規劃_

##### 驗證標準
- [ ] _待填寫_

---

## Low Priority Issues (P3)

**定義**: 影響輕微, 可以跳過 (Sprint 結束前)

_目前沒有 P3 Low Priority Issues_

### P3 問題模板 (供參考)

---

#### I9-P3-001: [模板] Low Priority Issue 標題

**ID**: I9-P3-001
**標籤**: 📝 Documentation
**優先級**: ⚪ P3 - Low
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 改進建議
_待提議_

---

## 已解決問題

**定義**: 已解決並驗證的問題

_目前沒有已解決問題_

### 已解決問題模板 (供參考)

---

#### I9-RESOLVED-001: [模板] 已解決問題標題

**ID**: I9-RESOLVED-001
**標籤**: 🐛 Bug
**優先級**: 🟡 P1 - High
**狀態**: 🔒 Closed
**報告人**: _待填寫_
**負責人**: _待填寫_
**創建日期**: _待填寫_
**解決日期**: _待填寫_
**關閉日期**: _待填寫_

##### 描述
_待填寫_

##### 解決方案
_已實施_

##### 驗證結果
✅ 所有驗證標準已通過

##### 相關 Commit
- Commit: _待填寫_
- PR: _待填寫_

---

## 常見問題與解決方案

### 接口設計相關

---

#### FAQ-001: IWorkflowEngine 接口應該包含哪些方法?

**類別**: 🏗️ Architecture
**頻率**: 常見

##### 問題描述
在設計 IWorkflowEngine 接口時, 應該包含哪些核心方法? 是否需要支持單步執行?

##### 解決方案
**核心方法 (必須實現)**:
- `ExecuteWorkflowAsync()` - 執行完整工作流

**擴展方法 (可選實現)**:
- `ExecuteStepAsync()` - 單步執行 (用於調試)
- `GetExecutionStatusAsync()` - 查詢執行狀態 (用於長時間運行的工作流)

**設計建議**:
1. 定義 `IWorkflowEngine` 基礎接口 (僅 ExecuteWorkflowAsync)
2. 定義 `IAdvancedWorkflowEngine : IWorkflowEngine` 擴展接口
3. SemanticKernelWorkflowEngine 實現 IAdvancedWorkflowEngine
4. MockWorkflowEngine 僅實現 IWorkflowEngine (簡化實現)

##### 參考文檔
- ADR-011: Framework Abstraction Layer
- TD-070: IWorkflowEngine 接口設計決策

---

#### FAQ-002: 如何處理 Semantic Kernel 特定功能?

**類別**: 🏗️ Architecture
**頻率**: 常見

##### 問題描述
Semantic Kernel 提供了許多特定功能 (如 Planner, Memory, Plugins), 如何在抽象層中處理這些功能?

##### 解決方案
**原則**: 抽象層應該是框架無關的, 不暴露特定框架的功能

**處理方式**:
1. **核心功能**: 抽象到接口 (如 ITaskGenerator 抽象 Planner)
2. **框架特定功能**: 通過配置參數傳遞 (如 Plugins 通過 ExecutionContext.Config)
3. **不抽象**: 保留在 Adapter 內部 (如 Memory 通過 IStateManager 抽象)

**示例**:
```csharp
// 不要這樣做 (暴露 Semantic Kernel)
public interface IWorkflowEngine
{
    Task<WorkflowExecutionResult> ExecuteWithPlannerAsync(Planner planner);
}

// 應該這樣做 (框架無關)
public interface IWorkflowEngine
{
    Task<WorkflowExecutionResult> ExecuteWorkflowAsync(
        Guid workflowId,
        Dictionary<string, object>? parameters = null); // Planner 配置通過 parameters 傳遞
}
```

##### 參考文檔
- ADR-011: Framework Abstraction Layer
- SPRINT-9-3-CONTEXT.md: Interface Design Principles

---

### Prompt Engineering 相關

---

#### FAQ-003: 如何提高 TaskGenerator 生成質量?

**類別**: 💡 Enhancement
**頻率**: 常見

##### 問題描述
ITaskGenerator 生成的任務質量不穩定, 如何提高生成質量?

##### 解決方案
**Prompt 優化策略**:
1. **Few-shot Examples**: 提供 3-5 個高質量範例
2. **Output Format**: 明確定義 JSON Schema
3. **Constraints**: 明確約束條件 (maxTasks, minTasks, 依賴規則)
4. **Chain-of-Thought**: 對於複雜場景, 要求 LLM 輸出推理過程

**質量控制**:
1. **JSON Schema 驗證**: 生成後立即驗證格式
2. **依賴檢測**: 檢測循環依賴
3. **重試機制**: 格式不正確時重試 (最多 3 次)
4. **降級處理**: 重試失敗後返回簡化版任務 (單任務)

**評估方法**:
- 準備 10 個測試場景 (簡單到複雜)
- 人工評分 (1-5 分)
- 目標: 平均分 >4 分, 80% 場景 ≥4 分

##### 參考文檔
- TD-072: Prompt Engineering 策略決策
- SPRINT-9-3-CONTEXT.md: Prompt Engineering Best Practices

---

#### FAQ-004: Few-shot Examples 應該如何選擇?

**類別**: 💡 Enhancement
**頻率**: 較少

##### 問題描述
應該選擇哪些 Few-shot Examples 來訓練 TaskGenerator?

##### 解決方案
**選擇原則**:
1. **覆蓋多種場景**: 簡單 (2-3 tasks), 中等 (4-5 tasks), 複雜 (6+ tasks)
2. **覆蓋多種類型**: API 調用, 數據處理, 用戶交互, 條件分支
3. **高質量**: 每個範例都經過驗證, 任務定義清晰, 依賴關係正確

**範例結構**:
```json
{
  "examples": [
    {
      "scenario": "Simple: Email Notification Workflow",
      "goal": "Send email notification to user",
      "tasks": [
        {
          "TaskId": "T1",
          "Title": "Validate email address",
          "Type": "Data_Processing",
          "Priority": "High",
          "DependsOn": []
        },
        {
          "TaskId": "T2",
          "Title": "Send email via SendGrid API",
          "Type": "API_Call",
          "Priority": "High",
          "DependsOn": ["T1"]
        }
      ]
    }
  ]
}
```

**動態選擇**:
- 根據工作流複雜度動態選擇範例
- 簡單工作流 (1-3 steps): 選擇簡單範例
- 複雜工作流 (4+ steps): 選擇中等 + 複雜範例

##### 參考文檔
- TD-072: Prompt Engineering 策略決策
- prompts/examples.json (Few-shot Examples 文件)

---

### 測試相關

---

#### FAQ-005: Contract Tests 應該測試什麼?

**類別**: 🧪 Testing
**頻率**: 常見

##### 問題描述
Contract Tests 應該測試哪些內容? 與單元測試有何區別?

##### 解決方案
**Contract Tests 目標**: 驗證接口實現是否符合接口契約

**應該測試**:
1. **基本功能**: 接口方法能否正常執行 (Happy Path)
2. **錯誤處理**: 異常情況下是否拋出正確的異常
3. **參數驗證**: 非法參數是否被拒絕
4. **返回值驗證**: 返回值是否符合接口定義 (非空, 類型正確)

**不應該測試**:
1. **實現細節**: 不測試具體如何實現
2. **性能**: 不測試執行速度 (另有性能測試)
3. **外部依賴**: 不測試真實 LLM API (使用 Mock)

**示例**:
```csharp
[Fact]
public async Task ExecuteWorkflowAsync_ValidWorkflow_ReturnsSuccessResult()
{
    // Arrange
    var engine = CreateWorkflowEngine(); // Can be SemanticKernelWorkflowEngine or MockWorkflowEngine
    var workflowId = Guid.NewGuid();

    // Act
    var result = await engine.ExecuteWorkflowAsync(workflowId, "test input");

    // Assert
    Assert.NotNull(result);
    Assert.Equal(workflowId, result.WorkflowId);
    Assert.NotNull(result.ExecutionId);
}
```

**運行策略**:
- 對每個接口實現運行相同的 Contract Tests
- SemanticKernelWorkflowEngine 應該通過所有 Contract Tests
- MockWorkflowEngine 也應該通過所有 Contract Tests

##### 參考文檔
- SPRINT-9-4-CHECKLIST.md: Contract Tests 任務清單
- tests/ContractTests/ (Contract Tests 代碼)

---

#### FAQ-006: 如何測試 MockWorkflowEngine?

**類別**: 🧪 Testing
**頻率**: 較少

##### 問題描述
MockWorkflowEngine 是 Mock 實現, 是否還需要測試?

##### 解決方案
**是的, 需要測試**, 原因:
1. MockWorkflowEngine 也需要實現 IWorkflowEngine 接口契約
2. 需要驗證可配置行為 (Success, Failure, Timeout) 是否正常工作
3. 需要驗證統計功能 (ExecutionCount, AverageExecutionTime) 是否準確

**測試策略**:
1. **Contract Tests**: 運行所有 IWorkflowEngine Contract Tests (驗證接口契約)
2. **行為測試**: 測試 SetBehavior() 是否生效
3. **統計測試**: 測試 GetStatistics() 是否準確

**示例**:
```csharp
[Fact]
public async Task MockWorkflowEngine_SetBehaviorToFailure_ThrowsException()
{
    // Arrange
    var engine = new MockWorkflowEngine();
    var workflowId = Guid.NewGuid();
    engine.SetBehavior(workflowId, MockBehavior.Failure);

    // Act & Assert
    await Assert.ThrowsAsync<ExecutionFailedException>(
        () => engine.ExecuteWorkflowAsync(workflowId, "test"));
}
```

##### 參考文檔
- TD-073: MockWorkflowEngine 實作決策
- SPRINT-9-4-CHECKLIST.md: T9.111 - T9.113

---

### 性能相關

---

#### FAQ-007: Workflow 執行時間目標是多少?

**類別**: ⚠️ Warning
**頻率**: 較少

##### 問題描述
Workflow 執行時間的性能目標是多少? 如何優化?

##### 解決方案
**性能目標** (來自 ADR-011):
- 簡單工作流 (2-3 steps): P95 < 2s
- 一般工作流 (4-5 steps): P95 < 5s
- 複雜工作流 (6+ steps): P95 < 10s

**優化策略**:
1. **並行執行**: 使用 Parallel Coordination 策略並行執行無依賴任務
2. **緩存**: 使用 IStateManager 緩存中間結果 (Redis)
3. **批量調用**: 批量調用 LLM API (減少 HTTP 開銷)
4. **超時控制**: 設置合理的 Timeout (避免長時間阻塞)

**性能測試**:
```csharp
[Fact]
public async Task ExecuteWorkflowAsync_SimpleWorkflow_CompletesWithin2Seconds()
{
    // Arrange
    var engine = CreateWorkflowEngine();
    var workflowId = CreateSimpleWorkflow(); // 2-3 steps
    var stopwatch = Stopwatch.StartNew();

    // Act
    var result = await engine.ExecuteWorkflowAsync(workflowId, "test");

    // Assert
    stopwatch.Stop();
    Assert.True(stopwatch.ElapsedMilliseconds < 2000,
        $"Expected < 2000ms, but took {stopwatch.ElapsedMilliseconds}ms");
}
```

##### 參考文檔
- ADR-011: Framework Abstraction Layer (Performance Requirements)
- SPRINT-9-4-CHECKLIST.md: T9.127 - T9.128 (Performance Tests)

---

### Feature Flags 相關

---

#### FAQ-008: 如何動態切換 Adapter?

**類別**: 🏗️ Architecture
**頻率**: 較少

##### 問題描述
在生產環境中, 如何動態切換 SemanticKernelAdapter 和 MockAdapter?

##### 解決方案
**方式 1: 修改 appsettings.json (需重啟)**
```json
{
  "FeatureManagement": {
    "UseSemanticKernelAdapter": {
      "EnabledFor": [
        {
          "Name": "Microsoft.Percentage",
          "Parameters": { "Value": 100 }
        }
      ]
    }
  }
}
```

**方式 2: 使用 Azure App Configuration (動態, 無需重啟)**
1. 在 Azure Portal 創建 App Configuration
2. 配置 Feature Flags
3. 應用程序連接到 App Configuration
4. 使用 `IFeatureManager.IsEnabledAsync()` 動態檢查

**方式 3: 灰度發布 (基於 Percentage Filter)**
```json
{
  "FeatureManagement": {
    "UseSemanticKernelAdapter": {
      "EnabledFor": [
        {
          "Name": "Microsoft.Percentage",
          "Parameters": { "Value": 50 }
        }
      ]
    }
  }
}
```
- 50% 流量使用 SemanticKernelAdapter
- 50% 流量使用 MockAdapter
- 逐步提高百分比 (10% → 50% → 100%)

##### 參考文檔
- TD-074: Feature Flags 選型決策
- ADR-013: Adapter Selection Strategy (待創建)

---

## 技術債務追蹤

### 技術債務定義

**技術債務**: 為了快速交付功能而採取的權宜之計, 未來需要重構或改進的地方

### 債務分類

| 類別 | 定義 | 處理策略 |
|------|------|----------|
| 代碼質量 | 重複代碼, 複雜度高, 可讀性差 | 定期重構 |
| 架構設計 | 設計不合理, 耦合度高 | 下個 Sprint 改進 |
| 性能問題 | 性能不達標, 需要優化 | 性能測試後優化 |
| 文檔缺失 | 缺少文檔, 註解不完整 | Sprint 結束前補齊 |
| 測試覆蓋 | 測試覆蓋率不足 | 逐步提高覆蓋率 |

### 技術債務清單

---

#### TD9-001: [預留位置] 技術債務標題

**ID**: TD9-001
**類別**: _待分類_
**嚴重程度**: _待評估_ (High / Medium / Low)
**狀態**: 🔓 Open
**記錄人**: _待填寫_
**創建日期**: _待填寫_

##### 描述
_待填寫_

##### 影響
_待評估_

##### 建議解決方案
_待提議_

##### 預計解決時間
_待估算_ (Story Points)

##### 計劃解決時間
_待規劃_ (Sprint XX)

---

#### TD9-002: [預留位置] 技術債務標題

**ID**: TD9-002
**類別**: _待分類_
**嚴重程度**: _待評估_
**狀態**: 🔓 Open
**記錄人**: _待填寫_
**創建日期**: _待填寫_

##### 描述
_待填寫_

##### 影響
_待評估_

##### 建議解決方案
_待提議_

##### 預計解決時間
_待估算_

##### 計劃解決時間
_待規劃_

---

#### TD9-003: [預留位置] 技術債務標題

**ID**: TD9-003
**類別**: _待分類_
**嚴重程度**: _待評估_
**狀態**: 🔓 Open
**記錄人**: _待填寫_
**創建日期**: _待填寫_

##### 描述
_待填寫_

##### 影響
_待評估_

##### 建議解決方案
_待提議_

##### 預計解決時間
_待估算_

##### 計劃解決時間
_待規劃_

---

### 債務統計

| 類別 | High | Medium | Low | 總計 |
|------|------|--------|-----|------|
| 代碼質量 | 0 | 0 | 0 | 0 |
| 架構設計 | 0 | 0 | 0 | 0 |
| 性能問題 | 0 | 0 | 0 | 0 |
| 文檔缺失 | 0 | 0 | 0 | 0 |
| 測試覆蓋 | 0 | 0 | 0 | 0 |
| **總計** | **0** | **0** | **0** | **0** |

---

## Sprint 9 問題總結

### 問題趨勢分析
_Sprint 結束時填寫_

### 高風險領域
_Sprint 結束時識別_

### 改進建議
_Sprint Retrospective 時提出_

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**總問題數**: 0
**總技術債務**: 0
**FAQ 數量**: 8 個
