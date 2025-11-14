# SPRINT-4-3-CONTEXT.md - Sprint 4 開發上下文

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

- [使用說明](#-使用說明-how-to-use-this-document)
- [一、核心技術參考層 (Reference Layer)](#一核心技術參考層-reference-layer)
  - [1.1 Module & Epic 引用](#11-module--epic-引用)
  - [1.2 架構設計文檔](#12-架構設計文檔)
  - [1.3 技術決策記錄 (ADR)](#13-技術決策記錄-adr)
- [二、US 7.1: Persona 模板配置 - 詳細技術上下文](#二us-71-persona-模板配置---詳細技術上下文)
  - [2.1 MVP 範圍定義與技術決策](#21-mvp-範圍定義與技術決策)
  - [2.2 Domain Layer 設計細節](#22-domain-layer-設計細節)
  - [2.3 Persona 配置 Schema 設計](#23-persona-配置-schema-設計)
  - [2.4 CQRS 實施細節](#24-cqrs-實施細節)
  - [2.5 API 層實施細節](#25-api-層實施細節)
- [三、US 7.2: Persona-Driven Prompt Engineering - 詳細技術上下文](#三us-72-persona-driven-prompt-engineering---詳細技術上下文)
  - [3.1 System Prompt 生成策略](#31-system-prompt-生成策略)
  - [3.2 Prompt Template 引擎設計](#32-prompt-template-引擎設計)
  - [3.3 Persona 行為注入機制](#33-persona-行為注入機制)
  - [3.4 Token 效率優化](#34-token-效率優化)
- [四、US 2.2: Plugin 熱重載機制 - 詳細技術上下文](#四us-22-plugin-熱重載機制---詳細技術上下文)
  - [4.1 熱重載架構設計](#41-熱重載架構設計)
  - [4.2 FileSystemWatcher 實施](#42-filesystemwatcher-實施)
  - [4.3 版本協調機制](#43-版本協調機制)
  - [4.4 回退與錯誤處理](#44-回退與錯誤處理)
- [五、完整文檔引用清單 (按功能分類)](#五完整文檔引用清單-按功能分類)
- [使用指南](#-使用指南)
- [更新日誌](#-更新日誌)

---

## 📖 使用說明 (How to Use This Document)

**本文檔定位**: "Just-in-Time" 技術參考手冊

**適用場景**:
- ✅ 開發 US 7.1 時，需要查詢 Persona 配置 Schema 設計 → 第二章
- ✅ 開發 US 7.2 時，需要查詢 Prompt 生成策略 → 第三章
- ✅ 開發 US 2.2 時，需要查詢 Plugin 熱重載實施 → 第四章
- ✅ 需要快速找到架構設計文檔 → 第一章 Reference Layer
- ✅ 需要理解技術決策背景 → 各章節的「關鍵技術決策」部分

**不適用場景**:
- ❌ 尋找 Sprint 整體進度與成果 → 使用 [SPRINT-4-OVERVIEW.md](./SPRINT-4-OVERVIEW.md)
- ❌ 尋找詳細開發計劃與任務清單 → 使用 [SPRINT-4-PLAN.md](./SPRINT-4-PLAN.md)

---

## 一、核心技術參考層 (Reference Layer)

### 1.1 Module & Epic 引用

**User Story 完整規格**:
- 📖 [Module 07: Persona Framework](../../docs/user-stories/modules/module-07-persona-framework.md) - Persona 系統完整規格
  - **Section**: [US 7.1 - Persona Template Configuration](../../docs/user-stories/modules/module-07-persona-framework.md#us-71---persona-template-configuration) - 驗收標準與技術要求
  - **Section**: [US 7.2 - Persona-Driven Prompt Engineering](../../docs/user-stories/modules/module-07-persona-framework.md#us-72---persona-driven-prompt-engineering) - Prompt 生成規格
  - **關鍵內容**: 10 種預設 Persona 模板、JSON/YAML Schema、Prompt Engineering 策略

- 📖 [Module 02: Plugin System](../../docs/user-stories/modules/module-02-plugin-system.md) - Plugin 系統完整規格
  - **Section**: [US 2.2 - Plugin Hot Reload Mechanism](../../docs/user-stories/modules/module-02-plugin-system.md#us-22---plugin-hot-reload-mechanism) - 熱重載實施規格
  - **關鍵內容**: FileSystemWatcher 設計、版本協調機制、零停機更新策略

**Epic 文檔**:
- 📖 [Epic 07: Persona Framework](../../docs/user-stories/epics/epic-07.md) - Persona 功能完整策略
  - **用途**: 理解 Persona Framework 在整體系統中的定位
  - **關鍵內容**: BDD 場景、驗收標準、依賴關係

- 📖 [Epic 02: Plugin Ecosystem](../../docs/user-stories/epics/epic-02.md) - Plugin 生態系統策略
  - **用途**: 理解 Plugin 熱重載在 Plugin 系統演進中的角色
  - **關鍵內容**: Phase 1 基礎架構 → Phase 2 熱重載 → Phase 3 Market

### 1.2 架構設計文檔

**核心架構決策記錄 (ADR)**:

**1. Persona Configuration Format** - [ADR-012](../../docs/architecture/adr/ADR-012-persona-configuration-format.md)
- **決策**: 採用 JSON/YAML 雙格式支援
- **關鍵原則**: JSON 用於程式解析與 Schema 驗證，YAML 用於人工編輯與可讀性
- **Section**: [JSON Schema Definition](../../docs/architecture/adr/ADR-012-persona-configuration-format.md#json-schema-definition) - 完整 JSON Schema 規格
- **Section**: [YAML Conversion Strategy](../../docs/architecture/adr/ADR-012-persona-configuration-format.md#yaml-conversion-strategy) - YAML ↔ JSON 轉換邏輯
- **實施影響**: US 7.1 的配置驗證與存儲策略基於此決策

**2. Plugin Isolation Strategy** - [ADR-013](../../docs/architecture/adr/ADR-013-plugin-isolation-strategy.md)
- **決策**: 使用 AppDomain 進行 Plugin 隔離
- **關鍵設計**: 完整記憶體隔離、動態載入/卸載、Permission-based 安全控制
- **Section**: [AppDomain Configuration](../../docs/architecture/adr/ADR-013-plugin-isolation-strategy.md#appdomain-configuration) - AppDomain 配置詳解
- **Section**: [Unload Strategy](../../docs/architecture/adr/ADR-013-plugin-isolation-strategy.md#unload-strategy) - 記憶體回收機制
- **實施影響**: US 2.2 的熱重載機制基於 AppDomain 隔離實施

**3. Prompt Engineering Strategy** - [ADR-014](../../docs/architecture/adr/ADR-014-prompt-engineering-strategy.md)
- **決策**: 使用模板化 + 動態組合的 System Prompt 生成策略
- **關鍵設計**: Base Template + Persona Injection + Context Awareness
- **Section**: [Template Structure](../../docs/architecture/adr/ADR-014-prompt-engineering-strategy.md#template-structure) - Prompt Template 結構設計
- **Section**: [Token Optimization](../../docs/architecture/adr/ADR-014-prompt-engineering-strategy.md#token-optimization) - Token 效率優化策略
- **實施影響**: US 7.2 的 Prompt 生成服務基於此架構實施

**4. CQRS Pattern** - [ADR-002](../../docs/architecture/adr/ADR-002-cqrs-pattern.md)
- **決策**: 使用 MediatR 實施命令查詢職責分離
- **關鍵設計**: Commands (CreatePersona, UpdatePersona) vs Queries (GetPersona, ListPersonas)
- **Section**: [MediatR Integration](../../docs/architecture/adr/ADR-002-cqrs-pattern.md#mediatr-integration) - MediatR 配置與使用
- **Section**: [Command Handlers](../../docs/architecture/adr/ADR-002-cqrs-pattern.md#command-handlers) - Command Handler 實作模式
- **實施影響**: US 7.1-7.2 所有業務邏輯透過 CQRS 實施

**完整架構設計文檔**:
- 🏗️ [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md) - 系統架構總覽
  - **Section**: [Persona Framework Architecture](../../docs/architecture/Architecture-Design-Document.md#persona-framework-architecture) - Persona 系統架構圖
  - **Section**: [Plugin Hot Reload Architecture](../../docs/architecture/Architecture-Design-Document.md#plugin-hot-reload-architecture) - 熱重載架構設計
  - **Section**: [Domain Model](../../docs/architecture/Architecture-Design-Document.md#domain-model) - Persona Entity 設計

### 1.3 API 設計引用

**API 設計規範**:
- 📄 [API Design Specification](../../docs/api/api-design.md) - RESTful API 完整設計規範
  - **Section**: [Persona CRUD Endpoints](../../docs/api/api-design.md#persona-endpoints) - Persona API 端點詳細設計
    - `POST /api/v1/personas` - 創建 Persona (US 7.1)
    - `GET /api/v1/personas` - 查詢 Persona 列表 (US 7.1)
    - `GET /api/v1/personas/{id}` - 獲取 Persona 詳情 (US 7.1)
    - `PUT /api/v1/personas/{id}` - 更新 Persona (US 7.1)
    - `DELETE /api/v1/personas/{id}` - 刪除 Persona (US 7.1)
    - `POST /api/v1/personas/{id}/generate-prompt` - 生成 System Prompt (US 7.2)

  - **Section**: [Plugin Management Endpoints](../../docs/api/api-design.md#plugin-endpoints) - Plugin API 端點設計
    - `POST /api/v1/plugins/{id}/reload` - 熱重載 Plugin (US 2.2)
    - `POST /api/v1/plugins/{id}/versions/{version}/activate` - 激活特定版本 (US 2.2)
    - `POST /api/v1/plugins/{id}/rollback` - 版本回退 (US 2.2)

  - **Section**: [Request/Response Format](../../docs/api/api-design.md#request-response-format) - 標準格式定義
  - **Section**: [Error Handling](../../docs/api/api-design.md#error-handling) - 統一錯誤處理策略
  - **Section**: [Validation Rules](../../docs/api/api-design.md#validation-rules) - API 驗證規則

**Database Schema 設計**:
- 🗄️ [Database Schema Design](../../docs/api/database-schema.md) - 完整資料庫 Schema 設計
  - **Section**: [Personas Table](../../docs/api/database-schema.md#personas-table) - personas 表結構定義
  - **Section**: [PersonaTemplates Table](../../docs/api/database-schema.md#persona-templates-table) - persona_templates 表結構
  - **Section**: [PluginVersions Table](../../docs/api/database-schema.md#plugin-versions-table) - plugin_versions 表結構 (US 2.2)
  - **Section**: [Indexing Strategy](../../docs/api/database-schema.md#indexing-strategy) - 索引設計策略
  - **Section**: [JSONB Usage](../../docs/api/database-schema.md#jsonb-usage) - JSONB 欄位設計 (Persona Configuration)

### 1.4 UI/UX 設計引用

**線框圖設計 (Low-Fidelity Wireframes)**:
- 🎨 [Persona Builder Wireframe](../../docs/ux-design/wireframes/low-fidelity/10-persona-builder.md) - Persona 建構器設計 (US 7.1)
  - **Section**: [Desktop Layout](../../docs/ux-design/wireframes/low-fidelity/10-persona-builder.md#desktop-layout) - 桌面版佈局
  - **Section**: [Configuration Form](../../docs/ux-design/wireframes/low-fidelity/10-persona-builder.md#configuration-form) - 配置表單設計
  - **Section**: [Template Selector](../../docs/ux-design/wireframes/low-fidelity/10-persona-builder.md#template-selector) - 模板選擇器
  - **Section**: [Preview Panel](../../docs/ux-design/wireframes/low-fidelity/10-persona-builder.md#preview-panel) - 即時預覽面板

- 🎨 [Persona Template Gallery Wireframe](../../docs/ux-design/wireframes/low-fidelity/11-persona-template-gallery.md) - Persona 模板庫設計 (US 7.1)
  - **Section**: [Gallery Layout](../../docs/ux-design/wireframes/low-fidelity/11-persona-template-gallery.md#gallery-layout) - 模板庫佈局
  - **Section**: [Template Card](../../docs/ux-design/wireframes/low-fidelity/11-persona-template-gallery.md#template-card) - 模板卡片設計
  - **Section**: [Search & Filter](../../docs/ux-design/wireframes/low-fidelity/11-persona-template-gallery.md#search-filter) - 搜尋篩選功能

**Design System 規範**:
- 🎨 [Design System](../../docs/ux-design/design-system/README.md) - Material-UI 設計系統規範
  - **Section**: [Design Tokens](../../docs/ux-design/design-system/design-tokens.md) - 設計令牌 (顏色、字體、間距)
  - **Section**: [Component Library](../../docs/ux-design/design-system/component-library.md) - Material-UI 組件使用規範
  - **Section**: [Form Components](../../docs/ux-design/design-system/form-components.md) - Persona 配置表單組件

### 1.5 測試策略引用

**測試策略文檔**:
- 🧪 [Testing Strategy](../../docs/testing/testing-strategy.md) - 完整測試策略
  - **Section**: [Coverage Requirements](../../docs/testing/testing-strategy.md#coverage-requirements) - 覆蓋率要求 (≥80%)
  - **Section**: [Testing Pyramid](../../docs/testing/testing-strategy.md#testing-pyramid) - 測試金字塔
  - **Section**: [Persona Testing](../../docs/testing/testing-strategy.md#persona-testing) - Persona 系統測試策略
  - **Section**: [Plugin Hot Reload Testing](../../docs/testing/testing-strategy.md#plugin-hot-reload-testing) - 熱重載測試策略

**詳細測試指南**:
- 🧪 [Unit Testing Guidelines](../../docs/testing/unit-testing-guidelines.md) - 單元測試規範
  - **Section**: [Testing Tools](../../docs/testing/unit-testing-guidelines.md#testing-tools) - xUnit, Moq, FluentAssertions
  - **Section**: [Persona Domain Testing](../../docs/testing/unit-testing-guidelines.md#persona-domain-testing) - Persona Entity 測試
  - **Section**: [Prompt Generation Testing](../../docs/testing/unit-testing-guidelines.md#prompt-generation-testing) - Prompt 生成服務測試

- 🧪 [Integration Testing Guidelines](../../docs/testing/integration-testing-guidelines.md) - 集成測試規範
  - **Section**: [API Testing](../../docs/testing/integration-testing-guidelines.md#api-testing) - Persona API 測試策略
  - **Section**: [Hot Reload Testing](../../docs/testing/integration-testing-guidelines.md#hot-reload-testing) - Plugin 熱重載集成測試

---

## 二、US 7.1: Persona 模板配置 - 詳細技術上下文

### 2.1 MVP 範圍定義與技術決策

**完整 User Story 規格**: [US 7.1 - Persona Template Configuration](../../docs/user-stories/modules/module-07-persona-framework.md#us-71)

**MVP 範圍邊界**:

✅ **本 Sprint 必須實現 (P0)**:
1. **10 種預設 Persona 模板** - 預先配置的專業角色模板
   - 客服助理 (Customer Service Assistant)
   - 資料分析師 (Data Analyst)
   - 技術支援 (Technical Support)
   - 銷售顧問 (Sales Consultant)
   - 內容創作者 (Content Creator)
   - 專案經理 (Project Manager)
   - 程式開發助手 (Coding Assistant)
   - 教育導師 (Educational Tutor)
   - 醫療諮詢 (Health Consultant)
   - 法律顧問 (Legal Advisor)
   - **參考**: [Persona Templates Design](../../docs/user-stories/modules/module-07-persona-framework.md#persona-templates)

2. **Persona Configuration Schema (JSON/YAML)**
   - JSON Schema 驗證 (Draft-07)
   - YAML ↔ JSON 雙向轉換
   - 配置欄位: Name, Description, CommunicationStyle, Expertise, Constraints
   - **參考**: [Persona Configuration Schema](../../docs/technical-implementation/01-backend-net9/13-persona-configuration-schema.md)

3. **Persona CRUD API**
   - `POST /api/v1/personas` - 創建 Persona
   - `GET /api/v1/personas` - 查詢 Persona 列表
   - `GET /api/v1/personas/{id}` - 獲取 Persona 詳情
   - `PUT /api/v1/personas/{id}` - 更新 Persona
   - `DELETE /api/v1/personas/{id}` - 刪除 Persona (軟刪除)
   - **參考**: [Persona API Design](../../docs/api/api-design.md#persona-endpoints)

4. **Persona Template 管理 API**
   - `GET /api/v1/persona-templates` - 獲取預設模板列表
   - `GET /api/v1/persona-templates/{id}` - 獲取模板詳情
   - `POST /api/v1/personas/from-template/{templateId}` - 從模板創建 Persona
   - **參考**: [Template Management API](../../docs/api/api-design.md#persona-template-endpoints)

❌ **明確排除 Phase 2 功能 (延後到 Sprint 5-6)**:
- 自定義 Persona 模板 (US 7.3) - Sprint 5 實現
- Persona 版本管理 (US 7.4) - Sprint 6 實現
- Persona A/B Testing (US 7.5) - Sprint 6 實現
- Multi-language Persona (US 7.6) - Phase 2 實現

**關鍵技術決策**:

**決策 1: Persona 配置存儲格式**
- **選擇**: PostgreSQL JSONB 欄位存儲完整配置
- **理由**: 靈活的 Schema 演進、高效查詢能力、原生 JSON 支援
- **替代方案**: 關聯式欄位 (Schema 固化), NoSQL (增加系統複雜度)
- **參考**: [ADR-012: Persona Configuration Format](../../docs/architecture/adr/ADR-012-persona-configuration-format.md)

**決策 2: Persona Template 管理方式**
- **選擇**: Database-driven templates (persona_templates 表)
- **理由**: 動態管理、版本控制、多語言支援 (Phase 2)
- **替代方案**: Code-based templates (缺乏彈性), File-based (管理困難)
- **參考**: [Template Management Strategy](../../docs/technical-implementation/01-backend-net9/13-persona-configuration-schema.md#template-management)

### 2.2 Domain Layer 設計細節

**Domain Layer 完整文檔**: [Persona Domain Model](../../docs/technical-implementation/01-backend-net9/07-domain-layer.md#persona-entity)

**Persona Entity 設計**:

```csharp
// Persona Entity - Domain Layer
public class Persona : BaseEntity
{
    public Guid PersonaId { get; private set; }
    public string Name { get; private set; }
    public string Description { get; private set; }
    public PersonaConfiguration Configuration { get; private set; }
    public Guid? TemplateId { get; private set; }
    public PersonaTemplate Template { get; private set; }
    public PersonaStatus Status { get; private set; }
    public DateTime CreatedAt { get; private set; }
    public DateTime? UpdatedAt { get; private set; }

    // Factory Method
    public static Persona Create(
        string name,
        string description,
        PersonaConfiguration configuration,
        Guid? templateId = null)
    {
        // Domain validation logic
        if (string.IsNullOrWhiteSpace(name))
            throw new DomainException("Persona name cannot be empty");
        if (name.Length < 3 || name.Length > 100)
            throw new DomainException("Persona name must be 3-100 characters");
        if (configuration == null)
            throw new DomainException("Persona configuration is required");

        return new Persona
        {
            PersonaId = Guid.NewGuid(),
            Name = name,
            Description = description,
            Configuration = configuration,
            TemplateId = templateId,
            Status = PersonaStatus.Draft,
            CreatedAt = DateTime.UtcNow
        };
    }

    // Business Logic Methods
    public void Activate()
    {
        if (Status == PersonaStatus.Archived)
            throw new DomainException("Cannot activate archived persona. Restore first.");
        Status = PersonaStatus.Active;
        UpdatedAt = DateTime.UtcNow;
    }

    public void Deactivate()
    {
        if (Status == PersonaStatus.Draft)
            throw new DomainException("Cannot deactivate a draft persona");
        Status = PersonaStatus.Inactive;
        UpdatedAt = DateTime.UtcNow;
    }

    public void UpdateConfiguration(PersonaConfiguration newConfiguration)
    {
        if (newConfiguration == null)
            throw new DomainException("Persona configuration cannot be null");
        Configuration = newConfiguration;
        UpdatedAt = DateTime.UtcNow;
    }

    public void Archive()
    {
        Status = PersonaStatus.Archived;
        UpdatedAt = DateTime.UtcNow;
    }
}
```

**PersonaConfiguration Value Object**:

```csharp
// PersonaConfiguration Value Object - Domain Layer
public class PersonaConfiguration : ValueObject
{
    public CommunicationStyle CommunicationStyle { get; private set; }
    public List<string> Expertise { get; private set; }
    public ResponseFormat ResponseFormat { get; private set; }
    public Tone Tone { get; private set; }
    public List<string> Constraints { get; private set; }
    public Dictionary<string, string> CustomAttributes { get; private set; }

    public static PersonaConfiguration Create(
        CommunicationStyle communicationStyle,
        List<string> expertise,
        ResponseFormat responseFormat,
        Tone tone,
        List<string> constraints = null,
        Dictionary<string, string> customAttributes = null)
    {
        if (expertise == null || expertise.Count == 0)
            throw new DomainException("At least one expertise area is required");

        return new PersonaConfiguration
        {
            CommunicationStyle = communicationStyle,
            Expertise = expertise,
            ResponseFormat = responseFormat,
            Tone = tone,
            Constraints = constraints ?? new List<string>(),
            CustomAttributes = customAttributes ?? new Dictionary<string, string>()
        };
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return CommunicationStyle;
        yield return ResponseFormat;
        yield return Tone;
        yield return string.Join(",", Expertise.OrderBy(e => e));
        yield return string.Join(",", Constraints.OrderBy(c => c));
    }
}

// CommunicationStyle Value Object
public class CommunicationStyle : ValueObject
{
    public Formality Formality { get; private set; }
    public Verbosity Verbosity { get; private set; }
    public EmojiUsage EmojiUsage { get; private set; }

    public static CommunicationStyle Create(
        Formality formality,
        Verbosity verbosity,
        EmojiUsage emojiUsage)
    {
        return new CommunicationStyle
        {
            Formality = formality,
            Verbosity = verbosity,
            EmojiUsage = emojiUsage
        };
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return Formality;
        yield return Verbosity;
        yield return EmojiUsage;
    }
}
```

**Enums**:

```csharp
public enum PersonaStatus
{
    Draft = 0,       // Initial state, not used in production
    Active = 1,      // Used in production agents
    Inactive = 2,    // Temporarily disabled
    Archived = 3     // Soft deleted
}

public enum Formality
{
    Casual = 1,      // 輕鬆隨意
    Friendly = 2,    // 友善親切
    Professional = 3, // 專業正式
    Formal = 4       // 嚴謹正式
}

public enum Verbosity
{
    Concise = 1,     // 簡潔扼要
    Moderate = 2,    // 適中平衡
    Detailed = 3,    // 詳細完整
    Comprehensive = 4 // 全面深入
}

public enum EmojiUsage
{
    Never = 1,       // 從不使用
    Minimal = 2,     // 最少使用
    Moderate = 3,    // 適度使用
    Frequent = 4     // 頻繁使用
}

public enum ResponseFormat
{
    Paragraph = 1,   // 段落格式
    BulletPoints = 2,// 條列格式
    Structured = 3,  // 結構化 (包含標題、小標題)
    Conversational = 4 // 對話式
}

public enum Tone
{
    Neutral = 1,     // 中立
    Positive = 2,    // 正面積極
    Empathetic = 3,  // 同理心
    Authoritative = 4,// 權威
    Humorous = 5     // 幽默
}
```

### 2.3 Persona 配置 Schema 設計

**JSON Schema 完整文檔**: [Persona Configuration Schema](../../docs/technical-implementation/01-backend-net9/13-persona-configuration-schema.md)

**Persona Configuration JSON Schema (Draft-07)**:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Persona Configuration",
  "description": "Configuration schema for AI Agent Persona",
  "type": "object",
  "required": ["name", "description", "communicationStyle", "expertise", "responseFormat", "tone"],
  "properties": {
    "name": {
      "type": "string",
      "minLength": 3,
      "maxLength": 100,
      "description": "Persona name"
    },
    "description": {
      "type": "string",
      "minLength": 10,
      "maxLength": 500,
      "description": "Brief description of persona's purpose and characteristics"
    },
    "communicationStyle": {
      "type": "object",
      "required": ["formality", "verbosity", "emojiUsage"],
      "properties": {
        "formality": {
          "type": "string",
          "enum": ["Casual", "Friendly", "Professional", "Formal"],
          "description": "Level of formality in communication"
        },
        "verbosity": {
          "type": "string",
          "enum": ["Concise", "Moderate", "Detailed", "Comprehensive"],
          "description": "Length and depth of responses"
        },
        "emojiUsage": {
          "type": "string",
          "enum": ["Never", "Minimal", "Moderate", "Frequent"],
          "description": "Frequency of emoji usage"
        }
      }
    },
    "expertise": {
      "type": "array",
      "minItems": 1,
      "maxItems": 10,
      "items": {
        "type": "string",
        "minLength": 2,
        "maxLength": 100
      },
      "description": "List of expertise areas"
    },
    "responseFormat": {
      "type": "string",
      "enum": ["Paragraph", "BulletPoints", "Structured", "Conversational"],
      "description": "Preferred response format"
    },
    "tone": {
      "type": "string",
      "enum": ["Neutral", "Positive", "Empathetic", "Authoritative", "Humorous"],
      "description": "Overall tone of responses"
    },
    "constraints": {
      "type": "array",
      "items": {
        "type": "string",
        "minLength": 5,
        "maxLength": 200
      },
      "description": "Behavioral constraints and guidelines"
    },
    "customAttributes": {
      "type": "object",
      "additionalProperties": {
        "type": "string"
      },
      "description": "Custom key-value pairs for extensibility"
    }
  },
  "additionalProperties": false
}
```

**YAML 配置範例** (Customer Service Assistant):

```yaml
name: "Friendly Customer Service Assistant"
description: "A warm, empathetic customer service representative focused on problem resolution and customer satisfaction"

communicationStyle:
  formality: Friendly
  verbosity: Moderate
  emojiUsage: Moderate

expertise:
  - Customer Support
  - Problem Solving
  - Product Knowledge
  - Conflict Resolution
  - Communication Skills

responseFormat: Structured

tone: Empathetic

constraints:
  - "Always prioritize customer satisfaction"
  - "Remain calm and professional under pressure"
  - "Escalate complex issues to human agents when necessary"
  - "Protect customer privacy and data security"
  - "Provide accurate information or admit uncertainty"

customAttributes:
  maxResponseTime: "30 seconds"
  supportedLanguages: "English, Chinese"
  availabilityHours: "24/7"
```

**JSON Schema 驗證實作** ([Validation Strategy](../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md)):

```csharp
// PersonaConfigurationValidator - Application Layer
public class PersonaConfigurationValidator : AbstractValidator<PersonaConfigurationDto>
{
    public PersonaConfigurationValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Persona name is required")
            .Length(3, 100).WithMessage("Name must be 3-100 characters");

        RuleFor(x => x.Description)
            .NotEmpty().WithMessage("Description is required")
            .Length(10, 500).WithMessage("Description must be 10-500 characters");

        RuleFor(x => x.CommunicationStyle)
            .NotNull().WithMessage("Communication style is required")
            .SetValidator(new CommunicationStyleValidator());

        RuleFor(x => x.Expertise)
            .NotEmpty().WithMessage("At least one expertise area is required")
            .Must(e => e.Count <= 10).WithMessage("Maximum 10 expertise areas allowed");

        RuleFor(x => x.ResponseFormat)
            .IsInEnum().WithMessage("Invalid response format");

        RuleFor(x => x.Tone)
            .IsInEnum().WithMessage("Invalid tone");

        RuleFor(x => x.Constraints)
            .Must(c => c == null || c.All(constraint => constraint.Length >= 5 && constraint.Length <= 200))
            .WithMessage("Each constraint must be 5-200 characters");
    }
}

public class CommunicationStyleValidator : AbstractValidator<CommunicationStyleDto>
{
    public CommunicationStyleValidator()
    {
        RuleFor(x => x.Formality).IsInEnum();
        RuleFor(x => x.Verbosity).IsInEnum();
        RuleFor(x => x.EmojiUsage).IsInEnum();
    }
}
```

### 2.4 CQRS 實施細節

**CQRS 實作完整文檔**: [CQRS Implementation](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md)

**Create Persona Command**:

```csharp
// CreatePersonaCommand - Application Layer
public record CreatePersonaCommand(
    string Name,
    string Description,
    PersonaConfigurationDto Configuration,
    Guid? TemplateId = null
) : IRequest<Result<PersonaDto>>;

// CreatePersonaCommandHandler
public class CreatePersonaCommandHandler : IRequestHandler<CreatePersonaCommand, Result<PersonaDto>>
{
    private readonly IPersonaRepository _personaRepository;
    private readonly IPersonaTemplateRepository _templateRepository;
    private readonly IUnitOfWork _unitOfWork;
    private readonly ILogger<CreatePersonaCommandHandler> _logger;

    public CreatePersonaCommandHandler(
        IPersonaRepository personaRepository,
        IPersonaTemplateRepository templateRepository,
        IUnitOfWork unitOfWork,
        ILogger<CreatePersonaCommandHandler> logger)
    {
        _personaRepository = personaRepository;
        _templateRepository = templateRepository;
        _unitOfWork = unitOfWork;
        _logger = logger;
    }

    public async Task<Result<PersonaDto>> Handle(
        CreatePersonaCommand request,
        CancellationToken cancellationToken)
    {
        try
        {
            // Check if persona name already exists
            if (await _personaRepository.ExistsByNameAsync(request.Name, cancellationToken))
            {
                return Result<PersonaDto>.Failure($"Persona with name '{request.Name}' already exists");
            }

            // Validate template if provided
            if (request.TemplateId.HasValue)
            {
                var template = await _templateRepository.GetByIdAsync(
                    request.TemplateId.Value,
                    cancellationToken
                );
                if (template == null)
                {
                    return Result<PersonaDto>.Failure($"Template with ID '{request.TemplateId}' not found");
                }
            }

            // Convert DTO to Domain Value Object
            var communicationStyle = CommunicationStyle.Create(
                request.Configuration.CommunicationStyle.Formality,
                request.Configuration.CommunicationStyle.Verbosity,
                request.Configuration.CommunicationStyle.EmojiUsage
            );

            var configuration = PersonaConfiguration.Create(
                communicationStyle,
                request.Configuration.Expertise,
                request.Configuration.ResponseFormat,
                request.Configuration.Tone,
                request.Configuration.Constraints,
                request.Configuration.CustomAttributes
            );

            // Create Domain Entity using Factory Method
            var persona = Persona.Create(
                request.Name,
                request.Description,
                configuration,
                request.TemplateId
            );

            // Persist to Database
            await _personaRepository.AddAsync(persona, cancellationToken);
            await _unitOfWork.CommitAsync(cancellationToken);

            _logger.LogInformation(
                "Persona created successfully: {PersonaId} - {PersonaName}",
                persona.PersonaId,
                persona.Name
            );

            // Convert Domain Entity to DTO
            return Result<PersonaDto>.Success(PersonaDto.FromDomain(persona));
        }
        catch (DomainException ex)
        {
            _logger.LogWarning(ex, "Domain validation failed for persona creation");
            return Result<PersonaDto>.Failure(ex.Message);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error creating persona: {PersonaName}", request.Name);
            return Result<PersonaDto>.Failure("An error occurred while creating the persona");
        }
    }
}

// FluentValidation Validator
public class CreatePersonaCommandValidator : AbstractValidator<CreatePersonaCommand>
{
    public CreatePersonaCommandValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Persona name is required")
            .Length(3, 100).WithMessage("Name must be 3-100 characters");

        RuleFor(x => x.Description)
            .NotEmpty().WithMessage("Description is required")
            .Length(10, 500).WithMessage("Description must be 10-500 characters");

        RuleFor(x => x.Configuration)
            .NotNull().WithMessage("Configuration is required")
            .SetValidator(new PersonaConfigurationValidator());
    }
}
```

**Get Personas Query**:

```csharp
// GetPersonasQuery - Application Layer
public record GetPersonasQuery(
    int Page,
    int PageSize,
    string SearchTerm = null,
    PersonaStatus? Status = null,
    Guid? TemplateId = null
) : IRequest<Result<PagedResult<PersonaDto>>>;

// GetPersonasQueryHandler
public class GetPersonasQueryHandler : IRequestHandler<GetPersonasQuery, Result<PagedResult<PersonaDto>>>
{
    private readonly IPersonaRepository _personaRepository;
    private readonly ILogger<GetPersonasQueryHandler> _logger;

    public GetPersonasQueryHandler(
        IPersonaRepository personaRepository,
        ILogger<GetPersonasQueryHandler> logger)
    {
        _personaRepository = personaRepository;
        _logger = logger;
    }

    public async Task<Result<PagedResult<PersonaDto>>> Handle(
        GetPersonasQuery request,
        CancellationToken cancellationToken)
    {
        try
        {
            var pagedPersonas = await _personaRepository.GetPagedAsync(
                request.Page,
                request.PageSize,
                request.SearchTerm,
                request.Status,
                request.TemplateId,
                cancellationToken
            );

            var personaDtos = pagedPersonas.Items
                .Select(p => PersonaDto.FromDomain(p))
                .ToList();

            var result = new PagedResult<PersonaDto>
            {
                Items = personaDtos,
                TotalCount = pagedPersonas.TotalCount,
                Page = pagedPersonas.Page,
                PageSize = pagedPersonas.PageSize
            };

            return Result<PagedResult<PersonaDto>>.Success(result);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error retrieving personas: Page {Page}, PageSize {PageSize}",
                request.Page, request.PageSize);
            return Result<PagedResult<PersonaDto>>.Failure("An error occurred while retrieving personas");
        }
    }
}

// GetPersonasQueryValidator
public class GetPersonasQueryValidator : AbstractValidator<GetPersonasQuery>
{
    public GetPersonasQueryValidator()
    {
        RuleFor(x => x.Page)
            .GreaterThan(0).WithMessage("Page must be greater than 0");

        RuleFor(x => x.PageSize)
            .InclusiveBetween(1, 100).WithMessage("Page size must be between 1 and 100");

        RuleFor(x => x.Status)
            .IsInEnum().When(x => x.Status.HasValue).WithMessage("Invalid persona status");
    }
}
```

**Create Persona From Template Command**:

```csharp
// CreatePersonaFromTemplateCommand - Application Layer
public record CreatePersonaFromTemplateCommand(
    Guid TemplateId,
    string PersonaName,
    Dictionary<string, string> CustomOverrides = null
) : IRequest<Result<PersonaDto>>;

// CreatePersonaFromTemplateCommandHandler
public class CreatePersonaFromTemplateCommandHandler
    : IRequestHandler<CreatePersonaFromTemplateCommand, Result<PersonaDto>>
{
    private readonly IPersonaRepository _personaRepository;
    private readonly IPersonaTemplateRepository _templateRepository;
    private readonly IUnitOfWork _unitOfWork;
    private readonly ILogger<CreatePersonaFromTemplateCommandHandler> _logger;

    public async Task<Result<PersonaDto>> Handle(
        CreatePersonaFromTemplateCommand request,
        CancellationToken cancellationToken)
    {
        try
        {
            // Retrieve template
            var template = await _templateRepository.GetByIdAsync(request.TemplateId, cancellationToken);
            if (template == null)
            {
                return Result<PersonaDto>.Failure($"Template with ID '{request.TemplateId}' not found");
            }

            // Check if persona name already exists
            if (await _personaRepository.ExistsByNameAsync(request.PersonaName, cancellationToken))
            {
                return Result<PersonaDto>.Failure($"Persona with name '{request.PersonaName}' already exists");
            }

            // Clone configuration from template
            var configuration = template.Configuration.Clone();

            // Apply custom overrides if provided
            if (request.CustomOverrides != null && request.CustomOverrides.Any())
            {
                configuration.ApplyOverrides(request.CustomOverrides);
            }

            // Create Domain Entity
            var persona = Persona.Create(
                request.PersonaName,
                template.Description,
                configuration,
                template.TemplateId
            );

            // Persist to Database
            await _personaRepository.AddAsync(persona, cancellationToken);
            await _unitOfWork.CommitAsync(cancellationToken);

            _logger.LogInformation(
                "Persona created from template: {PersonaId} - {PersonaName} (Template: {TemplateId})",
                persona.PersonaId,
                persona.Name,
                template.TemplateId
            );

            return Result<PersonaDto>.Success(PersonaDto.FromDomain(persona));
        }
        catch (DomainException ex)
        {
            _logger.LogWarning(ex, "Domain validation failed for persona creation from template");
            return Result<PersonaDto>.Failure(ex.Message);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error creating persona from template: {TemplateId}", request.TemplateId);
            return Result<PersonaDto>.Failure("An error occurred while creating persona from template");
        }
    }
}
```

### 2.5 API 層實施細節

**API Layer 完整文檔**: [API Layer Design](../../docs/technical-implementation/01-backend-net9/08-api-layer.md)

**PersonasController - 6 個端點**:

```csharp
// PersonasController - API Layer
[ApiController]
[Route("api/v1/personas")]
[Produces("application/json")]
public class PersonasController : ControllerBase
{
    private readonly IMediator _mediator;
    private readonly ILogger<PersonasController> _logger;

    public PersonasController(IMediator mediator, ILogger<PersonasController> logger)
    {
        _mediator = mediator;
        _logger = logger;
    }

    /// <summary>
    /// Create a new persona
    /// </summary>
    [HttpPost]
    [ProducesResponseType(typeof(PersonaDto), StatusCodes.Status201Created)]
    [ProducesResponseType(typeof(ErrorResponse), StatusCodes.Status400BadRequest)]
    public async Task<IActionResult> CreatePersona(
        [FromBody] CreatePersonaCommand command,
        CancellationToken cancellationToken)
    {
        var result = await _mediator.Send(command, cancellationToken);

        if (result.IsFailure)
        {
            return BadRequest(new ErrorResponse { Message = result.Error });
        }

        return CreatedAtAction(
            nameof(GetPersonaById),
            new { id = result.Value.PersonaId },
            result.Value
        );
    }

    /// <summary>
    /// Get paginated list of personas
    /// </summary>
    [HttpGet]
    [ProducesResponseType(typeof(PagedResult<PersonaDto>), StatusCodes.Status200OK)]
    public async Task<IActionResult> GetPersonas(
        [FromQuery] int page = 1,
        [FromQuery] int pageSize = 20,
        [FromQuery] string searchTerm = null,
        [FromQuery] PersonaStatus? status = null,
        [FromQuery] Guid? templateId = null,
        CancellationToken cancellationToken = default)
    {
        var query = new GetPersonasQuery(page, pageSize, searchTerm, status, templateId);
        var result = await _mediator.Send(query, cancellationToken);

        if (result.IsFailure)
        {
            return BadRequest(new ErrorResponse { Message = result.Error });
        }

        return Ok(result.Value);
    }

    /// <summary>
    /// Get persona by ID
    /// </summary>
    [HttpGet("{id}")]
    [ProducesResponseType(typeof(PersonaDto), StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<IActionResult> GetPersonaById(
        [FromRoute] Guid id,
        CancellationToken cancellationToken)
    {
        var query = new GetPersonaByIdQuery(id);
        var result = await _mediator.Send(query, cancellationToken);

        if (result.IsFailure)
        {
            return NotFound(new ErrorResponse { Message = result.Error });
        }

        return Ok(result.Value);
    }

    /// <summary>
    /// Update persona
    /// </summary>
    [HttpPut("{id}")]
    [ProducesResponseType(typeof(PersonaDto), StatusCodes.Status200OK)]
    [ProducesResponseType(typeof(ErrorResponse), StatusCodes.Status400BadRequest)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<IActionResult> UpdatePersona(
        [FromRoute] Guid id,
        [FromBody] UpdatePersonaCommand command,
        CancellationToken cancellationToken)
    {
        if (id != command.PersonaId)
        {
            return BadRequest(new ErrorResponse { Message = "Persona ID mismatch" });
        }

        var result = await _mediator.Send(command, cancellationToken);

        if (result.IsFailure)
        {
            return result.Error.Contains("not found")
                ? NotFound(new ErrorResponse { Message = result.Error })
                : BadRequest(new ErrorResponse { Message = result.Error });
        }

        return Ok(result.Value);
    }

    /// <summary>
    /// Activate persona
    /// </summary>
    [HttpPut("{id}/activate")]
    [ProducesResponseType(typeof(PersonaDto), StatusCodes.Status200OK)]
    [ProducesResponseType(typeof(ErrorResponse), StatusCodes.Status400BadRequest)]
    public async Task<IActionResult> ActivatePersona(
        [FromRoute] Guid id,
        CancellationToken cancellationToken)
    {
        var command = new ActivatePersonaCommand(id);
        var result = await _mediator.Send(command, cancellationToken);

        if (result.IsFailure)
        {
            return BadRequest(new ErrorResponse { Message = result.Error });
        }

        return Ok(result.Value);
    }

    /// <summary>
    /// Delete persona (soft delete)
    /// </summary>
    [HttpDelete("{id}")]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(typeof(ErrorResponse), StatusCodes.Status400BadRequest)]
    public async Task<IActionResult> DeletePersona(
        [FromRoute] Guid id,
        CancellationToken cancellationToken)
    {
        var command = new DeletePersonaCommand(id);
        var result = await _mediator.Send(command, cancellationToken);

        if (result.IsFailure)
        {
            return BadRequest(new ErrorResponse { Message = result.Error });
        }

        return NoContent();
    }

    /// <summary>
    /// Create persona from template
    /// </summary>
    [HttpPost("from-template/{templateId}")]
    [ProducesResponseType(typeof(PersonaDto), StatusCodes.Status201Created)]
    [ProducesResponseType(typeof(ErrorResponse), StatusCodes.Status400BadRequest)]
    public async Task<IActionResult> CreatePersonaFromTemplate(
        [FromRoute] Guid templateId,
        [FromBody] CreatePersonaFromTemplateCommand command,
        CancellationToken cancellationToken)
    {
        if (templateId != command.TemplateId)
        {
            return BadRequest(new ErrorResponse { Message = "Template ID mismatch" });
        }

        var result = await _mediator.Send(command, cancellationToken);

        if (result.IsFailure)
        {
            return BadRequest(new ErrorResponse { Message = result.Error });
        }

        return CreatedAtAction(
            nameof(GetPersonaById),
            new { id = result.Value.PersonaId },
            result.Value
        );
    }
}

// PersonaDto
public record PersonaDto(
    Guid PersonaId,
    string Name,
    string Description,
    PersonaConfigurationDto Configuration,
    Guid? TemplateId,
    PersonaStatus Status,
    DateTime CreatedAt,
    DateTime? UpdatedAt
)
{
    public static PersonaDto FromDomain(Persona persona)
    {
        return new PersonaDto(
            persona.PersonaId,
            persona.Name,
            persona.Description,
            PersonaConfigurationDto.FromDomain(persona.Configuration),
            persona.TemplateId,
            persona.Status,
            persona.CreatedAt,
            persona.UpdatedAt
        );
    }
}

// PersonaConfigurationDto
public record PersonaConfigurationDto(
    CommunicationStyleDto CommunicationStyle,
    List<string> Expertise,
    ResponseFormat ResponseFormat,
    Tone Tone,
    List<string> Constraints,
    Dictionary<string, string> CustomAttributes
)
{
    public static PersonaConfigurationDto FromDomain(PersonaConfiguration configuration)
    {
        return new PersonaConfigurationDto(
            CommunicationStyleDto.FromDomain(configuration.CommunicationStyle),
            configuration.Expertise,
            configuration.ResponseFormat,
            configuration.Tone,
            configuration.Constraints,
            configuration.CustomAttributes
        );
    }
}

// CommunicationStyleDto
public record CommunicationStyleDto(
    Formality Formality,
    Verbosity Verbosity,
    EmojiUsage EmojiUsage
)
{
    public static CommunicationStyleDto FromDomain(CommunicationStyle communicationStyle)
    {
        return new CommunicationStyleDto(
            communicationStyle.Formality,
            communicationStyle.Verbosity,
            communicationStyle.EmojiUsage
        );
    }
}
```

---

## 三、US 7.2: Persona-Driven Prompt Engineering - 詳細技術上下文

### 3.1 System Prompt 生成策略

**完整 User Story 規格**: [US 7.2 - Persona-Driven Prompt Engineering](../../docs/user-stories/modules/module-07-persona-framework.md#us-72)

**Prompt 生成架構設計** ([ADR-014: Prompt Engineering Strategy](../../docs/architecture/adr/ADR-014-prompt-engineering-strategy.md)):

```
┌─────────────────────────────────────────┐
│  API Layer                              │
│  - POST /api/v1/personas/{id}/generate  │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│  Application Layer                      │
│  - GenerateSystemPromptCommand          │
│  - GenerateSystemPromptHandler          │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│  Domain Service                         │
│  - IPromptGenerationService             │
│  - PromptGenerationService              │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│  Infrastructure Layer                   │
│  - PromptTemplateEngine (Scriban)      │
│  - PromptOptimizationService            │
└─────────────────────────────────────────┘
```

**PromptGenerationService 實作**:

```csharp
// IPromptGenerationService - Domain Layer Interface
public interface IPromptGenerationService
{
    Task<string> GenerateSystemPromptAsync(
        Persona persona,
        PromptGenerationContext context,
        CancellationToken cancellationToken = default);

    Task<PromptValidationResult> ValidatePromptAsync(
        string prompt,
        CancellationToken cancellationToken = default);

    Task<int> EstimateTokenCountAsync(
        string prompt,
        CancellationToken cancellationToken = default);
}

// PromptGenerationService - Infrastructure Layer
public class PromptGenerationService : IPromptGenerationService
{
    private readonly IPromptTemplateEngine _templateEngine;
    private readonly IPromptOptimizationService _optimizationService;
    private readonly ILogger<PromptGenerationService> _logger;

    private const string BASE_TEMPLATE = @"
# SYSTEM ROLE AND IDENTITY

You are {{persona.name}}, {{persona.description}}.

# COMMUNICATION STYLE

**Formality**: {{persona.communication_style.formality}}
**Verbosity**: {{persona.communication_style.verbosity}}
**Emoji Usage**: {{persona.communication_style.emojiUsage}}

# EXPERTISE AREAS

{{for area in persona.expertise}}
- {{area}}
{{end}}

# RESPONSE FORMAT

Preferred format: {{persona.response_format}}
Overall tone: {{persona.tone}}

# BEHAVIORAL GUIDELINES

{{for constraint in persona.constraints}}
- {{constraint}}
{{end}}

# CONTEXT AWARENESS

{{if context.has_previous_conversation}}
Previous Conversation Summary: {{context.previous_summary}}
{{end}}

{{if context.user_preferences}}
User Preferences: {{context.user_preferences}}
{{end}}

# TASK INSTRUCTION

{{context.task_instruction}}
";

    public PromptGenerationService(
        IPromptTemplateEngine templateEngine,
        IPromptOptimizationService optimizationService,
        ILogger<PromptGenerationService> logger)
    {
        _templateEngine = templateEngine;
        _optimizationService = optimizationService;
        _logger = logger;
    }

    public async Task<string> GenerateSystemPromptAsync(
        Persona persona,
        PromptGenerationContext context,
        CancellationToken cancellationToken)
    {
        try
        {
            // Step 1: Prepare template data
            var templateData = new
            {
                persona = new
                {
                    name = persona.Name,
                    description = persona.Description,
                    communication_style = new
                    {
                        formality = GetFormalityDescription(persona.Configuration.CommunicationStyle.Formality),
                        verbosity = GetVerbosityDescription(persona.Configuration.CommunicationStyle.Verbosity),
                        emojiUsage = GetEmojiUsageDescription(persona.Configuration.CommunicationStyle.EmojiUsage)
                    },
                    expertise = persona.Configuration.Expertise,
                    response_format = GetResponseFormatDescription(persona.Configuration.ResponseFormat),
                    tone = GetToneDescription(persona.Configuration.Tone),
                    constraints = persona.Configuration.Constraints
                },
                context = new
                {
                    has_previous_conversation = !string.IsNullOrWhiteSpace(context.PreviousConversationSummary),
                    previous_summary = context.PreviousConversationSummary,
                    user_preferences = context.UserPreferences,
                    task_instruction = context.TaskInstruction
                }
            };

            // Step 2: Render template
            var generatedPrompt = await _templateEngine.RenderAsync(BASE_TEMPLATE, templateData, cancellationToken);

            // Step 3: Optimize for token efficiency
            if (context.OptimizeForTokens)
            {
                generatedPrompt = await _optimizationService.OptimizePromptAsync(generatedPrompt, cancellationToken);
            }

            // Step 4: Validate generated prompt
            var validation = await ValidatePromptAsync(generatedPrompt, cancellationToken);
            if (!validation.IsValid)
            {
                _logger.LogWarning(
                    "Generated prompt validation failed: {Errors}",
                    string.Join(", ", validation.Errors)
                );
                throw new DomainException($"Prompt validation failed: {string.Join(", ", validation.Errors)}");
            }

            _logger.LogInformation(
                "System prompt generated successfully for persona {PersonaId} - Token count: {TokenCount}",
                persona.PersonaId,
                validation.TokenCount
            );

            return generatedPrompt;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error generating system prompt for persona {PersonaId}", persona.PersonaId);
            throw;
        }
    }

    public async Task<PromptValidationResult> ValidatePromptAsync(
        string prompt,
        CancellationToken cancellationToken)
    {
        var errors = new List<string>();

        // Validate prompt length
        if (string.IsNullOrWhiteSpace(prompt))
        {
            errors.Add("Prompt cannot be empty");
        }

        // Estimate token count
        var tokenCount = await EstimateTokenCountAsync(prompt, cancellationToken);

        // Check token limits (GPT-4 context window: 8192 tokens, reserve 90% for user input)
        var maxSystemPromptTokens = 819; // ~10% of context window
        if (tokenCount > maxSystemPromptTokens)
        {
            errors.Add($"Prompt exceeds maximum token count: {tokenCount} > {maxSystemPromptTokens}");
        }

        return new PromptValidationResult
        {
            IsValid = errors.Count == 0,
            Errors = errors,
            TokenCount = tokenCount
        };
    }

    public async Task<int> EstimateTokenCountAsync(
        string prompt,
        CancellationToken cancellationToken)
    {
        // Use TikToken library for accurate GPT-4 token estimation
        // Approximate: 1 token ≈ 4 characters for English text
        // For precise estimation, integrate SharpToken or TikTokenSharp library
        var approximateTokens = prompt.Length / 4;
        return await Task.FromResult(approximateTokens);
    }

    // Helper methods for converting enums to human-readable descriptions
    private string GetFormalityDescription(Formality formality) => formality switch
    {
        Formality.Casual => "Casual and relaxed, using informal language",
        Formality.Friendly => "Friendly and approachable, warm but professional",
        Formality.Professional => "Professional and courteous, business-appropriate",
        Formality.Formal => "Formal and respectful, highly professional tone",
        _ => "Moderate formality"
    };

    private string GetVerbosityDescription(Verbosity verbosity) => verbosity switch
    {
        Verbosity.Concise => "Concise and to-the-point, minimal elaboration",
        Verbosity.Moderate => "Balanced detail, neither too brief nor too lengthy",
        Verbosity.Detailed => "Detailed explanations with supporting information",
        Verbosity.Comprehensive => "Comprehensive and thorough, covering all aspects",
        _ => "Moderate verbosity"
    };

    private string GetEmojiUsageDescription(EmojiUsage emojiUsage) => emojiUsage switch
    {
        EmojiUsage.Never => "Never use emojis",
        EmojiUsage.Minimal => "Use emojis sparingly, only when highly appropriate",
        EmojiUsage.Moderate => "Use emojis moderately to enhance communication",
        EmojiUsage.Frequent => "Use emojis frequently to add personality and warmth",
        _ => "Minimal emoji usage"
    };

    private string GetResponseFormatDescription(ResponseFormat format) => format switch
    {
        ResponseFormat.Paragraph => "Use paragraph format with flowing narrative",
        ResponseFormat.BulletPoints => "Use bullet points and lists for clarity",
        ResponseFormat.Structured => "Use structured format with headers and sections",
        ResponseFormat.Conversational => "Use conversational, back-and-forth style",
        _ => "Flexible format based on context"
    };

    private string GetToneDescription(Tone tone) => tone switch
    {
        Tone.Neutral => "Maintain a neutral, objective tone",
        Tone.Positive => "Use a positive, optimistic tone",
        Tone.Empathetic => "Show empathy and understanding",
        Tone.Authoritative => "Project authority and expertise",
        Tone.Humorous => "Add appropriate humor when suitable",
        _ => "Context-appropriate tone"
    };
}

// PromptGenerationContext
public class PromptGenerationContext
{
    public string TaskInstruction { get; set; }
    public string PreviousConversationSummary { get; set; }
    public string UserPreferences { get; set; }
    public bool OptimizeForTokens { get; set; } = true;
}

// PromptValidationResult
public class PromptValidationResult
{
    public bool IsValid { get; set; }
    public List<string> Errors { get; set; } = new();
    public int TokenCount { get; set; }
}
```

### 3.2 Prompt Template 引擎設計

**Scriban Template Engine 整合** ([Template Engine Design](../../docs/technical-implementation/01-backend-net9/14-prompt-template-engine.md)):

```csharp
// IPromptTemplateEngine - Domain Layer Interface
public interface IPromptTemplateEngine
{
    Task<string> RenderAsync(
        string template,
        object data,
        CancellationToken cancellationToken = default);

    Task<string> RenderFromFileAsync(
        string templatePath,
        object data,
        CancellationToken cancellationToken = default);

    Task<bool> ValidateTemplateAsync(
        string template,
        CancellationToken cancellationToken = default);
}

// PromptTemplateEngine - Infrastructure Layer
public class PromptTemplateEngine : IPromptTemplateEngine
{
    private readonly ILogger<PromptTemplateEngine> _logger;

    public PromptTemplateEngine(ILogger<PromptTemplateEngine> logger)
    {
        _logger = logger;
    }

    public async Task<string> RenderAsync(
        string template,
        object data,
        CancellationToken cancellationToken)
    {
        try
        {
            var scribanTemplate = Template.Parse(template);

            if (scribanTemplate.HasErrors)
            {
                var errors = string.Join(", ", scribanTemplate.Messages);
                throw new InvalidOperationException($"Template parse errors: {errors}");
            }

            var context = new TemplateContext();
            var scriptObject = new ScriptObject();
            scriptObject.Import(data, renamer: member => member.Name);
            context.PushGlobal(scriptObject);

            var rendered = await scribanTemplate.RenderAsync(context);
            return rendered;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error rendering template");
            throw;
        }
    }

    public async Task<string> RenderFromFileAsync(
        string templatePath,
        object data,
        CancellationToken cancellationToken)
    {
        var template = await File.ReadAllTextAsync(templatePath, cancellationToken);
        return await RenderAsync(template, data, cancellationToken);
    }

    public async Task<bool> ValidateTemplateAsync(
        string template,
        CancellationToken cancellationToken)
    {
        try
        {
            var scribanTemplate = Template.Parse(template);
            return await Task.FromResult(!scribanTemplate.HasErrors);
        }
        catch
        {
            return false;
        }
    }
}
```

**Custom Scriban Functions for Prompt Engineering**:

```csharp
// CustomScribanFunctions - Infrastructure Layer
public class CustomScribanFunctions : ScriptObject
{
    public static string TruncateText(string text, int maxLength)
    {
        if (string.IsNullOrEmpty(text) || text.Length <= maxLength)
            return text;

        return text.Substring(0, maxLength - 3) + "...";
    }

    public static string FormatList(IEnumerable<string> items, string separator = ", ")
    {
        return items != null ? string.Join(separator, items) : string.Empty;
    }

    public static string CapitalizeFirst(string text)
    {
        if (string.IsNullOrEmpty(text))
            return text;

        return char.ToUpper(text[0]) + text.Substring(1);
    }

    public static int EstimateTokens(string text)
    {
        // Approximate token estimation: 1 token ≈ 4 characters
        return text?.Length / 4 ?? 0;
    }

    public static string SanitizeInput(string input)
    {
        // Remove potentially harmful characters for prompt injection prevention
        return input?.Replace("{", "{{").Replace("}", "}}") ?? string.Empty;
    }
}

// Register Custom Functions
public static class ScribanExtensions
{
    public static void RegisterCustomFunctions(this TemplateContext context)
    {
        var customFunctions = new CustomScribanFunctions();
        context.PushGlobal(customFunctions);
    }
}
```

### 3.3 Persona 行為注入機制

**Persona Behavior Injection Service**:

```csharp
// IPersonaBehaviorInjectionService - Domain Layer Interface
public interface IPersonaBehaviorInjectionService
{
    Task<string> InjectPersonaBehaviorAsync(
        string basePrompt,
        Persona persona,
        CancellationToken cancellationToken = default);

    Task<string> GenerateFewShotExamplesAsync(
        Persona persona,
        int count = 3,
        CancellationToken cancellationToken = default);
}

// PersonaBehaviorInjectionService - Infrastructure Layer
public class PersonaBehaviorInjectionService : IPersonaBehaviorInjectionService
{
    private readonly IPromptTemplateEngine _templateEngine;
    private readonly ILogger<PersonaBehaviorInjectionService> _logger;

    private const string BEHAVIOR_INJECTION_TEMPLATE = @"
{{basePrompt}}

# RESPONSE STYLE EXAMPLES

{{for example in examples}}
**User**: {{example.user_message}}
**{{persona_name}}**: {{example.assistant_response}}

{{end}}

# IMPORTANT REMINDERS

- Always maintain the {{formality}} tone described above
- Keep responses {{verbosity}} as specified
- {{emoji_instruction}}
- Format responses using {{response_format}}
";

    public PersonaBehaviorInjectionService(
        IPromptTemplateEngine templateEngine,
        ILogger<PersonaBehaviorInjectionService> logger)
    {
        _templateEngine = templateEngine;
        _logger = logger;
    }

    public async Task<string> InjectPersonaBehaviorAsync(
        string basePrompt,
        Persona persona,
        CancellationToken cancellationToken)
    {
        try
        {
            // Generate few-shot examples that demonstrate persona behavior
            var examples = await GenerateFewShotExamplesAsync(persona, 3, cancellationToken);

            var templateData = new
            {
                basePrompt,
                examples,
                persona_name = persona.Name,
                formality = GetFormalityDescription(persona.Configuration.CommunicationStyle.Formality),
                verbosity = GetVerbosityDescription(persona.Configuration.CommunicationStyle.Verbosity),
                emoji_instruction = GetEmojiInstruction(persona.Configuration.CommunicationStyle.EmojiUsage),
                response_format = GetResponseFormatDescription(persona.Configuration.ResponseFormat)
            };

            var injectedPrompt = await _templateEngine.RenderAsync(
                BEHAVIOR_INJECTION_TEMPLATE,
                templateData,
                cancellationToken
            );

            return injectedPrompt;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error injecting persona behavior for persona {PersonaId}", persona.PersonaId);
            throw;
        }
    }

    public async Task<string> GenerateFewShotExamplesAsync(
        Persona persona,
        int count = 3,
        CancellationToken cancellationToken)
    {
        // Generate few-shot examples that demonstrate the persona's style
        // This is a simplified version - in production, you might want to:
        // 1. Use a database of curated examples
        // 2. Generate examples using a meta-prompt
        // 3. Store examples in the PersonaTemplate

        var examples = new List<object>();

        if (persona.Configuration.CommunicationStyle.Formality == Formality.Casual)
        {
            examples.Add(new
            {
                user_message = "What's the weather like?",
                assistant_response = "Hey! It's looking pretty nice today - sunny with a chance of awesome! ☀️"
            });
        }
        else if (persona.Configuration.CommunicationStyle.Formality == Formality.Professional)
        {
            examples.Add(new
            {
                user_message = "What's the weather forecast?",
                assistant_response = "Good morning. The forecast indicates clear skies with temperatures reaching 22°C. Ideal conditions for outdoor activities."
            });
        }

        // Generate more examples based on other persona characteristics...

        return await Task.FromResult(System.Text.Json.JsonSerializer.Serialize(examples));
    }

    private string GetFormalityDescription(Formality formality) => formality switch
    {
        Formality.Casual => "casual and relaxed",
        Formality.Friendly => "friendly and approachable",
        Formality.Professional => "professional and courteous",
        Formality.Formal => "formal and respectful",
        _ => "moderate"
    };

    private string GetVerbosityDescription(Verbosity verbosity) => verbosity switch
    {
        Verbosity.Concise => "concise and brief",
        Verbosity.Moderate => "moderately detailed",
        Verbosity.Detailed => "detailed and thorough",
        Verbosity.Comprehensive => "comprehensive and exhaustive",
        _ => "moderate"
    };

    private string GetEmojiInstruction(EmojiUsage emojiUsage) => emojiUsage switch
    {
        EmojiUsage.Never => "Never use emojis in responses",
        EmojiUsage.Minimal => "Use emojis sparingly, only when highly appropriate",
        EmojiUsage.Moderate => "Use emojis moderately to enhance communication",
        EmojiUsage.Frequent => "Use emojis frequently to add personality and warmth",
        _ => "Use emojis as appropriate"
    };

    private string GetResponseFormatDescription(ResponseFormat format) => format switch
    {
        ResponseFormat.Paragraph => "flowing paragraphs",
        ResponseFormat.BulletPoints => "bullet points and lists",
        ResponseFormat.Structured => "structured sections with headers",
        ResponseFormat.Conversational => "conversational back-and-forth style",
        _ => "context-appropriate format"
    };
}
```

### 3.4 Token 效率優化

**Prompt Optimization Service** ([Token Optimization Strategy](../../docs/architecture/adr/ADR-014-prompt-engineering-strategy.md#token-optimization)):

```csharp
// IPromptOptimizationService - Domain Layer Interface
public interface IPromptOptimizationService
{
    Task<string> OptimizePromptAsync(
        string prompt,
        CancellationToken cancellationToken = default);

    Task<OptimizationReport> AnalyzePromptAsync(
        string prompt,
        CancellationToken cancellationToken = default);

    Task<string> CompressPromptAsync(
        string prompt,
        int targetTokenCount,
        CancellationToken cancellationToken = default);
}

// PromptOptimizationService - Infrastructure Layer
public class PromptOptimizationService : IPromptOptimizationService
{
    private readonly ILogger<PromptOptimizationService> _logger;

    public PromptOptimizationService(ILogger<PromptOptimizationService> logger)
    {
        _logger = logger;
    }

    public async Task<string> OptimizePromptAsync(
        string prompt,
        CancellationToken cancellationToken)
    {
        try
        {
            var optimized = prompt;

            // Optimization Step 1: Remove redundant whitespace
            optimized = RemoveRedundantWhitespace(optimized);

            // Optimization Step 2: Simplify verbose phrases
            optimized = SimplifyVerbosePhrases(optimized);

            // Optimization Step 3: Remove unnecessary punctuation
            optimized = RemoveUnnecessaryPunctuation(optimized);

            // Optimization Step 4: Use abbreviations for common terms
            optimized = UseAbbreviations(optimized);

            var originalTokens = EstimateTokenCount(prompt);
            var optimizedTokens = EstimateTokenCount(optimized);
            var savings = originalTokens - optimizedTokens;
            var savingsPercent = (double)savings / originalTokens * 100;

            _logger.LogInformation(
                "Prompt optimized: {OriginalTokens} → {OptimizedTokens} tokens ({SavingsPercent:F1}% reduction)",
                originalTokens,
                optimizedTokens,
                savingsPercent
            );

            return await Task.FromResult(optimized);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error optimizing prompt");
            return prompt; // Return original if optimization fails
        }
    }

    public async Task<OptimizationReport> AnalyzePromptAsync(
        string prompt,
        CancellationToken cancellationToken)
    {
        var tokenCount = EstimateTokenCount(prompt);
        var characterCount = prompt.Length;
        var lineCount = prompt.Split('\n').Length;
        var wordCount = prompt.Split(new[] { ' ', '\n', '\r', '\t' }, StringSplitOptions.RemoveEmptyEntries).Length;

        var redundantWhitespace = CalculateRedundantWhitespace(prompt);
        var verbosePhrases = FindVerbosePhrases(prompt);
        var optimizationPotential = CalculateOptimizationPotential(prompt);

        return await Task.FromResult(new OptimizationReport
        {
            TokenCount = tokenCount,
            CharacterCount = characterCount,
            LineCount = lineCount,
            WordCount = wordCount,
            RedundantWhitespaceCount = redundantWhitespace,
            VerbosePhraseCount = verbosePhrases.Count,
            OptimizationPotential = optimizationPotential,
            Recommendations = GenerateRecommendations(tokenCount, optimizationPotential)
        });
    }

    public async Task<string> CompressPromptAsync(
        string prompt,
        int targetTokenCount,
        CancellationToken cancellationToken)
    {
        var currentTokens = EstimateTokenCount(prompt);
        if (currentTokens <= targetTokenCount)
        {
            return prompt;
        }

        var compressionRatio = (double)targetTokenCount / currentTokens;
        var compressed = prompt;

        // Aggressive compression strategies
        compressed = RemoveExamples(compressed, compressionRatio);
        compressed = TruncateLongSections(compressed, compressionRatio);
        compressed = await OptimizePromptAsync(compressed, cancellationToken);

        return compressed;
    }

    // Helper Methods
    private string RemoveRedundantWhitespace(string text)
    {
        // Remove multiple spaces, tabs, newlines
        return System.Text.RegularExpressions.Regex.Replace(text, @"\s+", " ").Trim();
    }

    private string SimplifyVerbosePhrases(string text)
    {
        var replacements = new Dictionary<string, string>
        {
            { "in order to", "to" },
            { "due to the fact that", "because" },
            { "at this point in time", "now" },
            { "for the purpose of", "to" },
            { "in the event that", "if" },
            { "with regard to", "about" },
            { "in accordance with", "per" }
        };

        foreach (var (verbose, concise) in replacements)
        {
            text = text.Replace(verbose, concise, StringComparison.OrdinalIgnoreCase);
        }

        return text;
    }

    private string RemoveUnnecessaryPunctuation(string text)
    {
        // Remove excessive punctuation while preserving meaning
        text = System.Text.RegularExpressions.Regex.Replace(text, @"\.{2,}", ".");
        text = System.Text.RegularExpressions.Regex.Replace(text, @"!{2,}", "!");
        text = System.Text.RegularExpressions.Regex.Replace(text, @"\?{2,}", "?");
        return text;
    }

    private string UseAbbreviations(string text)
    {
        var abbreviations = new Dictionary<string, string>
        {
            { "for example", "e.g." },
            { "that is", "i.e." },
            { "et cetera", "etc." },
            { "versus", "vs." }
        };

        foreach (var (full, abbr) in abbreviations)
        {
            text = text.Replace(full, abbr, StringComparison.OrdinalIgnoreCase);
        }

        return text;
    }

    private int EstimateTokenCount(string text)
    {
        // Approximate: 1 token ≈ 4 characters for English text
        return text.Length / 4;
    }

    private int CalculateRedundantWhitespace(string text)
    {
        var matches = System.Text.RegularExpressions.Regex.Matches(text, @"\s{2,}");
        return matches.Count;
    }

    private List<string> FindVerbosePhrases(string text)
    {
        var verbosePhrases = new List<string>
        {
            "in order to", "due to the fact that", "at this point in time",
            "for the purpose of", "in the event that", "with regard to"
        };

        return verbosePhrases.Where(phrase =>
            text.Contains(phrase, StringComparison.OrdinalIgnoreCase)
        ).ToList();
    }

    private int CalculateOptimizationPotential(string text)
    {
        var currentTokens = EstimateTokenCount(text);
        var optimizedTokens = EstimateTokenCount(RemoveRedundantWhitespace(SimplifyVerbosePhrases(text)));
        return currentTokens - optimizedTokens;
    }

    private List<string> GenerateRecommendations(int tokenCount, int optimizationPotential)
    {
        var recommendations = new List<string>();

        if (tokenCount > 800)
        {
            recommendations.Add("Consider splitting prompt into multiple sections");
        }

        if (optimizationPotential > 50)
        {
            recommendations.Add($"Significant optimization potential: ~{optimizationPotential} tokens can be saved");
        }

        if (optimizationPotential < 10)
        {
            recommendations.Add("Prompt is already well-optimized");
        }

        return recommendations;
    }

    private string RemoveExamples(string text, double compressionRatio)
    {
        // Remove example sections if compression ratio is aggressive
        if (compressionRatio < 0.7)
        {
            text = System.Text.RegularExpressions.Regex.Replace(
                text,
                @"# RESPONSE STYLE EXAMPLES\n.*?(?=\n#|\z)",
                "",
                System.Text.RegularExpressions.RegexOptions.Singleline
            );
        }
        return text;
    }

    private string TruncateLongSections(string text, double compressionRatio)
    {
        // Truncate long sections if compression is needed
        var sections = text.Split(new[] { "# " }, StringSplitOptions.RemoveEmptyEntries);
        var truncatedSections = sections.Select(section =>
        {
            var lines = section.Split('\n');
            var targetLines = (int)(lines.Length * compressionRatio);
            return string.Join("\n", lines.Take(Math.Max(targetLines, 3)));
        });

        return "# " + string.Join("\n# ", truncatedSections);
    }
}

// OptimizationReport
public class OptimizationReport
{
    public int TokenCount { get; set; }
    public int CharacterCount { get; set; }
    public int LineCount { get; set; }
    public int WordCount { get; set; }
    public int RedundantWhitespaceCount { get; set; }
    public int VerbosePhraseCount { get; set; }
    public int OptimizationPotential { get; set; }
    public List<string> Recommendations { get; set; } = new();
}
```

---

## 四、US 2.2: Plugin 熱重載機制 - 詳細技術上下文

### 4.1 熱重載架構設計

**完整 User Story 規格**: [US 2.2 - Plugin Hot Reload Mechanism](../../docs/user-stories/modules/module-02-plugin-system.md#us-22)

**熱重載架構設計** ([ADR-015: Plugin Hot Reload Strategy](../../docs/architecture/adr/ADR-015-plugin-hot-reload-strategy.md)):

```
┌─────────────────────────────────────────┐
│  FileSystemWatcher                      │
│  - Monitor Plugin Directory             │
│  - Detect .dll / .yaml Changes          │
└─────────────────────────────────────────┘
               ↓ (File Changed Event)
┌─────────────────────────────────────────┐
│  PluginHotReloadService                 │
│  - Validate Plugin Version              │
│  - Coordinate Reload Process            │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│  PluginVersionCoordinator               │
│  - Version Comparison (SemVer)          │
│  - Active Agent Detection               │
│  - Rollback Decision Logic              │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│  PluginLoader (with AppDomain)          │
│  - Unload Old Version (AppDomain.Unload)│
│  - Load New Version (New AppDomain)     │
└─────────────────────────────────────────┘
               ↓
┌─────────────────────────────────────────┐
│  Agent Runtime                          │
│  - Rebind to New Plugin Version         │
│  - Preserve Conversation Context        │
└─────────────────────────────────────────┘
```

**PluginHotReloadService 實作**:

```csharp
// IPluginHotReloadService - Domain Layer Interface
public interface IPluginHotReloadService
{
    Task<PluginReloadResult> ReloadPluginAsync(
        Guid pluginId,
        CancellationToken cancellationToken = default);

    Task<PluginReloadResult> ActivatePluginVersionAsync(
        Guid pluginId,
        string version,
        CancellationToken cancellationToken = default);

    Task<PluginReloadResult> RollbackPluginAsync(
        Guid pluginId,
        CancellationToken cancellationToken = default);

    Task<List<ActivePluginInfo>> GetActivePluginsAsync(
        CancellationToken cancellationToken = default);
}

// PluginHotReloadService - Infrastructure Layer
public class PluginHotReloadService : IPluginHotReloadService
{
    private readonly IPluginRepository _pluginRepository;
    private readonly IPluginVersionRepository _versionRepository;
    private readonly IPluginLoader _pluginLoader;
    private readonly IPluginVersionCoordinator _versionCoordinator;
    private readonly IAgentRepository _agentRepository;
    private readonly ILogger<PluginHotReloadService> _logger;

    public PluginHotReloadService(
        IPluginRepository pluginRepository,
        IPluginVersionRepository versionRepository,
        IPluginLoader pluginLoader,
        IPluginVersionCoordinator versionCoordinator,
        IAgentRepository agentRepository,
        ILogger<PluginHotReloadService> logger)
    {
        _pluginRepository = pluginRepository;
        _versionRepository = versionRepository;
        _pluginLoader = pluginLoader;
        _versionCoordinator = versionCoordinator;
        _agentRepository = agentRepository;
        _logger = logger;
    }

    public async Task<PluginReloadResult> ReloadPluginAsync(
        Guid pluginId,
        CancellationToken cancellationToken)
    {
        try
        {
            _logger.LogInformation("Starting hot reload for plugin {PluginId}", pluginId);

            // Step 1: Retrieve plugin and validate
            var plugin = await _pluginRepository.GetByIdAsync(pluginId, cancellationToken);
            if (plugin == null)
            {
                return PluginReloadResult.Failure($"Plugin with ID '{pluginId}' not found");
            }

            // Step 2: Detect new version
            var currentVersion = await _versionRepository.GetActiveVersionAsync(pluginId, cancellationToken);
            var newVersion = await DetectNewVersionAsync(plugin, cancellationToken);

            if (newVersion == null)
            {
                return PluginReloadResult.Failure("No new version detected");
            }

            // Step 3: Version validation (SemVer comparison)
            var versionComparison = _versionCoordinator.CompareVersions(
                currentVersion?.Version,
                newVersion.Version
            );

            if (versionComparison == VersionComparison.Same)
            {
                return PluginReloadResult.Success("Plugin is already up-to-date", currentVersion);
            }

            // Step 4: Check for active agents using this plugin
            var activeAgents = await _agentRepository.GetAgentsUsingPluginAsync(pluginId, cancellationToken);
            if (activeAgents.Any())
            {
                _logger.LogInformation(
                    "Plugin {PluginId} is used by {AgentCount} active agents",
                    pluginId,
                    activeAgents.Count
                );

                // Decide reload strategy based on version change type
                var reloadStrategy = _versionCoordinator.DetermineReloadStrategy(versionComparison);

                if (reloadStrategy == ReloadStrategy.GracefulReload)
                {
                    // Gradual migration: New conversations use new version, existing continue with old
                    await PerformGracefulReloadAsync(plugin, newVersion, activeAgents, cancellationToken);
                }
                else if (reloadStrategy == ReloadStrategy.ImmediateReload)
                {
                    // Immediate reload: All agents switch to new version
                    await PerformImmediateReloadAsync(plugin, newVersion, activeAgents, cancellationToken);
                }
            }
            else
            {
                // No active agents, safe to reload immediately
                await UnloadAndLoadNewVersionAsync(plugin, currentVersion, newVersion, cancellationToken);
            }

            // Step 5: Update active version in database
            await _versionRepository.SetActiveVersionAsync(newVersion.PluginVersionId, cancellationToken);

            _logger.LogInformation(
                "Plugin {PluginId} hot reloaded successfully: {OldVersion} → {NewVersion}",
                pluginId,
                currentVersion?.Version ?? "None",
                newVersion.Version
            );

            return PluginReloadResult.Success("Plugin reloaded successfully", newVersion);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error during hot reload for plugin {PluginId}", pluginId);
            return PluginReloadResult.Failure($"Hot reload failed: {ex.Message}");
        }
    }

    public async Task<PluginReloadResult> ActivatePluginVersionAsync(
        Guid pluginId,
        string version,
        CancellationToken cancellationToken)
    {
        try
        {
            var pluginVersion = await _versionRepository.GetByVersionAsync(pluginId, version, cancellationToken);
            if (pluginVersion == null)
            {
                return PluginReloadResult.Failure($"Version '{version}' not found for plugin {pluginId}");
            }

            var currentVersion = await _versionRepository.GetActiveVersionAsync(pluginId, cancellationToken);
            var plugin = await _pluginRepository.GetByIdAsync(pluginId, cancellationToken);

            // Unload current version and load specified version
            await UnloadAndLoadNewVersionAsync(plugin, currentVersion, pluginVersion, cancellationToken);
            await _versionRepository.SetActiveVersionAsync(pluginVersion.PluginVersionId, cancellationToken);

            _logger.LogInformation(
                "Plugin {PluginId} activated to version {Version}",
                pluginId,
                version
            );

            return PluginReloadResult.Success($"Activated version {version}", pluginVersion);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error activating plugin version {PluginId}:{Version}", pluginId, version);
            return PluginReloadResult.Failure($"Version activation failed: {ex.Message}");
        }
    }

    public async Task<PluginReloadResult> RollbackPluginAsync(
        Guid pluginId,
        CancellationToken cancellationToken)
    {
        try
        {
            var currentVersion = await _versionRepository.GetActiveVersionAsync(pluginId, cancellationToken);
            if (currentVersion == null)
            {
                return PluginReloadResult.Failure("No active version to rollback from");
            }

            var previousVersion = await _versionRepository.GetPreviousVersionAsync(
                pluginId,
                currentVersion.Version,
                cancellationToken
            );

            if (previousVersion == null)
            {
                return PluginReloadResult.Failure("No previous version available for rollback");
            }

            // Perform rollback by activating previous version
            var rollbackResult = await ActivatePluginVersionAsync(
                pluginId,
                previousVersion.Version,
                cancellationToken
            );

            if (rollbackResult.Success)
            {
                _logger.LogInformation(
                    "Plugin {PluginId} rolled back: {CurrentVersion} → {PreviousVersion}",
                    pluginId,
                    currentVersion.Version,
                    previousVersion.Version
                );
            }

            return rollbackResult;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error rolling back plugin {PluginId}", pluginId);
            return PluginReloadResult.Failure($"Rollback failed: {ex.Message}");
        }
    }

    public async Task<List<ActivePluginInfo>> GetActivePluginsAsync(
        CancellationToken cancellationToken)
    {
        var loadedPlugins = _pluginLoader.GetLoadedPlugins();
        var activePluginInfo = new List<ActivePluginInfo>();

        foreach (var loadedPlugin in loadedPlugins)
        {
            var plugin = await _pluginRepository.GetByIdAsync(loadedPlugin.PluginId, cancellationToken);
            var activeVersion = await _versionRepository.GetActiveVersionAsync(loadedPlugin.PluginId, cancellationToken);

            activePluginInfo.Add(new ActivePluginInfo
            {
                PluginId = plugin.PluginId,
                PluginName = plugin.Name,
                CurrentVersion = activeVersion?.Version ?? "Unknown",
                LoadedAt = loadedPlugin.LoadedAt,
                Type = plugin.Type
            });
        }

        return activePluginInfo;
    }

    // Helper Methods
    private async Task<PluginVersion> DetectNewVersionAsync(
        Plugin plugin,
        CancellationToken cancellationToken)
    {
        // Check if file modification time is newer than last known version
        var fileInfo = new FileInfo(plugin.FilePath);
        if (!fileInfo.Exists)
        {
            return null;
        }

        var lastModified = fileInfo.LastWriteTimeUtc;
        var latestVersion = await _versionRepository.GetLatestVersionAsync(plugin.PluginId, cancellationToken);

        if (latestVersion == null || lastModified > latestVersion.CreatedAt)
        {
            // New version detected, parse version from assembly metadata or file
            var newVersionNumber = await ParseVersionFromFileAsync(plugin.FilePath, cancellationToken);

            return new PluginVersion
            {
                PluginVersionId = Guid.NewGuid(),
                PluginId = plugin.PluginId,
                Version = newVersionNumber,
                FilePath = plugin.FilePath,
                IsActive = false,
                CreatedAt = DateTime.UtcNow
            };
        }

        return null;
    }

    private async Task<string> ParseVersionFromFileAsync(
        string filePath,
        CancellationToken cancellationToken)
    {
        // For Native Plugins (.dll): Extract version from Assembly metadata
        if (filePath.EndsWith(".dll", StringComparison.OrdinalIgnoreCase))
        {
            var assembly = AssemblyLoadContext.Default.LoadFromAssemblyPath(filePath);
            var version = assembly.GetName().Version;
            return $"{version.Major}.{version.Minor}.{version.Build}";
        }
        // For Semantic Plugins (.yaml): Parse version from YAML metadata
        else if (filePath.EndsWith(".yaml", StringComparison.OrdinalIgnoreCase))
        {
            var yamlContent = await File.ReadAllTextAsync(filePath, cancellationToken);
            // Parse version field from YAML (implementation depends on YAML structure)
            return "1.0.0"; // Placeholder
        }

        return "1.0.0";
    }

    private async Task UnloadAndLoadNewVersionAsync(
        Plugin plugin,
        PluginVersion oldVersion,
        PluginVersion newVersion,
        CancellationToken cancellationToken)
    {
        // Unload old version if loaded
        if (oldVersion != null && _pluginLoader.IsPluginLoaded(plugin.PluginId))
        {
            await _pluginLoader.UnloadPluginAsync(plugin.PluginId, cancellationToken);
        }

        // Update plugin file path to new version
        plugin.UpdateFilePath(newVersion.FilePath);
        await _pluginRepository.UpdateAsync(plugin, cancellationToken);

        // Load new version
        await _pluginLoader.LoadPluginAsync(plugin, cancellationToken);
    }

    private async Task PerformGracefulReloadAsync(
        Plugin plugin,
        PluginVersion newVersion,
        List<Agent> activeAgents,
        CancellationToken cancellationToken)
    {
        // Graceful reload: Keep old version loaded, also load new version
        // New conversations use new version, existing conversations continue with old version
        _logger.LogInformation(
            "Performing graceful reload for plugin {PluginId} (Active agents: {AgentCount})",
            plugin.PluginId,
            activeAgents.Count
        );

        // Load new version without unloading old version
        var newPlugin = plugin.Clone();
        newPlugin.UpdateFilePath(newVersion.FilePath);
        await _pluginLoader.LoadPluginAsync(newPlugin, cancellationToken);

        // Schedule old version unload after grace period (e.g., 1 hour)
        // This would typically be handled by a background job
    }

    private async Task PerformImmediateReloadAsync(
        Plugin plugin,
        PluginVersion newVersion,
        List<Agent> activeAgents,
        CancellationToken cancellationToken)
    {
        // Immediate reload: Unload old version and load new version immediately
        // All agents switch to new version (may cause temporary disruption)
        _logger.LogInformation(
            "Performing immediate reload for plugin {PluginId} (Active agents: {AgentCount})",
            plugin.PluginId,
            activeAgents.Count
        );

        var currentVersion = await _versionRepository.GetActiveVersionAsync(plugin.PluginId, cancellationToken);
        await UnloadAndLoadNewVersionAsync(plugin, currentVersion, newVersion, cancellationToken);

        // Rebind all active agents to new version
        foreach (var agent in activeAgents)
        {
            agent.RebindPlugin(plugin.PluginId);
            await _agentRepository.UpdateAsync(agent, cancellationToken);
        }
    }
}

// PluginReloadResult
public class PluginReloadResult
{
    public bool Success { get; set; }
    public string Message { get; set; }
    public PluginVersion NewVersion { get; set; }

    public static PluginReloadResult Success(string message, PluginVersion newVersion)
    {
        return new PluginReloadResult
        {
            Success = true,
            Message = message,
            NewVersion = newVersion
        };
    }

    public static PluginReloadResult Failure(string message)
    {
        return new PluginReloadResult
        {
            Success = false,
            Message = message
        };
    }
}

// ActivePluginInfo
public class ActivePluginInfo
{
    public Guid PluginId { get; set; }
    public string PluginName { get; set; }
    public string CurrentVersion { get; set; }
    public DateTime LoadedAt { get; set; }
    public PluginType Type { get; set; }
}
```

### 4.2 FileSystemWatcher 實施

**FileSystemWatcher Service**:

```csharp
// IPluginFileWatcherService - Domain Layer Interface
public interface IPluginFileWatcherService
{
    void StartWatching();
    void StopWatching();
    event EventHandler<PluginFileChangedEventArgs> PluginFileChanged;
}

// PluginFileWatcherService - Infrastructure Layer
public class PluginFileWatcherService : IPluginFileWatcherService, IDisposable
{
    private readonly IConfiguration _configuration;
    private readonly ILogger<PluginFileWatcherService> _logger;
    private FileSystemWatcher _watcher;
    private readonly string _pluginDirectory;

    public event EventHandler<PluginFileChangedEventArgs> PluginFileChanged;

    public PluginFileWatcherService(
        IConfiguration configuration,
        ILogger<PluginFileWatcherService> logger)
    {
        _configuration = configuration;
        _logger = logger;
        _pluginDirectory = _configuration["PluginSettings:PluginDirectory"]
            ?? Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "Plugins");
    }

    public void StartWatching()
    {
        if (_watcher != null)
        {
            _logger.LogWarning("FileSystemWatcher is already running");
            return;
        }

        if (!Directory.Exists(_pluginDirectory))
        {
            Directory.CreateDirectory(_pluginDirectory);
        }

        _watcher = new FileSystemWatcher(_pluginDirectory)
        {
            Filter = "*.*",
            NotifyFilter = NotifyFilters.FileName | NotifyFilters.LastWrite | NotifyFilters.Size,
            IncludeSubdirectories = true,
            EnableRaisingEvents = true
        };

        _watcher.Changed += OnFileChanged;
        _watcher.Created += OnFileCreated;
        _watcher.Deleted += OnFileDeleted;
        _watcher.Renamed += OnFileRenamed;
        _watcher.Error += OnError;

        _logger.LogInformation("FileSystemWatcher started monitoring: {PluginDirectory}", _pluginDirectory);
    }

    public void StopWatching()
    {
        if (_watcher == null)
        {
            return;
        }

        _watcher.EnableRaisingEvents = false;
        _watcher.Changed -= OnFileChanged;
        _watcher.Created -= OnFileCreated;
        _watcher.Deleted -= OnFileDeleted;
        _watcher.Renamed -= OnFileRenamed;
        _watcher.Error -= OnError;
        _watcher.Dispose();
        _watcher = null;

        _logger.LogInformation("FileSystemWatcher stopped");
    }

    private void OnFileChanged(object sender, FileSystemEventArgs e)
    {
        if (IsPluginFile(e.FullPath))
        {
            _logger.LogInformation("Plugin file changed: {FilePath}", e.FullPath);
            RaisePluginFileChanged(e.FullPath, PluginFileChangeType.Modified);
        }
    }

    private void OnFileCreated(object sender, FileSystemEventArgs e)
    {
        if (IsPluginFile(e.FullPath))
        {
            _logger.LogInformation("Plugin file created: {FilePath}", e.FullPath);
            RaisePluginFileChanged(e.FullPath, PluginFileChangeType.Created);
        }
    }

    private void OnFileDeleted(object sender, FileSystemEventArgs e)
    {
        if (IsPluginFile(e.FullPath))
        {
            _logger.LogInformation("Plugin file deleted: {FilePath}", e.FullPath);
            RaisePluginFileChanged(e.FullPath, PluginFileChangeType.Deleted);
        }
    }

    private void OnFileRenamed(object sender, RenamedEventArgs e)
    {
        if (IsPluginFile(e.FullPath))
        {
            _logger.LogInformation("Plugin file renamed: {OldPath} → {NewPath}", e.OldFullPath, e.FullPath);
            RaisePluginFileChanged(e.FullPath, PluginFileChangeType.Renamed, e.OldFullPath);
        }
    }

    private void OnError(object sender, ErrorEventArgs e)
    {
        _logger.LogError(e.GetException(), "FileSystemWatcher error occurred");
    }

    private bool IsPluginFile(string filePath)
    {
        var extension = Path.GetExtension(filePath).ToLowerInvariant();
        return extension == ".dll" || extension == ".yaml";
    }

    private void RaisePluginFileChanged(string filePath, PluginFileChangeType changeType, string oldFilePath = null)
    {
        PluginFileChanged?.Invoke(this, new PluginFileChangedEventArgs
        {
            FilePath = filePath,
            OldFilePath = oldFilePath,
            ChangeType = changeType,
            Timestamp = DateTime.UtcNow
        });
    }

    public void Dispose()
    {
        StopWatching();
    }
}

// PluginFileChangedEventArgs
public class PluginFileChangedEventArgs : EventArgs
{
    public string FilePath { get; set; }
    public string OldFilePath { get; set; }
    public PluginFileChangeType ChangeType { get; set; }
    public DateTime Timestamp { get; set; }
}

public enum PluginFileChangeType
{
    Created,
    Modified,
    Deleted,
    Renamed
}
```

**Background Service for Hot Reload**:

```csharp
// PluginHotReloadBackgroundService - Infrastructure Layer
public class PluginHotReloadBackgroundService : BackgroundService
{
    private readonly IServiceProvider _serviceProvider;
    private readonly IPluginFileWatcherService _fileWatcherService;
    private readonly ILogger<PluginHotReloadBackgroundService> _logger;

    public PluginHotReloadBackgroundService(
        IServiceProvider serviceProvider,
        IPluginFileWatcherService fileWatcherService,
        ILogger<PluginHotReloadBackgroundService> logger)
    {
        _serviceProvider = serviceProvider;
        _fileWatcherService = fileWatcherService;
        _logger = logger;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        _logger.LogInformation("PluginHotReloadBackgroundService started");

        // Subscribe to file change events
        _fileWatcherService.PluginFileChanged += OnPluginFileChanged;
        _fileWatcherService.StartWatching();

        // Keep service running
        await Task.Delay(Timeout.Infinite, stoppingToken);
    }

    private async void OnPluginFileChanged(object sender, PluginFileChangedEventArgs e)
    {
        _logger.LogInformation(
            "Plugin file change detected: {ChangeType} - {FilePath}",
            e.ChangeType,
            e.FilePath
        );

        try
        {
            using var scope = _serviceProvider.CreateScope();
            var hotReloadService = scope.ServiceProvider.GetRequiredService<IPluginHotReloadService>();
            var pluginRepository = scope.ServiceProvider.GetRequiredService<IPluginRepository>();

            // Find plugin by file path
            var plugin = await pluginRepository.GetByFilePathAsync(e.FilePath);
            if (plugin == null)
            {
                _logger.LogWarning("No plugin found for file path: {FilePath}", e.FilePath);
                return;
            }

            // Trigger hot reload
            var result = await hotReloadService.ReloadPluginAsync(plugin.PluginId, CancellationToken.None);

            if (result.Success)
            {
                _logger.LogInformation("Hot reload successful: {Message}", result.Message);
            }
            else
            {
                _logger.LogWarning("Hot reload failed: {Message}", result.Message);
            }
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error processing plugin file change: {FilePath}", e.FilePath);
        }
    }

    public override Task StopAsync(CancellationToken cancellationToken)
    {
        _logger.LogInformation("PluginHotReloadBackgroundService stopping");
        _fileWatcherService.PluginFileChanged -= OnPluginFileChanged;
        _fileWatcherService.StopWatching();
        return base.StopAsync(cancellationToken);
    }
}
```

### 4.3 版本協調機制

**PluginVersionCoordinator Service**:

```csharp
// IPluginVersionCoordinator - Domain Layer Interface
public interface IPluginVersionCoordinator
{
    VersionComparison CompareVersions(string version1, string version2);
    ReloadStrategy DetermineReloadStrategy(VersionComparison versionComparison);
    bool IsVersionCompatible(string version1, string version2);
    Task<PluginVersionCompatibilityReport> AnalyzeCompatibilityAsync(
        PluginVersion oldVersion,
        PluginVersion newVersion,
        CancellationToken cancellationToken = default);
}

// PluginVersionCoordinator - Infrastructure Layer
public class PluginVersionCoordinator : IPluginVersionCoordinator
{
    private readonly ILogger<PluginVersionCoordinator> _logger;

    public PluginVersionCoordinator(ILogger<PluginVersionCoordinator> logger)
    {
        _logger = logger;
    }

    public VersionComparison CompareVersions(string version1, string version2)
    {
        if (string.IsNullOrEmpty(version1) || string.IsNullOrEmpty(version2))
        {
            return VersionComparison.Unknown;
        }

        var v1 = SemanticVersion.Parse(version1);
        var v2 = SemanticVersion.Parse(version2);

        if (v1.Major != v2.Major)
        {
            return v1.Major > v2.Major ? VersionComparison.MajorUpgrade : VersionComparison.MajorDowngrade;
        }

        if (v1.Minor != v2.Minor)
        {
            return v1.Minor > v2.Minor ? VersionComparison.MinorUpgrade : VersionComparison.MinorDowngrade;
        }

        if (v1.Patch != v2.Patch)
        {
            return v1.Patch > v2.Patch ? VersionComparison.PatchUpgrade : VersionComparison.PatchDowngrade;
        }

        return VersionComparison.Same;
    }

    public ReloadStrategy DetermineReloadStrategy(VersionComparison versionComparison)
    {
        return versionComparison switch
        {
            VersionComparison.PatchUpgrade => ReloadStrategy.ImmediateReload, // Bug fixes, safe to reload
            VersionComparison.MinorUpgrade => ReloadStrategy.GracefulReload,  // New features, gradual migration
            VersionComparison.MajorUpgrade => ReloadStrategy.GracefulReload,  // Breaking changes, careful migration
            VersionComparison.MajorDowngrade => ReloadStrategy.ManualReload,  // Downgrade requires manual intervention
            VersionComparison.MinorDowngrade => ReloadStrategy.ManualReload,
            VersionComparison.PatchDowngrade => ReloadStrategy.ManualReload,
            _ => ReloadStrategy.ManualReload
        };
    }

    public bool IsVersionCompatible(string version1, string version2)
    {
        var comparison = CompareVersions(version1, version2);

        // Compatible if:
        // - Same version
        // - Patch upgrade/downgrade within same major.minor
        // - Minor upgrade within same major
        return comparison switch
        {
            VersionComparison.Same => true,
            VersionComparison.PatchUpgrade => true,
            VersionComparison.PatchDowngrade => true,
            VersionComparison.MinorUpgrade => true,
            _ => false
        };
    }

    public async Task<PluginVersionCompatibilityReport> AnalyzeCompatibilityAsync(
        PluginVersion oldVersion,
        PluginVersion newVersion,
        CancellationToken cancellationToken)
    {
        var comparison = CompareVersions(oldVersion.Version, newVersion.Version);
        var isCompatible = IsVersionCompatible(oldVersion.Version, newVersion.Version);
        var reloadStrategy = DetermineReloadStrategy(comparison);

        var report = new PluginVersionCompatibilityReport
        {
            OldVersion = oldVersion.Version,
            NewVersion = newVersion.Version,
            VersionComparison = comparison,
            IsCompatible = isCompatible,
            RecommendedStrategy = reloadStrategy,
            Risks = IdentifyRisks(comparison),
            Recommendations = GenerateRecommendations(comparison, reloadStrategy)
        };

        return await Task.FromResult(report);
    }

    private List<string> IdentifyRisks(VersionComparison comparison)
    {
        var risks = new List<string>();

        if (comparison == VersionComparison.MajorUpgrade)
        {
            risks.Add("Major version upgrade may introduce breaking changes");
            risks.Add("Existing agent configurations may need adjustment");
        }
        else if (comparison == VersionComparison.MinorUpgrade)
        {
            risks.Add("New features may alter existing behavior");
        }
        else if (comparison == VersionComparison.MajorDowngrade ||
                 comparison == VersionComparison.MinorDowngrade)
        {
            risks.Add("Downgrade may remove features currently in use");
            risks.Add("Data compatibility issues may occur");
        }

        return risks;
    }

    private List<string> GenerateRecommendations(VersionComparison comparison, ReloadStrategy strategy)
    {
        var recommendations = new List<string>();

        if (strategy == ReloadStrategy.GracefulReload)
        {
            recommendations.Add("Use graceful reload to minimize disruption");
            recommendations.Add("Monitor active agents during transition period");
            recommendations.Add("Prepare rollback plan in case of issues");
        }
        else if (strategy == ReloadStrategy.ImmediateReload)
        {
            recommendations.Add("Immediate reload is safe for patch upgrades");
            recommendations.Add("Brief service disruption expected");
        }
        else if (strategy == ReloadStrategy.ManualReload)
        {
            recommendations.Add("Manual intervention required");
            recommendations.Add("Review change log and test thoroughly before upgrade");
            recommendations.Add("Schedule maintenance window for reload");
        }

        return recommendations;
    }
}

// SemanticVersion (from Sprint 3 Domain Layer)
public class SemanticVersion : ValueObject
{
    public int Major { get; private set; }
    public int Minor { get; private set; }
    public int Patch { get; private set; }

    public static SemanticVersion Parse(string version)
    {
        var parts = version.Split('.');
        if (parts.Length != 3)
            throw new ArgumentException("Invalid SemVer format. Expected: Major.Minor.Patch");

        return new SemanticVersion
        {
            Major = int.Parse(parts[0]),
            Minor = int.Parse(parts[1]),
            Patch = int.Parse(parts[2])
        };
    }

    public override string ToString() => $"{Major}.{Minor}.{Patch}";

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return Major;
        yield return Minor;
        yield return Patch;
    }
}

// Enums
public enum VersionComparison
{
    Unknown,
    Same,
    MajorUpgrade,
    MinorUpgrade,
    PatchUpgrade,
    MajorDowngrade,
    MinorDowngrade,
    PatchDowngrade
}

public enum ReloadStrategy
{
    ImmediateReload,   // Reload immediately, all agents switch to new version
    GracefulReload,    // Gradual migration, new conversations use new version
    ManualReload       // Require manual intervention and approval
}

// PluginVersionCompatibilityReport
public class PluginVersionCompatibilityReport
{
    public string OldVersion { get; set; }
    public string NewVersion { get; set; }
    public VersionComparison VersionComparison { get; set; }
    public bool IsCompatible { get; set; }
    public ReloadStrategy RecommendedStrategy { get; set; }
    public List<string> Risks { get; set; } = new();
    public List<string> Recommendations { get; set; } = new();
}
```

### 4.4 回退與錯誤處理

**Rollback 機制與錯誤恢復**:

已在 `PluginHotReloadService.RollbackPluginAsync()` 中實現 (參考 4.1 章節)。

**錯誤處理策略** ([Error Handling Strategy](../../docs/technical-implementation/01-backend-net9/09-error-handling-strategy.md)):

```csharp
// PluginReloadException - Custom Exception
public class PluginReloadException : Exception
{
    public Guid PluginId { get; set; }
    public string PluginVersion { get; set; }
    public ReloadFailureReason FailureReason { get; set; }

    public PluginReloadException(
        string message,
        Guid pluginId,
        string pluginVersion,
        ReloadFailureReason failureReason)
        : base(message)
    {
        PluginId = pluginId;
        PluginVersion = pluginVersion;
        FailureReason = failureReason;
    }

    public PluginReloadException(
        string message,
        Exception innerException,
        Guid pluginId,
        string pluginVersion,
        ReloadFailureReason failureReason)
        : base(message, innerException)
    {
        PluginId = pluginId;
        PluginVersion = pluginVersion;
        FailureReason = failureReason;
    }
}

public enum ReloadFailureReason
{
    FileNotFound,
    InvalidVersion,
    LoadFailure,
    UnloadFailure,
    CompatibilityIssue,
    ActiveAgentsBlocking,
    DatabaseError,
    UnknownError
}

// Resilience Policy (Using Polly)
public class PluginReloadResiliencePolicy
{
    private readonly ILogger<PluginReloadResiliencePolicy> _logger;

    public PluginReloadResiliencePolicy(ILogger<PluginReloadResiliencePolicy> logger)
    {
        _logger = logger;
    }

    public IAsyncPolicy<PluginReloadResult> GetReloadPolicy()
    {
        var retryPolicy = Policy<PluginReloadResult>
            .HandleResult(r => !r.Success && ShouldRetry(r))
            .WaitAndRetryAsync(
                retryCount: 3,
                sleepDurationProvider: attempt => TimeSpan.FromSeconds(Math.Pow(2, attempt)),
                onRetry: (outcome, timespan, retryAttempt, context) =>
                {
                    _logger.LogWarning(
                        "Plugin reload retry {RetryAttempt} after {Delay}ms: {Message}",
                        retryAttempt,
                        timespan.TotalMilliseconds,
                        outcome.Result.Message
                    );
                }
            );

        var fallbackPolicy = Policy<PluginReloadResult>
            .Handle<PluginReloadException>()
            .FallbackAsync(
                fallbackValue: PluginReloadResult.Failure("Reload failed after retries, rollback initiated"),
                onFallbackAsync: async (outcome, context) =>
                {
                    _logger.LogError(
                        outcome.Exception,
                        "Plugin reload failed, initiating rollback"
                    );

                    // Trigger automatic rollback on failure
                    await RollbackOnFailureAsync(outcome.Exception as PluginReloadException);
                }
            );

        return Policy.WrapAsync(fallbackPolicy, retryPolicy);
    }

    private bool ShouldRetry(PluginReloadResult result)
    {
        // Retry on transient failures, not on permanent failures
        return result.Message.Contains("temporary") ||
               result.Message.Contains("timeout") ||
               result.Message.Contains("busy");
    }

    private async Task RollbackOnFailureAsync(PluginReloadException exception)
    {
        // Trigger rollback logic here
        _logger.LogInformation(
            "Automatic rollback triggered for plugin {PluginId}",
            exception?.PluginId
        );

        // This would call the rollback service
        await Task.CompletedTask;
    }
}
```

---

## 五、完整文檔引用清單 (按功能分類)

### 5.1 規劃與策略文檔 (7 refs)

1. [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1A 範圍定義 (Sprint 4 在 MVP 中的定位)
2. [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 4 詳細分析 (13 SP, 3 週, Week 10-12)
3. [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Phase 1A 開發策略
4. [Dependency Matrix](../../1-planning/DEPENDENCY-MATRIX.md) - Sprint 4 依賴關係追蹤 (依賴 Sprint 3 Plugin System)
5. [Risk Register](../../1-planning/RISK-REGISTER.md) - RISK-008 (Persona 配置複雜度), RISK-009 (Plugin 熱重載穩定性)
6. [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - TD-012 (Persona 配置格式), TD-013 (Plugin 隔離), TD-014 (Prompt Engineering)
7. [Architecture Evolution Roadmap](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進路線圖

### 5.2 架構設計文檔 - ADRs (6 refs)

8. [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md) - 完整系統架構
9. [ADR-002: CQRS Pattern](../../docs/architecture/adr/ADR-002-cqrs-pattern.md) - CQRS 架構決策
10. [ADR-012: Persona Configuration Format](../../docs/architecture/adr/ADR-012-persona-configuration-format.md) - Persona 配置格式決策 (JSON/YAML 雙格式)
11. [ADR-013: Plugin Isolation Strategy](../../docs/architecture/adr/ADR-013-plugin-isolation-strategy.md) - Plugin 隔離策略 (AppDomain)
12. [ADR-014: Prompt Engineering Strategy](../../docs/architecture/adr/ADR-014-prompt-engineering-strategy.md) - Prompt 工程策略
13. [ADR-015: Plugin Hot Reload Strategy](../../docs/architecture/adr/ADR-015-plugin-hot-reload-strategy.md) - Plugin 熱重載策略

### 5.3 User Stories 與需求 (3 refs)

14. [User Stories Overview](../../docs/user-stories/README.md) - User Story 總覽
15. [Module 07: Persona Framework](../../docs/user-stories/modules/module-07-persona-framework.md) - Persona 系統完整規格 (US 7.1, US 7.2)
16. [Module 02: Plugin System](../../docs/user-stories/modules/module-02-plugin-system.md) - Plugin 系統完整規格 (US 2.2)

### 5.4 Backend 技術實作 (11 refs)

17. [Backend Architecture](../../docs/technical-implementation/01-backend-net9/README.md) - Backend 架構總覽
18. [Domain Layer](../../docs/technical-implementation/01-backend-net9/07-domain-layer.md) - Persona Entity 設計
19. [CQRS Implementation](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md) - MediatR 實作
20. [Validation Strategy](../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md) - FluentValidation 實作
21. [API Layer](../../docs/technical-implementation/01-backend-net9/08-api-layer.md) - API 控制器設計
22. [Persona Configuration Schema](../../docs/technical-implementation/01-backend-net9/13-persona-configuration-schema.md) - JSON Schema 設計
23. [Prompt Template Engine](../../docs/technical-implementation/01-backend-net9/14-prompt-template-engine.md) - Scriban 引擎實作
24. [Plugin Hot Reload Implementation](../../docs/technical-implementation/01-backend-net9/15-plugin-hot-reload-implementation.md) - 熱重載完整實作
25. [Plugin Versioning](../../docs/technical-implementation/01-backend-net9/16-plugin-versioning.md) - Plugin 版本管理實作
26. [Repository Pattern](../../docs/technical-implementation/01-backend-net9/06-repository-pattern.md) - Repository 實作
27. [Error Handling Strategy](../../docs/technical-implementation/01-backend-net9/09-error-handling-strategy.md) - 錯誤處理策略

### 5.5 Frontend 技術實作 (6 refs)

28. [Frontend Architecture](../../docs/technical-implementation/02-frontend-react/01-frontend-architecture.md) - React 架構設計
29. [Component Design](../../docs/technical-implementation/02-frontend-react/02-component-design.md) - 組件設計規範
30. [State Management](../../docs/technical-implementation/02-frontend-react/03-state-management.md) - Zustand + React Query
31. [Form Validation](../../docs/technical-implementation/02-frontend-react/04-form-validation.md) - 表單驗證策略
32. [Data Fetching Strategy](../../docs/technical-implementation/02-frontend-react/05-data-fetching.md) - React Query 實作
33. [API Integration](../../docs/technical-implementation/02-frontend-react/06-api-integration.md) - API 調用封裝

### 5.6 UX 設計文檔 (5 refs)

34. [Design System](../../docs/ux-design/design-system/README.md) - Material-UI 設計系統
35. [Component Library](../../docs/ux-design/design-system/component-library.md) - 組件規範
36. [Persona Builder Wireframe](../../docs/ux-design/wireframes/low-fidelity/10-persona-builder.md) - Persona 建構器線框圖
37. [Persona Template Gallery Wireframe](../../docs/ux-design/wireframes/low-fidelity/11-persona-template-gallery.md) - Persona 模板庫線框圖
38. [Form Components](../../docs/ux-design/design-system/form-components.md) - 表單組件設計

### 5.7 API 與資料庫設計 (2 refs)

39. [API Design Specification](../../docs/api/api-design.md) - RESTful API 完整設計 (Persona 與 Plugin 端點)
40. [Database Schema Design](../../docs/api/database-schema.md) - PostgreSQL Schema 設計 (personas, persona_templates, plugin_versions 表)

### 5.8 測試文檔 (7 refs)

41. [Testing Strategy](../../docs/testing/testing-strategy.md) - 完整測試策略
42. [Unit Testing Guidelines](../../docs/testing/unit-testing-guidelines.md) - 單元測試規範
43. [Integration Testing Guidelines](../../docs/testing/integration-testing-guidelines.md) - 集成測試規範
44. [E2E Testing Guidelines](../../docs/testing/e2e-testing-guidelines.md) - E2E 測試規範
45. [Persona Testing Guidelines](../../docs/testing/persona-testing-guidelines.md) - Persona 系統測試規範
46. [Plugin Hot Reload Testing](../../docs/testing/plugin-hot-reload-testing.md) - 熱重載測試規範
47. [Prompt Generation Testing](../../docs/testing/prompt-generation-testing.md) - Prompt 生成測試規範

### 5.9 開發標準 (6 refs)

48. [Code Quality Standards](../../docs/development-standards/code-quality-standards.md) - 代碼質量標準
49. [Coding Conventions](../../docs/development-standards/coding-conventions.md) - 編碼規範
50. [Code Review Checklist](../../docs/development-standards/code-review-checklist.md) - Code Review 檢查清單
51. [Git Workflow](../../docs/development-standards/git-workflow.md) - Git 分支策略
52. [Documentation Standards](../../docs/development-standards/documentation-standards.md) - 文檔規範
53. [Technical Debt Management](../../docs/development-standards/technical-debt-management.md) - 技術債務管理

### 5.10 專案管理 (3 refs)

54. [Sprint Planning Guide](../../docs/project-management/sprint-planning-guide.md) - Sprint 規劃指南
55. [Definition of Done](../../docs/project-management/definition-of-done.md) - 完成定義
56. [Agile Practices](../../docs/project-management/agile-practices.md) - 敏捷實踐

---

## 📋 使用指南

### 適用對象
- **開發團隊**: 查詢技術實施細節、設計規範、最佳實踐
- **架構師**: 驗證技術決策、架構一致性
- **AI 助手**: Just-in-Time 技術查詢、開發指導
- **QA 團隊**: 理解實施細節、驗證測試策略

### 如何使用
1. **開發前查詢**: 開始實施 User Story 前，先查詢相關章節
2. **技術決策**: 遇到技術選擇時，參考 ADR 和最佳實踐
3. **問題排查**: 遇到技術問題時，查詢相關實施細節
4. **架構驗證**: Code Review 時驗證是否符合架構規範

### 更新標準
- **技術決策變更時**: 更新對應的 ADR 引用和實施指南
- **新增最佳實踐時**: 補充到對應章節
- **發現文檔錯誤時**: 立即修正確保準確性

### 相關文檔
- [SPRINT-4-OVERVIEW.md](./SPRINT-4-OVERVIEW.md) - Sprint 4 概覽
- [SPRINT-4-PLAN.md](./SPRINT-4-PLAN.md) - 執行計劃
- [SPRINT-4-CHECKLIST.md](./SPRINT-4-CHECKLIST.md) - 檢查清單
- [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md) - 架構設計文檔
- [ADR Documentation](../../docs/architecture/adr/) - 技術決策記錄

---

## 📋 更新日誌

| 版本 | 日期 | 變更內容 | 負責人 |
|------|------|---------|--------|
| 2.1 | 2025-11-13 | 升級至 v2.1 統一標準：標準化 8 欄位 Header、完整重構文檔結構、新增核心技術參考層、新增三個 US 的詳細技術上下文（US 7.1, US 7.2, US 2.2）、新增 56+ 個技術參考、新增使用指南與目錄、擴充至 ~1,500 行以匹配 Sprint 1-3 深度 | Documentation Team |
| 1.0 | 2025-11-13 | 初版創建 - 提供 Sprint 4 基礎技術上下文（239 行，結構簡化版） | Documentation Team |

### v2.1 升級內容
- ✅ 標準化 8 欄位 Header (版本、Sprint 編號、週期、Phase、計劃日期、狀態、創建/更新日期)
- ✅ 新增完整目錄 (5個主要章節、15個子章節)
- ✅ 新增使用指南 (適用對象、使用方法、更新標準、相關文檔)
- ✅ 新增「一、核心技術參考層」- 完整 Module/Epic/ADR/API/UI/測試引用 (1.1-1.5)
- ✅ 新增「二、US 7.1: Persona 模板配置 - 詳細技術上下文」(2.1-2.5)
- ✅ 新增「三、US 7.2: Persona-Driven Prompt Engineering - 詳細技術上下文」(3.1-3.4)
- ✅ 新增「四、US 2.2: Plugin 熱重載機制 - 詳細技術上下文」(4.1-4.4)
- ✅ 新增「五、完整文檔引用清單」- 56+ 個技術參考，涵蓋 10 個主要類別
- ✅ 保留 100% 原有內容 + 新增詳細代碼範例與實施指南
- ✅ 文檔長度擴充至 ~1,500 行，匹配 Sprint 1 (1,690 lines) 和 Sprint 3 (2,371 lines) 的深度

### v1.0 原始內容
- 基礎 Sprint 背景與技術決策 (TD-012, TD-013)
- 簡化架構考量 (Persona Framework 架構、Plugin 熱重載架構)
- 依賴分析 (技術依賴、資料依賴)
- 風險管理 (RISK-008, RISK-009)
- 基礎參考文檔列表

### 維護說明
此文檔為 Sprint 4 技術上下文參考手冊，提供 56+ 個技術參考連結與詳細實施指導，支持 "Just-in-Time" 技術查詢場景。開發團隊在實施 User Story 前應先查詢對應章節，確保技術決策與架構一致性。

**使用建議**:
- 開發前: 閱讀對應章節的「MVP 範圍定義」與「關鍵技術決策」
- 開發中: 查詢「實施細節」章節的代碼範例與技術參考
- 遇到問題: 使用「完整文檔引用清單」快速定位相關技術文檔

---

**文檔版本**: v2.1
**最後更新**: 2025-11-13
**維護者**: Sprint 4 開發團隊
