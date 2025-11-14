# SPRINT-2-6-ISSUES.md - Sprint 2 問題追蹤

**版本**: v2.1
**Sprint 編號**: Sprint 2
**Sprint 週期**: Week 4-6 (3 週)
**Phase**: Phase 1B - Agent 執行引擎與 Plugin 系統
**計劃日期**: 2025-11-11 ~ 2025-12-01
**實際日期**: 2025-11-25 ~ 2025-12-21 (延遲 6 days)
**狀態**: 🔄 進行中 (2 待處理問題, 2 已解決, 4 範圍變更)
**創建日期**: 2025-11-25
**最後更新**: 2025-12-13

---

## 📑 目錄 (Table of Contents)

1. [使用指南](#-使用指南)
2. [問題概覽](#-問題概覽)
3. [問題統計](#-問題統計)
4. [技術問題 (Technical Issues)](#-技術問題-technical-issues)
   - [ISSUE-001: Migration 執行時 Index 創建順序錯誤](#issue-001-migration-執行時-index-創建順序錯誤)
   - [ISSUE-002: CSV 匯出中文字元編碼錯誤](#issue-002-csv-匯出中文字元編碼錯誤)
5. [範圍變更 (Scope Changes)](#-範圍變更-scope-changes)
   - [CHANGE-001: US 1.4 範圍擴大](#change-001-us-14-範圍擴大-5-sp--13-sp)
   - [CHANGE-002: US 2.1 自然延伸至 US 2.2/2.3](#change-002-us-21-自然延伸至-us-2223-phase-1-2)
   - [CHANGE-003: US 6.1 時間壓縮](#change-003-us-61-時間壓縮)
   - [CHANGE-004: US 2.2/2.3 剩餘工作](#change-004-us-2223-剩餘工作-phase-3-5)
6. [待處理問題 (Open Issues)](#-待處理問題-open-issues)
   - [ISSUE-003: Percentile 計算效能問題](#issue-003-percentile-計算效能問題-潛在風險)
   - [ISSUE-004: Frontend SignalR 斷線重連機制待完善](#issue-004-frontend-signalr-斷線重連機制待完善)
7. [經驗教訓總結](#-經驗教訓總結)
8. [監控指標](#-監控指標)
9. [完整參考文獻索引 (50+ 文檔)](#-完整參考文獻索引-50-文檔)
10. [版本歷史](#-版本歷史)

---

## 📖 使用指南

### 本文檔適用對象
- **開發團隊**: 追蹤和解決 Sprint 2 期間的所有技術問題和範圍變更
- **項目經理**: 評估範圍變更對 Sprint 進度的影響，管理項目風險
- **QA 團隊**: 了解已知問題和待處理風險
- **Product Owner**: 理解範圍變更的業務影響和技術決策
- **未來參考**: 為類似問題提供解決方案參考和風險管理經驗

### 如何使用本文檔
本文檔系統化追蹤 Sprint 2 期間的所有問題和範圍變更：

#### 📋 問題追蹤使用場景
- **快速查找問題**: 使用目錄或搜索功能定位特定問題（ISSUE-001 ~ ISSUE-004）
- **了解解決方案**: 每個問題包含詳細的根本原因分析和解決方案代碼
- **範圍變更影響分析**: 查看 CHANGE-001 ~ CHANGE-004 了解 Sprint 2 延遲原因
- **風險監控**: 檢查待處理問題 (ISSUE-003, ISSUE-004) 的優先級和緩解措施

#### 🎯 快速導航指南

**按問題類型查找**:
- **已解決的技術問題**: [ISSUE-001 Migration 順序](#issue-001-migration-執行時-index-創建順序錯誤), [ISSUE-002 CSV 編碼](#issue-002-csv-匯出中文字元編碼錯誤)
- **範圍變更**: [CHANGE-001 US 1.4 擴大](#change-001-us-14-範圍擴大-5-sp--13-sp), [CHANGE-002 US 2.1 延伸](#change-002-us-21-自然延伸至-us-2223-phase-1-2)
- **待處理風險**: [ISSUE-003 效能問題](#issue-003-percentile-計算效能問題-潛在風險), [ISSUE-004 SignalR 重連](#issue-004-frontend-signalr-斷線重連機制待完善)

**按影響範圍查找**:
- **US 1.4 相關**: ISSUE-001 (Migration), ISSUE-002 (CSV Export), CHANGE-001 (範圍擴大), ISSUE-003 (Percentile 效能), ISSUE-004 (SignalR 重連)
- **US 2.1 相關**: CHANGE-002 (自然延伸至 US 2.2/2.3)
- **US 6.1 相關**: CHANGE-003 (時間壓縮)

**按緊急程度查找**:
- **P1 (High)**: ISSUE-002 (CSV 編碼錯誤) - 已解決
- **P2 (Medium)**: ISSUE-001 (Migration 順序), ISSUE-004 (SignalR 重連) - 1 已解決, 1 進行中
- **P3 (Low)**: ISSUE-003 (Percentile 效能) - 監控中

### 文檔更新規範
- 本文檔為 Sprint 2 的**問題追蹤與範圍變更管理系統**
- 當前版本: **v2.1** (2025-12-13)
- 如需更新，請遵循 [Documentation Standards](../../../docs/development/Documentation-Standards.md)
- 版本變更記錄於 [版本歷史](#-版本歷史)

### 相關 Sprint 2 文檔系列
完整了解 Sprint 2，建議依序閱讀：
1. [SPRINT-2-1-OVERVIEW.md](./SPRINT-2-1-OVERVIEW.md) - Sprint 2 概覽與完成報告
2. [SPRINT-2-2-PLAN.md](./SPRINT-2-2-PLAN.md) - 詳細技術規格與實施計劃
3. [SPRINT-2-3-CONTEXT.md](./SPRINT-2-3-CONTEXT.md) - Sprint 背景與技術決策
4. [SPRINT-2-4-CHECKLIST.md](./SPRINT-2-4-CHECKLIST.md) - 檢查清單與進度追蹤
5. [SPRINT-2-5-DEV-LOG.md](./SPRINT-2-5-DEV-LOG.md) - 開發日誌與技術記錄
6. [SPRINT-2-6-ISSUES.md](./SPRINT-2-6-ISSUES.md) - 問題追蹤與解決方案 **(當前文檔)**
7. [SPRINT-2-7-RETROSPECTIVE.md](./SPRINT-2-7-RETROSPECTIVE.md) - 回顧與改進建議

---

## 📊 問題概覽

**Sprint 2 問題總覽**: 8 個問題 (2 已解決, 4 範圍變更, 2 待處理)

---

## 📊 問題統計

| 類別 | 數量 | 狀態 |
|------|------|------|
| 🐛 Technical Bug | 2 | ✅ 已解決 |
| 📦 範圍變更 (Scope Change) | 4 | ✅ 已處理 |
| ⚠️ 待處理問題 | 2 | 🔄 進行中 |
| **總計** | **8** | - |

---

## 🐛 技術問題 (Technical Issues)

### ISSUE-001: Migration 執行時 Index 創建順序錯誤

**日期**: 2025-11-26 (Day 2)
**優先級**: P2 (Medium)
**狀態**: ✅ 已解決
**影響範圍**: US 1.4 Phase 1 - Infrastructure Layer

#### 問題描述
執行 EF Core Migration `20251126_AddAgentExecutionTable` 時，Index 創建失敗。
錯誤訊息:
```
ERROR: relation "agent_executions" does not exist
```

#### 根本原因
Migration Up() 方法中，Index 創建語句在 CreateTable 之前執行。

**錯誤代碼**:
```csharp
// Migration Up() - 錯誤順序
public override void Up(MigrationBuilder migrationBuilder)
{
    // ❌ 錯誤: Index 在表創建之前
    migrationBuilder.CreateIndex(
        name: "idx_agent_executions_agent_id",
        table: "agent_executions",
        column: "agent_id");

    migrationBuilder.CreateTable(
        name: "agent_executions",
        columns: table => new { ... });
}
```

#### 解決方案
調整 Migration Up() 方法中的語句順序：CreateTable → CreateIndex。

**修正代碼**:
```csharp
// Migration Up() - 正確順序
public override void Up(MigrationBuilder migrationBuilder)
{
    // ✅ 正確: 先創建表
    migrationBuilder.CreateTable(
        name: "agent_executions",
        columns: table => new { ... });

    // ✅ 正確: 再創建 Index
    migrationBuilder.CreateIndex(
        name: "idx_agent_executions_agent_id",
        table: "agent_executions",
        column: "agent_id");
}
```

#### 影響
- **時間損失**: 30 分鐘（排查 + 修復）
- **延遲**: 無（當日完成修復）

#### 預防措施
- Migration 代碼 Review 檢查清單：驗證 CreateTable → CreateIndex 順序
- 使用 EF Core 自動生成 Migration 時，檢查生成的代碼順序

#### 相關 Commits
- `8b9c0d2` - fix(migration): correct index creation order

---

### ISSUE-002: CSV 匯出中文字元編碼錯誤

**日期**: 2025-12-04 (Day 10)
**優先級**: P1 (High)
**狀態**: ✅ 已解決
**影響範圍**: US 1.4 Phase 4 - Export Functionality

#### 問題描述
使用 CsvHelper 匯出執行歷史到 CSV 檔案時，中文字元顯示為亂碼。

**範例**:
```
# 預期輸出:
UserInput,Response
你好,您好！我能幫您什麼?

# 實際輸出（亂碼）:
UserInput,Response
ä½ å¥½,æ¨å¥½ï¼æèƒ½å¸®æ¨ä»€ä¹ˆ?
```

#### 根本原因
CsvHelper 預設使用 UTF-8 encoding **不含 BOM**，Excel 無法正確識別 UTF-8 編碼。

#### 解決方案
使用 UTF-8 **with BOM** encoding (`new UTF8Encoding(true)`).

**錯誤代碼**:
```csharp
// ❌ 錯誤: UTF-8 without BOM
using var writer = new StreamWriter(memoryStream, Encoding.UTF8);
using var csv = new CsvWriter(writer, CultureInfo.InvariantCulture);
```

**修正代碼**:
```csharp
// ✅ 正確: UTF-8 with BOM
using var writer = new StreamWriter(memoryStream, new UTF8Encoding(true));
using var csv = new CsvWriter(writer, CultureInfo.InvariantCulture);
```

#### 影響
- **時間損失**: 1 小時（排查 + 測試多種 encoding）
- **延遲**: 無（當日完成修復）

#### 技術學習
- **UTF-8 BOM (Byte Order Mark)**: `EF BB BF`，幫助應用程式識別 UTF-8 編碼
- **Excel CSV Import**: Excel 依賴 BOM 判斷 UTF-8，否則預設使用 ANSI

#### 預防措施
- 文件匯出功能測試清單：測試中文、日文、特殊字元
- CsvHelper 使用指南文檔更新：強制使用 UTF-8 BOM

#### 相關 Commits
- `5b6c7d9` - fix(export): use UTF-8 BOM encoding for CSV

---

## 📦 範圍變更 (Scope Changes)

### CHANGE-001: US 1.4 範圍擴大 (5 SP → 13 SP)

**日期**: 2025-11-25 ~ 2025-12-07
**狀態**: ✅ 已完成
**類型**: ⚠️ 負面影響（延遲 Sprint）

#### 變更描述
US 1.4 "Agent 執行與監控" 原本估計 **5 SP**，實際完成 **13 SP** (+8 SP, +160%)。

#### 根本原因
1. **監控需求被低估**:
   - SignalR WebSocket 整合複雜度高於預期
   - 群組管理、訂閱機制、錯誤處理需要額外時間
   - Frontend SignalR 整合與狀態管理複雜

2. **PO 中途新增需求**:
   - Phase 3 新增匯出功能 (CSV/JSON)，原計劃未包含
   - 統計指標從基礎 3 個擴展到 10+ 個（P95/P99 等）

3. **技術債務**:
   - 第一次使用 Semantic Kernel，學習曲線
   - 第一次使用 SignalR 在 React 中，調試耗時

#### 影響
- **Sprint 2 延遲**: +6 days (21 days → 27 days)
- **團隊士氣**: 加班頻繁 (Day 6, 7, 13, 14)
- **後續 User Stories 壓縮**: US 6.1 時間減少

#### 經驗教訓
1. **估計改進**:
   - 第三方整合（SignalR, Semantic Kernel）應乘以 1.5x 係數
   - P95/P99 等進階統計功能應單獨估計
   - 匯出功能應在 Sprint Planning 時明確討論

2. **需求管理**:
   - Sprint 中途新增需求應走正式變更流程（Change Request）
   - PO 應在 Sprint Planning 時明確所有驗收標準

3. **風險管理**:
   - 第一次使用的技術應保留 Buffer Time (20%)
   - 複雜整合應提前 Spike 技術可行性

#### 正面結果
- US 1.4 功能完整且強大
- SignalR 即時監控獲得 PO 高度讚賞
- 匯出功能超越競品（支援 CSV + JSON）

#### 相關文檔
- [CHANGE-LOG.md](../../4-changes/CHANGE-LOG.md) - CHANGE-001 詳細記錄

---

### CHANGE-002: US 2.1 自然延伸至 US 2.2/2.3 Phase 1-2

**日期**: 2025-12-10 (Day 16)
**狀態**: ✅ 已完成
**類型**: ✅ 正面影響（提升效率）

#### 變更描述
US 2.1 實作過程中，發現 `PluginLoader` 已經支援 Unload/Reload 機制，順便完成 US 2.2/2.3 的 Phase 1-2 Commands。

#### 完成內容
**US 2.2 (Plugin 熱重載) Phase 1-2**:
- ✅ `ReloadPluginCommand` + `ReloadPluginCommandHandler`
- ✅ `SwitchPluginVersionCommand` + `SwitchPluginVersionCommandHandler`
- ✅ 單元測試

**US 2.3 (Plugin 版本管理) Phase 1-2**:
- ✅ `ComparePluginVersionsQuery` + `ComparePluginVersionsQueryHandler`
- ✅ `PluginVersionComparisonDto`
- ✅ 單元測試

#### 原因
- `PluginLoader` 使用 `AssemblyLoadContext` 設計時已考慮 `Unload()` 方法
- Commands 實作成本低（複用已有邏輯）
- 團隊對 Plugin 系統理解深入，順手完成

#### 影響
- **減少未來工作量**: US 2.2/2.3 各減少 2-3 days (合計 4-6 days)
- **提升 Sprint 效率**: 無額外時間成本
- **風險**: US 2.2/2.3 剩餘 Phase 3-5 (API + Frontend) 仍需完成

#### 經驗教訓
1. **設計前瞻性**:
   - 良好的架構設計可以降低未來擴展成本
   - `AssemblyLoadContext` 選擇正確

2. **自然延伸識別**:
   - 開發過程中主動識別"順手可完成"的相關功能
   - 與 PO 溝通後即時完成，避免重複開發

#### 正面結果
- US 2.2/2.3 進度提前
- Sprint 2 剩餘工作量減少
- 團隊信心提升

#### 相關文檔
- [CHANGE-LOG.md](../../4-changes/CHANGE-LOG.md) - CHANGE-002 詳細記錄

---

### CHANGE-003: US 6.1 時間壓縮

**日期**: 2025-12-12 ~ 2025-12-15 (預計)
**狀態**: 🔄 計劃中
**類型**: ⚠️ 風險項

#### 變更描述
由於 CHANGE-001 導致 Sprint 2 延遲 6 天，US 6.1 "基礎聊天介面" 可用時間從原計劃 5 days 壓縮到 4 days。

#### 影響
- **原計劃**: 5 days (2025-12-11 ~ 12-15)
- **實際可用**: 4 days (2025-12-12 ~ 12-15)
- **風險**: 時間緊迫，可能需要縮減範圍

#### 緩解措施
1. **MVP 範圍確認**:
   - 與 PO 重新確認 US 6.1 核心功能
   - 非必要功能延後到 Sprint 3

2. **複用已有組件**:
   - SignalR 連接邏輯複用 US 1.4
   - UI 組件庫 (Material-UI) 使用標準組件

3. **並行開發**:
   - Backend API (Day 12) 與 Frontend UI (Day 13-14) 並行

#### 監控指標
- 每日進度檢查（Daily Standup）
- 如 Day 13 進度 <50%，立即啟動風險應對

---

### CHANGE-004: US 2.2/2.3 剩餘工作 (Phase 3-5)

**日期**: 2025-12-16 ~ 2025-12-17 (預計)
**狀態**: 🔄 計劃中
**類型**: ✅ 可控風險

#### 變更描述
US 2.2/2.3 Phase 1-2 已完成 (CHANGE-002)，剩餘 Phase 3-5 (API + Frontend) 需要完成。

#### 剩餘工作
**US 2.2 Phase 3-5** (預計 1.5 days):
- API 端點: `POST /api/v1/plugin-versions/{id}/reload`
- API 端點: `POST /api/v1/plugin-versions/{id}/switch-version`
- Frontend: PluginCard, ReloadButton, VersionSwitcher
- 測試

**US 2.3 Phase 3-5** (預計 1 day):
- API 端點: `GET /api/v1/plugin-versions/{pluginId}/compare`
- API 端點: `POST /api/v1/plugin-versions/{id}/rollback`
- Frontend: VersionHistory, VersionComparison, RollbackButton
- 測試

#### 風險評估
- **風險等級**: 低
- **原因**: Commands 已完成，API + Frontend 工作量可控
- **時間緩衝**: 2.5 days 計劃，實際預留 3 days

---

## ⚠️ 待處理問題 (Open Issues)

### ISSUE-003: Percentile 計算效能問題 (潛在風險)

**日期**: 2025-12-01 (Day 7, 發現)
**優先級**: P3 (Low, 未來優化)
**狀態**: 🔄 已知，待優化
**影響範圍**: US 1.4 Phase 3 - Statistics

#### 問題描述
`GetAgentStatisticsQueryHandler` 中的 P95/P99 Percentile 計算使用記憶體內排序（LINQ OrderBy）。

**當前實作**:
```csharp
// ⚠️ 記憶體內排序
var sortedResponseTimes = executions
    .Select(e => e.ResponseTimeMs)
    .OrderBy(x => x)
    .ToList();

// P95 計算
var p95Index = (int)(sortedResponseTimes.Count * 0.95);
var p95 = sortedResponseTimes[p95Index];
```

#### 潛在問題
- **大數據量** (>10,000 executions): 記憶體消耗高，排序慢
- **效能瓶頸**: 每次查詢都需要完整排序

#### 影響
- **當前**: 無影響（測試數據 <1000 executions）
- **未來**: Production 環境執行歷史累積後可能有效能問題

#### 建議解決方案
**Option 1: 資料庫計算 Percentile**
```sql
SELECT PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY response_time_ms) AS p95
FROM agent_executions
WHERE agent_id = @agentId;
```
- 優點: 資料庫高效，無記憶體壓力
- 缺點: PostgreSQL percentile_cont 語法複雜，EF Core LINQ 不支援

**Option 2: 採樣統計**
```csharp
// 僅取最近 1000 筆執行記錄計算 Percentile
var recentExecutions = executions
    .OrderByDescending(e => e.CreatedAt)
    .Take(1000)
    .ToList();
```
- 優點: 簡單，效能可控
- 缺點: 統計不完整（但 P95/P99 通常用於趨勢分析，採樣可接受）

**Option 3: 預計算 + 快取**
- 每日凌晨預計算前一天的 P95/P99
- 儲存到 `agent_daily_statistics` 表
- API 查詢從快取表讀取

#### 優先級評估
- **當前**: P3 (Low) - 測試環境無影響
- **監控指標**: 當 Production 執行歷史 >5000 時，重新評估

#### 相關 Issue
- 無（內部技術債務）

---

### ISSUE-004: Frontend SignalR 斷線重連機制待完善

**日期**: 2025-12-06 (Day 12, 發現)
**優先級**: P2 (Medium)
**狀態**: 🔄 部分實作，待完善
**影響範圍**: US 1.4 Phase 4 - SignalR Integration

#### 問題描述
Frontend `useSignalR` Hook 實作了基礎 SignalR 連接，但**斷線重連機制不完善**。

**當前實作**:
```typescript
// ⚠️ 基礎重連，無指數退避
connection.start()
  .then(() => console.log('SignalR Connected'))
  .catch(err => {
    console.error('SignalR Connection Error:', err);
    // ❌ 無自動重連邏輯
  });
```

#### 潛在問題
1. **網路不穩定**: 用戶網路斷線後，SignalR 不會自動重連
2. **後端重啟**: Backend 部署時，Frontend 連接斷開且不重連
3. **用戶體驗差**: 斷線後需要手動重新整理頁面

#### 建議解決方案
**實作自動重連 + 指數退避**:
```typescript
const startConnection = async (retryCount = 0) => {
  try {
    await connection.start();
    console.log('SignalR Connected');
    setRetryCount(0); // 重置重試次數
  } catch (err) {
    console.error('SignalR Connection Error:', err);

    // 指數退避重試: 1s, 2s, 4s, 8s, 16s, 30s (max)
    const delay = Math.min(1000 * Math.pow(2, retryCount), 30000);

    console.log(`Retrying in ${delay}ms...`);
    setTimeout(() => startConnection(retryCount + 1), delay);
  }
};

// SignalR 內建重連機制
connection.onreconnecting(error => {
  console.log('SignalR Reconnecting...', error);
  setConnectionStatus('reconnecting');
});

connection.onreconnected(connectionId => {
  console.log('SignalR Reconnected:', connectionId);
  setConnectionStatus('connected');

  // 重新訂閱所有群組
  resubscribeAllGroups();
});

connection.onclose(error => {
  console.log('SignalR Disconnected:', error);
  setConnectionStatus('disconnected');

  // 自動重連
  startConnection();
});
```

#### 額外改進
1. **UI 狀態顯示**:
   - 顯示 "連接中...", "已連接", "斷線重連中..."
   - 斷線時禁用發送訊息按鈕

2. **重新訂閱機制**:
   - 斷線重連後，自動重新訂閱之前的 Agent/Conversation

#### 優先級評估
- **當前**: P2 (Medium) - Development 環境可用，但 Production 需完善
- **目標完成**: Sprint 2 結束前 (2025-12-21)

#### 相關 User Story
- US 1.4 Phase 4 (SignalR 整合)
- US 6.1 Phase 3 (Chat UI SignalR)

---

## 📝 經驗教訓總結

### 估計準確性改進
1. **第三方整合**: 乘以 1.5x 係數（SignalR, Semantic Kernel）
2. **進階功能**: P95/P99 統計、匯出功能應單獨估計
3. **第一次使用技術**: 保留 20% Buffer Time

### 需求管理改進
1. **Sprint Planning**: 詳細討論所有驗收標準，避免中途新增需求
2. **變更流程**: Sprint 中途變更應走正式 Change Request 流程
3. **MVP 定義**: 明確區分 "Must Have" vs "Nice to Have"

### 技術決策改進
1. **設計前瞻性**: `AssemblyLoadContext` 選擇正確，支援熱重載
2. **效能監控**: Percentile 計算應從設計階段考慮大數據場景
3. **錯誤處理**: SignalR 斷線重連應在初期實作完整

### 團隊協作改進
1. **並行開發**: Backend 與 Frontend 並行，縮短總時程
2. **每日 Standup**: 及時發現阻塞問題（ISSUE-001, ISSUE-002 快速解決）
3. **Code Review**: Migration 順序、編碼問題應在 Review 階段發現

---

## 🔍 監控指標

### 問題解決效率
- **平均解決時間**: 45 分鐘 (ISSUE-001: 30min, ISSUE-002: 1h)
- **阻塞時間**: 0 天（所有問題當日解決）

### 範圍變更影響
- **CHANGE-001 延遲**: +6 days
- **CHANGE-002 節省**: +4-6 days (未來)
- **淨影響**: Sprint 2 仍延遲 6 days

### 待處理風險
- **ISSUE-003 (Percentile 效能)**: 低優先級，監控中
- **ISSUE-004 (SignalR 重連)**: 中優先級，Sprint 2 內完成

---

## 📚 完整參考文獻索引 (50+ 文檔)

本問題追蹤文檔中的技術決策與解決方案基於以下 50+ 專案文檔，按 15 個類別組織以便快速定位相關技術細節。

### 🎯 優先查閱 - Planning 文檔（8 documents）
> 📌 **重要**：此區域文檔是 /docs 的濃縮版，提供更全面的項目背景、架構設計與技術棧資訊

- [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - MVP 範圍定義與功能優先級
- [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 任務分配與工作量估計
- [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - 開發策略、工作流程與最佳實踐
- [Architecture Evolution Roadmap](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進路線圖（Phase 1-4）
- [Dependency Matrix](../../1-planning/DEPENDENCY-MATRIX.md) - 技術依賴關係與整合點
- [Risk Register](../../1-planning/RISK-REGISTER.md) - 風險管理與緩解措施
- [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 關鍵技術決策記錄
- [Sprint Execution Framework](../../SPRINT-EXECUTION-FRAMEWORK.md) - Sprint 執行框架與流程

### 📋 Sprint 文檔（6 documents）
- [SPRINT-1-OVERVIEW](../sprint-1/SPRINT-1-1-OVERVIEW.md) - Sprint 1 總覽與完成報告
- [SPRINT-1-RETROSPECTIVE](../sprint-1/SPRINT-1-7-RETROSPECTIVE.md) - Sprint 1 回顧與經驗教訓
- [SPRINT-2-OVERVIEW](./SPRINT-2-1-OVERVIEW.md) - Sprint 2 總覽與整體進度
- [SPRINT-2-PLAN](./SPRINT-2-2-PLAN.md) - Sprint 2 詳細執行計劃
- [SPRINT-2-CHECKLIST](./SPRINT-2-4-CHECKLIST.md) - Sprint 2 檢查清單與驗收標準
- [SPRINT-2-DEV-LOG](./SPRINT-2-5-DEV-LOG.md) - Sprint 2 開發日誌（問題首次記錄）

### 📊 User Story 規格文檔（6 documents）
- [US-1.1-Agent-CRUD-Operations](../../../docs/user-stories/US-1.1-Agent-CRUD-Operations.md) - Agent CRUD 完整規格
- [US-1.2-Agent-Search-Filtering](../../../docs/user-stories/US-1.2-Agent-Search-Filtering.md) - Agent 搜尋與篩選
- [US-1.3-Agent-State-Management](../../../docs/user-stories/US-1.3-Agent-State-Management.md) - Agent 狀態管理生命週期
- [US-1.4-Agent-Execution-Monitoring](../../../docs/user-stories/US-1.4-Agent-Execution-Monitoring.md) - Agent 執行與即時監控
- [US-2.1-Plugin-System-Foundation](../../../docs/user-stories/US-2.1-Plugin-System-Foundation.md) - Plugin 系統基礎架構
- [US-6.1-Basic-Chat-Interface](../../../docs/user-stories/US-6.1-Basic-Chat-Interface.md) - 基礎聊天介面

### 🏛️ ADR（架構決策記錄）（5 documents）
> 問題解決方案與架構決策相關

- [ADR-006-Hybrid-State-Management](../../../docs/architecture/decisions/ADR-006-Hybrid-State-Management.md) - 混合狀態管理策略
- [ADR-007-Phased-Communication-Architecture](../../../docs/architecture/decisions/ADR-007-Phased-Communication-Architecture.md) - 階段式通訊架構（SignalR 實作依據）
- [ADR-008-Code-Interpreter-Container-Pooling](../../../docs/architecture/decisions/ADR-008-Code-Interpreter-Container-Pooling.md) - Code Interpreter 容器池設計
- [ADR-009-LLM-Provider-Abstraction](../../../docs/architecture/decisions/ADR-009-LLM-Provider-Abstraction.md) - LLM Provider 抽象層
- [ADR-011-Framework-Abstraction-Layer](../../../docs/architecture/decisions/ADR-011-Framework-Abstraction-Layer.md) - Framework 抽象層（統一 AI 框架介面）

### 🔧 技術實施文檔（8 documents）
- [Agent-Execution-Implementation](../../../docs/technical-implementation/Agent-Execution-Implementation.md) - Agent 執行引擎實作細節
- [SignalR-Real-Time-Monitoring](../../../docs/technical-implementation/SignalR-Real-Time-Monitoring.md) - SignalR 即時監控實作
- [Plugin-System-Implementation](../../../docs/technical-implementation/Plugin-System-Implementation.md) - Plugin 系統實作
- [Semantic-Kernel-Integration](../../../docs/technical-implementation/Semantic-Kernel-Integration.md) - Semantic Kernel 整合指南
- [CQRS-MediatR-Implementation](../../../docs/technical-implementation/CQRS-MediatR-Implementation.md) - CQRS + MediatR 實作模式
- [Error-Handling-Strategy](../../../docs/technical-implementation/Error-Handling-Strategy.md) - 錯誤處理策略
- [Logging-Monitoring-Implementation](../../../docs/technical-implementation/Logging-Monitoring-Implementation.md) - 日誌與監控實作
- [Export-Functionality-Implementation](../../../docs/technical-implementation/Export-Functionality-Implementation.md) - 匯出功能實作（CSV/JSON）

### 🏗️ 架構設計文檔（8 documents）
- [Architecture-Design-Document](../../../docs/architecture/Architecture-Design-Document.md) - 完整架構設計文件
- [Clean-Architecture-Implementation](../../../docs/architecture/Clean-Architecture-Implementation.md) - Clean Architecture 實踐指南
- [DDD-Patterns-Guide](../../../docs/architecture/DDD-Patterns-Guide.md) - DDD 模式與實踐
- [CQRS-Pattern-Design](../../../docs/architecture/CQRS-Pattern-Design.md) - CQRS 模式設計
- [SignalR-Integration](../../../docs/architecture/SignalR-Integration.md) - SignalR 即時通訊整合最佳實踐
- [Frontend-Architecture](../../../docs/architecture/Frontend-Architecture.md) - React 前端架構設計
- [State-Management-Strategy](../../../docs/architecture/State-Management-Strategy.md) - 前端狀態管理策略
- [Microservices-Readiness-Assessment](../../../docs/architecture/Microservices-Readiness-Assessment.md) - 微服務就緒度評估

### 🔌 API 設計文檔（4 documents）
- [API-Design-Principles](../../../docs/api/API-Design-Principles.md) - RESTful API 設計原則
- [API-Versioning-Strategy](../../../docs/api/API-Versioning-Strategy.md) - API 版本管理策略
- [API-Documentation-Standards](../../../docs/api/API-Documentation-Standards.md) - API 文檔標準（Swagger/OpenAPI）
- [Error-Response-Format](../../../docs/api/Error-Response-Format.md) - 統一錯誤響應格式

### 💾 資料庫設計文檔（8 documents）
> ISSUE-001 Migration 問題相關

- [Database-Schema](../../../docs/database/Database-Schema.md) - 完整資料庫 Schema 設計
- [Migration-Strategy](../../../docs/database/Migration-Strategy.md) - EF Core Migration 策略與最佳實踐
- [Indexing-Strategy](../../../docs/database/Indexing-Strategy.md) - 索引優化策略
- [Query-Optimization-Guide](../../../docs/database/Query-Optimization-Guide.md) - 查詢優化指南
- [Data-Seeding-Strategy](../../../docs/database/Data-Seeding-Strategy.md) - 資料初始化策略
- [Connection-Pooling-Configuration](../../../docs/database/Connection-Pooling-Configuration.md) - 連接池配置
- [Soft-Delete-Implementation](../../../docs/database/Soft-Delete-Implementation.md) - Soft Delete 實作
- [Audit-Trail-Design](../../../docs/database/Audit-Trail-Design.md) - 審計日誌設計

### 🧪 測試策略文檔（6 documents）
> 問題預防與測試策略相關

- [Testing-Strategy](../../../docs/testing/Testing-Strategy.md) - 整體測試策略
- [Unit-Testing-Guidelines](../../../docs/testing/Unit-Testing-Guidelines.md) - 單元測試指南
- [Integration-Testing-Guidelines](../../../docs/testing/Integration-Testing-Guidelines.md) - 整合測試指南
- [E2E-Testing-Strategy](../../../docs/testing/E2E-Testing-Strategy.md) - E2E 測試策略（Playwright）
- [Test-Data-Management](../../../docs/testing/Test-Data-Management.md) - 測試資料管理
- [Performance-Testing-Guide](../../../docs/testing/Performance-Testing-Guide.md) - 效能測試指南

### 🛡️ 安全性文檔（2 documents）
- [Security-Architecture](../../../docs/security/Security-Architecture.md) - 安全架構設計
- [Authentication-Authorization-Design](../../../docs/security/Authentication-Authorization-Design.md) - 認證授權設計

### 🎨 UX 設計文檔（4 documents）
- [UX-Design-System](../../../docs/ux/UX-Design-System.md) - UX 設計系統
- [Component-Library-Guidelines](../../../docs/ux/Component-Library-Guidelines.md) - 組件庫指南（Material-UI）
- [Responsive-Design-Standards](../../../docs/ux/Responsive-Design-Standards.md) - 響應式設計標準
- [Accessibility-Guidelines](../../../docs/ux/Accessibility-Guidelines.md) - 無障礙設計指南

### 📐 開發標準文檔（5 documents）
> 問題預防流程改進相關

- [Coding-Standards](../../../docs/development/Coding-Standards.md) - 編碼標準（C# + TypeScript）
- [Git-Workflow-Guide](../../../docs/development/Git-Workflow-Guide.md) - Git 工作流程
- [Code-Review-Checklist](../../../docs/development/Code-Review-Checklist.md) - Code Review 檢查清單（預防 ISSUE-001 類問題）
- [Definition-of-Done](../../../docs/development/Definition-of-Done.md) - 完成定義標準
- [Documentation-Standards](../../../docs/development/Documentation-Standards.md) - 文檔標準

### 📊 效能監控文檔（2 documents）
- [Performance-Monitoring-Strategy](../../../docs/performance/Performance-Monitoring-Strategy.md) - 效能監控策略
- [Metrics-Collection-Design](../../../docs/performance/Metrics-Collection-Design.md) - 指標收集設計

### 🚀 部署維運文檔（2 documents）
- [Docker-Compose-Configuration](../../../docs/deployment/Docker-Compose-Configuration.md) - Docker Compose 配置
- [CI-CD-Pipeline-Design](../../../docs/deployment/CI-CD-Pipeline-Design.md) - CI/CD Pipeline 設計

### 📝 變更管理文檔（3 documents）
> 範圍變更詳細記錄

- [CHANGE-001-Sprint2-Scope-Adjustment](../../4-changes/CHANGE-001-Sprint2-Scope-Adjustment.md) - Sprint 2 範圍調整詳細記錄
- [CHANGE-002-Plugin-System-Enhancement](../../4-changes/CHANGE-002-Plugin-System-Enhancement.md) - Plugin 系統功能增強詳細記錄
- [CHANGE-LOG](../../4-changes/CHANGE-LOG.md) - 完整變更歷史

### 📚 其他參考文檔（7 documents）
- [BMad-Methodology-Overview](../../../docs/bmad/BMad-Methodology-Overview.md) - BMad 方法論總覽
- [PRD-AI-Agent-Platform](../../../docs/product/PRD-AI-Agent-Platform.md) - AI Agent 平台產品需求文件
- [Technical-Stack-Selection](../../../docs/technical-stack/Technical-Stack-Selection.md) - 技術棧選型分析
- [Third-Party-Integration-Guide](../../../docs/integration/Third-Party-Integration-Guide.md) - 第三方整合指南
- [License-Documentation](../../../LICENSE) - 授權文件
- [Contributing-Guidelines](../../../CONTRIBUTING.md) - 貢獻指南
- [Project-README](../../../README.md) - 專案總覽與快速開始

---

## 📋 版本歷史

| 版本 | 日期 | 變更內容 | 負責人 |
|------|------|---------|--------|
| 2.1 | 2025-12-13 | 升級至 v2.1 統一標準：標準化 8 欄位 Header、新增完整目錄、新增使用指南、擴展參考文獻索引至 50+ 文檔、保留所有原有內容 (568 行) | Documentation Team |
| 2.0 | 2025-12-11 | 新增 ISSUE-003, ISSUE-004 待處理問題；新增 CHANGE-003, CHANGE-004 範圍變更 | Development Team |
| 1.0 | 2025-12-07 | 初版創建 - 記錄 Sprint 2 問題追蹤（ISSUE-001, ISSUE-002, CHANGE-001, CHANGE-002） | Development Team |

**v2.1 升級摘要**:
- ✅ **標準化 Header**: 升級至 v2.1 8 欄位格式
- ✅ **新增完整目錄**: 包含所有問題和範圍變更的完整目錄結構，支援快速導航
- ✅ **新增使用指南**: 完整的問題追蹤使用說明、快速導航指南、相關文檔導航
- ✅ **擴展參考文獻索引**: 從 Planning/Sprint/ADR/Database 等少數類別擴展至 50+ 文檔，15 個分類
- ✅ **保留完整內容**: 所有原有 568 行內容完整保留

**保留內容清單** (568 行原始內容 100% 保留):
- ✅ 問題統計表格（8 個問題統計）
- ✅ ISSUE-001 完整記錄（Migration 順序錯誤的根本原因、解決方案、影響、預防措施）
- ✅ ISSUE-002 完整記錄（CSV 編碼問題的根本原因、技術學習、解決方案）
- ✅ CHANGE-001 完整記錄（US 1.4 範圍擴大分析，含根本原因、影響、經驗教訓）
- ✅ CHANGE-002 完整記錄（US 2.1 自然延伸分析，含正面結果）
- ✅ CHANGE-003 完整記錄（US 6.1 時間壓縮風險與緩解措施）
- ✅ CHANGE-004 完整記錄（US 2.2/2.3 剩餘工作與風險評估）
- ✅ ISSUE-003 完整記錄（Percentile 效能問題與 3 種解決方案）
- ✅ ISSUE-004 完整記錄（SignalR 重連機制與指數退避方案）
- ✅ 經驗教訓總結（4 個類別：估計改進、需求管理、技術決策、團隊協作）
- ✅ 監控指標（解決效率、範圍變更影響、待處理風險）
- ✅ 所有程式碼範例（錯誤代碼、修正代碼、建議方案）

---

**文檔版本**: v2.1
**創建日期**: 2025-11-25
**最後更新**: 2025-12-13
**下次更新**: 每週一 (Sprint 2 進行中)
**維護者**: Development Team
