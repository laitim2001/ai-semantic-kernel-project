# SPRINT-3-4-CHECKLIST.md - Sprint 3 任務清單：日級別執行追蹤

**版本**: v2.1
**Sprint 編號**: Sprint 3
**Sprint 週期**: Week 7-9 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2025-11-18 ~ 2025-12-08
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-13
**最後更新**: 2025-11-13

---

## 📋 目錄 (Table of Contents)

1. [Week 7 任務清單](#week-7-任務清單)
   - [Day 1-5: Domain Entities、Repository、Commands/Queries](#day-1-monday---domain-entities-設計)
2. [Week 8 任務清單](#week-8-任務清單)
   - [Day 6-10: Plugin Loader、MCP、Activator、Registry](#day-6-monday---plugin-loader-設計)
3. [Week 9 任務清單](#week-9-任務清單)
   - [Day 11-15: API Controllers、安全性、Bug修復、Sprint回顧](#day-11-monday---api-controllers-實作)
4. [Daily Checklist Template](#daily-checklist-template)
5. [Definition of Done](#definition-of-done)
6. [完整參考文獻索引](#完整參考文獻索引)
7. [使用指南](#使用指南)
8. [版本歷史](#版本歷史)

---

## Week 7 任務清單

### 🎯 本週目標：Domain Layer + Application Layer 基礎

**預期完成**: 40% Sprint 進度 (~5 SP)

#### Day 1 (Monday) - Domain Entities 設計

**Backend Team**:
- [ ] **Task 1.1**: 建立 Plugin 實體 (Entity)
  - [ ] 定義 `Plugin` 實體屬性（Id, Name, Version, Status, etc.）
  - [ ] 定義 `PluginMetadata` 值對象 (Value Object)
  - [ ] 實現 Plugin 狀態機 (Draft → Active → Inactive → Archived)
  - 參考: docs/architecture/domain-model.md
  - 參考: docs/user-stories/us-2-plugin-system.md
  - 估時: 4 小時

- [ ] **Task 1.2**: 建立 Plugin 值對象 (Value Objects)
  - [ ] `PluginIdentifier` (封裝 Name + Version)
  - [ ] `PluginDependency` (NuGet 依賴)
  - [ ] `PluginPermission` (權限控制)
  - 參考: docs/architecture/Architecture-Design-Document.md (Section 3.2)
  - 估時: 3 小時

- [ ] **Task 1.3**: 單元測試 (Domain Layer)
  - [ ] Plugin 實體單元測試 (覆蓋率 ≥85%)
  - [ ] 值對象單元測試
  - [ ] 狀態轉換測試
  - 參考: docs/testing/unit-testing-guidelines.md
  - 估時: 2 小時

**驗收標準**:
- ✅ Domain 實體定義完整
- ✅ 單元測試通過，覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

#### Day 2 (Tuesday) - Repository 實作

**Backend Team**:
- [ ] **Task 2.1**: 建立 Repository 接口
  - [ ] `IPluginRepository` 接口定義 (CRUD + Query)
  - [ ] `IPluginMetadataRepository` 接口
  - 參考: docs/architecture/repository-pattern.md
  - 估時: 2 小時

- [ ] **Task 2.2**: 實作 EF Core Repository
  - [ ] `PluginRepository` 實現 (EF Core)
  - [ ] `PluginMetadataRepository` 實現
  - [ ] Database Migrations
  - 參考: docs/technical-implementation/01-backend-net9/03-data-layer-persistence.md
  - 參考: docs/database/database-schema.md
  - 估時: 4 小時

- [ ] **Task 2.3**: Repository 整合測試
  - [ ] 測試 CRUD 操作
  - [ ] 測試查詢性能 (<100ms)
  - 參考: docs/testing/integration-testing-strategy.md
  - 估時: 2 小時

**驗收標準**:
- ✅ Repository CRUD 功能完整
- ✅ Migration 成功執行
- ✅ 整合測試通過

---

#### Day 3 (Wednesday) - Application Layer Commands

**Backend Team**:
- [ ] **Task 3.1**: 建立 CQRS Commands
  - [ ] `CreatePluginCommand` + Handler
  - [ ] `UpdatePluginCommand` + Handler
  - [ ] `DeletePluginCommand` + Handler
  - 參考: docs/technical-implementation/01-backend-net9/02-application-layer.md
  - 參考: docs/architecture/cqrs-pattern.md
  - 估時: 4 小時

- [ ] **Task 3.2**: 建立 Command 驗證
  - [ ] FluentValidation 驗證規則
  - [ ] Business Rules 驗證 (唯一性, 權限, etc.)
  - 參考: docs/standards/validation-guidelines.md
  - 估時: 2 小時

- [ ] **Task 3.3**: Command Handler 單元測試
  - [ ] 測試 Happy Path
  - [ ] 測試 Error Cases (驗證失敗, 業務規則違反)
  - 估時: 2 小時

**驗收標準**:
- ✅ Commands 定義完整
- ✅ FluentValidation 覆蓋所有驗證規則
- ✅ 單元測試覆蓋率 ≥85%

---

#### Day 4 (Thursday) - Application Layer Queries

**Backend Team**:
- [ ] **Task 4.1**: 建立 CQRS Queries
  - [ ] `GetPluginByIdQuery` + Handler
  - [ ] `GetAllPluginsQuery` + Handler (分頁, 篩選, 排序)
  - [ ] `SearchPluginsQuery` + Handler (全文搜尋)
  - 參考: docs/technical-implementation/01-backend-net9/02-application-layer.md
  - 估時: 3 小時

- [ ] **Task 4.2**: DTO 定義與映射
  - [ ] `PluginDto`, `PluginMetadataDto`
  - [ ] AutoMapper 配置
  - 參考: docs/standards/dto-guidelines.md
  - 估時: 2 小時

- [ ] **Task 4.3**: Query Handler 單元測試
  - [ ] 測試查詢邏輯
  - [ ] 測試映射正確性
  - 估時: 2 小時

**驗收標準**:
- ✅ Queries 定義完整
- ✅ DTO 映射正確
- ✅ 單元測試通過

---

#### Day 5 (Friday) - Week 7 回顧與整合

**Backend Team**:
- [ ] **Task 5.1**: 整合測試 (Application Layer)
  - [ ] 端到端測試 (Command → Repository → Query)
  - [ ] 測試異常場景
  - 參考: docs/testing/integration-testing-strategy.md
  - 估時: 3 小時

- [ ] **Task 5.2**: Code Review 與重構
  - [ ] Peer Code Review (至少 2 名 Reviewer)
  - [ ] 修復 Critical/High Issues
  - [ ] 重構重複代碼
  - 參考: docs/standards/code-review-checklist.md
  - 估時: 2 小時

- [ ] **Task 5.3**: 週回顧會議
  - [ ] Demo 已完成功能
  - [ ] 識別阻礙 (Blockers)
  - [ ] 調整 Week 8 計劃
  - 估時: 1 小時

**週末 Checkpoint**:
- ✅ Domain + Application Layer 完成度 ≥90%
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過率 100%
- ✅ 0 Critical/High Bugs

---

## Week 8 任務清單

### 🎯 本週目標：Plugin 載入引擎 + MCP 整合

**預期完成**: 75% Sprint 進度 (~10 SP)

#### Day 6 (Monday) - Plugin Loader 設計

**Backend Team**:
- [ ] **Task 6.1**: 設計 Plugin Loader 架構
  - [ ] `IPluginLoader` 接口定義
  - [ ] AppDomain 隔離策略設計
  - [ ] Plugin 生命週期管理設計
  - 參考: docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md
  - 參考: docs/architecture/adr/ADR-009-plugin-isolation-strategy.md
  - 估時: 3 小時

- [ ] **Task 6.2**: 實作 AppDomain 隔離
  - [ ] 建立 `PluginAppDomain` 類
  - [ ] 實現 Plugin 載入/卸載
  - [ ] 實現跨 AppDomain 通訊 (Marshaling)
  - 參考: docs/security/plugin-security-isolation.md
  - 估時: 4 小時

- [ ] **Task 6.3**: Plugin Loader 單元測試
  - [ ] 測試 Plugin 載入成功/失敗
  - [ ] 測試 AppDomain 隔離效果
  - 估時: 2 小時

**驗收標準**:
- ✅ AppDomain 隔離機制有效
- ✅ Plugin 載入/卸載正常
- ✅ 單元測試通過

---

#### Day 7 (Tuesday) - MCP 協議整合

**Backend Team**:
- [ ] **Task 7.1**: MCP 協議解析
  - [ ] 實現 MCP JSON Schema 解析
  - [ ] 實現 MCP Tool Definition 轉換
  - 參考: docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md
  - 參考: https://spec.modelcontextprotocol.io/specification/
  - 估時: 4 小時

- [ ] **Task 7.2**: MCP Plugin Adapter
  - [ ] `McpPluginAdapter` 類實現
  - [ ] MCP Tool → Internal Plugin 轉換
  - 估時: 3 小時

- [ ] **Task 7.3**: MCP 整合測試
  - [ ] 使用真實 MCP Plugin 測試
  - [ ] 測試 Tool Definition 正確性
  - 參考: docs/testing/integration-testing-strategy.md
  - 估時: 2 小時

**驗收標準**:
- ✅ MCP Plugin 可以正常載入
- ✅ Tool Definition 解析正確
- ✅ 整合測試通過

---

#### Day 8 (Wednesday) - Plugin Activator 實作

**Backend Team**:
- [ ] **Task 8.1**: Plugin Activator 設計
  - [ ] `IPluginActivator` 接口定義
  - [ ] Plugin 實例化邏輯
  - [ ] DI Container 整合
  - 參考: docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md
  - 估時: 3 小時

- [ ] **Task 8.2**: Plugin Activator 實作
  - [ ] `PluginActivator` 類實現
  - [ ] Reflection 實例化
  - [ ] 依賴注入處理
  - 估時: 3 小時

- [ ] **Task 8.3**: Activator 單元測試
  - [ ] 測試 Plugin 實例化
  - [ ] 測試 DI 注入
  - 估時: 2 小時

**驗收標準**:
- ✅ Plugin 可以成功實例化
- ✅ DI 依賴正確注入
- ✅ 單元測試通過

---

#### Day 9 (Thursday) - Plugin Registry 實作

**Backend Team**:
- [ ] **Task 9.1**: Plugin Registry 設計
  - [ ] `IPluginRegistry` 接口定義
  - [ ] Plugin Discovery 機制
  - [ ] Plugin Metadata 索引
  - 參考: docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md
  - 估時: 2 小時

- [ ] **Task 9.2**: Plugin Registry 實作
  - [ ] `PluginRegistry` 類實現
  - [ ] Plugin 註冊/取消註冊
  - [ ] Plugin 查詢 (By Name, By Version, By Tag)
  - 估時: 4 小時

- [ ] **Task 9.3**: Registry 單元測試
  - [ ] 測試註冊/取消註冊
  - [ ] 測試查詢功能
  - 估時: 2 小時

**驗收標準**:
- ✅ Plugin Registry 功能完整
- ✅ 查詢性能 <100ms
- ✅ 單元測試通過

---

#### Day 10 (Friday) - Week 8 整合與測試

**Backend Team**:
- [ ] **Task 10.1**: E2E 整合測試
  - [ ] 測試完整 Plugin 生命週期 (Load → Activate → Execute → Deactivate → Unload)
  - [ ] 測試 MCP Plugin 執行
  - 參考: docs/testing/e2e-testing-guide.md
  - 估時: 4 小時

- [ ] **Task 10.2**: 性能測試
  - [ ] Plugin 載入時間 <2 秒
  - [ ] Plugin 執行延遲 <500ms
  - 參考: docs/technical-implementation/08-performance-optimization/01-performance-requirements.md
  - 估時: 2 小時

- [ ] **Task 10.3**: Code Review 與重構
  - [ ] Peer Review
  - [ ] 修復 Issues
  - 估時: 2 小時

**週末 Checkpoint**:
- ✅ Plugin Loader 完成度 ≥95%
- ✅ MCP 整合完成
- ✅ E2E 測試通過
- ✅ 性能指標達標

---

## Week 9 任務清單

### 🎯 本週目標：API Layer + 文檔 + 收尾

**預期完成**: 100% Sprint 進度 (~13 SP)

#### Day 11 (Monday) - API Controllers 實作

**Backend Team**:
- [ ] **Task 11.1**: Plugins Controller
  - [ ] `POST /api/v1/plugins` (Create Plugin)
  - [ ] `GET /api/v1/plugins/{id}` (Get Plugin)
  - [ ] `GET /api/v1/plugins` (List Plugins with pagination)
  - [ ] `PUT /api/v1/plugins/{id}` (Update Plugin)
  - [ ] `DELETE /api/v1/plugins/{id}` (Delete Plugin)
  - 參考: docs/api/plugin-api-specification.md
  - 估時: 4 小時

- [ ] **Task 11.2**: Plugin Lifecycle Controller
  - [ ] `POST /api/v1/plugins/{id}/activate` (Activate Plugin)
  - [ ] `POST /api/v1/plugins/{id}/deactivate` (Deactivate Plugin)
  - 參考: docs/api/plugin-api-specification.md
  - 估時: 2 小時

- [ ] **Task 11.3**: API 單元測試
  - [ ] Controller 單元測試 (Mock Services)
  - [ ] 測試 HTTP 狀態碼正確性
  - 估時: 2 小時

**驗收標準**:
- ✅ 6 個 API 端點全部實現
- ✅ HTTP 狀態碼正確
- ✅ 單元測試通過

---

#### Day 12 (Tuesday) - API 整合測試與文檔

**Backend Team + QA**:
- [ ] **Task 12.1**: API 整合測試
  - [ ] 使用 Postman/REST Client 測試所有端點
  - [ ] 測試錯誤處理 (404, 400, 500)
  - [ ] 測試驗證邏輯 (FluentValidation)
  - 參考: docs/testing/api-testing-guide.md
  - 估時: 3 小時

- [ ] **Task 12.2**: Swagger 文檔完善
  - [ ] 添加 XML 註解
  - [ ] 配置 Swagger UI
  - [ ] 生成 API 文檔
  - 參考: docs/api/api-documentation-standards.md
  - 估時: 2 小時

- [ ] **Task 12.3**: Postman Collection
  - [ ] 建立 Postman Collection
  - [ ] 添加示例請求/響應
  - 估時: 1 小時

**驗收標準**:
- ✅ API 整合測試通過率 100%
- ✅ Swagger 文檔完整
- ✅ Postman Collection 可用

---

#### Day 13 (Wednesday) - 安全性加固

**Backend Team + Security Lead**:
- [ ] **Task 13.1**: Plugin 權限驗證
  - [ ] 實現 Plugin Permission 檢查
  - [ ] 防止未授權的 Plugin 操作
  - 參考: docs/security/plugin-security-isolation.md
  - 估時: 3 小時

- [ ] **Task 13.2**: Plugin 簽名驗證
  - [ ] 實現 Plugin Assembly 簽名檢查
  - [ ] 防止加載未簽名的 Plugin
  - 參考: docs/security/plugin-signing-verification.md
  - 估時: 3 小時

- [ ] **Task 13.3**: 安全測試
  - [ ] 嘗試載入惡意 Plugin (應被阻止)
  - [ ] 測試 AppDomain 隔離效果
  - 參考: docs/testing/security-testing-guide.md
  - 估時: 2 小時

**驗收標準**:
- ✅ 未簽名 Plugin 無法載入
- ✅ 權限驗證有效
- ✅ 安全測試通過

---

#### Day 14 (Thursday) - Bug 修復與優化

**Backend Team**:
- [ ] **Task 14.1**: Bug Triage & Fix
  - [ ] 修復所有 Critical Bugs (如有)
  - [ ] 修復所有 High Bugs (如有)
  - [ ] Medium Bugs 評估是否修復或延後
  - 參考: docs/standards/bug-severity-guidelines.md
  - 估時: 4 小時

- [ ] **Task 14.2**: 性能優化
  - [ ] 優化 Plugin 載入時間
  - [ ] 優化 Plugin 查詢性能
  - [ ] 優化記憶體使用
  - 參考: docs/technical-implementation/08-performance-optimization/02-performance-profiling.md
  - 估時: 3 小時

- [ ] **Task 14.3**: Code Quality 檢查
  - [ ] SonarQube 掃描
  - [ ] 修復 Code Smell
  - [ ] Code Coverage 補強 (目標 ≥85%)
  - 參考: docs/standards/code-quality-standards.md
  - 估時: 2 小時

**驗收標準**:
- ✅ 0 Critical/High Bugs
- ✅ 性能指標達標
- ✅ Code Coverage ≥85%

---

#### Day 15 (Friday) - Sprint 回顧與 Demo

**全體團隊**:
- [ ] **Task 15.1**: Sprint Demo 準備
  - [ ] 準備 Demo Script
  - [ ] 準備 Demo Data
  - [ ] 錄製 Demo Video (可選)
  - 估時: 2 小時

- [ ] **Task 15.2**: Sprint Demo
  - [ ] 展示 Plugin CRUD 功能
  - [ ] 展示 Plugin 載入與執行
  - [ ] 展示 MCP Plugin 整合
  - [ ] 展示 Swagger API 文檔
  - 估時: 1 小時

- [ ] **Task 15.3**: Sprint Retrospective
  - [ ] What went well?
  - [ ] What could be improved?
  - [ ] Action items for next Sprint
  - 參考: docs/project-management/retrospective-template.md
  - 估時: 1.5 小時

- [ ] **Task 15.4**: Sprint 文檔歸檔
  - [ ] 完成 SPRINT-3-DEV-LOG.md
  - [ ] 完成 SPRINT-3-ISSUES.md
  - [ ] 完成 SPRINT-3-RETROSPECTIVE.md
  - 估時: 2 小時

**Sprint 完成 Checkpoint**:
- ✅ 所有 AC (Acceptance Criteria) 驗收通過
- ✅ Sprint Demo 成功
- ✅ Retrospective 完成
- ✅ Sprint 文檔歸檔完成

---

## Daily Checklist Template

### 每日站會 (Daily Standup) - 9:00 AM (15 分鐘)

```yaml
參與者: Backend Team (3 名開發者) + Scrum Master

議程:
  1. 昨天完成了什麼? (每人 2 分鐘)
  2. 今天計劃做什麼? (每人 2 分鐘)
  3. 有什麼阻礙? (每人 1 分鐘)
  4. Sprint Burndown Chart 更新

阻礙升級:
  - Critical Blocker → 立即升級給 Tech Lead
  - High Blocker → 當天內解決或升級
  - Medium Blocker → 追蹤並在 2 天內解決

參考: docs/project-management/daily-standup-guidelines.md
```

### 每日收尾 (End of Day) - 5:30 PM (15 分鐘)

```yaml
每日收尾清單:
  1. 更新 JIRA/Azure DevOps 任務狀態
  2. 提交代碼到 Git (包含有意義的 Commit Message)
  3. 更新 Sprint Burndown Chart
  4. 記錄今日遇到的問題與解決方案 (在 SPRINT-3-ISSUES.md)
  5. 明天工作優先級確認

參考: docs/project-management/task-tracking-guidelines.md
```

---

## Definition of Done

### Task Level DoD (每個任務完成標準)

```yaml
Code Quality:
  - ✅ Code 符合 Coding Standards (參考: docs/standards/coding-standards.md)
  - ✅ 單元測試覆蓋率 ≥85%
  - ✅ 單元測試全部通過
  - ✅ Code Review 通過 (至少 1 名 Reviewer Approve)
  - ✅ 0 Critical/High SonarQube Issues
  - ✅ Code Complexity ≤15 (Cyclomatic Complexity)

Documentation:
  - ✅ 代碼包含 XML 註解 (Public API)
  - ✅ 複雜邏輯包含 Inline Comments
  - ✅ README 更新 (如適用)

Git:
  - ✅ 代碼提交到正確的 Feature Branch
  - ✅ Commit Message 遵循規範 (參考: docs/standards/git-commit-conventions.md)
  - ✅ Pull Request 建立並關聯 JIRA/Azure DevOps Task
```

### Story Level DoD (User Story 完成標準)

```yaml
Functionality:
  - ✅ 所有 Acceptance Criteria 驗收通過
  - ✅ 整合測試全部通過
  - ✅ E2E 測試通過 (如適用)
  - ✅ API 文檔完整 (Swagger)
  - ✅ Postman Collection 建立並測試通過

Quality:
  - ✅ 0 Critical Bugs
  - ✅ 0 High Bugs
  - ✅ Code Coverage ≥85%
  - ✅ Performance 指標達標 (參考: docs/technical-implementation/08-performance-optimization/01-performance-requirements.md)
  - ✅ Security 檢查通過 (參考: docs/security/security-testing-checklist.md)

Documentation:
  - ✅ 技術文檔更新 (Architecture, API, Database Schema)
  - ✅ User Story 標記為 Done
  - ✅ Sprint DEV-LOG 更新

Deployment:
  - ✅ 代碼合併到 develop 分支
  - ✅ CI/CD Pipeline 通過
  - ✅ 部署到 Dev 環境成功
```

### Sprint Level DoD (Sprint 完成標準)

```yaml
Deliverables:
  - ✅ 所有 Committed User Stories 完成 (US 1.3)
  - ✅ Sprint Demo 成功展示
  - ✅ Sprint Retrospective 完成
  - ✅ Sprint 文檔歸檔完成 (7 個文件)

Quality Gates:
  - ✅ All Tests Passed (Unit + Integration + E2E)
  - ✅ 0 Critical/High Bugs
  - ✅ Code Coverage ≥85%
  - ✅ Performance Benchmarks Met
  - ✅ Security Scan Passed

Documentation:
  - ✅ API 文檔完整 (Swagger)
  - ✅ Technical Documentation Updated
  - ✅ Sprint DEV-LOG 完成
  - ✅ Sprint ISSUES 記錄完成
  - ✅ Sprint RETROSPECTIVE 完成

Handoff:
  - ✅ Code Merged to develop
  - ✅ Deployed to Dev Environment
  - ✅ QA Team Handoff Completed
  - ✅ Product Owner Acceptance (UAT)

參考: docs/project-management/definition-of-done.md
```

---

## 完整參考文獻索引

> **重要提示**: 在執行 Sprint 3 任務時，請優先查閱 `/claudedocs/1-planning` 中的濃縮版規劃文檔，以快速理解專案整體背景和技術決策。完整的技術實施細節請參考 `/docs` 中的對應文檔。

### 📚 1. Planning 文檔（優先查閱 - 濃縮版）

**位置**: `/claudedocs/1-planning/`

這些文檔提供專案的整體規劃和策略，是理解 Sprint 3 目標和技術決策的關鍵。

| 文檔名稱 | 用途 | 何時查閱 |
|---------|------|---------|
| [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md) | MVP 範圍定義，6 Phases 詳細說明，Sprint 3 在 Phase 1A | 開始 Sprint 前必讀 |
| [SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) | Sprint 分配與時間線，Sprint 3 目標與依賴 | 規劃任務優先級時 |
| [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) | 開發策略，Plugin 系統整體架構設計 | 設計 Domain Layer 時 |
| [DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md) | 依賴關係矩陣，Sprint 3 對 Sprint 4-6 的影響 | 評估變更影響時 |
| [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md) | 風險登記冊，RISK-019 (MCP 整合複雜度), RISK-020 (Plugin 安全性) | 技術決策與風險評估時 |
| [TECHNICAL-DECISIONS-LOG.md](../../1-planning/TECHNICAL-DECISIONS-LOG.md) | 技術決策日誌，TD-005 (Plugin 隔離策略), TD-006 (MCP 協議選擇) | 實作技術方案時 |
| [ARCHITECTURE-EVOLUTION-ROADMAP.md](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) | 架構演進路線圖，Plugin 系統未來擴展計劃 | 設計擴展性時 |

---

### 📖 2. Sprint 文檔（Sprint 3 自身文檔）

**位置**: `/claudedocs/2-sprints/sprint-3/`

| 文檔名稱 | 用途 | 何時查閱 |
|---------|------|---------|
| [SPRINT-3-1-OVERVIEW.md](./SPRINT-3-1-OVERVIEW.md) | Sprint 3 概覽，目標與業務價值 | Sprint 開始前 |
| [SPRINT-3-2-PLAN.md](./SPRINT-3-2-PLAN.md) | Sprint 3 執行計劃，詳細任務分配 | 每日任務規劃時 |
| [SPRINT-3-3-CONTEXT.md](./SPRINT-3-3-CONTEXT.md) | Sprint 3 上下文，技術背景與決策依據 | 技術實作前 |
| **SPRINT-3-4-CHECKLIST.md** | **本文檔** - Sprint 3 任務清單 | **每日工作追蹤** |
| SPRINT-3-5-DEV-LOG.md | Sprint 3 開發日誌（Sprint 進行中更新） | 記錄每日進度時 |
| SPRINT-3-6-ISSUES.md | Sprint 3 問題追蹤（Sprint 進行中更新） | 遇到問題時記錄 |
| SPRINT-3-7-RETROSPECTIVE.md | Sprint 3 回顧（Sprint 結束後） | Sprint 回顧會議 |

---

### 📋 3. User Story 規格文檔

**位置**: `/docs/user-stories/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [us-2-plugin-system.md](../../../docs/user-stories/us-2-plugin-system.md) | US 1.3 Plugin 系統完整需求，AC (Acceptance Criteria) | 理解需求與驗收標準時 |

---

### 🏛️ 4. ADR（架構決策記錄）

**位置**: `/docs/architecture/adr/`

| 文檔名稱 | 決策內容 | 何時查閱 |
|---------|---------|---------|
| [ADR-009-plugin-isolation-strategy.md](../../../docs/architecture/adr/ADR-009-plugin-isolation-strategy.md) | Plugin 隔離策略 (AppDomain vs Process Isolation) | 實作 Plugin Loader 時 |
| [ADR-010-plugin-mcp-integration.md](../../../docs/architecture/adr/ADR-010-plugin-mcp-integration.md) | MCP 協議整合決策 | 實作 MCP Adapter 時 |
| [ADR-011-framework-abstraction-layer.md](../../../docs/architecture/adr/ADR-011-framework-abstraction-layer.md) | Framework Abstraction Layer 設計 | 設計 IPluginLoader 接口時 |

---

### 🔧 5. 技術實施文檔

**位置**: `/docs/technical-implementation/01-backend-net9/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [02-application-layer.md](../../../docs/technical-implementation/01-backend-net9/02-application-layer.md) | Application Layer 實作指南 (CQRS, MediatR) | 實作 Commands/Queries 時 |
| [03-data-layer-persistence.md](../../../docs/technical-implementation/01-backend-net9/03-data-layer-persistence.md) | Data Layer 實作指南 (EF Core, Repository Pattern) | 實作 Repository 時 |
| [04-plugin-system-mcp.md](../../../docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md) | Plugin 系統與 MCP 整合實作指南 | **核心實作文檔，必讀** |

---

### 🏗️ 6. 架構設計文檔

**位置**: `/docs/architecture/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [Architecture-Design-Document.md](../../../docs/architecture/Architecture-Design-Document.md) | 整體系統架構 (Clean Architecture + DDD + CQRS) | Sprint 開始前，理解整體架構 |
| [domain-model.md](../../../docs/architecture/domain-model.md) | 領域模型設計，Plugin 實體定義 | 實作 Domain Entities 時 |
| [repository-pattern.md](../../../docs/architecture/repository-pattern.md) | Repository Pattern 實作指南 | 實作 Repository 時 |
| [cqrs-pattern.md](../../../docs/architecture/cqrs-pattern.md) | CQRS Pattern 實作指南 | 實作 Commands/Queries 時 |

---

### 🌐 7. API 設計文檔

**位置**: `/docs/api/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [plugin-api-specification.md](../../../docs/api/plugin-api-specification.md) | Plugin API 規格（6 個端點定義） | 實作 API Controllers 時 |
| [api-documentation-standards.md](../../../docs/api/api-documentation-standards.md) | API 文檔標準 (Swagger, OpenAPI) | 撰寫 API 文檔時 |

---

### 🗄️ 8. Database 設計文檔

**位置**: `/docs/database/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [database-schema.md](../../../docs/database/database-schema.md) | Database Schema 設計，Plugins 和 PluginMetadata 資料表 | 建立 Migrations 時 |

---

### 🛡️ 9. 安全性文檔

**位置**: `/docs/security/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [plugin-security-isolation.md](../../../docs/security/plugin-security-isolation.md) | Plugin 安全隔離設計 (4-Layer Security) | 實作 AppDomain 隔離時 |
| [plugin-signing-verification.md](../../../docs/security/plugin-signing-verification.md) | Plugin 簽名驗證機制 | 實作簽名檢查時 |
| [security-testing-checklist.md](../../../docs/security/security-testing-checklist.md) | 安全測試清單 | 執行安全測試時 |

---

### 🧪 10. 測試文檔

**位置**: `/docs/testing/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [unit-testing-guidelines.md](../../../docs/testing/unit-testing-guidelines.md) | 單元測試指南 (xUnit, Moq, FluentAssertions) | 撰寫單元測試時 |
| [integration-testing-strategy.md](../../../docs/testing/integration-testing-strategy.md) | 整合測試策略 (WebApplicationFactory, TestContainers) | 撰寫整合測試時 |
| [e2e-testing-guide.md](../../../docs/testing/e2e-testing-guide.md) | E2E 測試指南 (Playwright, Postman) | 執行 E2E 測試時 |
| [api-testing-guide.md](../../../docs/testing/api-testing-guide.md) | API 測試指南 (REST Client, Postman) | 測試 API 時 |
| [security-testing-guide.md](../../../docs/testing/security-testing-guide.md) | 安全測試指南 (OWASP, Penetration Testing) | 執行安全測試時 |

---

### 📏 11. 開發標準文檔

**位置**: `/docs/standards/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [coding-standards.md](../../../docs/standards/coding-standards.md) | 編碼標準 (C# Style Guide, Naming Conventions) | 撰寫代碼前 |
| [code-review-checklist.md](../../../docs/standards/code-review-checklist.md) | Code Review 檢查清單 | 執行 Code Review 時 |
| [validation-guidelines.md](../../../docs/standards/validation-guidelines.md) | 驗證規則指南 (FluentValidation) | 實作驗證邏輯時 |
| [dto-guidelines.md](../../../docs/standards/dto-guidelines.md) | DTO 設計指南 (AutoMapper) | 定義 DTO 時 |
| [git-commit-conventions.md](../../../docs/standards/git-commit-conventions.md) | Git Commit Message 規範 | 提交代碼時 |
| [bug-severity-guidelines.md](../../../docs/standards/bug-severity-guidelines.md) | Bug 嚴重性分級標準 | Bug Triage 時 |
| [code-quality-standards.md](../../../docs/standards/code-quality-standards.md) | 代碼質量標準 (SonarQube, Code Coverage) | 代碼質量檢查時 |

---

### ⚡ 12. 性能優化文檔

**位置**: `/docs/technical-implementation/08-performance-optimization/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [01-performance-requirements.md](../../../docs/technical-implementation/08-performance-optimization/01-performance-requirements.md) | 性能需求定義，Plugin 載入 <2s, 執行 <500ms | 性能測試時 |
| [02-performance-profiling.md](../../../docs/technical-implementation/08-performance-optimization/02-performance-profiling.md) | 性能分析工具與方法 (BenchmarkDotNet, dotTrace) | 性能優化時 |

---

### 📊 13. 專案管理文檔

**位置**: `/docs/project-management/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [daily-standup-guidelines.md](../../../docs/project-management/daily-standup-guidelines.md) | 每日站會指南 | 每日站會前 |
| [task-tracking-guidelines.md](../../../docs/project-management/task-tracking-guidelines.md) | 任務追蹤指南 (JIRA/Azure DevOps) | 更新任務狀態時 |
| [retrospective-template.md](../../../docs/project-management/retrospective-template.md) | 回顧會議模板 | Sprint 回顧時 |
| [definition-of-done.md](../../../docs/project-management/definition-of-done.md) | Definition of Done 標準 | 檢查任務完成度時 |

---

### 🎨 14. UX 設計文檔

**位置**: `/docs/ux-design/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [user-research/personas.md](../../../docs/ux-design/user-research/personas.md) | 用戶角色定義 (Alex, Emma, Chris) | 理解用戶需求時 |
| [wireframes/low-fidelity/03-plugin-management.md](../../../docs/ux-design/wireframes/low-fidelity/03-plugin-management.md) | Plugin 管理 UI 設計（如需 Frontend 配合） | Frontend 整合時 |

---

### 🔄 15. Change Management

**位置**: `/claudedocs/4-changes/`

| 文檔名稱 | 內容 | 何時查閱 |
|---------|------|---------|
| [CHANGE-LOG.md](../../4-changes/CHANGE-LOG.md) | 變更日誌，追蹤所有變更 | 提交重大變更時 |
| [SCOPE-CHANGE-REQUEST-TEMPLATE.md](../../4-changes/SCOPE-CHANGE-REQUEST-TEMPLATE.md) | 範圍變更請求模板 | 需要變更 Sprint 範圍時 |

---

## 📌 快速查閱指南

### 🚀 任務開始前必讀
1. [SPRINT-3-1-OVERVIEW.md](./SPRINT-3-1-OVERVIEW.md) - 理解 Sprint 目標
2. [SPRINT-3-2-PLAN.md](./SPRINT-3-2-PLAN.md) - 理解任務分配
3. [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md) - 理解 MVP 範圍

### 🏗️ 技術實作時必讀
1. [04-plugin-system-mcp.md](../../../docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md) - **Plugin 系統核心實作指南**
2. [Architecture-Design-Document.md](../../../docs/architecture/Architecture-Design-Document.md) - 整體架構
3. [ADR-009-plugin-isolation-strategy.md](../../../docs/architecture/adr/ADR-009-plugin-isolation-strategy.md) - Plugin 隔離策略

### 🧪 測試時必讀
1. [unit-testing-guidelines.md](../../../docs/testing/unit-testing-guidelines.md) - 單元測試
2. [integration-testing-strategy.md](../../../docs/testing/integration-testing-strategy.md) - 整合測試
3. [security-testing-checklist.md](../../../docs/security/security-testing-checklist.md) - 安全測試

### 📝 代碼提交前必讀
1. [coding-standards.md](../../../docs/standards/coding-standards.md) - 編碼標準
2. [code-review-checklist.md](../../../docs/standards/code-review-checklist.md) - Code Review
3. [git-commit-conventions.md](../../../docs/standards/git-commit-conventions.md) - Git 規範

---

## 使用指南

### 📖 如何使用本文檔

本文檔是 **Sprint 3 的日級別任務清單**，提供每日 Checklist、DoD (Definition of Done) 與參考文獻快速導航。

#### 適用對象

**開發團隊 (Backend Team)**:
- **每日早上**：閱讀當日任務清單 (Day 1-15)
- **每日站會前**：更新任務勾選狀態 ([ ] → [x])
- **每日收尾**：執行 [Daily Checklist Template](#daily-checklist-template)
- **任務完成時**：檢查 [Definition of Done](#definition-of-done)

**Scrum Master**:
- **每日站會**：使用本文檔追蹤團隊進度
- **每週五**：檢查 Week Checkpoint (Week 7-9)
- **Sprint 結束**：更新實際完成狀態並歸檔

**Tech Lead**:
- **Code Review**：使用 [Definition of Done](#definition-of-done) 作為審查標準
- **技術指導**：參考 [完整參考文獻索引](#完整參考文獻索引) 提供技術文檔

#### 文檔關聯

```
SPRINT-3-2-PLAN.md (戰術層 - 3週執行計劃)
    ↓
SPRINT-3-4-CHECKLIST.md (本文檔 - 操作層 - 日級別任務)
    ↓
SPRINT-3-5-DEV-LOG.md (追蹤層 - 每日實際進度記錄)
```

#### 每日使用流程

**早上 9:00 - 站會前 (5分鐘)**:
1. 閱讀今日任務清單 (例如: Day 1 tasks)
2. 預估今日工作量與優先級
3. 標記昨日完成任務為 [x]

**每日站會 (15分鐘)**:
1. 分享昨日完成的任務 (已勾選的 checkboxes)
2. 分享今日計劃任務
3. 報告阻礙 (Blockers)

**下午 5:30 - 收尾 (15分鐘)**:
1. 執行 [Daily Checklist Template](#daily-checklist-template)
2. 更新任務狀態 ([ ] → [x])
3. 記錄問題到 SPRINT-3-6-ISSUES.md

#### 快速導航

- **查找今日任務** → 使用目錄跳轉到對應 Day
- **查找DoD標準** → 跳轉至 [Definition of Done](#definition-of-done)
- **查找技術文檔** → 使用 [完整參考文獻索引](#完整參考文獻索引)
- **每日站會流程** → 參考 [Daily Checklist Template](#daily-checklist-template)

---

## 版本歷史

### v2.1 (2025-11-13)
- ✅ 升級至 v2.1 統一標準
- ✅ 新增 8 欄位 Header (Sprint 編號、計劃日期等)
- ✅ 擴展目錄結構（新增子章節導航、使用指南、版本歷史）
- ✅ 新增「使用指南」區塊（適用對象、每日使用流程、快速導航）
- ✅ 格式統一：與 Sprint 1、Sprint 2 完全一致

### v2.0 (2025-11-13)
- ✅ 新增完整參考文獻索引（50+ 文檔，15 個類別）
- ✅ 詳細 DoD 定義 (Task/Story/Sprint 三層級)
- ✅ 日級別任務分解 (15 天完整 Checklist)

### v1.0 (2025-11-13)
- ✅ 初始版本建立
- ✅ 完整 3 週任務清單 (Week 7-9)
- ✅ Daily Checklist Template

---

**文檔建立日期**: 2025-11-13
**文檔維護者**: Scrum Master + Tech Lead
**下次更新**: Sprint 3 結束後更新實際完成狀態
