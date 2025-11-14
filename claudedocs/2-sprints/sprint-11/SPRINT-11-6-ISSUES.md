# SPRINT 11 - 問題追蹤 (Issue Tracking)

---

## 📋 文件資訊 (Document Information)

| 欄位 | 內容 |
|------|------|
| **版本** | v2.1 |
| **Sprint 編號** | Sprint 11 |
| **Sprint 週期** | Week 31-33 (3 週) |
| **Phase** | Phase 1C - 工作流編輯器 Backend (Workflow Editor Backend) |
| **計劃日期** | 2026-05-12 ~ 2026-06-01 |
| **狀態** | 📋 計劃階段 (Planned) |
| **創建日期** | 2025-11-14 |
| **最後更新** | 2025-11-14 |

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

### 按優先級統計

| 優先級 | 開放 | 進行中 | 已解決 | 已關閉 | 總計 |
|--------|------|--------|--------|--------|------|
| P0 - Critical | 0 | 0 | 0 | 0 | 0 |
| P1 - High | 0 | 0 | 0 | 0 | 0 |
| P2 - Medium | 0 | 0 | 0 | 0 | 0 |
| P3 - Low | 0 | 0 | 0 | 0 | 0 |

---

## Critical Issues (P0)

_目前沒有 P0 Critical Issues_

---

## High Priority Issues (P1)

_目前沒有 P1 High Priority Issues_

---

## Medium Priority Issues (P2)

_目前沒有 P2 Medium Priority Issues_

---

## Low Priority Issues (P3)

_目前沒有 P3 Low Priority Issues_

---

## 已解決問題

_目前沒有已解決問題_

---

## 常見問題與解決方案

### Execution Trigger 相關

---

#### FAQ-011: 如何設計統一的 Trigger 抽象?

**類別**: 🏗️ Architecture
**頻率**: 常見

##### 問題描述
4 種 Trigger types (Manual, Schedule, Event, Webhook) 需要統一的抽象設計

##### 解決方案
**使用 TriggerType enum + TriggerMetadata JSON**

```csharp
public enum TriggerType
{
    Manual = 0,
    Schedule = 1,
    Event = 2,
    Webhook = 3
}

public class WorkflowExecution
{
    public TriggerType TriggerType { get; private set; }
    public string TriggerMetadata { get; private set; } // JSON (包含 trigger 詳細資訊)
}
```

**優點**:
- ✅ 統一的數據結構
- ✅ TriggerMetadata 靈活存儲不同 Trigger 的詳細資訊
- ✅ 易於擴展新的 Trigger types

---

#### FAQ-012: Cron Scheduling 如何確保可靠性?

**類別**: ⚠️ Warning
**頻率**: 常見

##### 問題描述
BackgroundService 每分鐘檢查一次, 如何確保不會遺漏 Schedule?

##### 解決方案
**使用 NextExecutionTime 精確匹配**

```csharp
private async Task CheckAndTriggerScheduledWorkflowsAsync()
{
    var now = DateTime.UtcNow;
    var dueSchedules = await _repository.GetDueSchedulesAsync(now);

    foreach (var schedule in dueSchedules)
    {
        await TriggerWorkflowAsync(schedule);
        schedule.UpdateNextExecutionTime(CronHelper.GetNextOccurrence(schedule.CronExpression));
    }
}
```

**風險緩解**:
- ⚠️ 若 BackgroundService 停止, Schedule 可能遺漏
- 🎯 解決: 使用 Hangfire (分佈式 scheduling) 或記錄遺漏的 Schedule

---

### Template Management 相關

---

#### FAQ-013: Template Import/Export 如何處理版本兼容?

**類別**: 🏗️ Architecture
**頻率**: 較少

##### 問題描述
不同版本的 Template Export 格式如何相互兼容?

##### 解決方案
**使用 JSON Schema versioning**

```json
{
  "version": "1.0",
  "templateMetadata": { ... },
  "workflowDefinition": { ... }
}
```

**版本策略**:
- v1.0: 基礎格式
- v1.1: 向後兼容, 增加可選欄位
- v2.0: 破壞性變更, 需要 Migration

**Import Validation**:
```csharp
var exportData = JsonSerializer.Deserialize<TemplateExportData>(jsonContent);
if (exportData.Version != "1.0")
{
    throw new UnsupportedTemplateVersionException(exportData.Version);
}
```

---

#### FAQ-014: Marketplace Ranking 如何避免操縱?

**類別**: 💡 Enhancement
**頻率**: 較少

##### 問題描述
如何防止惡意刷 Rating 或 Usage count 來提升排名?

##### 解決方案
**限制 + 驗證**

1. **Rating 限制**:
   - 每個用戶只能對同一個 Template Rating 一次
   - 驗證用戶確實使用過 Template 才能 Rating

2. **Usage Tracking**:
   - 追蹤真實使用次數, 而非單純計數
   - 過濾異常高頻使用 (anti-spam)

3. **Weighted Ranking**:
   - 使用 Weighted average (Rating × 0.7 + Usage × 0.3)
   - 降低單一指標操縱影響

---

### Version Control 相關

---

#### FAQ-015: Workflow Versioning 如何處理大量版本?

**類別**: ⚠️ Warning
**頻率**: 常見

##### 問題描述
每次更新創建新 row, 可能產生大量版本, 如何管理?

##### 解決方案
**版本清理策略**

1. **保留策略**:
   - 保留最近 10 個版本
   - 保留所有 Active 版本
   - 刪除舊的 Inactive 版本 (Soft delete)

2. **版本壓縮**:
   - 定期壓縮舊版本 (合併為 snapshot)
   - 僅保留 DefinitionJson, 刪除 Nodes/Edges 詳細數據

3. **Archive 機制**:
   - 超過 30 天的舊版本移至 Archive 表
   - 減少主表查詢負擔

**實施時機**: Phase 2 (當版本數量 >1000)

---

#### FAQ-016: Version Rollback 如何確保安全?

**類別**: 🐛 Bug
**頻率**: 較少

##### 問題描述
Rollback 可能導致數據不一致或執行中的 Workflow 失敗

##### 解決方案
**Rollback Validation + Confirmation**

1. **Validation**:
   - 檢查目標版本是否有循環依賴
   - 檢查目標版本 Node types 是否仍然存在
   - 檢查是否有正在執行的 Workflow

2. **Confirmation**:
   - 提供 Rollback Preview (Diff)
   - 需要用戶明確確認 (confirmRollback=true)

3. **Rollback History**:
   - 記錄 Rollback 操作 (from Version X to Version Y)
   - 支援 Rollback 的 Rollback (恢復)

---

## 技術債務追蹤

### 技術債務清單

---

#### TD11-001: BackgroundService 單實例限制

**ID**: TD11-001
**類別**: 架構設計
**嚴重程度**: Medium
**狀態**: 🔓 Open

##### 描述
WorkflowSchedulerService 使用 BackgroundService, 僅適用於單實例部署

##### 影響
若部署多實例, Schedule 可能重複觸發或遺漏

##### 建議解決方案
遷移到 Hangfire (分佈式 scheduling)

##### 預計解決時間
2 SP

##### 計劃解決時間
Phase 2 (Sprint 15+)

---

#### TD11-002: InMemoryEventBus 無持久化

**ID**: TD11-002
**類別**: 架構設計
**嚴重程度**: Low
**狀態**: 🔓 Open

##### 描述
InMemoryEventBus 無持久化, 應用重啟後事件遺失

##### 影響
Event Trigger 可能遺失事件

##### 建議解決方案
遷移到 MassTransit + RabbitMQ

##### 預計解決時間
3 SP

##### 計劃解決時間
Phase 2 (Sprint 16+)

---

#### TD11-003: Template Version Management 未實現

**ID**: TD11-003
**類別**: 功能缺失
**嚴重程度**: Low
**狀態**: 🔓 Open

##### 描述
Template 本身沒有版本控制, 僅 Workflow Definition 有版本控制

##### 影響
Template 更新後, 使用舊 Template 創建的 Workflow 無法追溯

##### 建議解決方案
為 Template 增加版本控制機制

##### 預計解決時間
2 SP

##### 計劃解決時間
Sprint 12 或 Phase 2

---

### 債務統計

| 類別 | High | Medium | Low | 總計 |
|------|------|--------|-----|------|
| 架構設計 | 0 | 1 | 1 | 2 |
| 功能缺失 | 0 | 0 | 1 | 1 |
| **總計** | **0** | **1** | **2** | **3** |

---

## Sprint 11 問題總結

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
**總技術債務**: 3
**FAQ 數量**: 6 個 (FAQ-011 到 FAQ-016)
