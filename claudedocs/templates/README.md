# Sprint 執行文檔模板

本文件夾包含 Sprint 執行的標準化模板文件,用於系統化管理每個 Sprint 的執行過程。

---

## 📋 模板文件列表

### 1. SPRINT-OVERVIEW-TEMPLATE.md
**用途**: Sprint 概覽和總體計劃
**使用時機**: Sprint 開始前創建
**主要內容**:
- Sprint 目標和業務價值
- 包含的 User Stories
- 成功指標 (Definition of Done)
- 技術架構重點
- 風險與依賴

### 2. SPRINT-PLAN-TEMPLATE.md
**用途**: 詳細的技術實施計劃
**使用時機**: Sprint 開始前創建,開發過程中頻繁參考
**主要內容**:
- 每個 User Story 的 MVP 範圍定義
- 詳細技術規格 (API, Database, UI)
- 驗收標準
- 技術實施細節
- 測試策略

### 3. SPRINT-CHECKLIST-TEMPLATE.md
**用途**: 詳細的實施檢查清單和進度追蹤
**使用時機**: Sprint 開始後,每日更新
**主要內容**:
- 進度總覽
- Database 開發檢查清單
- Backend API 開發檢查清單
- Frontend UI 開發檢查清單
- 測試檢查清單
- 文檔和部署檢查清單

### 4. SPRINT-DEV-LOG-TEMPLATE.md
**用途**: 開發日誌,記錄每日進度和學習
**使用時機**: 每天結束前更新
**主要內容**:
- 每日目標和完成內容
- 遇到的問題和解決方案
- 學到的知識
- 協作與溝通記錄
- 明日計劃

### 5. SPRINT-CONTEXT-TEMPLATE.md
**用途**: 精簡的上下文參考,避免在 /docs 中搜尋
**使用時機**: Sprint 開始前創建,開發過程中參考
**主要內容**:
- 完整規劃文檔位置索引
- MVP 範圍摘要 (精簡版)
- 技術規格摘要
- 編碼規範摘要
- AI 助手使用指南

### 6. SPRINT-ISSUES-TEMPLATE.md
**用途**: 系統化追蹤問題、Blockers、技術債務
**使用時機**: 發現問題時立即記錄,每日更新
**主要內容**:
- Critical/Medium/Low Issues
- Technical Debt 追蹤
- Blockers 追蹤
- 問題分類統計
- 已解決問題歷史

### 7. SPRINT-RETROSPECTIVE-TEMPLATE.md
**用途**: Sprint 回顧和總結
**使用時機**: Sprint 結束後立即創建
**主要內容**:
- Sprint 成果總結
- 度量指標
- What Went Well
- What Could Be Improved
- Action Items
- 團隊健康度檢查

---

## 🚀 使用流程

### Sprint 開始前

1. **創建 Sprint 文件夾**:
   ```bash
   mkdir claudedocs/2-sprints/sprint-X
   ```

2. **複製並填寫模板**:
   - 複製所有模板到 Sprint 文件夾
   - 將文件名中的 `TEMPLATE` 替換為 Sprint 編號 (例如: `SPRINT-2-OVERVIEW.md`)
   - 填寫 OVERVIEW, PLAN, CONTEXT 三個文件

3. **初始化 CHECKLIST**:
   - 根據 PLAN 中的內容,填寫 CHECKLIST 的檢查項

### Sprint 進行中

**每日工作流程**:
1. 早上查看 CHECKLIST,確認今日任務
2. 參考 PLAN 和 CONTEXT 進行開發
3. 遇到問題立即記錄到 ISSUES
4. 完成任務後更新 CHECKLIST
5. 晚上更新 DEV-LOG,記錄今日進度和學習

**每週檢查**:
- 更新 OVERVIEW 中的進度和風險
- 檢查 ISSUES 中的問題解決進度
- 評估是否需要調整計劃

### Sprint 結束後

1. **最終檢查**:
   - 確認 CHECKLIST 中的 Definition of Done 全部完成
   - 整理 ISSUES 中未解決的問題

2. **創建 RETROSPECTIVE**:
   - 召開 Sprint Retrospective 會議
   - 填寫 RETROSPECTIVE 模板
   - 制定 Action Items

3. **歸檔**:
   - 將完成的 Sprint 文件夾移至 `/claudedocs/7-archive/sprints/` (可選)
   - 或保留在 `/claudedocs/2-sprints/` 作為歷史記錄

---

## 📊 文件關係圖

```
OVERVIEW (概覽)
    ↓
    ├─→ PLAN (詳細計劃)
    │      ↓
    │   CONTEXT (上下文參考)
    │      ↓
    ├─→ CHECKLIST (檢查清單) ←→ DEV-LOG (開發日誌)
    │      ↓
    │   ISSUES (問題追蹤)
    │      ↓
    └─→ RETROSPECTIVE (回顧總結)
```

**閱讀順序** (開發助手):
1. OVERVIEW → 了解 Sprint 目標
2. CONTEXT → 了解 MVP 範圍和技術規格
3. PLAN → 了解詳細執行計劃
4. CHECKLIST → 開始執行和追蹤進度
5. DEV-LOG & ISSUES → 記錄過程
6. RETROSPECTIVE → Sprint 結束後總結

---

## 💡 最佳實踐

### 對 AI 開發助手
- **從 CONTEXT 開始**: 避免直接查閱 /docs (180+ 文件太多)
- **MVP 範圍優先**: CONTEXT 和 PLAN 中明確標記了 MVP vs Phase 2
- **檢查清單驅動**: 使用 CHECKLIST 確保不遺漏任何任務
- **問題即時記錄**: 發現問題立即記錄到 ISSUES

### 對開發團隊
- **每日更新**: CHECKLIST 和 DEV-LOG 每日更新,保持同步
- **詳細記錄**: DEV-LOG 中的問題解決過程對團隊學習很有價值
- **主動溝通**: ISSUES 中的 Blockers 要及時通知團隊
- **定期回顧**: 每週檢查進度,每個 Sprint 結束後認真做 Retrospective

### 文檔維護
- **保持簡潔**: 模板已經很詳細,使用時可以刪除不需要的部分
- **及時更新**: 不要累積太多待更新內容
- **結構化**: 保持模板的結構,方便快速掃描
- **有鏈接**: 善用文檔之間的鏈接,方便跳轉

---

## 🔧 自定義模板

如果發現模板不符合項目需求,可以自定義:

1. **修改模板**: 在 `/claudedocs/templates/` 中修改模板
2. **添加章節**: 根據項目特點添加新的章節
3. **刪除章節**: 刪除不需要的章節
4. **調整格式**: 調整表格、列表等格式以符合團隊習慣

**注意**: 修改模板後,確保團隊成員都了解變更,並更新本 README.md

---

## 📚 相關資源

- [SPRINT-EXECUTION-FRAMEWORK.md](../SPRINT-EXECUTION-FRAMEWORK.md) - Sprint 執行框架完整說明
- [DOCUMENTATION-INVENTORY.md](../DOCUMENTATION-INVENTORY.md) - 項目文檔清單
- [/docs](../../docs/) - 完整項目規劃文檔

---

## 🆘 常見問題

**Q: 所有 7 個文件都必須使用嗎?**
A: OVERVIEW, PLAN, CHECKLIST 是必須的。DEV-LOG, CONTEXT, ISSUES, RETROSPECTIVE 根據需要使用。

**Q: CONTEXT 和 PLAN 的區別是什麼?**
A: CONTEXT 是精簡的參考指南,PLAN 是詳細的實施計劃。CONTEXT 幫助快速了解範圍,PLAN 提供完整的技術細節。

**Q: AI 助手應該先讀哪個文件?**
A: 順序是 OVERVIEW → CONTEXT → PLAN。CONTEXT 提供精簡的 MVP 範圍,避免在 /docs 中迷失。

**Q: ISSUES 和 DEV-LOG 都記錄問題,有什麼區別?**
A: ISSUES 是結構化的問題追蹤,DEV-LOG 是時間線式的開發日誌。ISSUES 適合追蹤狀態,DEV-LOG 適合記錄過程。

**Q: Retrospective 是否可以省略?**
A: 不建議。Retrospective 對團隊持續改進很重要,即使 Sprint 很成功也應該總結經驗。

---

**創建日期**: 2025-11-12
**最後更新**: 2025-11-12
**版本**: v1.0
