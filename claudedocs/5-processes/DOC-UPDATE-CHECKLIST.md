# 文件更新檢查清單 (Documentation Update Checklist)

**文檔版本**: 1.0.0
**最後更新**: 2025-12-11
**維護者**: Scrum Master + Tech Lead
**適用範圍**: 所有 AI Agent Platform 開發活動

---

## 📋 總覽 (Overview)

### 目的 (Purpose)
本檢查清單確保所有開發活動的文檔更新及時、完整、準確,避免文檔與實際開發進度脫節。

### 使用時機 (When to Use)
- ✅ 每日 Stand-up 後
- ✅ 完成任何 User Story Phase
- ✅ 完成整個 User Story
- ✅ 發生範圍變更
- ✅ Sprint 計劃會議前
- ✅ Sprint 回顧會議前

### 責任分工 (Responsibilities)
| 角色 | 主要責任 | 次要責任 |
|-----|---------|---------|
| **Tech Lead** | 技術文檔、架構設計、代碼統計 | 審查所有文檔更新 |
| **Scrum Master** | Sprint 文檔、流程文檔、變更記錄 | 協調文檔更新 |
| **Product Owner** | User Story、驗收標準、業務文檔 | 審查業務相關文檔 |
| **AI Assistant (Claude)** | 自動化文檔生成、統計分析、同步更新 | 提醒文檔更新需求 |
| **Development Team** | 代碼註釋、技術筆記、測試文檔 | 提供實際進度數據 |

---

## 🔄 日常更新 (Daily Updates)

### ✅ 每日必做 (Daily Must-Do)
**時機**: 每日 Stand-up 後 (15:00-15:30)
**負責人**: AI Assistant (Claude) + Tech Lead
**預估時間**: 10-15 分鐘

#### 檢查項目:

- [ ] **1. Sprint Burndown 更新**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新內容: Burndown Chart 當日數據
  - 數據來源: Git commits, 團隊 Stand-up 報告
  - 格式:
    ```markdown
    | [今日日期] | [剩餘 SP] | [累計完成 SP] | [狀態] | [備註] |
    ```

- [ ] **2. User Story 進度更新**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新內容: User Stories 狀態表中的進度百分比
  - 狀態選項: ⏳ 待開始 / 🔄 進行中 / ✅ 已完成 / ⚠️ 阻塞
  - 依據: 實際完成的 Phase 或任務

- [ ] **3. 當日完成任務記錄**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 更新內容: "最近更新" 章節
  - 格式: 日期 + 完成項目簡述
  - 示例: `2025-12-11: 完成 US 2.1 Phase 3 - Application Layer Commands`

- [ ] **4. 代碼統計更新** (如有新 commits)
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 更新內容: LOC (Lines of Code), 文件數量
  - 工具: `git diff --stat`, `cloc` 或手動統計
  - 頻率: 有代碼提交時

- [ ] **5. 測試統計更新** (如有新測試)
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 更新內容: 測試數量、通過率、覆蓋率
  - 來源: 測試執行結果
  - 格式:
    ```markdown
    | 測試類型 | 數量 | 通過率 | 覆蓋率 |
    |---------|-----|--------|--------|
    | 單元測試 | [X] | [Y%] | [Z%] |
    ```

### 📝 每日選做 (Daily Optional)
**時機**: 如有以下情況發生
**負責人**: 相關責任人

- [ ] **6. 阻塞問題記錄** (如遇到阻塞)
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 章節: "風險與問題追蹤"
  - 內容: 問題描述、影響、負責人、預計解決時間

- [ ] **7. 技術決策記錄** (如做出重要技術選擇)
  - 文件: `claudedocs/3-technical-decisions/ADR-XXX.md` (如需正式 ADR)
  - 或: `claudedocs/PROJECT-STATUS-REPORT.md` 的 "技術亮點" 章節
  - 內容: 決策背景、選項分析、最終選擇、理由

---

## 📅 每週更新 (Weekly Updates)

### ✅ 每週必做 (Weekly Must-Do)
**時機**: 每週五 Sprint Planning 前 (16:00-17:00)
**負責人**: Scrum Master + AI Assistant
**預估時間**: 30-45 分鐘

#### 檢查項目:

- [ ] **1. Sprint 進度總結**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 章節: "Sprint 進度概覽"
  - 更新內容:
    - 整體完成百分比
    - Story Points 統計 (已完成/剩餘)
    - Velocity 計算 (SP/day)
    - 與 Sprint 1 對比

- [ ] **2. 風險與問題更新**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 章節: "風險與問題追蹤"
  - 更新內容:
    - 新增風險/問題
    - 已解決項目狀態更新為 ✅
    - 未解決項目的緩解措施進度

- [ ] **3. 測試覆蓋率報告**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 章節: "質量指標"
  - 更新內容:
    - 單元測試/集成測試統計
    - 代碼覆蓋率趨勢
    - 測試通過率

- [ ] **4. 代碼質量指標**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 章節: "質量指標"
  - 指標:
    - 技術債務評估
    - Code Review 覆蓋率
    - 靜態分析結果 (如有使用 SonarQube)

- [ ] **5. Stakeholder 溝通記錄**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 章節: "Stakeholder 溝通" (如該章節存在)
  - 內容: 本週與 PO, PM, 業務方的重要溝通和決策

---

## 🎯 里程碑更新 (Milestone Updates)

### ✅ User Story Phase 完成時 (Phase Completion)
**時機**: 完成任何 Phase (即時)
**負責人**: Tech Lead + AI Assistant
**預估時間**: 15-20 分鐘

#### 檢查項目:

- [ ] **1. Phase 完成標記**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 位置: User Story 詳細進度章節
  - 更新: Phase 狀態從 🔄 改為 ✅
  - 記錄: 實際完成時間

- [ ] **2. Git Commit 驗證**
  - 檢查: 該 Phase 的代碼是否已提交
  - 驗證: Commit message 是否清楚說明 Phase 內容
  - 建議格式: `feat(US-2.1): Complete Phase 3 - Application Layer Commands`

- [ ] **3. 測試結果記錄**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 內容: 該 Phase 新增的測試及通過情況
  - 示例: `✅ Phase 3 完成 - 新增 8 個 Command Handler 測試,全部通過`

- [ ] **4. 代碼統計更新**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 內容: 該 Phase 新增的代碼行數、文件數
  - 命令: `git diff --stat [previous-commit] HEAD`

- [ ] **5. 技術文檔同步**
  - 檢查: API 文檔是否需要更新
  - 檢查: 架構圖是否需要更新
  - 檢查: README 是否需要更新

### ✅ 完整 User Story 完成時 (User Story Completion)
**時機**: 所有 Phases 完成且通過驗收 (即時)
**負責人**: Tech Lead + Scrum Master + AI Assistant
**預估時間**: 45-60 分鐘

#### 檢查項目:

- [ ] **1. User Story 狀態更新**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新:
    - User Stories 表格中狀態改為 ✅
    - 進度改為 100%
    - 驗收欄位改為 ✅
    - 記錄實際完成天數

- [ ] **2. Sprint Burndown 更新**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新: Burndown Chart 加入 US 完成記錄
  - 格式:
    ```markdown
    | [日期] | [剩餘 SP] | [累計完成 SP] | ✅ US X.X 完成 | [詳細說明] |
    ```

- [ ] **3. PROJECT-STATUS-REPORT 更新**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 更新章節:
    - "User Stories 完成狀態" 表格
    - "最近更新" 章節
    - "交付成果" 章節
  - 新增: US 完成的總結 (包含所有 Phases)

- [ ] **4. 完整代碼統計**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md` + `SPRINT-X-OVERVIEW.md`
  - 統計內容:
    - 總計新增 LOC (按層級: Domain, Application, Infrastructure, API, Tests)
    - 新增文件數量
    - 修改文件數量
    - Git commits 數量

- [ ] **5. 測試完整報告**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 內容:
    - 新增測試數量 (單元測試 + 集成測試)
    - 測試覆蓋率
    - 測試通過率
    - 關鍵測試用例說明

- [ ] **6. 驗收標準檢查**
  - 文件: `docs/user-stories/modules/module-XX.md`
  - 檢查: 所有驗收標準是否都已滿足
  - 記錄: 驗收測試結果
  - 簽核: PO 確認驗收通過

- [ ] **7. 用戶文檔更新** (如需要)
  - 文件: `README.md`, API 文檔, 使用手冊
  - 檢查: 是否有面向用戶的新功能
  - 更新: 使用說明、示例代碼、API 參考

- [ ] **8. Release Notes 準備** (如接近 Sprint 結束)
  - 文件: `claudedocs/releases/RELEASE-NOTES-vX.X.md`
  - 內容: 該 US 的用戶可見功能
  - 格式: 功能描述 + 使用示例 + Breaking Changes (如有)

---

## 🚨 特殊事件更新 (Event-Driven Updates)

### ✅ 範圍變更發生時 (Scope Change)
**時機**: 檢測到範圍變更 (即時)
**負責人**: Scrum Master + Tech Lead
**預估時間**: 1-2 小時 (含審批)

#### 檢查項目:

- [ ] **1. 填寫範圍變更申請**
  - 文件: 新建 `claudedocs/4-changes/SCR-[日期]-[序號].md`
  - 使用模板: `SCOPE-CHANGE-REQUEST-TEMPLATE.md`
  - 必填章節: 1-8 (根據影響等級調整)
  - 審批流程: 根據影響等級執行

- [ ] **2. CHANGE-LOG 記錄**
  - 文件: `claudedocs/4-changes/CHANGE-LOG.md`
  - 新增: 完整的 CHANGE-XXX 記錄
  - 章節:
    - 變更原因
    - 影響分析 (正面/中性/負面)
    - 審批記錄
    - 交付成果
    - 經驗教訓

- [ ] **3. Sprint Overview 更新**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 新增: "CHANGE-XXX: [變更標題]" 章節
  - 內容: 變更摘要、影響、當前狀態

- [ ] **4. User Stories 表格調整**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新:
    - Story Points 變化 (如有)
    - 計劃天數調整
    - 新增 🆕 標記 (如新增 US)
    - 新增 ⚠️ 標記 (如超出預期)

- [ ] **5. Burndown Chart 調整**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新: 如 SP 總數變化,更新理想燃盡線

- [ ] **6. PROJECT-STATUS-REPORT 更新**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 章節: "變更記錄" 或 "風險與挑戰"
  - 內容: 變更對項目整體的影響

- [ ] **7. Stakeholder 通知**
  - 執行: 按 SCR 模板中的溝通計劃
  - 通知對象: PO, PM, SM, Dev Team
  - 通知方式: Email + Meeting + Slack

- [ ] **8. 統計數據更新**
  - 文件: `claudedocs/4-changes/CHANGE-LOG.md`
  - 更新: "總體統計" 表格
  - 增加: 變更數量計數

### ✅ 風險或阻塞發生時 (Risk/Blocker)
**時機**: 發現風險或阻塞 (即時)
**負責人**: Scrum Master + 相關責任人
**預估時間**: 20-30 分鐘

#### 檢查項目:

- [ ] **1. 風險/問題記錄**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 章節: "風險與問題追蹤"
  - 格式:
    ```markdown
    | [ID] | [標題] | [類型] | [影響] | [狀態] | [負責人] | [緩解措施] | [更新日期] |
    ```

- [ ] **2. User Story 狀態標記**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 如影響 US: 狀態改為 ⚠️ 阻塞
  - 備註欄: 記錄阻塞原因和 Risk ID

- [ ] **3. Stakeholder 即時通知**
  - 如高影響: 立即通知 SM, PO, PM
  - 通知方式: Slack + Stand-up 說明
  - 後續: 安排問題解決會議

- [ ] **4. 緩解計劃記錄**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 內容: 具體緩解措施、責任人、預計解決時間

### ✅ Sprint 開始時 (Sprint Start)
**時機**: Sprint Planning 完成後 (當日)
**負責人**: Scrum Master + AI Assistant
**預估時間**: 30-45 分鐘

#### 檢查項目:

- [ ] **1. 創建新 Sprint Overview**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 使用模板: 複製上個 Sprint 的結構
  - 填寫: Sprint 目標、時間範圍、User Stories

- [ ] **2. User Stories 表格初始化**
  - 填寫: 所有計劃的 User Stories
  - 設定: Story Points, 計劃天數
  - 狀態: 全部設為 ⏳ 待開始

- [ ] **3. Burndown Chart 初始化**
  - 設定: Sprint 總 SP
  - 設定: 開始日期、結束日期
  - 計算: 理想燃盡線 (總 SP / Sprint 天數)

- [ ] **4. 風險預評估**
  - 章節: "風險與問題追蹤"
  - 內容: Sprint Planning 中識別的風險
  - 緩解措施: 預先規劃

- [ ] **5. PROJECT-STATUS-REPORT 更新**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 更新: "當前 Sprint" 資訊
  - 更新: Sprint 目標和計劃

### ✅ Sprint 結束時 (Sprint End)
**時機**: Sprint Review/Retrospective 前 (提前 1 天)
**負責人**: Scrum Master + Tech Lead + AI Assistant
**預估時間**: 2-3 小時

#### 檢查項目:

- [ ] **1. Sprint 完整總結**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 章節: "Sprint 總結"
  - 內容:
    - 目標達成情況
    - 完成的 User Stories
    - 未完成的 User Stories 及原因
    - Story Points 統計 (計劃 vs 實際)
    - Velocity 計算
    - 與前一個 Sprint 對比

- [ ] **2. 所有 Burndown 數據完整**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 檢查: 每個工作日都有記錄
  - 繪製: 實際 vs 理想燃盡曲線對比

- [ ] **3. 測試與質量報告**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 內容:
    - Sprint 新增測試總數
    - 測試覆蓋率趨勢
    - 發現和修復的 Bug 數量
    - Code Review 統計

- [ ] **4. 代碼交付統計**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 統計:
    - 總計新增 LOC
    - Git commits 數量
    - Pull Requests 數量
    - 涉及文件數

- [ ] **5. 變更記錄總結**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 如有變更: 列出所有 CHANGE-XXX
  - 分析: 變更對 Sprint 的影響

- [ ] **6. 風險與問題解決情況**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-OVERVIEW.md`
  - 更新: 所有風險/問題的最終狀態
  - 統計: 識別數量、解決數量、遺留數量

- [ ] **7. 經驗教訓 (Retrospective 輸入)**
  - 文件: `claudedocs/2-sprints/sprint-X/SPRINT-X-RETROSPECTIVE.md` (新建)
  - 章節:
    - 做得好的地方 (What Went Well)
    - 需要改進的地方 (What Could Be Improved)
    - 行動項目 (Action Items)

- [ ] **8. PROJECT-STATUS-REPORT 總更新**
  - 文件: `claudedocs/PROJECT-STATUS-REPORT.md`
  - 更新:
    - "Sprint 完成情況" 表格
    - "整體進度" 指標
    - "累計交付成果"
    - "下一步計劃" (指向下個 Sprint)

- [ ] **9. Release Notes 準備** (如到版本發布)
  - 文件: `claudedocs/releases/RELEASE-NOTES-vX.X.md`
  - 內容: 整個 Sprint 的用戶可見功能
  - 格式: 按模組分類,包含示例

---

## 📊 自動化與工具 (Automation and Tools)

### 🤖 AI Assistant 自動化任務
**執行者**: Claude (AI Assistant)
**觸發方式**: 定時或事件驅動

#### 自動化項目:

1. **每日自動統計** (每日 15:00)
   - Git commits 統計 (`git log --since="yesterday" --oneline | wc -l`)
   - 代碼行數統計 (`git diff --stat HEAD~1 HEAD`)
   - 測試執行結果解析

2. **Phase 完成檢測** (實時)
   - 監聽 Git commits 中的關鍵詞: `feat(US-X.X): Complete Phase Y`
   - 自動觸發 Phase 完成檢查清單
   - 提醒 Tech Lead 更新文檔

3. **User Story 完成檢測** (實時)
   - 監聽 Git commits 中的關鍵詞: `feat(US-X.X): Complete all phases`
   - 自動觸發 US 完成檢查清單
   - 通知 SM 和 PO 進行驗收

4. **文檔同步檢查** (每日 17:00)
   - 比對代碼變更和文檔更新時間
   - 如代碼更新但文檔未更新 >24 小時,發出提醒
   - 生成待更新文檔清單

5. **Burndown Chart 自動更新** (每日 15:30)
   - 根據 User Stories 狀態自動計算剩餘 SP
   - 更新 `SPRINT-X-OVERVIEW.md` 中的 Burndown 表格
   - 計算實際 vs 理想進度差異

### 🛠️ 手動工具與命令

#### Git 統計命令:
```bash
# 查看某個 US 的所有 commits
git log --oneline --grep="US-2.1"

# 統計某個 US 的代碼變更
git diff --stat $(git log --grep="US-2.1" --format="%H" | tail -1)..HEAD

# 查看本週的代碼變更
git log --since="1 week ago" --stat

# 統計測試文件變更
git diff --stat HEAD~10 HEAD -- "*Tests.cs"
```

#### 代碼統計命令:
```bash
# 使用 cloc 統計代碼行數
cloc src/ --by-file

# 統計某個層級的代碼
cloc src/AIAgentPlatform.Domain/

# 統計測試代碼
cloc tests/
```

#### 測試執行:
```bash
# 執行所有測試並生成報告
dotnet test --logger "console;verbosity=detailed" > test-results.txt

# 執行覆蓋率測試
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover
```

---

## ⚠️ 常見問題與解決 (FAQ and Troubleshooting)

### Q1: 發現文檔已經過時很久,如何補救?
**A1: 緊急文檔同步流程**
1. 立即停止新功能開發
2. Tech Lead + AI Assistant 協作完成完整同步
3. 使用 Git history 還原實際開發時間線
4. 按時間順序補充所有缺失的文檔更新
5. 預估時間: 1-2 天 (取決於落後程度)

### Q2: 不確定某個文件該由誰更新?
**A2: 責任分工參考**
- **技術類** (架構、代碼、測試) → Tech Lead
- **流程類** (Sprint, 變更記錄) → Scrum Master
- **業務類** (需求、驗收) → Product Owner
- **統計類** (自動化數據) → AI Assistant
- **不確定** → 預設由 Scrum Master 協調

### Q3: 更新頻率太高,影響開發效率怎麼辦?
**A3: 批次更新策略**
- **每日必做**: 只更新 Burndown 和進度 (5-10 分鐘)
- **Phase 完成時**: 集中更新該 Phase 相關文檔 (15-20 分鐘)
- **US 完成時**: 預留 1 小時進行完整文檔更新
- **利用工具**: 最大化使用 AI Assistant 自動化

### Q4: 範圍變更申請流程太複雜,可以簡化嗎?
**A4: 根據影響等級調整**
- **🟢 低影響**: 簡化流程,只填寫 SCR 模板的 1, 2, 4, 8 章節
- **🟡 中影響**: 標準流程,建議完整填寫
- **🔴🔥 高影響/緊急**: 完整流程,必須詳細記錄

### Q5: 如何避免文檔更新遺漏?
**A5: 三重保障機制**
1. **AI Assistant 自動提醒**: 每日檢查並提醒待更新項
2. **Stand-up 檢查**: SM 在 Stand-up 時詢問文檔更新狀態
3. **Sprint Review 前檢查**: 使用本檢查清單完整審查

---

## 📈 效果評估 (Effectiveness Evaluation)

### 成功指標 (Success Metrics)

| 指標 | 目標值 | 測量方式 | 評估頻率 |
|-----|--------|---------|---------|
| **文檔更新及時性** | <24 小時 | 代碼提交時間 vs 文檔更新時間 | 每週 |
| **文檔完整性** | 100% | 檢查清單完成率 | Sprint 結束 |
| **文檔準確性** | >95% | 代碼審查時驗證文檔與代碼一致性 | 每週 |
| **更新所需時間** | <10% 開發時間 | 統計文檔更新總耗時 | Sprint 結束 |
| **Stakeholder 滿意度** | >4/5 分 | Retrospective 調查 | Sprint 結束 |

### 持續改進 (Continuous Improvement)

- **每 Sprint 回顧**: 在 Retrospective 中討論文檔流程
- **流程優化**: 根據團隊反饋調整檢查清單
- **自動化增強**: 識別可自動化的重複性任務
- **模板改進**: 根據使用經驗優化文檔模板

---

## 📚 相關資源 (Related Resources)

### 文檔模板:
- [SCOPE-CHANGE-REQUEST-TEMPLATE.md](../4-changes/SCOPE-CHANGE-REQUEST-TEMPLATE.md): 範圍變更申請模板
- [SPRINT-OVERVIEW-TEMPLATE.md](../2-sprints/SPRINT-OVERVIEW-TEMPLATE.md): Sprint 概覽模板 (如有)
- [ADR-TEMPLATE.md](../3-technical-decisions/ADR-TEMPLATE.md): 架構決策記錄模板 (如有)

### 主要文檔位置:
- **Sprint 文檔**: `claudedocs/2-sprints/sprint-X/`
- **變更記錄**: `claudedocs/4-changes/CHANGE-LOG.md`
- **項目狀態**: `claudedocs/PROJECT-STATUS-REPORT.md`
- **User Stories**: `docs/user-stories/modules/`
- **技術決策**: `claudedocs/3-technical-decisions/`

### 工具與命令:
- **Git 統計**: `git log`, `git diff --stat`
- **代碼統計**: `cloc`, `tokei`
- **測試報告**: `dotnet test --logger`
- **覆蓋率**: `dotnet test /p:CollectCoverage=true`

---

## 📝 檢查清單使用記錄 (Checklist Usage Log)

### 本週使用情況 (Weekly Usage)
**週次**: Week [X] (2025-12-[開始日] to 2025-12-[結束日])

| 日期 | 檢查類型 | 完成項目 | 責任人 | 耗時 | 備註 |
|-----|---------|---------|--------|------|------|
| [日期] | 每日更新 | 5/5 | Claude | 10 min | 全部完成 |
| [日期] | Phase 完成 | 5/5 | Tech Lead | 15 min | US 2.1 Phase 3 |
| [日期] | US 完成 | 8/8 | Tech Lead + SM | 1 hr | US 2.1 完整 |

### 改進建議 (Improvement Suggestions)
**日期**: [YYYY-MM-DD]
**提出人**: [姓名]
**建議內容**: [具體建議]
**處理狀態**: ⏳ 待處理 / 🔄 處理中 / ✅ 已採納 / ❌ 不採納

---

**模板版本**: 1.0.0
**最後更新**: 2025-12-11
**維護者**: Scrum Master
**審核者**: Tech Lead + Product Owner

---

## 附錄:快速參考卡片 (Quick Reference Card)

### 📌 每日 15 分鐘文檔更新流程:
1. ✅ 更新 Burndown Chart (2 min)
2. ✅ 更新 US 進度 (2 min)
3. ✅ 記錄當日完成任務 (3 min)
4. ✅ 更新代碼統計 (如有 commits) (3 min)
5. ✅ 更新測試統計 (如有新測試) (3 min)
6. ✅ 記錄阻塞/風險 (如有) (2 min)

### 📌 Phase 完成 20 分鐘流程:
1. ✅ 標記 Phase 完成 (2 min)
2. ✅ 驗證 Git commits (3 min)
3. ✅ 記錄測試結果 (5 min)
4. ✅ 更新代碼統計 (5 min)
5. ✅ 檢查技術文檔 (5 min)

### 📌 US 完成 60 分鐘流程:
1. ✅ 更新 US 狀態 (5 min)
2. ✅ 更新 Burndown (5 min)
3. ✅ 更新 PROJECT-STATUS-REPORT (10 min)
4. ✅ 完整代碼統計 (10 min)
5. ✅ 測試完整報告 (10 min)
6. ✅ 驗收標準檢查 (10 min)
7. ✅ 用戶文檔更新 (5 min)
8. ✅ Release Notes 準備 (5 min)

### 📌 範圍變更 2 小時流程:
1. ✅ 填寫 SCR (30 min)
2. ✅ 更新 CHANGE-LOG (20 min)
3. ✅ 更新 Sprint Overview (15 min)
4. ✅ 調整 Burndown (10 min)
5. ✅ 更新 PROJECT-STATUS-REPORT (15 min)
6. ✅ Stakeholder 通知 (20 min)
7. ✅ 統計數據更新 (10 min)
