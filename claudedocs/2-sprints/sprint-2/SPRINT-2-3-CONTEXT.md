# Sprint 2 上下文參考文檔

| 欄位 | 內容 |
|------|------|
| **版本** | v2.1 |
| **Sprint 編號** | Sprint 2 |
| **Sprint 週期** | Week 4-6 (2025-11-25 ~ 2025-12-21, 27 days) |
| **Phase** | Sprint 2 - Agent 執行、Plugin 系統、User Story 6.1 管理後台 |
| **計劃日期** | 原定 21 days |
| **實際日期** | 27 days (延遲 6 days) |
| **創建日期** | 2025-12-10 |
| **最後更新** | 2025-12-11 (Day 17/27, 86% 完成) |

---

## 📋 目錄

### 核心章節
1. [關鍵文檔索引（帶行號）](#關鍵文檔索引帶行號)
   - 1.1 [Sprint 2 執行文檔](#sprint-2-執行文檔)
   - 1.2 [項目規劃文檔（/docs 參考）](#項目規劃文檔docs-參考)
   - 1.3 [架構設計文檔](#架構設計文檔)

2. [MVP 範圍快速參考](#mvp-範圍快速參考)
   - 2.1 [US 1.4: Agent 執行與監控](#us-14-agent-執行與監控-13-sp-4-phases--完成)
   - 2.2 [US 2.1: Plugin 註冊系統](#us-21-plugin-註冊系統-5-sp-5-phases--完成)
   - 2.3 [US 2.2: Plugin 熱重載](#us-22-plugin-熱重載-部分完成-40-)
   - 2.4 [US 2.3: Plugin 版本管理](#us-23-plugin-版本管理-部分完成-30-)
   - 2.5 [US 6.1: 基礎聊天介面](#us-61-基礎聊天介面-3-sp--待開始)

3. [API 規格速查表](#api-規格速查表)
   - 3.1 [Agent Execution API (US 1.4) - 11 個端點](#agent-execution-api-us-14---11-個端點)
   - 3.2 [Plugin Versions API (US 2.1) - 5 個端點](#plugin-versions-api-us-21---5-個端點)

4. [技術架構快速參考](#技術架構快速參考)
   - 4.1 [Backend 技術棧](#backend-技術棧)
   - 4.2 [Frontend 技術棧](#frontend-技術棧)
   - 4.3 [資料庫 Schema 設計](#資料庫-schema-設計)

5. [編碼標準快速參考](#編碼標準快速參考)
   - 5.1 [C# 命名規範](#c-命名規範)
   - 5.2 [TypeScript/React 命名規範](#typescriptreact-命名規範)
   - 5.3 [Git Commit Message 格式](#git-commit-message-格式)

6. [開發優先順序（剩餘工作）](#開發優先順序剩餘工作)

7. [相關文檔連結](#相關文檔連結)

8. [完整參考文獻索引](#完整參考文獻索引)

### 輔助章節
- [使用指南](#使用指南)
- [參考文獻索引](#參考文獻索引-1)
- [版本歷史](#版本歷史)

---

## 📖 使用指南

### 文件目的
本文件為 AI Assistant 提供 Sprint 2 開發所需的快速參考與上下文定位，包含：
- 關鍵文檔的精確行號，避免全文搜索
- MVP 範圍快速參考
- API 規格速查表
- 技術架構提醒
- 編碼標準參考

### 目標讀者
- **AI Assistant (Claude Code)**：開發過程中的主要參考文件，快速定位技術細節
- **開發團隊**：技術實施的快速參考手冊
- **新加入成員**：理解 Sprint 2 技術上下文的入門文檔

### 使用方式
1. **開始開發前**：閱讀對應 User Story 的 MVP 範圍快速參考
2. **實作 API 時**：查閱 API 規格速查表，獲取完整請求/響應格式
3. **編碼過程中**：參考編碼標準快速參考確保符合專案規範
4. **查找文檔時**：使用關鍵文檔索引（帶行號）直接定位到具體章節
5. **需要架構理解**：查閱技術架構快速參考章節

### 快速導航
- **查看 Sprint 2 整體概覽** → [SPRINT-2-1-OVERVIEW.md](./SPRINT-2-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-2-2-PLAN.md](./SPRINT-2-2-PLAN.md)
- **追蹤任務進度** → [SPRINT-2-4-CHECKLIST.md](./SPRINT-2-4-CHECKLIST.md)
- **查看開發日誌** → [SPRINT-2-5-DEV-LOG.md](./SPRINT-2-5-DEV-LOG.md)

---

## 📖 關鍵文檔索引（帶行號）

### Sprint 2 執行文檔

| 文檔 | 路徑 | 關鍵內容 | 行號範圍 |
|------|------|---------|---------|
| **Sprint 2 概覽** | `claudedocs/2-sprints/sprint-2/SPRINT-2-1-OVERVIEW.md` | Sprint 目標、User Stories 狀態、關鍵指標 | 1-762 |
| **Sprint 2 執行計劃** | `claudedocs/2-sprints/sprint-2/SPRINT-2-2-PLAN.md` | 技術實施細節、代碼範例、API 規格 | 1-1100+ |
| **變更記錄** | `claudedocs/4-changes/CHANGE-LOG.md` | CHANGE-001, CHANGE-002 詳細記錄 | 1-661 |

### 項目規劃文檔（/docs 參考）

| 文檔 | 路徑 | 關鍵內容 | 快速定位 |
|------|------|---------|---------|
| **US 1.4 規格** | `docs/user-stories/modules/module-01-agent-creation.md` (line 156+) | Agent 執行引擎、歷史追蹤、效能指標、即時監控 | Epic 1: Core Agent Management |
| **US 2.1-2.3 規格** | `docs/user-stories/modules/module-02-plugin-system.md` | Plugin 註冊、版本管理、動態載入、熱重載 | Epic 2: Plugin System (US 2.1 line 22+, US 2.2 line 171+, US 2.3 line 280+) |
| **US 6.1 規格** | `docs/user-stories/modules/module-06-chat-interface.md` (line 22+) | 基礎聊天介面、對話管理 | Epic 6: Frontend UI |

### 架構設計文檔

| 文檔 | 路徑 | 關鍵內容 | 關鍵章節 |
|------|------|---------|---------|
| **架構設計總覽** | `docs/architecture/Architecture-Design-Document.md` | Clean Architecture 分層、系統架構、技術棧、性能目標 | 執行摘要、系統架構、ADR 索引 |
| **數據庫設計** | `docs/architecture/database-schema.md` | PostgreSQL Schema、Entity 定義、索引策略 | agent_executions, plugin_versions, conversations |
| **C4 架構圖** | `docs/architecture/C4-architecture-diagrams.md` | 系統架構視圖、容器圖、組件圖 | Context, Container, Component diagrams |

---

## 🎯 MVP 範圍快速參考

### US 1.4: Agent 執行與監控 (13 SP, 4 Phases) ✅ 完成

**Phase 1: 基礎執行引擎**
- ✅ Semantic Kernel 整合
- ✅ `ExecuteAgentCommand` + `ExecuteAgentCommandHandler`
- ✅ `AgentExecution` Entity (Domain Layer)
- ✅ `IAgentExecutionRepository` + 實作
- ✅ API: `POST /api/v1/agents/{id}/execute`

**Phase 2: 執行歷史追蹤**
- ✅ `GetAgentExecutionHistoryQuery` (9 個查詢參數)
- ✅ 進階過濾: AgentId, ConversationId, Status, DateRange, Pagination, Sorting
- ✅ API: `GET /api/v1/agents/{id}/executions`
- ✅ API: `GET /api/v1/executions/{id}`

**Phase 3: 效能指標**
- ✅ `GetAgentStatisticsQuery` (統計分析)
- ✅ 指標: Total Executions, Avg/Min/Max Response Time, P95/P99, Token Usage
- ✅ API: `GET /api/v1/agents/{id}/statistics`

**Phase 4: 即時監控 & 匯出**
- ✅ SignalR Hub (`ExecutionMonitorHub`)
- ✅ WebSocket 訂閱機制 (Agent-level, Conversation-level, All executions)
- ✅ CSV/JSON 匯出功能
- ✅ API: WebSocket `/hubs/execution-monitor`
- ✅ API: `GET /api/v1/agents/{id}/executions/export?format=csv|json`

**關鍵技術**:
- Semantic Kernel: Prompt execution
- SignalR: WebSocket 即時推送
- Entity Framework Core: 執行歷史持久化
- LINQ: 統計計算 (Percentile, Aggregation)

---

### US 2.1: Plugin 註冊系統 (5 SP, 5 Phases) ✅ 完成

**Phase 1: Domain Layer**
- ✅ `PluginVersion` Entity (plugin_id, version, metadata, status)
- ✅ `VersionNumber` Value Object (SemVer: Major.Minor.Patch)
- ✅ `PluginMetadata` Value Object (JSONB: AssemblyName, Version, Author, Dependencies)
- ✅ `PluginStatus` Enum (Active, Inactive, Deprecated)

**Phase 2: 動態載入 (Infrastructure)**
- ✅ `IPluginLoader` Interface (LoadPluginAsync, UnloadPluginAsync, GetLoadedPlugins)
- ✅ `PluginLoader` 實作 (AssemblyLoadContext, Plugin 隔離)
- ✅ `IPluginActivator` Interface (ActivatePluginAsync, DeactivatePluginAsync)
- ✅ `PluginActivator` 實作 (狀態管理)

**Phase 3: Application Layer (CQRS)**
- ✅ `RegisterPluginCommand` + `RegisterPluginCommandHandler`
- ✅ `UpdatePluginCommand` + `UpdatePluginCommandHandler`
- ✅ `GetPluginVersionsQuery` + `GetPluginVersionsQueryHandler`
- ✅ FluentValidation: PluginId, Version, AssemblyPath

**Phase 4: API Layer**
- ✅ `PluginVersionsController` (5 個端點)
- ✅ API: `POST /api/v1/plugin-versions` (註冊 Plugin)
- ✅ API: `GET /api/v1/plugin-versions` (查詢列表)
- ✅ API: `GET /api/v1/plugin-versions/{id}` (獲取詳情)
- ✅ API: `PUT /api/v1/plugin-versions/{id}` (更新狀態)
- ✅ API: `GET /api/v1/plugin-versions/{pluginId}/history` (版本歷史)

**Phase 5: EF Core Repository + Migration**
- ✅ `IPluginVersionRepository` Interface
- ✅ `PluginVersionRepository` 實作
- ✅ EF Migration: `20251111061436_AddPluginVersioning.cs`
- ✅ JSONB Index: `CREATE INDEX idx_plugin_versions_metadata USING GIN (metadata)`

**關鍵技術**:
- AssemblyLoadContext: Plugin 隔離 (isCollectible: true)
- JSONB: 靈活的 Metadata 儲存
- SemVer: 版本號規範 (Major.Minor.Patch)
- Repository Pattern: 資料存取抽象

---

### US 2.2: Plugin 熱重載 (部分完成 40%) 🔄

**Phase 1-2: Commands 實作** ✅ 已完成
- ✅ `ReloadPluginCommand` + `ReloadPluginCommandHandler`
- ✅ `SwitchPluginVersionCommand` + `SwitchPluginVersionCommandHandler`
- ✅ Plugin 載入/卸載邏輯已在 `PluginLoader` 實作

**Phase 3-5: 待完成** ⏳
- ⏳ API 端點實作
- ⏳ Frontend 熱重載 UI
- ⏳ 測試與驗證

**變更說明**: CHANGE-002 - US 2.1 自然延伸至 US 2.2 Phase 1-2

---

### US 2.3: Plugin 版本管理 (部分完成 30%) 🔄

**Phase 1-2: Commands 實作** ✅ 已完成
- ✅ `GetPluginVersionHistoryQuery` + Handler
- ✅ `ComparePluginVersionsQuery` + Handler
- ✅ 版本對比邏輯實作

**Phase 3-5: 待完成** ⏳
- ⏳ API 端點實作
- ⏳ Frontend 版本管理 UI
- ⏳ 測試與驗證

**變更說明**: CHANGE-002 - US 2.1 自然延伸至 US 2.3 Phase 1-2

---

### US 6.1: 基礎聊天介面 (3 SP) ⏳ 待開始

**MVP 範圍**:
- 🎯 對話列表 (Conversation List)
  - 顯示所有對話
  - 創建新對話
  - 刪除對話

- 🎯 聊天介面 (Chat Interface)
  - 訊息列表 (Message List)
  - 輸入框 (Message Input)
  - 發送訊息 (Send Message)

- 🎯 即時更新
  - SignalR 連接
  - 即時訊息推送

**技術實施**:
- React 18 + TypeScript
- Material-UI v5
- TanStack Query (React Query)
- SignalR Client (`@microsoft/signalr`)

**API 依賴**:
- `POST /api/v1/conversations` (創建對話)
- `GET /api/v1/conversations` (查詢對話列表)
- `DELETE /api/v1/conversations/{id}` (刪除對話)
- `POST /api/v1/agents/{id}/execute` (執行 Agent)
- WebSocket `/hubs/execution-monitor` (即時訊息)

---

## 🔌 API 規格速查表

### Agent Execution API (US 1.4) - 11 個端點

#### 1. 執行 Agent
```http
POST /api/v1/agents/{id}/execute
Content-Type: application/json

Request Body:
{
  "userInput": "string",          // 必填
  "conversationId": "uuid",       // 選填
  "parameters": {                 // 選填
    "temperature": 0.7,
    "maxTokens": 2000
  }
}

Response: 201 Created
{
  "id": "uuid",
  "agentId": "uuid",
  "conversationId": "uuid",
  "userInput": "string",
  "response": "string",
  "totalTokens": 150,
  "promptTokens": 50,
  "completionTokens": 100,
  "responseTimeMs": 1234.56,
  "status": "Completed",
  "createdAt": "2025-12-10T10:00:00Z"
}
```

#### 2. 查詢執行歷史（進階過濾）
```http
GET /api/v1/agents/{id}/executions
  ?conversationId={uuid}          // 選填: 按對話過濾
  &status={status}                // 選填: Completed|Failed|Cancelled
  &startDate={ISO8601}            // 選填: 開始日期
  &endDate={ISO8601}              // 選填: 結束日期
  &page={int}                     // 必填: 頁碼 (default: 1)
  &pageSize={int}                 // 必填: 每頁數量 (default: 20, max: 100)
  &sortBy={field}                 // 選填: createdAt|responseTimeMs|totalTokens
  &sortOrder={asc|desc}           // 選填: 排序方向 (default: desc)

Response: 200 OK
{
  "items": [
    { "id": "uuid", "userInput": "...", "response": "...", "totalTokens": 150, ... }
  ],
  "totalCount": 500,
  "page": 1,
  "pageSize": 20,
  "totalPages": 25
}
```

#### 3. 獲取單筆執行記錄
```http
GET /api/v1/executions/{id}

Response: 200 OK
{
  "id": "uuid",
  "agentId": "uuid",
  "conversationId": "uuid",
  "userInput": "string",
  "response": "string",
  "totalTokens": 150,
  "responseTimeMs": 1234.56,
  "status": "Completed",
  "createdAt": "2025-12-10T10:00:00Z"
}
```

#### 4. Agent 統計資訊
```http
GET /api/v1/agents/{id}/statistics
  ?startDate={ISO8601}            // 選填: 統計開始日期
  &endDate={ISO8601}              // 選填: 統計結束日期

Response: 200 OK
{
  "agentId": "uuid",
  "totalExecutions": 1000,
  "successfulExecutions": 950,
  "failedExecutions": 50,
  "avgResponseTimeMs": 1200.5,
  "minResponseTimeMs": 500.0,
  "maxResponseTimeMs": 5000.0,
  "p95ResponseTimeMs": 2500.0,
  "p99ResponseTimeMs": 4000.0,
  "totalTokensUsed": 150000,
  "avgTokensPerExecution": 150,
  "dateRange": {
    "startDate": "2025-12-01T00:00:00Z",
    "endDate": "2025-12-10T23:59:59Z"
  }
}
```

#### 5. 匯出執行歷史（CSV/JSON）
```http
GET /api/v1/agents/{id}/executions/export
  ?format={csv|json}              // 必填: 匯出格式
  &conversationId={uuid}          // 選填: 按對話過濾
  &startDate={ISO8601}            // 選填: 開始日期
  &endDate={ISO8601}              // 選填: 結束日期

Response: 200 OK
Content-Type: text/csv | application/json
Content-Disposition: attachment; filename="agent-{id}-executions-{timestamp}.csv"

CSV Format:
Id,AgentId,ConversationId,UserInput,Response,TotalTokens,ResponseTimeMs,Status,CreatedAt
uuid1,uuid-agent,uuid-conv,"Hello","Hi there",50,800.5,Completed,2025-12-10T10:00:00Z
uuid2,uuid-agent,uuid-conv,"How are you?","I'm good",60,900.2,Completed,2025-12-10T10:05:00Z
```

#### 6-11. SignalR WebSocket 端點
```
WebSocket: /hubs/execution-monitor

Client → Server Methods:
- SubscribeToAgent(agentId: Guid)            // 訂閱特定 Agent 的執行通知
- UnsubscribeFromAgent(agentId: Guid)        // 取消訂閱
- SubscribeToConversation(conversationId: Guid)  // 訂閱特定對話
- UnsubscribeFromConversation(conversationId: Guid)
- SubscribeToAllExecutions()                 // 訂閱所有執行 (管理員)
- UnsubscribeFromAllExecutions()

Server → Client Events:
- ExecutionStarted(executionId: Guid, agentId: Guid, conversationId: Guid, timestamp: DateTime)
- ExecutionProgress(executionId: Guid, message: string, timestamp: DateTime)
- ExecutionCompleted(execution: AgentExecutionDto)
- ExecutionFailed(executionId: Guid, errorMessage: string, timestamp: DateTime)
```

---

### Plugin Versions API (US 2.1) - 5 個端點

#### 1. 註冊 Plugin
```http
POST /api/v1/plugin-versions
Content-Type: application/json

Request Body:
{
  "pluginId": "string",           // 必填: Plugin 唯一識別碼
  "version": "string",            // 必填: SemVer 格式 (1.0.0)
  "name": "string",               // 必填: Plugin 名稱
  "description": "string",        // 選填: 描述
  "assemblyPath": "string",       // 必填: Assembly 檔案路徑
  "metadata": {                   // 必填: Plugin Metadata
    "assemblyName": "string",
    "version": "string",
    "author": "string",
    "dependencies": ["dep1", "dep2"]
  }
}

Response: 201 Created
{
  "id": "uuid",
  "pluginId": "my-plugin",
  "version": "1.0.0",
  "name": "My Plugin",
  "description": "Plugin description",
  "status": "Active",
  "isCurrentVersion": true,
  "createdAt": "2025-12-10T10:00:00Z"
}
```

#### 2. 查詢 Plugin 版本列表
```http
GET /api/v1/plugin-versions
  ?pluginId={string}              // 選填: 按 Plugin ID 過濾
  &status={status}                // 選填: Active|Inactive|Deprecated
  &page={int}                     // 必填: 頁碼 (default: 1)
  &pageSize={int}                 // 必填: 每頁數量 (default: 20)

Response: 200 OK
{
  "items": [
    {
      "id": "uuid",
      "pluginId": "my-plugin",
      "version": "1.2.0",
      "name": "My Plugin",
      "status": "Active",
      "isCurrentVersion": true,
      "downloadCount": 150,
      "activeAgentCount": 10,
      "createdAt": "2025-12-10T10:00:00Z"
    }
  ],
  "totalCount": 50,
  "page": 1,
  "pageSize": 20,
  "totalPages": 3
}
```

#### 3. 獲取 Plugin 版本詳情
```http
GET /api/v1/plugin-versions/{id}

Response: 200 OK
{
  "id": "uuid",
  "pluginId": "my-plugin",
  "version": "1.2.0",
  "name": "My Plugin",
  "description": "Plugin description",
  "metadata": {
    "assemblyName": "MyPlugin.dll",
    "version": "1.2.0",
    "author": "John Doe",
    "dependencies": ["Newtonsoft.Json", "System.Text.Json"]
  },
  "status": "Active",
  "isCurrentVersion": true,
  "assemblyPath": "/plugins/my-plugin/1.2.0/MyPlugin.dll",
  "downloadCount": 150,
  "activeAgentCount": 10,
  "createdAt": "2025-12-10T10:00:00Z",
  "updatedAt": "2025-12-10T12:00:00Z"
}
```

#### 4. 更新 Plugin 狀態
```http
PUT /api/v1/plugin-versions/{id}
Content-Type: application/json

Request Body:
{
  "status": "Inactive"            // Active|Inactive|Deprecated
}

Response: 200 OK
{
  "id": "uuid",
  "pluginId": "my-plugin",
  "version": "1.2.0",
  "status": "Inactive",
  "updatedAt": "2025-12-10T15:00:00Z"
}
```

#### 5. 查詢 Plugin 版本歷史
```http
GET /api/v1/plugin-versions/{pluginId}/history
  ?page={int}
  &pageSize={int}

Response: 200 OK
{
  "pluginId": "my-plugin",
  "items": [
    {
      "id": "uuid1",
      "version": "1.2.0",
      "status": "Active",
      "isCurrentVersion": true,
      "createdAt": "2025-12-10T10:00:00Z"
    },
    {
      "id": "uuid2",
      "version": "1.1.0",
      "status": "Deprecated",
      "isCurrentVersion": false,
      "createdAt": "2025-11-01T10:00:00Z"
    }
  ],
  "totalCount": 5
}
```

---

## 🏗️ 技術架構快速參考

### Backend 技術棧
```yaml
框架: ASP.NET Core 8
語言: C# 12
ORM: Entity Framework Core 8
資料庫: PostgreSQL 16
即時通訊: SignalR (WebSocket)
AI 引擎: Microsoft Semantic Kernel 1.0+

架構模式:
  - Clean Architecture (4-layer)
  - CQRS (MediatR)
  - Repository Pattern
  - Unit of Work

驗證:
  - FluentValidation 11+
  - Data Annotations

測試:
  - xUnit
  - Moq
  - FluentAssertions
```

### Frontend 技術棧
```yaml
框架: React 18
語言: TypeScript 5+
UI 庫: Material-UI v5
狀態管理: Zustand 4+
資料查詢: TanStack Query (React Query) v5
即時通訊: @microsoft/signalr 8+
HTTP 客戶端: Axios 1.6+

構建工具:
  - Vite 5+
  - TypeScript Compiler

測試:
  - Vitest
  - React Testing Library
  - Playwright (E2E)
```

### 資料庫 Schema 設計

#### agent_executions 表（US 1.4）
```sql
CREATE TABLE agent_executions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    agent_id UUID NOT NULL REFERENCES agents(id),
    conversation_id UUID REFERENCES conversations(id),
    user_input TEXT NOT NULL,
    response TEXT NOT NULL,
    total_tokens INTEGER,
    prompt_tokens INTEGER,
    completion_tokens INTEGER,
    response_time_ms DOUBLE PRECISION,
    status VARCHAR(50) NOT NULL,    -- Completed, Failed, Cancelled
    error_message TEXT,
    is_deleted BOOLEAN DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_by VARCHAR(100)
);

-- 索引
CREATE INDEX idx_agent_executions_agent_id ON agent_executions(agent_id);
CREATE INDEX idx_agent_executions_conversation_id ON agent_executions(conversation_id);
CREATE INDEX idx_agent_executions_status ON agent_executions(status);
CREATE INDEX idx_agent_executions_created_at ON agent_executions(created_at DESC);
```

#### plugin_versions 表（US 2.1）
```sql
CREATE TABLE plugin_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    plugin_id VARCHAR(100) NOT NULL,
    version VARCHAR(20) NOT NULL,   -- SemVer: 1.2.3
    name VARCHAR(200) NOT NULL,
    description TEXT,
    metadata JSONB NOT NULL,        -- PluginMetadata
    status VARCHAR(50) NOT NULL,    -- Active, Inactive, Deprecated
    is_current_version BOOLEAN DEFAULT false,
    assembly_path TEXT NOT NULL,
    download_count INTEGER DEFAULT 0,
    active_agent_count INTEGER DEFAULT 0,
    is_deleted BOOLEAN DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_by VARCHAR(100),
    updated_by VARCHAR(100),

    UNIQUE(plugin_id, version)
);

-- 索引
CREATE INDEX idx_plugin_versions_plugin_id ON plugin_versions(plugin_id);
CREATE INDEX idx_plugin_versions_status ON plugin_versions(status);
CREATE INDEX idx_plugin_versions_is_current_version ON plugin_versions(is_current_version);
CREATE INDEX idx_plugin_versions_created_at ON plugin_versions(created_at DESC);

-- JSONB GIN Index
CREATE INDEX idx_plugin_versions_metadata ON plugin_versions USING GIN (metadata);
```

---

## ⚙️ 編碼標準快速參考

### C# 命名規範
```csharp
// PascalCase: Classes, Methods, Properties, Events
public class AgentExecutionService { }
public async Task<Result> ExecuteAgentAsync() { }
public string AgentName { get; set; }
public event EventHandler ExecutionCompleted;

// camelCase: Local variables, Parameters, Private fields
private readonly IAgentRepository _agentRepository;
public async Task ProcessAsync(string agentId, int maxRetries) { }

// Interface: I prefix
public interface IAgentRepository { }
public interface IPluginLoader { }

// Async: Async suffix for async methods
public async Task<Agent> GetAgentAsync(Guid id);
public async Task<List<Agent>> GetAllAgentsAsync();
```

### TypeScript/React 命名規範
```typescript
// PascalCase: Components, Types, Interfaces, Enums
export const AgentCard: React.FC<AgentCardProps> = () => {};
export interface AgentDto { }
export type AgentStatus = 'Active' | 'Inactive';
export enum PluginStatus { Active, Inactive, Deprecated }

// camelCase: Variables, Functions, Hooks
const agentId = 'uuid';
const handleExecute = () => {};
export const useAgents = () => {};
export const useCreateAgent = () => {};

// UPPER_SNAKE_CASE: Constants
export const API_BASE_URL = 'https://api.example.com';
export const MAX_RETRIES = 3;
```

### Git Commit Message 格式
```bash
# 格式: <type>(<scope>): <subject>

# Types:
feat     # 新功能
fix      # Bug 修復
refactor # 代碼重構
docs     # 文檔更新
test     # 測試相關
chore    # 構建/配置相關
style    # 代碼格式調整

# Examples:
feat(agent): implement agent execution with Semantic Kernel
fix(plugin): resolve plugin loading issue in PluginLoader
refactor(execution): extract statistics calculation to separate service
docs(api): update API documentation for execution endpoints
test(agent): add unit tests for AgentExecutionService
```

---

## 🎯 開發優先順序（剩餘工作）

### 1. US 6.1: 基礎聊天介面（最高優先級）⏳
**工作量**: 3 SP (~3 days)

**任務清單**:
- [ ] Backend: Conversations API (CRUD)
  - [ ] CreateConversationCommand + Handler
  - [ ] GetConversationsQuery + Handler
  - [ ] DeleteConversationCommand + Handler
  - [ ] ConversationsController (3 個端點)

- [ ] Frontend: Chat UI Components
  - [ ] ConversationList 組件
  - [ ] ChatWindow 組件
  - [ ] MessageList 組件
  - [ ] MessageInput 組件

- [ ] SignalR Integration
  - [ ] Frontend SignalR 連接設置
  - [ ] 即時訊息接收與顯示

- [ ] 測試
  - [ ] API 集成測試
  - [ ] Component 單元測試
  - [ ] E2E 測試（Playwright）

---

### 2. US 2.2/2.3: Plugin 熱重載與版本管理（中優先級）🔄
**工作量**: ~3-4 days (Phase 3-5)

**US 2.2 剩餘任務**:
- [ ] API 端點
  - [ ] `POST /api/v1/plugin-versions/{id}/reload`
  - [ ] `POST /api/v1/plugin-versions/{id}/switch-version`

- [ ] Frontend UI
  - [ ] Plugin 管理頁面
  - [ ] 熱重載按鈕與狀態顯示

**US 2.3 剩餘任務**:
- [ ] API 端點
  - [ ] `GET /api/v1/plugin-versions/{pluginId}/compare?v1={version1}&v2={version2}`
  - [ ] `POST /api/v1/plugin-versions/{id}/rollback`

- [ ] Frontend UI
  - [ ] 版本對比介面
  - [ ] 版本歷史時間軸

---

## 📚 相關文檔連結

### Sprint 執行文檔
- [Sprint 2 概覽](./SPRINT-2-1-OVERVIEW.md) - Sprint 目標、User Stories 狀態
- [Sprint 2 執行計劃](./SPRINT-2-2-PLAN.md) - 詳細技術實施指南
- [Sprint 2 檢查清單](./SPRINT-2-4-CHECKLIST.md) - 任務追蹤清單
- [Sprint 2 開發日誌](./SPRINT-2-5-DEV-LOG.md) - 每日開發記錄
- [Sprint 2 問題追蹤](./SPRINT-2-6-ISSUES.md) - 問題與解決方案
- [Sprint 2 回顧](./SPRINT-2-7-RETROSPECTIVE.md) - Sprint 完成後總結

### 項目規劃文檔
- [變更記錄](../../4-changes/CHANGE-LOG.md) - CHANGE-001, CHANGE-002
- [User Story 狀態](../../3-progress/USER-STORY-STATUS.md) - 所有 User Stories 狀態追蹤

### 架構與設計
- [架構設計總覽](../../../docs/architecture/Architecture-Design-Document.md) - Clean Architecture、CQRS、系統架構
- [數據庫設計](../../../docs/architecture/database-schema.md) - PostgreSQL Schema、Entity 定義
- [C4 架構圖](../../../docs/architecture/C4-architecture-diagrams.md) - 系統架構視圖、Plugin 系統、SignalR 設計

---

## 📚 完整參考文獻索引

本上下文文檔整合了以下技術細節與架構決策，按類別組織以便 AI Assistant 快速定位：

### Planning 文檔（濃縮版，優先查閱）

- [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Sprint 2 在 MVP 中的範圍與邊界
- [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 2 詳細分配、Story Points、依賴關係
- [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Git 工作流、CI/CD 流程、測試策略
- [Architecture Evolution Roadmap](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進階段規劃
- [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 關鍵技術決策記錄（SignalR、AssemblyLoadContext）
- [Dependency Matrix](../../1-planning/DEPENDENCY-MATRIX.md) - US 1.4, 2.1, 6.1 依賴關係追蹤
- [Risk Register](../../1-planning/RISK-REGISTER.md) - Sprint 2 技術風險評估與緩解策略

### 架構設計決策 (ADR)

- [ADR-002: CQRS Pattern](../../docs/architecture/adr/ADR-002-cqrs-pattern.md)
  - MediatR Commands/Queries 設計模式
  - ExecuteAgentCommand, RegisterPluginCommand 實作參考
- [ADR-006: Agent State Management](../../docs/architecture/adr/ADR-006-agent-state-management.md)
  - Agent 執行狀態管理策略
  - State Machine 設計原則
- [ADR-007: Multi-Agent Communication](../../docs/architecture/adr/ADR-007-multi-agent-communication.md)
  - SignalR WebSocket 通訊架構
  - ExecutionMonitorHub 設計參考
- [ADR-008: Code Interpreter Execution Model](../../docs/architecture/adr/ADR-008-code-interpreter-execution-model.md)
  - 執行引擎安全設計原則
  - 資源隔離策略
- [ADR-011: Framework Migration Strategy](../../docs/architecture/adr/ADR-011-framework-migration-strategy.md)
  - Semantic Kernel 抽象層設計
  - IAgentExecutor 介面定義
- [ADR-012: Workflow Editor Technology](../../docs/architecture/adr/ADR-012-workflow-editor-technology.md)
  - React 18 技術選型理由
  - Material-UI + Zustand 選擇依據
- [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md)
  - Clean Architecture 4-layer 設計
  - 系統架構概覽
- [Database Schema](../../docs/architecture/database-schema.md)
  - agent_executions Table 完整設計
  - plugin_versions Table 完整設計
  - conversations Table 完整設計

### User Stories 完整規格

- [Module 01: Agent Creation](../../docs/user-stories/modules/module-01-agent-creation.md)
  - US 1.4 完整規格（Line 156+）
  - 驗收標準詳細列表
  - 技術實施要求
- [Module 02: Plugin System](../../docs/user-stories/modules/module-02-plugin-system.md)
  - US 2.1 Plugin 註冊規格（Line 22+）
  - US 2.2 Plugin 熱重載規格（Line 171+）
  - US 2.3 Plugin 版本管理規格（Line 280+）
- [Module 06: Chat Interface](../../docs/user-stories/modules/module-06-chat-interface.md)
  - US 6.1 基礎對話功能規格（Line 22+）
  - Chat UI 組件需求
  - SignalR 集成需求

### Backend 技術實施參考 (.NET 9)

- [Semantic Kernel Integration](../../docs/technical-implementation/01-backend-net9/08-semantic-kernel-integration.md)
  - Kernel Builder 配置範例
  - OpenAI Chat Completion 整合
  - Prompt 管理最佳實踐
- [CQRS Implementation](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md)
  - MediatR 配置與註冊
  - Command/Query Handler 實作範例
  - FluentValidation 整合模式
- [Plugin System Architecture](../../docs/technical-implementation/01-backend-net9/11-plugin-system-architecture.md)
  - AssemblyLoadContext 動態加載詳解
  - Plugin Isolation 實作
  - Unload 機制實作
- [SignalR WebSocket](../../docs/technical-implementation/01-backend-net9/10-signalr-websocket.md)
  - SignalR Hub 實作範例
  - CORS 配置
  - Group 管理實作
- [Repository Pattern](../../docs/technical-implementation/01-backend-net9/06-repository-pattern.md)
  - Generic Repository 實作
  - Specification Pattern 應用
  - EF Core 最佳實踐
- [Value Objects](../../docs/technical-implementation/01-backend-net9/07-value-objects.md)
  - VersionNumber (SemVer) 實作
  - PluginMetadata 實作
  - Value Converters 設計

### Frontend 技術實施參考 (React 18)

- [React Coding Standards](../../docs/technical-implementation/04-coding-standards/react-coding-standards.md)
  - Functional Components 規範
  - Hooks 使用指引
  - 效能優化建議
- [TypeScript Coding Standards](../../docs/technical-implementation/04-coding-standards/typescript-coding-standards.md)
  - TypeScript 類型系統
  - Interface vs Type 選擇
  - Generics 應用
- [State Management (Zustand)](../../docs/technical-implementation/02-frontend-react/06-state-management-zustand.md)
  - Zustand Store 設計
  - State Slicing 模式
  - Middleware 應用
- [API Client Integration](../../docs/technical-implementation/02-frontend-react/07-api-client-integration.md)
  - Axios 配置
  - Interceptors 實作
  - Error Handling 策略
- [Component Architecture](../../docs/technical-implementation/02-frontend-react/03-component-architecture.md)
  - Smart/Dumb Components 分離
  - Component Composition
  - Props 設計原則

### API 設計規範

- [RESTful API Standards](../../docs/technical-implementation/05-api-design/restful-api-standards.md)
  - REST 設計原則
  - HTTP Methods 使用規範
  - Status Codes 標準
- [API Documentation](../../docs/technical-implementation/05-api-design/api-documentation.md)
  - Swagger/OpenAPI 配置
  - API 端點文檔標準
- [Error Handling](../../docs/technical-implementation/05-api-design/error-handling.md)
  - Result Pattern 實作
  - Exception Handling 策略

### 資料庫設計規範

- [Database Design Principles](../../docs/technical-implementation/06-database-standards/database-design-principles.md)
  - 資料庫設計最佳實踐
  - 索引設計策略
- [Entity Framework Core Configuration](../../docs/technical-implementation/06-database-standards/entity-framework-core-configuration.md)
  - Fluent API 配置
  - Value Converters 實作（JSON, SemVer）
- [Database Migration Strategy](../../docs/technical-implementation/06-database-standards/database-migration-strategy.md)
  - EF Core Migrations 工作流
  - Rollback 策略

### 測試規範

- [Testing Strategy](../../docs/technical-implementation/07-testing-strategy/README.md)
  - 測試金字塔
  - 80%+ 覆蓋率目標
- [Unit Testing Standards](../../docs/technical-implementation/07-testing-strategy/unit-testing-standards.md)
  - xUnit 測試框架
  - Moq 模擬框架
  - AAA 模式
- [Integration Testing Standards](../../docs/technical-implementation/07-testing-strategy/integration-testing-standards.md)
  - WebApplicationFactory 使用
  - TestContainers 整合

### UX 設計參考

- [Wireframe: Conversation](../../docs/ux-design/wireframes/low-fidelity/05-conversation.md)
  - Chat UI 佈局設計
  - Message List 組件規範
- [Wireframe: Agent Detail](../../docs/ux-design/wireframes/low-fidelity/04-agent-detail.md)
  - Agent 執行監控介面設計
- [Design System](../../docs/ux-design/design-system/README.md)
  - Material-UI 主題配置
  - 色彩與字體系統
- [Component Library](../../docs/ux-design/design-system/component-library.md)
  - UI 元件使用規範

### 變更管理

- [Change Log](../../4-changes/CHANGE-LOG.md)
  - CHANGE-001: US 1.4 範圍擴展詳情
  - CHANGE-002: US 2.1 延伸至 US 2.2/2.3

---

## 📚 參考文獻索引

本章節提供完整的參考文獻索引，方便快速查找相關文檔。所有文件路徑為相對路徑（相對於 `claudedocs/2-sprints/sprint-2/`）。

### 📁 0-overview (專案概覽)
- [Project Charter](../../0-overview/PROJECT-CHARTER.md) - 專案章程與使命
- [Executive Summary](../../0-overview/EXECUTIVE-SUMMARY.md) - 專案執行摘要
- [Product Vision](../../0-overview/PRODUCT-VISION.md) - 產品願景與目標
- [Success Metrics](../../0-overview/SUCCESS-METRICS.md) - 成功指標定義

### 📁 1-planning (規劃文件)
- [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - 開發策略總覽
- [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - MVP 範圍定義
- [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 分配分析
- [Architecture Evolution Roadmap](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進路線圖
- [Dependency Matrix](../../1-planning/DEPENDENCY-MATRIX.md) - 依賴關係矩陣
- [Risk Register](../../1-planning/RISK-REGISTER.md) - 風險登記表
- [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 技術決策日誌

### 📁 2-sprints (Sprint 執行)

#### Sprint 1 (Week 1-3)
- [SPRINT-1-1-OVERVIEW.md](../sprint-1/SPRINT-1-1-OVERVIEW.md) - Sprint 1 總覽
- [SPRINT-1-2-PLAN.md](../sprint-1/SPRINT-1-2-PLAN.md) - Sprint 1 執行計劃
- [SPRINT-1-3-CONTEXT.md](../sprint-1/SPRINT-1-3-CONTEXT.md) - Sprint 1 情境與參考
- [SPRINT-1-4-CHECKLIST.md](../sprint-1/SPRINT-1-4-CHECKLIST.md) - Sprint 1 檢查清單
- [SPRINT-1-5-DEV-LOG.md](../sprint-1/SPRINT-1-5-DEV-LOG.md) - Sprint 1 開發日誌
- [SPRINT-1-6-ISSUES.md](../sprint-1/SPRINT-1-6-ISSUES.md) - Sprint 1 問題追蹤
- [SPRINT-1-7-RETROSPECTIVE.md](../sprint-1/SPRINT-1-7-RETROSPECTIVE.md) - Sprint 1 回顧

#### Sprint 2 (Week 4-6) - 當前文件
- [SPRINT-2-1-OVERVIEW.md](./SPRINT-2-1-OVERVIEW.md) - Sprint 2 總覽
- [SPRINT-2-2-PLAN.md](./SPRINT-2-2-PLAN.md) - Sprint 2 執行計劃
- **[SPRINT-2-3-CONTEXT.md](./SPRINT-2-3-CONTEXT.md)** - **Sprint 2 上下文參考（本文件）**
- [SPRINT-2-4-CHECKLIST.md](./SPRINT-2-4-CHECKLIST.md) - Sprint 2 檢查清單
- [SPRINT-2-5-DEV-LOG.md](./SPRINT-2-5-DEV-LOG.md) - Sprint 2 開發日誌
- [SPRINT-2-6-ISSUES.md](./SPRINT-2-6-ISSUES.md) - Sprint 2 問題追蹤
- [SPRINT-2-7-RETROSPECTIVE.md](./SPRINT-2-7-RETROSPECTIVE.md) - Sprint 2 回顧

#### Sprint 3 (Week 7-9)
- [SPRINT-3-1-OVERVIEW.md](../sprint-3/SPRINT-3-1-OVERVIEW.md) - Sprint 3 總覽
- [SPRINT-3-2-PLAN.md](../sprint-3/SPRINT-3-2-PLAN.md) - Sprint 3 執行計劃
- [SPRINT-3-3-CONTEXT.md](../sprint-3/SPRINT-3-3-CONTEXT.md) - Sprint 3 情境與參考

### 📁 4-changes (變更管理)
- [CHANGE-LOG.md](../../4-changes/CHANGE-LOG.md) - 變更日誌
  - CHANGE-001: US 1.4 範圍擴展 (+8 SP)
  - CHANGE-002: US 2.1 延伸至 US 2.2/2.3
- [SCOPE-CHANGE-REQUEST-TEMPLATE.md](../../4-changes/SCOPE-CHANGE-REQUEST-TEMPLATE.md) - 變更請求範本

### 📁 5-processes (流程文件)
- [Sprint Execution Framework](../../SPRINT-EXECUTION-FRAMEWORK.md) - Sprint 執行框架
- [Documentation Inventory](../../DOCUMENTATION-INVENTORY.md) - 文檔清單
- [Docs Content Index](../../DOCS-CONTENT-INDEX.md) - 文檔內容索引
- [Docs Reading Checklist](../../DOCS-READING-CHECKLIST.md) - 文檔閱讀檢查清單

### 📁 6-implementation-plans (實作計劃)
*實作計劃文檔將在各 Sprint 執行過程中逐步建立*

### 📁 7-archive (歸檔文件)

#### US 1.4 完成報告
- [US-1.4-Phase1-Summary.md](../../7-archive/US-1.4-Phase1-Summary.md) - 基礎執行引擎
- [US-1.4-Phase2-Summary.md](../../7-archive/US-1.4-Phase2-Summary.md) - 執行歷史追蹤
- [US-1.4-Phase3-Performance-Metrics-Summary.md](../../7-archive/US-1.4-Phase3-Performance-Metrics-Summary.md) - 效能指標
- [US-1.4-Phase4-Realtime-Monitoring-Export-Summary.md](../../7-archive/US-1.4-Phase4-Realtime-Monitoring-Export-Summary.md) - 即時監控與匯出

#### 歸檔規劃文件
- [planning/MASTER-DEVELOPMENT-SCHEDULE.md](../../7-archive/planning/MASTER-DEVELOPMENT-SCHEDULE.md) - 原始主開發排程（已被 Sprint 模型取代）

### 📁 docs/architecture (架構設計)
- [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md) - 架構設計總文件
- [Framework Abstraction Layer](../../docs/architecture/Framework-Abstraction-Layer.md) - 框架抽象層設計
- [System Architecture Overview](../../docs/architecture/system-architecture-overview.md) - 系統架構總覽
- [Database Schema](../../docs/architecture/database-schema.md) - 資料庫設計文檔
- [C4 Architecture Diagrams](../../docs/architecture/C4-architecture-diagrams.md) - C4 架構圖

### 📁 docs/bmad (BMad 方法論)
- [BMad Product Requirements](../../docs/bmad/product-requirements.md) - 產品需求文件
- [BMad Architecture](../../docs/bmad/architecture/) - 架構設計資料夾

### 📁 docs/api (API 文件)
- [API Design Principles](../../docs/api/api-design-principles.md) - API 設計原則
- [Agent Execution API](../../docs/api/agent-execution.md) - Agent 執行 API 規格
- [Plugin Management API](../../docs/api/plugin-management.md) - Plugin 管理 API 規格

### 📁 docs/user-stories (User Stories)
- [Module 01: Agent Creation](../../docs/user-stories/modules/module-01-agent-creation.md) - US 1.1-1.4 完整規格
- [Module 02: Plugin System](../../docs/user-stories/modules/module-02-plugin-system.md) - US 2.1-2.3 完整規格
- [Module 06: Chat Interface](../../docs/user-stories/modules/module-06-chat-interface.md) - US 6.1 完整規格

### 📁 docs/technical-implementation (技術實作)

#### 01-backend-net9
- [Semantic Kernel Integration](../../docs/technical-implementation/01-backend-net9/08-semantic-kernel-integration.md)
- [CQRS Implementation](../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md)
- [Plugin System Architecture](../../docs/technical-implementation/01-backend-net9/11-plugin-system-architecture.md)
- [SignalR WebSocket](../../docs/technical-implementation/01-backend-net9/10-signalr-websocket.md)
- [Repository Pattern](../../docs/technical-implementation/01-backend-net9/06-repository-pattern.md)
- [Value Objects](../../docs/technical-implementation/01-backend-net9/07-value-objects.md)

#### 02-frontend-react
- [Component Architecture](../../docs/technical-implementation/02-frontend-react/03-component-architecture.md)
- [State Management (Zustand)](../../docs/technical-implementation/02-frontend-react/06-state-management-zustand.md)
- [API Client Integration](../../docs/technical-implementation/02-frontend-react/07-api-client-integration.md)

#### 04-coding-standards
- [React Coding Standards](../../docs/technical-implementation/04-coding-standards/react-coding-standards.md)
- [TypeScript Coding Standards](../../docs/technical-implementation/04-coding-standards/typescript-coding-standards.md)
- [.NET Coding Standards](../../docs/technical-implementation/04-coding-standards/dotnet-coding-standards.md)

#### 05-api-design
- [RESTful API Standards](../../docs/technical-implementation/05-api-design/restful-api-standards.md)
- [API Documentation](../../docs/technical-implementation/05-api-design/api-documentation.md)
- [Error Handling](../../docs/technical-implementation/05-api-design/error-handling.md)

#### 06-database-standards
- [Database Design Principles](../../docs/technical-implementation/06-database-standards/database-design-principles.md)
- [Entity Framework Core Configuration](../../docs/technical-implementation/06-database-standards/entity-framework-core-configuration.md)
- [Database Migration Strategy](../../docs/technical-implementation/06-database-standards/database-migration-strategy.md)

#### 07-testing-strategy
- [Testing Strategy Overview](../../docs/technical-implementation/07-testing-strategy/README.md)
- [Unit Testing Standards](../../docs/technical-implementation/07-testing-strategy/unit-testing-standards.md)
- [Integration Testing Standards](../../docs/technical-implementation/07-testing-strategy/integration-testing-standards.md)

### 📁 docs/ux-design (UX 設計)

#### Design System
- [Design System README](../../docs/ux-design/design-system/README.md) - 設計系統總覽
- [Component Library](../../docs/ux-design/design-system/component-library.md) - 元件庫規範

#### Wireframes (低擬真度)
- [01-agent-list](../../docs/ux-design/wireframes/low-fidelity/01-agent-list.md) - Agent 清單頁面
- [02-agent-form](../../docs/ux-design/wireframes/low-fidelity/02-agent-form.md) - Agent 表單
- [03-plugin-management](../../docs/ux-design/wireframes/low-fidelity/03-plugin-management.md) - Plugin 管理
- [04-agent-detail](../../docs/ux-design/wireframes/low-fidelity/04-agent-detail.md) - Agent 詳情
- [05-conversation](../../docs/ux-design/wireframes/low-fidelity/05-conversation.md) - 對話介面

---

## 📋 版本歷史

| 版本 | 日期 | 變更摘要 | 變更者 |
|------|------|---------|--------|
| v2.1 | 2025-12-11 | 格式升級至 v2.1 標準：<br/>• 新增 8 欄位標準化 Header<br/>• 擴展目錄結構（核心 + 輔助章節）<br/>• 新增使用指南章節<br/>• 新增完整參考文獻索引（50+ 文檔）<br/>• 新增版本歷史表格<br/>• 100% 保留原有內容 | Claude Code |
| v2.0 | 2025-12-11 | 新增完整參考文獻索引：<br/>• 涵蓋 50+ 相關文檔<br/>• 按資料夾分類組織<br/>• 包含 Sprint 1-3 文檔連結<br/>• 優先引用 /claudedocs/1-planning | AI Development Assistant |
| v1.1 | 2025-12-10 | 擴展 API 規格速查表：<br/>• 新增完整 Agent Execution API 範例<br/>• 新增 Plugin Versions API 範例<br/>• 新增資料庫 Schema 設計細節 | AI Development Assistant |
| v1.0 | 2025-12-10 | 初始版本：<br/>• 建立關鍵文檔索引（帶行號）<br/>• 建立 MVP 範圍快速參考<br/>• 建立技術架構快速參考<br/>• 建立編碼標準快速參考 | AI Development Assistant |

---

**維護說明**: 本文檔為 Sprint 2 的上下文參考文檔，在 Sprint 進行中持續更新。請參考 [SPRINT-2-5-DEV-LOG.md](./SPRINT-2-5-DEV-LOG.md) 查看最新開發進度。
