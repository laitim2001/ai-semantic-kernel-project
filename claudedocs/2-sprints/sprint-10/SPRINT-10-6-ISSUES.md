# SPRINT 10 - 問題追蹤 (Issue Tracking)

---

## 📋 文件資訊 (Document Information)

| 欄位 | 內容 |
|------|------|
| **版本** | v2.1 |
| **Sprint 編號** | Sprint 10 |
| **Sprint 週期** | Week 28-30 (3 週) |
| **Phase** | Phase 1C - 工作流編輯器 Backend (Workflow Editor Backend) |
| **計劃日期** | 2026-04-21 ~ 2026-05-11 |
| **狀態** | 📋 計劃階段 (Planned) |
| **創建日期** | 2025-11-14 |
| **最後更新** | 2025-11-14 |

---

## 📑 目錄 (Table of Contents)

1. [問題追蹤系統說明](#問題追蹤系統說明)
2. [問題統計概覽](#問題統計概覽)
3. [Critical Issues (P0)](#critical-issues-p0)
4. [High Priority Issues (P1)](#high-priority-issues-p1)
5. [Medium Priority Issues (P2)](#medium-priority-issues-p2)
6. [Low Priority Issues (P3)](#low-priority-issues-p3)
7. [已解決問題](#已解決問題)
8. [常見問題與解決方案](#常見問題與解決方案)
9. [技術債務追蹤](#技術債務追蹤)

---

## 問題追蹤系統說明

### 問題分類

Sprint 10 問題追蹤使用以下標籤系統:

| 標籤 | 含義 | 顏色 | 使用場景 |
|------|------|------|----------|
| 🐛 Bug | 程式錯誤 | 紅色 | 功能不正常, 產生錯誤 |
| 🔥 Blocker | 阻礙問題 | 深紅色 | 阻止開發進行, 必須立即解決 |
| ⚠️ Warning | 警告 | 橙色 | 潛在問題, 需要注意 |
| 💡 Enhancement | 改進建議 | 藍色 | 功能增強, 性能優化 |
| 📝 Documentation | 文檔相關 | 綠色 | 文檔缺失, 需要更新 |
| 🧪 Testing | 測試相關 | 紫色 | 測試失敗, 覆蓋率不足 |
| 🏗️ Architecture | 架構相關 | 灰色 | 設計問題, 架構調整 |

### 優先級定義

| 優先級 | 符號 | 定義 | 解決時限 |
|--------|------|------|----------|
| P0 - Critical | 🔴 | 阻止 Sprint 完成, 影響核心功能 | 24 小時內 |
| P1 - High | 🟡 | 影響重要功能, 需盡快解決 | 3 天內 |
| P2 - Medium | 🟢 | 影響次要功能, 可以延後 | 1 週內 |
| P3 - Low | ⚪ | 影響輕微, 可以跳過 | Sprint 結束前 |

### 問題狀態

| 狀態 | 符號 | 定義 |
|------|------|------|
| Open | 🔓 | 問題已記錄, 待處理 |
| In Progress | 🔄 | 問題正在解決中 |
| Blocked | ⏸️ | 問題因其他依賴而暫停 |
| Resolved | ✅ | 問題已解決, 待驗證 |
| Closed | 🔒 | 問題已驗證並關閉 |
| Deferred | 🗓️ | 問題延後到下個 Sprint |

### 問題記錄格式

每個問題包含:
- **ID**: 唯一標識 (I10-XXX)
- **標題**: 簡短描述
- **標籤**: 問題類型標籤
- **優先級**: P0 / P1 / P2 / P3
- **狀態**: Open / In Progress / Resolved / Closed
- **報告人**: 發現問題的人
- **負責人**: 負責解決問題的人
- **創建日期**: 問題創建時間
- **更新日期**: 最後更新時間
- **描述**: 詳細問題描述
- **重現步驟**: 如何重現問題 (Bug)
- **預期行為**: 應該如何運作
- **實際行為**: 當前如何運作
- **影響範圍**: 影響哪些模塊/功能
- **解決方案**: 提議的解決方法
- **驗證標準**: 如何驗證已解決

---

## 問題統計概覽

### 整體統計

| 統計項 | 數量 | 百分比 |
|--------|------|--------|
| 總問題數 | 0 | - |
| 開放問題 | 0 | 0% |
| 進行中問題 | 0 | 0% |
| 已解決問題 | 0 | 0% |
| 已關閉問題 | 0 | 0% |
| 延後問題 | 0 | 0% |

### 按優先級統計

| 優先級 | 開放 | 進行中 | 已解決 | 已關閉 | 總計 |
|--------|------|--------|--------|--------|------|
| P0 - Critical | 0 | 0 | 0 | 0 | 0 |
| P1 - High | 0 | 0 | 0 | 0 | 0 |
| P2 - Medium | 0 | 0 | 0 | 0 | 0 |
| P3 - Low | 0 | 0 | 0 | 0 | 0 |

### 按標籤統計

| 標籤 | 數量 | 百分比 |
|------|------|--------|
| 🐛 Bug | 0 | 0% |
| 🔥 Blocker | 0 | 0% |
| ⚠️ Warning | 0 | 0% |
| 💡 Enhancement | 0 | 0% |
| 📝 Documentation | 0 | 0% |
| 🧪 Testing | 0 | 0% |
| 🏗️ Architecture | 0 | 0% |

### 按負責人統計

| 負責人 | 開放 | 進行中 | 已解決 | 總計 |
|--------|------|--------|--------|------|
| Architect | 0 | 0 | 0 | 0 |
| Dev Lead | 0 | 0 | 0 | 0 |
| Backend Dev | 0 | 0 | 0 | 0 |
| QA Lead | 0 | 0 | 0 | 0 |

---

## Critical Issues (P0)

**定義**: 阻止 Sprint 完成, 影響核心功能, 需要立即解決 (24 小時內)

_目前沒有 P0 Critical Issues_

### P0 問題模板 (供參考)

---

#### I10-P0-001: [模板] Critical Issue 標題

**ID**: I10-P0-001
**標籤**: 🔥 Blocker, 🐛 Bug
**優先級**: 🔴 P0 - Critical
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 重現步驟
1. _待填寫_
2. _待填寫_
3. _待填寫_

##### 預期行為
_待填寫_

##### 實際行為
_待填寫_

##### 影響範圍
- **影響的模塊**: _待填寫_
- **影響的功能**: _待填寫_
- **阻礙的任務**: _待填寫_

##### 解決方案
_待提議_

##### 驗證標準
- [ ] _待填寫_
- [ ] _待填寫_

---

## High Priority Issues (P1)

**定義**: 影響重要功能, 需盡快解決 (3 天內)

_目前沒有 P1 High Priority Issues_

### P1 問題模板 (供參考)

---

#### I10-P1-001: [模板] High Priority Issue 標題

**ID**: I10-P1-001
**標籤**: 🐛 Bug
**優先級**: 🟡 P1 - High
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 重現步驟
1. _待填寫_
2. _待填寫_

##### 預期行為
_待填寫_

##### 實際行為
_待填寫_

##### 影響範圍
- **影響的模塊**: _待填寫_
- **影響的功能**: _待填寫_

##### 解決方案
_待提議_

##### 驗證標準
- [ ] _待填寫_

---

## Medium Priority Issues (P2)

**定義**: 影響次要功能, 可以延後 (1 週內)

_目前沒有 P2 Medium Priority Issues_

### P2 問題模板 (供參考)

---

#### I10-P2-001: [模板] Medium Priority Issue 標題

**ID**: I10-P2-001
**標籤**: 💡 Enhancement
**優先級**: 🟢 P2 - Medium
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 改進建議
_待提議_

##### 預期效益
_待評估_

##### 影響範圍
- **影響的模塊**: _待填寫_

##### 實施計劃
_待規劃_

##### 驗證標準
- [ ] _待填寫_

---

## Low Priority Issues (P3)

**定義**: 影響輕微, 可以跳過 (Sprint 結束前)

_目前沒有 P3 Low Priority Issues_

### P3 問題模板 (供參考)

---

#### I10-P3-001: [模板] Low Priority Issue 標題

**ID**: I10-P3-001
**標籤**: 📝 Documentation
**優先級**: ⚪ P3 - Low
**狀態**: 🔓 Open
**報告人**: _待填寫_
**負責人**: _待指定_
**創建日期**: _待填寫_
**更新日期**: _待填寫_

##### 描述
_待填寫_

##### 改進建議
_待提議_

---

## 已解決問題

**定義**: 已解決並驗證的問題

_目前沒有已解決問題_

### 已解決問題模板 (供參考)

---

#### I10-RESOLVED-001: [模板] 已解決問題標題

**ID**: I10-RESOLVED-001
**標籤**: 🐛 Bug
**優先級**: 🟡 P1 - High
**狀態**: 🔒 Closed
**報告人**: _待填寫_
**負責人**: _待填寫_
**創建日期**: _待填寫_
**解決日期**: _待填寫_
**關閉日期**: _待填寫_

##### 描述
_待填寫_

##### 解決方案
_已實施_

##### 驗證結果
✅ 所有驗證標準已通過

##### 相關 Commit
- Commit: _待填寫_
- PR: _待填寫_

---

## 常見問題與解決方案

### Workflow Definition 相關

---

#### FAQ-001: WorkflowDefinition 應該使用 JSONB 還是關聯式表?

**類別**: 🏗️ Architecture
**頻率**: 常見

##### 問題描述
在設計 WorkflowDefinition 存儲時, 應該使用 JSONB 欄位還是完全關聯式表結構?

##### 解決方案
**推薦: 混合模式**

**設計原則**:
- WorkflowDefinitions 表存儲 metadata (Name, Status, Version, CreatedAt)
- DefinitionJson JSONB 欄位存儲完整 workflow definition
- NodeDefinitions 表存儲結構化 Node 數據
- EdgeDefinitions 表存儲結構化 Edge 數據

**優點**:
- ✅ Metadata 查詢快速 (Name, Status 等)
- ✅ 支援 schema evolution (JSONB 欄位)
- ✅ 結構化數據便於驗證和查詢
- ✅ 支援複雜查詢 (JOIN + JSONB 查詢)

**注意事項**:
- ⚠️ 需要保持 DefinitionJson 和結構化表的數據一致性
- ⚠️ 使用 Domain Events 確保同步
- ⚠️ 在 Repository 層封裝同步邏輯

##### 參考文檔
- TD-076: Workflow Schema 存儲格式決策
- docs/database/workflow-schema.md

---

#### FAQ-002: 如何處理 Workflow Version 管理?

**類別**: 🏗️ Architecture
**頻率**: 常見

##### 問題描述
WorkflowDefinition 的 Version 欄位應該如何管理? 是自動遞增還是手動設置?

##### 解決方案
**Sprint 10: 簡化版本控制**
- Version 欄位預留 (integer)
- 目前固定為 1 (創建時設置)
- 更新時不改變 Version (覆蓋原版本)

**Sprint 11: 完整版本控制**
- 每次更新創建新版本 (Version++)
- 保留歷史版本 (IsActive flag)
- 支援版本回滾

**示例**:
```csharp
// Sprint 10: 簡化版本
public static WorkflowDefinition Create(string name, string definitionJson)
{
    return new WorkflowDefinition
    {
        Id = Guid.NewGuid(),
        Name = name,
        DefinitionJson = definitionJson,
        Version = 1, // 固定為 1
        CreatedAt = DateTime.UtcNow
    };
}

// Sprint 11: 完整版本控制
public WorkflowDefinition CreateNewVersion()
{
    var newVersion = new WorkflowDefinition
    {
        Id = Guid.NewGuid(),
        Name = this.Name,
        DefinitionJson = this.DefinitionJson,
        Version = this.Version + 1, // 自動遞增
        CreatedAt = DateTime.UtcNow
    };

    this.IsActive = false; // 舊版本設為非活躍
    return newVersion;
}
```

##### 參考文檔
- SPRINT-10-3-CONTEXT.md: WorkflowDefinition Design
- Sprint 11 計劃 (Schema Versioning)

---

### Node Registry 相關

---

#### FAQ-003: 如何擴展 Node Registry 支援自定義 Node types?

**類別**: 💡 Enhancement
**頻率**: 常見

##### 問題描述
未來需要支援第三方 Plugin 註冊自定義 Node types, 如何設計?

##### 解決方案
**當前設計已預留擴展空間**:

1. **INodeTypeRegistry 介面**:
   - `RegisterNodeType(NodeTypeDefinition)` 支援動態註冊
   - 使用 ConcurrentDictionary 確保線程安全

2. **未來 Plugin 整合**:
```csharp
// Plugin 載入時註冊自定義 Node type
public class CustomNodePlugin : IPlugin
{
    public void Initialize(IServiceProvider serviceProvider)
    {
        var registry = serviceProvider.GetRequiredService<INodeTypeRegistry>();

        registry.RegisterNodeType(new NodeTypeDefinition
        {
            Type = "CustomEmailNode",
            DisplayName = "Custom Email Sender",
            ConfigurationSchema = @"{ ... JSON Schema ... }",
            CompatibleSourceTypes = new List<string> { "Agent", "Tool" },
            CompatibleTargetTypes = new List<string> { "Agent", "End" }
        });
    }
}
```

3. **驗證機制**:
   - JSON Schema validation
   - Compatibility matrix 檢查
   - Node configuration 驗證

##### 參考文檔
- TD-077: Node Registry 實作策略決策
- docs/architecture/plugin-architecture.md (Phase 2)

---

#### FAQ-004: Node Configuration JSON Schema 應該如何設計?

**類別**: 📝 Documentation
**頻率**: 常見

##### 問題描述
為每種 Node type 設計 JSON Schema 時, 應該遵循什麼原則?

##### 解決方案
**JSON Schema 設計原則**:

1. **明確必填欄位**:
```json
{
  "type": "object",
  "properties": {
    "agentId": { "type": "string", "format": "uuid" },
    "parameters": { "type": "object" }
  },
  "required": ["agentId"]
}
```

2. **提供預設值**:
```json
{
  "timeout": {
    "type": "integer",
    "minimum": 1,
    "maximum": 3600,
    "default": 30
  }
}
```

3. **詳細描述**:
```json
{
  "agentId": {
    "type": "string",
    "format": "uuid",
    "description": "The unique identifier of the Agent to execute"
  }
}
```

4. **支援驗證**:
   - 使用 `format`: uuid, email, url
   - 使用 `minimum`/`maximum`: 數值範圍
   - 使用 `pattern`: 正則表達式

##### 參考文檔
- https://json-schema.org/
- SPRINT-10-3-CONTEXT.md: JSON Schema Validation

---

### Edge Validation 相關

---

#### FAQ-005: Circular Dependency Detection 如何處理複雜情況?

**類別**: 🧪 Testing
**頻率**: 較少

##### 問題描述
DFS 算法能否正確檢測所有類型的 circular dependency? 有哪些 edge cases?

##### 解決方案
**DFS 算法可以檢測所有循環**, 但需要測試以下 edge cases:

1. **簡單循環**: A → B → A
2. **長鏈循環**: A → B → C → D → A
3. **多個循環**: A → B → A, C → D → C (獨立)
4. **巢狀循環**: A → B → C → B (內部循環) + A → D → A (外部循環)
5. **Self-loop**: A → A
6. **深度嵌套**: >50 nodes 的深度循環

**測試策略**:
```csharp
[Theory]
[InlineData("A->B,B->A", true)] // Simple cycle
[InlineData("A->B,B->C,C->A", true)] // 3-node cycle
[InlineData("A->B,B->C,C->D", false)] // No cycle
[InlineData("A->A", true)] // Self-loop
public void DetectCircularDependency_VariousScenarios_ReturnsExpected(
    string edgeDefinition,
    bool expectedHasCycle)
{
    // Test implementation
}
```

**性能考量**:
- 時間複雜度: O(V + E)
- 空間複雜度: O(V)
- 實際 Workflow 通常 <100 nodes, 性能足夠

##### 參考文檔
- TD-078: Circular Dependency Detection 算法決策
- SPRINT-10-3-CONTEXT.md: Edge Validation Engine

---

#### FAQ-006: Node Compatibility Matrix 如何維護?

**類別**: 🏗️ Architecture
**頻率**: 較少

##### 問題描述
當新增 Node types 時, 如何更新 Compatibility Matrix?

##### 解決方案
**當前設計: 每種 Node type 定義自己的 compatibility**

1. **BuiltInNodeTypes 定義**:
```csharp
public static readonly NodeTypeDefinition Agent = new()
{
    Type = "Agent",
    CompatibleSourceTypes = new List<string> { "Start", "Tool", "Condition", "Agent" },
    CompatibleTargetTypes = new List<string> { "Tool", "Condition", "Agent", "End" }
};
```

2. **新增 Node type 時更新**:
```csharp
registry.RegisterNodeType(new NodeTypeDefinition
{
    Type = "NewNodeType",
    CompatibleSourceTypes = new List<string> { "Agent", "Tool" },
    CompatibleTargetTypes = new List<string> { "Agent", "End" }
});
```

3. **驗證邏輯**:
```csharp
public bool AreCompatible(string sourceNodeType, string targetNodeType)
{
    var sourceDef = GetNodeType(sourceNodeType);
    return sourceDef?.CompatibleTargetTypes.Contains(targetNodeType) ?? false;
}
```

**優點**:
- ✅ 去中心化: 每種 Node type 管理自己的規則
- ✅ 易於擴展: 新增 Node type 不影響現有規則
- ✅ 靈活: 支援動態註冊

##### 參考文檔
- SPRINT-10-3-CONTEXT.md: Node Compatibility Matrix

---

### API 設計相關

---

#### FAQ-007: API 響應時間如何優化?

**類別**: ⚠️ Warning
**頻率**: 較少

##### 問題描述
Workflow Definition API 響應時間目標是 P95 <200ms, 如何達成?

##### 解決方案
**優化策略**:

1. **Database 查詢優化**:
   - 使用索引 (Name, Status, CreatedAt)
   - 使用分頁 (PageNumber, PageSize)
   - 避免 N+1 查詢 (使用 Include)

```csharp
public async Task<WorkflowDefinition?> GetByIdWithDetailsAsync(Guid id)
{
    return await _dbContext.WorkflowDefinitions
        .Include(w => w.Nodes)
        .Include(w => w.Edges)
        .FirstOrDefaultAsync(w => w.Id == id && !w.IsDeleted);
}
```

2. **JSONB 查詢優化**:
   - 僅在需要時查詢 DefinitionJson (避免大欄位傳輸)
   - 使用 PostgreSQL JSONB 索引 (GIN index)

3. **Caching** (Sprint 11):
   - Redis 緩存熱門 Workflow Definition
   - 緩存 Node Registry (減少查詢)

4. **性能測試**:
```csharp
[Fact]
public async Task GetWorkflowDefinitionById_Performance_UnderP95Target()
{
    var stopwatch = Stopwatch.StartNew();
    var result = await _controller.GetWorkflowDefinitionById(workflowId);
    stopwatch.Stop();

    Assert.True(stopwatch.ElapsedMilliseconds < 200,
        $"P95 target: <200ms, actual: {stopwatch.ElapsedMilliseconds}ms");
}
```

##### 參考文檔
- docs/performance/performance-targets.md
- SPRINT-10-4-CHECKLIST.md: T10.135 - T10.137

---

#### FAQ-008: API 錯誤響應格式應該如何標準化?

**類別**: 📝 Documentation
**頻率**: 常見

##### 問題描述
API 錯誤響應應該遵循什麼格式? 如何提供有用的錯誤訊息?

##### 解決方案
**使用 RFC 7807 Problem Details**

1. **標準格式**:
```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "Name": ["The Name field is required."],
    "DefinitionJson": ["Invalid JSON format."]
  },
  "traceId": "00-abc123-xyz789-00"
}
```

2. **實施**:
```csharp
// Program.cs
builder.Services.AddProblemDetails();

app.UseExceptionHandler();
app.UseStatusCodePages();
```

3. **FluentValidation 整合**:
```csharp
public class CreateWorkflowDefinitionCommandValidator : AbstractValidator<CreateWorkflowDefinitionCommand>
{
    public CreateWorkflowDefinitionCommandValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Workflow name is required.")
            .MaximumLength(200).WithMessage("Workflow name must not exceed 200 characters.");

        RuleFor(x => x.DefinitionJson)
            .NotEmpty().WithMessage("Workflow definition is required.")
            .Must(BeValidJson).WithMessage("Workflow definition must be valid JSON.");
    }
}
```

##### 參考文檔
- TD-081: Error Response 標準化格式
- docs/api/api-documentation-standards.md

---

## 技術債務追蹤

### 技術債務定義

**技術債務**: 為了快速交付功能而採取的權宜之計, 未來需要重構或改進的地方

### 債務分類

| 類別 | 定義 | 處理策略 |
|------|------|----------|
| 代碼質量 | 重複代碼, 複雜度高, 可讀性差 | 定期重構 |
| 架構設計 | 設計不合理, 耦合度高 | 下個 Sprint 改進 |
| 性能問題 | 性能不達標, 需要優化 | 性能測試後優化 |
| 文檔缺失 | 缺少文檔, 註解不完整 | Sprint 結束前補齊 |
| 測試覆蓋 | 測試覆蓋率不足 | 逐步提高覆蓋率 |

### 技術債務清單

---

#### TD10-001: [預留位置] 技術債務標題

**ID**: TD10-001
**類別**: _待分類_
**嚴重程度**: _待評估_ (High / Medium / Low)
**狀態**: 🔓 Open
**記錄人**: _待填寫_
**創建日期**: _待填寫_

##### 描述
_待填寫_

##### 影響
_待評估_

##### 建議解決方案
_待提議_

##### 預計解決時間
_待估算_ (Story Points)

##### 計劃解決時間
_待規劃_ (Sprint XX)

---

#### TD10-002: [預留位置] 技術債務標題

**ID**: TD10-002
**類別**: _待分類_
**嚴重程度**: _待評估_
**狀態**: 🔓 Open
**記錄人**: _待填寫_
**創建日期**: _待填寫_

##### 描述
_待填寫_

##### 影響
_待評估_

##### 建議解決方案
_待提議_

##### 預計解決時間
_待估算_

##### 計劃解決時間
_待規劃_

---

#### TD10-003: [預留位置] 技術債務標題

**ID**: TD10-003
**類別**: _待分類_
**嚴重程度**: _待評估_
**狀態**: 🔓 Open
**記錄人**: _待填寫_
**創建日期**: _待填寫_

##### 描述
_待填寫_

##### 影響
_待評估_

##### 建議解決方案
_待提議_

##### 預計解決時間
_待估算_

##### 計劃解決時間
_待規劃_

---

### 債務統計

| 類別 | High | Medium | Low | 總計 |
|------|------|--------|-----|------|
| 代碼質量 | 0 | 0 | 0 | 0 |
| 架構設計 | 0 | 0 | 0 | 0 |
| 性能問題 | 0 | 0 | 0 | 0 |
| 文檔缺失 | 0 | 0 | 0 | 0 |
| 測試覆蓋 | 0 | 0 | 0 | 0 |
| **總計** | **0** | **0** | **0** | **0** |

---

## Sprint 10 問題總結

### 問題趨勢分析
_Sprint 結束時填寫_

### 高風險領域
_Sprint 結束時識別_

### 改進建議
_Sprint Retrospective 時提出_

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**總問題數**: 0
**總技術債務**: 0
**FAQ 數量**: 8 個
