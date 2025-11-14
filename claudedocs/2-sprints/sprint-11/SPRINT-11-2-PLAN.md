# SPRINT 11 - 執行計劃 (Execution Plan)

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

## 📑 目錄 (Table of Contents)

1. [Sprint 11 Overview](#sprint-11-overview)
2. [Phase 1C 完成計劃](#phase-1c-完成計劃)
3. [Feature 1: Workflow Execution Trigger (3-4 SP)](#feature-1-workflow-execution-trigger-3-4-sp)
4. [Feature 2: Workflow Template Management (3-4 SP)](#feature-2-workflow-template-management-3-4-sp)
5. [Feature 3: Version Control基礎 (1-4 SP)](#feature-3-version-control基礎-1-4-sp)
6. [Phase 1C 完成與驗收](#phase-1c-完成與驗收)
7. [Phase 1D 移交準備](#phase-1d-移交準備)

---

## Sprint 11 Overview

### Sprint 目標

**主要目標**: 完成 Phase 1C Backend 開發,交付完整 Workflow Editor Backend API

**關鍵交付物**:
1. **Workflow Execution Trigger System** (3-4 SP)
   - Manual Trigger
   - Schedule Trigger (Cron)
   - Event Trigger
   - Webhook Trigger

2. **Workflow Template Management** (3-4 SP)
   - Template CRUD API
   - Template Categories
   - Import/Export
   - Marketplace基礎

3. **Version Control基礎** (1-4 SP)
   - Workflow Versioning
   - Version Comparison
   - Version Rollback
   - Version History

**Sprint Points**: 9.5 SP
**Sprint Duration**: Week 31-33 (3 週, 15 working days)
**Phase**: Phase 1C Final Sprint

---

### Sprint 11 在 Phase 1C 中的定位

**Phase 1C 整體規劃**:
```yaml
Sprint 10 (Week 28-30):
  Status: ✅ 完成
  Deliverables:
    - Workflow Definition CRUD
    - Node Registry System
    - Edge Validation Engine
  Story Points: 8 SP
  Phase 1C Progress: 50%

Sprint 11 (Week 31-33):
  Status: 🚀 本 Sprint
  Deliverables:
    - Workflow Execution Trigger
    - Workflow Template Management
    - Version Control基礎
  Story Points: 9.5 SP
  Phase 1C Progress: 100%

Phase 1C 總計:
  Duration: 6 weeks
  Story Points: 17.5 SP
  Completion: Sprint 11 結束時達到 100%
```

---

### Sprint 11 關鍵里程碑

| 里程碑 | 目標日期 | 交付物 | 成功標準 |
|--------|----------|--------|----------|
| M11.1: Execution Trigger 完成 | Week 32 End | 4 種 Trigger types + API | 100% 功能實現 + 測試通過 |
| M11.2: Template Management 完成 | Week 33 Mid | Template CRUD + Marketplace | 100% 功能實現 + 測試通過 |
| M11.3: Version Control 完成 | Week 33 Mid | Versioning + Rollback | 100% 功能實現 + 測試通過 |
| M11.4: Phase 1C 完成驗收 | Week 33 End | Phase 1C 100% 交付 | 所有驗收標準通過 |
| M11.5: Phase 1D 移交準備 | Week 33 End | API 文檔 + 移交 Checklist | Frontend 準備就緒 |

---

### Sprint 11 成功標準

**功能完整性**:
- ✅ 4 種 Execution Trigger 全部實現
- ✅ Template Management CRUD 完整
- ✅ Version Control 基礎功能可用
- ✅ 所有 API 端點正常運作

**質量標準**:
- ✅ 代碼覆蓋率 ≥85%
- ✅ API 響應時間 P95 <200ms
- ✅ 零 P0/P1 Bugs
- ✅ 所有整合測試通過

**文檔完整性**:
- ✅ API 文檔 (Swagger) 100% 完整
- ✅ Phase 1C 交付文檔完成
- ✅ Phase 1D Handoff Checklist 完成

**Phase 1C 完成標準**:
- ✅ Sprint 10-11 所有交付物驗收通過
- ✅ Phase 1C Critical Path 100% 完成
- ✅ 無已知 P0/P1 技術債務
- ✅ Phase 1D 準備度 ≥90%

---

## Phase 1C 完成計劃

### Phase 1C 回顧與完成慶祝

**Phase 1C 成果總結**:
```yaml
Duration: 6 weeks (Sprint 10-11)
Story Points: 17.5 SP
Team Size: 5 developers
Features Delivered:
  1. Workflow Definition CRUD (Sprint 10)
  2. Node Registry System (Sprint 10)
  3. Edge Validation Engine (Sprint 10)
  4. Workflow Execution Trigger (Sprint 11)
  5. Workflow Template Management (Sprint 11)
  6. Version Control基礎 (Sprint 11)

Technical Achievements:
  - 10+ API endpoints
  - 5+ Database tables
  - 6 technical decisions (TD-076 to TD-087)
  - >85% code coverage
  - Zero P0 bugs
```

**Phase 1C 完成慶祝活動**:
- 🎉 Team Celebration Meeting (Week 33 End)
- 📊 Phase 1C Demo to Stakeholders
- 🏆 Recognition of Key Contributors
- 📝 Phase 1C Retrospective & Lessons Learned
- 🚀 Phase 1D Kickoff Preview

---

### Phase 1C → Phase 1D 技術移交

**移交內容**:

1. **API 文檔**:
   - Swagger/OpenAPI 3.0 完整文檔
   - API 使用範例 (C#, TypeScript, cURL)
   - 錯誤代碼表
   - 性能基準數據

2. **Database Schema**:
   - Complete ER diagram
   - Table definitions
   - Indexes and constraints
   - Sample data

3. **技術決策記錄**:
   - TD-076 到 TD-087 (12 個技術決策)
   - ADR-013 到 ADR-015 (3 個架構決策)
   - Design rationale

4. **測試報告**:
   - Unit test report (coverage >85%)
   - Integration test report
   - Performance test report
   - API test collection (Postman)

5. **Known Issues & Tech Debt**:
   - Issue tracking report
   - Tech debt list (prioritized)
   - Improvement suggestions

**移交時程**:
- Week 33 Day 14: 移交文檔準備完成
- Week 33 Day 15: Phase 1C → Phase 1D Handoff Meeting
- Week 34 Day 1: Phase 1D Sprint 12 Kickoff

---

## Feature 1: Workflow Execution Trigger (3-4 SP)

### Feature Overview

**User Story**: US 7.2 - Workflow Execution Trigger
**Story Points**: 3-4 SP
**Priority**: P0 (Critical)
**Dependencies**: Workflow Definition CRUD (Sprint 10)

**Feature Description**:
IT 開發者需要多種方式觸發 Workflow 執行:
- **Manual Trigger**: 手動觸發執行
- **Schedule Trigger**: 定時觸發 (Cron expression)
- **Event Trigger**: 事件驅動觸發
- **Webhook Trigger**: 外部系統 Webhook 觸發

---

### Phase 1: Manual Trigger (1 SP)

**Goal**: 實現手動觸發 Workflow 執行

#### Task Breakdown

**T11.001 - T11.010: Manual Trigger API**

**Domain Model** (T11.001 - T11.003):
- [ ] T11.001: WorkflowExecution 實體設計
  - ExecutionId, WorkflowDefinitionId
  - TriggerType (Manual/Schedule/Event/Webhook)
  - TriggerMetadata (JSON)
  - ExecutionStatus (Queued/Running/Completed/Failed)
  - StartTime, EndTime, Duration

- [ ] T11.002: WorkflowExecutionRepository
  - GetByIdAsync()
  - GetByWorkflowDefinitionIdAsync()
  - GetRecentExecutionsAsync()
  - AddAsync()
  - UpdateAsync()

- [ ] T11.003: Domain Events
  - WorkflowExecutionTriggeredEvent
  - WorkflowExecutionStartedEvent
  - WorkflowExecutionCompletedEvent
  - WorkflowExecutionFailedEvent

**CQRS Commands/Queries** (T11.004 - T11.006):
- [ ] T11.004: TriggerManualWorkflowCommand
  - WorkflowDefinitionId
  - InputParameters (optional)
  - Handler: 創建 WorkflowExecution + 觸發執行

- [ ] T11.005: GetWorkflowExecutionByIdQuery
  - ExecutionId
  - Return: WorkflowExecutionDto (包含狀態和結果)

- [ ] T11.006: GetWorkflowExecutionsQuery
  - WorkflowDefinitionId (optional)
  - Status (optional)
  - PageNumber, PageSize
  - Return: PagedResult<WorkflowExecutionDto>

**API Endpoints** (T11.007 - T11.009):
- [ ] T11.007: POST /api/v1/workflow-executions/trigger
  - Request: TriggerManualWorkflowRequest
  - Response: 201 Created + WorkflowExecutionDto

- [ ] T11.008: GET /api/v1/workflow-executions/{id}
  - Response: 200 OK + WorkflowExecutionDto

- [ ] T11.009: GET /api/v1/workflow-executions
  - Query params: ?workflowDefinitionId=xxx&status=xxx
  - Response: 200 OK + PagedResult<WorkflowExecutionDto>

**Testing** (T11.010):
- [ ] T11.010: Integration Tests
  - Manual trigger 成功觸發
  - 查詢執行狀態
  - 查詢執行歷史
  - Error handling (workflow not found)

---

### Phase 2: Schedule Trigger (Cron) (1 SP)

**Goal**: 實現定時觸發 Workflow 執行 (Cron expression)

#### Task Breakdown

**T11.011 - T11.020: Schedule Trigger**

**Domain Model** (T11.011 - T11.013):
- [ ] T11.011: WorkflowSchedule 實體設計
  - ScheduleId, WorkflowDefinitionId
  - CronExpression (string)
  - IsActive (boolean)
  - NextExecutionTime (DateTime)
  - LastExecutionTime (DateTime)

- [ ] T11.012: WorkflowScheduleRepository
  - GetActiveSchedulesAsync()
  - GetByWorkflowDefinitionIdAsync()
  - AddAsync()
  - UpdateAsync()
  - DeleteAsync()

- [ ] T11.013: Cron Expression Validator
  - ValidateCronExpression(string expression)
  - CalculateNextExecutionTime(string expression)

**CQRS Commands** (T11.014 - T11.016):
- [ ] T11.014: CreateWorkflowScheduleCommand
  - WorkflowDefinitionId
  - CronExpression
  - Validator: 驗證 Cron expression 有效性

- [ ] T11.015: UpdateWorkflowScheduleCommand
  - ScheduleId
  - CronExpression
  - IsActive

- [ ] T11.016: DeleteWorkflowScheduleCommand
  - ScheduleId

**Background Service** (T11.017 - T11.018):
- [ ] T11.017: WorkflowSchedulerService (BackgroundService)
  - 每分鐘檢查 NextExecutionTime
  - 觸發到期的 Workflows
  - 更新 NextExecutionTime

- [ ] T11.018: Hangfire 整合 (alternative)
  - 使用 Hangfire 實現 Cron scheduling
  - 比較 BackgroundService vs Hangfire 優劣

**API Endpoints** (T11.019):
- [ ] T11.019: Schedule Management API
  - POST /api/v1/workflow-schedules
  - GET /api/v1/workflow-schedules/{id}
  - PUT /api/v1/workflow-schedules/{id}
  - DELETE /api/v1/workflow-schedules/{id}

**Testing** (T11.020):
- [ ] T11.020: Integration Tests
  - Create schedule with valid Cron
  - Cron expression validation (valid/invalid)
  - Schedule trigger at NextExecutionTime
  - Update schedule (activate/deactivate)

---

### Phase 3: Event Trigger (1 SP)

**Goal**: 實現事件驅動觸發 Workflow 執行

#### Task Breakdown

**T11.021 - T11.030: Event Trigger**

**Domain Model** (T11.021 - T11.023):
- [ ] T11.021: WorkflowEventSubscription 實體設計
  - SubscriptionId, WorkflowDefinitionId
  - EventType (string)
  - EventFilter (JSON, optional)
  - IsActive (boolean)

- [ ] T11.022: WorkflowEventSubscriptionRepository
  - GetByEventTypeAsync(eventType)
  - GetByWorkflowDefinitionIdAsync()
  - AddAsync()
  - UpdateAsync()
  - DeleteAsync()

- [ ] T11.023: EventMatchingService
  - MatchEventToSubscriptions(eventType, eventData)
  - ApplyEventFilter(subscription, eventData)

**Event Bus Integration** (T11.024 - T11.026):
- [ ] T11.024: IEventBus 介面設計
  - PublishAsync(eventType, eventData)
  - SubscribeAsync(eventType, handler)

- [ ] T11.025: EventBus 實作 (Memory-based)
  - In-memory event bus for MVP
  - 事件分發邏輯

- [ ] T11.026: WorkflowEventHandler
  - 接收事件
  - 匹配訂閱
  - 觸發 Workflow

**CQRS Commands** (T11.027 - T11.028):
- [ ] T11.027: CreateWorkflowEventSubscriptionCommand
  - WorkflowDefinitionId
  - EventType
  - EventFilter (optional)

- [ ] T11.028: DeleteWorkflowEventSubscriptionCommand
  - SubscriptionId

**API Endpoints** (T11.029):
- [ ] T11.029: Event Subscription API
  - POST /api/v1/workflow-event-subscriptions
  - GET /api/v1/workflow-event-subscriptions
  - DELETE /api/v1/workflow-event-subscriptions/{id}

**Testing** (T11.030):
- [ ] T11.030: Integration Tests
  - Create event subscription
  - Publish event → Workflow triggered
  - Event filter matching
  - Multiple workflows subscribe to same event

---

### Phase 4: Webhook Trigger (0.5-1 SP)

**Goal**: 實現 Webhook 觸發 Workflow 執行

#### Task Breakdown

**T11.031 - T11.040: Webhook Trigger**

**Domain Model** (T11.031 - T11.033):
- [ ] T11.031: WorkflowWebhook 實體設計
  - WebhookId, WorkflowDefinitionId
  - WebhookUrl (唯一, 自動生成)
  - SecretKey (用於驗證)
  - IsActive (boolean)

- [ ] T11.032: WorkflowWebhookRepository
  - GetByWebhookUrlAsync(webhookUrl)
  - GetByWorkflowDefinitionIdAsync()
  - AddAsync()
  - DeleteAsync()

- [ ] T11.033: WebhookSecurityService
  - GenerateWebhookUrl()
  - GenerateSecretKey()
  - ValidateWebhookSignature(request, secretKey)

**Webhook Endpoint** (T11.034 - T11.036):
- [ ] T11.034: POST /webhooks/{webhookUrl}
  - 接收外部系統 POST 請求
  - 驗證 Signature (HMAC-SHA256)
  - 觸發對應 Workflow

- [ ] T11.035: Webhook Request Validation
  - 驗證 Signature header
  - 驗證 Webhook 存在且 Active
  - 解析 Request body

- [ ] T11.036: Webhook Response
  - 202 Accepted (異步執行)
  - 返回 ExecutionId

**CQRS Commands** (T11.037 - T11.038):
- [ ] T11.037: CreateWorkflowWebhookCommand
  - WorkflowDefinitionId
  - 自動生成 WebhookUrl 和 SecretKey

- [ ] T11.038: DeleteWorkflowWebhookCommand
  - WebhookId

**API Endpoints** (T11.039):
- [ ] T11.039: Webhook Management API
  - POST /api/v1/workflow-webhooks
  - GET /api/v1/workflow-webhooks
  - DELETE /api/v1/workflow-webhooks/{id}

**Testing** (T11.040):
- [ ] T11.040: Integration Tests
  - Create webhook → receive webhookUrl + secretKey
  - POST to webhook → Workflow triggered
  - Invalid signature → 401 Unauthorized
  - Inactive webhook → 404 Not Found

---

### Feature 1 驗收標準

**Execution Trigger 完整性**:
- ✅ 4 種 Trigger types 全部實現
- ✅ Manual Trigger API 正常運作
- ✅ Schedule Trigger (Cron) 定時執行
- ✅ Event Trigger 事件驅動執行
- ✅ Webhook Trigger 外部觸發執行

**API 設計質量**:
- ✅ RESTful API 設計規範
- ✅ Swagger 文檔完整
- ✅ 錯誤處理標準化 (RFC 7807)

**測試覆蓋**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋所有 Trigger types
- ✅ End-to-end 測試通過

**性能目標**:
- ✅ Trigger API 響應時間 P95 <200ms
- ✅ Schedule 檢查延遲 <5s
- ✅ Event 觸發延遲 <1s
- ✅ Webhook 觸發延遲 <500ms

---

## Feature 2: Workflow Template Management (3-4 SP)

### Feature Overview

**User Story**: US 7.3 - Workflow Template Management
**Story Points**: 3-4 SP
**Priority**: P0 (Critical)
**Dependencies**: Workflow Definition CRUD (Sprint 10)

**Feature Description**:
IT 開發者需要管理 Workflow Templates:
- **Template CRUD**: 創建、查詢、更新、刪除 Template
- **Template Categories**: 分類管理 (Integration, Data Processing, etc.)
- **Import/Export**: JSON 格式 Import/Export
- **Marketplace 基礎**: 公開 Template 市場基礎功能

---

### Phase 1: Template CRUD API (1.5 SP)

**Goal**: 實現 Workflow Template CRUD API

#### Task Breakdown

**T11.041 - T11.050: Template CRUD**

**Domain Model** (T11.041 - T11.043):
- [ ] T11.041: WorkflowTemplate 實體設計
  - TemplateId, Name, Description
  - CategoryId (FK to WorkflowTemplateCategory)
  - DefinitionJson (JSONB)
  - IsPublic (boolean)
  - CreatedBy, CreatedAt, UpdatedAt

- [ ] T11.042: WorkflowTemplateCategory 實體設計
  - CategoryId, Name, Description
  - ParentCategoryId (self-reference, for hierarchy)
  - IconUrl

- [ ] T11.043: WorkflowTemplateRepository
  - GetByIdAsync()
  - GetByCategoryIdAsync()
  - GetPublicTemplatesAsync()
  - AddAsync()
  - UpdateAsync()
  - DeleteAsync()

**CQRS Commands/Queries** (T11.044 - T11.046):
- [ ] T11.044: CreateWorkflowTemplateCommand
  - Name, Description
  - CategoryId
  - DefinitionJson
  - IsPublic

- [ ] T11.045: UpdateWorkflowTemplateCommand
  - TemplateId
  - Name, Description, CategoryId, IsPublic

- [ ] T11.046: GetWorkflowTemplatesQuery
  - CategoryId (optional)
  - IsPublic (optional)
  - SearchTerm (optional)
  - PageNumber, PageSize

**API Endpoints** (T11.047 - T11.049):
- [ ] T11.047: Template CRUD API
  - POST /api/v1/workflow-templates
  - GET /api/v1/workflow-templates/{id}
  - GET /api/v1/workflow-templates
  - PUT /api/v1/workflow-templates/{id}
  - DELETE /api/v1/workflow-templates/{id}

- [ ] T11.048: Category CRUD API
  - POST /api/v1/workflow-template-categories
  - GET /api/v1/workflow-template-categories
  - PUT /api/v1/workflow-template-categories/{id}
  - DELETE /api/v1/workflow-template-categories/{id}

- [ ] T11.049: Template Discovery API
  - GET /api/v1/workflow-templates/public
  - GET /api/v1/workflow-templates/categories/{categoryId}

**Testing** (T11.050):
- [ ] T11.050: Integration Tests
  - Create template
  - Query templates (by category, public)
  - Update template
  - Delete template

---

### Phase 2: Template Categories (0.5 SP)

**Goal**: 實現 Template 分類管理

#### Task Breakdown

**T11.051 - T11.055: Template Categories**

**Seed Data** (T11.051):
- [ ] T11.051: 創建內建 Template Categories
  - Integration (整合類)
  - Data Processing (數據處理類)
  - Automation (自動化類)
  - Notification (通知類)
  - Custom (自定義類)

**Category Hierarchy** (T11.052 - T11.053):
- [ ] T11.052: Category Hierarchy 支持
  - ParentCategoryId self-reference
  - GetCategoryTreeAsync() 遞迴查詢
  - GetCategoryPathAsync() 面包屑導航

- [ ] T11.053: Category Validation
  - 避免循環引用 (Parent → Child → Parent)
  - 限制層級深度 (最多 3 層)

**API Enhancements** (T11.054):
- [ ] T11.054: Category Tree API
  - GET /api/v1/workflow-template-categories/tree
  - 返回完整分類樹狀結構

**Testing** (T11.055):
- [ ] T11.055: Integration Tests
  - Get category tree
  - Create sub-category
  - Circular reference prevention
  - Category path generation

---

### Phase 3: Import/Export (1 SP)

**Goal**: 實現 Workflow Template Import/Export

#### Task Breakdown

**T11.056 - T11.065: Import/Export**

**Export Format** (T11.056 - T11.058):
- [ ] T11.056: 定義 Template Export JSON Schema
  ```json
  {
    "version": "1.0",
    "templateMetadata": {
      "name": "...",
      "description": "...",
      "category": "...",
      "author": "..."
    },
    "workflowDefinition": { ... },
    "nodes": [...],
    "edges": [...]
  }
  ```

- [ ] T11.057: ExportTemplateService
  - ExportToJsonAsync(templateId)
  - 包含 metadata + workflow definition

- [ ] T11.058: Export Validation
  - 驗證 Template 完整性
  - 移除敏感資訊 (credentials)

**Import Logic** (T11.059 - T11.061):
- [ ] T11.059: ImportTemplateService
  - ImportFromJsonAsync(jsonContent)
  - 驗證 JSON schema
  - 創建 WorkflowTemplate

- [ ] T11.060: Import Validation
  - JSON schema 驗證
  - Node types 存在性驗證
  - Circular dependency 檢查

- [ ] T11.061: Import Conflict Resolution
  - Template name 衝突處理
  - Category mapping (如 category 不存在)

**API Endpoints** (T11.062 - T11.063):
- [ ] T11.062: Export API
  - GET /api/v1/workflow-templates/{id}/export
  - Response: JSON file download

- [ ] T11.063: Import API
  - POST /api/v1/workflow-templates/import
  - Request: multipart/form-data (JSON file)
  - Response: 201 Created + TemplateId

**Testing** (T11.064 - T11.065):
- [ ] T11.064: Export Tests
  - Export valid template
  - Export format validation
  - Sensitive data removal

- [ ] T11.065: Import Tests
  - Import valid JSON
  - Import validation (invalid JSON)
  - Conflict resolution
  - Round-trip test (Export → Import → Verify)

---

### Phase 4: Marketplace 基礎 (1 SP)

**Goal**: 實現公開 Template Marketplace 基礎功能

#### Task Breakdown

**T11.066 - T11.075: Marketplace**

**Template Rating** (T11.066 - T11.068):
- [ ] T11.066: TemplateRating 實體設計
  - RatingId, TemplateId, UserId
  - Rating (1-5 stars)
  - Review (text, optional)
  - CreatedAt

- [ ] T11.067: TemplateRatingRepository
  - GetByTemplateIdAsync()
  - AddAsync()
  - UpdateAsync()
  - CalculateAverageRating(templateId)

- [ ] T11.068: Rating API
  - POST /api/v1/workflow-templates/{id}/ratings
  - GET /api/v1/workflow-templates/{id}/ratings
  - PUT /api/v1/workflow-templates/ratings/{ratingId}

**Template Usage Statistics** (T11.069 - T11.070):
- [ ] T11.069: TemplateUsage 實體設計
  - UsageId, TemplateId, UserId
  - UsedAt (DateTime)
  - 追蹤 Template 使用次數

- [ ] T11.070: Usage Tracking
  - TrackTemplateUsageAsync(templateId)
  - GetTemplateUsageCountAsync(templateId)

**Marketplace Discovery** (T11.071 - T11.073):
- [ ] T11.071: Marketplace Search API
  - GET /api/v1/marketplace/templates
  - Query params: ?category=xxx&search=xxx&sort=rating|usage
  - 返回 public templates 排序列表

- [ ] T11.072: Popular Templates API
  - GET /api/v1/marketplace/templates/popular
  - 返回最受歡迎的 Templates (按 usage count)

- [ ] T11.073: Top Rated Templates API
  - GET /api/v1/marketplace/templates/top-rated
  - 返回評分最高的 Templates (按 average rating)

**Testing** (T11.074 - T11.075):
- [ ] T11.074: Rating Tests
  - Add rating
  - Calculate average rating
  - Update rating

- [ ] T11.075: Marketplace Tests
  - Search templates
  - Get popular templates
  - Get top-rated templates
  - Sort by rating/usage

---

### Feature 2 驗收標準

**Template Management 完整性**:
- ✅ Template CRUD API 正常運作
- ✅ Template Categories 分類管理
- ✅ Import/Export 功能正常
- ✅ Marketplace 基礎功能可用

**API 設計質量**:
- ✅ RESTful API 設計規範
- ✅ Swagger 文檔完整
- ✅ JSON Schema 驗證

**測試覆蓋**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋所有功能
- ✅ Round-trip 測試通過 (Export → Import)

**性能目標**:
- ✅ Template CRUD API 響應時間 P95 <200ms
- ✅ Import/Export 延遲 <2s
- ✅ Marketplace Search 響應時間 <300ms

---

## Feature 3: Version Control基礎 (1-4 SP)

### Feature Overview

**User Story**: US 7.4 - Version Control基礎
**Story Points**: 1-4 SP
**Priority**: P1 (High)
**Dependencies**: Workflow Definition CRUD (Sprint 10)

**Feature Description**:
IT 開發者需要 Workflow 版本控制:
- **Workflow Versioning**: 自動版本號管理
- **Version Comparison**: 比較不同版本差異
- **Version Rollback**: 回滾到歷史版本
- **Version History**: 查看版本變更歷史

---

### Phase 1: Workflow Versioning (1 SP)

**Goal**: 實現 Workflow 自動版本號管理

#### Task Breakdown

**T11.076 - T11.085: Versioning**

**Domain Model 調整** (T11.076 - T11.078):
- [ ] T11.076: WorkflowDefinition 版本化調整
  - Version (integer) → 自動遞增
  - IsActive (boolean) → 標記當前活躍版本
  - BaseVersionId (Guid?, FK to self) → 指向原始版本
  - VersionComment (string) → 版本變更說明

- [ ] T11.077: WorkflowVersion 實體設計 (alternative)
  - VersionId, WorkflowDefinitionId, VersionNumber
  - DefinitionJson (snapshot)
  - CreatedBy, CreatedAt
  - VersionComment

- [ ] T11.078: Versioning Strategy 決策
  - **Option 1**: WorkflowDefinition 每次更新創建新 row (Version++)
  - **Option 2**: WorkflowVersion 獨立表存儲版本歷史
  - **Decision**: 選擇 Option 1 (簡化實現)

**CQRS Commands** (T11.079 - T11.081):
- [ ] T11.079: UpdateWorkflowDefinitionCommand 調整
  - 創建新版本 (Version++)
  - 舊版本設為 IsActive = false
  - 新版本設為 IsActive = true

- [ ] T11.080: PublishWorkflowVersionCommand
  - 發佈 Draft → Active
  - 創建新版本

- [ ] T11.081: GetWorkflowVersionsQuery
  - WorkflowDefinitionId (base)
  - 返回所有版本 (包含非 Active)

**Repository 調整** (T11.082 - T11.083):
- [ ] T11.082: GetActiveVersionAsync(workflowDefinitionId)
  - 返回 IsActive = true 的版本

- [ ] T11.083: GetAllVersionsAsync(workflowDefinitionId)
  - 返回所有版本 (按 Version DESC)

**API Endpoints** (T11.084):
- [ ] T11.084: Version Management API
  - GET /api/v1/workflow-definitions/{id}/versions
  - GET /api/v1/workflow-definitions/{id}/versions/{version}
  - POST /api/v1/workflow-definitions/{id}/publish

**Testing** (T11.085):
- [ ] T11.085: Integration Tests
  - Create workflow (Version = 1)
  - Update workflow → Version = 2
  - Get all versions
  - Get active version

---

### Phase 2: Version Comparison (1 SP)

**Goal**: 實現版本比較功能

#### Task Breakdown

**T11.086 - T11.095: Version Comparison**

**Comparison Service** (T11.086 - T11.088):
- [ ] T11.086: WorkflowVersionComparisonService
  - CompareVersionsAsync(versionId1, versionId2)
  - 返回差異報告

- [ ] T11.087: JSON Diff Algorithm
  - 比較兩個 DefinitionJson
  - 返回 Added/Removed/Modified nodes/edges

- [ ] T11.088: Diff Result Model
  ```csharp
  public class WorkflowVersionDiff
  {
      public List<NodeDiff> AddedNodes { get; set; }
      public List<NodeDiff> RemovedNodes { get; set; }
      public List<NodeDiff> ModifiedNodes { get; set; }
      public List<EdgeDiff> AddedEdges { get; set; }
      public List<EdgeDiff> RemovedEdges { get; set; }
  }
  ```

**CQRS Queries** (T11.089):
- [ ] T11.089: CompareWorkflowVersionsQuery
  - WorkflowDefinitionId
  - VersionNumber1, VersionNumber2
  - 返回 WorkflowVersionDiff

**API Endpoints** (T11.090):
- [ ] T11.090: Version Comparison API
  - GET /api/v1/workflow-definitions/{id}/versions/compare?v1=1&v2=2
  - 返回 WorkflowVersionDiff

**UI-Friendly Diff Format** (T11.091 - T11.092):
- [ ] T11.091: 生成 Diff Summary
  - "Added 2 nodes, removed 1 node, modified 3 edges"

- [ ] T11.092: Diff Visualization Data
  - 為 Frontend 提供可視化 Diff 數據
  - 標記 Added (green), Removed (red), Modified (yellow)

**Testing** (T11.093 - T11.095):
- [ ] T11.093: Diff Algorithm Tests
  - Compare identical versions → no diff
  - Compare different versions → correct diff
  - Edge cases (empty workflow)

- [ ] T11.094: Integration Tests
  - Compare versions API
  - Diff format validation

- [ ] T11.095: Performance Tests
  - Large workflow comparison (<1s)

---

### Phase 3: Version Rollback (1 SP)

**Goal**: 實現版本回滾功能

#### Task Breakdown

**T11.096 - T11.105: Version Rollback**

**Rollback Logic** (T11.096 - T11.098):
- [ ] T11.096: RollbackWorkflowVersionCommand
  - WorkflowDefinitionId
  - TargetVersionNumber
  - 創建新版本 (Version++), DefinitionJson 從 targetVersion 複製

- [ ] T11.097: Rollback Validation
  - 驗證 targetVersion 存在
  - 驗證 targetVersion 不是當前 Active 版本
  - 驗證 targetVersion 沒有循環依賴

- [ ] T11.098: Rollback History Tracking
  - 記錄 Rollback 操作 (from Version X to Version Y)
  - WorkflowVersionChange 實體 (optional)

**CQRS Commands** (T11.099):
- [ ] T11.099: RollbackWorkflowVersionCommandHandler
  - 獲取 targetVersion DefinitionJson
  - 創建新版本 (Version++)
  - 設為 Active
  - 觸發 WorkflowVersionRolledBackEvent

**API Endpoints** (T11.100):
- [ ] T11.100: Rollback API
  - POST /api/v1/workflow-definitions/{id}/rollback
  - Request: { "targetVersionNumber": 2 }
  - Response: 200 OK + new WorkflowDefinitionDto

**Rollback Confirmation** (T11.101 - T11.102):
- [ ] T11.101: Rollback Preview
  - GET /api/v1/workflow-definitions/{id}/rollback/preview?version=2
  - 返回 Rollback 會造成的變更 (Diff)

- [ ] T11.102: Rollback Confirmation Required
  - 需要 confirmRollback=true 參數才執行

**Testing** (T11.103 - T11.105):
- [ ] T11.103: Rollback Tests
  - Rollback to previous version
  - Rollback creates new version (Version++)
  - Rollback validation (invalid version)

- [ ] T11.104: Integration Tests
  - Rollback API
  - Rollback preview
  - Rollback confirmation

- [ ] T11.105: End-to-end Rollback Test
  - Create v1 → Update to v2 → Rollback to v1 → Verify v3 = v1

---

### Phase 4: Version History (0.5 SP)

**Goal**: 實現版本變更歷史查詢

#### Task Breakdown

**T11.106 - T11.110: Version History**

**History Query** (T11.106 - T11.107):
- [ ] T11.106: GetWorkflowVersionHistoryQuery
  - WorkflowDefinitionId
  - 返回完整版本歷史 (Version, CreatedAt, CreatedBy, VersionComment)

- [ ] T11.107: Version Timeline
  - 按時間順序返回版本變更
  - 包含 Rollback 操作記錄

**API Endpoints** (T11.108):
- [ ] T11.108: Version History API
  - GET /api/v1/workflow-definitions/{id}/versions/history
  - 返回完整版本 Timeline

**Version Metadata** (T11.109):
- [ ] T11.109: 增強版本 Metadata
  - CreatedBy (userId)
  - VersionComment (變更說明)
  - ChangeType (Created/Updated/Rollback)

**Testing** (T11.110):
- [ ] T11.110: Integration Tests
  - Get version history
  - Version timeline 排序正確
  - Rollback 操作記錄在 history 中

---

### Feature 3 驗收標準

**Version Control 完整性**:
- ✅ Workflow Versioning 自動管理
- ✅ Version Comparison 正常運作
- ✅ Version Rollback 功能可用
- ✅ Version History 查詢正常

**API 設計質量**:
- ✅ RESTful API 設計規範
- ✅ Swagger 文檔完整
- ✅ Versioning 邏輯清晰

**測試覆蓋**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋所有功能
- ✅ End-to-end Rollback 測試通過

**性能目標**:
- ✅ Version Query 響應時間 P95 <200ms
- ✅ Version Comparison 延遲 <1s
- ✅ Rollback 操作延遲 <2s

---

## Phase 1C 完成與驗收

### Phase 1C 最終驗收 Checklist

**Sprint 10 交付物驗收**:
- [ ] ✅ Workflow Definition CRUD (5 API endpoints)
- [ ] ✅ Node Registry System (5 Node types, Registry Service)
- [ ] ✅ Edge Validation Engine (Circular dependency detection)
- [ ] ✅ Database Schema (3 tables + indexes)
- [ ] ✅ API 文檔 (Swagger) 完整
- [ ] ✅ 整合測試通過
- [ ] ✅ 代碼覆蓋率 >85%

**Sprint 11 交付物驗收**:
- [ ] ✅ Workflow Execution Trigger (4 Trigger types)
- [ ] ✅ Workflow Template Management (Template CRUD + Marketplace)
- [ ] ✅ Version Control基礎 (Versioning + Rollback)
- [ ] ✅ Database Schema (4+ tables)
- [ ] ✅ API 文檔更新
- [ ] ✅ 整合測試通過
- [ ] ✅ 代碼覆蓋率 >85%

**Phase 1C 整體驗收**:
- [ ] ✅ 所有 API endpoints 正常運作 (10+ endpoints)
- [ ] ✅ 零 P0/P1 Bugs
- [ ] ✅ 性能目標達成 (API P95 <200ms)
- [ ] ✅ 安全標準符合 (Authentication + Authorization)
- [ ] ✅ 文檔完整 (API + Tech Docs + Handoff)
- [ ] ✅ Phase 1D 準備度 ≥90%

---

### Phase 1C 完成慶祝與回顧

**完成慶祝活動** (Week 33 End):
- 🎉 Team Celebration Meeting
- 📊 Phase 1C Demo to Stakeholders
- 🏆 Key Contributors Recognition
- 📝 Phase 1C Retrospective
- 🚀 Phase 1D Kickoff Preview

**Phase 1C 成果總結**:
```yaml
Duration: 6 weeks (Sprint 10-11)
Story Points: 17.5 SP (8 + 9.5)
Features Delivered: 6 major features
API Endpoints: 10+ endpoints
Database Tables: 7+ tables
Code Coverage: >85%
Performance: API P95 <200ms
Quality: Zero P0 bugs

Team Achievement:
  - On-time delivery: 100%
  - Quality standards: 100% met
  - Team satisfaction: High
  - Stakeholder satisfaction: High
```

**Phase 1C Retrospective 重點**:
- 📚 What Went Well (技術成就, 團隊協作)
- 📚 What Could Be Improved (改進建議)
- 📚 Lessons Learned (經驗教訓)
- 🎯 Action Items for Phase 1D

---

## Phase 1D 移交準備

### Phase 1D Overview

**Phase 1D Scope**: Sprint 12-14 (9 weeks)
**Goal**: Workflow Editor Frontend (VueFlow + Module Federation)

**Phase 1D 關鍵交付物**:
1. VueFlow Workflow Editor (視覺化編輯器)
2. Node Palette (Node types 拖拽)
3. Real-time Collaboration (CRDT)
4. Workflow Execution Monitoring
5. Template Marketplace UI

---

### Phase 1D 準備度評估

**技術準備度 Checklist**:
- [ ] ✅ Backend API 100% 完成
- [ ] ✅ API 文檔完整 (Swagger + Examples)
- [ ] ✅ Database Schema 穩定
- [ ] ✅ Performance 基準達成
- [ ] ✅ Security 標準符合

**Frontend 技術準備**:
- [ ] ✅ VueFlow PoC 完成 (Sprint 11 期間)
- [ ] ✅ Module Federation 配置完成
- [ ] ✅ API Client 生成 (TypeScript)
- [ ] ✅ Design System 準備 (UI components)
- [ ] ✅ Real-time Collaboration 架構設計

**團隊準備度**:
- [ ] ✅ Frontend 團隊 Vue 3 培訓完成
- [ ] ✅ VueFlow 技術研究完成
- [ ] ✅ CRDT 技術研究完成
- [ ] ✅ Backend API 理解 Workshop 完成

---

### Phase 1C → Phase 1D Handoff Meeting

**Meeting Agenda** (Week 33 Day 15):
1. **Phase 1C 成果展示** (30 min)
   - Backend API Demo
   - Performance 測試結果
   - Quality 指標達成

2. **技術移交** (60 min)
   - API 文檔講解
   - Database Schema 講解
   - 技術決策背景說明
   - Known Issues & Tech Debt

3. **Phase 1D Kickoff** (30 min)
   - Sprint 12-14 規劃
   - Frontend 架構設計
   - 團隊分工
   - Q&A

**Handoff Checklist**:
- [ ] ✅ API 文檔移交 (Swagger + Examples)
- [ ] ✅ Database Schema 移交 (ER Diagram)
- [ ] ✅ Tech Decisions 移交 (TD-076 to TD-087)
- [ ] ✅ Test Reports 移交 (Coverage + Performance)
- [ ] ✅ Known Issues 移交 (Issue Tracking + Tech Debt)

**Post-Handoff Support**:
- Backend 團隊提供 2 週技術支持
- Daily standup 聯合會議
- API 問題快速響應 (<4 hours)

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**Sprint Story Points**: 9.5 SP
**Phase 1C 完成**: 100% (Sprint 11 結束時)
**Phase 1D 啟動**: Week 34 (Sprint 12 Kickoff)
