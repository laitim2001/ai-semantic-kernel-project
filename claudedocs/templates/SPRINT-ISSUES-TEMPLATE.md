# Sprint [X] 問題追蹤

**目的**: 系統化追蹤 Sprint 期間遇到的所有問題、Blockers、技術債務

**最後更新**: YYYY-MM-DD HH:MM

---

## 📊 問題概覽

| 狀態 | 數量 | 百分比 |
|------|------|--------|
| 🚨 Open (Critical) | 0 | 0% |
| 🟡 Open (Medium) | 0 | 0% |
| 🟢 Open (Low) | 0 | 0% |
| ✅ Resolved | 0 | 0% |
| ❌ Closed (Won't Fix) | 0 | 0% |
| **總計** | **0** | **100%** |

---

## 🚨 Critical Issues (P0 - 阻塞開發)

### Issue #[X]: [問題標題]

**狀態**: 🚨 Open / 🔄 In Progress / ✅ Resolved / ❌ Won't Fix
**嚴重程度**: 🔴 Critical
**優先級**: P0
**創建日期**: YYYY-MM-DD
**解決日期**: YYYY-MM-DD (如已解決)

**影響範圍**:
- [ ] Backend API
- [ ] Frontend UI
- [ ] Database
- [ ] Deployment
- [ ] Testing
- [ ] Documentation

**阻塞的任務**:
- [ ] CHECKLIST 項目: [具體任務] (Line [X])
- [ ] User Story: US X.X

**問題描述**:
[詳細描述問題的表現、錯誤訊息、重現步驟]

**重現步驟**:
1. 步驟 1
2. 步驟 2
3. 步驟 3
4. 預期行為: [...]
5. 實際行為: [...]

**錯誤訊息/截圖**:
```
[錯誤訊息或日誌]
```

**根本原因分析** (RCA):
[深入分析問題的根本原因]

**解決方案** (如已解決):
[詳細描述如何解決]

**代碼變更**:
- Commit: `[git-commit-hash]`
- PR: #[PR-number]
- Files Changed:
  - `[file-path-1]`
  - `[file-path-2]`

**測試驗證**:
- [ ] 單元測試: `[test-file]`
- [ ] 集成測試: `[test-file]`
- [ ] 手動測試: [測試結果]

**相關資源**:
- Stack Overflow: [link]
- GitHub Issue: [link]
- 官方文檔: [link]

**負責人**: [名字]
**審查人**: [名字]

---

## 🟡 Medium Issues (P1 - 影響功能但不阻塞)

### Issue #[X]: [問題標題]

**狀態**: 🟡 Open / 🔄 In Progress / ✅ Resolved / ❌ Won't Fix
**嚴重程度**: 🟡 Medium
**優先級**: P1
**創建日期**: YYYY-MM-DD
**目標解決日期**: YYYY-MM-DD

**影響範圍**:
- [ ] Backend API
- [ ] Frontend UI
- [ ] Performance
- [ ] UX
- [ ] Documentation

**問題描述**:
[描述問題]

**影響**:
[描述這個問題對用戶/系統的影響]

**解決方案建議**:
[提出可能的解決方案]

**Workaround** (臨時方案):
[如有臨時解決方案,描述如何繞過問題]

**負責人**: [名字]

---

## 🟢 Low Issues (P2 - 小問題或改進建議)

### Issue #[X]: [問題標題]

**狀態**: 🟢 Open / 🔄 In Progress / ✅ Resolved / ❌ Won't Fix
**嚴重程度**: 🟢 Low
**優先級**: P2
**創建日期**: YYYY-MM-DD

**問題描述**:
[描述問題或改進建議]

**是否影響交付**: ❌ No

**解決時間**: Phase 2 / Next Sprint / Backlog

**負責人**: [名字]

---

## 🔧 Technical Debt (技術債務)

### TD #[X]: [技術債務標題]

**類型**:
- [ ] Code Quality (代碼質量)
- [ ] Test Coverage (測試覆蓋率)
- [ ] Documentation (文檔缺失)
- [ ] Performance (性能優化)
- [ ] Refactoring (重構需求)
- [ ] Security (安全改進)

**創建日期**: YYYY-MM-DD
**預估工作量**: [X] 小時/天

**債務描述**:
[描述技術債務的具體內容]

**產生原因**:
- [ ] 時間壓力 (MVP 優先)
- [ ] 需求變更
- [ ] 缺少最佳實踐
- [ ] 遺留代碼

**影響**:
- **可維護性**: High / Medium / Low
- **可擴展性**: High / Medium / Low
- **性能**: High / Medium / Low
- **安全性**: High / Medium / Low

**償還計劃**:
- **何時償還**: Next Sprint / Phase 2 / Backlog
- **如何償還**: [具體計劃]

**受影響的組件**:
- `[file-path-1]`
- `[file-path-2]`

**負責人**: [名字]

---

## 🚧 Blockers (當前阻礙)

### Blocker #[X]: [阻礙標題]

**狀態**: 🚧 Blocked / ✅ Unblocked
**創建日期**: YYYY-MM-DD
**解除日期**: YYYY-MM-DD (如已解除)

**阻礙描述**:
[描述什麼事情阻礙了開發]

**阻礙類型**:
- [ ] 外部依賴 (第三方服務、API)
- [ ] 團隊依賴 (等待其他團隊交付)
- [ ] 決策依賴 (等待 Product Owner 決策)
- [ ] 技術問題 (環境、工具、Bug)
- [ ] 資源依賴 (人力、設備)

**阻礙的任務**:
- [ ] CHECKLIST 項目: [具體任務]
- [ ] User Story: US X.X

**影響時間**: [估計會延遲多久]

**緩解措施**:
[採取了什麼措施來減輕影響或繞過阻礙]

**需要的幫助**:
[需要誰提供什麼幫助來解除阻礙]

**負責跟進**: [名字]
**預計解除日期**: YYYY-MM-DD

---

## 📋 問題分類統計

### 按嚴重程度

| 嚴重程度 | Open | In Progress | Resolved | Won't Fix | Total |
|---------|------|------------|----------|-----------|-------|
| 🔴 Critical | 0 | 0 | 0 | 0 | 0 |
| 🟡 Medium | 0 | 0 | 0 | 0 | 0 |
| 🟢 Low | 0 | 0 | 0 | 0 | 0 |

### 按類型

| 類型 | 數量 |
|------|------|
| 🐛 Bug | 0 |
| 🚨 Critical Issue | 0 |
| 🔧 Technical Debt | 0 |
| 🚧 Blocker | 0 |
| 💡 Enhancement | 0 |

### 按負責人

| 負責人 | Open | In Progress | Resolved |
|--------|------|------------|----------|
| [名字 1] | 0 | 0 | 0 |
| [名字 2] | 0 | 0 | 0 |

---

## 📈 問題趨勢分析

### 本週新增問題
- 🚨 Critical: [X] 個
- 🟡 Medium: [X] 個
- 🟢 Low: [X] 個

### 本週解決問題
- ✅ Resolved: [X] 個
- 平均解決時間: [X] 小時/天

### 問題來源分析
- Backend: [X]%
- Frontend: [X]%
- Database: [X]%
- Integration: [X]%
- Other: [X]%

---

## 🔄 已解決問題 (歷史記錄)

### Issue #[X]: [問題標題] ✅

**解決日期**: YYYY-MM-DD
**問題摘要**: [簡短描述]
**解決方案**: [簡短描述]
**Commit**: `[git-commit-hash]`

---

### Issue #[X]: [問題標題] ✅

[重複上述結構]

---

## ❌ 關閉的問題 (Won't Fix)

### Issue #[X]: [問題標題] ❌

**關閉日期**: YYYY-MM-DD
**關閉原因**:
- [ ] 不在 MVP 範圍 (Phase 2 實現)
- [ ] 設計變更 (需求已改)
- [ ] 重複問題
- [ ] 無法重現
- [ ] 成本/收益不划算

**決策人**: [名字]

---

## 📝 問題追蹤最佳實踐

### 何時創建 Issue
- **發現 Bug 時**: 立即記錄,即使很小
- **遇到 Blocker 時**: 立即記錄並通知團隊
- **發現技術債務時**: 記錄下來,計劃償還時間
- **有改進建議時**: 記錄為 Enhancement

### 如何寫好 Issue
- **清晰的標題**: 一眼看出問題是什麼
- **詳細的描述**: 包含重現步驟、錯誤訊息、預期vs實際
- **影響範圍**: 明確說明影響哪些功能/組件
- **優先級**: 根據影響和緊急程度設定
- **負責人**: 明確誰來解決

### Issue 生命週期
```
Open → In Progress → Resolved → Closed
       ↓
    Blocked (如遇阻礙)
```

### 問題升級機制
- **P2 → P1**: 影響範圍擴大時
- **P1 → P0**: 變成 Blocker 時
- **P0**: 立即通知 Scrum Master 和團隊

---

## 🔗 相關文檔

- 📋 [SPRINT-[X]-OVERVIEW.md](./SPRINT-[X]-OVERVIEW.md)
- 📝 [SPRINT-[X]-DEV-LOG.md](./SPRINT-[X]-DEV-LOG.md) - 問題解決過程記錄
- ✅ [SPRINT-[X]-CHECKLIST.md](./SPRINT-[X]-CHECKLIST.md) - 被阻塞的任務

---

**創建日期**: YYYY-MM-DD
**Sprint [X] 開始日期**: YYYY-MM-DD
**Sprint [X] 結束日期**: YYYY-MM-DD
