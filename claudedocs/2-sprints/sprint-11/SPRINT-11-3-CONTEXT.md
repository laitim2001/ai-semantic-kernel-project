# SPRINT 11 - 技術上下文 (Technical Context)

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

## 技術上下文說明

### 文檔目的

本文檔為 Sprint 11 提供詳細的技術上下文,幫助開發團隊理解:
- **Phase 1C 完成技術路徑** - Sprint 11 如何完成 Phase 1C 所有交付物
- **Execution Trigger 技術挑戰** - 4 種 Trigger 類型的實現細節
- **Template Management 設計** - Template CRUD + Marketplace 基礎
- **Version Control 最佳實踐** - Workflow Versioning + Rollback 策略
- **Phase 1D 技術準備** - 為 Frontend 開發提供完整 API 支持

---

## Phase 1C 完成技術準備

### Sprint 10 完成技術總結

**已交付的技術基礎設施**:

```yaml
Sprint 10 技術成果:
  Workflow Definition Management:
    ✅ WorkflowDefinition Domain Model (Aggregate Root)
    ✅ Repository Pattern 實作
    ✅ CQRS Commands/Queries (MediatR)
    ✅ 5 個 RESTful API endpoints
    ✅ Soft Delete 機制
    ✅ JSONB + 關聯式表混合存儲

  Node Registry:
    ✅ 5 種內建 Node types (Agent, Tool, Condition, Parallel, Sequential)
    ✅ Dynamic Node registration (ConcurrentDictionary)
    ✅ JSON Schema validation (NJsonSchema)
    ✅ Node Compatibility Matrix
    ✅ 3 個 API endpoints

  Edge Validation:
    ✅ Circular Dependency Detection (DFS algorithm)
    ✅ Node Compatibility validation
    ✅ Detailed error messages + suggestions
    ✅ 2 個 Validation API endpoints

參考文檔:
  - claudedocs/2-sprints/sprint-10/SPRINT-10-7-RETROSPECTIVE.md
  - docs/architecture/ADR-012-workflow-editor-technology.md
```

### Sprint 11 技術目標與範疇

**Sprint 11 (最後一個 Sprint) 技術交付**:

```yaml
Sprint 11 (本 Sprint):
  Workflow Execution Trigger:
    🎯 Manual Trigger API
    🎯 Schedule Trigger (Cron expression)
    🎯 Event Trigger (Event Bus)
    🎯 Webhook Trigger (HMAC-SHA256 security)

  Workflow Template Management:
    🎯 Template CRUD API
    🎯 Template Categories (hierarchical)
    🎯 Import/Export (JSON format)
    🎯 Marketplace基礎 (Rating + Usage tracking)

  Version Control基礎:
    🎯 Workflow Versioning (auto-increment)
    🎯 Version Comparison (Diff algorithm)
    🎯 Version Rollback
    🎯 Version History

Phase 1C 完成:
  - Sprint 10: 8 SP (50%)
  - Sprint 11: 9.5 SP (50%)
  - 總計: 17.5 SP, 6週, 100%完成
```

### Phase 1C → Phase 1D 技術銜接

**為 Phase 1D (Frontend) 準備的完整 API**:

```yaml
API 穩定性:
  - RESTful 設計原則 100% 遵循
  - API 版本化 (v1) 鎖定
  - 向後兼容保證
  - No breaking changes

API 文檔:
  - Swagger/OpenAPI 3.0 完整
  - TypeScript API Client 自動生成
  - 請求/響應範例完整
  - 錯誤代碼文檔詳細

性能基準:
  - API 響應時間 P95 <200ms ✅
  - Trigger 延遲 <1s ✅
  - Template Search <300ms ✅
  - Version Comparison <1s ✅

參考文檔:
  - docs/api/workflow-api-complete-reference.md
  - docs/api/typescript-client-generation.md
```

---

## Workflow Execution Trigger 技術上下文

### 設計背景

**業務需求**:
- IT 開發者需要多種方式觸發 Workflow 執行
- 支援手動觸發、定時執行、事件驅動、外部系統 Webhook
- 為 Workflow Automation 提供靈活的觸發機制

**技術挑戰**:
1. **多種 Trigger 類型統一抽象**: 如何設計統一的 Trigger 介面
2. **Schedule Trigger 可靠性**: Cron expression 解析和定時執行
3. **Event Trigger 解耦**: Event Bus 設計和事件匹配
4. **Webhook Security**: HMAC-SHA256 簽名驗證

### Manual Trigger 設計

**WorkflowExecution 實體**:

```csharp
public class WorkflowExecution : BaseEntity
{
    public Guid ExecutionId { get; private set; }
    public Guid WorkflowDefinitionId { get; private set; }

    public TriggerType TriggerType { get; private set; } // Manual/Schedule/Event/Webhook
    public string TriggerMetadata { get; private set; } // JSON (包含 trigger 詳細資訊)

    public ExecutionStatus Status { get; private set; } // Queued/Running/Completed/Failed
    public DateTime QueuedAt { get; private set; }
    public DateTime? StartedAt { get; private set; }
    public DateTime? CompletedAt { get; private set; }
    public TimeSpan? Duration { get; private set; }

    public string? ErrorMessage { get; private set; }
    public string? ExecutionResult { get; private set; } // JSON

    // Factory method
    public static WorkflowExecution CreateManualTrigger(
        Guid workflowDefinitionId,
        Dictionary<string, object>? inputParameters = null)
    {
        return new WorkflowExecution
        {
            ExecutionId = Guid.NewGuid(),
            WorkflowDefinitionId = workflowDefinitionId,
            TriggerType = TriggerType.Manual,
            TriggerMetadata = JsonSerializer.Serialize(new { InputParameters = inputParameters }),
            Status = ExecutionStatus.Queued,
            QueuedAt = DateTime.UtcNow
        };
    }
}

public enum TriggerType
{
    Manual = 0,
    Schedule = 1,
    Event = 2,
    Webhook = 3
}

public enum ExecutionStatus
{
    Queued = 0,      // 已加入執行隊列
    Running = 1,     // 正在執行
    Completed = 2,   // 執行完成
    Failed = 3,      // 執行失敗
    Cancelled = 4    // 已取消
}
```

**API 設計**:

```yaml
POST /api/v1/workflow-executions/trigger:
  描述: 手動觸發 Workflow 執行
  請求:
    {
      "workflowDefinitionId": "uuid",
      "inputParameters": { ... } // optional
    }
  響應: 201 Created + WorkflowExecutionDto
  錯誤: 400 (invalid workflowDefinitionId), 404 (workflow not found)

GET /api/v1/workflow-executions/{id}:
  描述: 查詢執行狀態
  響應: 200 OK + WorkflowExecutionDto (包含 status, result, error)

GET /api/v1/workflow-executions:
  描述: 查詢執行歷史 (分頁)
  查詢參數: ?workflowDefinitionId=xxx&status=xxx&pageNumber=1&pageSize=10
  響應: 200 OK + PagedResult<WorkflowExecutionDto>
```

---

### Schedule Trigger 設計 (Cron)

**WorkflowSchedule 實體**:

```csharp
public class WorkflowSchedule : BaseEntity
{
    public Guid ScheduleId { get; private set; }
    public Guid WorkflowDefinitionId { get; private set; }

    public string CronExpression { get; private set; } // "0 0 * * *" (daily at midnight)
    public bool IsActive { get; private set; }

    public DateTime? NextExecutionTime { get; private set; }
    public DateTime? LastExecutionTime { get; private set; }
    public int ExecutionCount { get; private set; }

    public static WorkflowSchedule Create(Guid workflowDefinitionId, string cronExpression)
    {
        // Validate cron expression
        var validator = new CronExpressionValidator();
        if (!validator.IsValid(cronExpression))
            throw new InvalidCronExpressionException(cronExpression);

        var nextExecutionTime = CronHelper.GetNextOccurrence(cronExpression);

        return new WorkflowSchedule
        {
            ScheduleId = Guid.NewGuid(),
            WorkflowDefinitionId = workflowDefinitionId,
            CronExpression = cronExpression,
            IsActive = true,
            NextExecutionTime = nextExecutionTime
        };
    }
}
```

**Background Service 實作**:

```csharp
public class WorkflowSchedulerService : BackgroundService
{
    private readonly IServiceProvider _serviceProvider;
    private readonly ILogger<WorkflowSchedulerService> _logger;
    private readonly TimeSpan _checkInterval = TimeSpan.FromMinutes(1);

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                await CheckAndTriggerScheduledWorkflowsAsync();
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Error checking scheduled workflows");
            }

            await Task.Delay(_checkInterval, stoppingToken);
        }
    }

    private async Task CheckAndTriggerScheduledWorkflowsAsync()
    {
        using var scope = _serviceProvider.CreateScope();
        var repository = scope.ServiceProvider.GetRequiredService<IWorkflowScheduleRepository>();
        var mediator = scope.ServiceProvider.GetRequiredService<IMediator>();

        var now = DateTime.UtcNow;
        var dueSchedules = await repository.GetDueSchedulesAsync(now);

        foreach (var schedule in dueSchedules)
        {
            // Trigger workflow execution
            await mediator.Send(new TriggerScheduledWorkflowCommand
            {
                WorkflowDefinitionId = schedule.WorkflowDefinitionId,
                ScheduleId = schedule.ScheduleId
            });

            // Update NextExecutionTime
            schedule.UpdateNextExecutionTime(CronHelper.GetNextOccurrence(schedule.CronExpression));
            await repository.UpdateAsync(schedule);
        }
    }
}
```

**Cron Expression 驗證**:

```csharp
public class CronExpressionValidator
{
    public bool IsValid(string cronExpression)
    {
        try
        {
            // Use Cronos library
            var expression = CronExpression.Parse(cronExpression);
            return true;
        }
        catch
        {
            return false;
        }
    }
}

public static class CronHelper
{
    public static DateTime GetNextOccurrence(string cronExpression)
    {
        var expression = CronExpression.Parse(cronExpression);
        return expression.GetNextOccurrence(DateTime.UtcNow, TimeZoneInfo.Utc) ?? DateTime.MaxValue;
    }
}
```

**技術決策**: TD-082 - Cron Scheduling 實作策略
- **選擇**: BackgroundService + Cronos library
- **理由**: 簡單、輕量、無額外依賴 (Hangfire 較重)
- **權衡**: BackgroundService 僅適用於單實例, 若需分佈式需遷移到 Hangfire

---

### Event Trigger 設計

**WorkflowEventSubscription 實體**:

```csharp
public class WorkflowEventSubscription : BaseEntity
{
    public Guid SubscriptionId { get; private set; }
    public Guid WorkflowDefinitionId { get; private set; }

    public string EventType { get; private set; } // "user.created", "order.completed"
    public string? EventFilter { get; private set; } // JSON (optional filter conditions)
    public bool IsActive { get; private set; }

    public static WorkflowEventSubscription Create(
        Guid workflowDefinitionId,
        string eventType,
        string? eventFilter = null)
    {
        return new WorkflowEventSubscription
        {
            SubscriptionId = Guid.NewGuid(),
            WorkflowDefinitionId = workflowDefinitionId,
            EventType = eventType,
            EventFilter = eventFilter,
            IsActive = true
        };
    }
}
```

**Event Bus 設計** (Memory-based for MVP):

```csharp
public interface IEventBus
{
    Task PublishAsync<TEvent>(string eventType, TEvent eventData) where TEvent : class;
    void Subscribe<TEvent>(string eventType, Func<TEvent, Task> handler) where TEvent : class;
}

public class InMemoryEventBus : IEventBus
{
    private readonly ConcurrentDictionary<string, List<Func<object, Task>>> _handlers = new();
    private readonly ILogger<InMemoryEventBus> _logger;

    public async Task PublishAsync<TEvent>(string eventType, TEvent eventData) where TEvent : class
    {
        if (_handlers.TryGetValue(eventType, out var handlers))
        {
            foreach (var handler in handlers)
            {
                await handler(eventData);
            }
        }
    }

    public void Subscribe<TEvent>(string eventType, Func<TEvent, Task> handler) where TEvent : class
    {
        var handlers = _handlers.GetOrAdd(eventType, _ => new List<Func<object, Task>>());
        handlers.Add(async (eventData) => await handler((TEvent)eventData));
    }
}
```

**Workflow Event Handler**:

```csharp
public class WorkflowEventHandler
{
    private readonly IMediator _mediator;
    private readonly IWorkflowEventSubscriptionRepository _repository;

    public async Task HandleEventAsync(string eventType, object eventData)
    {
        // Get subscriptions for this event type
        var subscriptions = await _repository.GetByEventTypeAsync(eventType);

        foreach (var subscription in subscriptions.Where(s => s.IsActive))
        {
            // Apply event filter if exists
            if (!string.IsNullOrEmpty(subscription.EventFilter))
            {
                if (!EventFilterMatcher.Matches(eventData, subscription.EventFilter))
                    continue;
            }

            // Trigger workflow
            await _mediator.Send(new TriggerEventWorkflowCommand
            {
                WorkflowDefinitionId = subscription.WorkflowDefinitionId,
                EventType = eventType,
                EventData = eventData
            });
        }
    }
}
```

**技術決策**: TD-083 - Event Bus 選擇
- **MVP**: InMemoryEventBus (簡單、無外部依賴)
- **Phase 2**: 遷移到 MassTransit + RabbitMQ (分佈式、可靠性)

---

### Webhook Trigger 設計

**WorkflowWebhook 實體**:

```csharp
public class WorkflowWebhook : BaseEntity
{
    public Guid WebhookId { get; private set; }
    public Guid WorkflowDefinitionId { get; private set; }

    public string WebhookUrl { get; private set; } // Unique, auto-generated
    public string SecretKey { get; private set; } // For HMAC signature validation
    public bool IsActive { get; private set; }

    public DateTime CreatedAt { get; private set; }
    public DateTime? LastTriggeredAt { get; private set; }
    public int TriggerCount { get; private set; }

    public static WorkflowWebhook Create(Guid workflowDefinitionId)
    {
        return new WorkflowWebhook
        {
            WebhookId = Guid.NewGuid(),
            WorkflowDefinitionId = workflowDefinitionId,
            WebhookUrl = WebhookUrlGenerator.Generate(),
            SecretKey = SecretKeyGenerator.Generate(),
            IsActive = true,
            CreatedAt = DateTime.UtcNow
        };
    }
}
```

**Webhook Security (HMAC-SHA256)**:

```csharp
public class WebhookSecurityService
{
    public string GenerateSignature(string payload, string secretKey)
    {
        using var hmac = new HMACSHA256(Encoding.UTF8.GetBytes(secretKey));
        var hash = hmac.ComputeHash(Encoding.UTF8.GetBytes(payload));
        return Convert.ToBase64String(hash);
    }

    public bool ValidateSignature(string payload, string signature, string secretKey)
    {
        var expectedSignature = GenerateSignature(payload, secretKey);
        return signature == expectedSignature;
    }
}
```

**Webhook Endpoint**:

```csharp
[ApiController]
[Route("webhooks")]
public class WebhookController : ControllerBase
{
    private readonly IMediator _mediator;
    private readonly IWorkflowWebhookRepository _repository;
    private readonly WebhookSecurityService _securityService;

    [HttpPost("{webhookUrl}")]
    public async Task<IActionResult> ReceiveWebhook(string webhookUrl)
    {
        // Get webhook by URL
        var webhook = await _repository.GetByWebhookUrlAsync(webhookUrl);
        if (webhook == null || !webhook.IsActive)
            return NotFound();

        // Validate signature
        var signature = Request.Headers["X-Webhook-Signature"].FirstOrDefault();
        var payload = await new StreamReader(Request.Body).ReadToEndAsync();

        if (string.IsNullOrEmpty(signature) || !_securityService.ValidateSignature(payload, signature, webhook.SecretKey))
            return Unauthorized();

        // Trigger workflow (async)
        var result = await _mediator.Send(new TriggerWebhookWorkflowCommand
        {
            WorkflowDefinitionId = webhook.WorkflowDefinitionId,
            WebhookUrl = webhookUrl,
            Payload = payload
        });

        return Accepted(new { executionId = result.ExecutionId });
    }
}
```

**技術決策**: TD-084 - Webhook Security 實作
- **選擇**: HMAC-SHA256 簽名驗證
- **理由**: 業界標準 (GitHub, Stripe webhooks)
- **安全**: Secret key 不可外露, 僅用於驗證

---

## Workflow Template Management 技術上下文

### Template CRUD 設計

**WorkflowTemplate 實體**:

```csharp
public class WorkflowTemplate : BaseEntity
{
    public Guid TemplateId { get; private set; }
    public string Name { get; private set; }
    public string Description { get; private set; }

    public Guid CategoryId { get; private set; }
    public WorkflowTemplateCategory Category { get; private set; }

    public string DefinitionJson { get; private set; } // Workflow definition snapshot
    public bool IsPublic { get; private set; } // Public in marketplace

    public Guid CreatedBy { get; private set; }
    public DateTime CreatedAt { get; private set; }
    public DateTime? UpdatedAt { get; private set; }

    // Statistics
    public int UsageCount { get; private set; }
    public double AverageRating { get; private set; }

    public void IncrementUsageCount()
    {
        UsageCount++;
        UpdatedAt = DateTime.UtcNow;
    }

    public void UpdateAverageRating(double newAverage)
    {
        AverageRating = newAverage;
        UpdatedAt = DateTime.UtcNow;
    }
}
```

### Template Categories (Hierarchical)

**WorkflowTemplateCategory 實體**:

```csharp
public class WorkflowTemplateCategory : BaseEntity
{
    public Guid CategoryId { get; private set; }
    public string Name { get; private set; }
    public string Description { get; private set; }

    // Hierarchical structure
    public Guid? ParentCategoryId { get; private set; }
    public WorkflowTemplateCategory? ParentCategory { get; private set; }

    public string IconUrl { get; private set; }
    public int DisplayOrder { get; private set; }

    // Navigation
    private readonly List<WorkflowTemplateCategory> _subCategories = new();
    public IReadOnlyCollection<WorkflowTemplateCategory> SubCategories => _subCategories.AsReadOnly();
}
```

**內建 Categories Seed Data**:

```csharp
public static class BuiltInTemplateCategories
{
    public static readonly WorkflowTemplateCategory[] Categories = new[]
    {
        new WorkflowTemplateCategory { Name = "Integration", Description = "整合外部系統", IconUrl = "/icons/integration.svg" },
        new WorkflowTemplateCategory { Name = "Data Processing", Description = "數據處理與轉換", IconUrl = "/icons/data.svg" },
        new WorkflowTemplateCategory { Name = "Automation", Description = "業務流程自動化", IconUrl = "/icons/automation.svg" },
        new WorkflowTemplateCategory { Name = "Notification", Description = "通知與提醒", IconUrl = "/icons/notification.svg" },
        new WorkflowTemplateCategory { Name = "Custom", Description = "自定義模板", IconUrl = "/icons/custom.svg" }
    };
}
```

### Import/Export 設計

**Template Export JSON Schema**:

```json
{
  "version": "1.0",
  "templateMetadata": {
    "name": "Sample Integration Template",
    "description": "Integrate with external API",
    "category": "Integration",
    "author": "John Doe",
    "tags": ["api", "integration"]
  },
  "workflowDefinition": {
    "name": "API Integration Workflow",
    "version": 1,
    "definitionJson": { ... }
  },
  "nodes": [
    {
      "nodeId": "node-1",
      "nodeType": "Agent",
      "configuration": { ... },
      "position": { "x": 100, "y": 100 }
    }
  ],
  "edges": [
    {
      "sourceNodeId": "node-1",
      "targetNodeId": "node-2"
    }
  ]
}
```

**Export Service**:

```csharp
public class TemplateExportService
{
    public async Task<string> ExportToJsonAsync(Guid templateId)
    {
        var template = await _repository.GetByIdWithDetailsAsync(templateId);

        var exportData = new
        {
            Version = "1.0",
            TemplateMetadata = new
            {
                template.Name,
                template.Description,
                Category = template.Category.Name,
                Author = await GetAuthorNameAsync(template.CreatedBy)
            },
            WorkflowDefinition = JsonSerializer.Deserialize<object>(template.DefinitionJson)
        };

        return JsonSerializer.Serialize(exportData, new JsonSerializerOptions { WriteIndented = true });
    }
}
```

**技術決策**: TD-085 - Template Export 格式
- **選擇**: JSON Schema versioning (version 1.0)
- **理由**: 支援未來 schema evolution, 向後兼容

### Marketplace 基礎

**Template Rating**:

```csharp
public class TemplateRating : BaseEntity
{
    public Guid RatingId { get; private set; }
    public Guid TemplateId { get; private set; }
    public Guid UserId { get; private set; }

    public int Rating { get; private set; } // 1-5 stars
    public string? Review { get; private set; }
    public DateTime CreatedAt { get; private set; }
    public DateTime? UpdatedAt { get; private set; }

    public static TemplateRating Create(Guid templateId, Guid userId, int rating, string? review = null)
    {
        if (rating < 1 || rating > 5)
            throw new ArgumentOutOfRangeException(nameof(rating), "Rating must be between 1 and 5");

        return new TemplateRating
        {
            RatingId = Guid.NewGuid(),
            TemplateId = templateId,
            UserId = userId,
            Rating = rating,
            Review = review,
            CreatedAt = DateTime.UtcNow
        };
    }
}
```

**技術決策**: TD-086 - Marketplace Ranking Algorithm
- **選擇**: Weighted average (Rating × 0.7 + Usage × 0.3)
- **理由**: 平衡質量 (Rating) 和流行度 (Usage)

---

## Version Control 技術上下文

### Workflow Versioning 策略

**Versioning Strategy Decision (TD-087)**:

**Option 1**: Create new WorkflowDefinition row for each version (選擇)
- 優點: 簡單, 每個版本獨立 row
- 缺點: 可能產生大量 rows

**Option 2**: WorkflowVersion 獨立表
- 優點: WorkflowDefinition 保持單一 row
- 缺點: 需要額外 JOIN, 複雜度增加

**Decision**: 選擇 Option 1 (簡化 MVP 實現)

**WorkflowDefinition Versioning 調整**:

```csharp
public class WorkflowDefinition : BaseEntity
{
    // Existing fields
    public Guid Id { get; private set; }
    public string Name { get; private set; }

    // Versioning fields
    public int Version { get; private set; } // Auto-increment
    public bool IsActive { get; private set; } // 只有一個版本 IsActive = true
    public Guid? BaseDefinitionId { get; private set; } // FK to original definition
    public string? VersionComment { get; private set; } // Version change description

    public ChangeType ChangeType { get; private set; } // Created/Updated/Rollback

    public WorkflowDefinition CreateNewVersion(string versionComment)
    {
        var newVersion = new WorkflowDefinition
        {
            Id = Guid.NewGuid(),
            Name = this.Name,
            DefinitionJson = this.DefinitionJson, // Copy from current
            Version = this.Version + 1,
            IsActive = true,
            BaseDefinitionId = this.BaseDefinitionId ?? this.Id, // Point to original
            VersionComment = versionComment,
            ChangeType = ChangeType.Updated,
            CreatedAt = DateTime.UtcNow
        };

        this.IsActive = false; // Deactivate current version
        return newVersion;
    }
}

public enum ChangeType
{
    Created = 0,
    Updated = 1,
    Rollback = 2
}
```

### Version Comparison (Diff Algorithm)

**JSON Diff Implementation**:

```csharp
public class WorkflowVersionDiffService
{
    public WorkflowVersionDiff CompareVersions(WorkflowDefinition v1, WorkflowDefinition v2)
    {
        var def1 = JsonSerializer.Deserialize<WorkflowDefinitionJson>(v1.DefinitionJson);
        var def2 = JsonSerializer.Deserialize<WorkflowDefinitionJson>(v2.DefinitionJson);

        var diff = new WorkflowVersionDiff
        {
            AddedNodes = def2.Nodes.Where(n2 => !def1.Nodes.Any(n1 => n1.NodeId == n2.NodeId)).ToList(),
            RemovedNodes = def1.Nodes.Where(n1 => !def2.Nodes.Any(n2 => n2.NodeId == n1.NodeId)).ToList(),
            ModifiedNodes = def1.Nodes
                .Join(def2.Nodes, n1 => n1.NodeId, n2 => n2.NodeId, (n1, n2) => new { n1, n2 })
                .Where(x => !JsonEquals(x.n1, x.n2))
                .Select(x => new NodeDiff { Old = x.n1, New = x.n2 })
                .ToList(),

            AddedEdges = def2.Edges.Where(e2 => !def1.Edges.Any(e1 => EdgeEquals(e1, e2))).ToList(),
            RemovedEdges = def1.Edges.Where(e1 => !def2.Edges.Any(e2 => EdgeEquals(e1, e2))).ToList()
        };

        return diff;
    }
}
```

### Version Rollback

**Rollback Logic**:

```csharp
public async Task<WorkflowDefinition> RollbackToVersionAsync(Guid currentVersionId, int targetVersion)
{
    var currentDef = await _repository.GetByIdAsync(currentVersionId);
    var targetDef = await _repository.GetByVersionAsync(currentDef.BaseDefinitionId ?? currentVersionId, targetVersion);

    if (targetDef == null)
        throw new VersionNotFoundException(targetVersion);

    // Create new version with target's DefinitionJson
    var rolledBackVersion = new WorkflowDefinition
    {
        Id = Guid.NewGuid(),
        Name = currentDef.Name,
        DefinitionJson = targetDef.DefinitionJson, // Copy from target version
        Version = currentDef.Version + 1,
        IsActive = true,
        BaseDefinitionId = currentDef.BaseDefinitionId ?? currentDef.Id,
        VersionComment = $"Rolled back to version {targetVersion}",
        ChangeType = ChangeType.Rollback,
        CreatedAt = DateTime.UtcNow
    };

    currentDef.IsActive = false;
    await _repository.AddAsync(rolledBackVersion);
    await _repository.UpdateAsync(currentDef);

    return rolledBackVersion;
}
```

---

## 技術決策記錄

### TD-082: Cron Scheduling 實作策略

**決策**: BackgroundService + Cronos library
**理由**: 簡單、輕量、無外部依賴
**權衡**: 僅適用於單實例部署
**未來**: 若需分佈式, 遷移到 Hangfire

### TD-083: Event Bus 選擇

**MVP**: InMemoryEventBus
**Phase 2**: MassTransit + RabbitMQ
**理由**: MVP 簡化實現, Phase 2 增加可靠性

### TD-084: Webhook Security

**選擇**: HMAC-SHA256
**理由**: 業界標準 (GitHub, Stripe)
**安全**: Secret key 保密

### TD-085: Template Export 格式

**選擇**: JSON Schema versioning (v1.0)
**理由**: 支援未來 schema evolution

### TD-086: Marketplace Ranking

**選擇**: Weighted average (Rating × 0.7 + Usage × 0.3)
**理由**: 平衡質量和流行度

### TD-087: Versioning Strategy

**選擇**: New row per version
**理由**: 簡化 MVP 實現

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**Phase 1C 完成**: Sprint 11 結束時達到 100%
