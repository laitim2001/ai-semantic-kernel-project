# Sprint [X] 執行計劃

**目的**: 提供詳細的技術實施指引,讓 AI 開發助手能夠準確理解和執行開發任務

---

## 📋 本 Sprint 要實現的功能 (What to Build)

### US X.1: [User Story 標題]

#### 🎯 MVP 範圍定義

**✅ 必須實現 (P0 - 本 Sprint)**:
- [ ] **功能點 1**: [具體描述這個功能點要做什麼]
  - 輸入: [...]
  - 輸出: [...]
  - 業務邏輯: [...]
- [ ] **功能點 2**: [具體描述]
- [ ] **功能點 3**: [具體描述]

**🔶 可選實現 (P1 - 如果時間允許)**:
- [ ] **功能點 4**: [Nice to have 的功能]

**❌ Phase 2 功能 (明確排除,不在本 Sprint)**:
- ❌ **功能點 5**: [明確說明這個不做,例如: 3 欄布局]
- ❌ **功能點 6**: [明確說明這個不做,例如: Function Calling 可視化]

#### 📚 技術規格摘要

**來源**: `/docs/user-stories/modules/module-XX.md` 第 [line-start] - [line-end] 行 (精簡版)

##### Backend API 規格

**API 端點**: `[METHOD] /api/v1/[resource]`

**Request**:
```json
{
  "field1": "string",
  "field2": "integer"
}
```

**Response**:
```json
{
  "id": "uuid",
  "field1": "string",
  "createdAt": "ISO8601"
}
```

**驗證規則**:
- `field1`: 必填, 長度 3-100 字元
- `field2`: 必填, 範圍 1-1000

**錯誤處理**:
- `400 Bad Request`: [何時返回]
- `404 Not Found`: [何時返回]
- `500 Internal Server Error`: [何時返回]

##### Database Schema

**Table**: `[TableName]`

```sql
CREATE TABLE [TableName] (
    Id UUID PRIMARY KEY,
    Field1 VARCHAR(100) NOT NULL,
    Field2 INTEGER NOT NULL,
    CreatedAt TIMESTAMP DEFAULT NOW(),
    UpdatedAt TIMESTAMP DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_[table]_[field] ON [TableName]([Field]);
```

**關聯**:
- `ForeignKey`: 關聯到 `[OtherTable].[Field]`

##### Frontend UI 規格

**來源**: `/docs/ux-design/wireframes/low-fidelity/XX.md` (精簡版,僅 MVP 範圍)

**本 Sprint 實現的 UI 組件**:

**1. [Component Name]**
- **位置**: [描述在頁面的哪個位置]
- **Layout**: [描述佈局,例如: 2 欄布局 (Sidebar + Main Area)]
- **主要元素**:
  - Element 1: [描述,例如: Button - 位置: Top Right]
  - Element 2: [描述,例如: List - 位置: Left Sidebar]
- **交互行為**:
  - Action 1: [描述用戶操作和系統響應]
  - Action 2: [描述]

**本 Sprint 不實現的 UI** (Phase 2):
- ❌ **3 欄布局**: 暫時使用 2 欄布局
- ❌ **Function Calling 可視化面板**: Phase 2 實現

##### 性能標準

**來源**: `/docs/user-stories/epics/epic-XX.md`

- **響應時間**: P95 <500ms, P99 <1000ms
- **吞吐量**: ≥1000 requests/second
- **並發用戶**: ≥100 concurrent users
- **數據量**: 支持 ≥10,000 records

##### 安全要求

- **認證**: [描述認證機制,例如: JWT Bearer Token]
- **授權**: [描述權限檢查,例如: RBAC - Admin, Developer, Viewer]
- **數據驗證**: [描述輸入驗證規則]
- **敏感數據**: [描述加密和脫敏要求]

#### ✅ 驗收標準 (Definition of Done)

**功能驗收**:
- [ ] 所有 P0 功能點完成並可演示
- [ ] 符合技術規格要求
- [ ] 通過所有驗收測試

**技術質量**:
- [ ] 單元測試覆蓋率 ≥80%
- [ ] 集成測試通過
- [ ] Code Review 完成並通過
- [ ] 無 Critical/High severity bugs

**性能驗證**:
- [ ] P95 響應時間 <500ms (經過負載測試驗證)
- [ ] 並發測試通過 (100 concurrent users)
- [ ] 錯誤率 <1%

**文檔完成**:
- [ ] API 文檔更新 (Swagger/OpenAPI)
- [ ] README 更新 (如有需要)
- [ ] 代碼註釋清晰

**UI/UX 驗證** (如適用):
- [ ] UI 符合設計規格 (MVP 範圍)
- [ ] 響應式設計測試通過
- [ ] 無明顯 UI bugs

---

### US X.2: [下一個 User Story]

[重複上述結構]

---

## 🏗️ 技術實施細節

### 後端架構

**項目結構**:
```
src/
├── AIAgentPlatform.Application/
│   ├── [Feature]/
│   │   ├── Commands/
│   │   ├── Queries/
│   │   └── DTOs/
├── AIAgentPlatform.Domain/
│   ├── Entities/
│   └── Interfaces/
└── AIAgentPlatform.Infrastructure/
    ├── Repositories/
    └── Services/
```

**關鍵類和方法**:

**1. Entity**: `src/AIAgentPlatform.Domain/Entities/[Entity].cs`
```csharp
public class [Entity]
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    // ... 其他屬性
}
```

**2. Repository Interface**: `src/AIAgentPlatform.Domain/Interfaces/I[Entity]Repository.cs`
```csharp
public interface I[Entity]Repository
{
    Task<[Entity]> GetByIdAsync(Guid id);
    Task<IEnumerable<[Entity]>> GetAllAsync();
    Task AddAsync([Entity] entity);
    Task UpdateAsync([Entity] entity);
    Task DeleteAsync(Guid id);
}
```

**3. Command/Query (CQRS)**: `src/AIAgentPlatform.Application/[Feature]/Commands/[Command].cs`
```csharp
public class [Command] : IRequest<[Response]>
{
    public string Property { get; set; }
}

public class [Command]Handler : IRequestHandler<[Command], [Response]>
{
    public async Task<[Response]> Handle([Command] request, CancellationToken cancellationToken)
    {
        // 實現邏輯
    }
}
```

### 前端架構

**組件結構**:
```
apps/web-app/src/
├── features/
│   └── [feature]/
│       ├── components/
│       ├── hooks/
│       ├── types/
│       └── [FeaturePage].tsx
```

**關鍵組件**:

**1. 主頁面組件**: `apps/web-app/src/features/[feature]/[FeaturePage].tsx`
```typescript
export const [FeaturePage]: React.FC = () => {
  // State management
  const [data, setData] = useState<DataType[]>([]);

  // API calls
  useEffect(() => {
    // Fetch data
  }, []);

  return (
    // JSX
  );
};
```

**2. Custom Hook**: `apps/web-app/src/features/[feature]/hooks/use[Feature].ts`
```typescript
export const use[Feature] = () => {
  // Hook logic
  return { data, isLoading, error };
};
```

### 數據流

```
User Action → Frontend Component → API Call → Backend Controller
→ Application Layer (Command/Query Handler) → Domain Logic → Repository
→ Database → Response → Frontend State Update → UI Re-render
```

---

## 🧪 測試策略

### Backend 測試

**單元測試**: `tests/AIAgentPlatform.UnitTests/`
```csharp
[Fact]
public async Task [TestName]_Should[ExpectedBehavior]_When[Condition]()
{
    // Arrange
    // Act
    // Assert
}
```

**集成測試**: `tests/AIAgentPlatform.IntegrationTests/`
```csharp
public class [Feature]ControllerTests : IClassFixture<WebApplicationFactory<Program>>
{
    [Fact]
    public async Task [TestName]() { }
}
```

### Frontend 測試

**組件測試**: `apps/web-app/src/features/[feature]/__tests__/`
```typescript
describe('[Component]', () => {
  it('should render correctly', () => {
    render(<[Component] />);
    expect(screen.getByText('...')).toBeInTheDocument();
  });
});
```

### E2E 測試

**Playwright 測試**: `tests/e2e/`
```typescript
test('[Feature] workflow', async ({ page }) => {
  // Test user journey
});
```

---

## 📦 部署和配置

### 環境變量

**Backend** (`.env`):
```
DATABASE_URL=postgresql://...
AZURE_OPENAI_ENDPOINT=https://...
AZURE_OPENAI_KEY=...
```

**Frontend** (`.env.local`):
```
VITE_API_BASE_URL=http://localhost:5000
```

### Migration

```bash
# 創建 Migration
dotnet ef migrations add [MigrationName] --project src/AIAgentPlatform.Infrastructure

# 應用 Migration
dotnet ef database update --project src/AIAgentPlatform.Infrastructure
```

---

## 🔗 相關資源

**完整規劃文檔** (詳細參考):
- Module 定義: `/docs/user-stories/modules/module-XX.md`
- Epic 詳細: `/docs/user-stories/epics/epic-XX.md`
- 架構設計: `/docs/architecture/[relevant-adr].md`
- API 規範: `/docs/technical-implementation/04-API-SPECIFICATION.md`
- UI 設計: `/docs/ux-design/wireframes/low-fidelity/XX.md`

**本 Sprint 其他文檔**:
- 📋 [SPRINT-[X]-OVERVIEW.md](./SPRINT-[X]-OVERVIEW.md)
- ✅ [SPRINT-[X]-CHECKLIST.md](./SPRINT-[X]-CHECKLIST.md)
- 📝 [SPRINT-[X]-DEV-LOG.md](./SPRINT-[X]-DEV-LOG.md)

---

**創建日期**: YYYY-MM-DD
**最後更新**: YYYY-MM-DD
**版本**: v1.0
