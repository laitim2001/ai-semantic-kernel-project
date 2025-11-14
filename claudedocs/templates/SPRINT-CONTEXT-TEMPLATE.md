# Sprint [X] 上下文參考

**目的**: 提供精簡的 /docs 文檔參考,避免 AI 助手在 180+ 文件中搜尋

**原則**: Just-in-Time Documentation - 只提供本 Sprint 需要的精簡資訊

---

## 📚 完整規劃文檔位置 (Reference Layer)

### Module & Epic 定義
- **Module XX 定義**: `/docs/user-stories/modules/module-XX.md`
  - 完整需求規格
  - 技術架構設計
  - 驗收標準
- **Epic XX 詳細**: `/docs/user-stories/epics/epic-XX.md`
  - User Stories 詳細描述
  - BDD 驗收場景
  - 性能標準

### 架構設計文檔
- **ADR-XXX**: `/docs/architecture/[relevant-adr].md`
  - 架構決策記錄
  - 技術選型理由
- **C4 圖**: `/docs/architecture/c4-diagrams/[relevant-diagram].md`
  - 系統架構視圖
  - 組件關係

### UI/UX 設計
- **Wireframe**: `/docs/ux-design/wireframes/low-fidelity/XX-[feature].md`
  - Low-fidelity 設計稿
  - 交互流程
- **Design System**: `/docs/ux-design/design-system/[relevant-doc].md`
  - 組件規範
  - 樣式指南

### API 規範
- **API Specification**: `/docs/technical-implementation/04-API-SPECIFICATION.md`
  - 完整 API 定義
  - 數據模型
  - 錯誤處理

---

## 🎯 US X.1: [User Story 標題]

### MVP 範圍摘要 (精簡版)

**✅ 必須實現的核心功能** (P0):

1. **功能點 1**: [從 Epic 文檔精簡提取]
   - **輸入**: [...]
   - **輸出**: [...]
   - **業務邏輯**: [核心邏輯摘要]

2. **功能點 2**: [從 Epic 文檔精簡提取]
   - **輸入**: [...]
   - **輸出**: [...]

3. **功能點 3**: [從 Epic 文檔精簡提取]

**❌ 明確排除的功能** (Phase 2, 不在本 Sprint):
- ❌ **高級功能 1**: [例如: 3 欄布局 - Phase 2 實現]
- ❌ **高級功能 2**: [例如: Function Calling 可視化 - Phase 2 實現]
- ❌ **高級功能 3**: [例如: 實時協作 - Phase 2 實現]

### 技術規格摘要

**完整規格參考**: `/docs/user-stories/modules/module-XX.md` (Line [start]-[end])

#### Backend API

**核心端點**:
```
GET    /api/v1/[resource]           # 列表 (支持分頁、排序、篩選)
GET    /api/v1/[resource]/{id}      # 單個
POST   /api/v1/[resource]           # 創建
PUT    /api/v1/[resource]/{id}      # 更新
DELETE /api/v1/[resource]/{id}      # 刪除
```

**Request 示例** (POST /api/v1/[resource]):
```json
{
  "field1": "value1",
  "field2": "value2"
}
```

**Response 示例**:
```json
{
  "id": "uuid",
  "field1": "value1",
  "createdAt": "2025-11-12T10:00:00Z"
}
```

**驗證規則**:
- `field1`: 必填, 長度 3-100
- `field2`: 必填, 枚舉值 [option1, option2]

**錯誤響應**:
- `400`: 請求驗證失敗
- `404`: 資源不存在
- `500`: 服務器錯誤

#### Database Schema

**參考**: `/docs/architecture/database-schema.md` (Line [start]-[end])

**Table: [TableName]**
```sql
CREATE TABLE [TableName] (
    Id UUID PRIMARY KEY,
    Field1 VARCHAR(100) NOT NULL,
    Field2 INTEGER NOT NULL,
    CreatedAt TIMESTAMP DEFAULT NOW(),
    UpdatedAt TIMESTAMP DEFAULT NOW()
);

-- Indexes (性能優化)
CREATE INDEX idx_[table]_[field] ON [TableName]([Field]);
```

**關聯關係**:
- `ForeignKey`: 關聯到 `[OtherTable].[Field]`
- Relationship: One-to-Many / Many-to-Many

#### Frontend UI (MVP 範圍)

**參考**: `/docs/ux-design/wireframes/low-fidelity/XX-[feature].md` (Line [start]-[end])

**Layout**: 2 欄布局 (MVP)
```
+-------------------+---------------------------+
| Sidebar (280px)   | Main Area (flex-grow)     |
| - Navigation      | - Header                  |
| - List Items      | - Content                 |
|                   | - Footer                  |
+-------------------+---------------------------+
```

**本 Sprint 實現的 UI 組件**:
1. **Sidebar**:
   - Component: `ConversationSidebar`
   - 功能: 列表顯示, 新增按鈕, 搜尋
2. **Main Area**:
   - Component: `ChatArea`
   - 功能: Header, MessageList, ChatInput

**本 Sprint 不實現** (Phase 2):
- ❌ 3 欄布局 (暫時使用 2 欄)
- ❌ Function Calling 可視化面板
- ❌ 拖拽排序功能

### 性能標準

**參考**: `/docs/user-stories/epics/epic-XX.md` (Performance Requirements)

- **響應時間**: P95 <500ms, P99 <1000ms
- **吞吐量**: ≥1000 requests/second
- **並發用戶**: ≥100 concurrent users
- **數據量**: 支持 ≥10,000 records

### 安全需求

**參考**: `/docs/architecture/ADR-XXX-security.md`

- **認證**: JWT Bearer Token
- **授權**: RBAC (Admin, Developer, Viewer)
- **數據驗證**: FluentValidation + 前端驗證
- **敏感數據**: 加密存儲 (AES-256)

---

## 🎯 US X.2: [下一個 User Story 標題]

[重複上述結構]

---

## 🏗️ 架構設計摘要

### 系統架構

**參考**: `/docs/architecture/ADR-XXX-system-architecture.md`

**項目結構** (Clean Architecture):
```
src/
├── AIAgentPlatform.Domain/          # 領域層 (實體、接口)
├── AIAgentPlatform.Application/     # 應用層 (用例、CQRS)
├── AIAgentPlatform.Infrastructure/  # 基礎設施層 (Repository、服務)
└── AIAgentPlatform.Api/             # API 層 (Controllers)

apps/
└── web-app/                         # React 前端應用
    └── src/
        ├── features/                # 功能模組
        ├── components/              # 共用組件
        └── services/                # API 服務
```

### 數據流

**參考**: `/docs/architecture/data-flow.md`

```
User Action → Frontend Component → API Service → HTTP Request
→ Backend Controller → MediatR → Command/Query Handler → Domain Logic
→ Repository → Database → Response → Frontend State → UI Update
```

### 技術棧

**參考**: `/docs/PROJECT-INITIALIZATION-DECISION.md`

**Backend**:
- .NET 8 Web API
- Entity Framework Core 8
- PostgreSQL 15
- MediatR (CQRS)
- FluentValidation

**Frontend**:
- React 18 + TypeScript
- Material-UI (MUI v5)
- React Router v6
- Axios

---

## 📏 編碼規範摘要

### Backend Coding Standards

**參考**: `/docs/technical-implementation/01-CODING-STANDARDS.md` (C# Section)

**命名規則**:
- Class: `PascalCase` (e.g., `AgentRepository`)
- Method: `PascalCase` (e.g., `GetAgentByIdAsync`)
- Variable: `camelCase` (e.g., `agentId`)
- Interface: `IPascalCase` (e.g., `IAgentRepository`)

**文件組織**:
- 每個文件一個 Class
- 相關的 Classes 放在同一個文件夾
- 使用 Namespace 組織代碼

**異步規範**:
```csharp
// ✅ 正確
public async Task<Agent> GetAgentByIdAsync(Guid id, CancellationToken cancellationToken)
{
    return await _dbContext.Agents
        .FirstOrDefaultAsync(a => a.Id == id, cancellationToken);
}

// ❌ 錯誤 - 缺少 Async 後綴和 CancellationToken
public async Task<Agent> GetAgentById(Guid id)
{
    return await _dbContext.Agents.FirstOrDefaultAsync(a => a.Id == id);
}
```

### Frontend Coding Standards

**參考**: `/docs/technical-implementation/01-CODING-STANDARDS.md` (TypeScript Section)

**命名規則**:
- Component: `PascalCase` (e.g., `ConversationSidebar`)
- Function: `camelCase` (e.g., `handleSendMessage`)
- Hook: `usePascalCase` (e.g., `useConversations`)
- Interface: `PascalCase` (e.g., `Conversation`)

**Component 結構**:
```typescript
// ✅ 正確的組件結構
export const MyComponent: React.FC<MyComponentProps> = ({ prop1, prop2 }) => {
  // 1. Hooks
  const [state, setState] = useState<StateType>(initialState);

  // 2. Effects
  useEffect(() => {
    // ...
  }, [dependencies]);

  // 3. Handlers
  const handleAction = () => {
    // ...
  };

  // 4. Render
  return (
    <div>
      {/* JSX */}
    </div>
  );
};
```

---

## 🧪 測試策略摘要

**參考**: `/docs/technical-implementation/07-TESTING-STRATEGY.md`

### Backend 測試

**單元測試** (`tests/AIAgentPlatform.UnitTests/`):
- 測試 Command/Query Handlers
- 測試 Domain Logic
- 測試 Validators
- Mock 外部依賴

**集成測試** (`tests/AIAgentPlatform.IntegrationTests/`):
- 測試 API Endpoints
- 使用 Test Database
- 測試完整請求-響應流程

### Frontend 測試

**組件測試** (`__tests__/`):
- 測試組件渲染
- 測試用戶交互
- Mock API 調用

**E2E 測試** (`tests/e2e/`):
- 測試完整用戶流程
- 使用 Playwright
- 測試跨頁面交互

---

## 🔗 關鍵文檔快速鏈接

### 必讀文檔 (本 Sprint 核心)
- 📋 [SPRINT-[X]-PLAN.md](./SPRINT-[X]-PLAN.md) - 詳細執行計劃
- ✅ [SPRINT-[X]-CHECKLIST.md](./SPRINT-[X]-CHECKLIST.md) - 實施檢查清單

### 參考文檔 (按需查閱)
- 📚 `/docs/user-stories/modules/module-XX.md` - 完整 Module 定義
- 🎨 `/docs/ux-design/wireframes/low-fidelity/XX.md` - 完整 UI 設計
- 🏗️ `/docs/architecture/ADR-XXX.md` - 架構決策
- 📖 `/docs/technical-implementation/01-CODING-STANDARDS.md` - 編碼規範
- 🧪 `/docs/technical-implementation/07-TESTING-STRATEGY.md` - 測試策略

---

## 💡 AI 助手使用指南

### 何時使用本文檔
- ✅ **開始開發前**: 快速了解本 Sprint 要做什麼
- ✅ **遇到不確定時**: 查閱 MVP 範圍,確認是否要實現某功能
- ✅ **需要技術規格**: 查看精簡的 API、Database、UI 規格
- ✅ **需要完整資訊**: 通過鏈接跳轉到 /docs 完整文檔

### 何時查閱 /docs 完整文檔
- ❌ 不要一開始就查閱 /docs (太多資訊會overwhelm)
- ✅ 當本文檔的精簡資訊不足時,通過鏈接查閱完整文檔
- ✅ 需要了解完整的 BDD 場景、邊界條件時
- ✅ 需要了解架構決策的完整背景和權衡時

### 使用流程
```
1. 閱讀 SPRINT-[X]-OVERVIEW.md → 了解 Sprint 目標
2. 閱讀 SPRINT-[X]-CONTEXT.md (本文檔) → 了解 MVP 範圍和技術規格
3. 閱讀 SPRINT-[X]-PLAN.md → 了解詳細執行計劃
4. 開始開發 → 使用 SPRINT-[X]-CHECKLIST.md 追蹤進度
5. 如有疑問 → 查閱 /docs 完整文檔 (通過本文檔提供的鏈接)
```

---

**創建日期**: YYYY-MM-DD
**最後更新**: YYYY-MM-DD
**版本**: v1.0
