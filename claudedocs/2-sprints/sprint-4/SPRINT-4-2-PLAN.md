# SPRINT-4-2-PLAN.md - Sprint 4 計劃書：Persona Framework 與 Plugin 熱重載實施計劃

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

1. [規劃文檔參考](#規劃文檔參考)
2. [第一部分：本 Sprint 要建立什麼](#第一部分本-sprint-要建立什麼-what-to-build)
3. [第二部分：技術實施方案](#第二部分技術實施方案-how-to-build)
4. [第三部分：編碼規範](#第三部分編碼規範)
5. [第四部分：質量保證](#第四部分質量保證)
6. [第五部分：參考文檔](#第五部分參考文檔)
7. [使用指南](#使用指南)
8. [版本歷史](#版本歷史)

---

## 規劃文檔參考

**規劃文檔參考**:
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1A 範圍
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-4) - Sprint 4 分析
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Persona 系統策略
- 📐 [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md) - 系統架構

---

## 第一部分：本 Sprint 要建立什麼 (What to Build)

### US 7.1: Persona 模板配置 (5 SP)

**User Story 完整規格**: [US 7.1 - Persona 模板配置](../../docs/user-stories/modules/module-07-persona-framework.md#us-71)

#### 一、MVP 範圍定義

**必須實現功能 (P0 - 本 Sprint)**:
- [x] **Persona 配置載入器**: 支援 JSON/YAML 格式
  - 載入 Persona 配置檔
  - 解析配置結構
  - 驗證配置正確性
  - 配置熱重載（開發模式）
  - **參考**: [Persona Framework](../../docs/technical-implementation/01-backend-net9/03-persona-framework.md)

- [x] **10 種預設 Persona 模板**: 開箱即用的模板
  - 專業顧問 (Professional Consultant)
  - 技術專家 (Technical Expert)
  - 客服助理 (Customer Support)
  - 創意夥伴 (Creative Partner)
  - 數據分析師 (Data Analyst)
  - 教育導師 (Educational Mentor)
  - 研究助理 (Research Assistant)
  - 產品經理 (Product Manager)
  - 銷售顧問 (Sales Consultant)
  - 法律顧問 (Legal Advisor)
  - **參考**: [Persona Templates](../../docs/persona-templates/)

- [x] **Schema 驗證機制**: JSON Schema / YAML Schema
  - 配置格式驗證
  - 必填欄位檢查
  - 數值範圍驗證
  - 枚舉值驗證
  - **參考**: [Validation Strategy](../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md)

- [x] **配置一致性檢測**: 防止衝突配置
  - 風格一致性檢查（例如：professional + casual 衝突）
  - 禁止詞彙清單驗證
  - 安全守則檢查
  - **參考**: [Persona Configuration Validation](../../docs/technical-implementation/01-backend-net9/03-persona-framework.md#validation)

**明確排除 Phase 2 功能 (延後)**:
- ❌ **Persona A/B 測試**: 延後到 Sprint 5
- ❌ **Persona 動態調整**: 延後到 Sprint 5
- ❌ **Persona 一致性評分**: 延後到 Sprint 6
- ❌ **多語言 Persona 支援**: 延後到 Phase 2

**MVP 範圍參考**:
- 📖 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md#persona-framework) - Persona 系統在 Phase 1A 的範圍
- 📖 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-4) - Sprint 4 任務分配

#### 二、詳細技術規格

##### Persona 配置格式規範

**YAML 配置範例**:
```yaml
# persona-config.yaml
persona:
  name: "Alice"
  role: "技術顧問"
  expertise:
    - "雲端架構"
    - "DevOps"
    - "安全性"

  communication_style:
    formality: "professional"      # professional, casual, friendly
    verbosity: "balanced"           # concise, balanced, detailed
    tone: "encouraging"             # neutral, encouraging, assertive

  response_pattern:
    proactivity: "moderate"         # passive, moderate, proactive
    style: "exploratory"            # directive, exploratory, collaborative

  language_preferences:
    technical_terms: "always_explain"  # use_freely, explain_when_complex, always_explain
    examples: "provide_when_helpful"   # minimal, provide_when_helpful, abundant

  safety_guardrails:
    forbidden_topics:
      - "政治"
      - "宗教"
    forbidden_words:
      - "絕對"
      - "保證"
      - "一定"
    max_speculation: "low"          # none, low, moderate

  personality_traits:
    - "耐心解釋複雜概念"
    - "使用類比和實例"
    - "鼓勵最佳實踐"
    - "避免過度技術術語"
```

**JSON 配置範例**:
```json
{
  "persona": {
    "name": "Alice",
    "role": "技術顧問",
    "expertise": ["雲端架構", "DevOps", "安全性"],
    "communication_style": {
      "formality": "professional",
      "verbosity": "balanced",
      "tone": "encouraging"
    },
    "response_pattern": {
      "proactivity": "moderate",
      "style": "exploratory"
    },
    "language_preferences": {
      "technical_terms": "always_explain",
      "examples": "provide_when_helpful"
    },
    "safety_guardrails": {
      "forbidden_topics": ["政治", "宗教"],
      "forbidden_words": ["絕對", "保證", "一定"],
      "max_speculation": "low"
    },
    "personality_traits": [
      "耐心解釋複雜概念",
      "使用類比和實例",
      "鼓勵最佳實踐",
      "避免過度技術術語"
    ]
  }
}
```

**JSON Schema 驗證規範**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["persona"],
  "properties": {
    "persona": {
      "type": "object",
      "required": ["name", "role", "communication_style"],
      "properties": {
        "name": {
          "type": "string",
          "minLength": 2,
          "maxLength": 50,
          "description": "Persona 名稱"
        },
        "role": {
          "type": "string",
          "minLength": 2,
          "maxLength": 100,
          "description": "Persona 角色"
        },
        "expertise": {
          "type": "array",
          "items": { "type": "string" },
          "minItems": 1,
          "maxItems": 10,
          "description": "專業領域清單"
        },
        "communication_style": {
          "type": "object",
          "required": ["formality", "verbosity", "tone"],
          "properties": {
            "formality": {
              "type": "string",
              "enum": ["professional", "casual", "friendly"]
            },
            "verbosity": {
              "type": "string",
              "enum": ["concise", "balanced", "detailed"]
            },
            "tone": {
              "type": "string",
              "enum": ["neutral", "encouraging", "assertive"]
            }
          }
        },
        "response_pattern": {
          "type": "object",
          "properties": {
            "proactivity": {
              "type": "string",
              "enum": ["passive", "moderate", "proactive"]
            },
            "style": {
              "type": "string",
              "enum": ["directive", "exploratory", "collaborative"]
            }
          }
        },
        "safety_guardrails": {
          "type": "object",
          "properties": {
            "forbidden_topics": {
              "type": "array",
              "items": { "type": "string" }
            },
            "forbidden_words": {
              "type": "array",
              "items": { "type": "string" }
            },
            "max_speculation": {
              "type": "string",
              "enum": ["none", "low", "moderate"]
            }
          }
        }
      }
    }
  }
}
```

**Schema 驗證參考**:
- 📄 [JSON Schema Specification](https://json-schema.org/) - JSON Schema 官方規範
- 📄 [Validation Strategy](../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md) - FluentValidation 實作

##### Backend API 規格

**API 端點**: `POST /api/v1/personas`

**Request**:
```json
{
  "name": "Alice",
  "role": "技術顧問",
  "description": "專精於雲端架構與 DevOps 的技術顧問",
  "configJson": "{\"persona\": {...}}",  // 完整 Persona 配置 JSON
  "isTemplate": false,
  "templateId": null
}
```

**Response** (201 Created):
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Alice",
  "role": "技術顧問",
  "description": "專精於雲端架構與 DevOps 的技術顧問",
  "configJson": "{\"persona\": {...}}",
  "isTemplate": false,
  "templateId": null,
  "validationStatus": "Valid",
  "validationErrors": [],
  "createdAt": "2025-12-16T10:00:00Z",
  "updatedAt": "2025-12-16T10:00:00Z"
}
```

**驗證規則**:
- `name`: 必填, 長度 2-50 字符, 不可重複
- `role`: 必填, 長度 2-100 字符
- `configJson`: 必填, 必須是有效的 JSON, 符合 Persona Schema
- `communication_style.formality`: 必填, 枚舉值 ["professional", "casual", "friendly"]
- `communication_style.verbosity`: 必填, 枚舉值 ["concise", "balanced", "detailed"]

**錯誤處理**:
- `400 Bad Request`: 驗證失敗 (JSON 格式錯誤、Schema 不符)
- `409 Conflict`: Persona 名稱已存在
- `422 Unprocessable Entity`: 配置衝突（例如：professional + casual）
- `500 Internal Server Error`: 服務器錯誤

**錯誤處理參考**:
- 📄 [Error Handling Strategy](../../docs/api/api-design.md#error-handling) - 統一錯誤處理
- 📄 [HTTP Status Codes](../../docs/api/api-design.md#http-status-codes) - 狀態碼規範

##### Database Schema

**Table**: `personas`

```sql
CREATE TABLE personas (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(50) NOT NULL,
    role VARCHAR(100) NOT NULL,
    description TEXT,
    config_json JSONB NOT NULL,
    is_template BOOLEAN DEFAULT false,
    template_id UUID REFERENCES persona_templates(id),
    validation_status VARCHAR(20) NOT NULL DEFAULT 'Valid', -- Valid, Invalid, Warning
    validation_errors JSONB,
    is_active BOOLEAN DEFAULT true,
    is_deleted BOOLEAN DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_by VARCHAR(100),

    CONSTRAINT uk_personas_name UNIQUE(name),
    CONSTRAINT ck_validation_status CHECK (validation_status IN ('Valid', 'Invalid', 'Warning'))
);

-- Indexes
CREATE INDEX idx_personas_name ON personas(name);
CREATE INDEX idx_personas_role ON personas(role);
CREATE INDEX idx_personas_is_template ON personas(is_template);
CREATE INDEX idx_personas_is_active ON personas(is_active);
CREATE INDEX idx_personas_is_deleted ON personas(is_deleted);
CREATE INDEX idx_personas_created_at ON personas(created_at DESC);

-- JSONB Index for fast queries on config
CREATE INDEX idx_personas_config_json ON personas USING GIN (config_json);
```

**Table**: `persona_templates`

```sql
CREATE TABLE persona_templates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(50) NOT NULL UNIQUE,
    category VARCHAR(50) NOT NULL, -- Professional, Technical, Support, Creative, etc.
    description TEXT,
    config_json JSONB NOT NULL,
    preview_prompt TEXT,
    usage_count INT DEFAULT 0,
    is_builtin BOOLEAN DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_persona_templates_category ON persona_templates(category);
CREATE INDEX idx_persona_templates_is_builtin ON persona_templates(is_builtin);
```

**Database 設計參考**:
- 🗄️ [Database Schema Design](../../docs/api/database-schema.md) - 完整 Schema 設計
- 🗄️ [JSONB in PostgreSQL](https://www.postgresql.org/docs/current/datatype-json.html) - JSONB 使用指南
- 🗄️ [ADR-004: PostgreSQL Selection](../../docs/architecture/adr/ADR-004-database-selection.md) - 資料庫選型

##### C# Domain Entity 設計

```csharp
// Domain/Entities/Persona.cs
public sealed class Persona : BaseEntity
{
    // Properties
    public Guid Id { get; private set; }
    public string Name { get; private set; }
    public string Role { get; private set; }
    public string? Description { get; private set; }
    public PersonaConfig Config { get; private set; }
    public bool IsTemplate { get; private set; }
    public Guid? TemplateId { get; private set; }
    public ValidationStatus ValidationStatus { get; private set; }
    public List<string> ValidationErrors { get; private set; }
    public bool IsActive { get; private set; }

    // Factory Method
    public static Persona Create(
        Guid userId,
        string name,
        string role,
        PersonaConfig config,
        string? description = null,
        bool isTemplate = false,
        Guid? templateId = null)
    {
        // Validation
        if (string.IsNullOrWhiteSpace(name))
            throw new ArgumentException("Persona name is required", nameof(name));

        if (string.IsNullOrWhiteSpace(role))
            throw new ArgumentException("Persona role is required", nameof(role));

        var persona = new Persona
        {
            Id = Guid.NewGuid(),
            Name = name,
            Role = role,
            Description = description,
            Config = config ?? throw new ArgumentNullException(nameof(config)),
            IsTemplate = isTemplate,
            TemplateId = templateId,
            ValidationStatus = ValidationStatus.Valid,
            ValidationErrors = new List<string>(),
            IsActive = true,
            CreatedBy = userId.ToString(),
            CreatedAt = DateTime.UtcNow,
            UpdatedAt = DateTime.UtcNow
        };

        // Validate configuration
        var validationResult = persona.ValidateConfig();
        persona.ValidationStatus = validationResult.IsValid ? ValidationStatus.Valid : ValidationStatus.Invalid;
        persona.ValidationErrors = validationResult.Errors;

        // Domain Event
        persona.RaiseDomainEvent(new PersonaCreatedDomainEvent(persona.Id, persona.Name));

        return persona;
    }

    // Domain Methods
    public void UpdateConfig(PersonaConfig newConfig)
    {
        Config = newConfig ?? throw new ArgumentNullException(nameof(newConfig));

        // Re-validate
        var validationResult = ValidateConfig();
        ValidationStatus = validationResult.IsValid ? ValidationStatus.Valid : ValidationStatus.Invalid;
        ValidationErrors = validationResult.Errors;

        UpdatedAt = DateTime.UtcNow;
        RaiseDomainEvent(new PersonaConfigUpdatedDomainEvent(Id, Name));
    }

    public void Activate()
    {
        if (ValidationStatus == ValidationStatus.Invalid)
            throw new InvalidOperationException("Cannot activate a Persona with invalid configuration");

        IsActive = true;
        UpdatedAt = DateTime.UtcNow;
    }

    public void Deactivate()
    {
        IsActive = false;
        UpdatedAt = DateTime.UtcNow;
    }

    private ValidationResult ValidateConfig()
    {
        var errors = new List<string>();

        // Check communication style consistency
        if (Config.CommunicationStyle.Formality == Formality.Professional &&
            Config.CommunicationStyle.Tone == Tone.Casual)
        {
            errors.Add("Professional formality conflicts with casual tone");
        }

        // Check forbidden words in personality traits
        foreach (var trait in Config.PersonalityTraits)
        {
            foreach (var forbiddenWord in Config.SafetyGuardrails.ForbiddenWords)
            {
                if (trait.Contains(forbiddenWord, StringComparison.OrdinalIgnoreCase))
                {
                    errors.Add($"Personality trait contains forbidden word: '{forbiddenWord}'");
                }
            }
        }

        return new ValidationResult(errors.Count == 0, errors);
    }
}

// Domain/ValueObjects/PersonaConfig.cs
public sealed class PersonaConfig : ValueObject
{
    public string Name { get; private set; }
    public string Role { get; private set; }
    public List<string> Expertise { get; private set; }
    public CommunicationStyle CommunicationStyle { get; private set; }
    public ResponsePattern ResponsePattern { get; private set; }
    public LanguagePreferences LanguagePreferences { get; private set; }
    public SafetyGuardrails SafetyGuardrails { get; private set; }
    public List<string> PersonalityTraits { get; private set; }

    public static PersonaConfig FromJson(string json)
    {
        // Parse JSON and create PersonaConfig
        var jsonDoc = JsonDocument.Parse(json);
        var personaElement = jsonDoc.RootElement.GetProperty("persona");

        return new PersonaConfig
        {
            Name = personaElement.GetProperty("name").GetString()!,
            Role = personaElement.GetProperty("role").GetString()!,
            Expertise = personaElement.GetProperty("expertise")
                .EnumerateArray()
                .Select(e => e.GetString()!)
                .ToList(),
            CommunicationStyle = CommunicationStyle.FromJson(personaElement.GetProperty("communication_style")),
            ResponsePattern = ResponsePattern.FromJson(personaElement.GetProperty("response_pattern")),
            LanguagePreferences = LanguagePreferences.FromJson(personaElement.GetProperty("language_preferences")),
            SafetyGuardrails = SafetyGuardrails.FromJson(personaElement.GetProperty("safety_guardrails")),
            PersonalityTraits = personaElement.GetProperty("personality_traits")
                .EnumerateArray()
                .Select(t => t.GetString()!)
                .ToList()
        };
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return Name;
        yield return Role;
        yield return string.Join(",", Expertise);
    }
}

// Domain/ValueObjects/CommunicationStyle.cs
public sealed class CommunicationStyle : ValueObject
{
    public Formality Formality { get; private set; }
    public Verbosity Verbosity { get; private set; }
    public Tone Tone { get; private set; }

    public static CommunicationStyle FromJson(JsonElement json)
    {
        return new CommunicationStyle
        {
            Formality = Enum.Parse<Formality>(json.GetProperty("formality").GetString()!, ignoreCase: true),
            Verbosity = Enum.Parse<Verbosity>(json.GetProperty("verbosity").GetString()!, ignoreCase: true),
            Tone = Enum.Parse<Tone>(json.GetProperty("tone").GetString()!, ignoreCase: true)
        };
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return Formality;
        yield return Verbosity;
        yield return Tone;
    }
}

// Domain/Enums
public enum Formality { Professional, Casual, Friendly }
public enum Verbosity { Concise, Balanced, Detailed }
public enum Tone { Neutral, Encouraging, Assertive }
public enum Proactivity { Passive, Moderate, Proactive }
public enum ResponseStyle { Directive, Exploratory, Collaborative }
public enum TechnicalTermUsage { UseFreely, ExplainWhenComplex, AlwaysExplain }
public enum ExampleUsage { Minimal, ProvideWhenHelpful, Abundant }
public enum SpeculationLevel { None, Low, Moderate }
public enum ValidationStatus { Valid, Invalid, Warning }
```

**Domain Layer 設計參考**:
- 📐 [Domain Layer Design](../../docs/technical-implementation/01-backend-net9/07-domain-layer.md)
- 📐 [Value Objects Pattern](../../docs/architecture/adr/ADR-005-value-objects.md)
- 📐 [Domain Events](../../docs/technical-implementation/01-backend-net9/07-domain-layer.md#domain-events)

### US 7.2: Persona-Driven Prompt Engineering (5 SP)

**目標**: 根據 Persona 配置動態生成 System Prompt，實現個性化 AI 行為

#### US 7.2.1: MVP 範圍定義

**Must Have (Phase 1)**:
- ✅ **Prompt 模板引擎**: Liquid/Handlebars 模板語法支援，變數插值與條件渲染
- ✅ **Persona 行為注入**: 根據 `CommunicationStyle`, `ResponsePattern`, `Expertise` 動態生成指令
- ✅ **安全守則注入**: 自動注入 `forbidden_topics`, `forbidden_words`, 輸出格式限制
- ✅ **基礎模板管理**: 10 個內建模板 (對應 10 種 Persona 類型)
- ✅ **動態 Prompt 組合**: 系統指令 + Persona 指令 + 安全規則 + 使用者訊息
- ✅ **Token 限制管理**: 自動截斷過長的 Prompt，保留核心指令

**Excluded (Phase 2)**:
- ❌ 使用者自定義模板 (Phase 2: US 8.1)
- ❌ 模板版本控制與回滾 (Phase 2)
- ❌ A/B Testing 與模板效果分析 (Phase 2)
- ❌ 多語言模板支援 (Phase 2)

#### US 7.2.2: Prompt 模板格式

**Liquid 模板範例**:
```liquid
# System Role
You are {{ persona.name }}, a {{ persona.role }}.

# Expertise Areas
Your areas of expertise include:
{% for area in persona.expertise %}
- {{ area }}
{% endfor %}

# Communication Style
- Formality: {{ persona.communication_style.formality }}
- Verbosity: {{ persona.communication_style.verbosity }}
- Tone: {{ persona.communication_style.tone }}

# Response Patterns
{% if persona.response_pattern.use_examples %}
Always provide concrete examples to illustrate your points.
{% endif %}

{% if persona.response_pattern.step_by_step %}
Break down complex problems into step-by-step solutions.
{% endif %}

# Safety Guardrails
## Forbidden Topics
{% for topic in persona.safety_guardrails.forbidden_topics %}
- Do not discuss: {{ topic }}
{% endfor %}

## Output Restrictions
- Language: {{ persona.safety_guardrails.language_restriction }}
- Max response length: {{ persona.safety_guardrails.max_response_length }} tokens

# Your Task
{{ user_message }}
```

**生成的 System Prompt 範例**:
```
# System Role
You are Alice, a Cloud Architect.

# Expertise Areas
Your areas of expertise include:
- AWS Architecture
- Kubernetes
- DevOps Best Practices

# Communication Style
- Formality: professional
- Verbosity: concise
- Tone: encouraging

# Response Patterns
Always provide concrete examples to illustrate your points.
Break down complex problems into step-by-step solutions.

# Safety Guardrails
## Forbidden Topics
- Do not discuss: politics
- Do not discuss: religion

## Output Restrictions
- Language: zh-TW
- Max response length: 2000 tokens

# Your Task
How do I design a highly available Kubernetes cluster?
```

#### US 7.2.3: Prompt 生成演算法

**生成流程**:
```yaml
Step 1: 載入 Persona 配置
  - 從資料庫讀取 Persona Entity
  - 反序列化 PersonaConfig Value Object

Step 2: 選擇模板
  - 根據 Persona.Role 選擇對應模板
  - 支援 Template Override (Persona 可指定自定義模板)

Step 3: 變數注入
  - 將 PersonaConfig 所有屬性映射為模板變數
  - 額外注入 system_time, user_context 等運行時變數

Step 4: 模板渲染
  - 使用 Liquid Template Engine 渲染
  - 處理條件邏輯、迴圈、過濾器

Step 5: 安全檢查與截斷
  - 驗證生成的 Prompt 不含敏感資訊
  - Token Counting (使用 TikToken)
  - 超過限制時智能截斷 (保留核心指令 + 部分 context)

Step 6: 快取與返回
  - 快取生成的 Prompt (Redis, TTL 1 hour)
  - 返回最終 Prompt 給 Agent Runtime
```

**C# 演算法實作**:
```csharp
public sealed class PromptGenerationService : IPromptGenerationService
{
    private readonly IPromptTemplateRepository _templateRepo;
    private readonly IFluidTemplateEngine _liquidEngine;
    private readonly ITokenCounter _tokenCounter;
    private readonly IDistributedCache _cache;
    private readonly ILogger<PromptGenerationService> _logger;

    public async Task<Result<GeneratedPrompt>> GeneratePromptAsync(
        Guid personaId,
        string userMessage,
        CancellationToken cancellationToken = default)
    {
        try
        {
            // Step 1: 載入 Persona 配置
            var persona = await _personaRepo.GetByIdAsync(personaId, cancellationToken);
            if (persona is null)
                return Result<GeneratedPrompt>.Failure("Persona not found");

            // Step 2: 選擇模板
            var templateKey = persona.Config.TemplateOverride
                ?? GetDefaultTemplateKey(persona.Role);
            var template = await _templateRepo.GetByKeyAsync(templateKey, cancellationToken);

            // Step 3: 變數注入
            var variables = new Dictionary<string, object>
            {
                ["persona"] = new
                {
                    name = persona.Name,
                    role = persona.Role,
                    expertise = persona.Config.Expertise,
                    communication_style = persona.Config.CommunicationStyle,
                    response_pattern = persona.Config.ResponsePattern,
                    safety_guardrails = persona.Config.SafetyGuardrails
                },
                ["user_message"] = userMessage,
                ["system_time"] = DateTime.UtcNow.ToString("o")
            };

            // Step 4: 模板渲染
            var renderedPrompt = await _liquidEngine.RenderAsync(
                template.Content,
                variables,
                cancellationToken);

            // Step 5: 安全檢查與截斷
            var tokenCount = _tokenCounter.Count(renderedPrompt);
            var maxTokens = persona.Config.SafetyGuardrails.MaxPromptTokens ?? 4000;

            if (tokenCount > maxTokens)
            {
                renderedPrompt = TruncatePrompt(renderedPrompt, maxTokens);
                _logger.LogWarning(
                    "Prompt truncated for Persona {PersonaId}: {Original} -> {Truncated} tokens",
                    personaId, tokenCount, maxTokens);
            }

            var result = new GeneratedPrompt
            {
                Content = renderedPrompt,
                TokenCount = tokenCount,
                PersonaId = personaId,
                GeneratedAt = DateTime.UtcNow
            };

            // Step 6: 快取
            await CachePromptAsync(personaId, userMessage, result, cancellationToken);

            return Result<GeneratedPrompt>.Success(result);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to generate prompt for Persona {PersonaId}", personaId);
            return Result<GeneratedPrompt>.Failure($"Prompt generation failed: {ex.Message}");
        }
    }

    private string TruncatePrompt(string prompt, int maxTokens)
    {
        // 智能截斷：保留 System Role + Safety Guardrails，縮減 Examples/Context
        var sections = prompt.Split("# ", StringSplitOptions.RemoveEmptyEntries);
        var criticalSections = sections.Where(s =>
            s.StartsWith("System Role") ||
            s.StartsWith("Safety Guardrails")).ToList();

        var truncated = string.Join("# ", criticalSections);
        return truncated;
    }

    private string GetDefaultTemplateKey(string role)
    {
        return role switch
        {
            "Cloud Architect" => "template_cloud_architect",
            "Data Scientist" => "template_data_scientist",
            "Product Manager" => "template_product_manager",
            // ... 其他 7 種 Persona
            _ => "template_default"
        };
    }
}
```

#### US 7.2.4: Backend API 規格

**API Endpoint**: `POST /api/v1/personas/{personaId}/prompts/generate`

**Request**:
```json
{
  "userMessage": "How do I design a highly available Kubernetes cluster?",
  "includeContext": true,
  "contextData": {
    "currentProject": "E-Commerce Platform",
    "userRole": "DevOps Engineer"
  }
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "promptId": "123e4567-e89b-12d3-a456-426614174000",
    "content": "# System Role\nYou are Alice, a Cloud Architect...",
    "tokenCount": 1250,
    "personaId": "123e4567-e89b-12d3-a456-426614174001",
    "generatedAt": "2024-01-15T10:30:00Z",
    "cacheHit": false
  }
}
```

**Error Response (400 Bad Request)**:
```json
{
  "success": false,
  "error": {
    "code": "PROMPT_GENERATION_FAILED",
    "message": "Template not found for role: Cloud Architect",
    "details": {
      "personaId": "123e4567-e89b-12d3-a456-426614174001",
      "templateKey": "template_cloud_architect"
    }
  }
}
```

#### US 7.2.5: Database Schema

**Prompt Templates Table**:
```sql
CREATE TABLE prompt_templates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    template_key VARCHAR(100) UNIQUE NOT NULL,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    content TEXT NOT NULL, -- Liquid template 內容
    persona_role VARCHAR(100) NOT NULL, -- 關聯的 Persona 角色
    version INTEGER NOT NULL DEFAULT 1,
    is_active BOOLEAN DEFAULT true,
    created_by UUID REFERENCES users(id),
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW(),

    CONSTRAINT chk_template_key_format CHECK (template_key ~ '^template_[a-z_]+$')
);

CREATE INDEX idx_prompt_templates_role ON prompt_templates(persona_role);
CREATE INDEX idx_prompt_templates_active ON prompt_templates(is_active) WHERE is_active = true;
```

**Generated Prompts Cache Table** (可選，輔助快取查詢):
```sql
CREATE TABLE generated_prompts_cache (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    persona_id UUID NOT NULL REFERENCES personas(id) ON DELETE CASCADE,
    user_message_hash VARCHAR(64) NOT NULL, -- SHA256(userMessage)
    content TEXT NOT NULL,
    token_count INTEGER NOT NULL,
    generated_at TIMESTAMP NOT NULL DEFAULT NOW(),
    expires_at TIMESTAMP NOT NULL, -- TTL 1 hour

    CONSTRAINT uq_persona_message UNIQUE (persona_id, user_message_hash)
);

CREATE INDEX idx_generated_prompts_expires ON generated_prompts_cache(expires_at);
```

#### US 7.2.6: Application Layer 設計

**CQRS Command**:
```csharp
public sealed record GeneratePromptCommand(
    Guid PersonaId,
    string UserMessage,
    bool IncludeContext,
    Dictionary<string, object>? ContextData) : IRequest<Result<GeneratedPromptDto>>;

public sealed class GeneratePromptCommandHandler
    : IRequestHandler<GeneratePromptCommand, Result<GeneratedPromptDto>>
{
    private readonly IPromptGenerationService _promptService;
    private readonly IMapper _mapper;

    public async Task<Result<GeneratedPromptDto>> Handle(
        GeneratePromptCommand request,
        CancellationToken cancellationToken)
    {
        var result = await _promptService.GeneratePromptAsync(
            request.PersonaId,
            request.UserMessage,
            cancellationToken);

        if (!result.IsSuccess)
            return Result<GeneratedPromptDto>.Failure(result.Error);

        var dto = _mapper.Map<GeneratedPromptDto>(result.Value);
        return Result<GeneratedPromptDto>.Success(dto);
    }
}
```

**CQRS Query**:
```csharp
public sealed record GetPromptTemplatesQuery(
    string? PersonaRole) : IRequest<Result<List<PromptTemplateDto>>>;

public sealed class GetPromptTemplatesQueryHandler
    : IRequestHandler<GetPromptTemplatesQuery, Result<List<PromptTemplateDto>>>
{
    private readonly IPromptTemplateRepository _repository;
    private readonly IMapper _mapper;

    public async Task<Result<List<PromptTemplateDto>>> Handle(
        GetPromptTemplatesQuery request,
        CancellationToken cancellationToken)
    {
        var templates = await _repository.GetByRoleAsync(
            request.PersonaRole,
            cancellationToken);

        var dtos = _mapper.Map<List<PromptTemplateDto>>(templates);
        return Result<List<PromptTemplateDto>>.Success(dtos);
    }
}
```

#### US 7.2.7: Token Counting 與限制管理

**使用 TikToken Library**:
```csharp
public sealed class TikTokenCounter : ITokenCounter
{
    private readonly Encoding _encoding;

    public TikTokenCounter()
    {
        // 使用 GPT-4 的 cl100k_base encoding
        _encoding = Encoding.Get("cl100k_base");
    }

    public int Count(string text)
    {
        var tokens = _encoding.Encode(text);
        return tokens.Count;
    }

    public string Truncate(string text, int maxTokens)
    {
        var tokens = _encoding.Encode(text);
        if (tokens.Count <= maxTokens)
            return text;

        var truncatedTokens = tokens.Take(maxTokens).ToList();
        return _encoding.Decode(truncatedTokens);
    }
}
```

**相關參考文件**:
- 🔧 [TikToken .NET Library](https://github.com/microsoft/Tokenizer)
- 📐 [OpenAI Tokenizer Guide](https://platform.openai.com/tokenizer)

#### US 7.2.8: 參考文件

**Domain 設計參考**:
- 📐 [Prompt Engineering Best Practices](../../docs/technical-implementation/03-ai-integration/01-prompt-engineering.md)
- 📐 [Liquid Template Syntax](https://shopify.github.io/liquid/)
- 📐 [Token Counting Strategies](../../docs/technical-implementation/03-ai-integration/02-token-management.md)

**Implementation 參考**:
- 🔨 [IPromptGenerationService Interface](../../src/AIAgentPlatform.Application/Interfaces/IPromptGenerationService.cs)
- 🔨 [PromptTemplateRepository](../../src/AIAgentPlatform.Infrastructure/Repositories/PromptTemplateRepository.cs)
- 🔨 [Liquid Template Engine Integration](../../src/AIAgentPlatform.Infrastructure/Services/LiquidTemplateEngine.cs)

**Testing 參考**:
- 🧪 [Prompt Generation Tests](../../tests/AIAgentPlatform.UnitTests/Application/PromptGeneration/)
- 🧪 [Template Rendering Tests](../../tests/AIAgentPlatform.IntegrationTests/PromptTemplates/)

### US 2.2: Plugin 熱重載機制 (3 SP)

**目標**: 實現零停機 Plugin 熱更新，支援多版本並存與優雅切換

#### US 2.2.1: MVP 範圍定義

**Must Have (Phase 1)**:
- ✅ **AssemblyLoadContext 隔離**: 每個 Plugin 版本獨立 AssemblyLoadContext，避免版本衝突
- ✅ **IPluginLoader 實作**: 動態載入與卸載 Plugin Assembly
- ✅ **版本管理**: 多版本並存（v1.0.0 與 v1.1.0 同時運行）
- ✅ **優雅切換**: 執行中的 Agent 完成當前任務後自動切換到新版本
- ✅ **熱部署 API**: 上傳新版本 Plugin DLL 並觸發熱重載
- ✅ **回滾機制**: 熱更新失敗時自動回滾到舊版本

**Excluded (Phase 2)**:
- ❌ A/B Testing 支援 (部分 Agent 使用新版本) - Phase 2
- ❌ 版本相容性自動檢查 (API 破壞性變更偵測) - Phase 2
- ❌ 熱更新效果監控與分析 - Phase 2
- ❌ 分散式環境下的多節點熱更新協調 - Phase 2

#### US 2.2.2: AssemblyLoadContext 架構設計

**隔離策略**:
```yaml
Plugin 隔離層級:
  - 每個 Plugin Name 一個專屬 AssemblyLoadContext
  - 同一個 Plugin 的不同版本共用相同 Context (透過版本切換管理)
  - 支援 Assembly 卸載 (需確保無引用時才卸載)

依賴管理:
  - Plugin 依賴的第三方套件隔離載入
  - 共用框架依賴 (Semantic Kernel, Microsoft.Extensions.*) 使用主 Context
  - 避免依賴衝突與版本污染
```

**C# AssemblyLoadContext 實作**:
```csharp
public sealed class PluginAssemblyLoadContext : AssemblyLoadContext
{
    private readonly AssemblyDependencyResolver _resolver;
    private readonly ILogger<PluginAssemblyLoadContext> _logger;

    public string PluginName { get; }
    public VersionNumber Version { get; }

    public PluginAssemblyLoadContext(
        string pluginName,
        VersionNumber version,
        string pluginPath,
        ILogger<PluginAssemblyLoadContext> logger)
        : base(name: $"{pluginName}_v{version}", isCollectible: true)
    {
        PluginName = pluginName;
        Version = version;
        _resolver = new AssemblyDependencyResolver(pluginPath);
        _logger = logger;
    }

    protected override Assembly? Load(AssemblyName assemblyName)
    {
        // 優先使用主 Context 載入共用框架
        if (IsSharedFrameworkAssembly(assemblyName))
        {
            return null; // 由預設 Context 載入
        }

        // 使用 Resolver 解析 Plugin 依賴
        var assemblyPath = _resolver.ResolveAssemblyToPath(assemblyName);
        if (assemblyPath != null)
        {
            _logger.LogDebug(
                "Loading assembly {AssemblyName} for Plugin {PluginName} v{Version}",
                assemblyName.Name, PluginName, Version);
            return LoadFromAssemblyPath(assemblyPath);
        }

        return null;
    }

    protected override IntPtr LoadUnmanagedDll(string unmanagedDllName)
    {
        var libraryPath = _resolver.ResolveUnmanagedDllToPath(unmanagedDllName);
        return libraryPath != null
            ? LoadUnmanagedDllFromPath(libraryPath)
            : IntPtr.Zero;
    }

    private static bool IsSharedFrameworkAssembly(AssemblyName assemblyName)
    {
        var sharedPrefixes = new[]
        {
            "Microsoft.Extensions.",
            "Microsoft.SemanticKernel.",
            "System.",
            "Newtonsoft.Json"
        };

        return sharedPrefixes.Any(prefix =>
            assemblyName.Name?.StartsWith(prefix) == true);
    }
}
```

#### US 2.2.3: IPluginLoader 核心實作

**介面定義**:
```csharp
public interface IPluginLoader
{
    /// <summary>
    /// 載入指定版本的 Plugin
    /// </summary>
    Task<Result<PluginLoadResult>> LoadPluginAsync(
        string pluginName,
        VersionNumber version,
        string assemblyPath,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 卸載指定版本的 Plugin (優雅卸載)
    /// </summary>
    Task<Result> UnloadPluginAsync(
        string pluginName,
        VersionNumber version,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 切換 Plugin 版本 (原子操作)
    /// </summary>
    Task<Result> SwitchPluginVersionAsync(
        string pluginName,
        VersionNumber fromVersion,
        VersionNumber toVersion,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 取得目前載入的 Plugin 版本清單
    /// </summary>
    IReadOnlyList<LoadedPluginInfo> GetLoadedPlugins();
}

public sealed record PluginLoadResult(
    string PluginName,
    VersionNumber Version,
    Assembly Assembly,
    PluginAssemblyLoadContext LoadContext,
    List<Type> PluginTypes);

public sealed record LoadedPluginInfo(
    string PluginName,
    VersionNumber Version,
    int ActiveAgentCount,
    DateTime LoadedAt);
```

**實作類別**:
```csharp
public sealed class PluginLoader : IPluginLoader, IDisposable
{
    private readonly ConcurrentDictionary<string, PluginVersionManager> _pluginManagers = new();
    private readonly IPluginActivator _activator;
    private readonly ILogger<PluginLoader> _logger;
    private readonly SemaphoreSlim _switchLock = new(1, 1);

    public async Task<Result<PluginLoadResult>> LoadPluginAsync(
        string pluginName,
        VersionNumber version,
        string assemblyPath,
        CancellationToken cancellationToken = default)
    {
        try
        {
            // Step 1: 建立隔離的 AssemblyLoadContext
            var loadContext = new PluginAssemblyLoadContext(
                pluginName,
                version,
                assemblyPath,
                _logger);

            // Step 2: 載入 Assembly
            var assembly = loadContext.LoadFromAssemblyPath(assemblyPath);

            // Step 3: 掃描 Plugin 類型
            var pluginTypes = ScanPluginTypes(assembly);
            if (pluginTypes.Count == 0)
            {
                loadContext.Unload();
                return Result<PluginLoadResult>.Failure(
                    $"No plugin types found in assembly {assemblyPath}");
            }

            // Step 4: 註冊到版本管理器
            var manager = _pluginManagers.GetOrAdd(
                pluginName,
                _ => new PluginVersionManager(pluginName, _logger));

            manager.RegisterVersion(version, loadContext, pluginTypes);

            var result = new PluginLoadResult(
                pluginName,
                version,
                assembly,
                loadContext,
                pluginTypes);

            _logger.LogInformation(
                "Successfully loaded Plugin {PluginName} v{Version} with {TypeCount} types",
                pluginName, version, pluginTypes.Count);

            return Result<PluginLoadResult>.Success(result);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex,
                "Failed to load Plugin {PluginName} v{Version} from {Path}",
                pluginName, version, assemblyPath);
            return Result<PluginLoadResult>.Failure(
                $"Plugin load failed: {ex.Message}");
        }
    }

    public async Task<Result> SwitchPluginVersionAsync(
        string pluginName,
        VersionNumber fromVersion,
        VersionNumber toVersion,
        CancellationToken cancellationToken = default)
    {
        await _switchLock.WaitAsync(cancellationToken);
        try
        {
            if (!_pluginManagers.TryGetValue(pluginName, out var manager))
            {
                return Result.Failure($"Plugin {pluginName} not found");
            }

            // Step 1: 驗證目標版本已載入
            if (!manager.IsVersionLoaded(toVersion))
            {
                return Result.Failure($"Target version {toVersion} not loaded");
            }

            // Step 2: 切換活動版本 (原子操作)
            manager.SwitchActiveVersion(toVersion);

            _logger.LogInformation(
                "Successfully switched Plugin {PluginName}: v{FromVersion} -> v{ToVersion}",
                pluginName, fromVersion, toVersion);

            // Step 3: 非同步卸載舊版本 (等待所有 Agent 完成)
            _ = Task.Run(async () =>
            {
                await Task.Delay(TimeSpan.FromMinutes(5), cancellationToken);
                await UnloadPluginAsync(pluginName, fromVersion, cancellationToken);
            }, cancellationToken);

            return Result.Success();
        }
        finally
        {
            _switchLock.Release();
        }
    }

    public async Task<Result> UnloadPluginAsync(
        string pluginName,
        VersionNumber version,
        CancellationToken cancellationToken = default)
    {
        if (!_pluginManagers.TryGetValue(pluginName, out var manager))
        {
            return Result.Failure($"Plugin {pluginName} not found");
        }

        // 等待所有使用此版本的 Agent 完成
        var activeCount = manager.GetActiveAgentCount(version);
        if (activeCount > 0)
        {
            _logger.LogWarning(
                "Cannot unload Plugin {PluginName} v{Version}: {ActiveCount} agents still active",
                pluginName, version, activeCount);
            return Result.Failure("Plugin still in use");
        }

        // 卸載 AssemblyLoadContext
        manager.UnloadVersion(version);

        // 觸發 GC 確保資源釋放
        for (int i = 0; i < 3; i++)
        {
            GC.Collect();
            GC.WaitForPendingFinalizers();
        }

        _logger.LogInformation(
            "Successfully unloaded Plugin {PluginName} v{Version}",
            pluginName, version);

        return Result.Success();
    }

    private List<Type> ScanPluginTypes(Assembly assembly)
    {
        return assembly.GetTypes()
            .Where(t => t.IsClass && !t.IsAbstract)
            .Where(t => t.GetCustomAttribute<PluginAttribute>() != null)
            .ToList();
    }

    public void Dispose()
    {
        _switchLock?.Dispose();
    }
}
```

#### US 2.2.4: 版本管理器設計

**PluginVersionManager 類別**:
```csharp
public sealed class PluginVersionManager
{
    private readonly string _pluginName;
    private readonly ConcurrentDictionary<VersionNumber, PluginVersionContext> _versions = new();
    private readonly ILogger _logger;
    private VersionNumber? _activeVersion;

    public PluginVersionManager(string pluginName, ILogger logger)
    {
        _pluginName = pluginName;
        _logger = logger;
    }

    public void RegisterVersion(
        VersionNumber version,
        PluginAssemblyLoadContext loadContext,
        List<Type> pluginTypes)
    {
        var context = new PluginVersionContext(
            version,
            loadContext,
            pluginTypes,
            DateTime.UtcNow);

        _versions[version] = context;
        _activeVersion ??= version; // 首次註冊設為活動版本
    }

    public void SwitchActiveVersion(VersionNumber newVersion)
    {
        if (!_versions.ContainsKey(newVersion))
            throw new InvalidOperationException($"Version {newVersion} not loaded");

        _activeVersion = newVersion;
    }

    public bool IsVersionLoaded(VersionNumber version)
        => _versions.ContainsKey(version);

    public int GetActiveAgentCount(VersionNumber version)
    {
        return _versions.TryGetValue(version, out var context)
            ? context.ActiveAgentCount
            : 0;
    }

    public void UnloadVersion(VersionNumber version)
    {
        if (_versions.TryRemove(version, out var context))
        {
            context.LoadContext.Unload();
        }
    }

    public VersionNumber? GetActiveVersion() => _activeVersion;

    public List<Type> GetActivePluginTypes()
    {
        if (_activeVersion == null)
            return new List<Type>();

        return _versions.TryGetValue(_activeVersion, out var context)
            ? context.PluginTypes
            : new List<Type>();
    }
}

public sealed record PluginVersionContext(
    VersionNumber Version,
    PluginAssemblyLoadContext LoadContext,
    List<Type> PluginTypes,
    DateTime LoadedAt)
{
    public int ActiveAgentCount { get; set; }
}
```

#### US 2.2.5: Backend API 規格

**API Endpoint 1**: `POST /api/v1/plugins/{pluginName}/versions/{version}/hot-reload`

**Request**:
```json
{
  "assemblyFileId": "123e4567-e89b-12d3-a456-426614174000",
  "switchStrategy": "graceful",
  "rollbackOnFailure": true
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "pluginName": "WeatherPlugin",
    "newVersion": "1.1.0",
    "previousVersion": "1.0.0",
    "switchedAt": "2024-01-15T10:30:00Z",
    "affectedAgents": 5,
    "status": "completed"
  }
}
```

**API Endpoint 2**: `POST /api/v1/plugins/{pluginName}/versions/{version}/rollback`

**Request**:
```json
{
  "targetVersion": "1.0.0",
  "reason": "Performance regression detected"
}
```

**Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "pluginName": "WeatherPlugin",
    "rolledBackTo": "1.0.0",
    "rolledBackFrom": "1.1.0",
    "rolledBackAt": "2024-01-15T10:35:00Z"
  }
}
```

#### US 2.2.6: Application Layer 設計

**CQRS Command**:
```csharp
public sealed record HotReloadPluginCommand(
    string PluginName,
    VersionNumber NewVersion,
    Guid AssemblyFileId,
    SwitchStrategy Strategy) : IRequest<Result<HotReloadResultDto>>;

public sealed class HotReloadPluginCommandHandler
    : IRequestHandler<HotReloadPluginCommand, Result<HotReloadResultDto>>
{
    private readonly IPluginLoader _pluginLoader;
    private readonly IPluginVersionRepository _versionRepo;
    private readonly IFileStorageService _fileStorage;
    private readonly ILogger<HotReloadPluginCommandHandler> _logger;

    public async Task<Result<HotReloadResultDto>> Handle(
        HotReloadPluginCommand request,
        CancellationToken cancellationToken)
    {
        try
        {
            // Step 1: 下載 Assembly 檔案
            var assemblyPath = await _fileStorage.DownloadFileAsync(
                request.AssemblyFileId,
                cancellationToken);

            // Step 2: 載入新版本
            var loadResult = await _pluginLoader.LoadPluginAsync(
                request.PluginName,
                request.NewVersion,
                assemblyPath,
                cancellationToken);

            if (!loadResult.IsSuccess)
            {
                _logger.LogError("Failed to load new version: {Error}", loadResult.Error);
                return Result<HotReloadResultDto>.Failure(loadResult.Error);
            }

            // Step 3: 取得舊版本
            var currentVersion = await _versionRepo.GetActiveVersionAsync(
                request.PluginName,
                cancellationToken);

            // Step 4: 切換版本
            var switchResult = await _pluginLoader.SwitchPluginVersionAsync(
                request.PluginName,
                currentVersion,
                request.NewVersion,
                cancellationToken);

            if (!switchResult.IsSuccess)
            {
                _logger.LogError("Failed to switch version: {Error}", switchResult.Error);
                // 回滾：卸載新版本
                await _pluginLoader.UnloadPluginAsync(
                    request.PluginName,
                    request.NewVersion,
                    cancellationToken);
                return Result<HotReloadResultDto>.Failure(switchResult.Error);
            }

            // Step 5: 更新資料庫活動版本
            await _versionRepo.SetActiveVersionAsync(
                request.PluginName,
                request.NewVersion,
                cancellationToken);

            var result = new HotReloadResultDto
            {
                PluginName = request.PluginName,
                NewVersion = request.NewVersion.ToString(),
                PreviousVersion = currentVersion.ToString(),
                SwitchedAt = DateTime.UtcNow,
                Status = "completed"
            };

            return Result<HotReloadResultDto>.Success(result);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Hot reload failed for Plugin {PluginName}", request.PluginName);
            return Result<HotReloadResultDto>.Failure($"Hot reload failed: {ex.Message}");
        }
    }
}
```

#### US 2.2.7: 參考文件

**Architecture 參考**:
- 📐 [AssemblyLoadContext Documentation](https://learn.microsoft.com/en-us/dotnet/core/dependency-loading/understanding-assemblyloadcontext)
- 📐 [Plugin Hot Reload ADR](../../docs/architecture/adr/ADR-009-plugin-hot-reload.md)
- 📐 [Version Management Strategy](../../docs/technical-implementation/01-backend-net9/09-plugin-versioning.md)

**Implementation 參考**:
- 🔨 [IPluginLoader Interface](../../src/AIAgentPlatform.Application/Interfaces/IPluginLoader.cs)
- 🔨 [PluginLoader Service](../../src/AIAgentPlatform.Infrastructure/Services/PluginLoader.cs)
- 🔨 [PluginVersionManager](../../src/AIAgentPlatform.Infrastructure/Services/PluginVersionManager.cs)

**Testing 參考**:
- 🧪 [Plugin Hot Reload Tests](../../tests/AIAgentPlatform.IntegrationTests/PluginHotReload/)
- 🧪 [AssemblyLoadContext Tests](../../tests/AIAgentPlatform.UnitTests/Infrastructure/Services/PluginLoaderTests.cs)

---

## 第二部分：技術實施方案 (How to Build)

**完整技術文檔**: [Backend Technical Guide](../../docs/technical-implementation/01-backend-net9/README.md)

### Backend 實施

#### Clean Architecture 目錄結構

**架構設計參考**:
- 🏗️ [ADR-001: Clean Architecture](../../docs/architecture/adr/ADR-001-clean-architecture.md) - 架構分層決策
- 🏗️ [Monorepo Setup](../../docs/technical-implementation/01-backend-net9/01-monorepo-setup.md) - 專案結構

```
AIAgentPlatform.sln
├── src/
│   ├── AIAgentPlatform.Domain/                     # 領域層 (不依賴任何外部層)
│   │   ├── Entities/
│   │   │   ├── Persona.cs                          # Persona 實體 (US 7.1)
│   │   │   ├── PersonaTemplate.cs                  # Persona 模板實體
│   │   │   ├── PromptTemplate.cs                   # Prompt 模板實體 (US 7.2)
│   │   │   ├── PluginVersion.cs                    # Plugin 版本實體 (US 2.2)
│   │   │   └── PluginVersionHistory.cs             # Plugin 版本歷史
│   │   ├── ValueObjects/
│   │   │   ├── PersonaConfig.cs                    # Persona 配置 VO
│   │   │   ├── CommunicationStyle.cs               # 溝通風格 VO
│   │   │   ├── ResponsePattern.cs                  # 回應模式 VO
│   │   │   ├── SafetyGuardrails.cs                 # 安全守則 VO
│   │   │   └── VersionNumber.cs                    # 版本號 VO
│   │   └── Interfaces/
│   │       ├── IPersonaRepository.cs
│   │       ├── IPromptTemplateRepository.cs
│   │       └── IPluginVersionRepository.cs
│   │
│   ├── AIAgentPlatform.Application/                # 應用層 (依賴 Domain)
│   │   ├── Personas/
│   │   │   ├── Commands/
│   │   │   │   ├── CreatePersona/
│   │   │   │   │   ├── CreatePersonaCommand.cs
│   │   │   │   │   ├── CreatePersonaCommandHandler.cs
│   │   │   │   │   └── CreatePersonaCommandValidator.cs
│   │   │   │   ├── UpdatePersona/
│   │   │   │   │   ├── UpdatePersonaCommand.cs
│   │   │   │   │   └── UpdatePersonaCommandHandler.cs
│   │   │   │   └── DeletePersona/
│   │   │   │       ├── DeletePersonaCommand.cs
│   │   │   │       └── DeletePersonaCommandHandler.cs
│   │   │   └── Queries/
│   │   │       ├── GetPersonaById/
│   │   │       │   ├── GetPersonaByIdQuery.cs
│   │   │       │   └── GetPersonaByIdQueryHandler.cs
│   │   │       └── GetPersonasList/
│   │   │           ├── GetPersonasQuery.cs
│   │   │           └── GetPersonasQueryHandler.cs
│   │   │
│   │   ├── PromptGeneration/
│   │   │   ├── Commands/
│   │   │   │   ├── GeneratePrompt/
│   │   │   │   │   ├── GeneratePromptCommand.cs
│   │   │   │   │   └── GeneratePromptCommandHandler.cs
│   │   │   └── Queries/
│   │   │       └── GetPromptTemplates/
│   │   │           ├── GetPromptTemplatesQuery.cs
│   │   │           └── GetPromptTemplatesQueryHandler.cs
│   │   │
│   │   ├── PluginVersions/
│   │   │   ├── Commands/
│   │   │   │   ├── HotReloadPlugin/
│   │   │   │   │   ├── HotReloadPluginCommand.cs
│   │   │   │   │   └── HotReloadPluginCommandHandler.cs
│   │   │   │   └── RollbackPlugin/
│   │   │   │       ├── RollbackPluginCommand.cs
│   │   │   │       └── RollbackPluginCommandHandler.cs
│   │   │   └── Queries/
│   │   │       └── GetPluginVersions/
│   │   │           ├── GetPluginVersionsQuery.cs
│   │   │           └── GetPluginVersionsQueryHandler.cs
│   │   │
│   │   └── Interfaces/
│   │       ├── IPromptGenerationService.cs
│   │       ├── IPluginLoader.cs
│   │       ├── IPluginActivator.cs
│   │       └── ITokenCounter.cs
│   │
│   ├── AIAgentPlatform.Infrastructure/             # 基礎設施層 (依賴 Application)
│   │   ├── Persistence/
│   │   │   ├── ApplicationDbContext.cs             # EF Core DbContext
│   │   │   ├── Configurations/
│   │   │   │   ├── PersonaConfiguration.cs         # Persona 實體配置
│   │   │   │   ├── PromptTemplateConfiguration.cs  # PromptTemplate 配置
│   │   │   │   └── PluginVersionConfiguration.cs   # PluginVersion 配置
│   │   │   └── Repositories/
│   │   │       ├── PersonaRepository.cs
│   │   │       ├── PromptTemplateRepository.cs
│   │   │       └── PluginVersionRepository.cs
│   │   │
│   │   ├── Services/
│   │   │   ├── PromptGenerationService.cs          # Prompt 生成服務
│   │   │   ├── LiquidTemplateEngine.cs             # Liquid 模板引擎封裝
│   │   │   ├── TikTokenCounter.cs                  # Token 計數服務
│   │   │   ├── PluginLoader.cs                     # Plugin 動態載入
│   │   │   ├── PluginActivator.cs                  # Plugin 實例化
│   │   │   └── PluginVersionManager.cs             # Plugin 版本管理
│   │   │
│   │   └── Migrations/                             # EF Core Migrations
│   │       └── 20250115_AddPersonaAndPlugin.cs
│   │
│   └── AIAgentPlatform.API/                        # API 層 (最外層)
│       ├── Controllers/
│       │   ├── PersonasController.cs               # Persona CRUD API
│       │   ├── PromptGenerationController.cs       # Prompt 生成 API
│       │   └── PluginVersionsController.cs         # Plugin 版本管理 API
│       └── Program.cs                              # 依賴注入配置
│
└── tests/
    ├── AIAgentPlatform.UnitTests/
    │   ├── Domain/
    │   │   ├── PersonaTests.cs
    │   │   └── ValueObjects/
    │   │       ├── PersonaConfigTests.cs
    │   │       └── VersionNumberTests.cs
    │   ├── Application/
    │   │   ├── Personas/
    │   │   │   ├── CreatePersonaCommandHandlerTests.cs
    │   │   │   └── GetPersonaByIdQueryHandlerTests.cs
    │   │   └── PromptGeneration/
    │   │       └── GeneratePromptCommandHandlerTests.cs
    │   └── Infrastructure/
    │       ├── Services/
    │       │   ├── PromptGenerationServiceTests.cs
    │       │   ├── PluginLoaderTests.cs
    │       │   └── PluginActivatorTests.cs
    │       └── Repositories/
    │           └── PersonaRepositoryTests.cs
    │
    └── AIAgentPlatform.IntegrationTests/
        ├── Personas/
        │   └── PersonasControllerTests.cs
        ├── PromptGeneration/
        │   └── PromptGenerationControllerTests.cs
        └── PluginHotReload/
            └── PluginHotReloadTests.cs
```

**分層設計參考**:
- 📐 [Domain Layer Design](../../docs/technical-implementation/01-backend-net9/07-domain-layer.md)
- 📐 [Application Layer Design](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md)
- 📐 [Infrastructure Layer Design](../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md)
- 📐 [API Layer Design](../../docs/technical-implementation/01-backend-net9/08-api-layer.md)

#### 核心實施策略

**架構模式參考**:
- 📚 [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md) - 完整架構設計

**1. CQRS with MediatR**:
- Commands: 創建, 更新, 刪除 Persona, 生成 Prompt, Plugin 熱重載 (寫入操作)
- Queries: 查詢 Persona, 查詢 Prompt 模板, 查詢 Plugin 版本 (讀取操作)
- 好處: 關注點分離, 易於測試, 支援複雜業務邏輯

**參考**:
- 📐 [ADR-002: CQRS Pattern](../../docs/architecture/adr/ADR-002-cqrs-pattern.md) - CQRS 架構決策
- 📐 [CQRS Implementation](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md) - MediatR 實作指南
- 📐 [Pipeline Behaviors](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md#pipeline-behaviors)

**MediatR 配置範例**:
```csharp
// Program.cs - MediatR 註冊
builder.Services.AddMediatR(cfg => {
    cfg.RegisterServicesFromAssembly(typeof(CreatePersonaCommand).Assembly);
    cfg.AddBehavior(typeof(IPipelineBehavior<,>), typeof(ValidationBehavior<,>));
    cfg.AddBehavior(typeof(IPipelineBehavior<,>), typeof(LoggingBehavior<,>));
});
```

**2. Repository Pattern + Unit of Work**:
- PersonaRepository: 封裝 Persona 資料存取
- PromptTemplateRepository: 管理 Prompt 模板
- PluginVersionRepository: 管理 Plugin 版本歷史
- 好處: 隔離業務邏輯與資料存取實現, 可替換資料來源

**參考**:
- 📐 [ADR-003: Repository Pattern](../../docs/architecture/adr/ADR-003-repository-pattern.md) - Repository 架構決策
- 📐 [Data Access Layer](../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md) - Repository 實作
- 📐 [Generic Repository](../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md#generic-repository)

**3. FluentValidation**:
- Persona 配置驗證: 驗證 JSON Schema, 必填欄位, 資料範圍
- Plugin 版本驗證: SemVer 格式驗證, 版本衝突檢查
- 好處: 驗證與業務邏輯分離, 可讀性高, 易於維護

**參考**:
- 📐 [Validation Strategy](../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md) - FluentValidation 實作
- 📐 [Complex Validation Rules](../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md#complex-rules)

**FluentValidation 範例**:
```csharp
public sealed class CreatePersonaCommandValidator : AbstractValidator<CreatePersonaCommand>
{
    public CreatePersonaCommandValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Persona name is required")
            .MaximumLength(50).WithMessage("Name cannot exceed 50 characters");

        RuleFor(x => x.Role)
            .NotEmpty().WithMessage("Role is required")
            .MaximumLength(100);

        RuleFor(x => x.Config)
            .NotNull().WithMessage("Persona config is required")
            .SetValidator(new PersonaConfigValidator());
    }
}
```

**4. Entity Framework Core 9**:
- Code-First Migration: 資料庫結構版本控制
- LINQ 查詢: 強型別查詢, IntelliSense 支援
- JSONB 支援: PersonaConfig 儲存為 JSONB (PostgreSQL)
- 自動追蹤變更: 自動偵測實體狀態變化

**參考**:
- 📐 [Data Access Layer](../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md#ef-core-configuration)
- 📐 [Migration Strategy](../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md#ef-core-migrations)
- 📐 [JSONB Column Configuration](../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md#jsonb-support)

**EF Core 配置範例**:
```csharp
// PersonaConfiguration.cs
public sealed class PersonaConfiguration : IEntityTypeConfiguration<Persona>
{
    public void Configure(EntityTypeBuilder<Persona> builder)
    {
        builder.ToTable("personas");

        builder.HasKey(p => p.Id);

        builder.Property(p => p.Name)
            .HasMaxLength(50)
            .IsRequired();

        builder.Property(p => p.Role)
            .HasMaxLength(100)
            .IsRequired();

        // JSONB 配置
        builder.Property(p => p.Config)
            .HasColumnType("jsonb")
            .HasConversion(
                v => JsonSerializer.Serialize(v, (JsonSerializerOptions)null),
                v => JsonSerializer.Deserialize<PersonaConfig>(v, (JsonSerializerOptions)null));

        builder.HasIndex(p => p.Name);
        builder.HasIndex(p => p.Role);
    }
}
```

**5. Liquid Template Engine**:
- Fluid Library: .NET Liquid 模板引擎實作
- 模板快取: 解析後的模板快取到記憶體
- 安全模式: 限制模板中的可執行程式碼

**參考**:
- 📐 [Prompt Engineering](../../docs/technical-implementation/03-ai-integration/01-prompt-engineering.md)
- 📐 [Template Engine Integration](../../docs/technical-implementation/03-ai-integration/01-prompt-engineering.md#liquid-templates)

**Fluid 配置範例**:
```csharp
// Program.cs - Fluid 註冊
builder.Services.AddSingleton<IFluidParser>(sp =>
{
    var parser = new FluidParser();
    // 註冊自定義過濾器
    TemplateContext.GlobalMemberAccessStrategy.Register<PersonaConfig>();
    return parser;
});

// LiquidTemplateEngine.cs
public sealed class LiquidTemplateEngine : ITemplateEngine
{
    private readonly IFluidParser _parser;
    private readonly MemoryCache _cache;

    public async Task<string> RenderAsync(
        string template,
        Dictionary<string, object> variables,
        CancellationToken cancellationToken)
    {
        // 1. 從快取取得或解析模板
        var parsedTemplate = _cache.GetOrCreate(template, entry =>
        {
            entry.SlidingExpiration = TimeSpan.FromHours(1);
            return _parser.Parse(template);
        });

        // 2. 建立 Template Context
        var context = new TemplateContext(variables);

        // 3. 渲染模板
        return await parsedTemplate.RenderAsync(context);
    }
}
```

**6. AssemblyLoadContext (Plugin 熱重載)**:
- 隔離載入: 每個 Plugin 版本獨立 Context
- 可卸載: 支援 GC 回收舊版本
- 依賴解析: AssemblyDependencyResolver 自動解析依賴

**參考**:
- 📐 [ADR-009: Plugin Hot Reload](../../docs/architecture/adr/ADR-009-plugin-hot-reload.md)
- 📐 [AssemblyLoadContext Best Practices](https://learn.microsoft.com/en-us/dotnet/core/dependency-loading/understanding-assemblyloadcontext)
- 📐 [Plugin Versioning Strategy](../../docs/technical-implementation/01-backend-net9/09-plugin-versioning.md)

### Frontend 實施

**Frontend 架構完整文檔**: [Frontend Technical Guide](../../docs/technical-implementation/02-frontend-react/README.md)

**Sprint 4 重點**: 由於 Sprint 4 主要專注於 Backend 功能，Frontend 僅需基礎 API 整合與簡單 UI，完整的 Persona 管理 UI 將在 Sprint 5 (US 8.1) 實作。

#### 目錄結構

**前端架構參考**:
- 🎨 [Frontend Architecture](../../docs/technical-implementation/02-frontend-react/01-frontend-architecture.md) - React 架構設計
- 🎨 [Project Structure](../../docs/technical-implementation/02-frontend-react/01-frontend-architecture.md#project-structure)

```
apps/web-app/
├── src/
│   ├── features/
│   │   └── personas/                       # Persona 功能模組 (Sprint 4 基礎版)
│   │       ├── components/
│   │       │   ├── PersonaList.tsx         # Persona 列表組件
│   │       │   ├── PersonaCard.tsx         # Persona 卡片
│   │       │   └── PersonaBasicForm.tsx    # 基礎 CRUD 表單
│   │       ├── hooks/
│   │       │   └── usePersonas.ts          # React Query hooks
│   │       └── services/
│   │           └── personaService.ts       # API 調用封裝
│   │
│   ├── components/                         # 共用組件
│   │   └── common/
│   │       ├── LoadingSpinner.tsx
│   │       └── ErrorBoundary.tsx
│   │
│   └── types/                              # TypeScript 類型
│       └── persona.ts                      # Persona 類型定義
```

**組件設計參考**:
- 🎨 [Component Design](../../docs/technical-implementation/02-frontend-react/02-component-design.md)
- 🎨 [Component Library](../../docs/ux-design/design-system/component-library.md)

#### API 整合策略

**React Query 配置**:
```typescript
// hooks/usePersonas.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { personaService } from '../services/personaService';

export const usePersonas = () => {
  return useQuery({
    queryKey: ['personas'],
    queryFn: () => personaService.getAll(),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
};

export const useCreatePersona = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: personaService.create,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['personas'] });
    },
  });
};
```

**API Service 封裝**:
```typescript
// services/personaService.ts
import { apiClient } from '@/services/apiClient';
import type { Persona, CreatePersonaRequest } from '@/types/persona';

export const personaService = {
  getAll: async (): Promise<Persona[]> => {
    const response = await apiClient.get<Persona[]>('/api/v1/personas');
    return response.data;
  },

  getById: async (id: string): Promise<Persona> => {
    const response = await apiClient.get<Persona>(`/api/v1/personas/${id}`);
    return response.data;
  },

  create: async (request: CreatePersonaRequest): Promise<Persona> => {
    const response = await apiClient.post<Persona>('/api/v1/personas', request);
    return response.data;
  },

  update: async (id: string, request: Partial<CreatePersonaRequest>): Promise<Persona> => {
    const response = await apiClient.put<Persona>(`/api/v1/personas/${id}`, request);
    return response.data;
  },

  delete: async (id: string): Promise<void> => {
    await apiClient.delete(`/api/v1/personas/${id}`);
  },
};
```

**參考**:
- 🎨 [Data Fetching Strategy](../../docs/technical-implementation/02-frontend-react/05-data-fetching.md) - React Query 實作
- 🎨 [API Client Configuration](../../docs/technical-implementation/02-frontend-react/05-data-fetching.md#api-client)

#### 基礎 UI 組件

**PersonaList 組件**:
```typescript
// components/PersonaList.tsx
export const PersonaList: React.FC = () => {
  const { data: personas, isLoading, error } = usePersonas();

  if (isLoading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;

  return (
    <div className="persona-list">
      {personas?.map((persona) => (
        <PersonaCard key={persona.id} persona={persona} />
      ))}
    </div>
  );
};
```

**參考**:
- 🎨 [Component Testing Guidelines](../../docs/testing/testing-strategy.md#component-testing)

#### TypeScript 類型定義

```typescript
// types/persona.ts
export interface Persona {
  id: string;
  name: string;
  role: string;
  description?: string;
  config: PersonaConfig;
  isTemplate: boolean;
  isActive: boolean;
  createdAt: string;
  updatedAt: string;
}

export interface PersonaConfig {
  expertise: string[];
  communicationStyle: CommunicationStyle;
  responsePattern: ResponsePattern;
  safetyGuardrails: SafetyGuardrails;
}

export interface CommunicationStyle {
  formality: 'casual' | 'professional' | 'formal';
  verbosity: 'concise' | 'balanced' | 'detailed';
  tone: 'neutral' | 'encouraging' | 'authoritative';
}

export interface CreatePersonaRequest {
  name: string;
  role: string;
  description?: string;
  config: PersonaConfig;
}
```

**參考**:
- 🎨 [TypeScript Best Practices](../../docs/technical-implementation/02-frontend-react/06-typescript-practices.md)

### 測試策略

**測試策略完整文檔**: [Testing Strategy](../../docs/testing/testing-strategy.md)

#### Backend 測試

**Backend 測試指南**: [Backend Testing Guidelines](../../docs/testing/unit-testing-guidelines.md)

**1. 單元測試** (目標: 60 tests, ≥85% 覆蓋率):

**Domain Layer 測試**:
- Persona Entity 測試 (8 tests)
  - Persona 創建驗證
  - Persona 配置更新
  - Persona 狀態切換 (啟用/停用)
- Value Objects 測試 (12 tests)
  - PersonaConfig 驗證
  - CommunicationStyle 驗證
  - VersionNumber 格式驗證

**Application Layer 測試**:
- Command Handlers 測試 (15 tests)
  - CreatePersonaCommandHandler (成功/失敗情境)
  - UpdatePersonaCommandHandler (部分更新/完整更新)
  - GeneratePromptCommandHandler (快取命中/未命中)
  - HotReloadPluginCommandHandler (成功/回滾)
- Query Handlers 測試 (8 tests)
  - GetPersonaByIdQuery (存在/不存在)
  - GetPersonasQuery (篩選/排序)
- Validators 測試 (10 tests)
  - CreatePersonaCommandValidator (各種驗證規則)
  - PersonaConfigValidator (JSON Schema 驗證)

**Infrastructure Layer 測試**:
- Services 測試 (12 tests)
  - PromptGenerationService (模板渲染/Token 計數)
  - PluginLoader (載入/卸載/切換版本)
  - TikTokenCounter (Token 計數準確性)
- Repositories 測試 (5 tests, 使用 In-Memory Database)
  - PersonaRepository CRUD 操作
  - JSONB 序列化/反序列化

**測試工具**:
- xUnit - 測試框架
- Moq - Mocking Library
- FluentAssertions - 斷言庫
- AutoFixture - 測試資料產生

**參考**:
- 🧪 [Unit Testing Guidelines](../../docs/testing/unit-testing-guidelines.md)
- 🧪 [Mocking Strategy](../../docs/testing/unit-testing-guidelines.md#mocking-strategy)
- 🧪 [Test Coverage Requirements](../../docs/testing/testing-strategy.md#coverage-requirements)

**單元測試範例**:
```csharp
// CreatePersonaCommandHandlerTests.cs
public sealed class CreatePersonaCommandHandlerTests
{
    private readonly Mock<IPersonaRepository> _mockRepo;
    private readonly CreatePersonaCommandHandler _handler;

    [Fact]
    public async Task Handle_ValidCommand_ReturnsSuccessResult()
    {
        // Arrange
        var command = new CreatePersonaCommand(
            Name: "Alice",
            Role: "Cloud Architect",
            Config: CreateValidPersonaConfig()
        );

        _mockRepo.Setup(r => r.AddAsync(It.IsAny<Persona>(), default))
            .ReturnsAsync(Result.Success());

        // Act
        var result = await _handler.Handle(command, default);

        // Assert
        result.IsSuccess.Should().BeTrue();
        result.Value.Name.Should().Be("Alice");
        _mockRepo.Verify(r => r.AddAsync(It.IsAny<Persona>(), default), Times.Once);
    }
}
```

**2. 集成測試** (目標: 18 tests, ≥80% 覆蓋率):

**API 端點測試**:
- Persona API 測試 (8 tests)
  - POST /api/v1/personas (創建成功/驗證失敗)
  - GET /api/v1/personas (列表查詢/篩選)
  - PUT /api/v1/personas/{id} (更新成功/不存在)
  - DELETE /api/v1/personas/{id} (刪除成功/不存在)
- Prompt Generation API 測試 (5 tests)
  - POST /api/v1/personas/{id}/prompts/generate (成功/Token 超限)
- Plugin Hot Reload API 測試 (5 tests)
  - POST /api/v1/plugins/{name}/versions/{version}/hot-reload (成功/回滾)
  - POST /api/v1/plugins/{name}/versions/{version}/rollback

**資料庫操作測試**:
- EF Core JSONB 操作 (序列化/反序列化)
- Migration 驗證 (資料庫結構正確性)

**測試工具**:
- WebApplicationFactory - API 測試
- Testcontainers - PostgreSQL 容器
- xUnit - 測試框架

**參考**:
- 🧪 [Integration Testing Guidelines](../../docs/testing/integration-testing-guidelines.md)
- 🧪 [API Testing Strategy](../../docs/testing/integration-testing-guidelines.md#api-testing)
- 🧪 [Database Testing](../../docs/testing/integration-testing-guidelines.md#database-testing)

**集成測試範例**:
```csharp
// PersonasControllerTests.cs
public sealed class PersonasControllerTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly HttpClient _client;

    [Fact]
    public async Task CreatePersona_ValidRequest_ReturnsCreated()
    {
        // Arrange
        var request = new CreatePersonaRequest(
            Name: "Alice",
            Role: "Cloud Architect",
            Config: CreateValidPersonaConfig()
        );

        // Act
        var response = await _client.PostAsJsonAsync("/api/v1/personas", request);

        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.Created);
        var persona = await response.Content.ReadFromJsonAsync<PersonaDto>();
        persona.Name.Should().Be("Alice");
    }
}
```

#### Frontend 測試

**Frontend 測試指南**: [Frontend Testing Guidelines](../../docs/testing/testing-strategy.md#frontend-testing)

**1. 組件測試** (目標: 10 tests):
- PersonaList 組件 (3 tests)
  - 載入狀態顯示
  - 成功顯示列表
  - 錯誤處理
- PersonaCard 組件 (2 tests)
  - 正確顯示 Persona 資訊
  - 點擊事件處理
- PersonaBasicForm 組件 (5 tests)
  - 表單驗證
  - 提交成功/失敗
  - Loading 狀態

**測試工具**:
- Vitest - 測試框架
- React Testing Library - 組件測試
- MSW (Mock Service Worker) - API Mocking

**參考**:
- 🧪 [Component Testing Guidelines](../../docs/testing/testing-strategy.md#component-testing)
- 🧪 [React Testing Library Best Practices](../../docs/testing/testing-strategy.md#rtl-best-practices)

**2. E2E 測試** (目標: 3 tests):
- Persona 創建流程 (1 test)
- Plugin 熱重載流程 (1 test)
- Persona 配置載入流程 (1 test)

**測試工具**:
- Playwright - E2E 測試框架

**參考**:
- 🧪 [E2E Testing Guidelines](../../docs/testing/testing-strategy.md#e2e-testing)

### 部署與監控

**部署策略完整文檔**: [Deployment Strategy](../../docs/deployment/deployment-strategy.md)

#### Database Migration 策略

**Migration 執行順序**:
```bash
# 1. 建立 Migration
cd src/AIAgentPlatform.Infrastructure
dotnet ef migrations add AddPersonaAndPluginVersioning \
  --startup-project ../AIAgentPlatform.API

# 2. 驗證 Migration SQL
dotnet ef migrations script \
  --startup-project ../AIAgentPlatform.API \
  --output migrations.sql

# 3. 執行 Migration (Development)
dotnet ef database update \
  --startup-project ../AIAgentPlatform.API

# 4. 執行 Migration (Production) - 透過 CI/CD
# Migration 會在部署時自動執行
```

**Migration 內容**:
- 新增 `personas` 表
- 新增 `persona_templates` 表
- 新增 `prompt_templates` 表
- 新增 `plugin_versions` 表
- 新增 `plugin_version_history` 表

**參考**:
- 📦 [Database Migration Strategy](../../docs/deployment/database-migrations.md)
- 📦 [Zero-Downtime Deployment](../../docs/deployment/deployment-strategy.md#zero-downtime)

#### 監控與日誌

**監控指標**:
- Prompt 生成延遲 (P50, P95, P99)
- Plugin 熱重載成功率
- Persona API 請求數與錯誤率
- JSONB 查詢效能

**日誌策略**:
- Structured Logging (Serilog)
- Log Level: Information (Production)
- 關鍵操作日誌: Plugin 載入/卸載, Prompt 生成, Persona CRUD

**參考**:
- 📊 [Monitoring Strategy](../../docs/deployment/monitoring-strategy.md)
- 📊 [Logging Best Practices](../../docs/deployment/logging-guidelines.md)

---

## 第三部分：編碼規範

**編碼規範完整文檔**: [Coding Conventions](../../docs/development-standards/coding-conventions.md)

### Backend (.NET 9 / C#)

**Backend 編碼標準**: [.NET Coding Standards](../../docs/development-standards/coding-conventions.md#dotnet)

#### 命名規範

**參考**:
- 📝 [Naming Conventions](../../docs/development-standards/coding-conventions.md#naming-conventions)
- 📝 [C# Style Guide](../../docs/development-standards/coding-conventions.md#csharp-style)

**命名規則**:
```csharp
// Pascal Case: Classes, Methods, Properties, Public Fields
public sealed class PersonaRepository : IPersonaRepository
{
    public async Task<Persona> GetByIdAsync(Guid id) { }
}

// Camel Case: Local variables, parameters, private fields
private readonly ILogger<PersonaService> _logger;
public async Task<Result> CreatePersonaAsync(string personaName) { }

// Interface: 'I' prefix
public interface IPromptGenerationService { }

// Constants: Pascal Case with descriptive names
public const int MaxPromptTokens = 4000;
public const string DefaultTemplateKey = "template_default";

// Async methods: 'Async' suffix
public async Task<Result<Persona>> LoadPersonaAsync(Guid id) { }
```

#### SOLID 原則

**參考**:
- 📝 [SOLID Principles](../../docs/development-standards/code-quality-standards.md#solid-principles)
- 📝 [Design Patterns](../../docs/development-standards/code-quality-standards.md#design-patterns)

**實踐原則**:
1. **單一職責原則 (SRP)**:
   - `PromptGenerationService` 只負責 Prompt 生成
   - `PluginLoader` 只負責 Plugin 載入/卸載
   - `PersonaRepository` 只負責 Persona 資料存取

2. **開放封閉原則 (OCP)**:
   - 使用 Interface 定義行為 (`IPluginLoader`, `IPromptGenerationService`)
   - 透過 Strategy Pattern 支援不同 Template Engine (Liquid, Handlebars)

3. **里氏替換原則 (LSP)**:
   - 所有 Repository 實作必須遵守 `IRepository<T>` 契約
   - Mock 物件可完全替換真實實作進行測試

4. **接口隔離原則 (ISP)**:
   - `IPluginLoader` 與 `IPluginActivator` 分離
   - `IPromptGenerationService` 與 `ITokenCounter` 分離

5. **依賴反轉原則 (DIP)**:
   - Application Layer 依賴 Interface, 不依賴具體實作
   - 使用 Dependency Injection Container 管理依賴

#### Clean Architecture 規範

**參考**:
- 🏗️ [ADR-001: Clean Architecture](../../docs/architecture/adr/ADR-001-clean-architecture.md)
- 🏗️ [Dependency Rules](../../docs/architecture/adr/ADR-001-clean-architecture.md#dependency-rules)

**分層依賴規則**:
```
API Layer (最外層)
    ↓ 依賴
Infrastructure Layer
    ↓ 依賴
Application Layer
    ↓ 依賴
Domain Layer (核心, 不依賴任何外部層)
```

**違反範例 (❌ 錯誤)**:
```csharp
// Domain Layer 不應該依賴 Infrastructure
namespace AIAgentPlatform.Domain.Entities
{
    using AIAgentPlatform.Infrastructure.Persistence; // ❌ 錯誤!
}
```

**正確範例 (✅ 正確)**:
```csharp
// Domain Layer 定義接口
namespace AIAgentPlatform.Domain.Interfaces
{
    public interface IPersonaRepository { }
}

// Infrastructure Layer 實作接口
namespace AIAgentPlatform.Infrastructure.Repositories
{
    using AIAgentPlatform.Domain.Interfaces; // ✅ 正確
    public class PersonaRepository : IPersonaRepository { }
}
```

#### CQRS 與 MediatR 規範

**參考**:
- 📐 [ADR-002: CQRS Pattern](../../docs/architecture/adr/ADR-002-cqrs-pattern.md)
- 📐 [CQRS Implementation](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md)

**Command 規範**:
```csharp
// 1. Command 使用 record 語法 (immutable)
public sealed record CreatePersonaCommand(
    string Name,
    string Role,
    PersonaConfig Config) : IRequest<Result<PersonaDto>>;

// 2. Handler 遵循單一職責
public sealed class CreatePersonaCommandHandler
    : IRequestHandler<CreatePersonaCommand, Result<PersonaDto>>
{
    private readonly IPersonaRepository _repository;
    private readonly IMapper _mapper;

    // 3. 使用建構函數注入依賴
    public CreatePersonaCommandHandler(
        IPersonaRepository repository,
        IMapper mapper)
    {
        _repository = repository;
        _mapper = mapper;
    }

    // 4. 回傳 Result Pattern (不拋出異常)
    public async Task<Result<PersonaDto>> Handle(
        CreatePersonaCommand request,
        CancellationToken cancellationToken)
    {
        // Implementation
    }
}
```

**Query 規範**:
```csharp
// Query 使用 record 語法
public sealed record GetPersonaByIdQuery(Guid PersonaId)
    : IRequest<Result<PersonaDto>>;

// Query Handler 只讀操作
public sealed class GetPersonaByIdQueryHandler
    : IRequestHandler<GetPersonaByIdQuery, Result<PersonaDto>>
{
    // 只依賴 Repository Query 方法
    public async Task<Result<PersonaDto>> Handle(
        GetPersonaByIdQuery request,
        CancellationToken cancellationToken)
    {
        // 使用 AsNoTracking() 提升查詢效能
        var persona = await _repository.GetByIdAsync(
            request.PersonaId,
            asNoTracking: true,
            cancellationToken);
    }
}
```

#### Code Quality 標準

**參考**:
- 📝 [Code Quality Standards](../../docs/development-standards/code-quality-standards.md)
- 📝 [Static Analysis Rules](../../docs/development-standards/code-quality-standards.md#static-analysis)

**靜態分析規則**:
```yaml
Cyclomatic Complexity:
  Maximum: 15
  Target: ≤10

Code Duplication:
  Maximum: 5%
  Target: <3%

Method Length:
  Maximum: 100 lines
  Target: ≤50 lines

Class Size:
  Maximum: 500 lines
  Target: ≤300 lines
```

**Code Smell 偵測**:
- Long Method (>100 lines)
- Large Class (>500 lines)
- Too Many Parameters (>5 parameters)
- Duplicate Code
- Dead Code

### Frontend (React 18 / TypeScript)

**Frontend 編碼標準**: [React/TypeScript Coding Standards](../../docs/development-standards/coding-conventions.md#react-typescript)

#### 命名規範

**參考**:
- 📝 [React Naming Conventions](../../docs/development-standards/coding-conventions.md#react-naming)
- 📝 [TypeScript Style Guide](../../docs/development-standards/coding-conventions.md#typescript-style)

**命名規則**:
```typescript
// Pascal Case: Components, Types, Interfaces
export const PersonaList: React.FC = () => { };
export interface Persona { }
export type PersonaConfig = { };

// Camel Case: Functions, variables, props
const fetchPersonas = async () => { };
const personaList = usePersonas();
const handleSubmit = (data: FormData) => { };

// Component 檔案名稱與組件名稱一致
// PersonaList.tsx exports PersonaList component

// Hooks: 'use' prefix
export const usePersonas = () => { };
export const useCreatePersona = () => { };
```

#### 組件設計原則

**參考**:
- 🎨 [Component Design Principles](../../docs/technical-implementation/02-frontend-react/02-component-design.md#design-principles)
- 🎨 [TypeScript Best Practices](../../docs/development-standards/coding-conventions.md#typescript-best-practices)

**組件規範**:
```typescript
// 1. 使用 TypeScript 定義 Props
interface PersonaCardProps {
  persona: Persona;
  onEdit?: (id: string) => void;
  onDelete?: (id: string) => void;
}

// 2. 使用 React.FC 或 函數組件
export const PersonaCard: React.FC<PersonaCardProps> = ({
  persona,
  onEdit,
  onDelete,
}) => {
  // 3. 使用 hooks 管理狀態
  const [isExpanded, setIsExpanded] = useState(false);

  // 4. 使用 useCallback 優化回調函數
  const handleExpand = useCallback(() => {
    setIsExpanded((prev) => !prev);
  }, []);

  // 5. 提早返回 (Guard Clauses)
  if (!persona) return null;

  return (
    <div className="persona-card">
      {/* Component JSX */}
    </div>
  );
};
```

#### Hooks 規範

**參考**:
- 🎨 [React Hooks Guidelines](../../docs/technical-implementation/02-frontend-react/01-frontend-architecture.md#hooks-guidelines)
- 🎨 [Custom Hooks Best Practices](../../docs/technical-implementation/02-frontend-react/01-frontend-architecture.md#custom-hooks)

**Hooks 使用規則**:
```typescript
// 1. 自訂 Hook 以 'use' 開頭
export const usePersonas = () => {
  return useQuery({
    queryKey: ['personas'],
    queryFn: personaService.getAll,
  });
};

// 2. 不在條件語句中使用 Hooks (❌ 錯誤)
if (condition) {
  const data = usePersonas(); // ❌ 錯誤!
}

// 3. 正確用法 (✅ 正確)
const { data } = usePersonas();
if (condition && data) {
  // Use data
}

// 4. 遵循 React Hooks 依賴規則
useEffect(() => {
  fetchPersonas();
}, [fetchPersonas]); // 必須列出所有依賴
```

---

## 第四部分：質量保證

### Code Review

**Code Review 完整指南**: [Code Review Checklist](../../docs/development-standards/code-review-checklist.md)

#### Code Review 重點

**參考**:
- ✅ [Code Review Checklist](../../docs/development-standards/code-review-checklist.md)
- ✅ [Code Review Process](../../docs/development-standards/git-workflow.md#pull-request-process)

**Review Checklist**:
1. **功能正確性**:
   - ✅ 所有驗收標準滿足
   - ✅ Edge Cases 處理完整
   - ✅ 錯誤處理適當 (Result Pattern)

2. **架構一致性**:
   - ✅ 遵循 Clean Architecture 分層
   - ✅ CQRS 正確使用
   - ✅ 依賴注入正確配置

3. **代碼可讀性**:
   - ✅ 命名清晰描述性
   - ✅ 方法長度適中 (≤50 lines)
   - ✅ 註解適當 (Why, not What)

4. **測試覆蓋率**:
   - ✅ 單元測試 ≥85%
   - ✅ 集成測試覆蓋關鍵路徑
   - ✅ 測試可讀性高 (Arrange-Act-Assert)

5. **安全性檢查**:
   - ✅ 無 SQL Injection 風險
   - ✅ 輸入驗證完整 (FluentValidation)
   - ✅ 敏感資訊不記錄日誌
   - ✅ JSONB 資料驗證 (JSON Schema)

6. **效能考量**:
   - ✅ 查詢使用 AsNoTracking() (Query)
   - ✅ Async/Await 正確使用
   - ✅ 避免 N+1 查詢問題

### Definition of Done

**完成定義文檔**: [Definition of Done](../../docs/project-management/definition-of-done.md)

#### Sprint 4 Definition of Done

**參考**:
- ✅ [Definition of Done](../../docs/project-management/definition-of-done.md)
- ✅ [Acceptance Criteria](../../docs/user-stories/modules/module-07-persona-framework.md#acceptance-criteria)

**通用 DoD**:
- ✅ 所有驗收標準通過
- ✅ 單元測試覆蓋率 ≥ 85%
- ✅ 集成測試通過 (API 端點測試)
- ✅ Code Review 通過 (至少 1 位 Reviewer 批准)
- ✅ 文檔更新完成 (API 文檔、README)
- ✅ 部署到 Development 環境
- ✅ 無 Critical / High Priority Bugs

**US 7.1 - Persona Template Configuration**:
- ✅ Persona 配置載入器實作完成
- ✅ 10 種預設 Persona 模板建立並通過驗證
- ✅ JSON Schema 驗證測試通過 (Valid/Invalid 情境)
- ✅ `personas` 與 `persona_templates` 資料表建立 (Migration 成功)
- ✅ CRUD API 端點測試通過 (8 tests)
- ✅ 單元測試覆蓋率 ≥85% (Domain + Application + Infrastructure)
- ✅ Swagger 文檔自動生成並正確
- ✅ Code Review 通過

**US 7.2 - Persona-Driven Prompt Engineering**:
- ✅ Prompt 模板引擎實作完成 (Liquid Template Engine)
- ✅ Persona-Driven Prompt 生成測試通過 (10+ 種 Persona 組合)
- ✅ Token Counting 準確性驗證 (TikToken)
- ✅ 動態調整機制驗證 (Token 超限時智能截斷)
- ✅ `prompt_templates` 資料表建立 (Migration 成功)
- ✅ Prompt Generation API 測試通過 (5 tests)
- ✅ 模板快取功能驗證 (Cache Hit/Miss)
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過 (End-to-End Prompt Generation)
- ✅ Code Review 通過

**US 2.2 - Plugin Hot Reload**:
- ✅ `IPluginLoader` 介面與實作完成
- ✅ AssemblyLoadContext 隔離驗證 (多版本並存)
- ✅ 熱重載功能測試通過 (載入 → 切換 → 卸載)
- ✅ 記憶體洩漏測試通過 (GC 回收驗證)
- ✅ 版本切換原子性驗證 (無 Race Condition)
- ✅ 回滾機制測試通過 (失敗自動回滾)
- ✅ `plugin_versions` 與 `plugin_version_history` 資料表建立
- ✅ Hot Reload API 測試通過 (5 tests)
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過

### 技術債務管理

**技術債務管理指南**: [Technical Debt Management](../../docs/development-standards/technical-debt-management.md)

#### 已知技術債務

**參考**:
- 📝 [Technical Debt Management](../../docs/development-standards/technical-debt-management.md)
- 📝 [Continuous Improvement Log](../../4-changes/CONTINUOUS-IMPROVEMENT-LOG.md)

**Sprint 4 技術債務清單**:
1. **Persona 模板 A/B Testing 支援** (Priority: Low):
   - 描述: 目前 Persona 只支援單一模板，Phase 2 需支援 A/B Testing
   - 影響: 無法進行 Persona 效果比較與優化
   - 規劃: Sprint 5 實作 (US 8.1)

2. **Plugin 版本相容性自動檢查** (Priority: Medium):
   - 描述: 熱重載時未檢查 API 破壞性變更
   - 影響: 可能導致 Runtime 錯誤
   - 規劃: Sprint 6 實作 (US 9.2)

3. **Prompt 生成效能優化** (Priority: Low):
   - 描述: 首次生成 Prompt 需解析模板，效能較低
   - 影響: API 延遲約 50-100ms
   - 緩解: 已實作模板快取 (Cache Hit 延遲 <5ms)
   - 規劃: 監控後決定是否需進一步優化

4. **Frontend Persona 管理 UI** (Priority: High):
   - 描述: Sprint 4 只實作基礎 API 整合，完整 UI 待實作
   - 影響: 使用者無法透過 UI 管理 Persona
   - 規劃: Sprint 5 實作 (US 8.1)

### 風險管理

**風險登記簿**: [Risk Register](../../1-planning/RISK-REGISTER.md)

#### Sprint 4 風險項目

**參考**:
- ⚠️ [Risk Register](../../1-planning/RISK-REGISTER.md)
- ⚠️ [Risk Mitigation Strategies](../../1-planning/RISK-REGISTER.md#mitigation-strategies)

**關鍵風險**:
1. **AssemblyLoadContext 記憶體洩漏** (Likelihood: Medium, Impact: High):
   - 緩解措施: 實作完整的單元測試與記憶體監控
   - 驗證方法: 使用 dotMemory 進行 Memory Profiling
   - 狀態: ✅ 已緩解 (測試通過)

2. **Liquid Template Engine 安全性** (Likelihood: Low, Impact: High):
   - 緩解措施: 限制模板可執行程式碼，使用安全模式
   - 驗證方法: Security Code Review + Penetration Testing
   - 狀態: ✅ 已緩解 (安全限制已實作)

3. **JSONB 查詢效能** (Likelihood: Medium, Impact: Medium):
   - 緩解措施: 建立適當索引，監控查詢效能
   - 驗證方法: Load Testing (1000+ Personas)
   - 狀態: ⏳ 持續監控

### 質量指標

**質量指標追蹤**:
```yaml
測試覆蓋率:
  單元測試: ≥85% (目標)
  集成測試: ≥80% (目標)
  E2E 測試: 關鍵流程覆蓋

程式碼品質:
  Cyclomatic Complexity: ≤15
  Code Duplication: <5%
  Code Smells: 0 Critical, ≤5 Major

效能指標:
  Prompt 生成延遲: P95 <200ms
  Plugin 熱重載時間: <5s
  API 回應時間: P95 <500ms
```

---

## 第五部分：參考文檔

### Planning
- [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md)
- [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md)
- [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md)

### User Stories
- [Module 07: Persona Framework](../../docs/user-stories/modules/module-07-persona-framework.md)
- [Module 02: Plugin System](../../docs/user-stories/modules/module-02-plugin-system.md)

### Technical Implementation
- [Persona Framework](../../docs/technical-implementation/01-backend-net9/03-persona-framework.md)
- [Prompt Engineering](../../docs/technical-implementation/01-backend-net9/09-prompt-engineering.md)
- [Plugin System MCP](../../docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md)

---

## 使用指南

### 目標讀者
- 開發團隊成員
- Tech Lead
- QA Engineer

### 使用方式
1. **開發前**: 閱讀技術實施方案
2. **開發中**: 遵循編碼規範
3. **完成後**: 檢查 Definition of Done

---

## 版本歷史

### v2.1 (2025-11-13)
- ✅ 建立 Sprint 4 PLAN 文件
- ✅ 定義技術實施方案
- ✅ 明確質量標準

---

**文件版本**: v2.1
**最後更新**: 2025-11-13
**維護者**: Sprint 4 開發團隊
