# CLAUDE.md

此文件為 Claude Code (claude.ai/code) 在此代碼庫中工作時提供指導。

---

## 專案概述

**Semantic Kernel Agentic Framework** 是基於 Microsoft Semantic Kernel 的企業級 Multi-Agent 協作框架，專為彌補 Microsoft Copilot Studio 的核心能力缺失而設計。

### 核心差異化能力
1. **Persona Framework** - 結構化個性配置系統（>85% 一致性）
2. **Code Interpreter** - 4 層安全沙箱執行 Python/R 代碼
3. **Text-to-SQL** - 智能結構化數據查詢（>85% 準確率）
4. **Knowledge Management** - 混合檢索 + Reranking（90%+ 檢索準確率）
5. **Multi-Agent Workflow** - 可視化工作流編輯器（拖拽式編排）

### 技術棧

**後端**:
- .NET 9 (C# 12) + ASP.NET Core 9
- Entity Framework Core 9.0 + PostgreSQL 16
- Semantic Kernel 1.66.0+
- MediatR 12.4 (CQRS)
- FluentValidation 11.11
- Redis 7 (緩存)
- Qdrant 1.7.4 (向量數據庫)

**前端**:
- 主應用: React 18 + TypeScript 5 + Material-UI + Redux Toolkit + Vite 5
- 工作流編輯器: Vue 3 + VueFlow 1.45.0 + Pinia + Element Plus (微前端)
- 整合: Module Federation + Yjs CRDT

**LLM 整合**:
- Azure OpenAI (主要) / OpenAI / Anthropic Claude (備選)
- text-embedding-ada-002 (Embedding)

**DevOps**:
- Docker + Kubernetes (AKS)
- GitHub Actions (CI/CD)
- Terraform (IaC)
- Prometheus + Grafana (監控)

---

## 開發方法論

本專案採用 **BMad Method** - AI 輔助軟體開發方法論：
- 10 個專業角色 (Analyst, Architect, PM, PO, Dev, QA, SM, UX...)
- 完整文檔模板和工作流
- 詳見 `.bmad-core/` 和文檔結構

---

## 文檔架構

所有專案文檔位於 `docs/` 目錄：

```
docs/
├── README.md                           # 主索引 - 完整導航和進度追蹤
├── brief.md                            # Project Brief (商業需求)
├── project-management/
│   └── Project-Management-Plan.md     # 項目管理計劃
├── architecture/
│   ├── Architecture-Design-Document.md  # 系統架構設計
│   ├── ADR-006-agent-state-management.md  # Agent 狀態管理決策
│   ├── ADR-007-multi-agent-communication.md  # Multi-Agent 通訊決策
│   ├── ADR-008-code-interpreter-execution-model.md  # Code Interpreter 執行決策
│   ├── ADR-011-framework-migration-strategy.md  # 框架遷移策略
│   ├── ADR-012-workflow-editor-technology.md  # 工作流編輯器技術選型
│   ├── C4-architecture-diagrams.md     # C4 架構圖
│   ├── database-schema.md              # PostgreSQL 數據庫設計
│   └── performance-scalability-strategy.md  # 性能與擴展性
├── user-stories/
│   ├── README.md                       # User Stories 總覽 (43 個)
│   ├── implementation-strategy.md      # 技術難點與解決方案
│   ├── mvp-planning.md                 # MVP 範圍與時程
│   ├── modules/                        # 10 個功能模塊 (module-01 到 module-10)
│   ├── sprints/                        # 18 個 Sprint 規劃
│   └── templates/                      # User Story 模板和 DoD
├── ux-design/                          # UI/UX 設計文檔
│   ├── user-research/                  # 用戶研究和 Persona
│   ├── information-architecture/        # 資訊架構和導航
│   ├── wireframes/low-fidelity/        # 12 個核心頁面線框圖
│   └── design-system/                  # Design Tokens、組件庫、無障礙指南
└── technical-implementation/
    ├── 1-poc-validation/               # 6 個 PoC 驗證計劃
    └── [其他技術文檔]
```

### 關鍵文檔快速參考

**必讀文檔** (開始工作前):
1. `docs/README.md` - 完整文檔導航和專案進度
2. `docs/architecture/Architecture-Design-Document.md` - 系統架構和設計決策
3. `docs/user-stories/README.md` - 43 個 User Stories 總覽
4. `docs/user-stories/implementation-strategy.md` - 4 大核心能力技術難點

**架構決策記錄 (ADRs)**:
- ADR-006: Agent 狀態管理（Hybrid: Redis + PostgreSQL）
- ADR-007: Multi-Agent 通訊（Event-Driven + Direct Invocation）
- ADR-008: Code Interpreter 執行模型（4 層安全防護）
- ADR-011: 框架遷移策略（Docker Compose → Kubernetes）
- ADR-012: 工作流編輯器技術選型（VueFlow + Module Federation）

---

## 架構模式

### Clean Architecture + DDD
- **Core Layer**: Domain 實體、值對象、聚合根
- **Application Layer**: Use Cases、CQRS (MediatR)、FluentValidation
- **Infrastructure Layer**: EF Core Repositories、External Services
- **Presentation Layer**: ASP.NET Core Web API

### 關鍵設計模式
- **CQRS**: Commands (寫) 和 Queries (讀) 分離 (MediatR)
- **Repository Pattern**: 數據訪問抽象層
- **Unit of Work**: 事務邊界管理
- **Domain Events**: 領域事件驅動
- **Specification Pattern**: 查詢條件組合

### 狀態管理策略 (ADR-006)
```yaml
短期執行 Agent (<5分鐘):
  存儲: Redis (內存緩存，TTL 30 分鐘)
  持久化: 僅在執行完成時寫入 PostgreSQL

長期執行 Agent (>5分鐘):
  存儲: Redis (狀態緩存) + PostgreSQL (checkpoint)
  checkpoint 頻率: 每 30 秒
  恢復策略: 從最近 checkpoint 重啟

對話歷史:
  存儲: PostgreSQL (主要) + Redis (最近 100 條緩存)
  清理策略: 超過 1000 條觸發自動摘要
```

---

## 開發環境設置

### 先決條件
```bash
# Windows
winget install Microsoft.DotNet.SDK.9
winget install OpenJS.NodeJS.LTS
winget install Docker.DockerDesktop

# macOS
brew install dotnet@9
brew install node@20
brew install --cask docker
```

### 快速啟動

**1. 啟動基礎設施服務**:
```bash
# 啟動 PostgreSQL, Redis, Qdrant
docker-compose up -d

# 檢查服務狀態
.\scripts\health-check.ps1  # Windows
# 或
./scripts/health-check.sh   # macOS/Linux
```

**2. 後端開發**:
```bash
# 數據庫遷移
cd src/AIAgentPlatform.API
dotnet ef database update

# 啟動 API
dotnet run
# API: http://localhost:5095
# Swagger: http://localhost:5095/swagger
```

**3. 前端開發**:
```bash
# React 主應用
cd packages/host
npm install
npm run dev  # Port 3000

# Vue 工作流編輯器 (遠程模塊)
cd packages/remote
npm install
npm run dev  # Port 3001

# 或並行啟動
npm run dev  # 同時啟動 Host 和 Remote
```

---

## 常用開發命令

### .NET Backend

**建置與測試**:
```bash
# 建置解決方案
dotnet build

# 執行所有測試
dotnet test

# 執行特定測試專案
dotnet test tests/AIAgentPlatform.UnitTests

# 測試覆蓋率報告
dotnet test /p:CollectCoverage=true /p:CoverageReportsFormat=lcov

# 代碼格式化
dotnet format
```

**數據庫遷移**:
```bash
# 創建新遷移
dotnet ef migrations add <MigrationName> --project src/AIAgentPlatform.Infrastructure

# 應用遷移
dotnet ef database update --project src/AIAgentPlatform.API

# 回滾遷移
dotnet ef database update <PreviousMigrationName>

# 生成 SQL 腳本
dotnet ef migrations script --output migration.sql
```

**開發伺服器**:
```bash
# Development mode (hot reload)
dotnet watch run --project src/AIAgentPlatform.API

# Production mode
dotnet run --project src/AIAgentPlatform.API --configuration Release
```

### Frontend

**React 主應用** (packages/host):
```bash
# 開發
npm run dev

# 建置
npm run build

# Lint
npm run lint

# 類型檢查
npm run type-check

# 測試
npm run test
npm run test:watch
npm run test:coverage
```

**Vue 工作流編輯器** (packages/remote):
```bash
# 開發
npm run dev

# 建置
npm run build

# Lint
npm run lint

# 類型檢查
npm run type-check
```

---

## 測試策略

### 測試層級
1. **單元測試**: Domain、Application、Infrastructure 層
2. **整合測試**: API 端點、數據庫操作、External Services
3. **E2E 測試**: 關鍵用戶流程（Playwright）

### 測試標準
- 單元測試覆蓋率: ≥80%
- 關鍵路徑測試覆蓋率: 100%
- CI/CD 自動運行所有測試

### 執行測試
```bash
# 後端單元測試
dotnet test tests/AIAgentPlatform.UnitTests

# 後端整合測試
dotnet test tests/AIAgentPlatform.IntegrationTests

# 前端測試
cd packages/host
npm run test

# E2E 測試
npm run test:e2e
```

---

## 編碼標準

### C# 後端
- **命名規範**: PascalCase (類、方法)、camelCase (私有欄位、參數)
- **非同步方法**: 後綴 `Async`，返回 `Task` 或 `Task<T>`
- **Null 處理**: 使用 Nullable Reference Types (C# 8+)
- **LINQ**: 優先使用 LINQ 而非迴圈
- **依賴注入**: 建構子注入，避免 Service Locator

### TypeScript 前端
- **命名規範**: camelCase (變數、函數)、PascalCase (組件、類型)
- **類型定義**: 明確定義類型，避免 `any`
- **React Hooks**: 優先使用 Functional Components + Hooks
- **狀態管理**: Redux Toolkit (React) / Pinia (Vue)

### Git 工作流
- **分支策略**: feature/* (功能)、bugfix/* (修復)、hotfix/* (緊急修復)
- **Commit 訊息**: 遵循 Conventional Commits
  - `feat:` 新功能
  - `fix:` 錯誤修復
  - `docs:` 文檔變更
  - `refactor:` 重構
  - `test:` 測試相關
  - `chore:` 建置/工具變更

---

## 性能目標 (SLA)

```yaml
API 響應時間:
  P50: <100ms
  P95: <300ms
  P99: <500ms

Agent 執行時間:
  簡單查詢: P95 <2s
  一般執行: P95 <5s
  Code Interpreter: P95 <8s

系統可用性:
  MVP: 99.5%
  Phase 2: 99.9%

並發能力:
  MVP: 20 並發 Agent
  Phase 2: 200 並發 Agent
```

---

## 安全性要求

### Code Interpreter 4 層安全防護 (ADR-008)
1. **網絡隔離**: 無外網訪問 (Docker `--network none`)
2. **文件系統隔離**: 唯讀 `/code`、可寫 `/workspace`
3. **資源限制**: CPU (0.5 核)、Memory (512MB)、Time (30s)
4. **代碼審查**: AST 靜態分析 + LLM 動態檢查

### Text-to-SQL 安全防護
- **SQL Injection**: 參數化查詢、語法樹驗證
- **權限控制**: 唯讀連接、Schema 白名單
- **查詢限制**: LIMIT 強制、超時控制

### 認證授權
- **認證**: OAuth 2.0 (Microsoft Entra ID)
- **授權**: RBAC (Role-Based Access Control)
- **API**: JWT Token，過期時間 1 小時

---

## 專案狀態與里程碑

### 當前狀態 (2025-11-04)
```
✅ Phase 1 - Business (Analyst): Project Brief
✅ Phase 2 - Management (PM): Project Management Plan
✅ Phase 3.1 - Architecture: ADD + 5 ADRs
✅ Phase 3.2 - Requirements: 43 User Stories + Sprint Planning
✅ Phase 3.3 - UI/UX Designer: 完成
✅ Phase 3.4 - Tech Lead: 完成
✅ Sprint 0 - 環境建置: 完成
🚀 Sprint 1 - 開發中 (33% 完成)
   ├── ✅ US 1.1: Agent CRUD API (100%)
   ├── ⏳ US 1.2: Conversation CRUD (0%)
   └── ⏳ US 1.3: Agent Execution (0%)
⏸️ Phase 3.5 - Integration (待開始)
⏸️ Phase 4 - Development: 18 Sprints × 3 weeks
```

### Sprint 規劃
- **總 Sprints**: 18 個 (MVP)
- **Sprint 長度**: 3 週
- **總時程**: 54 週 (約 10-12 個月)
- **當前 Sprint**: Sprint 1 (Week 1-3)

### 已完成功能
**✅ Agent CRUD API** (US 1.1):
- `POST /api/agents` - 創建 Agent
- `GET /api/agents` - 取得 Agent 列表 (分頁)
- `GET /api/agents/{id}` - 取得單一 Agent
- `PUT /api/agents/{id}` - 更新 Agent
- `DELETE /api/agents/{id}` - 刪除 Agent (軟刪除)
- 完成時間: 19.5h (預估 24h, 快 18.75%)
- 測試: 43 個單元測試 (100% 通過)

---

## PoC 驗證計劃

在正式開發前需完成 6 個關鍵技術 PoC 驗證 (詳見 `docs/technical-implementation/1-poc-validation/`):

1. **PoC 1**: Semantic Kernel Agents (🔴 P0)
2. **PoC 2**: Persona Builder (🔴 P0)
3. **PoC 3**: Code Interpreter Sandbox (🔴 P0)
4. **PoC 4**: Text-to-SQL Engine (🔴 P0)
5. **PoC 5**: Knowledge RAG 90%+ Accuracy (🟡 P1)
6. **PoC 6**: VueFlow + CRDT Collaboration (🟡 P1)

**成功標準**: P0 PoC 必須 100% 通過，P1 至少 50% 通過

---

## 故障排除

### Docker 服務無法啟動
```bash
# 檢查 Docker 狀態
docker ps
docker-compose ps

# 查看服務日誌
docker-compose logs postgresql
docker-compose logs redis
docker-compose logs qdrant

# 重啟服務
docker-compose down
docker-compose up -d
```

### 數據庫連接失敗
```bash
# 檢查連接字串 (appsettings.json)
# 確認 PostgreSQL 正在運行
docker-compose ps postgresql

# 測試連接
psql -h localhost -p 5432 -U postgres -d aiagent
```

### 前端無法連接 API
- 檢查 CORS 設定 (Program.cs)
- 確認 API 正在運行: `http://localhost:5095`
- 檢查環境變數 (`.env` 文件)

---

## 貢獻指南

1. **閱讀文檔**: 從 `docs/README.md` 開始
2. **理解 User Stories**: 查看 `docs/user-stories/README.md`
3. **遵循架構**: 參考 `docs/architecture/Architecture-Design-Document.md`
4. **編碼標準**: 遵循上述編碼標準
5. **測試要求**: 單元測試覆蓋率 ≥80%
6. **提交 PR**: 確保通過所有測試和代碼審查

---

## 相關連結

- **文檔主索引**: `docs/README.md`
- **架構設計**: `docs/architecture/Architecture-Design-Document.md`
- **User Stories**: `docs/user-stories/README.md`
- **BMad Method**: `.bmad-core/user-guide.md`
- **GitHub Issues**: [提交問題或建議](https://github.com/laitim2001/ai-semantic-kernel-framework/issues)

---

**最後更新**: 2025-11-14
**專案版本**: 1.0.0-dev
**狀態**: Sprint 1 開發中
