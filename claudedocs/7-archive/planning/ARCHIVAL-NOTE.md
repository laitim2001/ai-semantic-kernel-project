# 規劃文件 Archive 說明

## MASTER-DEVELOPMENT-SCHEDULE.md

**Archive 日期**: 2025-11-12

**原因**: 文件內容已過時，被新的規劃文件取代

### 詳細說明

**原文件狀態**:
- **創建日期**: 2025-11-07
- **基於計劃**: 18 Sprint 計劃
- **總週數**: 54 週
- **總 Story Points**: ~240 SP
- **Sprint 1**: 已完成 ✅
- **Sprint 2**: 進行中 (40%)

**Archive 原因**:

1. **計劃已更新**:
   - 原計劃: 18 Sprint, 54週, 160 SP
   - 新計劃: 21 Sprint, 63週, 223-250 SP
   - 增加了 3 個 Sprint (+9週)

2. **內容已被取代**:
   - `SPRINT-ALLOCATION-ANALYSIS.md` 提供了更詳細的 21 Sprint 分配
   - 包含完整的 Gap 分析 (61-83 SP)
   - 說明了為何需要額外的 Sprint

3. **新的 Sprint 詳細文件**:
   - 每個 Sprint 將有 7 個詳細文件 (OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE)
   - 提供更精確和可追蹤的執行細節
   - 避免單一大文件的維護困難

4. **缺少關鍵內容**:
   - 工作流編輯器 (35-45 SP, Sprint 10-14) - 原計劃完全缺少
   - 完整工作流引擎 (34-43 SP vs 原13 SP) - 嚴重低估
   - Framework 抽象層 (5-8 SP) - 未分配

### 取代文件

**主要取代文件**:
1. `SPRINT-ALLOCATION-ANALYSIS.md` - 21 Sprint 完整分配和 Gap 分析
2. `MVP-SCOPE-DEFINITION.md` - 6 個 Phase 的系統化組織
3. `ARCHITECTURE-EVOLUTION-ROADMAP.md` - 架構演進時間表
4. 每個 Sprint 的詳細文件 (SPRINT-X-*.md)

**進度追蹤**:
- Sprint 級別: `SPRINT-X-OVERVIEW.md` + `SPRINT-X-CHECKLIST.md`
- 整體進度: `SPRINT-ALLOCATION-ANALYSIS.md` 的分配表
- 變更記錄: `CHANGE-LOG.md`
- User Story 狀態: `USER-STORY-STATUS.md`

### 歷史價值

雖然此文件已 archive，但它記錄了:
- ✅ Sprint 1 提前 3 天完成 (21天 → 18天)
- ⚠️ Sprint 2 範圍擴展問題 (US 1.4: 5 SP → 13 SP)
- 📊 初期的進度追蹤方法
- 🎯 早期的里程碑定義

這些經驗教訓已整合到新的規劃文件中。

---

**備註**: 如需參考原始 18 Sprint 計劃，可查閱此 archive 文件。新的開發計劃請參考 `/claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md` 和各 Sprint 的詳細文件。
