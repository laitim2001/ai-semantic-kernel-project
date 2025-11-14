# SPRINT 10 - 開發日誌 (Development Log)

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

1. [日誌使用說明](#日誌使用說明)
2. [Week 28 - Phase 1: Workflow Definition CRUD](#week-28---phase-1-workflow-definition-crud)
3. [Week 29 - Phase 2: Node Registry + Edge Validation](#week-29---phase-2-node-registry--edge-validation)
4. [Week 30 - Phase 3-4: 整合測試與文檔](#week-30---phase-3-4-整合測試與文檔)
5. [技術決策記錄](#技術決策記錄)
6. [問題追蹤](#問題追蹤)
7. [經驗教訓](#經驗教訓)

---

## 日誌使用說明

### 日誌格式

每日日誌包含以下部分:
- **日期與進度**: 當前日期, Sprint Day, 整體進度百分比
- **今日目標**: 計劃完成的任務 (參考 CHECKLIST)
- **完成工作**: 實際完成的任務清單
- **技術決策**: 當天做出的技術決策 (TD-XXX)
- **遇到的問題**: 阻礙、Bug、技術挑戰
- **解決方案**: 問題的解決方法
- **明日計劃**: 下一個工作日的計劃
- **備註**: 其他重要信息

### 更新頻率
- **每日更新**: 每個工作日結束前更新
- **問題記錄**: 遇到問題時即時記錄
- **技術決策**: 做出決策時即時記錄

### Phase 1C 啟動特別記錄
- **Sprint 10 Day 1**: 記錄 Phase 1B → Phase 1C 轉換
- **Phase 1C 啟動儀式**: Sprint Kickoff Meeting 要點
- **Phase 1B 完成驗收**: Sprint 9 交付驗收確認

---

## Week 28 - Phase 1: Workflow Definition CRUD

**Phase 目標**: 實現 Workflow Definition 完整 CRUD API
**預計完成**: 52 tasks (31% of Sprint 10)

---

### Day 1 (2026-04-21, Monday) - Sprint 10 Start & Phase 1C 啟動

**日期**: 2026-04-21 (Monday)
**Sprint Day**: Day 1/15
**進度**: 0% → 目標 7%

#### 📋 Phase 1C 啟動記錄

**Phase 1B → Phase 1C 轉換**:
- ✅ Phase 1B (Sprint 7-9) 驗收完成
- ✅ Framework Abstraction Layer 100% 交付
- ✅ 無已知 P0/P1 bugs
- ✅ Phase 1C 技術準備檢查通過

**Sprint 10 Kickoff Meeting**:
- 時間: 09:00 - 10:30
- 參與者: Dev Team, Architect, PM, Tech Lead
- 議程:
  - Phase 1B 完成回顧 (15 min)
  - Phase 1C 目標與範疇 (20 min)
  - Sprint 10 任務分配 (30 min)
  - 技術上下文講解 (20 min)
  - Q&A (15 min)

**Phase 1C 里程碑宣布**:
- 🎉 Phase 1B Critical Path 100% 完成
- 🚀 Phase 1C 正式啟動 (Workflow Editor Backend)
- 🎯 Sprint 10-11 目標: Backend API + Validation
- 📅 Sprint 12-14 預告: Frontend 視覺化編輯器

#### 今日目標
- [x] Sprint 10 Kickoff Meeting
- [x] Phase 1C 啟動儀式
- [ ] 環境準備與依賴檢查
- [ ] WorkflowDefinition 實體設計 (T10.001 - T10.004)
- [ ] NodeDefinition 實體設計 (T10.005 - T10.007)
- [ ] EdgeDefinition 實體設計 (T10.008 - T10.010)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] 完成 WorkflowDefinition Domain Model (T10.001 - T10.010)
- [ ] EF Core Configuration 設計 (T10.011 - T10.013)
- [ ] Database Migration 創建 (T10.014)

#### 備註
- **Phase 轉換**: Phase 1B → Phase 1C 順利完成
- **團隊士氣**: 高度興奮,準備開始新 Phase

---

### Day 2 (2026-04-22, Tuesday)

**日期**: 2026-04-22 (Tuesday)
**Sprint Day**: Day 2/15
**進度**: 7% → 目標 14%

#### 今日目標
- [ ] 完成 WorkflowDefinition Domain Model (T10.001 - T10.010)
- [ ] EF Core Configuration 設計 (T10.011 - T10.013)
- [ ] Database Migration 創建 (T10.014)
- [ ] WorkflowDefinitionRepository 介面定義 (T10.015 - T10.016)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] WorkflowDefinitionRepository 實作 (T10.017 - T10.019)
- [ ] Repository 單元測試 (T10.020 - T10.022)
- [ ] CreateWorkflowDefinitionCommand 設計 (T10.023 - T10.026)

#### 備註
_待填寫_

---

### Day 3 (2026-04-23, Wednesday)

**日期**: 2026-04-23 (Wednesday)
**Sprint Day**: Day 3/15
**進度**: 14% → 目標 21%

#### 今日目標
- [ ] WorkflowDefinitionRepository 實作 (T10.017 - T10.019)
- [ ] Repository 單元測試 (T10.020 - T10.022)
- [ ] CreateWorkflowDefinitionCommand 設計 (T10.023 - T10.026)
- [ ] CreateWorkflowDefinitionCommandHandler (T10.027 - T10.029)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] UpdateWorkflowDefinitionCommand 設計 (T10.030 - T10.033)
- [ ] DeleteWorkflowDefinitionCommand 設計 (T10.034 - T10.037)
- [ ] Commands 單元測試 (T10.038 - T10.040)

#### 備註
_待填寫_

---

### Day 4 (2026-04-24, Thursday)

**日期**: 2026-04-24 (Thursday)
**Sprint Day**: Day 4/15
**進度**: 21% → 目標 28%

#### 今日目標
- [ ] UpdateWorkflowDefinitionCommand 設計 (T10.030 - T10.033)
- [ ] DeleteWorkflowDefinitionCommand 設計 (T10.034 - T10.037)
- [ ] Commands 單元測試 (T10.038 - T10.040)
- [ ] GetWorkflowDefinitionByIdQuery (T10.041 - T10.043)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] GetWorkflowDefinitionsQuery 設計 (T10.044 - T10.046)
- [ ] Queries 單元測試 (T10.047 - T10.049)
- [ ] WorkflowDefinitionsController 創建 (T10.050)
- [ ] Week 28 Review Meeting

#### 備註
_待填寫_

---

### Day 5 (2026-04-25, Friday) - Week 28 End

**日期**: 2026-04-25 (Friday)
**Sprint Day**: Day 5/15
**進度**: 28% → 目標 35%

#### 今日目標
- [ ] GetWorkflowDefinitionsQuery 設計 (T10.044 - T10.046)
- [ ] Queries 單元測試 (T10.047 - T10.049)
- [ ] WorkflowDefinitionsController 創建 (T10.050)
- [ ] Week 28 Review Meeting

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] Weekend: 整理 Week 28 產出
- [ ] 準備 Week 29 Node Registry 開發

#### 備註
- **Week 28 總結**: Phase 1 完成 35%, Workflow Definition CRUD 基礎完成
- **下週重點**: Node Registry + Edge Validation

---

## Week 29 - Phase 2: Node Registry + Edge Validation

**Phase 目標**: 實現 Node Registry 和 Edge Validation
**預計完成**: 80 tasks (48% of Sprint 10)

---

### Day 6 (2026-04-28, Monday)

**日期**: 2026-04-28 (Monday)
**Sprint Day**: Day 6/15
**進度**: 35% → 目標 42%

#### 今日目標
- [ ] POST /api/v1/workflow-definitions 實作 (T10.051 - T10.052)
- [ ] Swagger 註解與文檔 (T10.053)
- [ ] Controller 單元測試 (T10.054 - T10.056)
- [ ] NodeTypeDefinition 設計 (T10.057 - T10.060)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] BuiltInNodeTypes 定義 (T10.061 - T10.065)
- [ ] INodeTypeRegistry 介面設計 (T10.066 - T10.069)
- [ ] NodeTypeRegistry 實作 (T10.070 - T10.073)

#### 備註
_待填寫_

---

### Day 7 (2026-04-29, Tuesday)

**日期**: 2026-04-29 (Tuesday)
**Sprint Day**: Day 7/15
**進度**: 42% → 目標 49%

#### 今日目標
- [ ] BuiltInNodeTypes 定義 (T10.061 - T10.065)
- [ ] INodeTypeRegistry 介面設計 (T10.066 - T10.069)
- [ ] NodeTypeRegistry 實作 (T10.070 - T10.073)
- [ ] JSON Schema Validation 整合 (T10.074 - T10.076)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] NodeTypeRegistry 單元測試 (T10.077 - T10.080)
- [ ] NodeTypesController 創建 (T10.081 - T10.084)
- [ ] Node Registry API 整合測試 (T10.085 - T10.087)

#### 備註
_待填寫_

---

### Day 8 (2026-04-30, Wednesday)

**日期**: 2026-04-30 (Wednesday)
**Sprint Day**: Day 8/15
**進度**: 49% → 目標 56%

#### 今日目標
- [ ] NodeTypeRegistry 單元測試 (T10.077 - T10.080)
- [ ] NodeTypesController 創建 (T10.081 - T10.084)
- [ ] Node Registry API 整合測試 (T10.085 - T10.087)
- [ ] NodeCompatibilityMatrix 設計 (T10.088 - T10.091)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] CircularDependencyDetector 設計 (T10.092 - T10.095)
- [ ] DFS Algorithm 實作 (T10.096 - T10.098)
- [ ] Circular Dependency 測試案例 (T10.099 - T10.101)

#### 備註
_待填寫_

---

### Day 9 (2026-05-01, Thursday)

**日期**: 2026-05-01 (Thursday)
**Sprint Day**: Day 9/15
**進度**: 56% → 目標 63%

#### 今日目標
- [ ] CircularDependencyDetector 設計 (T10.092 - T10.095)
- [ ] DFS Algorithm 實作 (T10.096 - T10.098)
- [ ] Circular Dependency 測試案例 (T10.099 - T10.101)
- [ ] IEdgeValidationService 介面設計 (T10.102 - T10.105)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] EdgeValidationService 實作 (T10.106 - T10.109)
- [ ] Edge Validation 單元測試 (T10.110 - T10.113)
- [ ] WorkflowValidationController (T10.114 - T10.117)
- [ ] Week 29 Review Meeting

#### 備註
_待填寫_

---

### Day 10 (2026-05-02, Friday) - Week 29 End

**日期**: 2026-05-02 (Friday)
**Sprint Day**: Day 10/15
**進度**: 63% → 目標 70%

#### 今日目標
- [ ] EdgeValidationService 實作 (T10.106 - T10.109)
- [ ] Edge Validation 單元測試 (T10.110 - T10.113)
- [ ] WorkflowValidationController (T10.114 - T10.117)
- [ ] Week 29 Review Meeting

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] Weekend: 整理 Week 29 產出
- [ ] 準備 Week 30 整合測試與文檔

#### 備註
- **Week 29 總結**: Phase 2 完成 70%, Node Registry + Edge Validation 基礎完成
- **下週重點**: 整合測試、API 文檔、Sprint 回顧

---

## Week 30 - Phase 3-4: 整合測試與文檔

**Phase 目標**: 完成整合測試並生成完整文檔
**預計完成**: 35 tasks (21% of Sprint 10)

---

### Day 11 (2026-05-05, Monday)

**日期**: 2026-05-05 (Monday)
**Sprint Day**: Day 11/15
**進度**: 70% → 目標 77%

#### 今日目標
- [ ] Validation API 整合測試 (T10.118 - T10.120)
- [ ] Edge Validation 錯誤訊息測試 (T10.121 - T10.123)
- [ ] Workflow Definition CRUD 整合測試 (T10.124 - T10.127)
- [ ] TestContainers 配置 (T10.128)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] Node Registry API 整合測試 (T10.129 - T10.131)
- [ ] API 錯誤處理測試 (T10.132 - T10.134)
- [ ] 性能測試 (T10.135 - T10.137)

#### 備註
_待填寫_

---

### Day 12 (2026-05-06, Tuesday)

**日期**: 2026-05-06 (Tuesday)
**Sprint Day**: Day 12/15
**進度**: 77% → 目標 84%

#### 今日目標
- [ ] Node Registry API 整合測試 (T10.129 - T10.131)
- [ ] API 錯誤處理測試 (T10.132 - T10.134)
- [ ] 性能測試 (T10.135 - T10.137)
- [ ] Workflow API 文檔生成 (T10.138 - T10.140)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] Node Registry API 文檔 (T10.141 - T10.143)
- [ ] Validation API 文檔 (T10.144 - T10.146)
- [ ] API 使用範例編寫 (T10.147 - T10.149)

#### 備註
_待填寫_

---

### Day 13 (2026-05-07, Wednesday)

**日期**: 2026-05-07 (Wednesday)
**Sprint Day**: Day 13/15
**進度**: 84% → 目標 91%

#### 今日目標
- [ ] Node Registry API 文檔 (T10.141 - T10.143)
- [ ] Validation API 文檔 (T10.144 - T10.146)
- [ ] API 使用範例編寫 (T10.147 - T10.149)
- [ ] 技術文檔編寫 (T10.150 - T10.152)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] 測試報告生成 (T10.153 - T10.155)
- [ ] Code Coverage 分析 (T10.156 - T10.158)
- [ ] Phase 1C Part 1 完成度評估 (T10.159 - T10.161)

#### 備註
_待填寫_

---

### Day 14 (2026-05-08, Thursday)

**日期**: 2026-05-08 (Thursday)
**Sprint Day**: Day 14/15
**進度**: 91% → 目標 98%

#### 今日目標
- [ ] 測試報告生成 (T10.153 - T10.155)
- [ ] Code Coverage 分析 (T10.156 - T10.158)
- [ ] Phase 1C Part 1 完成度評估 (T10.159 - T10.161)
- [ ] Sprint 10 回顧文檔初稿 (T10.162 - T10.164)

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] Sprint 11 準備評估 (T10.165 - T10.167)
- [ ] Sprint 10 Review & Retrospective
- [ ] Phase 1C Part 1 Demo

#### 備註
_待填寫_

---

### Day 15 (2026-05-09, Friday) - Sprint 10 End

**日期**: 2026-05-09 (Friday)
**Sprint Day**: Day 15/15
**進度**: 98% → 目標 100%

#### 今日目標
- [ ] Sprint 11 準備評估 (T10.165 - T10.167)
- [ ] Sprint 10 Review Meeting
- [ ] Sprint 10 Retrospective
- [ ] Phase 1C Part 1 Demo

#### 完成工作
_待填寫_

#### 技術決策
_待記錄_

#### 遇到的問題
_待記錄_

#### 解決方案
_待記錄_

#### 明日計劃
- [ ] Weekend: 整理 Sprint 10 產出
- [ ] 準備 Sprint 11 Kickoff (Week 31)

#### 備註
- **Sprint 10 總結**: Phase 1C Part 1 完成 100%, 所有交付物已驗證
- **Phase 1C Part 1 里程碑**: Workflow Editor Backend API 基礎完成
- **下週重點**: Sprint 11 Kickoff, Workflow Validation 開發啟動

---

## 技術決策記錄

### TD-076: Workflow Schema 存儲格式決策

**日期**: _待記錄_
**決策者**: Architect + Dev Lead
**背景**: 需要設計靈活且高效的 Workflow Definition 存儲格式

#### 問題陳述
- 如何存儲 Workflow Definition 以支持靈活的 schema evolution?
- 如何平衡查詢性能和數據靈活性?
- 如何支持大型 Workflow (>100 nodes) 的高效存儲和查詢?

#### 選項分析
**選項 1: 純關聯式表結構**
- 優點: 查詢靈活, 支援 SQL JOIN, 強類型
- 缺點: 大型 Workflow 查詢慢, schema 變更需要 migration
- 評估: 不夠靈活

**選項 2: 純 JSONB 欄位**
- 優點: 極度靈活, schema evolution 容易
- 缺點: 查詢複雜, 需要 JSONB 查詢語法, metadata 查詢慢
- 評估: 查詢性能不佳

**選項 3: 混合模式 (Metadata 關聯式 + Content JSONB)** (選擇)
- 優點: 平衡查詢性能和靈活性
- 缺點: 複雜度較高, 需要維護兩套數據結構
- 評估: 最佳方案

#### 決策
選擇選項 3: 混合模式
- WorkflowDefinitions 表存儲 metadata (Name, Status, Version, CreatedAt, etc.)
- DefinitionJson JSONB 欄位存儲完整 workflow definition (JSON 格式)
- NodeDefinitions 表存儲結構化 Node 數據 (便於查詢和驗證)
- EdgeDefinitions 表存儲結構化 Edge 數據 (便於 circular dependency 檢測)

#### 實施計劃
```sql
CREATE TABLE "WorkflowDefinitions" (
    "Id" uuid PRIMARY KEY,
    "Name" varchar(200) NOT NULL,
    "Version" integer NOT NULL DEFAULT 1,
    "Status" integer NOT NULL DEFAULT 0,
    "DefinitionJson" jsonb NOT NULL,
    "CreatedAt" timestamp with time zone NOT NULL,
    "IsDeleted" boolean NOT NULL DEFAULT false
);

CREATE TABLE "NodeDefinitions" (
    "Id" uuid PRIMARY KEY,
    "WorkflowDefinitionId" uuid NOT NULL REFERENCES "WorkflowDefinitions"("Id"),
    "NodeId" varchar(100) NOT NULL,
    "NodeType" varchar(50) NOT NULL,
    "ConfigurationJson" jsonb NOT NULL
);

CREATE TABLE "EdgeDefinitions" (
    "Id" uuid PRIMARY KEY,
    "WorkflowDefinitionId" uuid NOT NULL REFERENCES "WorkflowDefinitions"("Id"),
    "SourceNodeId" varchar(100) NOT NULL,
    "TargetNodeId" varchar(100) NOT NULL
);

-- Indexes for performance
CREATE INDEX "IX_WorkflowDefinitions_Name" ON "WorkflowDefinitions"("Name");
CREATE INDEX "IX_WorkflowDefinitions_Status" ON "WorkflowDefinitions"("Status");
CREATE INDEX "IX_NodeDefinitions_NodeType" ON "NodeDefinitions"("NodeType");
```

#### 影響範圍
- WorkflowDefinition Domain Model 設計 (T10.001 - T10.004)
- EF Core Configuration (T10.011 - T10.013)
- Repository 查詢邏輯 (T10.017 - T10.019)
- API DTO 設計 (需包含 DefinitionJson 和結構化數據)

#### 優點
- ✅ Metadata 查詢快速 (Name, Status, CreatedAt)
- ✅ 支援 schema evolution (JSONB 欄位)
- ✅ 結構化數據便於驗證 (NodeDefinitions, EdgeDefinitions)
- ✅ 支援複雜查詢 (JOIN + JSONB 查詢)

#### 風險
- ⚠️ 數據同步: DefinitionJson 和結構化表需要保持一致
- ⚠️ 複雜度: 需要維護兩套數據結構

#### 緩解措施
- 使用 Domain Events 確保數據同步
- 在 Repository 層封裝同步邏輯
- 完整的單元測試覆蓋

---

### TD-077: Node Registry 實作策略決策

**日期**: _待記錄_
**決策者**: Architect + Dev Lead
**背景**: 需要設計可擴展的 Node Registry 以支持動態註冊

#### 問題陳述
- Node Registry 應該是靜態還是動態?
- 如何支持第三方 Plugin 註冊自定義 Node types?
- 如何管理 Node type 的生命週期?

#### 選項分析
**選項 1: 靜態註冊 (Startup-time only)**
- 優點: 簡單, 性能好, 線程安全
- 缺點: 不支援動態擴展, 需要重啟才能註冊新 Node types
- 評估: 不滿足未來 Plugin 需求

**選項 2: 完全動態註冊 (Runtime registration)** (選擇)
- 優點: 靈活, 支持 Plugin 擴展, 無需重啟
- 缺點: 需要線程安全機制, 複雜度較高
- 評估: 最佳方案

**選項 3: 混合模式 (Built-in static + Custom dynamic)**
- 優點: 平衡性能和靈活性
- 缺點: 實現複雜, 兩套邏輯
- 評估: 過度設計

#### 決策
選擇選項 2: 完全動態註冊
- 使用 ConcurrentDictionary<string, NodeTypeDefinition> 存儲所有 Node types
- 內建 Node types 在 constructor 時註冊
- 支援運行時動態註冊自定義 Node types
- 使用 INodeTypeRegistry 介面抽象實現

#### 實施計劃
```csharp
public class NodeTypeRegistry : INodeTypeRegistry
{
    private readonly ConcurrentDictionary<string, NodeTypeDefinition> _nodeTypes = new();
    private readonly ILogger<NodeTypeRegistry> _logger;

    public NodeTypeRegistry(ILogger<NodeTypeRegistry> logger)
    {
        _logger = logger;
        RegisterBuiltInNodeTypes();
    }

    private void RegisterBuiltInNodeTypes()
    {
        RegisterNodeType(BuiltInNodeTypes.Agent);
        RegisterNodeType(BuiltInNodeTypes.Tool);
        RegisterNodeType(BuiltInNodeTypes.Condition);
        RegisterNodeType(BuiltInNodeTypes.Parallel);
        RegisterNodeType(BuiltInNodeTypes.Sequential);
    }

    public void RegisterNodeType(NodeTypeDefinition nodeType)
    {
        if (_nodeTypes.TryAdd(nodeType.Type, nodeType))
        {
            _logger.LogInformation("Registered node type: {Type}", nodeType.Type);
        }
        else
        {
            _logger.LogWarning("Node type already registered: {Type}", nodeType.Type);
        }
    }

    public NodeTypeDefinition? GetNodeType(string nodeType)
    {
        _nodeTypes.TryGetValue(nodeType, out var definition);
        return definition;
    }
}
```

#### 影響範圍
- INodeTypeRegistry 介面設計 (T10.066 - T10.069)
- NodeTypeRegistry 實作 (T10.070 - T10.073)
- DI Container 註冊 (Singleton lifetime)
- 未來 Plugin System 整合

#### 優點
- ✅ 支援動態擴展 (Plugin system 預留)
- ✅ 線程安全 (ConcurrentDictionary)
- ✅ 簡單實現 (無複雜生命週期管理)
- ✅ 易於測試 (Mock INodeTypeRegistry)

#### 風險
- ⚠️ 記憶體使用: 所有 Node types 常駐內存
- ⚠️ 無 unregister 機制 (Sprint 10 不實作)

#### 緩解措施
- 內建 Node types 數量有限 (<10)
- 記憶體使用可忽略不計 (<1MB)
- 未來可擴展 unregister 機制

---

### TD-078: Circular Dependency Detection 算法決策

**日期**: _待記錄_
**決策者**: Architect + Dev Lead
**背景**: 需要高效檢測 Workflow 中的循環依賴

#### 問題陳述
- 如何檢測 Workflow 中的 circular dependency?
- 使用 DFS 還是 BFS?
- 如何生成有用的錯誤訊息 (circular path)?

#### 選項分析
**選項 1: DFS (Depth-First Search)** (選擇)
- 優點: 簡單, 遞迴實現, 早期檢測, 代碼可讀性高
- 缺點: 遞迴棧深度限制 (實際不是問題)
- 評估: 最佳方案

**選項 2: BFS (Breadth-First Search)**
- 優點: 迭代實現, 無遞迴棧限制
- 缺點: 複雜, 不如 DFS 直觀, 難以追蹤 circular path
- 評估: 不推薦

**選項 3: Tarjan's Algorithm (Strongly Connected Components)**
- 優點: 可找出所有 strongly connected components
- 缺點: 過度複雜, 對本場景不必要
- 評估: 過度設計

#### 決策
選擇選項 1: DFS (Depth-First Search)
- 時間複雜度: O(V + E), V = nodes, E = edges
- 空間複雜度: O(V), 遞迴棧 + visited/recursionStack 集合
- 早期檢測: 發現循環即可停止
- 易於追蹤 circular path

#### 實施計劃
```csharp
public class CircularDependencyDetector
{
    private readonly Dictionary<string, List<string>> _adjacencyList;
    private readonly HashSet<string> _visited = new();
    private readonly HashSet<string> _recursionStack = new();

    public bool HasCircularDependency()
    {
        foreach (var node in _adjacencyList.Keys)
        {
            if (!_visited.Contains(node))
            {
                if (DFS(node))
                    return true;
            }
        }
        return false;
    }

    private bool DFS(string node)
    {
        _visited.Add(node);
        _recursionStack.Add(node);

        if (_adjacencyList.TryGetValue(node, out var neighbors))
        {
            foreach (var neighbor in neighbors)
            {
                if (!_visited.Contains(neighbor))
                {
                    if (DFS(neighbor))
                        return true;
                }
                else if (_recursionStack.Contains(neighbor))
                {
                    return true; // Found cycle
                }
            }
        }

        _recursionStack.Remove(node);
        return false;
    }
}
```

#### 影響範圍
- CircularDependencyDetector 設計 (T10.092 - T10.095)
- DFS Algorithm 實作 (T10.096 - T10.098)
- Edge Validation Service 整合 (T10.106 - T10.109)

#### 優點
- ✅ 簡單實現 (遞迴邏輯清晰)
- ✅ 高效 O(V + E)
- ✅ 早期檢測 (發現即停止)
- ✅ 易於追蹤 circular path

#### 性能分析
- 最壞情況: O(V + E), 遍歷所有節點和邊
- 平均情況: O(V), 大部分 Workflow 沒有循環
- 空間: O(V), 遞迴棧深度通常 <100

#### 測試策略
- 測試無循環 Workflow
- 測試簡單循環 (A → B → A)
- 測試複雜循環 (A → B → C → A)
- 測試多個獨立循環
- 測試深度嵌套 (>50 nodes)

---

### TD-079: JSON Schema Validation 庫選擇

**日期**: _待記錄_
**決策者**: Dev Lead
**背景**: 需要選擇 .NET JSON Schema validation 庫

#### 問題陳述
- 使用哪個 .NET JSON Schema library?
- 是否支持 JSON Schema Draft 7?

#### 選項分析
**選項 1: NJsonSchema** (選擇)
- 優點: 成熟, 活躍維護, 支援 Draft 7, 錯誤訊息詳細
- 缺點: 較大 (依賴多)
- 評估: 最佳方案

**選項 2: Json.Schema.Net**
- 優點: 輕量, 現代化
- 缺點: 相對較新, 社群較小
- 評估: 可選

**選項 3: 自己實現**
- 優點: 完全控制
- 缺點: 開發成本極高, 不推薦
- 評估: 不可行

#### 決策
選擇 NJsonSchema
- 版本: NJsonSchema 11.0+
- 支援 JSON Schema Draft 7
- 詳細錯誤訊息
- 成熟穩定

#### 實施計劃
```bash
dotnet add package NJsonSchema
```

```csharp
public bool ValidateConfiguration(string nodeType, string configurationJson)
{
    var definition = GetNodeType(nodeType);
    if (definition == null)
        return false;

    try
    {
        var schema = JsonSchema.FromJsonAsync(definition.ConfigurationSchema).Result;
        var instance = JsonDocument.Parse(configurationJson);
        var errors = schema.Validate(instance.RootElement);

        return !errors.Any();
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Configuration validation failed");
        return false;
    }
}
```

#### 影響範圍
- NodeTypeRegistry validation 邏輯 (T10.074 - T10.076)
- Node configuration validation 測試 (T10.077 - T10.080)

---

### TD-080: API 版本化策略

**日期**: _待記錄_
**決策者**: Architect + Dev Lead
**背景**: 需要設計 API 版本化策略以支持未來演進

#### 問題陳述
- 如何設計 API 版本化?
- 使用 URL path (/v1/) 還是 HTTP header?

#### 選項分析
**選項 1: URL Path Versioning (/api/v1/)** (選擇)
- 優點: 簡單, 直觀, 易於測試, Swagger 支援好
- 缺點: URL 變更
- 評估: 最佳方案

**選項 2: HTTP Header Versioning**
- 優點: URL 不變
- 缺點: 複雜, 測試困難, Swagger 支援不佳
- 評估: 不推薦

#### 決策
選擇 URL Path Versioning
- 格式: /api/v1/workflow-definitions
- 版本: v1 (Sprint 10)
- 未來版本: v2, v3 (視需要)

#### 實施計劃
```csharp
[ApiController]
[Route("api/v1/[controller]")]
public class WorkflowDefinitionsController : ControllerBase
{
    // API endpoints
}
```

#### 影響範圍
- 所有 API Controllers (T10.050, T10.081, T10.114)
- Swagger 配置
- API 文檔

---

### TD-081: Error Response 標準化格式

**日期**: _待記錄_
**決策者**: Dev Lead
**背景**: 需要標準化 API 錯誤響應格式

#### 問題陳述
- 如何設計統一的錯誤響應格式?
- 如何提供有用的錯誤訊息給 Frontend?

#### 選項分析
**選項 1: RFC 7807 Problem Details** (選擇)
- 優點: 標準化, ASP.NET Core 內建支援
- 缺點: 稍複雜
- 評估: 最佳方案

**選項 2: 自定義 ErrorResponse**
- 優點: 完全控制
- 缺點: 非標準
- 評估: 不推薦

#### 決策
選擇 RFC 7807 Problem Details
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

#### 實施計劃
```csharp
// Program.cs
builder.Services.AddProblemDetails();

// Middleware
app.UseExceptionHandler();
app.UseStatusCodePages();
```

#### 影響範圍
- 所有 API Controllers 錯誤處理
- FluentValidation 整合
- API 文檔錯誤範例

---

## 問題追蹤

### 問題記錄格式

每個問題包含:
- **ID**: 問題唯一標識 (I10-XXX)
- **日期**: 問題發現日期
- **嚴重程度**: Critical / High / Medium / Low
- **狀態**: Open / In Progress / Resolved / Closed
- **描述**: 問題詳細描述
- **影響**: 對 Sprint 的影響
- **負責人**: 問題負責人
- **解決方案**: 問題解決方法
- **解決日期**: 問題解決日期

---

### I10-001: [預留位置] 問題標題

**ID**: I10-001
**日期**: _待記錄_
**嚴重程度**: _待記錄_
**狀態**: Open

#### 描述
_待記錄_

#### 影響
_待記錄_

#### 負責人
_待記錄_

#### 解決方案
_待記錄_

#### 解決日期
_待記錄_

---

### I10-002: [預留位置] 問題標題

**ID**: I10-002
**日期**: _待記錄_
**嚴重程度**: _待記錄_
**狀態**: Open

#### 描述
_待記錄_

#### 影響
_待記錄_

#### 負責人
_待記錄_

#### 解決方案
_待記錄_

#### 解決日期
_待記錄_

---

### I10-003: [預留位置] 問題標題

**ID**: I10-003
**日期**: _待記錄_
**嚴重程度**: _待記錄_
**狀態**: Open

#### 描述
_待記錄_

#### 影響
_待記錄_

#### 負責人
_待記錄_

#### 解決方案
_待記錄_

#### 解決日期
_待記錄_

---

## 經驗教訓

### 成功經驗 (What Went Well)

#### Week 28 經驗
_待記錄_

#### Week 29 經驗
_待記錄_

#### Week 30 經驗
_待記錄_

---

### 需要改進 (What Could Be Improved)

#### Week 28 改進點
_待記錄_

#### Week 29 改進點
_待記錄_

#### Week 30 改進點
_待記錄_

---

### 行動項目 (Action Items)

| ID | 行動項目 | 負責人 | 截止日期 | 狀態 |
|----|----------|--------|----------|------|
| A10.1 | _待記錄_ | _待指定_ | _待設定_ | Open |
| A10.2 | _待記錄_ | _待指定_ | _待設定_ | Open |
| A10.3 | _待記錄_ | _待指定_ | _待設定_ | Open |

---

### 技術債務記錄

| ID | 技術債務 | 嚴重程度 | 預計解決時間 | 負責人 | 狀態 |
|----|----------|----------|--------------|--------|------|
| TD10.1 | _待記錄_ | _待評估_ | _待估算_ | _待指定_ | Open |
| TD10.2 | _待記錄_ | _待評估_ | _待估算_ | _待指定_ | Open |
| TD10.3 | _待記錄_ | _待評估_ | _待估算_ | _待指定_ | Open |

---

## Sprint 10 總結

### 整體完成度
- **計劃任務**: 167 tasks
- **完成任務**: _待統計_
- **完成率**: _待計算_

### 關鍵成果
1. _待記錄_
2. _待記錄_
3. _待記錄_

### 技術決策總數
- TD-076 到 TD-081: 共 6 個技術決策

### 遇到的主要挑戰
1. _待記錄_
2. _待記錄_
3. _待記錄_

### Phase 1C Part 1 里程碑
- **Phase 1C Part 1 完成度**: 目標 100%
- **Sprint 10 總 Story Points**: 8 SP
- **下一步**: Sprint 11 (Workflow Validation + Schema Versioning)

### 下一步計劃
- **Sprint 11**: Workflow Validation API (完整驗證邏輯)
- **Phase 1C 完成**: Sprint 11 結束 (Phase 1C 100%)

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**總技術決策**: 6 個 (TD-076 到 TD-081)
**總問題記錄**: 3 個預留位置 (I10-001 到 I10-003)
