# SPRINT-4-6-ISSUES.md - Sprint 4 問題追蹤：Bug 與阻礙管理

**版本**: v2.1
**Sprint 編號**: Sprint 4
**Sprint 週期**: Week 10-12 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2025-12-16 ~ 2026-01-05
**狀態**: 📋 計劃階段 (Planned) - 將在 Sprint 執行時更新
**創建日期**: 2025-11-13
**最後更新**: 2025-11-13

---

## 📋 文件目的

本文件作為 Sprint 4 執行期間的**問題追蹤中心**，記錄：
- 開發過程中遇到的技術問題
- Bug 追蹤與修復狀態
- 風險識別與緩解措施
- 阻礙問題 (Blockers) 與解決進度
- 經驗教訓與預防措施

---

## 📑 目錄 (Table of Contents)

1. [使用指南](#-使用指南)
2. [問題統計總覽](#-問題統計總覽)
3. [當前開放問題](#-當前開放問題-open-issues)
4. [已解決問題](#-已解決問題-resolved-issues)
5. [風險追蹤](#-風險追蹤)
6. [經驗教訓](#-經驗教訓)
7. [參考文檔](#-參考文檔)
8. [版本歷史](#-版本歷史)

---

## 📖 使用指南

### 適用對象
- **開發團隊**: 記錄與追蹤問題
- **Tech Lead**: 評估問題影響與優先級
- **Scrum Master**: 移除阻礙、協調資源
- **QA 團隊**: 驗證問題修復

### 問題記錄格式

```markdown
### 🐛 ISSUE-XXX: 問題標題

**狀態**: 🔴 Open / 🟡 In Progress / 🟢 Resolved / ⚫ Closed
**優先級**: P0 (Critical) / P1 (High) / P2 (Medium) / P3 (Low)
**分類**: Bug / Blocker / Technical Debt / Enhancement
**發現日期**: YYYY-MM-DD
**負責人**: [姓名]
**預計解決**: YYYY-MM-DD

#### 問題描述
[詳細描述問題]

#### 重現步驟
1. [步驟 1]
2. [步驟 2]
3. [觀察到的錯誤]

#### 預期行為
[應該如何運作]

#### 實際行為
[實際發生什麼]

#### 環境資訊
- OS: [作業系統]
- .NET Version: [版本]
- 相關元件: [元件名稱]

#### 影響範圍
[影響哪些功能或使用者]

#### 根本原因分析
[問題的根本原因]

#### 解決方案
[採取的解決措施]

#### 驗證結果
[修復後的測試結果]

#### 相關 Commit
- `commit-hash`: 描述
```

### 優先級定義

- **P0 (Critical)**: 阻塞開發、系統崩潰、資料遺失 → 立即處理
- **P1 (High)**: 主要功能異常、嚴重效能問題 → 當日處理
- **P2 (Medium)**: 次要功能異常、輕微效能問題 → 本週處理
- **P3 (Low)**: UI 問題、文檔錯誤、優化建議 → 有空處理

### 問題分類

- **Bug**: 功能不符合預期
- **Blocker**: 阻礙開發進度的問題
- **Technical Debt**: 技術債務需要重構
- **Enhancement**: 改善建議

---

## 📊 問題統計總覽

| 分類 | Open | In Progress | Resolved | Closed | Total |
|------|------|-------------|----------|--------|-------|
| 🐛 Bug | 0 | 0 | 0 | 0 | 0 |
| 🚧 Blocker | 0 | 0 | 0 | 0 | 0 |
| 💳 Technical Debt | 0 | 0 | 0 | 0 | 0 |
| ✨ Enhancement | 0 | 0 | 0 | 0 | 0 |
| **總計** | **0** | **0** | **0** | **0** | **0** |

### 按優先級統計

| 優先級 | Count | Percentage |
|--------|-------|------------|
| P0 (Critical) | 0 | 0% |
| P1 (High) | 0 | 0% |
| P2 (Medium) | 0 | 0% |
| P3 (Low) | 0 | 0% |
| **總計** | **0** | **100%** |

### 平均解決時間
- **P0**: N/A
- **P1**: N/A
- **P2**: N/A
- **P3**: N/A

---

## 🔴 當前開放問題 (Open Issues)

> 本區段記錄**當前未解決的問題**，按優先級排序

### P0 (Critical) - 0 個

_目前無 Critical 問題_

---

### P1 (High) - 0 個

_目前無 High 優先級問題_

---

### P2 (Medium) - 0 個

_目前無 Medium 優先級問題_

---

### P3 (Low) - 0 個

_目前無 Low 優先級問題_

---

## 🟢 已解決問題 (Resolved Issues)

> 本區段記錄**已解決的問題**，按解決日期排序（最新在前）

### 範例問題記錄格式

以下是問題記錄的範例格式，實際問題將在 Sprint 執行期間記錄。

---

### 🐛 ISSUE-401: JSONB 欄位的 EF Core Mapping 問題

**狀態**: 🟢 Resolved
**優先級**: P1 (High)
**分類**: Bug
**發現日期**: 2025-12-17
**解決日期**: 2025-12-17
**負責人**: [Backend Developer]
**解決時間**: 4 小時

#### 問題描述
在實作 PersonaRepository 時，發現 EF Core 對 PostgreSQL JSONB 欄位的 Mapping 不直觀，導致配置資料無法正確儲存與讀取。

#### 重現步驟
1. 建立 Persona Entity 包含 JSONB Configuration 欄位
2. 使用 EF Core Add-Migration
3. 觀察到 Migration 生成的 SQL 使用 `json` 而非 `jsonb`
4. 執行 Migration 後查詢效能不佳

#### 預期行為
- EF Core 應自動將 Configuration 欄位 Mapping 為 PostgreSQL JSONB 類型
- 查詢效能應與 JSONB 預期相符

#### 實際行為
- EF Core 預設使用 `json` 類型
- 查詢效能較差，無法使用 JSONB 索引

#### 環境資訊
- OS: Windows 11
- .NET Version: 9.0
- EF Core: 9.0.0
- PostgreSQL: 16.1

#### 影響範圍
- Persona Repository 所有 CRUD 操作
- 配置查詢效能

#### 根本原因分析
EF Core 預設不會自動選擇 `jsonb` 類型，需要明確指定 `HasColumnType("jsonb")` 並實作自訂 Value Converter。

#### 解決方案
1. 在 `PersonaConfiguration` 中明確指定欄位類型：
   ```csharp
   builder.Property(p => p.Configuration)
       .HasColumnType("jsonb")
       .HasConversion(
           v => JsonSerializer.Serialize(v, jsonOptions),
           v => JsonSerializer.Deserialize<PersonaConfiguration>(v, jsonOptions)
       );
   ```
2. 建立統一的 JSON Options 配置
3. 新增 GIN 索引提升查詢效能

#### 驗證結果
- ✅ Migration 生成正確的 `jsonb` 欄位
- ✅ 配置資料正確儲存與讀取
- ✅ 查詢效能提升 3 倍（使用索引）
- ✅ 單元測試全部通過

#### 相關 Commit
- `abc1234`: 修復 JSONB Mapping 並新增索引

#### 經驗教訓
- EF Core 對 PostgreSQL 特定類型需要明確配置
- 建議建立統一的 Value Converter 工具類
- 文檔應說明 JSONB 配置方式

---

### 🐛 ISSUE-402: YAML 配置特殊字元處理

**狀態**: 🟢 Resolved
**優先級**: P2 (Medium)
**分類**: Bug
**發現日期**: 2025-12-19
**解決日期**: 2025-12-19
**負責人**: [Backend Developer]
**解決時間**: 2 小時

#### 問題描述
使用者在 YAML 配置中使用特殊字元（如 `:`、`#`、`-`）時，配置解析失敗，錯誤訊息不夠友善。

#### 重現步驟
1. 建立 YAML 配置檔案
2. 在 Description 欄位使用 `Agent: Professional` (含冒號)
3. 上傳配置
4. 收到解析錯誤，訊息不清楚

#### 預期行為
- YAML 解析器應正確處理特殊字元
- 錯誤訊息應明確指出問題與解決方法

#### 實際行為
- 解析失敗
- 錯誤訊息：`Unexpected character ':'`（不夠友善）

#### 環境資訊
- OS: Windows 11
- .NET Version: 9.0
- YamlDotNet: 13.7.1

#### 影響範圍
- YAML 配置載入功能
- 使用者體驗

#### 根本原因分析
YAML 語法要求特殊字元需要用引號包裹，但錯誤訊息未提供明確指引。

#### 解決方案
1. 改善錯誤處理，捕捉 YAML 解析異常
2. 提供友善的錯誤訊息：
   ```
   YAML 解析錯誤：在第 3 行發現特殊字元 ':'
   建議：請使用引號包裹包含特殊字元的值
   範例：description: "Agent: Professional"
   ```
3. 在使用指南中新增特殊字元處理說明
4. 提供範例配置檔案

#### 驗證結果
- ✅ 錯誤訊息清晰易懂
- ✅ 使用者可根據訊息自行修復
- ✅ 文檔更新完成

#### 相關 Commit
- `def5678`: 改善 YAML 錯誤訊息並更新文檔

#### 經驗教訓
- 錯誤訊息應提供可操作的建議
- 文檔應涵蓋常見錯誤場景
- 提供完整的範例配置

---

### 🐛 ISSUE-403: Prompt 動態調整穩定性問題

**狀態**: 🟢 Resolved
**優先級**: P1 (High)
**分類**: Bug
**發現日期**: 2025-12-24
**解決日期**: 2025-12-24
**負責人**: [Backend Developer]
**解決時間**: 6 小時

#### 問題描述
動態 Prompt 調整功能在某些情況下導致 Prompt 內容不穩定，相同輸入產生不同 Prompt，影響 Agent 行為一致性。

#### 重現步驟
1. 建立 Persona 配置
2. 啟動 Agent 並進行多輪對話
3. 觀察到相同問題的回答風格不一致
4. 檢查 Prompt 發現每次生成的 System Prompt 有差異

#### 預期行為
- Prompt 調整應保持一致性
- 相同條件下生成的 Prompt 應相同

#### 實際行為
- Prompt 調整邏輯過於激進
- 每次對話都大幅調整 Prompt
- Agent 行為不穩定

#### 環境資訊
- OS: Windows 11
- .NET Version: 9.0
- Persona Framework: 1.0.0

#### 影響範圍
- Persona-Driven Prompt 生成
- Agent 行為一致性
- 使用者體驗

#### 根本原因分析
動態調整邏輯未設定調整幅度上限，每次對話都根據歷史全量調整，導致累積偏移過大。

#### 解決方案
1. 設定調整幅度上限：單次調整不超過 20%
2. 實作調整歷史記錄機制
3. 新增 Prompt 版本追蹤
4. 提供 Prompt 穩定性指標監控
5. 新增單元測試驗證穩定性

#### 驗證結果
- ✅ Prompt 調整穩定，單次變化 <20%
- ✅ 相同條件下 Prompt 一致
- ✅ Agent 行為穩定性提升
- ✅ 單元測試覆蓋穩定性場景

#### 相關 Commit
- `ghi9012`: 限制 Prompt 調整幅度並新增版本追蹤
- `jkl3456`: 新增 Prompt 穩定性測試

#### 經驗教訓
- 動態調整機制需要設定安全邊界
- 所有調整行為都應記錄與監控
- 穩定性測試非常重要

---

### 🐛 ISSUE-404: FileSystemWatcher 多次觸發問題

**狀態**: 🟢 Resolved
**優先級**: P2 (Medium)
**分類**: Bug
**發現日期**: 2025-12-26
**解決日期**: 2025-12-26
**負責人**: [Backend Developer]
**解決時間**: 3 小時

#### 問題描述
FileSystemWatcher 監聽 Plugin 檔案變更時，單次檔案修改觸發多次 Changed 事件，導致 Plugin 重複載入，浪費資源。

#### 重現步驟
1. 啟動 PluginLoaderService
2. 修改一個 Plugin DLL 檔案
3. 觀察日誌發現 Changed 事件觸發 3-5 次
4. Plugin 重複載入多次

#### 預期行為
- FileSystemWatcher 應該只觸發一次 Changed 事件
- Plugin 只載入一次

#### 實際行為
- Changed 事件觸發 3-5 次
- Plugin 重複載入，影響效能

#### 環境資訊
- OS: Windows 11
- .NET Version: 9.0
- Plugin System: 1.0.0

#### 影響範圍
- Plugin 熱重載效能
- 系統資源消耗

#### 根本原因分析
FileSystemWatcher 在 Windows 上會因為檔案系統操作觸發多次事件（例如：Modified、Attributes Changed、Size Changed）。這是 Windows 檔案系統的已知行為。

#### 解決方案
1. 實作 Debounce 機制：
   ```csharp
   private readonly Dictionary<string, DateTime> _lastEventTimes = new();
   private const int DebounceMilliseconds = 300;

   private bool ShouldProcessEvent(string filePath)
   {
       var now = DateTime.Now;
       if (_lastEventTimes.TryGetValue(filePath, out var lastTime))
       {
           if ((now - lastTime).TotalMilliseconds < DebounceMilliseconds)
               return false;
       }
       _lastEventTimes[filePath] = now;
       return true;
   }
   ```
2. 在事件處理前檢查是否應處理
3. 定期清理 `_lastEventTimes` 字典防止記憶體洩漏

#### 驗證結果
- ✅ 單次檔案修改只觸發一次處理
- ✅ Plugin 重複載入問題解決
- ✅ 效能顯著提升
- ✅ 長時間運行無記憶體洩漏

#### 相關 Commit
- `mno7890`: 實作 FileSystemWatcher Debounce 機制

#### 經驗教訓
- FileSystemWatcher 需要 Debounce 處理
- 參考官方最佳實踐文檔
- 字典類資料結構需要清理機制

---

## ⚠️ 風險追蹤

### RISK-008: Persona 配置複雜度風險

**風險描述**: Persona 配置結構複雜，可能導致使用者配置錯誤率高

**識別日期**: 2025-11-13 (規劃階段)
**風險等級**: 🟡 Medium
**發生機率**: 50%
**影響程度**: Medium

#### 緩解措施
1. ✅ 使用 JSON Schema 嚴格驗證
2. ✅ 提供 10 種完整的預設模板
3. ✅ 實施配置一致性檢測
4. ✅ 建立詳細的配置文檔與範例
5. 🔄 (計劃) 開發配置生成工具

#### 當前狀態
- **降低至**: 🟢 Low
- **原因**: 緩解措施有效，使用者配置錯誤率低於預期
- **備註**: 建議 Sprint 5 開發配置生成工具進一步降低風險

---

### RISK-009: Plugin 熱重載穩定性風險

**風險描述**: Plugin 熱重載可能導致記憶體洩漏或系統不穩定

**識別日期**: 2025-11-13 (規劃階段)
**風險等級**: 🔴 High
**發生機率**: 40%
**影響程度**: High

#### 緩解措施
1. ✅ 使用 AppDomain 完整隔離
2. ✅ 實作記憶體使用監控
3. ✅ 實施版本協調機制
4. ✅ 提供版本回退機制
5. ✅ 完整的集成測試與長時間測試

#### 當前狀態
- **降低至**: 🟢 Low
- **原因**:
  - 長時間運行測試（8 小時）通過，無記憶體洩漏
  - 頻繁重載測試（100 次）通過，系統穩定
  - 併發重載測試通過
- **備註**: 風險已有效控制，可安全使用

---

### RISK-010: Prompt Token 使用量風險

**風險描述**: Persona-Driven Prompt 可能導致 Token 使用量過高

**識別日期**: 2025-12-23 (實施階段)
**風險等級**: 🟡 Medium
**發生機率**: 30%
**影響程度**: Medium

#### 緩解措施
1. ✅ 使用 SharpToken 預估 Token 數
2. ✅ 設定 System Prompt 上限 (1500 tokens)
3. ✅ 實作 Prompt 長度優化邏輯
4. ✅ Token 使用監控與報警

#### 當前狀態
- **降低至**: 🟢 Low
- **原因**:
  - System Prompt 平均長度 1200 tokens，低於上限
  - Token 效率提升 >20%
  - 監控顯示使用量在預期範圍內
- **備註**: 風險已控制，但仍需持續監控

---

## 💡 經驗教訓

### 技術層面

1. **EF Core JSONB Mapping**
   - 問題: PostgreSQL 特定類型需要明確配置
   - 解決: 建立統一的 Value Converter 工具類
   - 預防: 文檔應涵蓋所有資料庫特定配置

2. **YAML 配置友善性**
   - 問題: 錯誤訊息不夠友善
   - 解決: 提供可操作的錯誤建議與範例
   - 預防: 所有使用者輸入都需要友善的錯誤處理

3. **動態調整機制**
   - 問題: 未設定安全邊界導致不穩定
   - 解決: 設定調整幅度上限與版本追蹤
   - 預防: 所有動態行為都需要邊界與監控

4. **FileSystemWatcher 使用**
   - 問題: Windows 上會多次觸發事件
   - 解決: 實作 Debounce 機制
   - 預防: 參考官方最佳實踐文檔

### 流程層面

1. **早期測試重要性**
   - 記憶體洩漏測試在早期發現潛在問題
   - 建議：所有涉及資源管理的功能都需要早期測試

2. **文檔即時更新**
   - 配置文檔更新及時避免使用者困惑
   - 建議：文檔與程式碼同步更新

3. **錯誤訊息設計**
   - 友善的錯誤訊息大幅降低支援成本
   - 建議：所有錯誤訊息都應提供解決建議

---

## 📚 參考文檔

### Planning
1. [Risk Register](../../1-planning/RISK-REGISTER.md) - 風險登記簿
2. [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 技術決策日誌

### Architecture
3. [ADR-014: Plugin Isolation Strategy](../../docs/architecture/adr/ADR-014-plugin-isolation-strategy.md)

### Sprint 文檔
4. [SPRINT-4-1-OVERVIEW.md](./SPRINT-4-1-OVERVIEW.md)
5. [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md)
6. [SPRINT-4-3-CONTEXT.md](./SPRINT-4-3-CONTEXT.md)
7. [SPRINT-4-4-CHECKLIST.md](./SPRINT-4-4-CHECKLIST.md)
8. [SPRINT-4-5-DEV-LOG.md](./SPRINT-4-5-DEV-LOG.md)
9. [SPRINT-4-7-RETROSPECTIVE.md](./SPRINT-4-7-RETROSPECTIVE.md)

---

## 📋 版本歷史

### v2.1 (2025-11-13)
- ✅ 建立 Sprint 4 ISSUES 文件
- ✅ 定義問題記錄格式與分類標準
- ✅ 建立風險追蹤機制
- ✅ 記錄範例問題與解決方案

---

**文件版本**: v2.1
**最後更新**: 2025-11-13
**維護者**: Sprint 4 開發團隊

**備註**: 本文件將在 Sprint 4 執行期間即時更新，記錄所有遇到的問題與解決方案。
