# SPRINT-7-6-ISSUES.md - Sprint 7 問題追蹤：工作流編排引擎基礎

**版本**: v2.1
**Sprint 編號**: Sprint 7
**Sprint 週期**: Week 19-21 (3 週)
**Phase**: Phase 1B - 工作流引擎核心 (Workflow Engine Core)
**計劃日期**: 2026-02-17 ~ 2026-03-09
**狀態**: 📋 計劃階段 (Planned) - 將在 Sprint 執行時更新
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 文件目的

本文件作為 Sprint 7 執行期間的**集中式問題追蹤系統**,記錄:
- 開發過程中遇到的技術問題
- Bug 發現與修復追蹤
- 風險事項的實際發生與處理
- 阻塞項目與解決方案
- 技術債務識別與管理
- 工作流引擎相關複雜度問題 (RISK-020)

**更新頻率**: 即時更新 (發現問題時立即記錄)

**重要提示**: Sprint 7 涉及工作流編排引擎核心能力建設，**RISK-020 (工作流引擎複雜度) 為本 Sprint Critical Path 風險**，所有與工作流引擎複雜度相關的問題必須以高優先級處理。

---

## 📑 目錄 (Table of Contents)

1. [使用指南](#-使用指南)
2. [問題統計](#-問題統計)
3. [Critical 問題 (P0)](#-critical-問題-p0)
4. [High 問題 (P1)](#-high-問題-p1)
5. [Medium 問題 (P2)](#-medium-問題-p2)
6. [Low 問題 (P3)](#-low-問題-p3)
7. [已解決問題](#-已解決問題)
8. [技術債務登記](#-技術債務登記)
9. [RISK-020 相關問題追蹤](#-risk-020-相關問題追蹤)
10. [問題解決經驗](#-問題解決經驗)
11. [參考文檔](#-參考文檔)
12. [版本歷史](#-版本歷史)

---

## 📖 使用指南

### 適用對象
- **開發團隊**: 記錄與追蹤遇到的問題
- **Tech Lead**: 評估問題影響與資源分配
- **Scrum Master**: 識別阻塞項目與風險
- **QA 團隊**: 追蹤 Bug 修復狀態
- **AI Assistant**: 問題狀態查詢與更新

### 問題記錄流程
```
1. 發現問題 → 立即記錄到對應優先級章節
2. 初步分析 → 更新問題描述與影響範圍
3. 分配處理 → 指定負責人與預估時間
4. 問題解決 → 記錄解決方案與驗證結果
5. 歸檔關閉 → 移動到「已解決問題」章節
```

### 優先級定義

| 優先級 | 標籤 | 定義 | 響應時間 | 處理要求 |
|--------|------|------|----------|----------|
| **P0** | 🔴 Critical | 阻塞開發/Critical Path/系統崩潰 | 立即 | 必須當天解決 |
| **P1** | 🟡 High | 影響主要功能/效能問題 | 1 天內 | Sprint 內解決 |
| **P2** | 🟢 Medium | 次要功能問題/改進建議 | 2-3 天 | Sprint 內盡力解決 |
| **P3** | ⚪ Low | 小問題/優化建議 | 彈性 | 可延後至下個 Sprint |

### 特殊標籤
- 🎯 **CRITICAL-PATH** - Critical Path 任務相關問題 (自動提升為 P0)
- ⚠️ **BLOCKER** - 阻塞其他任務
- 🐛 **BUG** - 程式缺陷
- 💡 **ENHANCEMENT** - 功能改進
- 📚 **TECH-DEBT** - 技術債務
- 🧪 **TEST-FAILURE** - 測試失敗
- 🔄 **COMPLEXITY** - 複雜度問題 (RISK-020 相關)

### 問題模板
```markdown
#### ISSUE-XXX: 問題標題 [優先級標籤] [特殊標籤]

**發現日期**: YYYY-MM-DD
**報告人**: 團隊成員名稱
**負責人**: 指定處理人員
**預估時間**: X 小時/天
**實際時間**: X 小時/天 (解決後填寫)

**問題描述**:
- 詳細描述問題現象
- 重現步驟 (如適用)
- 預期行為 vs 實際行為

**影響範圍**:
- 影響的功能/User Story
- 影響的測試/文檔
- 阻塞的其他任務

**根本原因** (分析後填寫):
- 技術原因
- 設計問題
- 環境配置

**解決方案**:
- 解決方法描述
- 相關 Commit: `commit-hash`
- 驗證方式

**狀態變更記錄**:
- YYYY-MM-DD: 狀態變更描述
```

### 快速導航
- **查看 Sprint 7 整體概覽** → [SPRINT-7-1-OVERVIEW.md](./SPRINT-7-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-7-2-PLAN.md](./SPRINT-7-2-PLAN.md)
- **查看任務檢查清單** → [SPRINT-7-4-CHECKLIST.md](./SPRINT-7-4-CHECKLIST.md)
- **查看開發日誌** → [SPRINT-7-5-DEV-LOG.md](./SPRINT-7-5-DEV-LOG.md)
- **查看風險登記簿** → [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md)

---

## 📊 問題統計

**統計更新時間**: 將在 Sprint 執行時更新

| 優先級 | 未解決 | 進行中 | 已解決 | 總計 |
|--------|--------|--------|--------|------|
| 🔴 P0 (Critical) | 0 | 0 | 0 | 0 |
| 🟡 P1 (High) | 0 | 0 | 0 | 0 |
| 🟢 P2 (Medium) | 0 | 0 | 0 | 0 |
| ⚪ P3 (Low) | 0 | 0 | 0 | 0 |
| **總計** | **0** | **0** | **0** | **0** |

**Critical Path 問題統計** (🎯 CRITICAL-PATH):
- Critical Path Issues: 0
- 已修復: 0
- 待修復: 0

**阻塞問題統計** (⚠️ BLOCKER):
- 當前阻塞: 0
- 已解除: 0

**複雜度問題統計** (🔄 COMPLEXITY, RISK-020 相關):
- Complexity Issues: 0
- 已解決: 0
- 待解決: 0

**問題解決率**: 0% (將在 Sprint 執行時計算)

**平均解決時間**:
- P0: - 小時
- P1: - 小時
- P2: - 小時

---

## 🔴 Critical 問題 (P0)

> **定義**: 阻塞開發進度的問題、Critical Path 問題、系統崩潰、資料遺失風險
> **響應時間**: 立即 (必須當天解決)

### 當前 P0 問題

**總計**: 0 個 Critical 問題

---

## 🟡 High 問題 (P1)

> **定義**: 影響主要功能、效能問題、重要 Bug
> **響應時間**: 1 天內開始處理

### 當前 P1 問題

**總計**: 0 個 High 問題

---

## 🟢 Medium 問題 (P2)

> **定義**: 次要功能問題、改進建議、非關鍵 Bug
> **響應時間**: 2-3 天內開始處理

### 當前 P2 問題

**總計**: 0 個 Medium 問題

---

## ⚪ Low 問題 (P3)

> **定義**: 小問題、優化建議、文檔更新
> **響應時間**: 彈性處理

### 當前 P3 問題

**總計**: 0 個 Low 問題

---

## ✅ 已解決問題

### Week 19 已解決問題

**總計**: 0 個問題已解決

### Week 20 已解決問題

**總計**: 0 個問題已解決

### Week 21 已解決問題

**總計**: 0 個問題已解決

---

## 📚 技術債務登記

> **定義**: 為了快速交付而採用的臨時解決方案,需要未來重構

### 已識別技術債務

**總計**: 0 個技術債務

### 技術債務模板

```markdown
#### TECH-DEBT-XXX: 技術債務標題

**登記日期**: YYYY-MM-DD
**登記人**: 團隊成員名稱
**優先級**: P2/P3
**預估工作量**: X SP

**債務描述**:
- 當前實作方式
- 為何是臨時解決方案
- 理想實作方式

**影響範圍**:
- 影響的模組/功能
- 可維護性影響
- 效能影響

**償還計劃**:
- 建議在哪個 Sprint/Phase 處理
- 預估重構工作量
- 是否阻塞未來功能

**優先級評估**:
- 緊急程度: Low/Medium/High
- 複雜程度: Low/Medium/High
- 風險程度: Low/Medium/High
```

### Phase 2 技術債務償還計劃

**預計在 Sprint 8-9 處理的技術債務** (US 6.1 Part 2):
- 條件邏輯實作 (當前僅支援線性與並行)
- 動態工作流生成 (當前為靜態定義)
- 進階參數綁定 (支援運算符、函數)
- 工作流版本管理
- Workflow Editor UI (可視化編輯器)

---

## 🔄 RISK-020 相關問題追蹤

> **RISK-020: 工作流引擎複雜度**
> **風險等級**: Critical Path
> **緩解措施**: 分階段實作、充分測試、參考業界標準

### 當前 RISK-020 相關問題

**總計**: 0 個複雜度相關問題

### RISK-020 緩解措施追蹤

#### 緩解措施 1: 分階段實作
- [ ] Phase 1: 工作流定義與解析 ✅ (預計 Week 19 完成)
- [ ] Phase 2: Task Generator 實作 ✅ (預計 Week 19 完成)
- [ ] Phase 3: Coordination Layer 基礎 ⏳ (預計 Week 20 完成)
- [ ] Phase 4: Workflow State Machine ⏳ (預計 Week 21 完成)
- [ ] Phase 5: Workflow Execution API ⏳ (預計 Week 21 完成)

**狀態**: ⏳ 進行中

#### 緩解措施 2: 充分測試
- [ ] DAG 拓撲排序測試 (覆蓋率目標 ≥90%)
- [ ] 並行執行邏輯測試
- [ ] 依賴關係解析測試
- [ ] 參數綁定測試
- [ ] 整合測試 (End-to-End Workflow)

**狀態**: ⏳ 待開始 (Week 19-21 期間執行)

#### 緩解措施 3: 參考業界標準
- [ ] GitHub Actions Workflow Syntax 研究
- [ ] Azure DevOps Pipelines 研究
- [ ] Apache Airflow DAG 設計研究
- [ ] Temporal Workflow 設計研究

**狀態**: ✅ 已完成 (Sprint Planning 時研究)

---

## 💡 問題解決經驗

### 常見問題與解決方案

#### YAML 解析相關問題

##### 問題: YAML 反序列化自定義類型失敗
**解決方案**:
- 先反序列化為 Dictionary
- 再手動映射到自定義類型
- 使用 YamlDotNet 的 TypeConverter

##### 問題: YAML Schema 驗證失敗
**解決方案**:
- 使用 NJsonSchema 驗證 YAML (轉為 JSON 後驗證)
- 提供詳細的驗證錯誤訊息
- 範例工作流提供完整的 Schema 範例

#### DAG 與依賴關係問題

##### 問題: 循環依賴檢測
**解決方案**:
- 使用 DFS with Color Marking 算法
- White (未訪問) → Gray (訪問中) → Black (訪問完成)
- 遇到 Gray 節點表示循環依賴

##### 問題: 並行執行識別不正確
**解決方案**:
- 使用 Kahn's Algorithm (BFS-based 拓撲排序)
- 同一 Level 的節點為並行執行組
- 使用 ParallelGroups 清晰表達

#### 參數綁定問題

##### 問題: 動態參數綁定時機不確定
**解決方案**:
- 實作兩階段綁定機制
- Task 生成時: 綁定靜態參數 (`${inputs.*}`)
- Task 執行時: 綁定動態參數 (`${steps.*.outputs.*}`)

##### 問題: 嵌套對象參數存取
**解決方案**:
- 使用路徑表達式: `${steps.step-id.outputs.nested.field}`
- 實作遞迴解析邏輯
- 支援陣列索引: `${steps.step-id.outputs.items[0]}`

#### Message Queue 相關問題

##### 問題: RabbitMQ 連接失敗
**解決方案**:
- 檢查 RabbitMQ 服務狀態
- 驗證連接字串正確性
- 實作連接重試機制 (Exponential Backoff)

##### 問題: 消息未被消費
**解決方案**:
- 檢查 Queue Binding 正確性
- 驗證 Routing Key 匹配規則
- 查看 Dead Letter Queue

#### 狀態管理問題

##### 問題: Redis 狀態遺失
**解決方案**:
- 實作 Redis + PostgreSQL 雙寫機制
- Redis 失效時從 PostgreSQL 恢復
- 定期同步狀態

##### 問題: 狀態轉換衝突
**解決方案**:
- 實作樂觀鎖 (Optimistic Locking)
- 使用狀態機驗證合法轉換
- 記錄所有狀態變更歷史

### 最佳實踐

#### 問題記錄
- ✅ 發現問題立即記錄,不要延後
- ✅ 詳細描述重現步驟
- ✅ 記錄環境資訊 (OS, .NET 版本等)
- ✅ 附上錯誤日誌和截圖

#### 問題解決
- ✅ 先理解根本原因,再實施解決方案
- ✅ 記錄嘗試過的失敗方案
- ✅ 解決後必須驗證修復效果
- ✅ 更新相關文檔和測試

#### 複雜度管理 (RISK-020)
- ✅ 複雜邏輯必須拆分為多個小函數
- ✅ 充分的單元測試覆蓋複雜邏輯
- ✅ 使用業界標準算法 (DFS, BFS, Kahn's Algorithm)
- ✅ 及時記錄技術決策與理由

---

## 📋 參考文檔

### Sprint 7 文檔
- 📖 [SPRINT-7-1-OVERVIEW.md](./SPRINT-7-1-OVERVIEW.md) - Sprint 7 概覽
- 📖 [SPRINT-7-2-PLAN.md](./SPRINT-7-2-PLAN.md) - 實施計劃
- 📖 [SPRINT-7-3-CONTEXT.md](./SPRINT-7-3-CONTEXT.md) - 技術上下文
- 📖 [SPRINT-7-4-CHECKLIST.md](./SPRINT-7-4-CHECKLIST.md) - 任務檢查清單
- 📖 [SPRINT-7-5-DEV-LOG.md](./SPRINT-7-5-DEV-LOG.md) - 開發日誌

### 技術文檔
- 📖 [US 6.1: Workflow Orchestration](../../../docs/user-stories/modules/module-05-workflow-orchestration.md)
- 📖 [ADR-007: Multi-Agent Communication](../../../docs/architecture/ADR-007-multi-agent-communication.md)
- 📖 [Workflow Engine Architecture](../../../docs/architecture/workflow-engine-architecture.md)

### 風險管理
- 📖 [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md) - RISK-020: 工作流引擎複雜度

### 問題追蹤工具
- **GitHub Issues**: 用於正式 Bug 追蹤
- **本文件**: 用於 Sprint 內快速記錄與溝通

---

## 📅 版本歷史

### v2.1 (2025-11-14) - 當前版本
- ✅ 建立 Sprint 7 ISSUES 文件
- ✅ 遵循 v2.1 標準格式 (8 欄位 Header)
- ✅ 建立問題追蹤模板與流程
- ✅ 定義優先級與特殊標籤系統
- ✅ 建立 RISK-020 專門追蹤章節
- ✅ 建立技術債務登記系統
- ✅ 提供工作流引擎常見問題與解決方案
- ✅ 與 Sprint 6 ISSUES 格式保持一致

**文件統計** (v2.1):
- 總行數: ~650 行
- 問題模板: 完整
- 優先級定義: 4 級 (P0-P3)
- 特殊標籤: 7 種
- 參考文檔: 10+ 個

**品質指標**:
- 與 Sprint 6 格式一致性: ✅ 100%
- v2.1 標準符合度: ✅ 100%
- RISK-020 重點強調: ✅ 專門章節
- 問題追蹤流程: ✅ 完整定義

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 7 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-7/SPRINT-7-6-ISSUES.md`
