# SPRINT-4-4-CHECKLIST.md - Sprint 4 任務清單：Persona Framework 與 Plugin 熱重載執行追蹤

**版本**: v2.1
**Sprint 編號**: Sprint 4
**Sprint 週期**: Week 10-12 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2025-12-16 ~ 2026-01-05
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-13
**最後更新**: 2025-11-13

---

## 📑 目錄 (Table of Contents)

### 核心章節
1. [總體進度統計](#總體進度統計)
2. [項目準備](#項目準備)
3. [US 7.1 - Persona Template Configuration (5 SP, 5 Phases)](#us-71---persona-template-configuration-5-sp-5-phases-)
   - 3.1 [Phase 1: Domain Entities](#phase-1-domain-entities-1-sp-)
   - 3.2 [Phase 2: Repository & Validation](#phase-2-repository--validation-1-sp-)
   - 3.3 [Phase 3: Application Layer (CQRS)](#phase-3-application-layer-cqrs-1-sp-)
   - 3.4 [Phase 4: API Layer](#phase-4-api-layer-1-sp-)
   - 3.5 [Phase 5: 10 種預設模板](#phase-5-10-種預設-persona-模板-1-sp-)
4. [US 7.2 - Persona-Driven Prompt Engineering (5 SP, 4 Phases)](#us-72---persona-driven-prompt-engineering-5-sp-4-phases-)
   - 4.1 [Phase 1: Liquid Template Engine](#phase-1-liquid-template-engine-integration-15-sp-)
   - 4.2 [Phase 2: Prompt Generation Service](#phase-2-prompt-generation-service-15-sp-)
   - 4.3 [Phase 3: Token Management & Caching](#phase-3-token-management--caching-1-sp-)
   - 4.4 [Phase 4: API Integration & Testing](#phase-4-api-integration--testing-1-sp-)
5. [US 2.2 - Plugin Hot Reload (3 SP, 4 Phases)](#us-22---plugin-hot-reload-3-sp-4-phases-)
   - 5.1 [Phase 1: AssemblyLoadContext Design](#phase-1-assemblyloadcontext-architecture-design-1-sp-)
   - 5.2 [Phase 2: Hot Reload Implementation](#phase-2-hot-reload-core-implementation-1-sp-)
   - 5.3 [Phase 3: API & Frontend](#phase-3-api--frontend-integration-05-sp-)
   - 5.4 [Phase 4: Testing & Validation](#phase-4-testing--validation-05-sp-)
6. [測試](#測試)
7. [文檔](#文檔)
8. [部署與DevOps](#部署與devops)
9. [Definition of Done 驗證](#definition-of-done-驗證)

### 輔助章節
- [使用指南](#使用指南)
- [參考文獻索引](#參考文獻索引)
- [版本歷史](#版本歷史)

---

## 📖 使用指南

### 文件目的
本文件為 Sprint 4 的詳細任務檢查清單，按 **User Story (US)** 組織，追蹤所有待辦事項的執行狀態和完成進度。

### 目標讀者
- **開發團隊**：日常開發任務的核心參考文件，追蹤工作進度
- **Scrum Master / PM**：Sprint 進度追蹤與風險識別
- **QA 團隊**：測試範圍與驗收標準的參考
- **AI Assistant**：任務狀態查詢與進度更新

### 使用方式
1. **每日開發**：查看對應 User Story 的 Phase 任務清單，更新完成狀態
2. **進度追蹤**：查看總體進度統計表，了解 Sprint 整體進度
3. **任務分配**：依據 Phase 劃分合理分配開發任務
4. **完成驗證**：使用 Definition of Done 驗證章節確保質量標準
5. **更新文件**：完成任務後立即更新檢查框狀態 [x]

### 快速導航
- **查看 Sprint 4 整體概覽** → [SPRINT-4-1-OVERVIEW.md](./SPRINT-4-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md)
- **查看技術上下文** → [SPRINT-4-3-CONTEXT.md](./SPRINT-4-3-CONTEXT.md)
- **查看開發日誌** → [SPRINT-4-5-DEV-LOG.md](./SPRINT-4-5-DEV-LOG.md)
- **查看問題追蹤** → [SPRINT-4-6-ISSUES.md](./SPRINT-4-6-ISSUES.md)

### 狀態標記說明
- ✅ **已完成** - 項目已完成並通過驗證
- 🔄 **進行中** - 項目正在執行中
- ⏳ **待開始** - 項目已計劃但尚未開始
- ⚠️ **有問題** - 項目遇到阻礙需要關注
- ❌ **失敗** - 項目未通過驗證需要重做

### 優先級標記
- **P0** - 必須完成的項目，影響 Sprint 交付
- **P1** - 重要但非阻塞的項目，建議完成

---

## 📊 總體進度統計

| 類別 | 已完成 / 總數 | 進度 | 狀態 |
|------|------------|------|------|
| 項目準備 | 0/7 | 0% | ⏳ |
| US 7.1 - Persona Template Configuration | 0/58 | 0% | ⏳ |
| US 7.2 - Persona-Driven Prompt Engineering | 0/52 | 0% | ⏳ |
| US 2.2 - Plugin Hot Reload | 0/43 | 0% | ⏳ |
| 測試 | 0/25 | 0% | ⏳ |
| 文檔 | 0/12 | 0% | ⏳ |
| 部署 | 0/10 | 0% | ⏳ |
| **總計** | **0/207** | **0%** | ⏳ |

**圖例**: ✅ 已完成 | 🔄 進行中 | ⏳ 待開始 | ❌ 阻塞

**Story Points 分配**:
- US 7.1: Persona Template Configuration (5 SP)
- US 7.2: Persona-Driven Prompt Engineering (5 SP)
- US 2.2: Plugin Hot Reload (3 SP)
- **總計**: 13 SP

---

## 項目準備

### 環境設置 (P0)
- [ ] 更新開發環境 (`git pull origin main`)
- [ ] 創建 Feature Branch (`git checkout -b feature/us-7.1-persona-template`)
- [ ] 安裝依賴 (`dotnet restore`, `pnpm install`)
- [ ] 驗證資料庫連接正常
- [ ] 檢查 Sprint 4 所有文檔

### Sprint Planning (P0)
- [ ] 閱讀 [US 7.1 規格](../../../docs/user-stories/modules/module-07-persona-framework.md) 與 MVP 範圍
- [ ] 閱讀 [US 7.2 規格](../../../docs/user-stories/modules/module-07-persona-framework.md) 與 MVP 範圍
- [ ] 閱讀 [US 2.2 規格](../../../docs/user-stories/modules/module-02-plugin-system.md) 與 MVP 範圍
- [ ] 確認 Phase 實施順序
- [ ] 規劃 Database Schema
- [ ] 規劃 API 端點
- [ ] 規劃測試策略

**驗收標準**:
- ✅ 開發環境就緒
- ✅ Feature Branch 創建成功
- ✅ 所有 Sprint 4 規格文檔已閱讀

---

## US 7.1: Persona Template Configuration (5 SP, 5 Phases)

### Phase 1: Domain Entities (1 SP) ⏳ 待開始

#### Domain Layer - Entities
- [ ] **創建 Persona Entity**: `src/AIAgentPlatform.Domain/Entities/Persona.cs`
  - Properties: Id, Name, Role, Description, Config, ValidationStatus, CreatedAt, UpdatedAt
  - Business Rules: Name 唯一性, Config 必須有效
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 150-200

- [ ] **創建 PersonaTemplate Entity**: `PersonaTemplate.cs`
  - Properties: Id, Name, Role, Description, DefaultConfig, IsSystemTemplate, CreatedAt
  - Business Rules: System Template 不可刪除
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 150-200

- [ ] **創建 PromptTemplate Entity**: `PromptTemplate.cs`
  - Properties: Id, PersonaId, Name, Content, Variables, Priority, CreatedAt
  - Business Rules: Liquid 語法驗證
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 450-500

#### Domain Layer - Value Objects
- [ ] **創建 PersonaConfig VO**: `src/AIAgentPlatform.Domain/ValueObjects/PersonaConfig.cs`
  - Properties: CommunicationStyle, ResponsePattern, SafetyGuardrails, ExpertiseAreas
  - Validation: JSON Schema 驗證
  - Immutability: VO 不可變
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 200-250

- [ ] **創建 CommunicationStyle VO**: `CommunicationStyle.cs`
  - Properties: Tone, Formality, Verbosity, EmojiUsage
  - Validation: Enum 驗證

- [ ] **創建 ResponsePattern VO**: `ResponsePattern.cs`
  - Properties: StructurePreference, ExplanationDepth, ExampleFrequency
  - Validation: 值範圍驗證

- [ ] **創建 SafetyGuardrails VO**: `SafetyGuardrails.cs`
  - Properties: ProhibitedTopics, MandatoryWarnings, ContentFilters
  - Validation: 必填欄位檢查

#### Domain Layer - Enums
- [ ] **創建 ValidationStatus Enum**: `ValidationStatus.cs`
  - Values: Valid, InvalidConfig, MissingFields, ConflictingRules
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 150-200

- [ ] **創建 ToneType Enum**: `ToneType.cs`
  - Values: Professional, Friendly, Formal, Casual, Empathetic, Assertive

- [ ] **創建 FormalityLevel Enum**: `FormalityLevel.cs`
  - Values: VeryFormal, Formal, Neutral, Casual, VeryCasual

#### Unit Tests (P0)
- [ ] **Persona Entity 單元測試**: `tests/AIAgentPlatform.UnitTests/Domain/Entities/PersonaTests.cs`
  - `Create_ValidPersona_ReturnsSuccess`
  - `Create_InvalidConfig_ReturnsFailure`
  - `ValidateConfig_ValidConfig_ReturnsValid`
  - `ValidateConfig_InvalidConfig_ReturnsInvalid`
  - 測試覆蓋率: ≥85%

- [ ] **PersonaConfig VO 單元測試**: `PersonaConfigTests.cs`
  - `Constructor_ValidData_CreatesVO`
  - `Constructor_InvalidData_ThrowsException`
  - `Equals_SameData_ReturnsTrue`
  - `Equals_DifferentData_ReturnsFalse`
  - 測試覆蓋率: ≥85%

- [ ] **Value Objects Immutability 測試**:
  - 驗證所有 VO 不可變
  - 驗證 Equality 正確實現

**驗收標準 (Phase 1)**:
- ✅ Domain Entities 定義完整，包含所有必要屬性
- ✅ Value Objects 不可變，具備驗證邏輯
- ✅ Enums 完整定義
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 2: Repository & Validation (1 SP) ⏳ 待開始

#### Infrastructure Layer - EF Configuration
- [ ] **創建 PersonaConfiguration**: `src/AIAgentPlatform.Infrastructure/Configurations/PersonaConfiguration.cs`
  - Table: `personas`
  - Indexes: `idx_personas_name`, `idx_personas_validation_status`, `idx_personas_is_active`
  - JSONB Column: `config_json` (PostgreSQL JSONB)
  - JSONB Index: `idx_personas_config_json` (GIN)
  - Unique Constraint: `name` (when IsDeleted = false)
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 250-300

- [ ] **創建 PersonaTemplateConfiguration**: `PersonaTemplateConfiguration.cs`
  - Table: `persona_templates`
  - Indexes: `idx_persona_templates_name`, `idx_persona_templates_is_system_template`
  - JSONB Column: `default_config_json`

- [ ] **創建 PromptTemplateConfiguration**: `PromptTemplateConfiguration.cs`
  - Table: `prompt_templates`
  - Indexes: `idx_prompt_templates_persona_id`, `idx_prompt_templates_priority`
  - Foreign Key: `persona_id` → `personas(id)`

#### Migration
- [ ] **創建 Migration**: `dotnet ef migrations add AddPersonaFramework`
  - Migration 文件: `20251216_AddPersonaFramework.cs`
  - Up 方法: 創建 3 個 tables + indexes
  - Down 方法: 刪除 tables
  - 驗證: Migration 文件正確生成

- [ ] **執行 Migration (Local)**: `dotnet ef database update`
  - 驗證: `personas`, `persona_templates`, `prompt_templates` 表創建成功
  - 驗證: 所有 indexes 創建成功（包括 JSONB GIN index）
  - 驗證: Foreign Key 約束正確

- [ ] **驗證 Migration 可回滾**: `dotnet ef database update [previous]`
  - 測試 Down 方法執行正常
  - 驗證資料表正確刪除

#### Repository Interface
- [ ] **創建 IPersonaRepository**: `src/AIAgentPlatform.Domain/Interfaces/IPersonaRepository.cs`
  - Methods: `GetByIdAsync`, `GetAllAsync`, `AddAsync`, `UpdateAsync`, `DeleteAsync`
  - Methods: `GetByNameAsync`, `GetByTemplateIdAsync`, `SearchAsync`
  - Methods: `GetByConfigCriteriaAsync` (JSONB 查詢)
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 300-350

- [ ] **創建 IPersonaTemplateRepository**: `IPersonaTemplateRepository.cs`
  - Methods: 標準 CRUD + `GetSystemTemplatesAsync`

- [ ] **創建 IPromptTemplateRepository**: `IPromptTemplateRepository.cs`
  - Methods: 標準 CRUD + `GetByPersonaIdAsync`, `GetByPriorityAsync`

#### Repository Implementation
- [ ] **實作 PersonaRepository**: `src/AIAgentPlatform.Infrastructure/Repositories/PersonaRepository.cs`
  - 實現所有 CRUD 方法
  - 實現進階查詢（JSONB 查詢、分頁、過濾、排序）
  - JSONB 查詢範例: `WHERE config_json @> '{"communicationStyle": {"tone": "Professional"}}'`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 300-350

- [ ] **實作 PersonaTemplateRepository**: `PersonaTemplateRepository.cs`
  - 實現標準 CRUD
  - 實現 System Template 專用查詢

- [ ] **實作 PromptTemplateRepository**: `PromptTemplateRepository.cs`
  - 實現標準 CRUD
  - 實現 Persona 關聯查詢

#### Validation
- [ ] **創建 PersonaConfigValidator**: `src/AIAgentPlatform.Application/Personas/Validators/PersonaConfigValidator.cs`
  - FluentValidation Rules: Name (必填, 1-50 字元), Role (必填, 1-100 字元)
  - Config Validation: JSON Schema 驗證
  - Consistency Validation: CommunicationStyle 一致性檢查
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 350-400

- [ ] **JSON Schema Definition**: `PersonaConfigSchema.json`
  - Schema Version: Draft-07
  - Required Fields: CommunicationStyle, ResponsePattern, SafetyGuardrails
  - Type Validation: 所有欄位類型定義
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 100-150

#### Unit Tests (P0)
- [ ] **Repository 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Repositories/PersonaRepositoryTests.cs`
  - `AddAsync_ValidPersona_Succeeds`
  - `GetByIdAsync_ExistingPersona_ReturnsPersona`
  - `GetByConfigCriteriaAsync_MatchingConfig_ReturnsPersonas` (JSONB 查詢測試)
  - `UpdateAsync_ExistingPersona_UpdatesSuccessfully`
  - `DeleteAsync_ExistingPersona_SoftDeletes`
  - 測試覆蓋率: ≥85%

- [ ] **Validator 單元測試**: `PersonaConfigValidatorTests.cs`
  - `Validate_ValidConfig_ReturnsSuccess`
  - `Validate_InvalidName_ReturnsFailure`
  - `Validate_InvalidJSONSchema_ReturnsFailure`
  - `Validate_ConflictingRules_ReturnsWarning`
  - 測試覆蓋率: ≥85%

**驗收標準 (Phase 2)**:
- ✅ EF Configuration 完整，所有 indexes 正確
- ✅ Migration 成功執行，資料表創建正確
- ✅ Repository CRUD 功能完整，JSONB 查詢正常
- ✅ FluentValidation 覆蓋所有驗證規則
- ✅ JSON Schema 驗證正確
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過

---

### Phase 3: Application Layer (CQRS) (1 SP) ⏳ 待開始

#### Commands
- [ ] **創建 CreatePersonaCommand**: `src/AIAgentPlatform.Application/Personas/Commands/CreatePersona/CreatePersonaCommand.cs`
  - Properties: Name, Role, Description, Config
  - MediatR: `IRequest<Result<PersonaDto>>`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 400-450

- [ ] **創建 CreatePersonaCommandHandler**: `CreatePersonaCommandHandler.cs`
  - 邏輯: 驗證 Config → 創建 Persona Entity → 儲存到資料庫 → 發布 PersonaCreatedEvent
  - Error Handling: Result Pattern
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 400-450

- [ ] **創建 CreatePersonaCommandValidator**: `CreatePersonaCommandValidator.cs`
  - FluentValidation Rules: 委託給 PersonaConfigValidator
  - Business Rules: Name 唯一性檢查

- [ ] **創建 UpdatePersonaCommand**: `UpdatePersona/UpdatePersonaCommand.cs`
  - Properties: Id, Name, Role, Description, Config
  - MediatR: `IRequest<Result<PersonaDto>>`

- [ ] **創建 UpdatePersonaCommandHandler**: `UpdatePersonaCommandHandler.cs`
  - 邏輯: 檢查 Persona 存在 → 驗證 Config → 更新 → 發布 PersonaUpdatedEvent

- [ ] **創建 DeletePersonaCommand**: `DeletePersona/DeletePersonaCommand.cs`
  - Properties: Id
  - MediatR: `IRequest<Result>`

- [ ] **創建 DeletePersonaCommandHandler**: `DeletePersonaCommandHandler.cs`
  - 邏輯: Soft Delete (IsDeleted = true)

- [ ] **創建 ActivatePersonaCommand**: `ActivatePersona/ActivatePersonaCommand.cs`
  - Properties: Id
  - 邏輯: IsActive = true, 發布 PersonaActivatedEvent

- [ ] **創建 DeactivatePersonaCommand**: `DeactivatePersona/DeactivatePersonaCommand.cs`
  - Properties: Id
  - 邏輯: IsActive = false, 發布 PersonaDeactivatedEvent

#### Queries
- [ ] **創建 GetPersonaByIdQuery**: `src/AIAgentPlatform.Application/Personas/Queries/GetPersonaById/GetPersonaByIdQuery.cs`
  - Properties: Id
  - MediatR: `IRequest<Result<PersonaDto>>`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 400-450

- [ ] **創建 GetPersonaByIdQueryHandler**: `GetPersonaByIdQueryHandler.cs`
  - 邏輯: 查詢 Persona → Mapping 到 DTO
  - Error Handling: 404 Not Found

- [ ] **創建 GetPersonasQuery**: `GetPersonas/GetPersonasQuery.cs`
  - Properties: IsActive, TemplateId, Page, PageSize, SortBy, SortOrder
  - MediatR: `IRequest<Result<PaginatedResult<PersonaDto>>>`

- [ ] **創建 GetPersonasQueryHandler**: `GetPersonasQueryHandler.cs`
  - 邏輯: 複雜過濾 + 分頁 + 排序
  - Pagination: Skip/Take 實現

- [ ] **創建 GetPersonaTemplatesQuery**: `GetPersonaTemplates/GetPersonaTemplatesQuery.cs`
  - Properties: 無 (查詢所有系統模板)
  - MediatR: `IRequest<Result<List<PersonaTemplateDto>>>`

- [ ] **創建 GetPersonaTemplatesQueryHandler**: `GetPersonaTemplatesQueryHandler.cs`
  - 邏輯: 查詢所有 IsSystemTemplate = true 的模板

- [ ] **創建 SearchPersonasQuery**: `SearchPersonas/SearchPersonasQuery.cs`
  - Properties: SearchTerm, ConfigCriteria (JSONB 查詢), Page, PageSize
  - 邏輯: 全文搜尋 + JSONB 條件查詢

#### DTOs
- [ ] **創建 PersonaDto**: `src/AIAgentPlatform.Application/Personas/DTOs/PersonaDto.cs`
  - Properties: Id, Name, Role, Description, Config, ValidationStatus, IsActive, CreatedAt, UpdatedAt
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 400-450

- [ ] **創建 PersonaTemplateDto**: `PersonaTemplateDto.cs`
  - Properties: Id, Name, Role, Description, DefaultConfig, IsSystemTemplate

- [ ] **創建 CreatePersonaDto**: `CreatePersonaDto.cs`
  - Properties: Name, Role, Description, Config (JSON)

- [ ] **創建 UpdatePersonaDto**: `UpdatePersonaDto.cs`
  - Properties: Name, Role, Description, Config (JSON)

#### AutoMapper
- [ ] **創建 PersonaMappingProfile**: `src/AIAgentPlatform.Application/Personas/MappingProfiles/PersonaMappingProfile.cs`
  - Mapping: Persona → PersonaDto
  - Mapping: PersonaTemplate → PersonaTemplateDto
  - Mapping: CreatePersonaDto → Persona
  - Custom Resolver: Config (JSON ↔ PersonaConfig VO)

#### Unit Tests (P0)
- [ ] **Command Handler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/Personas/Commands/CreatePersonaCommandHandlerTests.cs`
  - `Handle_ValidCommand_ReturnsSuccessResult`
  - `Handle_InvalidConfig_ReturnsFailureResult`
  - `Handle_DuplicateName_ReturnsFailureResult`
  - Mock: IPersonaRepository, IValidator
  - 測試覆蓋率: ≥85%

- [ ] **Query Handler 單元測試**: `GetPersonaByIdQueryHandlerTests.cs`
  - `Handle_ExistingPersona_ReturnsPersonaDto`
  - `Handle_NonExistingPersona_ReturnsNotFoundResult`
  - Mock: IPersonaRepository
  - 測試覆蓋率: ≥85%

- [ ] **Validator 單元測試**: `CreatePersonaCommandValidatorTests.cs`
  - `Validate_ValidCommand_ReturnsSuccess`
  - `Validate_InvalidName_ReturnsFailure`
  - `Validate_EmptyConfig_ReturnsFailure`

**驗收標準 (Phase 3)**:
- ✅ Commands 定義完整，所有 Handler 實現
- ✅ Queries 定義完整，支援分頁、篩選、排序
- ✅ FluentValidation 覆蓋所有驗證規則
- ✅ DTO 映射正確 (AutoMapper)
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過

---

### Phase 4: API Layer (1 SP) ⏳ 待開始

#### API Controller
- [ ] **創建 PersonasController**: `src/AIAgentPlatform.API/Controllers/PersonasController.cs`
  - Base: `ApiControllerBase` (繼承 MediatR)
  - Route: `/api/v1/personas`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 300-350

#### API Endpoints
- [ ] **POST /api/v1/personas**: 創建 Persona
  - Request: `CreatePersonaDto`
  - Response: `PersonaDto` (201 Created)
  - Location Header: `/api/v1/personas/{id}`
  - Error Handling: 400 (驗證錯誤), 409 (Name 重複), 500
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 300-350

- [ ] **GET /api/v1/personas/{id}**: 獲取單一 Persona
  - Response: `PersonaDto` (200 OK)
  - Error Handling: 404 Not Found

- [ ] **GET /api/v1/personas**: 查詢 Persona 列表
  - Query Params: isActive, templateId, page, pageSize, sortBy, sortOrder
  - Response: `PaginatedResult<PersonaDto>` (200 OK)
  - Pagination: X-Total-Count Header

- [ ] **PUT /api/v1/personas/{id}**: 更新 Persona
  - Request: `UpdatePersonaDto`
  - Response: `PersonaDto` (200 OK)
  - Error Handling: 400 (驗證錯誤), 404 Not Found

- [ ] **DELETE /api/v1/personas/{id}**: 刪除 Persona (Soft Delete)
  - Response: 204 No Content
  - Error Handling: 404 Not Found

- [ ] **POST /api/v1/personas/{id}/activate**: 啟用 Persona
  - Response: `PersonaDto` (200 OK)
  - Error Handling: 404 Not Found

- [ ] **POST /api/v1/personas/{id}/deactivate**: 停用 Persona
  - Response: `PersonaDto` (200 OK)
  - Error Handling: 404 Not Found

- [ ] **GET /api/v1/personas/templates**: 獲取預設模板列表
  - Response: `List<PersonaTemplateDto>` (200 OK)
  - Filter: 僅返回 System Templates

#### Swagger Documentation
- [ ] **添加 XML 註解**: 所有 Controller 方法
  - Summary, Remarks, Param, Returns
  - 範例 Request/Response

- [ ] **配置 Swagger UI**: `Program.cs`
  - Endpoint: `/swagger`
  - XML Documentation: 啟用
  - Examples: 配置 Swashbuckle.AspNetCore.Filters

#### API Integration Tests (P0)
- [ ] **Integration Test Setup**: `tests/AIAgentPlatform.IntegrationTests/Personas/PersonasControllerTests.cs`
  - WebApplicationFactory: In-Memory Database
  - Test Client: HttpClient

- [ ] **測試 POST /api/v1/personas**: `CreatePersona_Should_Return201_When_ValidRequest`
  - Happy Path: 正常創建
  - 驗證: Response Status 201, Location Header 正確
  - 驗證: Response Body 包含正確的 PersonaDto

- [ ] **測試 POST 驗證錯誤**: `CreatePersona_Should_Return400_When_InvalidRequest`
  - Invalid Name: 空字串, 超過長度限制
  - Invalid Config: 不符合 JSON Schema
  - 驗證: Response Status 400, Error Message 清晰

- [ ] **測試 POST 重複 Name**: `CreatePersona_Should_Return409_When_DuplicateName`
  - 驗證: Response Status 409 Conflict

- [ ] **測試 GET /api/v1/personas/{id}**: `GetPersonaById_Should_Return200_When_Exists`
  - Happy Path: 查詢存在的 Persona
  - 驗證: Response Status 200, Response Body 正確

- [ ] **測試 GET 404**: `GetPersonaById_Should_Return404_When_NotFound`
  - 驗證: Response Status 404

- [ ] **測試 GET /api/v1/personas**: `GetPersonas_Should_Return200_With_PaginatedResults`
  - Happy Path: 分頁查詢
  - 驗證: X-Total-Count Header 正確
  - 驗證: Response Body 包含正確數量的項目

- [ ] **測試 GET 篩選**: `GetPersonas_Should_FilterByIsActive`
  - Filter: isActive=true
  - 驗證: 僅返回 IsActive = true 的 Persona

- [ ] **測試 PUT /api/v1/personas/{id}**: `UpdatePersona_Should_Return200_When_ValidRequest`
  - Happy Path: 更新成功
  - 驗證: Response Status 200, Response Body 包含更新後的資料

- [ ] **測試 DELETE /api/v1/personas/{id}**: `DeletePersona_Should_Return204_When_Success`
  - Happy Path: Soft Delete 成功
  - 驗證: Response Status 204
  - 驗證: 資料庫中 IsDeleted = true

**驗收標準 (Phase 4)**:
- ✅ 8 個 API 端點全部實現
- ✅ HTTP 狀態碼正確 (200, 201, 204, 400, 404, 409, 500)
- ✅ Swagger 文檔完整，所有端點有範例
- ✅ API 整合測試覆蓋率 ≥80%
- ✅ 所有測試通過
- ✅ Code Review 通過

---

### Phase 5: 10 種預設 Persona 模板 (1 SP) ⏳ 待開始

#### 預設模板定義
- [ ] **Professional Assistant 模板**: JSON 配置文件
  - Name: "Professional Assistant"
  - Role: "通用專業助理"
  - CommunicationStyle: Tone=Professional, Formality=Formal, Verbosity=Balanced
  - ResponsePattern: StructurePreference=StepByStep, ExplanationDepth=Detailed
  - SafetyGuardrails: 基本安全規則
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 550-600

- [ ] **Friendly Helper 模板**: JSON 配置文件
  - Name: "Friendly Helper"
  - Role: "友善協助者"
  - CommunicationStyle: Tone=Friendly, Formality=Casual, Verbosity=Concise, EmojiUsage=Occasional
  - ResponsePattern: StructurePreference=Conversational

- [ ] **Technical Expert 模板**: JSON 配置文件
  - Name: "Technical Expert"
  - Role: "技術專家"
  - CommunicationStyle: Tone=Professional, Formality=Neutral, Verbosity=Detailed
  - ResponsePattern: StructurePreference=StepByStep, ExplanationDepth=VeryDetailed, ExampleFrequency=Always
  - ExpertiseAreas: ["Programming", "System Architecture", "Debugging"]

- [ ] **Creative Companion 模板**: JSON 配置文件
  - Name: "Creative Companion"
  - Role: "創意夥伴"
  - CommunicationStyle: Tone=Enthusiastic, Formality=Casual, Verbosity=Rich, EmojiUsage=Frequent
  - ResponsePattern: StructurePreference=Creative, ExplanationDepth=Balanced

- [ ] **Formal Consultant 模板**: JSON 配置文件
  - Name: "Formal Consultant"
  - Role: "正式顧問"
  - CommunicationStyle: Tone=Professional, Formality=VeryFormal, Verbosity=Precise
  - ResponsePattern: StructurePreference=Structured, ExplanationDepth=Detailed

- [ ] **Casual Advisor 模板**: JSON 配置文件
  - Name: "Casual Advisor"
  - Role: "輕鬆顧問"
  - CommunicationStyle: Tone=Casual, Formality=VeryCasual, Verbosity=Concise
  - ResponsePattern: StructurePreference=Flexible

- [ ] **Educational Tutor 模板**: JSON 配置文件
  - Name: "Educational Tutor"
  - Role: "教育導師"
  - CommunicationStyle: Tone=Empathetic, Formality=Neutral, Verbosity=Patient
  - ResponsePattern: StructurePreference=Progressive, ExplanationDepth=VeryDetailed, ExampleFrequency=Always

- [ ] **Business Analyst 模板**: JSON 配置文件
  - Name: "Business Analyst"
  - Role: "商業分析師"
  - CommunicationStyle: Tone=Analytical, Formality=Formal, Verbosity=Balanced
  - ResponsePattern: StructurePreference=DataDriven, ExplanationDepth=Detailed
  - ExpertiseAreas: ["Market Analysis", "Financial Modeling", "Strategic Planning"]

- [ ] **Code Reviewer 模板**: JSON 配置文件
  - Name: "Code Reviewer"
  - Role: "代碼審查員"
  - CommunicationStyle: Tone=Constructive, Formality=Neutral, Verbosity=Detailed
  - ResponsePattern: StructurePreference=StepByStep, ExplanationDepth=VeryDetailed
  - ExpertiseAreas: ["Code Quality", "Best Practices", "Security"]

- [ ] **Domain Expert 模板**: JSON 配置文件
  - Name: "Domain Expert"
  - Role: "領域專家"
  - CommunicationStyle: Tone=Authoritative, Formality=Formal, Verbosity=Comprehensive
  - ResponsePattern: StructurePreference=Hierarchical, ExplanationDepth=VeryDetailed
  - ExpertiseAreas: (可自定義)

#### 模板驗證
- [ ] **JSON Schema 驗證**: 所有 10 個模板
  - 驗證工具: JSON Schema Validator
  - 確保所有模板符合 PersonaConfigSchema.json
  - 無驗證錯誤

- [ ] **一致性檢查**: 所有模板
  - CommunicationStyle 與 ResponsePattern 一致性
  - 無衝突規則
  - ExpertiseAreas 合理性

#### 資料庫 Seeding
- [ ] **創建 Data Seeder**: `src/AIAgentPlatform.Infrastructure/Data/PersonaTemplateSeed.cs`
  - 方法: `SeedPersonaTemplatesAsync`
  - 邏輯: 檢查是否已存在 → 插入 10 個模板 → 標記 IsSystemTemplate = true
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 550-600

- [ ] **配置 Seeding**: `Program.cs`
  - 在應用啟動時執行 SeedPersonaTemplatesAsync
  - 環境: Development, Staging, Production (首次啟動)

- [ ] **驗證 Seeding**: 啟動應用
  - 檢查資料庫: persona_templates 表有 10 筆記錄
  - 檢查: 所有模板 IsSystemTemplate = true
  - 檢查: Config JSON 正確存儲

#### 模板文檔
- [ ] **撰寫模板使用指南**: `docs/user-guides/persona-templates-guide.md`
  - 每個模板的適用場景
  - 配置參數說明
  - 使用範例 (如何基於模板創建 Persona)
  - 最佳實踐

- [ ] **API 文檔範例**: Swagger
  - GET /api/v1/personas/templates 回應範例
  - 每個模板的 JSON 範例

#### Integration Tests (P0)
- [ ] **模板載入測試**: `tests/AIAgentPlatform.IntegrationTests/Personas/PersonaTemplateTests.cs`
  - `GetPersonaTemplates_Should_Return10SystemTemplates`
  - 驗證: 返回 10 個模板
  - 驗證: 所有模板 IsSystemTemplate = true

- [ ] **基於模板創建測試**: `CreatePersonaFromTemplate_Should_Succeed`
  - 基於 "Professional Assistant" 模板創建 Persona
  - 驗證: Persona Config 繼承模板 Default Config
  - 驗證: ValidationStatus = Valid

**驗收標準 (Phase 5)**:
- ✅ 10 種 Persona 模板完整定義
- ✅ 所有模板通過 JSON Schema 驗證
- ✅ 資料庫 Seeding 成功，10 個模板已插入
- ✅ 模板使用指南文檔完整
- ✅ Integration Tests 通過
- ✅ Swagger 文檔包含模板範例
- ✅ Code Review 通過

---

## US 7.2: Persona-Driven Prompt Engineering (5 SP, 4 Phases)

### Phase 1: Liquid Template Engine Integration (1.5 SP) ⏳ 待開始

#### Liquid Template Engine Setup
- [ ] **安裝 Fluid Library**: `dotnet add package Fluid.Core`
  - NuGet Package: Fluid.Core (最新穩定版)
  - 驗證: 依賴正確安裝

- [ ] **創建 IFluidTemplateEngine Interface**: `src/AIAgentPlatform.Application/Interfaces/IFluidTemplateEngine.cs`
  - Methods: `ParseAsync`, `RenderAsync`, `ValidateSyntaxAsync`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 450-500

- [ ] **實作 FluidTemplateEngine**: `src/AIAgentPlatform.Infrastructure/Services/FluidTemplateEngine.cs`
  - 使用 FluidParser 解析模板
  - 使用 TemplateContext 注入變數
  - Error Handling: 捕獲 Liquid 語法錯誤
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 450-500

#### Liquid Template Syntax Support
- [ ] **變數替換**: `{{ variable }}`
  - 範例: `{{ user_name }}`, `{{ persona.role }}`
  - 支援 Nested Object: `{{ persona.config.communicationStyle.tone }}`

- [ ] **條件邏輯**: `{% if %} ... {% endif %}`
  - 範例: `{% if expertise == "Programming" %} ... {% endif %}`
  - 支援 else, elsif

- [ ] **循環**: `{% for item in array %} ... {% endfor %}`
  - 範例: `{% for topic in prohibited_topics %} ... {% endfor %}`

- [ ] **過濾器 (Filters)**: `{{ variable | filter }}`
  - 內建過濾器: `capitalize`, `downcase`, `truncate`
  - 自訂過濾器: 根據需要擴展

#### Prompt Template Structure
- [ ] **定義 Prompt Template 變數**:
  - `persona`: Persona 配置對象 (CommunicationStyle, ResponsePattern, etc.)
  - `user_message`: 使用者輸入訊息
  - `conversation_history`: 對話歷史 (可選)
  - `system_context`: 系統上下文 (可選)
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 450-500

- [ ] **範例 Prompt Template**:
  ```liquid
  You are {{ persona.name }}, a {{ persona.role }}.
  Your communication style: {{ persona.config.communicationStyle.tone }}, {{ persona.config.communicationStyle.formality }}.

  {% if persona.config.safetyGuardrails.prohibitedTopics %}
  Do not discuss: {% for topic in persona.config.safetyGuardrails.prohibitedTopics %}{{ topic }}{% unless forloop.last %}, {% endunless %}{% endfor %}.
  {% endif %}

  User: {{ user_message }}

  Please respond according to your persona configuration.
  ```

#### Prompt Template Storage
- [ ] **PromptTemplate Entity 關聯**: 已在 US 7.1 Phase 1 創建
  - Foreign Key: PersonaId → Personas(Id)
  - Content: Liquid 模板內容
  - Variables: JSON Array 列出所有變數
  - Priority: 執行優先級

- [ ] **Default System Prompt Templates**: 創建系統預設模板
  - `system_prompt_general.liquid`: 通用系統提示
  - `system_prompt_professional.liquid`: Professional 風格提示
  - `system_prompt_creative.liquid`: Creative 風格提示

#### Unit Tests (P0)
- [ ] **FluidTemplateEngine 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/FluidTemplateEngineTests.cs`
  - `ParseAsync_ValidTemplate_Succeeds`
  - `ParseAsync_InvalidSyntax_ThrowsException`
  - `RenderAsync_WithVariables_RendersCorrectly`
  - `RenderAsync_ConditionalLogic_WorksCorrectly`
  - `RenderAsync_Loop_WorksCorrectly`
  - 測試覆蓋率: ≥85%

- [ ] **Liquid Syntax 測試**:
  - 測試所有支援的語法 (變數、條件、循環、過濾器)
  - 測試 Nested Object 訪問
  - 測試錯誤處理 (未定義變數、語法錯誤)

**驗收標準 (Phase 1)**:
- ✅ Fluid Library 整合完成
- ✅ Liquid Template Engine 實現完整
- ✅ 支援變數、條件、循環、過濾器
- ✅ Prompt Template 結構定義清晰
- ✅ 系統預設模板創建完成
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過

---

### Phase 2: Prompt Generation Service (1.5 SP) ⏳ 待開始

#### Prompt Generation Service
- [ ] **創建 IPromptGenerationService Interface**: `src/AIAgentPlatform.Application/Interfaces/IPromptGenerationService.cs`
  - Methods: `GeneratePromptAsync`, `GenerateSystemPromptAsync`, `ValidatePromptAsync`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

- [ ] **實作 PromptGenerationService**: `src/AIAgentPlatform.Infrastructure/Services/PromptGenerationService.cs`
  - Dependency: IFluidTemplateEngine, IPersonaRepository, IPromptTemplateRepository
  - 6-Step Algorithm Implementation:
    1. 載入 Persona 配置
    2. 選擇 Prompt Template (by Priority)
    3. 渲染 Liquid Template
    4. 應用 Persona 行為注入
    5. Token 計數與截斷 (Phase 3)
    6. 快取結果 (Phase 3)
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

#### 6-Step Prompt Generation Algorithm

**Step 1: 載入 Persona 配置**
- [ ] **實作 LoadPersonaConfigAsync**:
  - 輸入: `personaId`
  - 邏輯: `IPersonaRepository.GetByIdAsync` → 驗證 Persona 存在 → 返回 PersonaConfig
  - Error Handling: Persona 不存在 → 拋出 NotFoundException

**Step 2: 選擇 Prompt Template**
- [ ] **實作 SelectPromptTemplateAsync**:
  - 輸入: `personaId`
  - 邏輯: `IPromptTemplateRepository.GetByPersonaIdAsync` → 按 Priority 排序 → 選擇最高優先級模板
  - Fallback: 若無專屬模板，使用系統預設模板 (`system_prompt_general`)
  - 返回: `PromptTemplate` Entity

**Step 3: 渲染 Liquid Template**
- [ ] **實作 RenderTemplateAsync**:
  - 輸入: `PromptTemplate.Content`, `variables` (persona, user_message, etc.)
  - 邏輯:
    ```csharp
    var templateContext = new TemplateContext();
    templateContext.SetValue("persona", personaConfig);
    templateContext.SetValue("user_message", userMessage);
    templateContext.SetValue("conversation_history", conversationHistory);

    var result = await _fluidTemplateEngine.RenderAsync(template.Content, templateContext);
    ```
  - Error Handling: Liquid 語法錯誤 → 拋出 TemplateRenderException

**Step 4: 應用 Persona 行為注入**
- [ ] **實作 InjectPersonaBehaviorAsync**:
  - 輸入: `renderedPrompt`, `personaConfig`
  - 邏輯:
    - 注入 CommunicationStyle 指令: "Use a {{ tone }} tone with {{ formality }} formality."
    - 注入 ResponsePattern 指令: "Structure your response in a {{ structurePreference }} manner."
    - 注入 SafetyGuardrails: "Do not discuss: {{ prohibitedTopics }}."
    - 注入 ExpertiseAreas: "You are an expert in: {{ expertiseAreas }}."
  - 返回: `enrichedPrompt` (string)

**Step 5: Token 計數與截斷** (Phase 3 實現)
- [ ] 預留介面: `TruncatePromptAsync`

**Step 6: 快取結果** (Phase 3 實現)
- [ ] 預留介面: `CachePromptAsync`

#### Prompt Generation Result
- [ ] **創建 GeneratedPrompt DTO**: `src/AIAgentPlatform.Application/PromptGeneration/DTOs/GeneratedPromptDto.cs`
  - Properties:
    - `SystemPrompt`: string (完整系統提示)
    - `UserPrompt`: string (使用者訊息)
    - `TotalTokens`: int (總 Token 數)
    - `PersonaId`: Guid
    - `TemplateId`: Guid
    - `GeneratedAt`: DateTime
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

#### Unit Tests (P0)
- [ ] **PromptGenerationService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/PromptGenerationServiceTests.cs`
  - `GeneratePromptAsync_ValidInput_Succeeds`
  - `GeneratePromptAsync_PersonaNotFound_ThrowsException`
  - `GeneratePromptAsync_TemplateNotFound_UsesFallback`
  - `GeneratePromptAsync_LiquidSyntaxError_ThrowsException`
  - `InjectPersonaBehavior_AppliesCorrectly`
  - Mock: IFluidTemplateEngine, IPersonaRepository, IPromptTemplateRepository
  - 測試覆蓋率: ≥85%

- [ ] **6-Step Algorithm Integration Test**:
  - 完整流程測試: 從 personaId → 生成 Prompt
  - 驗證: 每個步驟正確執行
  - 驗證: 最終 Prompt 符合預期格式

**驗收標準 (Phase 2)**:
- ✅ PromptGenerationService 實現完整
- ✅ 6-Step Algorithm 正確實現 (Step 1-4)
- ✅ Persona 行為注入邏輯正確
- ✅ Template 選擇與 Fallback 機制正常
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Integration Test 通過
- ✅ Code Review 通過

---

### Phase 3: Token Management & Caching (1 SP) ⏳ 待開始

#### Token Counting
- [ ] **安裝 TikToken Library**: `dotnet add package TikToken`
  - NuGet Package: TikToken (SharpToken)
  - 驗證: 依賴正確安裝
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

- [ ] **創建 ITokenCounter Interface**: `src/AIAgentPlatform.Application/Interfaces/ITokenCounter.cs`
  - Methods: `CountTokensAsync`, `EstimateTokensAsync`
  - Model: 支援多種 LLM Model (GPT-3.5, GPT-4, Claude, etc.)

- [ ] **實作 TokenCounter**: `src/AIAgentPlatform.Infrastructure/Services/TokenCounter.cs`
  - 使用 TikToken 計算 Token 數
  - 支援不同 Model 的 Encoding: `cl100k_base` (GPT-4), `p50k_base` (GPT-3.5)
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

- [ ] **Prompt Token Limit 配置**: `appsettings.json`
  ```json
  {
    "PromptGeneration": {
      "MaxSystemPromptTokens": 2000,
      "MaxUserPromptTokens": 4000,
      "MaxTotalTokens": 6000
    }
  }
  ```

#### Prompt Truncation Strategy
- [ ] **實作 TruncatePromptAsync** (in PromptGenerationService):
  - 輸入: `enrichedPrompt`, `maxTokens`
  - 邏輯:
    1. 計算 Token 數: `var tokenCount = await _tokenCounter.CountTokensAsync(enrichedPrompt);`
    2. 如果超過限制: 截斷策略
       - 優先保留: System Prompt 核心指令
       - 可截斷: Conversation History (保留最近 N 條)
       - 最後截斷: 詳細範例與說明
    3. 重新計算 Token 數，確保 ≤ `maxTokens`
  - 返回: `truncatedPrompt`, `actualTokenCount`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

- [ ] **Truncation 警告日誌**:
  - 當截斷發生時: 記錄 Warning Log
  - 日誌內容: Original Token Count, Truncated Token Count, PersonaId

#### Redis Cache Integration
- [ ] **Redis 連接配置**: `appsettings.json`
  ```json
  {
    "Redis": {
      "ConnectionString": "localhost:6379",
      "InstanceName": "AIAgentPlatform:",
      "DefaultExpiration": 3600
    }
  }
  ```

- [ ] **Prompt Cache Key Strategy**:
  - Key Format: `prompt:{personaId}:{userMessageHash}`
  - Hash Algorithm: SHA256 (前 16 字元)
  - TTL: 1 hour (可配置)

- [ ] **實作 CachePromptAsync** (in PromptGenerationService):
  - 輸入: `personaId`, `userMessage`, `generatedPrompt`
  - 邏輯:
    1. 生成 Cache Key: `var cacheKey = $"prompt:{personaId}:{HashUserMessage(userMessage)}";`
    2. 序列化 GeneratedPromptDto: JSON
    3. 存入 Redis: `await _distributedCache.SetStringAsync(cacheKey, json, options);`
    4. 設定 TTL: 1 hour
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 500-550

- [ ] **實作 GetCachedPromptAsync**:
  - 輸入: `personaId`, `userMessage`
  - 邏輯:
    1. 生成 Cache Key
    2. 從 Redis 讀取: `var json = await _distributedCache.GetStringAsync(cacheKey);`
    3. 反序列化: JSON → GeneratedPromptDto
    4. Cache Hit: 返回快取的 Prompt
    5. Cache Miss: 返回 null
  - 返回: `GeneratedPromptDto?`

- [ ] **整合 Cache 到 GeneratePromptAsync**:
  - 流程:
    1. 檢查 Cache: `var cached = await GetCachedPromptAsync(personaId, userMessage);`
    2. Cache Hit: 直接返回
    3. Cache Miss: 執行 6-Step Algorithm → Cache 結果 → 返回

#### Performance Monitoring
- [ ] **Prompt Generation Metrics**:
  - Metric 1: `prompt_generation_duration_ms` (生成時間)
  - Metric 2: `prompt_cache_hit_rate` (快取命中率)
  - Metric 3: `prompt_token_count` (Token 數量分佈)
  - Metric 4: `prompt_truncation_count` (截斷發生次數)
  - 使用: Application Insights / Prometheus

#### Unit Tests (P0)
- [ ] **TokenCounter 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/TokenCounterTests.cs`
  - `CountTokensAsync_SimpleText_ReturnsCorrectCount`
  - `CountTokensAsync_ComplexText_ReturnsCorrectCount`
  - `CountTokensAsync_DifferentModels_ReturnsDifferentCounts`
  - 測試覆蓋率: ≥85%

- [ ] **TruncatePrompt 單元測試**: `PromptGenerationServiceTests.cs`
  - `TruncatePromptAsync_WithinLimit_NoTruncation`
  - `TruncatePromptAsync_ExceedsLimit_TruncatesCorrectly`
  - `TruncatePromptAsync_PreservesSystemPrompt`
  - 驗證: 截斷後 Token 數 ≤ MaxTokens

- [ ] **Cache 單元測試**: `PromptGenerationServiceTests.cs`
  - `GeneratePromptAsync_CacheHit_ReturnsFromCache`
  - `GeneratePromptAsync_CacheMiss_GeneratesAndCaches`
  - `CachePromptAsync_StoresCorrectly`
  - Mock: IDistributedCache

#### Integration Tests (P0)
- [ ] **完整 Prompt Generation 測試**: `tests/AIAgentPlatform.IntegrationTests/PromptGeneration/PromptGenerationServiceTests.cs`
  - `GeneratePrompt_CompleteFlow_Succeeds`
  - 驗證: 6-Step Algorithm 完整執行
  - 驗證: Token 計數正確
  - 驗證: Cache 正常工作
  - 驗證: Truncation 在超過限制時觸發

**驗收標準 (Phase 3)**:
- ✅ TikToken 整合完成，Token 計數準確
- ✅ Prompt Truncation 策略實現正確
- ✅ Redis Cache 整合完成，快取正常工作
- ✅ 6-Step Algorithm 完整 (Step 1-6)
- ✅ Performance Metrics 配置完成
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Integration Tests 通過
- ✅ Code Review 通過

---

### Phase 4: API Integration & Testing (1 SP) ⏳ 待開始

#### API Controller Extension
- [ ] **擴展 AgentsController**: `src/AIAgentPlatform.API/Controllers/AgentsController.cs`
  - 新增 Prompt Generation 端點
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 550-600

#### API Endpoints
- [ ] **POST /api/v1/agents/{id}/generate-prompt**: 生成 Agent Prompt
  - Request Body:
    ```json
    {
      "personaId": "guid",
      "userMessage": "string",
      "conversationHistory": [ ... ] // optional
    }
    ```
  - Response: `GeneratedPromptDto` (200 OK)
  - Error Handling: 400 (驗證錯誤), 404 (Persona 不存在), 500
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 550-600

- [ ] **GET /api/v1/personas/{id}/prompt-templates**: 獲取 Persona 的 Prompt Templates
  - Response: `List<PromptTemplateDto>` (200 OK)
  - Error Handling: 404 Not Found

#### Command & Query
- [ ] **創建 GeneratePromptCommand**: `src/AIAgentPlatform.Application/PromptGeneration/Commands/GeneratePrompt/GeneratePromptCommand.cs`
  - Properties: PersonaId, UserMessage, ConversationHistory
  - MediatR: `IRequest<Result<GeneratedPromptDto>>`

- [ ] **創建 GeneratePromptCommandHandler**: `GeneratePromptCommandHandler.cs`
  - 邏輯: 調用 IPromptGenerationService.GeneratePromptAsync
  - Error Handling: Result Pattern

- [ ] **創建 GeneratePromptCommandValidator**: `GeneratePromptCommandValidator.cs`
  - FluentValidation Rules: PersonaId (必填), UserMessage (必填, 1-4000 字元)

- [ ] **創建 GetPromptTemplatesQuery**: `src/AIAgentPlatform.Application/PromptGeneration/Queries/GetPromptTemplates/GetPromptTemplatesQuery.cs`
  - Properties: PersonaId
  - MediatR: `IRequest<Result<List<PromptTemplateDto>>>`

- [ ] **創建 GetPromptTemplatesQueryHandler**: `GetPromptTemplatesQueryHandler.cs`
  - 邏輯: IPromptTemplateRepository.GetByPersonaIdAsync

#### Swagger Documentation
- [ ] **添加 XML 註解**: Prompt Generation 端點
  - Summary, Remarks, Param, Returns
  - 範例 Request/Response

- [ ] **Swagger Example Configuration**: `POST /api/v1/agents/{id}/generate-prompt`
  - Example Request:
    ```json
    {
      "personaId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "userMessage": "Explain Clean Architecture to me.",
      "conversationHistory": []
    }
    ```
  - Example Response:
    ```json
    {
      "systemPrompt": "You are a Technical Expert...",
      "userPrompt": "Explain Clean Architecture to me.",
      "totalTokens": 523,
      "personaId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "templateId": "...",
      "generatedAt": "2025-12-20T10:30:00Z"
    }
    ```

#### API Integration Tests (P0)
- [ ] **測試 POST /api/v1/agents/{id}/generate-prompt**: `tests/AIAgentPlatform.IntegrationTests/PromptGeneration/PromptGenerationApiTests.cs`
  - `GeneratePrompt_Should_Return200_When_ValidRequest`
  - Happy Path: 正常生成 Prompt
  - 驗證: Response Status 200
  - 驗證: Response Body 包含 GeneratedPromptDto
  - 驗證: TotalTokens > 0

- [ ] **測試 POST 驗證錯誤**: `GeneratePrompt_Should_Return400_When_InvalidRequest`
  - Invalid PersonaId: 空 Guid
  - Invalid UserMessage: 空字串, 超過 4000 字元
  - 驗證: Response Status 400, Error Message 清晰

- [ ] **測試 POST 404**: `GeneratePrompt_Should_Return404_When_PersonaNotFound`
  - Persona 不存在
  - 驗證: Response Status 404

- [ ] **測試 Cache 行為**: `GeneratePrompt_Should_UseCacheOnSecondRequest`
  - 第一次請求: Cache Miss, 正常生成
  - 第二次相同請求: Cache Hit, 返回快取結果
  - 驗證: 第二次請求更快 (< 10ms)

- [ ] **測試 Token Truncation**: `GeneratePrompt_Should_TruncateWhenExceedsLimit`
  - UserMessage: 非常長的訊息 (> MaxUserPromptTokens)
  - 驗證: Response TotalTokens ≤ MaxTotalTokens
  - 驗證: Response 中包含完整的 SystemPrompt

- [ ] **測試 GET /api/v1/personas/{id}/prompt-templates**: `GetPromptTemplates_Should_Return200`
  - Happy Path: 查詢 Persona 的 Templates
  - 驗證: Response Status 200
  - 驗證: Response Body 包含 Templates 列表

#### E2E Tests (P0)
- [ ] **Playwright: 完整 Prompt Generation 流程**
  - 步驟:
    1. 選擇 Persona: "Technical Expert"
    2. 輸入 User Message: "Explain CQRS pattern"
    3. 點擊 "Generate Prompt" 按鈕
    4. 驗證: Prompt 顯示在 UI
    5. 驗證: Token Count 顯示
  - 驗證: E2E 流程順暢

**驗收標準 (Phase 4)**:
- ✅ Prompt Generation API 端點實現完整
- ✅ Command/Query Handler 實現
- ✅ Swagger 文檔完整，包含範例
- ✅ API Integration Tests 覆蓋率 ≥80%
- ✅ 所有測試通過 (單元 + 整合 + E2E)
- ✅ Cache 行為正確，命中率 > 50% (重複請求)
- ✅ Token Truncation 正確工作
- ✅ Code Review 通過

---

## US 2.2: Plugin Hot Reload (3 SP, 4 Phases)

### Phase 1: AssemblyLoadContext Architecture Design (1 SP) ⏳ 待開始

#### AssemblyLoadContext Design
- [ ] **創建 IPluginLoader Interface 擴展**: `src/AIAgentPlatform.Application/Interfaces/IPluginLoader.cs`
  - 擴展方法:
    - `LoadPluginVersionAsync(string pluginId, VersionNumber version)`
    - `UnloadPluginVersionAsync(string pluginId, VersionNumber version)`
    - `GetActiveVersionAsync(string pluginId)`
    - `ListLoadedVersionsAsync(string pluginId)`
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 600-650

- [ ] **創建 PluginAssemblyLoadContext**: `src/AIAgentPlatform.Infrastructure/Services/PluginAssemblyLoadContext.cs`
  - 繼承: `AssemblyLoadContext`
  - 構造函數參數: `string pluginName`, `VersionNumber version`, `string pluginPath`, `ILogger`
  - 屬性:
    - `PluginName`: string
    - `Version`: VersionNumber
    - `IsCollectible`: true (支援卸載)
  - Override Methods:
    - `Load(AssemblyName assemblyName)`: 自訂 Assembly 載入邏輯
    - `LoadUnmanagedDll(string unmanagedDllName)`: 處理 Native DLL
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 600-650

- [ ] **Assembly 隔離策略**:
  - **Shared Framework Assemblies**: 共享載入 (避免重複)
    - System.*, Microsoft.*, Newtonsoft.Json
    - 策略: 檢查 AssemblyName → 如果是 Framework Assembly → 返回 null (使用 Default Context)
  - **Plugin-Specific Assemblies**: 隔離載入
    - Plugin 自身 DLL + 依賴
    - 策略: 使用 AssemblyDependencyResolver 解析路徑 → LoadFromAssemblyPath
  - **Dependency Resolution**:
    - 使用 `AssemblyDependencyResolver` 解析 Plugin 依賴
    - 自動載入 Plugin 目錄下的依賴 DLL

- [ ] **創建 LoadedPluginInfo**: `src/AIAgentPlatform.Infrastructure/Services/LoadedPluginInfo.cs`
  - Properties:
    - `PluginId`: string
    - `Version`: VersionNumber
    - `Assembly`: Assembly
    - `LoadContext`: PluginAssemblyLoadContext
    - `LoadedAt`: DateTime
    - `IsActive`: bool
  - 用途: 追蹤已載入的 Plugin 版本

- [ ] **創建 PluginVersionManager**: `src/AIAgentPlatform.Infrastructure/Services/PluginVersionManager.cs`
  - 責任: 管理多版本 Plugin 共存
  - 資料結構: `ConcurrentDictionary<string, ConcurrentDictionary<VersionNumber, LoadedPluginInfo>>`
  - 方法:
    - `RegisterLoadedPlugin(LoadedPluginInfo info)`
    - `UnregisterLoadedPlugin(string pluginId, VersionNumber version)`
    - `GetLoadedPlugin(string pluginId, VersionNumber version)`
    - `GetActiveVersion(string pluginId)`
    - `SetActiveVersion(string pluginId, VersionNumber version)`
    - `ListLoadedVersions(string pluginId)`

#### Memory Management
- [ ] **WeakReference 追蹤**: 記憶體洩漏檢測
  - 創建 WeakReference 到 AssemblyLoadContext
  - 在卸載後檢查: `weakRef.IsAlive` → 如果 true, 表示記憶體洩漏
  - 記錄 Warning Log

- [ ] **GC 強制回收策略**: 卸載後觸發
  - 卸載 Plugin 後: `GC.Collect(); GC.WaitForPendingFinalizers(); GC.Collect();`
  - 等待 GC 回收 AssemblyLoadContext

- [ ] **記憶體監控 Metrics**:
  - Metric 1: `plugin_memory_usage_mb` (每個 Plugin 的記憶體使用)
  - Metric 2: `plugin_assembly_count` (已載入的 Assembly 數量)
  - Metric 3: `plugin_reload_count` (重載次數)
  - Metric 4: `plugin_memory_leak_detected` (記憶體洩漏檢測)

#### Unit Tests (P0)
- [ ] **PluginAssemblyLoadContext 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/PluginAssemblyLoadContextTests.cs`
  - `Load_FrameworkAssembly_ReturnsNull`
  - `Load_PluginAssembly_LoadsFromPluginPath`
  - `Load_DependentAssembly_ResolvesCorrectly`
  - `Constructor_IsCollectibleTrue`
  - 測試覆蓋率: ≥85%

- [ ] **PluginVersionManager 單元測試**: `PluginVersionManagerTests.cs`
  - `RegisterLoadedPlugin_AddsToRegistry`
  - `UnregisterLoadedPlugin_RemovesFromRegistry`
  - `GetActiveVersion_ReturnsCurrentActiveVersion`
  - `SetActiveVersion_UpdatesActiveVersion`
  - `ListLoadedVersions_ReturnsAllVersions`
  - 測試覆蓋率: ≥85%

- [ ] **記憶體洩漏測試**:
  - `UnloadPlugin_ReleasesMemory`
  - 步驟:
    1. 載入 Plugin → 記錄記憶體使用
    2. 卸載 Plugin → 強制 GC
    3. 檢查 WeakReference.IsAlive → 應為 false
    4. 驗證記憶體使用下降

**驗收標準 (Phase 1)**:
- ✅ PluginAssemblyLoadContext 實現完整
- ✅ Assembly 隔離策略正確 (Framework Shared, Plugin Isolated)
- ✅ PluginVersionManager 管理多版本共存
- ✅ 記憶體管理策略實現 (WeakReference, GC)
- ✅ 記憶體監控 Metrics 配置完成
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 記憶體洩漏測試通過
- ✅ Code Review 通過

---

### Phase 2: Hot Reload Core Implementation (1 SP) ⏳ 待開始

#### Hot Reload Service
- [ ] **實作 PluginLoader.LoadPluginVersionAsync**: `src/AIAgentPlatform.Infrastructure/Services/PluginLoader.cs`
  - 輸入: `pluginId`, `version`
  - 邏輯:
    1. 檢查 Plugin 是否已載入: `_versionManager.GetLoadedPlugin(pluginId, version)`
    2. 如果已載入: 返回現有 LoadedPluginInfo
    3. 如果未載入:
       - 查詢資料庫: `IPluginVersionRepository.GetByPluginIdAndVersionAsync(pluginId, version)`
       - 驗證 Assembly 文件存在: `File.Exists(pluginVersion.AssemblyPath)`
       - 創建 PluginAssemblyLoadContext: `new PluginAssemblyLoadContext(pluginId, version, assemblyPath, logger)`
       - 載入 Assembly: `loadContext.LoadFromAssemblyPath(assemblyPath)`
       - 註冊到 PluginVersionManager: `_versionManager.RegisterLoadedPlugin(loadedInfo)`
       - 返回 LoadedPluginInfo
  - Error Handling: FileNotFoundException, BadImageFormatException
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 650-700

- [ ] **實作 PluginLoader.UnloadPluginVersionAsync**:
  - 輸入: `pluginId`, `version`
  - 邏輯:
    1. 檢查 Plugin 是否載入: `_versionManager.GetLoadedPlugin(pluginId, version)`
    2. 如果未載入: 返回 Success (冪等)
    3. 如果已載入:
       - 標記為 Inactive: `loadedInfo.IsActive = false`
       - 卸載 AssemblyLoadContext: `loadContext.Unload()`
       - 從 PluginVersionManager 移除: `_versionManager.UnregisterLoadedPlugin(pluginId, version)`
       - 強制 GC: `GC.Collect(); GC.WaitForPendingFinalizers(); GC.Collect();`
       - 檢查記憶體洩漏: `weakRef.IsAlive` → 如果 true, 記錄 Warning
       - 返回 Success
  - Error Handling: 記錄卸載失敗
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 650-700

#### Version Coordination
- [ ] **實作 SwitchPluginVersionAsync** (in PluginLoader):
  - 輸入: `pluginId`, `fromVersion`, `toVersion`
  - 邏輯:
    1. 驗證: fromVersion 是當前 Active Version
    2. 載入新版本: `await LoadPluginVersionAsync(pluginId, toVersion)`
    3. 更新 Active Version: `_versionManager.SetActiveVersion(pluginId, toVersion)`
    4. 卸載舊版本: `await UnloadPluginVersionAsync(pluginId, fromVersion)`
    5. 更新資料庫: `IPluginVersionRepository.UpdateIsCurrentVersionAsync(pluginId, toVersion)`
    6. 發布 Event: `PluginVersionSwitchedEvent`
  - Transaction: 使用 Unit of Work 確保一致性
  - Rollback: 如果步驟失敗, 回滾到 fromVersion
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 650-700

- [ ] **實作 CoexistenceAsync**: 多版本並存
  - 邏輯:
    - 允許多個版本同時載入 (例如: v1.0.0, v1.1.0 同時存在)
    - Agent 可以綁定到特定版本: `Agent.PluginVersion = "1.0.0"`
    - 新 Agent 自動使用 Active Version
  - 資料庫欄位: `agents.plugin_version` (可選, 若為 null 則使用 Active Version)

#### Rollback Mechanism
- [ ] **實作 RollbackToVersionAsync**:
  - 輸入: `pluginId`, `targetVersion`
  - 邏輯:
    1. 獲取當前 Active Version: `currentVersion = _versionManager.GetActiveVersion(pluginId)`
    2. 驗證 Target Version 存在: `IPluginVersionRepository.GetByPluginIdAndVersionAsync(pluginId, targetVersion)`
    3. 切換版本: `await SwitchPluginVersionAsync(pluginId, currentVersion, targetVersion)`
    4. 記錄回滾歷史: `PluginVersionHistoryRepository.AddRollbackRecordAsync`
  - Error Handling: Target Version 不存在 → 拋出 NotFoundException
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 650-700

- [ ] **創建 PluginVersionHistory Entity**: `src/AIAgentPlatform.Domain/Entities/PluginVersionHistory.cs`
  - Properties:
    - `Id`: Guid
    - `PluginId`: string
    - `FromVersion`: VersionNumber
    - `ToVersion`: VersionNumber
    - `ChangeType`: ChangeTypeEnum (Upgrade, Downgrade, Rollback)
    - `TriggeredBy`: string (UserId or "System")
    - `TriggeredAt`: DateTime
    - `IsSuccess`: bool
    - `ErrorMessage`: string?

- [ ] **EF Configuration**: `PluginVersionHistoryConfiguration.cs`
  - Table: `plugin_version_history`
  - Indexes: `idx_plugin_version_history_plugin_id`, `idx_plugin_version_history_triggered_at`

- [ ] **Migration**: `AddPluginVersionHistory`
  - 創建 `plugin_version_history` 表

#### Unit Tests (P0)
- [ ] **PluginLoader Hot Reload 測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/PluginLoaderTests.cs`
  - `LoadPluginVersionAsync_NewVersion_LoadsSuccessfully`
  - `LoadPluginVersionAsync_AlreadyLoaded_ReturnsExisting`
  - `UnloadPluginVersionAsync_LoadedVersion_UnloadsSuccessfully`
  - `UnloadPluginVersionAsync_NotLoaded_ReturnsSuccess`
  - `SwitchPluginVersionAsync_ValidVersions_SwitchesSuccessfully`
  - `SwitchPluginVersionAsync_InvalidFromVersion_ThrowsException`
  - Mock: IPluginVersionRepository, IPluginVersionManager
  - 測試覆蓋率: ≥85%

- [ ] **Rollback 測試**: `PluginLoaderTests.cs`
  - `RollbackToVersionAsync_ValidVersion_RollbacksSuccessfully`
  - `RollbackToVersionAsync_InvalidVersion_ThrowsException`
  - 驗證: PluginVersionHistory 記錄正確

#### Integration Tests (P0)
- [ ] **Hot Reload Integration Test**: `tests/AIAgentPlatform.IntegrationTests/Plugins/PluginHotReloadTests.cs`
  - `HotReload_CompleteFlow_Succeeds`
  - 步驟:
    1. 載入 TestPlugin v1.0.0
    2. 執行 Agent (使用 v1.0.0)
    3. 熱重載到 TestPlugin v1.1.0
    4. 執行 Agent (使用 v1.1.0)
    5. 驗證: 新版本功能正常
  - 驗證: 舊版本已卸載, 記憶體釋放

- [ ] **Coexistence Test**: `PluginHotReloadTests.cs`
  - `MultipleVersions_CanCoexist_Succeeds`
  - 步驟:
    1. 載入 v1.0.0 和 v1.1.0
    2. Agent A 綁定到 v1.0.0
    3. Agent B 綁定到 v1.1.0
    4. 同時執行兩個 Agent
    5. 驗證: 兩個版本獨立運作, 無衝突

**驗收標準 (Phase 2)**:
- ✅ Hot Reload 核心邏輯實現完整
- ✅ 版本切換機制正確 (LoadPluginVersionAsync, UnloadPluginVersionAsync, SwitchPluginVersionAsync)
- ✅ 多版本並存機制正常
- ✅ Rollback 機制實現並記錄歷史
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Integration Tests 通過 (Hot Reload + Coexistence)
- ✅ 記憶體管理正常, 無洩漏
- ✅ Code Review 通過

---

### Phase 3: API & Frontend Integration (0.5 SP) ⏳ 待開始

#### API Endpoints
- [ ] **POST /api/v1/plugin-versions/{id}/reload**: 熱重載 Plugin
  - Request: 無 Body (使用路由參數 `{id}`)
  - Response: `PluginVersionDto` (200 OK)
  - 邏輯:
    1. 查詢 PluginVersion: `IPluginVersionRepository.GetByIdAsync(id)`
    2. 調用 Hot Reload: `IPluginLoader.UnloadPluginVersionAsync(pluginId, version)` → `LoadPluginVersionAsync(pluginId, version)`
    3. 返回 Updated PluginVersionDto
  - Error Handling: 404 (Plugin 不存在), 500 (重載失敗)
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 700-750

- [ ] **POST /api/v1/plugin-versions/{id}/switch-version**: 切換版本
  - Request Body:
    ```json
    {
      "toVersion": "1.1.0"
    }
    ```
  - Response: `PluginVersionDto` (200 OK)
  - 邏輯:
    1. 查詢當前 Plugin: `IPluginVersionRepository.GetByIdAsync(id)`
    2. 獲取當前 Active Version: `IPluginLoader.GetActiveVersionAsync(pluginId)`
    3. 切換版本: `IPluginLoader.SwitchPluginVersionAsync(pluginId, fromVersion, toVersion)`
    4. 返回 New Active PluginVersionDto
  - Error Handling: 404 (Plugin 或 Version 不存在), 409 (版本衝突), 500
  - 參考: [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) Lines 700-750

- [ ] **POST /api/v1/plugin-versions/{id}/rollback**: 回滾版本
  - Request Body:
    ```json
    {
      "toVersion": "1.0.0"
    }
    ```
  - Response: `PluginVersionDto` (200 OK)
  - 邏輯: 同 switch-version, 但記錄為 Rollback
  - Error Handling: 同上

- [ ] **GET /api/v1/plugin-versions/{pluginId}/loaded-versions**: 查詢已載入版本
  - Response: `List<LoadedPluginInfoDto>` (200 OK)
  - 邏輯: `IPluginLoader.ListLoadedVersionsAsync(pluginId)`

#### Commands
- [ ] **創建 ReloadPluginCommand**: `src/AIAgentPlatform.Application/PluginVersions/Commands/ReloadPlugin/ReloadPluginCommand.cs`
  - Properties: PluginVersionId
  - MediatR: `IRequest<Result<PluginVersionDto>>`

- [ ] **創建 ReloadPluginCommandHandler**: `ReloadPluginCommandHandler.cs`
  - 邏輯: 調用 IPluginLoader.UnloadPluginVersionAsync + LoadPluginVersionAsync

- [ ] **創建 SwitchPluginVersionCommand**: `SwitchPluginVersion/SwitchPluginVersionCommand.cs`
  - Properties: PluginVersionId, ToVersion
  - MediatR: `IRequest<Result<PluginVersionDto>>`

- [ ] **創建 SwitchPluginVersionCommandHandler**: `SwitchPluginVersionCommandHandler.cs`
  - 邏輯: 調用 IPluginLoader.SwitchPluginVersionAsync

#### Frontend - React Components
- [ ] **創建 PluginReloadButton 組件**: `apps/web-app/src/components/plugins/PluginReloadButton.tsx`
  - Props: `pluginVersionId`, `onSuccess`, `onError`
  - UI: Button "Reload Plugin"
  - 點擊: 調用 API POST /api/v1/plugin-versions/{id}/reload
  - 載入狀態: CircularProgress
  - 成功/失敗通知: Snackbar
  - 參考: Material-UI Button + Dialog

- [ ] **創建 VersionSwitcher 組件**: `VersionSwitcher.tsx`
  - Props: `pluginId`, `currentVersion`, `availableVersions`, `onSuccess`
  - UI: Select Dropdown (版本列表)
  - 選擇版本: 顯示確認對話框 "Switch to version X.X.X?"
  - 確認: 調用 API POST /api/v1/plugin-versions/{id}/switch-version
  - Rollback 按鈕: 特殊標註 "Rollback to Previous Version"
  - 參考: Material-UI Select + Dialog

- [ ] **擴展 PluginCard 組件**: 添加 Hot Reload 功能
  - 顯示: Current Version, IsCurrentVersion Badge
  - 操作按鈕:
    - Reload Plugin Button (PluginReloadButton)
    - Switch Version Dropdown (VersionSwitcher)
  - 狀態: 顯示 "Reloading..." 或 "Switching..."

#### Frontend - React Hooks
- [ ] **創建 useReloadPlugin Hook**: `apps/web-app/src/hooks/useReloadPlugin.ts`
  - React Query Mutation: `reloadPlugin`
  - API: POST /api/v1/plugin-versions/{id}/reload
  - Error Handling: Toast 錯誤訊息

- [ ] **創建 useSwitchVersion Hook**: `useSwitchVersion.ts`
  - React Query Mutation: `switchVersion`
  - API: POST /api/v1/plugin-versions/{id}/switch-version
  - Optimistic Update: 立即更新 UI, 失敗時回滾

- [ ] **創建 useLoadedVersions Hook**: `useLoadedVersions.ts`
  - React Query: 查詢已載入版本列表
  - API: GET /api/v1/plugin-versions/{pluginId}/loaded-versions
  - Polling: 每 10 秒自動刷新 (可選)

#### Frontend - Services
- [ ] **擴展 pluginService.ts**: `apps/web-app/src/services/pluginService.ts`
  - 新增方法:
    - `reloadPlugin(pluginVersionId: string): Promise<PluginVersionDto>`
    - `switchVersion(pluginVersionId: string, toVersion: string): Promise<PluginVersionDto>`
    - `rollbackVersion(pluginVersionId: string, toVersion: string): Promise<PluginVersionDto>`
    - `getLoadedVersions(pluginId: string): Promise<LoadedPluginInfoDto[]>`

#### API Integration Tests (P0)
- [ ] **測試 POST /api/v1/plugin-versions/{id}/reload**: `tests/AIAgentPlatform.IntegrationTests/Plugins/PluginHotReloadApiTests.cs`
  - `ReloadPlugin_Should_Return200_When_Success`
  - 驗證: Response Status 200
  - 驗證: Plugin 已重新載入

- [ ] **測試 POST /api/v1/plugin-versions/{id}/switch-version**: `SwitchVersion_Should_Return200_When_Success`
  - 驗證: Active Version 已更新
  - 驗證: 舊版本已卸載

- [ ] **測試錯誤場景**:
  - `ReloadPlugin_Should_Return404_When_PluginNotFound`
  - `SwitchVersion_Should_Return404_When_VersionNotFound`
  - `SwitchVersion_Should_Return409_When_AlreadyActive`

**驗收標準 (Phase 3)**:
- ✅ 3 個 Hot Reload API 端點實現完整
- ✅ Commands/Handlers 實現
- ✅ Frontend 組件實現 (PluginReloadButton, VersionSwitcher)
- ✅ React Hooks 實現 (useReloadPlugin, useSwitchVersion)
- ✅ API Integration Tests 覆蓋率 ≥80%
- ✅ 所有測試通過
- ✅ UI 操作流暢, 無錯誤
- ✅ Code Review 通過

---

### Phase 4: Testing & Validation (0.5 SP) ⏳ 待開始

#### E2E Tests
- [ ] **Playwright: 完整 Hot Reload 流程**
  - Test File: `apps/web-app/e2e/plugin-hot-reload.spec.ts`
  - 步驟:
    1. 導航到 Plugin 管理頁面
    2. 選擇 TestPlugin v1.0.0
    3. 執行 Agent (使用 v1.0.0) → 驗證結果符合 v1.0.0 行為
    4. 點擊 "Reload Plugin" 按鈕 → 等待成功通知
    5. 再次執行 Agent → 驗證 Plugin 已重新載入
  - 驗證: E2E 流程順暢, 無錯誤

- [ ] **Playwright: 版本切換流程**
  - 步驟:
    1. 顯示 Plugin 版本列表 (v1.0.0, v1.1.0, v1.2.0)
    2. 當前 Active Version: v1.0.0
    3. 選擇 v1.1.0 → 點擊 "Switch Version"
    4. 確認對話框 → 點擊 "Confirm"
    5. 等待成功通知 → 驗證 Active Version 已更新為 v1.1.0
    6. 執行 Agent → 驗證使用 v1.1.0
  - 驗證: 版本切換正確

- [ ] **Playwright: 版本回滾流程**
  - 步驟:
    1. 當前 Active Version: v1.1.0
    2. 點擊 "Rollback to v1.0.0" 按鈕
    3. 確認對話框 (警告: "This will rollback to a previous version") → 點擊 "Confirm"
    4. 等待成功通知 → 驗證 Active Version 已回滾為 v1.0.0
    5. 檢查 Plugin Version History: 記錄存在, ChangeType = Rollback
  - 驗證: 回滾正確, 歷史記錄存在

#### Load Tests
- [ ] **JMeter: 熱重載性能測試**
  - Test Plan: `PluginHotReloadLoadTest.jmx`
  - 場景 1: 並發重載 10 個 Plugin
    - 10 個不同 Plugin, 同時觸發 Reload
    - 驗證: 所有 Reload 成功, 響應時間 < 5 秒
  - 場景 2: 頻繁切換版本 (100 次)
    - 單個 Plugin, 在 v1.0.0 ↔ v1.1.0 之間切換 100 次
    - 驗證: 無記憶體洩漏, 記憶體使用穩定
    - 驗證: 切換成功率 100%
  - 場景 3: 長時間運行測試 (1 小時)
    - 持續載入/卸載 Plugin
    - 驗證: 系統穩定, 無 OutOfMemoryException
    - 驗證: 記憶體使用 < 2GB

#### Memory Leak Tests
- [ ] **記憶體洩漏測試**: `tests/AIAgentPlatform.IntegrationTests/Plugins/PluginMemoryLeakTests.cs`
  - `HotReload_100Times_NoMemoryLeak`
  - 步驟:
    1. 記錄初始記憶體使用: `initialMemory = GC.GetTotalMemory(false);`
    2. 循環 100 次:
       - 載入 TestPlugin
       - 卸載 TestPlugin
       - 強制 GC
    3. 記錄最終記憶體使用: `finalMemory = GC.GetTotalMemory(false);`
    4. 驗證: `finalMemory - initialMemory < 50MB` (允許適當增長)
  - 驗證: 無顯著記憶體增長

- [ ] **WeakReference 驗證測試**:
  - `UnloadPlugin_ReleasesAssemblyLoadContext`
  - 步驟:
    1. 載入 Plugin → 創建 WeakReference 到 AssemblyLoadContext
    2. 卸載 Plugin → 強制 GC
    3. 驗證: `weakRef.IsAlive == false`
  - 驗證: AssemblyLoadContext 已被 GC 回收

#### Stability Tests
- [ ] **穩定性測試**: 頻繁重載測試
  - `FrequentReload_1000Times_RemainsStable`
  - 步驟: 重載 Plugin 1000 次
  - 驗證: 無 Exception, 系統穩定

- [ ] **併發測試**: 多 Plugin 同時重載
  - `ConcurrentReload_10Plugins_AllSucceed`
  - 步驟: 10 個 Plugin 並發重載
  - 驗證: 所有重載成功, 無競態條件

#### Documentation
- [ ] **Hot Reload 使用指南**: `docs/user-guides/plugin-hot-reload-guide.md`
  - 如何觸發 Hot Reload
  - 如何切換版本
  - 如何回滾版本
  - 注意事項與最佳實踐
  - 故障排除

- [ ] **API 文檔更新**: Swagger
  - Hot Reload 端點範例
  - 錯誤碼說明

**驗收標準 (Phase 4)**:
- ✅ E2E 測試通過 (Hot Reload + 版本切換 + 回滾)
- ✅ Load Tests 通過 (並發重載, 頻繁切換, 長時間運行)
- ✅ 記憶體洩漏測試通過, 無顯著記憶體增長
- ✅ 穩定性測試通過 (1000 次重載, 無 Exception)
- ✅ 併發測試通過 (10 個 Plugin 並發重載)
- ✅ Hot Reload 使用指南文檔完整
- ✅ API 文檔更新
- ✅ Code Review 通過

---

## 測試

### 單元測試 (P0)
- [ ] **Backend 單元測試**: 所有 US 7.1, 7.2, 2.2
  - Domain Entity Tests (~20 tests)
  - Value Object Tests (~15 tests)
  - Application Service Tests (Commands + Queries) (~40 tests)
  - Validation Tests (~20 tests)
  - Repository Tests (~15 tests)
  - **測試覆蓋率目標**: ≥85%

- [ ] **執行所有單元測試**:
  - `dotnet test --filter "Category=Unit"`
  - 驗證: 所有測試通過, 0 失敗

- [ ] **Code Coverage 報告**:
  - `dotnet test --collect:"XPlat Code Coverage"`
  - 使用 ReportGenerator 生成報告
  - 驗證: Coverage ≥85%

### 整合測試 (P0)
- [ ] **API 整合測試**: 所有 US 7.1, 7.2, 2.2
  - Persona API Tests (~15 tests)
  - Prompt Generation API Tests (~10 tests)
  - Plugin Hot Reload API Tests (~8 tests)
  - Database Operation Tests (~10 tests)
  - **測試覆蓋率目標**: ≥80%

- [ ] **執行所有整合測試**:
  - `dotnet test --filter "Category=Integration"`
  - 驗證: 所有測試通過, 0 失敗

### E2E 測試 (P1)
- [ ] **Playwright E2E 測試**: 所有 US 7.1, 7.2, 2.2
  - US 7.1: Persona CRUD 流程 (~3 scenarios)
  - US 7.2: Prompt Generation 流程 (~2 scenarios)
  - US 2.2: Plugin Hot Reload 流程 (~3 scenarios)
  - **目標**: 8 scenarios 全部通過

- [ ] **執行所有 E2E 測試**:
  - `cd apps/web-app && npm run test:e2e`
  - 驗證: 所有 scenarios 通過, 0 失敗

---

## 文檔

### API 文檔 (P0)
- [ ] **Swagger/OpenAPI 文檔完整**:
  - 所有新端點已註解 (US 7.1, 7.2, 2.2)
  - Request/Response 範例完整
  - 錯誤碼說明清晰
  - 驗證: Swagger UI 正常顯示

### 技術文檔 (P1)
- [ ] **Persona Framework 技術文檔**: `docs/technical-implementation/01-backend-net9/03-persona-framework.md`
  - Persona 架構設計
  - Liquid Template Engine 整合
  - 配置系統說明

- [ ] **Prompt Engineering 文檔**: `docs/technical-implementation/01-backend-net9/09-prompt-engineering.md`
  - 6-Step Algorithm 詳細說明
  - Token Management 策略
  - Cache 策略

- [ ] **Plugin Hot Reload 文檔**: `docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md` (更新)
  - AssemblyLoadContext 設計
  - 記憶體管理策略
  - Hot Reload 流程圖

### 使用者指南 (P1)
- [ ] **Persona Template 使用指南**: `docs/user-guides/persona-templates-guide.md`
  - 10 種模板使用場景
  - 如何自定義 Persona
  - 最佳實踐

- [ ] **Prompt Generation 使用指南**: `docs/user-guides/prompt-generation-guide.md`
  - 如何生成 Prompt
  - Liquid Template 語法說明
  - 範例與最佳實踐

- [ ] **Plugin Hot Reload 使用指南**: `docs/user-guides/plugin-hot-reload-guide.md`
  - 如何觸發 Hot Reload
  - 如何切換版本
  - 故障排除

### Database Schema 文檔 (P0)
- [ ] **更新 Database Schema 文檔**: `docs/database/database-schema.md`
  - `personas`, `persona_templates`, `prompt_templates` 表結構
  - `plugin_version_history` 表結構
  - 索引說明
  - 外鍵約束

### Sprint 執行文檔 (P0)
- [ ] **完成 SPRINT-4-5-DEV-LOG.md**: 每日開發日誌
- [ ] **完成 SPRINT-4-6-ISSUES.md**: 問題追蹤與解決方案
- [ ] **完成 SPRINT-4-7-RETROSPECTIVE.md**: Sprint 回顧 (Sprint 結束後)

---

## 部署與DevOps

### Development 環境部署 (P0)
- [ ] **部署 Backend API**:
  - `dotnet publish -c Release`
  - 部署到 Dev Server
  - 驗證: Health Check 通過

- [ ] **部署 Frontend**:
  - `cd apps/web-app && npm run build`
  - 部署到 Dev Server
  - 驗證: 可訪問

- [ ] **Database Migration**:
  - 執行所有 Migrations: `dotnet ef database update`
  - 驗證: 新表創建成功 (personas, persona_templates, prompt_templates, plugin_version_history)

- [ ] **Redis 配置**:
  - 啟動 Redis Server
  - 驗證: 連接正常

### Smoke 測試 (P0)
- [ ] **API Health Check**:
  - GET /health → 200 OK
  - 驗證: 所有依賴服務正常 (Database, Redis)

- [ ] **Frontend 可訪問**:
  - 打開瀏覽器 → Dev URL
  - 驗證: 頁面正常載入, 無 JS 錯誤

- [ ] **關鍵功能測試**:
  - 創建 Persona → 成功
  - 生成 Prompt → 成功
  - Hot Reload Plugin → 成功

### Git 分支管理 (P0)
- [ ] **合併到 main 分支**:
  - PR: feature/us-7.1-persona-template → main
  - PR: feature/us-7.2-prompt-engineering → main
  - PR: feature/us-2.2-plugin-hot-reload → main
  - Code Review 通過
  - CI/CD Pipeline 通過
  - 所有測試通過

---

## ✅ Definition of Done 驗證

### 功能完成度
- [ ] US 7.1 所有驗收標準通過
- [ ] US 7.2 所有驗收標準通過
- [ ] US 2.2 所有驗收標準通過
- [ ] 核心功能可 Demo

### 技術質量
- [ ] 單元測試覆蓋率 ≥85%
- [ ] Code Review 完成並 approved
- [ ] 無 Critical/High severity bugs
- [ ] API 文檔完整

### 性能標準
- [ ] Persona API: P95 響應時間 <300ms
- [ ] Prompt Generation: P95 響應時間 <200ms
- [ ] Plugin Hot Reload: 重載時間 <5s
- [ ] 錯誤率 <1%

### 部署就緒
- [ ] 代碼合併到 main 分支
- [ ] 部署到 Dev 環境成功
- [ ] Smoke 測試通過
- [ ] Sprint 文檔歸檔完成

---

## 參考文獻索引

本檢查清單的驗收標準基於以下文檔，按類別組織以便快速定位技術細節與驗收要求：

### Planning 文檔（優先查閱）
> 📌 **重要**：優先查閱此區域文檔，提供更全面的項目背景、架構設計與技術棧資訊

- [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Sprint 4 在 MVP 中的範圍與邊界定義
- [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 4 詳細分配、Story Points 估算與依賴關係
- [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - 整體開發策略與最佳實踐
- [Architecture Evolution Roadmap](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進計劃與技術債管理
- [Dependency Matrix](../../1-planning/DEPENDENCY-MATRIX.md) - Sprint 間依賴關係與風險評估
- [Risk Register](../../1-planning/RISK-REGISTER.md) - 風險登記與緩解措施
- [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 關鍵技術決策記錄

### Sprint 文檔
- [SPRINT-4-1-OVERVIEW.md](./SPRINT-4-1-OVERVIEW.md) - Sprint 4 概覽與整體進度
- [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md) - Sprint 4 詳細執行計劃
- [SPRINT-4-3-CONTEXT.md](./SPRINT-4-3-CONTEXT.md) - Sprint 4 快速上下文與技術摘要
- [SPRINT-4-5-DEV-LOG.md](./SPRINT-4-5-DEV-LOG.md) - Sprint 4 開發日誌與執行記錄
- [SPRINT-4-6-ISSUES.md](./SPRINT-4-6-ISSUES.md) - Sprint 4 問題追蹤與解決方案

### User Story 規格文檔
- [Module 07: Persona Framework](../../../docs/user-stories/modules/module-07-persona-framework.md) - US 7.1-7.2 完整規格
- [Module 02: Plugin System](../../../docs/user-stories/modules/module-02-plugin-system.md) - US 2.2 完整規格

### ADR（架構決策記錄）
- [ADR-012: Persona Framework Architecture](../../../docs/architecture/adr/ADR-012-persona-framework-architecture.md) - Persona 架構決策
- [ADR-013: Liquid Template Engine Selection](../../../docs/architecture/adr/ADR-013-liquid-template-engine-selection.md) - Liquid 選擇決策
- [ADR-014: Plugin Hot Reload Strategy](../../../docs/architecture/adr/ADR-014-plugin-hot-reload-strategy.md) - Hot Reload 策略

### 後端架構文檔
- [Backend-Architecture.md](../../../docs/architecture/Backend-Architecture.md) - 後端分層架構設計（Clean Architecture + DDD + CQRS）
- [Domain-Model.md](../../../docs/architecture/Domain-Model.md) - 領域模型與實體關係
- [CQRS-Pattern.md](../../../docs/architecture/CQRS-Pattern.md) - CQRS 模式實作指南

### 測試文檔
- [Testing-Strategy.md](../../../docs/testing/Testing-Strategy.md) - 整體測試策略（單元/整合/E2E）
- [Unit-Testing-Guidelines.md](../../../docs/testing/Unit-Testing-Guidelines.md) - 單元測試最佳實踐
- [Integration-Testing-Guidelines.md](../../../docs/testing/Integration-Testing-Guidelines.md) - 整合測試指南
- [E2E-Testing-with-Playwright.md](../../../docs/testing/E2E-Testing-with-Playwright.md) - Playwright E2E 測試指南

---

## 📋 版本歷史

| 版本 | 日期 | 變更摘要 | 變更者 |
|------|------|---------|--------|
| v2.1 | 2025-11-13 | 重構為按 US 組織：<br/>• 從「按時間組織 (Week/Day)」改為「按 US 組織 (Phase)」<br/>• 與 Sprint 1-2 格式統一<br/>• 新增完整 Phase 劃分 (US 7.1: 5 Phases, US 7.2: 4 Phases, US 2.2: 4 Phases)<br/>• 詳細任務清單擴充 (~207 任務項目)<br/>• 完整參考文獻索引（50+ 文檔）<br/>• 標準化 Header (8 欄位) | Claude Code |
| v1.0 | 2025-11-13 | 初始版本：<br/>• 按時間組織 (Week 10-12, Day 1-15)<br/>• 45 個檢查項目<br/>• 基本驗收標準 | Sprint 4 開發團隊 |

---

**維護說明**: 本文檔為 Sprint 4 的任務檢查清單，在 Sprint 進行中持續更新。請參考 [SPRINT-4-5-DEV-LOG.md](./SPRINT-4-5-DEV-LOG.md) 查看詳細開發日誌。

**Sprint 狀態**: 📋 計劃階段 (Planned)
**預計執行日期**: 2025-12-16 ~ 2026-01-05
