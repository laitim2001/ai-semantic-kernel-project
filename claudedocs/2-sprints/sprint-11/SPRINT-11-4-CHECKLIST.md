# SPRINT 11 - 任務檢查清單 (Task Checklist)

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

## 總體進度追蹤 (Overall Progress)

### Sprint 11 整體完成度

| Feature | 任務數 | 已完成 | 進行中 | 待開始 | 完成率 |
|---------|--------|--------|--------|--------|--------|
| Feature 1: Workflow Execution Trigger | 40 | 0 | 0 | 40 | 0% |
| Feature 2: Workflow Template Management | 35 | 0 | 0 | 35 | 0% |
| Feature 3: Version Control基礎 | 35 | 0 | 0 | 35 | 0% |
| Phase 1C 完成與驗收 | 20 | 0 | 0 | 20 | 0% |
| **總計** | **130** | **0** | **0** | **130** | **0%** |

### 關鍵里程碑追蹤

| 里程碑 | 目標日期 | 狀態 | 備註 |
|--------|----------|------|------|
| M11.1: Execution Trigger 完成 | Week 32 End | ⏳ 待開始 | 4 種 Trigger types |
| M11.2: Template Management 完成 | Week 33 Mid | ⏳ 待開始 | Template CRUD + Marketplace |
| M11.3: Version Control 完成 | Week 33 Mid | ⏳ 待開始 | Versioning + Rollback |
| M11.4: Phase 1C 完成驗收 | Week 33 End | ⏳ 待開始 | 100% 交付 |
| M11.5: Phase 1D 移交 | Week 33 End | ⏳ 待開始 | API 文檔 + Handoff |

---

## Feature 1: Workflow Execution Trigger (40 tasks)

**目標**: 實現 4 種 Workflow Execution Trigger
**工期**: Week 31-32 (10 days)
**Story Points**: 3-4 SP

### 1.1 Manual Trigger (10 tasks)

#### Domain Model
- [ ] **T11.001** - WorkflowExecution 實體設計
- [ ] **T11.002** - WorkflowExecutionRepository
- [ ] **T11.003** - Domain Events (Triggered/Started/Completed/Failed)

#### CQRS
- [ ] **T11.004** - TriggerManualWorkflowCommand
- [ ] **T11.005** - GetWorkflowExecutionByIdQuery
- [ ] **T11.006** - GetWorkflowExecutionsQuery (paged)

#### API Endpoints
- [ ] **T11.007** - POST /api/v1/workflow-executions/trigger
- [ ] **T11.008** - GET /api/v1/workflow-executions/{id}
- [ ] **T11.009** - GET /api/v1/workflow-executions

#### Testing
- [ ] **T11.010** - Integration Tests (Manual trigger + Query)

---

### 1.2 Schedule Trigger (Cron) (10 tasks)

#### Domain Model
- [ ] **T11.011** - WorkflowSchedule 實體設計
- [ ] **T11.012** - WorkflowScheduleRepository
- [ ] **T11.013** - Cron Expression Validator + Helper

#### CQRS
- [ ] **T11.014** - CreateWorkflowScheduleCommand
- [ ] **T11.015** - UpdateWorkflowScheduleCommand
- [ ] **T11.016** - DeleteWorkflowScheduleCommand

#### Background Service
- [ ] **T11.017** - WorkflowSchedulerService (BackgroundService)
- [ ] **T11.018** - Hangfire 整合評估 (alternative)

#### API Endpoints
- [ ] **T11.019** - Schedule Management API (POST/GET/PUT/DELETE)

#### Testing
- [ ] **T11.020** - Integration Tests (Schedule creation + Trigger)

---

### 1.3 Event Trigger (10 tasks)

#### Domain Model
- [ ] **T11.021** - WorkflowEventSubscription 實體設計
- [ ] **T11.022** - WorkflowEventSubscriptionRepository
- [ ] **T11.023** - EventMatchingService

#### Event Bus
- [ ] **T11.024** - IEventBus 介面設計
- [ ] **T11.025** - EventBus 實作 (Memory-based)
- [ ] **T11.026** - WorkflowEventHandler

#### CQRS
- [ ] **T11.027** - CreateWorkflowEventSubscriptionCommand
- [ ] **T11.028** - DeleteWorkflowEventSubscriptionCommand

#### API Endpoints
- [ ] **T11.029** - Event Subscription API (POST/GET/DELETE)

#### Testing
- [ ] **T11.030** - Integration Tests (Subscription + Event trigger)

---

### 1.4 Webhook Trigger (10 tasks)

#### Domain Model
- [ ] **T11.031** - WorkflowWebhook 實體設計
- [ ] **T11.032** - WorkflowWebhookRepository
- [ ] **T11.033** - WebhookSecurityService (HMAC-SHA256)

#### Webhook Endpoint
- [ ] **T11.034** - POST /webhooks/{webhookUrl}
- [ ] **T11.035** - Webhook Request Validation (Signature)
- [ ] **T11.036** - Webhook Response (202 Accepted + ExecutionId)

#### CQRS
- [ ] **T11.037** - CreateWorkflowWebhookCommand
- [ ] **T11.038** - DeleteWorkflowWebhookCommand

#### API Endpoints
- [ ] **T11.039** - Webhook Management API (POST/GET/DELETE)

#### Testing
- [ ] **T11.040** - Integration Tests (Webhook creation + POST trigger)

---

## Feature 2: Workflow Template Management (35 tasks)

**目標**: 實現 Template CRUD + Marketplace 基礎
**工期**: Week 32-33 (8 days)
**Story Points**: 3-4 SP

### 2.1 Template CRUD API (10 tasks)

#### Domain Model
- [ ] **T11.041** - WorkflowTemplate 實體設計
- [ ] **T11.042** - WorkflowTemplateCategory 實體設計
- [ ] **T11.043** - WorkflowTemplateRepository

#### CQRS
- [ ] **T11.044** - CreateWorkflowTemplateCommand
- [ ] **T11.045** - UpdateWorkflowTemplateCommand
- [ ] **T11.046** - GetWorkflowTemplatesQuery (paged + filtered)

#### API Endpoints
- [ ] **T11.047** - Template CRUD API (POST/GET/PUT/DELETE)
- [ ] **T11.048** - Category CRUD API (POST/GET/PUT/DELETE)
- [ ] **T11.049** - Template Discovery API (public templates)

#### Testing
- [ ] **T11.050** - Integration Tests (Template CRUD + Query)

---

### 2.2 Template Categories (5 tasks)

#### Seed Data
- [ ] **T11.051** - 創建內建 Template Categories (5 categories)

#### Category Hierarchy
- [ ] **T11.052** - Category Hierarchy 支持 (ParentCategoryId)
- [ ] **T11.053** - Category Validation (循環引用檢測)

#### API Enhancements
- [ ] **T11.054** - Category Tree API (GET /tree)

#### Testing
- [ ] **T11.055** - Integration Tests (Category tree + Hierarchy)

---

### 2.3 Import/Export (10 tasks)

#### Export Format
- [ ] **T11.056** - Template Export JSON Schema 定義
- [ ] **T11.057** - ExportTemplateService
- [ ] **T11.058** - Export Validation (移除敏感資訊)

#### Import Logic
- [ ] **T11.059** - ImportTemplateService
- [ ] **T11.060** - Import Validation (JSON schema)
- [ ] **T11.061** - Import Conflict Resolution

#### API Endpoints
- [ ] **T11.062** - Export API (GET /export)
- [ ] **T11.063** - Import API (POST /import)

#### Testing
- [ ] **T11.064** - Export Tests
- [ ] **T11.065** - Import Tests + Round-trip test

---

### 2.4 Marketplace 基礎 (10 tasks)

#### Template Rating
- [ ] **T11.066** - TemplateRating 實體設計
- [ ] **T11.067** - TemplateRatingRepository
- [ ] **T11.068** - Rating API (POST/GET/PUT)

#### Template Usage Statistics
- [ ] **T11.069** - TemplateUsage 實體設計
- [ ] **T11.070** - Usage Tracking (TrackTemplateUsageAsync)

#### Marketplace Discovery
- [ ] **T11.071** - Marketplace Search API
- [ ] **T11.072** - Popular Templates API
- [ ] **T11.073** - Top Rated Templates API

#### Testing
- [ ] **T11.074** - Rating Tests
- [ ] **T11.075** - Marketplace Tests (Search + Sort)

---

## Feature 3: Version Control基礎 (35 tasks)

**目標**: 實現 Workflow Versioning + Rollback
**工期**: Week 33 (7 days)
**Story Points**: 1-4 SP

### 3.1 Workflow Versioning (10 tasks)

#### Domain Model
- [ ] **T11.076** - WorkflowDefinition 版本化調整 (Version, IsActive)
- [ ] **T11.077** - WorkflowVersion 實體設計 (alternative)
- [ ] **T11.078** - Versioning Strategy 決策 (TD-087)

#### CQRS
- [ ] **T11.079** - UpdateWorkflowDefinitionCommand 調整 (創建新版本)
- [ ] **T11.080** - PublishWorkflowVersionCommand
- [ ] **T11.081** - GetWorkflowVersionsQuery

#### Repository
- [ ] **T11.082** - GetActiveVersionAsync()
- [ ] **T11.083** - GetAllVersionsAsync()

#### API Endpoints
- [ ] **T11.084** - Version Management API (GET /versions)

#### Testing
- [ ] **T11.085** - Integration Tests (Versioning + Update)

---

### 3.2 Version Comparison (10 tasks)

#### Comparison Service
- [ ] **T11.086** - WorkflowVersionComparisonService
- [ ] **T11.087** - JSON Diff Algorithm
- [ ] **T11.088** - Diff Result Model (WorkflowVersionDiff)

#### CQRS
- [ ] **T11.089** - CompareWorkflowVersionsQuery

#### API Endpoints
- [ ] **T11.090** - Version Comparison API (GET /compare)

#### UI-Friendly Diff
- [ ] **T11.091** - Diff Summary 生成
- [ ] **T11.092** - Diff Visualization Data

#### Testing
- [ ] **T11.093** - Diff Algorithm Tests
- [ ] **T11.094** - Integration Tests (Compare API)
- [ ] **T11.095** - Performance Tests (Large workflow)

---

### 3.3 Version Rollback (10 tasks)

#### Rollback Logic
- [ ] **T11.096** - RollbackWorkflowVersionCommand
- [ ] **T11.097** - Rollback Validation
- [ ] **T11.098** - Rollback History Tracking

#### CQRS
- [ ] **T11.099** - RollbackWorkflowVersionCommandHandler

#### API Endpoints
- [ ] **T11.100** - Rollback API (POST /rollback)

#### Rollback Confirmation
- [ ] **T11.101** - Rollback Preview (GET /rollback/preview)
- [ ] **T11.102** - Rollback Confirmation Required

#### Testing
- [ ] **T11.103** - Rollback Tests
- [ ] **T11.104** - Integration Tests (Rollback API)
- [ ] **T11.105** - End-to-end Rollback Test

---

### 3.4 Version History (5 tasks)

#### History Query
- [ ] **T11.106** - GetWorkflowVersionHistoryQuery
- [ ] **T11.107** - Version Timeline

#### API Endpoints
- [ ] **T11.108** - Version History API (GET /history)

#### Version Metadata
- [ ] **T11.109** - 增強版本 Metadata (CreatedBy, ChangeType)

#### Testing
- [ ] **T11.110** - Integration Tests (Version history + Timeline)

---

## Phase 1C 完成與驗收 (20 tasks)

### Sprint 10-11 交付物總驗收

#### Sprint 10 驗收
- [ ] **T11.111** - Workflow Definition CRUD 驗收
- [ ] **T11.112** - Node Registry System 驗收
- [ ] **T11.113** - Edge Validation Engine 驗收

#### Sprint 11 驗收
- [ ] **T11.114** - Workflow Execution Trigger 驗收
- [ ] **T11.115** - Workflow Template Management 驗收
- [ ] **T11.116** - Version Control 驗收

#### 整合測試
- [ ] **T11.117** - 完整 End-to-end 測試 (Create → Trigger → Execute)
- [ ] **T11.118** - 性能測試 (API 響應時間)
- [ ] **T11.119** - 安全測試 (Authentication + Webhook signature)

#### 代碼質量
- [ ] **T11.120** - Code Coverage 檢查 (≥85%)
- [ ] **T11.121** - Code Review 完成
- [ ] **T11.122** - StyleCop 零違規

#### 文檔
- [ ] **T11.123** - API 文檔更新 (Swagger)
- [ ] **T11.124** - TypeScript API Client 生成
- [ ] **T11.125** - Phase 1C 技術文檔完成

#### Phase 1D 移交準備
- [ ] **T11.126** - Handoff Checklist 準備
- [ ] **T11.127** - API 文檔移交包
- [ ] **T11.128** - Database Schema 移交包
- [ ] **T11.129** - Known Issues + Tech Debt 清單

#### Phase 1C 完成慶祝
- [ ] **T11.130** - Phase 1C Retrospective 完成
- [ ] **T11.131** - Team Celebration Meeting
- [ ] **T11.132** - Phase 1C Demo to Stakeholders

---

## Definition of Done (Sprint 11)

### 代碼質量
- [ ] 所有代碼已提交到 Git (feature/sprint-11 branch)
- [ ] Code Review 已完成 (至少 2 位 Reviewers)
- [ ] 代碼符合 C# Coding Standards
- [ ] 零 ReSharper 警告

### 測試要求
- [ ] 單元測試覆蓋率 ≥85%
- [ ] 所有整合測試通過 (0 failures)
- [ ] End-to-end 測試通過
- [ ] 性能測試通過 (API P95 <200ms)

### 文檔要求
- [ ] XML 文檔註解完整
- [ ] API 文檔生成 (Swagger)
- [ ] README 更新
- [ ] API 使用指南編寫完成

### Phase 1C 完成要求
- [ ] Sprint 10-11 所有交付物 100% 完成
- [ ] Phase 1C Critical Path 100% 完成
- [ ] 零 P0/P1 技術債務
- [ ] Phase 1D 準備度 ≥90%
- [ ] Sprint 11 Retrospective 完成
- [ ] Phase 1D Handoff Meeting 完成

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**總任務數**: 130 tasks
**預計完成度**: 0% (0/130 tasks completed)
**Phase 1C 完成**: Sprint 11 結束時達到 100%
