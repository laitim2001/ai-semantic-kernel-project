# SPRINT-4-5-DEV-LOG.md - Sprint 4 開發日誌：Persona Framework 與 Plugin 熱重載實作記錄

**版本**: v2.1
**Sprint 編號**: Sprint 4
**Sprint 週期**: Week 10-12 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2025-12-16 ~ 2026-01-05
**狀態**: 📋 計劃階段 (Planned) - 將在 Sprint 執行時更新
**創建日期**: 2025-11-13
**最後更新**: 2025-11-13

---

## 📋 文件目的

本文件作為 Sprint 4 執行期間的**每日開發日誌**，記錄：
- 每日開發進度與完成任務
- 技術決策與實作細節
- 遇到的問題與解決方案
- 程式碼變更與 Commit 記錄
- 團隊協作與溝通事項

---

## 📑 目錄 (Table of Contents)

1. [使用指南](#-使用指南)
2. [Week 10 開發日誌](#week-10-開發日誌)
3. [Week 11 開發日誌](#week-11-開發日誌)
4. [Week 12 開發日誌](#week-12-開發日誌)
5. [技術決策記錄](#-技術決策記錄)
6. [問題與解決方案](#-問題與解決方案)
7. [經驗總結](#-經驗總結)
8. [參考文檔](#-參考文檔)
9. [版本歷史](#-版本歷史)

---

## 📖 使用指南

### 適用對象
- **開發團隊**: 記錄每日進度與技術決策
- **Tech Lead**: 追蹤開發狀況與問題
- **Scrum Master**: 監控 Sprint 進展與阻礙
- **未來團隊成員**: 了解歷史決策與實作脈絡

### 更新時機
- **每日站會後**: 更新昨日完成的任務與今日計劃
- **重要決策時**: 即時記錄技術決策與理由
- **遇到問題時**: 記錄問題與暫時/最終解決方案
- **完成任務時**: 記錄實作細節與 Commit ID

### 記錄格式
```markdown
### 📅 YYYY-MM-DD (DayOfWeek) - 標題

#### ✅ 完成任務
- 任務描述與完成細節
- 相關 Commit: `commit-hash`

#### 🔧 技術決策
- 決策內容與理由

#### ⚠️ 遇到問題
- 問題描述
- 解決方案或暫時處理

#### 📝 備註
- 其他重要資訊
```

---

## Week 10 開發日誌

> **Week 10 目標**: Persona 領域模型、Repository、API 端點、配置系統與預設模板

### 📅 2025-12-16 (Monday) - Sprint 4 啟動與領域模型設計

#### ✅ 完成任務
- [ ] **Sprint Planning 會議**
  - 時間: 09:00-11:00
  - 參與者: 全團隊
  - 產出: Sprint 4 Backlog 確認，13 SP 分配

- [ ] **Persona Domain Entities 設計**
  - 建立 `Persona.cs` Entity
    ```csharp
    // src/AIAgentPlatform.Domain/Entities/Persona.cs
    public class Persona : BaseEntity
    {
        public string Name { get; private set; }
        public string Description { get; private set; }
        public PersonaConfiguration Configuration { get; private set; }
        public bool IsActive { get; private set; }
    }
    ```
  - 建立 `PersonaTemplate.cs` Entity
  - 建立 `PromptTemplate.cs` Entity
  - 相關 Commit: `[待填寫]`

- [ ] **Value Objects 設計**
  - 建立 `CommunicationStyle` VO
  - 建立 `ResponsePattern` VO
  - 建立 `SafetyGuardrails` VO
  - 實作 Equality 與 Validation
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-015: Persona Configuration 儲存格式**
  - 決策: 使用 PostgreSQL JSONB 欄位儲存配置
  - 理由: 彈性高、查詢效能佳、支援 JSON 操作
  - 替代方案: EAV 模式（過於複雜）、獨立表格（過於死板）

#### 📝 備註
- [ ] Team 對 Persona 配置格式達成共識
- [ ] 預計明天完成 Repository 實作

---

### 📅 2025-12-17 (Tuesday) - Repository 與 Commands 實作

#### ✅ 完成任務
- [ ] **IPersonaRepository 介面設計**
  - 定義完整 CRUD 方法
  - 新增 `GetByTemplateId` 方法
  - 新增 `FindByConfiguration` 方法
  - 相關 Commit: `[待填寫]`

- [ ] **PersonaRepository 實作**
  - EF Core 實作完成
  - 新增 Configuration JSONB 索引
  - 實作搜尋與篩選邏輯
  - 相關 Commit: `[待填寫]`

- [ ] **CQRS Commands 實作**
  - `CreatePersonaCommand` + Handler
  - `UpdatePersonaCommand` + Handler
  - FluentValidation 驗證規則
  - Domain Events 發布
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-016: Persona Configuration 序列化策略**
  - 決策: 使用 System.Text.Json
  - 理由: .NET 原生、效能佳、與 ASP.NET Core 整合良好
  - 設定: `JsonSerializerOptions` 統一配置

#### ⚠️ 遇到問題
- [ ] **問題**: JSONB 欄位的 EF Core Mapping 不直觀
  - 解決方案: 使用 `HasColumnType("jsonb")` 與自訂 Value Converter
  - 參考: [EF Core JSON Columns](https://learn.microsoft.com/en-us/ef/core/modeling/value-conversions)

#### 📝 備註
- [ ] Repository 單元測試覆蓋率達到 90%

---

### 📅 2025-12-18 (Wednesday) - Queries 與 API 端點

#### ✅ 完成任務
- [ ] **CQRS Queries 實作**
  - `GetPersonaByIdQuery` + Handler
  - `GetPersonasQuery` (含分頁) + Handler
  - `GetPersonaTemplatesQuery` + Handler
  - 相關 Commit: `[待填寫]`

- [ ] **PersonasController 實作**
  - POST `/api/personas` - 建立 Persona
  - GET `/api/personas/{id}` - 查詢單一 Persona
  - GET `/api/personas` - 查詢列表 (含分頁)
  - PUT `/api/personas/{id}` - 更新 Persona
  - DELETE `/api/personas/{id}` - 刪除 Persona
  - GET `/api/personas/templates` - 取得模板列表
  - Swagger 文檔完整
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-017: API 回應格式統一**
  - 決策: 使用 Result Pattern 包裝所有回應
  - 格式: `{ "isSuccess": true, "data": {...}, "errors": [] }`
  - 理由: 一致的錯誤處理、前端易於處理

#### 📝 備註
- [ ] Postman 測試所有端點通過
- [ ] Swagger UI 文檔清晰完整

---

### 📅 2025-12-19 (Thursday) - 配置載入與驗證

#### ✅ 完成任務
- [ ] **PersonaConfigurationLoader 實作**
  - JSON 配置解析器
  - YAML 配置解析器
  - 格式自動檢測
  - 錯誤處理與友善訊息
  - 相關 Commit: `[待填寫]`

- [ ] **Schema 驗證機制**
  - JSON Schema 定義
  - FluentValidation 規則完整
  - 必填欄位檢查
  - 資料型別驗證
  - 格式一致性檢查
  - 相關 Commit: `[待填寫]`

- [ ] **配置一致性檢測**
  - CommunicationStyle 一致性檢查
  - ResponsePattern 邏輯驗證
  - SafetyGuardrails 衝突檢測
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-018: YAML 解析器選擇**
  - 決策: 使用 YamlDotNet 套件
  - 理由: 成熟穩定、社群活躍、與 .NET 整合良好
  - 版本: YamlDotNet 13.x

#### ⚠️ 遇到問題
- [ ] **問題**: YAML 中的特殊字元需要 escape
  - 解決方案: 提供清晰的錯誤訊息與範例
  - 文檔: 在使用指南中說明特殊字元處理

#### 📝 備註
- [ ] 配置驗證錯誤訊息非常清晰易懂

---

### 📅 2025-12-20 (Friday) - 預設 Persona 模板建立

#### ✅ 完成任務
- [ ] **10 種預設 Persona 模板**
  - Professional Assistant 模板
  - Friendly Helper 模板
  - Technical Expert 模板
  - Creative Companion 模板
  - Formal Consultant 模板
  - Casual Advisor 模板
  - Educational Tutor 模板
  - Business Analyst 模板
  - Code Reviewer 模板
  - Domain Expert 模板
  - 所有模板通過 Schema 驗證
  - 相關 Commit: `[待填寫]`

- [ ] **模板文檔撰寫**
  - 每個模板的使用場景說明
  - 配置參數詳細說明
  - 範例配置檔案 (JSON + YAML)
  - 最佳實踐建議
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-019: 模板儲存位置**
  - 決策: 模板定義存於資料庫，範例檔案存於 `templates/` 目錄
  - 理由: 模板可動態管理，範例檔案可版本控制
  - 資料庫遷移: 新增 `PersonaTemplates` Seed Data

#### 📝 備註
- [ ] Week 10 目標達成：Persona 基礎架構完成
- [ ] 下週重點：Prompt Engineering 與 Plugin 熱重載

---

## Week 11 開發日誌

> **Week 11 目標**: Prompt Template Engine、Prompt 生成、Plugin 熱重載機制

### 📅 2025-12-23 (Monday) - Prompt Template Engine 核心

#### ✅ 完成任務
- [ ] **PromptTemplateEngine 設計**
  - 模板語法定義 (變數 `{{var}}`、條件 `{#if}`、循環 `{#each}`)
  - 模板解析器實作
  - 變數替換引擎
  - 條件邏輯處理
  - 相關 Commit: `[待填寫]`

- [ ] **Persona 行為注入**
  - CommunicationStyle 注入邏輯
  - ResponsePattern 注入邏輯
  - Tone 與 Formality 調整
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-020: 模板語法選擇**
  - 決策: 使用類似 Handlebars 的語法
  - 理由: 簡單易懂、業界標準、易於擴展
  - 實作: 自訂輕量級解析器（不依賴外部套件）

#### 📝 備註
- [ ] Prompt 模板引擎基礎功能完成

---

### 📅 2025-12-24 (Tuesday) - Prompt 生成與優化

#### ✅ 完成任務
- [ ] **PromptGenerationService 實作**
  - 基礎 Prompt 生成
  - Persona 配置整合
  - 安全守則注入
  - Context 整合
  - 相關 Commit: `[待填寫]`

- [ ] **動態 Prompt 調整**
  - 根據對話歷史調整
  - 根據使用者偏好調整
  - Token 限制處理
  - 相關 Commit: `[待填寫]`

- [ ] **Prompt 優化**
  - Prompt 長度優化
  - 語義清晰度優化
  - Token 效率優化
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-021: Token 計算策略**
  - 決策: 使用 SharpToken 套件預估 Token 數
  - 理由: 支援多種 OpenAI 模型、計算準確
  - 閾值: System Prompt 保持在 1500 tokens 以下

#### ⚠️ 遇到問題
- [ ] **問題**: 動態調整可能導致 Prompt 不穩定
  - 解決方案: 設定調整幅度上限，記錄調整歷史
  - 監控: 新增 Prompt 版本追蹤

#### 📝 備註
- [ ] Prompt 生成效能良好，平均 <50ms

---

### 📅 2025-12-25 (Wednesday) - Plugin 熱重載架構

#### ✅ 完成任務
- [ ] **IPluginLoader 介面擴展**
  - `LoadPluginVersion` 方法定義
  - `UnloadPluginVersion` 方法定義
  - `GetActiveVersion` 方法定義
  - `ListVersions` 方法定義
  - 相關 Commit: `[待填寫]`

- [ ] **AppDomain 隔離機制**
  - 獨立 AppDomain 建立邏輯
  - Plugin 載入到隔離域
  - 記憶體隔離驗證
  - 卸載與清理邏輯
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-022: Plugin 隔離策略**
  - 決策: 使用 AppDomain 隔離 (與 ADR-014 一致)
  - 理由: .NET Framework 穩定支援、完整記憶體隔離
  - 注意: .NET 9 需要特別處理 AppDomain API 變化

#### 📝 備註
- [ ] AppDomain 隔離測試通過，記憶體隔離有效

---

### 📅 2025-12-26 (Thursday) - 熱重載實作與版本協調

#### ✅ 完成任務
- [ ] **PluginLoaderService 實作**
  - FileSystemWatcher 監聽實作
  - 檔案變更檢測邏輯
  - 自動重載觸發機制
  - 版本號自動管理
  - 相關 Commit: `[待填寫]`

- [ ] **版本協調機制**
  - 新舊版本並存邏輯
  - 平滑切換實作
  - Agent 綁定版本管理
  - 版本回退機制
  - 相關 Commit: `[待填寫]`

- [ ] **依賴管理**
  - Plugin 依賴檢測
  - 依賴版本衝突處理
  - 自動依賴載入
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-023: 版本切換策略**
  - 決策: 新 Agent 使用新版本，舊 Agent 完成後切換
  - 理由: 平滑過渡、不中斷執行中的 Agent
  - 超時: 舊版本保留 5 分鐘後強制卸載

#### ⚠️ 遇到問題
- [ ] **問題**: FileSystemWatcher 可能多次觸發
  - 解決方案: 實作 Debounce 機制，300ms 內只處理一次
  - 參考: [FileSystemWatcher 最佳實踐](https://learn.microsoft.com/en-us/dotnet/api/system.io.filesystemwatcher)

#### 📝 備註
- [ ] 熱重載基本功能完成

---

### 📅 2025-12-27 (Friday) - 記憶體管理與穩定性測試

#### ✅ 完成任務
- [ ] **記憶體洩漏測試**
  - 長時間運行測試 (8 小時)
  - 記憶體使用監控
  - AppDomain 卸載驗證
  - 垃圾回收測試
  - 結果: 無記憶體洩漏
  - 相關 Commit: `[待填寫]`

- [ ] **穩定性測試**
  - 頻繁重載測試 (100 次)
  - 併發重載測試 (10 個並發)
  - 異常情況處理測試
  - 結果: 系統穩定，無崩潰
  - 相關 Commit: `[待填寫]`

#### 📝 備註
- [ ] Week 11 目標達成：Prompt Engineering 與 Plugin 熱重載完成
- [ ] 下週重點：整合測試與文檔

---

## Week 12 開發日誌

> **Week 12 目標**: 整合測試、E2E 測試、文檔、驗收與交付

### 📅 2025-12-30 (Monday) - 測試準備與基礎設施

#### ✅ 完成任務
- [ ] **測試環境準備**
  - 測試資料庫初始化
  - 測試 Persona 資料準備
  - 測試 Plugin 準備
  - 相關 Commit: `[待填寫]`

- [ ] **測試案例設計**
  - Persona CRUD 測試案例 (15 個)
  - Prompt 生成測試案例 (12 個)
  - Plugin 熱重載測試案例 (10 個)
  - 相關 Commit: `[待填寫]`

#### 📝 備註
- [ ] 測試環境就緒，所有測試資料準備完成

---

### 📅 2025-12-31 (Tuesday) - 單元與整合測試

#### ✅ 完成任務
- [ ] **Unit Tests 執行**
  - PersonaService 單元測試：覆蓋率 **87%** ✅
  - PromptGenerationService 單元測試：覆蓋率 **88%** ✅
  - PluginLoader 單元測試：覆蓋率 **86%** ✅
  - 所有單元測試通過：**152/152** ✅
  - 相關 Commit: `[待填寫]`

- [ ] **Integration Tests 執行**
  - Persona API 整合測試：**23/23** 通過 ✅
  - Plugin Reload API 整合測試：**15/15** 通過 ✅
  - 整合測試覆蓋率：**82%** ✅
  - 相關 Commit: `[待填寫]`

#### 📝 備註
- [ ] 測試覆蓋率目標達成

---

### 📅 2026-01-02 (Thursday) - E2E 測試與 Bug 修復

#### ✅ 完成任務
- [ ] **E2E Tests 執行**
  - Persona 配置載入流程：**5/5** 通過 ✅
  - Plugin 熱重載流程：**4/4** 通過 ✅
  - 錯誤恢復流程：**3/3** 通過 ✅
  - 相關 Commit: `[待填寫]`

- [ ] **Bug 修復**
  - 修復測試中發現的 2 個 Bug
  - 回歸測試：所有測試通過 ✅
  - 相關 Commit: `[待填寫]`

#### 📝 備註
- [ ] E2E 測試全部通過

---

### 📅 2026-01-03 (Friday) - 文檔撰寫

#### ✅ 完成任務
- [ ] **API 文檔**
  - Swagger/OpenAPI 文檔完整
  - 所有端點範例完整
  - 錯誤碼說明完整
  - 相關 Commit: `[待填寫]`

- [ ] **Persona 配置指南**
  - 配置格式說明
  - 10 種模板使用指南
  - 最佳實踐文檔
  - 常見問題 FAQ
  - 相關 Commit: `[待填寫]`

- [ ] **Plugin 熱重載指南**
  - 使用流程說明
  - 版本管理說明
  - 注意事項與最佳實踐
  - 相關 Commit: `[待填寫]`

- [ ] **技術實作文檔**
  - 架構設計說明
  - 技術決策記錄 (TD-015 ~ TD-023)
  - 擴展指南
  - 相關 Commit: `[待填寫]`

#### 📝 備註
- [ ] 所有文檔撰寫完成

---

### 📅 2026-01-05 (Sunday) - 驗收與交付

#### ✅ 完成任務
- [ ] **驗收標準檢查**
  - 執行完整驗收檢查清單：**100%** 通過 ✅
  - 確認所有 DoD 達成：**100%** ✅
  - Code Review 完成：通過 ✅
  - 相關 Commit: `[待填寫]`

- [ ] **Demo 準備與執行**
  - Demo 腳本準備完成
  - Demo 環境設置完成
  - Demo 資料準備完成
  - Demo 順利執行：成功 ✅

#### Sprint Review
- [ ] **功能展示**
  - Persona 模板配置展示
  - Persona-Driven Prompt 生成展示
  - Plugin 熱重載展示
  - Stakeholders 反饋：正面 ✅

#### Sprint Retrospective
- [ ] **回顧會議**
  - 時間: 16:00-18:00
  - 參與者: 全團隊
  - 產出: RETROSPECTIVE.md 完成

#### 📝 備註
- [ ] **Sprint 4 正式交付** ✅
- [ ] 13 Story Points 全部完成
- [ ] 所有驗收標準達成

---

## 🔧 技術決策記錄

本節記錄 Sprint 4 期間的重要技術決策。

### TD-015: Persona Configuration 儲存格式
- **日期**: 2025-12-16
- **決策**: 使用 PostgreSQL JSONB 欄位儲存配置
- **理由**: 彈性高、查詢效能佳、支援 JSON 操作
- **影響**: Domain Layer, Infrastructure Layer
- **狀態**: ✅ 已實施

### TD-016: Persona Configuration 序列化策略
- **日期**: 2025-12-17
- **決策**: 使用 System.Text.Json
- **理由**: .NET 原生、效能佳、與 ASP.NET Core 整合良好
- **影響**: Application Layer, Infrastructure Layer
- **狀態**: ✅ 已實施

### TD-017: API 回應格式統一
- **日期**: 2025-12-18
- **決策**: 使用 Result Pattern 包裝所有回應
- **理由**: 一致的錯誤處理、前端易於處理
- **影響**: API Layer, Frontend
- **狀態**: ✅ 已實施

### TD-018: YAML 解析器選擇
- **日期**: 2025-12-19
- **決策**: 使用 YamlDotNet 13.x
- **理由**: 成熟穩定、社群活躍、與 .NET 整合良好
- **影響**: Application Layer
- **狀態**: ✅ 已實施

### TD-019: 模板儲存位置
- **日期**: 2025-12-20
- **決策**: 模板定義存於資料庫，範例檔案存於 `templates/` 目錄
- **理由**: 模板可動態管理，範例檔案可版本控制
- **影響**: Infrastructure Layer, Repository
- **狀態**: ✅ 已實施

### TD-020: 模板語法選擇
- **日期**: 2025-12-23
- **決策**: 使用類似 Handlebars 的語法
- **理由**: 簡單易懂、業界標準、易於擴展
- **影響**: Prompt Engineering
- **狀態**: ✅ 已實施

### TD-021: Token 計算策略
- **日期**: 2025-12-24
- **決策**: 使用 SharpToken 套件預估 Token 數
- **理由**: 支援多種 OpenAI 模型、計算準確
- **影響**: Prompt Generation
- **狀態**: ✅ 已實施

### TD-022: Plugin 隔離策略
- **日期**: 2025-12-25
- **決策**: 使用 AppDomain 隔離
- **理由**: .NET Framework 穩定支援、完整記憶體隔離
- **影響**: Plugin System
- **狀態**: ✅ 已實施

### TD-023: 版本切換策略
- **日期**: 2025-12-26
- **決策**: 新 Agent 使用新版本，舊 Agent 完成後切換
- **理由**: 平滑過渡、不中斷執行中的 Agent
- **影響**: Plugin Hot Reload
- **狀態**: ✅ 已實施

---

## ⚠️ 問題與解決方案

本節記錄開發過程中遇到的問題與解決方案。

### 問題 #1: JSONB 欄位的 EF Core Mapping
- **日期**: 2025-12-17
- **描述**: JSONB 欄位的 EF Core Mapping 不直觀
- **影響**: Repository 實作進度
- **解決方案**:
  - 使用 `HasColumnType("jsonb")` 與自訂 Value Converter
  - 參考官方文檔完成配置
- **狀態**: ✅ 已解決
- **相關**: TD-015

### 問題 #2: YAML 特殊字元處理
- **日期**: 2025-12-19
- **描述**: YAML 中的特殊字元需要 escape
- **影響**: 使用者體驗
- **解決方案**:
  - 提供清晰的錯誤訊息
  - 在使用指南中說明特殊字元處理
  - 提供範例配置
- **狀態**: ✅ 已解決
- **相關**: TD-018

### 問題 #3: 動態調整 Prompt 穩定性
- **日期**: 2025-12-24
- **描述**: 動態調整可能導致 Prompt 不穩定
- **影響**: Prompt 品質
- **解決方案**:
  - 設定調整幅度上限
  - 記錄調整歷史
  - 新增 Prompt 版本追蹤
- **狀態**: ✅ 已解決
- **相關**: TD-021

### 問題 #4: FileSystemWatcher 多次觸發
- **日期**: 2025-12-26
- **描述**: FileSystemWatcher 可能多次觸發相同事件
- **影響**: Plugin 重載效能
- **解決方案**:
  - 實作 Debounce 機制
  - 300ms 內只處理一次
  - 參考官方最佳實踐
- **狀態**: ✅ 已解決
- **相關**: TD-023

---

## 💡 經驗總結

### 做得好的地方 (What Went Well)

1. **Persona 配置設計清晰**
   - JSON/YAML 雙格式支援受到好評
   - Schema 驗證防止了大量錯誤配置
   - 10 種預設模板覆蓋常見場景

2. **Prompt Engineering 效能優異**
   - Prompt 生成時間 <50ms
   - Token 效率提升 >20%
   - 動態調整邏輯穩定

3. **Plugin 熱重載穩定性高**
   - 記憶體洩漏測試通過
   - 頻繁重載測試通過
   - 版本協調機制正確

4. **測試覆蓋率達標**
   - 單元測試覆蓋率 >85%
   - 整合測試覆蓋率 >80%
   - E2E 測試全部通過

### 可以改進的地方 (What Could Be Improved)

1. **AppDomain API 學習曲線**
   - .NET 9 的 AppDomain API 變化較大
   - 需要更多時間研究最佳實踐
   - 建議：建立內部知識庫

2. **YAML 配置使用者體驗**
   - YAML 特殊字元處理不夠直觀
   - 錯誤訊息可以更友善
   - 建議：提供配置生成工具

3. **E2E 測試時間較長**
   - 完整 E2E 測試需要 2 小時
   - 影響快速反饋
   - 建議：增加並行測試、優化測試資料

### 行動項目 (Action Items for Next Sprint)

1. **建立 AppDomain 最佳實踐文檔**
   - 負責人: [待指定]
   - 截止日期: Sprint 5 Week 1
   - 優先級: P1

2. **開發 Persona 配置生成工具**
   - 負責人: [待指定]
   - 截止日期: Sprint 5 Week 2
   - 優先級: P2

3. **優化 E2E 測試效能**
   - 負責人: [待指定]
   - 截止日期: Sprint 5 Week 1
   - 優先級: P1

---

## 📚 參考文檔

### Planning
1. [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md)
2. [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-4)
3. [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md)
4. [Technical Decisions Log](../../1-planning/TECHNICAL-DECISIONS-LOG.md)

### User Stories
5. [Module 07: Persona Framework](../../docs/user-stories/modules/module-07-persona-framework.md)
6. [Module 02: Plugin System](../../docs/user-stories/modules/module-02-plugin-system.md)

### Architecture
7. [Architecture Design Document](../../docs/architecture/Architecture-Design-Document.md)
8. [ADR-014: Plugin Isolation Strategy](../../docs/architecture/adr/ADR-014-plugin-isolation-strategy.md)

### Technical Implementation
9. [Persona Framework](../../docs/technical-implementation/01-backend-net9/03-persona-framework.md)
10. [Prompt Engineering](../../docs/technical-implementation/01-backend-net9/09-prompt-engineering.md)
11. [Plugin System MCP](../../docs/technical-implementation/01-backend-net9/04-plugin-system-mcp.md)

### Sprint 文檔
12. [SPRINT-4-1-OVERVIEW.md](./SPRINT-4-1-OVERVIEW.md)
13. [SPRINT-4-2-PLAN.md](./SPRINT-4-2-PLAN.md)
14. [SPRINT-4-3-CONTEXT.md](./SPRINT-4-3-CONTEXT.md)
15. [SPRINT-4-4-CHECKLIST.md](./SPRINT-4-4-CHECKLIST.md)
16. [SPRINT-4-6-ISSUES.md](./SPRINT-4-6-ISSUES.md)
17. [SPRINT-4-7-RETROSPECTIVE.md](./SPRINT-4-7-RETROSPECTIVE.md)

---

## 📋 版本歷史

### v2.1 (2025-11-13)
- ✅ 建立 Sprint 4 DEV-LOG 模板
- ✅ 定義開發日誌格式與使用指南
- ✅ 規劃 3 週日誌結構
- ✅ 建立技術決策與問題追蹤區塊

---

**文件版本**: v2.1
**最後更新**: 2025-11-13
**維護者**: Sprint 4 開發團隊

**備註**: 本文件將在 Sprint 4 執行期間每日更新，記錄實際開發進度與決策。
