# Sprint [X] 實施檢查清單

**目的**: 追蹤詳細實施進度,作為開發進度記錄

**完成度**: 0/[總數] (0%)
**最後更新**: YYYY-MM-DD

---

## 📊 進度總覽

| 類別 | 完成 / 總數 | 百分比 | 狀態 |
|------|------------|--------|------|
| 📋 準備工作 | 0/[X] | 0% | ⏳ |
| 🗄️ Database | 0/[X] | 0% | ⏳ |
| 🔧 Backend API | 0/[X] | 0% | ⏳ |
| 🎨 Frontend UI | 0/[X] | 0% | ⏳ |
| 🧪 測試 | 0/[X] | 0% | ⏳ |
| 📚 文檔 | 0/[X] | 0% | ⏳ |
| 🚀 部署 | 0/[X] | 0% | ⏳ |
| **總計** | **0/[總數]** | **0%** | ⏳ |

**圖例**: ✅ 完成 | 🔄 進行中 | ⏳ 待開始 | ❌ 被阻塞 | 🔶 可選

---

## 📋 準備工作

### 環境設置
- [ ] 拉取最新代碼 (`git pull origin main`)
- [ ] 創建 Feature Branch (`git checkout -b feature/sprint-[X]-[feature-name]`)
- [ ] 安裝依賴 (`dotnet restore`, `pnpm install`)
- [ ] 驗證開發環境正常運行
- [ ] 閱讀 SPRINT-[X]-PLAN.md 和 SPRINT-[X]-CONTEXT.md

### 需求確認
- [ ] 確認 US X.1 的 MVP 範圍
- [ ] 確認 US X.2 的 MVP 範圍
- [ ] 確認 Phase 2 排除的功能
- [ ] 確認 UI 設計稿 (僅 MVP 範圍)
- [ ] 確認 API 規格
- [ ] 確認性能標準

---

## 🗄️ Database 開發

### US X.1: [User Story 標題]

#### Schema 設計
- [ ] **設計 [Table] Schema**
  - 文件: `[path-to-design-doc]`
  - 確認日期: YYYY-MM-DD
  - 審查人: [名字]

#### Entity & Configuration
- [ ] **創建 Domain Entity**: `src/AIAgentPlatform.Domain/Entities/[Entity].cs`
  - Commit: [git-hash]
  - Properties: [列出關鍵屬性]
- [ ] **創建 EF Core Configuration**: `src/AIAgentPlatform.Infrastructure/Configurations/[Entity]Configuration.cs`
  - Commit: [git-hash]
  - Indexes: [列出 indexes]
  - Relationships: [列出關聯]

#### Migration
- [ ] **創建 Migration**
  - 命令: `dotnet ef migrations add Add[Entity]Table`
  - Migration 文件: `[migration-file-name].cs`
  - 確認 Up/Down 方法正確
- [ ] **執行 Migration (Local)**
  - 命令: `dotnet ef database update`
  - 驗證: 檢查數據庫 Schema
- [ ] **驗證 Migration 可回滾**
  - 命令: `dotnet ef database update [previous-migration]`
  - 確認 Down 方法正確執行

#### Repository
- [ ] **創建 Repository Interface**: `src/AIAgentPlatform.Domain/Interfaces/I[Entity]Repository.cs`
  - Commit: [git-hash]
  - Methods: [列出方法]
- [ ] **實現 Repository**: `src/AIAgentPlatform.Infrastructure/Repositories/[Entity]Repository.cs`
  - Commit: [git-hash]
  - 實現 CRUD 操作
  - 實現自定義查詢方法

#### Repository 單元測試
- [ ] **測試 GetByIdAsync**
  - 文件: `tests/AIAgentPlatform.UnitTests/Repositories/[Entity]RepositoryTests.cs`
  - Test: `GetByIdAsync_Should_ReturnEntity_When_EntityExists`
- [ ] **測試 GetAllAsync**
  - Test: `GetAllAsync_Should_ReturnAllEntities`
- [ ] **測試 AddAsync**
  - Test: `AddAsync_Should_AddEntity`
- [ ] **測試 UpdateAsync**
  - Test: `UpdateAsync_Should_UpdateEntity_When_EntityExists`
- [ ] **測試 DeleteAsync**
  - Test: `DeleteAsync_Should_DeleteEntity_When_EntityExists`

---

## 🔧 Backend API 開發

### US X.1: [User Story 標題]

#### DTOs (Data Transfer Objects)
- [ ] **創建 Request DTO**: `src/AIAgentPlatform.Application/[Feature]/DTOs/[Request]Dto.cs`
  - Commit: [git-hash]
  - Properties: [列出屬性]
  - Validation Attributes: [列出驗證]
- [ ] **創建 Response DTO**: `src/AIAgentPlatform.Application/[Feature]/DTOs/[Response]Dto.cs`
  - Commit: [git-hash]
  - Properties: [列出屬性]

#### Commands (CQRS - Write Operations)
- [ ] **創建 [Command]**: `src/AIAgentPlatform.Application/[Feature]/Commands/[Command]/[Command].cs`
  - Commit: [git-hash]
  - Properties: [列出屬性]
- [ ] **創建 [Command]Handler**: `src/AIAgentPlatform.Application/[Feature]/Commands/[Command]/[Command]Handler.cs`
  - Commit: [git-hash]
  - 實現業務邏輯
  - 錯誤處理
- [ ] **創建 [Command]Validator**: `src/AIAgentPlatform.Application/[Feature]/Commands/[Command]/[Command]Validator.cs`
  - Commit: [git-hash]
  - FluentValidation 規則

#### Queries (CQRS - Read Operations)
- [ ] **創建 [Query]**: `src/AIAgentPlatform.Application/[Feature]/Queries/[Query]/[Query].cs`
  - Commit: [git-hash]
- [ ] **創建 [Query]Handler**: `src/AIAgentPlatform.Application/[Feature]/Queries/[Query]/[Query]Handler.cs`
  - Commit: [git-hash]
  - 實現查詢邏輯

#### Controllers (API Endpoints)
- [ ] **創建 [Controller]**: `src/AIAgentPlatform.Api/Controllers/[Controller].cs`
  - Commit: [git-hash]
- [ ] **實現 GET /api/[resource]** (列表)
  - Endpoint: `GET /api/v1/[resource]`
  - 支持分頁: `?page=[X]&pageSize=[Y]`
  - 支持排序: `?sortBy=[field]&sortOrder=[asc|desc]`
  - 支持篩選: `?filter=[field]:[value]`
- [ ] **實現 GET /api/[resource]/{id}** (單個)
  - Endpoint: `GET /api/v1/[resource]/{id}`
  - 404 處理
- [ ] **實現 POST /api/[resource]** (創建)
  - Endpoint: `POST /api/v1/[resource]`
  - 請求驗證
  - 201 Created 響應
- [ ] **實現 PUT /api/[resource]/{id}** (更新)
  - Endpoint: `PUT /api/v1/[resource]/{id}`
  - 404 處理
  - 200 OK 響應
- [ ] **實現 DELETE /api/[resource]/{id}** (刪除)
  - Endpoint: `DELETE /api/v1/[resource]/{id}`
  - 404 處理
  - 204 No Content 響應

#### API 單元測試
- [ ] **測試 Command Handler**
  - 文件: `tests/AIAgentPlatform.UnitTests/Application/[Feature]/Commands/[Command]HandlerTests.cs`
  - Test: `Handle_Should_Create[Entity]_When_Valid`
  - Test: `Handle_Should_ThrowException_When_Invalid`
- [ ] **測試 Query Handler**
  - 文件: `tests/AIAgentPlatform.UnitTests/Application/[Feature]/Queries/[Query]HandlerTests.cs`
  - Test: `Handle_Should_Return[Entity]_When_Exists`
- [ ] **測試 Validator**
  - 文件: `tests/AIAgentPlatform.UnitTests/Application/[Feature]/Commands/[Command]ValidatorTests.cs`
  - Test: `Validate_Should_HaveError_When_[Field]IsInvalid`

#### API 集成測試
- [ ] **測試 GET /api/[resource]**
  - 文件: `tests/AIAgentPlatform.IntegrationTests/Controllers/[Controller]Tests.cs`
  - Test: `Get[Resource]_Should_Return200_WithList`
- [ ] **測試 GET /api/[resource]/{id}**
  - Test: `Get[Resource]ById_Should_Return200_When_Exists`
  - Test: `Get[Resource]ById_Should_Return404_When_NotExists`
- [ ] **測試 POST /api/[resource]**
  - Test: `Create[Resource]_Should_Return201_When_Valid`
  - Test: `Create[Resource]_Should_Return400_When_Invalid`
- [ ] **測試 PUT /api/[resource]/{id}**
  - Test: `Update[Resource]_Should_Return200_When_Valid`
- [ ] **測試 DELETE /api/[resource]/{id}**
  - Test: `Delete[Resource]_Should_Return204_When_Exists`

#### API 文檔
- [ ] **更新 Swagger/OpenAPI 註釋**
  - 所有端點有 `<summary>` 註釋
  - 所有參數有 `<param>` 註釋
  - 所有響應有 `<response>` 註釋
- [ ] **驗證 Swagger UI**
  - 訪問: `http://localhost:5000/swagger`
  - 確認所有端點正確顯示
  - 測試「Try it out」功能

---

## 🎨 Frontend UI 開發

### US X.1: [User Story 標題]

#### Types & Interfaces
- [ ] **創建 Types**: `apps/web-app/src/features/[feature]/types/index.ts`
  - Commit: [git-hash]
  - Interfaces: [列出 interfaces]

#### API Service
- [ ] **創建 API Service**: `apps/web-app/src/features/[feature]/services/[service].ts`
  - Commit: [git-hash]
  - 方法:
    - `get[Resource]s()`: 獲取列表
    - `get[Resource]ById(id)`: 獲取單個
    - `create[Resource](data)`: 創建
    - `update[Resource](id, data)`: 更新
    - `delete[Resource](id)`: 刪除
  - 錯誤處理
  - Loading 狀態管理

#### Custom Hooks
- [ ] **創建 use[Feature] Hook**: `apps/web-app/src/features/[feature]/hooks/use[Feature].ts`
  - Commit: [git-hash]
  - State: `data`, `isLoading`, `error`
  - Methods: CRUD 操作
  - useEffect 數據加載

#### UI 組件

##### 1. [Component A]
- [ ] **創建組件**: `apps/web-app/src/features/[feature]/components/[ComponentA].tsx`
  - Commit: [git-hash]
  - Props interface 定義
  - 基本結構實現
- [ ] **實現樣式**
  - Material-UI 組件使用
  - 響應式設計 (Breakpoints)
  - Theme 變量使用
- [ ] **實現交互邏輯**
  - 事件處理器
  - 狀態管理
  - 副作用處理
- [ ] **錯誤處理**
  - Error Boundary
  - 錯誤提示 UI
- [ ] **Loading 狀態**
  - Skeleton 或 Spinner
  - 禁用狀態

##### 2. [Component B]
- [ ] **創建組件**: `apps/web-app/src/features/[feature]/components/[ComponentB].tsx`
- [ ] **實現樣式**
- [ ] **實現交互邏輯**
- [ ] **錯誤處理**
- [ ] **Loading 狀態**

#### 主頁面組件
- [ ] **創建頁面**: `apps/web-app/src/features/[feature]/[FeaturePage].tsx`
  - Commit: [git-hash]
  - 組合子組件
  - 狀態提升
  - 路由集成

#### 組件測試
- [ ] **測試 [ComponentA]**
  - 文件: `apps/web-app/src/features/[feature]/__tests__/[ComponentA].test.tsx`
  - Test: `renders correctly`
  - Test: `handles user interaction`
  - Test: `displays error state`
- [ ] **測試 [ComponentB]**
  - 類似結構

---

## 🧪 測試

### Backend 測試覆蓋率
- [ ] **運行單元測試**
  - 命令: `dotnet test --collect:"XPlat Code Coverage"`
  - 目標: ≥80% 覆蓋率
  - 當前: [X]%
- [ ] **運行集成測試**
  - 命令: `dotnet test --filter Category=Integration`
  - 所有測試通過

### Frontend 測試覆蓋率
- [ ] **運行單元測試**
  - 命令: `pnpm test --coverage`
  - 目標: ≥80% 覆蓋率
  - 當前: [X]%

### E2E 測試
- [ ] **創建 E2E 測試**: `tests/e2e/[feature].spec.ts`
  - Commit: [git-hash]
  - 測試完整用戶流程
- [ ] **運行 E2E 測試**
  - 命令: `pnpm test:e2e`
  - 所有測試通過

### 性能測試
- [ ] **Backend 性能測試**
  - 工具: Apache Bench 或 K6
  - 測試 P95 <500ms
  - 測試 100 concurrent users
  - 結果: [記錄結果]
- [ ] **Frontend 性能測試**
  - 工具: Lighthouse
  - Performance Score: ≥90
  - 結果: [記錄結果]

### 手動測試
- [ ] **功能測試**
  - 測試所有 CRUD 操作
  - 測試邊界條件
  - 測試錯誤處理
- [ ] **UI/UX 測試**
  - 對比設計稿
  - 測試響應式佈局
  - 測試交互反饋

---

## 📚 文檔

### API 文檔
- [ ] **Swagger/OpenAPI 完整**
  - 所有端點有文檔
  - 所有參數有說明
  - 所有響應有示例
- [ ] **API 使用指南** (如需要)
  - 文件: `docs/api/[feature]-api-guide.md`

### 代碼文檔
- [ ] **代碼註釋完整**
  - 複雜邏輯有註釋
  - Public 方法有 XML 註釋 (C#)
  - Public 函數有 JSDoc (TypeScript)
- [ ] **README 更新** (如有需要)
  - 新增功能說明
  - 配置說明
  - 使用示例

### Sprint 文檔
- [ ] **更新 SPRINT-[X]-DEV-LOG.md**
  - 記錄每日進度
  - 記錄遇到的問題和解決方案
- [ ] **更新 SPRINT-[X]-ISSUES.md**
  - 記錄所有問題
  - 更新問題狀態

---

## 🚀 部署

### Pre-deployment 檢查
- [ ] **所有測試通過**
  - 單元測試 ✅
  - 集成測試 ✅
  - E2E 測試 ✅
- [ ] **Code Review 完成**
  - PR 創建: [PR-link]
  - 審查人: [名字]
  - 審查通過日期: YYYY-MM-DD
- [ ] **無 Critical/High Bugs**
  - 檢查 Issue Tracker
  - 所有 Blocker 已解決

### Database Migration (如適用)
- [ ] **準備 Migration Script**
  - 文件: `[migration-file].sql`
  - 已在 Staging 環境測試
- [ ] **備份數據庫**
  - 備份時間: YYYY-MM-DD HH:MM
  - 備份位置: [path]
- [ ] **執行 Migration**
  - 執行時間: YYYY-MM-DD HH:MM
  - 執行人: [名字]
  - 驗證: Schema 正確

### 部署到 Staging
- [ ] **部署 Backend**
  - 命令: `[deployment-command]`
  - URL: `https://staging-api.[domain].com`
  - 健康檢查: `/health` 返回 200
- [ ] **部署 Frontend**
  - 命令: `[deployment-command]`
  - URL: `https://staging.[domain].com`
  - 驗證: 頁面正常加載
- [ ] **Staging 環境測試**
  - 冒煙測試通過
  - 關鍵流程測試通過

### 部署到 Production (如適用)
- [ ] **部署 Backend**
  - 部署時間: YYYY-MM-DD HH:MM
  - URL: `https://api.[domain].com`
  - 健康檢查通過
- [ ] **部署 Frontend**
  - 部署時間: YYYY-MM-DD HH:MM
  - URL: `https://[domain].com`
  - 驗證通過
- [ ] **Production 驗證**
  - 關鍵功能測試
  - 監控指標正常

---

## ✅ Definition of Done 總檢查

### 功能完成度
- [ ] 所有 P0 User Stories 完成
- [ ] 所有驗收標準通過
- [ ] 功能可演示

### 技術質量
- [ ] 單元測試覆蓋率 ≥80%
- [ ] 所有測試通過
- [ ] Code Review 完成
- [ ] 無 Critical/High severity bugs

### 性能標準
- [ ] P95 響應時間 <500ms
- [ ] 並發用戶支持 ≥100
- [ ] 錯誤率 <1%

### 文檔完成
- [ ] API 文檔完整
- [ ] 代碼註釋清晰
- [ ] Sprint 文檔更新

### 部署就緒
- [ ] Staging 環境測試通過
- [ ] Production 部署 (如適用)

---

**檢查清單完成日期**: YYYY-MM-DD
**最終驗收**: [名字] - YYYY-MM-DD
