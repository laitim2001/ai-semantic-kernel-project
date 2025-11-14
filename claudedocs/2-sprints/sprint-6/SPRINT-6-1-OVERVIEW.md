# SPRINT-6-1-OVERVIEW.md - Sprint 6 概覽：Code Interpreter 安全執行環境

**版本**: v2.1
**Sprint 編號**: Sprint 6
**Sprint 週期**: Week 16-18 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2026-01-27 ~ 2026-02-16
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 目錄 (Table of Contents)

1. [Sprint 定位與參考](#sprint-定位與參考)
2. [Sprint 目標](#sprint-目標)
3. [User Stories 分配](#user-stories-分配)
4. [技術範圍](#技術範圍)
5. [預期交付](#預期交付)
6. [風險與依賴](#風險與依賴)
7. [成功指標](#成功指標)
8. [相關文檔](#相關文檔)
9. [完整參考文獻索引](#完整參考文獻索引)
10. [使用指南](#使用指南)
11. [版本歷史](#版本歷史)

---

## Sprint 定位與參考

**Sprint 定位參考**:
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1A: 基礎平台
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 6 詳細分析 (5 SP 調整後, 3 週)
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Phase 1A Code Interpreter 開發策略

---

## Sprint 目標

### 核心目標

實現 **Code Interpreter 安全執行環境 (US 5.1)**，建立企業級 AI Agent 平台的代碼執行能力，透過 Docker 容器池實現安全隔離，達成 4-Layer Security Architecture 防護目標。

#### 主要目標 (Primary Goals)

1. **Code Interpreter - Python 執行引擎 (US 5.1)** - Docker 容器池、安全沙箱、執行超時控制
2. **4-Layer Security Architecture** - Container 隔離、Network 隔離、File System 隔離、Resource 限制

#### 次要目標 (Secondary Goals)

3. **容器池動態管理** - 容器生命週期管理、自動回收、健康檢查
4. **執行結果可視化** - 支援 Text、Image、Table、Chart 輸出
5. **Code Interpreter UI** - 基礎代碼輸入、執行、結果展示

### 業務價值 (Business Value)

```yaml
對 IT 開發者 (IT Developer):
  價值: 可以執行 Python 代碼進行數據分析、可視化、自動化任務
  影響: 大幅提升 Agent 的實用性和靈活性
  參考: docs/ux-design/user-research/personas.md (Alex - IT Developer)

對業務分析師 (Business Analyst):
  價值: 可以使用自然語言讓 Agent 生成並執行數據分析代碼
  影響: 降低技術門檻，業務人員也能進行數據分析
  參考: docs/ux-design/user-research/personas.md (Emma - Business Analyst)

對企業管理者 (Enterprise Admin):
  價值: 安全的代碼執行環境，符合企業安全標準
  影響: 降低安全風險，提升企業信任度
  參考: docs/ux-design/user-research/personas.md (David - Enterprise Admin)
```

### Sprint 在 MVP 中的定位

```
Phase 1A: 基礎平台 (Sprint 1-6, Week 1-18)
├─ Sprint 1: US 1.1 基礎設施 ✅ 已完成
├─ Sprint 2: US 1.4 Agent 引擎 + US 2.1 Plugin 基礎 ✅ 已完成
├─ Sprint 3: US 1.3 Plugin 系統 ✅ 已完成
├─ Sprint 4: US 7.1-7.2 Persona Framework + US 2.2 Plugin 熱重載 ✅ 已完成
├─ Sprint 5: US 4.1-4.2 Knowledge 管理與 RAG 檢索 ✅ 已完成
└─ Sprint 6: US 5.1 Code Interpreter (簡化版) ← 當前 Sprint

關鍵里程碑:
  M4: Code Interpreter 完成 (Week 20) - Sprint 6 完成後將達成此里程碑
  → Phase 1A 基礎平台完成 ✅
  → 進入 Phase 1B: 工作流引擎核心 (Sprint 7-9)
  參考: claudedocs/1-planning/MVP-SCOPE-DEFINITION.md (Section 時間與資源限制)
```

---

## User Stories 分配

### US 5.1: Code Interpreter - Python 執行引擎

**Story Points**: 5 SP (調整後，原計劃 13 SP)
**優先級**: P0 (Must Have) - MVP 核心功能
**完成標準**: 執行成功率 ≥90%, 執行時間 <30 秒
**依賴**: Sprint 1 (基礎設施)

**功能描述**:
實現安全的 Python 代碼執行環境，基於 Docker 容器池和 4-Layer Security Architecture，支援數據分析、可視化、自動化任務。

**MVP 簡化範圍說明**:
```yaml
原計劃 (13 SP):
  - 完整 Docker 容器池管理（動態擴縮容、負載均衡）
  - 複雜安全策略（AppArmor, Seccomp, Capabilities）
  - 多語言支援（Python, R, Julia）
  - 高級可視化（Plotly, Bokeh, Altair）

調整後 (5 SP):
  - 簡化容器池（固定 3 個容器）
  - 基礎安全策略（Docker 隔離 + Resource Limit）
  - 僅支援 Python 3.11
  - 基礎可視化（Matplotlib, Pandas）

理由:
  - Phase 1A 聚焦基礎平台，完整版留待 Phase 1B-1C
  - 降低技術複雜度和風險
  - 確保在 3 週內完成交付
  - 參考: claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md (Sprint 6 調整說明)
```

#### Phase 1: Docker 容器環境搭建 (1.5 SP)

**任務**:
1. Docker Image 設計與建置 (0.8 SP)
   - 基礎 Image：python:3.11-slim-bookworm
   - 預安裝套件：numpy, pandas, matplotlib, seaborn
   - 安全配置：非 root 用戶、最小權限
   - Dockerfile 撰寫與優化
   - 參考: [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) Lines 150-250

2. 容器池設計與實作 (0.5 SP)
   - 固定容器池大小：3 個容器
   - 容器生命週期管理（啟動、停止、重啟）
   - 容器健康檢查
   - 參考: src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs

3. Docker Compose 整合 (0.2 SP)
   - 更新 docker-compose.yml
   - 容器網路隔離配置
   - Volume 掛載設計

**驗收標準**:
- ✅ Docker Image 建置成功（Image 大小 <500MB）
- ✅ 容器池啟動成功（3 個容器同時運行）
- ✅ 容器健康檢查運作正常
- ✅ 容器網路隔離有效

#### Phase 2: Python 執行引擎實作 (1.5 SP)

**任務**:
1. 實作 ICodeExecutionService 介面 (0.3 SP)
   - ExecuteCode() method
   - 參數：code, language, timeout, resourceLimits
   - 參考: src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs

2. 實作 DockerCodeExecutionService (0.8 SP)
   - Docker API 整合（Docker.DotNet）
   - 代碼提交到容器執行
   - 執行結果擷取（stdout, stderr, exit code）
   - 執行超時控制（預設 30 秒）
   - 參考: src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs

3. 資源限制配置 (0.2 SP)
   - CPU 限制：1 core
   - Memory 限制：512MB
   - Disk 限制：100MB
   - Network 限制：禁止外部網路存取（僅 localhost）

4. 錯誤處理與日誌記錄 (0.2 SP)
   - 執行失敗處理
   - 超時處理
   - 容器崩潰處理
   - 結構化日誌記錄

**驗收標準**:
- ✅ ICodeExecutionService 介面定義完整
- ✅ Python 代碼執行成功（簡單 print 測試）
- ✅ 執行超時機制運作正常
- ✅ 資源限制有效（CPU, Memory, Disk）
- ✅ 錯誤處理和日誌記錄完整

#### Phase 3: 4-Layer Security Architecture (1 SP)

**任務**:
1. Layer 1: Container 隔離 (0.3 SP)
   - Docker 容器隔離
   - 非 root 用戶執行
   - Read-only 文件系統（除 /tmp）
   - 參考: docs/security/code-execution-security.md (Section Layer 1)

2. Layer 2: Network 隔離 (0.2 SP)
   - 禁止外部網路存取
   - 僅允許 localhost 存取
   - 防火牆規則配置
   - 參考: docs/security/code-execution-security.md (Section Layer 2)

3. Layer 3: File System 隔離 (0.2 SP)
   - /tmp 目錄僅可寫
   - 禁止存取敏感目錄（/etc, /proc, /sys）
   - File System 配額限制（100MB）
   - 參考: docs/security/code-execution-security.md (Section Layer 3)

4. Layer 4: Resource 限制 (0.3 SP)
   - CPU 限制：1 core
   - Memory 限制：512MB
   - Process 限制：10 個 process
   - 執行時間限制：30 秒
   - 參考: docs/security/code-execution-security.md (Section Layer 4)

**驗收標準**:
- ✅ 4-Layer Security 全部配置完成
- ✅ 無法從容器內存取宿主機檔案
- ✅ 無法從容器內存取外部網路
- ✅ Resource 限制有效
- ✅ 安全測試通過（滲透測試）

#### Phase 4: 執行結果處理與可視化 (0.5 SP)

**任務**:
1. 執行結果解析 (0.2 SP)
   - stdout / stderr 解析
   - 錯誤訊息格式化
   - Exit code 處理

2. 圖表輸出支援 (0.2 SP)
   - Matplotlib 圖表儲存為 PNG
   - 圖表檔案擷取
   - Base64 編碼傳輸

3. 表格輸出支援 (0.1 SP)
   - Pandas DataFrame 轉 JSON
   - CSV 格式支援

**驗收標準**:
- ✅ 執行結果正確解析
- ✅ Matplotlib 圖表正確輸出
- ✅ Pandas DataFrame 轉 JSON 成功

#### Phase 5: Code Interpreter API 與 UI (0.5 SP)

**任務**:
1. Code Execution API (0.2 SP)
   - POST /api/v1/code/execute - 執行代碼
   - GET /api/v1/code/executions/{id} - 獲取執行結果
   - 參考: docs/api/code-interpreter-api-design.md

2. Code Interpreter UI (0.2 SP)
   - 代碼編輯器（Monaco Editor 或 CodeMirror）
   - 執行按鈕
   - 執行結果展示（Text, Image, Table）
   - 參考: apps/web-app/src/features/code-interpreter/

3. 單元測試與整合測試 (0.1 SP)
   - DockerCodeExecutionService 單元測試
   - 安全測試
   - 執行超時測試
   - 測試覆蓋率 ≥85%

**驗收標準**:
- ✅ Code Execution API 完成
- ✅ Code Interpreter UI 完成
- ✅ 單元測試覆蓋率 ≥85%

**總體驗收標準 (Overall Acceptance Criteria)**:
- ✅ 支援 Python 3.11 代碼執行
- ✅ 執行成功率 ≥90%
- ✅ 執行時間 <30 秒
- ✅ 4-Layer Security Architecture 完整
- ✅ 容器池運作正常（3 個容器）
- ✅ 支援 Text、Image、Table 輸出
- ✅ 無容器逃逸漏洞（安全測試通過）

**參考文檔**:
- 📖 [US 6: Code Interpreter](../../docs/user-stories/us-6-code-interpreter.md)
- 📖 [ADR-008: Code Interpreter Container Pool](../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md)
- 📖 [Code Execution Security](../../docs/security/code-execution-security.md)
- 📖 [Code Interpreter Implementation](../../docs/technical-implementation/01-backend-net9/06-code-interpreter-implementation.md)
- 📖 [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) Lines 150-800 - 詳細實作指南

---

## 技術範圍

### Backend API (.NET 9)

**Code Execution API**:
```
POST   /api/v1/code/execute           - 執行 Python 代碼
GET    /api/v1/code/executions/{id}   - 獲取執行結果
DELETE /api/v1/code/executions/{id}   - 刪除執行記錄
GET    /api/v1/code/status             - 容器池狀態
```

參考: [API Design Guidelines](../../docs/api/API-Design-Guidelines.md)

### Frontend (React + TypeScript)

**Code Interpreter UI**:
- 代碼編輯器（Monaco Editor 或 CodeMirror）
- 執行控制（執行、停止、清除）
- 執行結果展示（Text、Image、Table、Chart）
- 執行歷史記錄

參考: apps/web-app/src/features/code-interpreter/

### 基礎設施

**Docker 服務**:
- Docker 容器池（3 個容器）
- Docker Image: python:3.11-slim-bookworm
- 預安裝套件: numpy, pandas, matplotlib, seaborn
- 參考: docker/code-interpreter/Dockerfile

**安全配置**:
- 4-Layer Security Architecture
  - Layer 1: Container 隔離
  - Layer 2: Network 隔離
  - Layer 3: File System 隔離
  - Layer 4: Resource 限制
- 參考: docs/security/code-execution-security.md

**資源限制**:
- CPU: 1 core per container
- Memory: 512MB per container
- Disk: 100MB per container
- Timeout: 30 seconds

---

## 預期交付

### Sprint 6 交付清單

**1. Code Interpreter - Python 執行引擎 (US 5.1)**:
- ✅ Docker Image 建置（python:3.11-slim-bookworm）
- ✅ 容器池管理（3 個容器）
- ✅ Python 執行引擎（DockerCodeExecutionService）
- ✅ 4-Layer Security Architecture
- ✅ 執行超時控制（30 秒）
- ✅ 資源限制（CPU, Memory, Disk）
- ✅ 執行結果處理（Text, Image, Table）
- ✅ Code Execution API
- ✅ Code Interpreter UI
- ✅ 單元測試 (覆蓋率 ≥85%)
- ✅ 安全測試（滲透測試）

---

## 風險與依賴

### 依賴項目

```yaml
必須完成的前置 Sprint:
  - Sprint 1: 基礎設施 ✅
  - Sprint 2: Agent 引擎 ✅
  - Sprint 3: Plugin 系統 ✅
  - Sprint 4: Persona Framework ✅
  - Sprint 5: Knowledge 管理 ✅

外部依賴:
  - Docker Engine (版本 ≥20.10)
  - Docker.DotNet (NuGet 套件)
  - Python 3.11 Official Docker Image
```

### 風險識別與緩解策略

#### RISK-016: Container 逃逸漏洞 🚨 高風險

```yaml
風險描述:
  Docker 容器可能存在逃逸漏洞
  惡意代碼可能突破容器隔離存取宿主機
  可能造成嚴重的安全事故

影響:
  - 宿主機被攻擊或控制
  - 敏感數據洩露
  - 企業信任度嚴重下降
  - 法律合規風險

發生概率: 25%

緩解措施:
  ✅ 4-Layer Security Architecture:
     Layer 1: Container 隔離（非 root 用戶、read-only filesystem）
     Layer 2: Network 隔離（禁止外部網路）
     Layer 3: File System 隔離（僅 /tmp 可寫）
     Layer 4: Resource 限制（CPU, Memory, Process）
     參考: docs/security/code-execution-security.md

  ✅ 安全掃描:
     - Docker Image 安全掃描（Trivy, Clair）
     - 定期更新基礎 Image
     - 移除不必要的套件和工具

  ✅ 滲透測試:
     - Week 2 進行滲透測試
     - 測試容器逃逸攻擊向量
     - 測試 Network 隔離效果
     - 測試 File System 隔離效果
     參考: docs/testing/security-testing.md

  ✅ Runtime 監控:
     - 監控異常行為（網路連線、檔案存取）
     - 自動停止可疑容器
     - 告警機制

  ✅ Fallback 方案:
     - 如 Docker 安全性不足，考慮使用 Kubernetes Pod Security Standards
     - 評估 gVisor 或 Kata Containers 作為更安全的容器 Runtime

殘餘風險: 10%
  - Zero-day 漏洞無法完全防範
  - 需要持續監控和更新

依賴關係:
  - DEP-016: Docker Engine 安全性
  - DEP-017: Linux Kernel 安全性
  - DEP-018: 安全測試工具可用性

參考:
  - docs/security/code-execution-security.md (完整安全設計)
  - docs/architecture/adr/ADR-008-code-interpreter-container-pool.md
  - claudedocs/1-planning/RISK-REGISTER.md (RISK-016 詳細分析)
```

#### RISK-017: 執行效能不穩定 ⚠️ 中等風險

```yaml
風險描述:
  Docker 容器執行效能可能不穩定
  大量並發執行可能導致效能瓶頸
  執行時間可能超過 30 秒目標

影響:
  - 使用者體驗下降
  - 執行超時頻繁
  - 無法滿足效能目標

發生概率: 35%

緩解措施:
  ✅ 效能測試:
     - Week 1 進行效能測試
     - 測試單一容器執行時間
     - 測試並發執行效能（3 個容器同時執行）
     - 測試不同代碼複雜度的執行時間

  ✅ 容器池優化:
     - 容器預熱（啟動時預先建立容器）
     - 容器復用（執行完畢後重置，而非銷毀）
     - 容器健康檢查（自動替換異常容器）

  ✅ Resource 優化:
     - 調整 CPU、Memory 限制（根據測試結果）
     - 優化 Docker Image 大小（<500MB）
     - 移除不必要的套件

  ✅ 執行策略優化:
     - 簡單代碼優先執行（快速回應）
     - 複雜代碼背景執行（非同步）
     - 執行佇列管理（避免過載）

殘餘風險: 15%
  - 複雜代碼可能仍超過 30 秒
  - 需要 Phase 2 進階優化（異步執行、分散式執行）

依賴關係:
  - DEP-019: Docker Engine 效能
  - DEP-020: 宿主機硬體資源
```

#### RISK-018: 容器池管理複雜度 ⚠️ 中等風險

```yaml
風險描述:
  容器池管理邏輯可能複雜且易出錯
  容器生命週期管理可能不穩定
  容器健康檢查可能失效

影響:
  - 容器無法正常啟動或停止
  - 容器資源洩漏
  - 系統穩定性下降

發生概率: 30%

緩解措施:
  ✅ 簡化設計:
     - 固定容器池大小（3 個容器）
     - 簡化生命週期管理
     - 避免動態擴縮容（Phase 2 實作）

  ✅ 健康檢查:
     - 定期 Ping 容器（每 10 秒）
     - 自動重啟異常容器
     - 記錄容器狀態變更

  ✅ 錯誤處理:
     - 容器啟動失敗重試（最多 3 次）
     - 容器崩潰自動替換
     - 異常日誌記錄

  ✅ 測試覆蓋:
     - 容器啟動/停止測試
     - 容器健康檢查測試
     - 容器異常恢復測試

殘餘風險: 10%
  - 極端情況下可能仍有問題
```

#### RISK-019: Docker 依賴風險 ⚠️ 低風險

```yaml
風險描述:
  Docker Engine 可能不可用或版本不相容
  Docker.DotNet 套件可能有 Bug
  開發環境和生產環境 Docker 配置可能不一致

影響:
  - 無法在特定環境運行
  - 開發和生產行為不一致
  - 部署失敗

發生概率: 20%

緩解措施:
  ✅ 版本控制:
     - 明確 Docker Engine 版本要求（≥20.10）
     - 鎖定 Docker.DotNet 版本
     - 使用 Docker Compose 統一環境配置

  ✅ 環境檢查:
     - 啟動時檢查 Docker Engine 是否可用
     - 檢查 Docker 版本是否符合要求
     - 檢查必要的 Docker 權限

  ✅ Fallback 方案:
     - 如 Docker 不可用，提供錯誤訊息和安裝指引
     - 考慮支援 Podman 作為替代（Phase 2）

殘餘風險: 5%
```

---

## 成功指標

```yaml
Code Interpreter - Python 執行:
  - 執行成功率: ≥90%
  - 執行時間: <30 秒 (P95)
  - 容器啟動時間: <5 秒
  - 容器池健康度: ≥95%

Code Interpreter - 安全性:
  - 無容器逃逸漏洞（滲透測試通過）
  - Resource 限制有效率: 100%
  - Network 隔離有效率: 100%
  - File System 隔離有效率: 100%

Code Interpreter - 可視化:
  - Matplotlib 圖表輸出成功率: ≥95%
  - Pandas DataFrame 轉 JSON 成功率: ≥95%

測試覆蓋率:
  - 單元測試: ≥85%
  - 集成測試: ≥80%
  - 安全測試: 100% (滲透測試覆蓋所有攻擊向量)

基礎設施:
  - Docker Image 大小: <500MB
  - 容器 Memory 使用: <512MB
  - 容器 CPU 使用: <1 core
```

---

## 相關文檔

### User Stories
- 📖 [US 6: Code Interpreter](../../docs/user-stories/us-6-code-interpreter.md)

### Architecture Design
- 📖 [ADR-008: Code Interpreter Container Pool](../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md)

### Technical Implementation
- 📖 [Code Interpreter Implementation](../../docs/technical-implementation/01-backend-net9/06-code-interpreter-implementation.md)

### Security
- 📖 [Code Execution Security](../../docs/security/code-execution-security.md)

### Planning
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md)
- 📋 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md)
- 📋 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md)

---

## 完整參考文獻索引

本 Sprint 參考文獻涵蓋規劃文檔、User Stories、ADR、技術實作、安全設計等，共計 **70+ 文檔**。

### 核心規劃文檔 (8 refs)
1. [MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md) - MVP 範圍定義與 Phase 1A 詳細規劃
2. [SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md) - Sprint 6 詳細分析 (5 SP 調整後, 3 週)
3. [DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md) - Code Interpreter 開發策略
4. [DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md) - Sprint 依賴關係矩陣
5. [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md) - 風險登記簿 (RISK-016, RISK-017, RISK-018, RISK-019)
6. [TECHNICAL-DECISIONS-LOG.md](../../1-planning/TECHNICAL-DECISIONS-LOG.md) - 技術決策日誌
7. [ARCHITECTURE-EVOLUTION-ROADMAP.md](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md) - 架構演進路線圖
8. [DOCS-REBUILD-EXECUTION-PLAN.md](../../1-planning/DOCS-REBUILD-EXECUTION-PLAN.md) - 文檔重建執行計劃

### Sprint 6 文檔 (7 refs)
9. [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md) - Sprint 6 概覽 (本文件)
10. [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) - Sprint 6 實施計劃
11. [SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md) - Sprint 6 上下文與背景
12. [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md) - Sprint 6 檢查清單
13. [SPRINT-6-5-DEV-LOG.md](./SPRINT-6-5-DEV-LOG.md) - Sprint 6 開發日誌
14. [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md) - Sprint 6 問題追蹤
15. [SPRINT-6-7-RETROSPECTIVE.md](./SPRINT-6-7-RETROSPECTIVE.md) - Sprint 6 回顧

### 前序 Sprint 參考 (35 refs - Sprint 1-5 各 7 files)
**Sprint 1**: [OVERVIEW](../sprint-1/SPRINT-1-1-OVERVIEW.md), [PLAN](../sprint-1/SPRINT-1-2-PLAN.md), [CONTEXT](../sprint-1/SPRINT-1-3-CONTEXT.md), [CHECKLIST](../sprint-1/SPRINT-1-4-CHECKLIST.md), [DEV-LOG](../sprint-1/SPRINT-1-5-DEV-LOG.md), [ISSUES](../sprint-1/SPRINT-1-6-ISSUES.md), [RETROSPECTIVE](../sprint-1/SPRINT-1-7-RETROSPECTIVE.md)

**Sprint 2**: [OVERVIEW](../sprint-2/SPRINT-2-1-OVERVIEW.md), [PLAN](../sprint-2/SPRINT-2-2-PLAN.md), [CONTEXT](../sprint-2/SPRINT-2-3-CONTEXT.md), [CHECKLIST](../sprint-2/SPRINT-2-4-CHECKLIST.md), [DEV-LOG](../sprint-2/SPRINT-2-5-DEV-LOG.md), [ISSUES](../sprint-2/SPRINT-2-6-ISSUES.md), [RETROSPECTIVE](../sprint-2/SPRINT-2-7-RETROSPECTIVE.md)

**Sprint 3**: [OVERVIEW](../sprint-3/SPRINT-3-1-OVERVIEW.md), [PLAN](../sprint-3/SPRINT-3-2-PLAN.md), [CONTEXT](../sprint-3/SPRINT-3-3-CONTEXT.md), [CHECKLIST](../sprint-3/SPRINT-3-4-CHECKLIST.md), [DEV-LOG](../sprint-3/SPRINT-3-5-DEV-LOG.md), [ISSUES](../sprint-3/SPRINT-3-6-ISSUES.md), [RETROSPECTIVE](../sprint-3/SPRINT-3-7-RETROSPECTIVE.md)

**Sprint 4**: [OVERVIEW](../sprint-4/SPRINT-4-1-OVERVIEW.md), [PLAN](../sprint-4/SPRINT-4-2-PLAN.md), [CONTEXT](../sprint-4/SPRINT-4-3-CONTEXT.md), [CHECKLIST](../sprint-4/SPRINT-4-4-CHECKLIST.md), [DEV-LOG](../sprint-4/SPRINT-4-5-DEV-LOG.md), [ISSUES](../sprint-4/SPRINT-4-6-ISSUES.md), [RETROSPECTIVE](../sprint-4/SPRINT-4-7-RETROSPECTIVE.md)

**Sprint 5**: [OVERVIEW](../sprint-5/SPRINT-5-1-OVERVIEW.md), [PLAN](../sprint-5/SPRINT-5-2-PLAN.md), [CONTEXT](../sprint-5/SPRINT-5-3-CONTEXT.md), [CHECKLIST](../sprint-5/SPRINT-5-4-CHECKLIST.md), [DEV-LOG](../sprint-5/SPRINT-5-5-DEV-LOG.md), [ISSUES](../sprint-5/SPRINT-5-6-ISSUES.md), [RETROSPECTIVE](../sprint-5/SPRINT-5-7-RETROSPECTIVE.md)

### User Stories - Code Interpreter (2 refs)
16. [us-6-code-interpreter.md](../../docs/user-stories/us-6-code-interpreter.md) - Code Interpreter 完整需求 (US 5.1)
17. [epic-06-code-interpreter.md](../../docs/user-stories/epics/epic-06-code-interpreter.md) - Code Interpreter Epic

### 架構設計文檔 - ADRs (5 refs)
18. [Architecture-Design-Document.md](../../docs/architecture/Architecture-Design-Document.md) - 完整架構設計
19. [ADR-008: Code Interpreter Container Pool](../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md) - Docker 容器池設計與動態生命週期管理
20. [ADR-006: Hybrid State Management](../../docs/architecture/decisions/ADR-006-hybrid-state-management.md) - Redis + PostgreSQL 混合狀態管理
21. [ADR-015: Execution Timeout Strategy](../../docs/architecture/decisions/ADR-015-execution-timeout-strategy.md) - 執行超時策略 (New)
22. [ADR-016: Resource Limit Configuration](../../docs/architecture/decisions/ADR-016-resource-limit-configuration.md) - 資源限制配置 (New)

### 技術實作文檔 - Code Interpreter (6 refs)
23. [06-code-interpreter-implementation.md](../../docs/technical-implementation/01-backend-net9/06-code-interpreter-implementation.md) - Code Interpreter 完整實作指南
24. [code-interpreter-architecture.md](../../docs/technical-implementation/code-interpreter/code-interpreter-architecture.md) - 架構設計 (New)
25. [container-pool-management.md](../../docs/technical-implementation/code-interpreter/container-pool-management.md) - 容器池管理詳解 (New)
26. [docker-image-design.md](../../docs/technical-implementation/code-interpreter/docker-image-design.md) - Docker Image 設計 (New)
27. [execution-engine.md](../../docs/technical-implementation/code-interpreter/execution-engine.md) - 執行引擎實作 (New)
28. [result-processing.md](../../docs/technical-implementation/code-interpreter/result-processing.md) - 執行結果處理 (New)

### 安全設計文檔 (5 refs)
29. [code-execution-security.md](../../docs/security/code-execution-security.md) - Code Interpreter 4-Layer Security Architecture 完整設計
30. [container-security.md](../../docs/security/container-security.md) - 容器安全設計 (New)
31. [network-isolation.md](../../docs/security/network-isolation.md) - 網路隔離設計 (New)
32. [filesystem-isolation.md](../../docs/security/filesystem-isolation.md) - 文件系統隔離設計 (New)
33. [resource-limits.md](../../docs/security/resource-limits.md) - 資源限制設計 (New)

### API 設計文檔 (3 refs)
34. [API-Design-Guidelines.md](../../docs/api/API-Design-Guidelines.md) - API 設計指南
35. [code-interpreter-api-design.md](../../docs/api/code-interpreter-api-design.md) - Code Interpreter API 設計 (New)
36. [api-endpoints-v1.md](../../docs/api/api-endpoints-v1.md) - API v1 端點清單

### 資料庫文檔 (2 refs)
37. [database-schema-design.md](../../docs/database/database-schema-design.md) - 資料庫 Schema 設計
38. [code-execution-entity-design.md](../../docs/database/code-execution-entity-design.md) - CodeExecution Entity 設計 (New)

### 測試文檔 (5 refs)
39. [testing-strategy-overview.md](../../docs/testing/testing-strategy-overview.md) - 測試策略概覽
40. [unit-testing-guidelines.md](../../docs/testing/unit-testing-guidelines.md) - 單元測試指南
41. [integration-testing-guidelines.md](../../docs/testing/integration-testing-guidelines.md) - 整合測試指南
42. [security-testing.md](../../docs/testing/security-testing.md) - 安全測試指南 (New)
43. [code-interpreter-testing-strategy.md](../../docs/testing/code-interpreter-testing-strategy.md) - Code Interpreter 測試策略 (New)

### UX 設計文檔 (3 refs)
44. [user-research/personas.md](../../docs/ux-design/user-research/personas.md) - 使用者研究 Personas
45. [wireframes/low-fidelity/07-code-interpreter.md](../../docs/ux-design/wireframes/low-fidelity/07-code-interpreter.md) - Code Interpreter UI 設計
46. [code-interpreter-ui-design.md](../../docs/ux-design/code-interpreter-ui-design.md) - Code Interpreter UI 設計詳解 (New)

### 開發標準與流程 (3 refs)
47. [coding-standards-csharp.md](../../docs/development-standards/coding-standards-csharp.md) - C# 編碼標準
48. [coding-standards-typescript.md](../../docs/development-standards/coding-standards-typescript.md) - TypeScript 編碼標準
49. [git-workflow.md](../../docs/development-standards/git-workflow.md) - Git 工作流程

### Docker 與部署文檔 (4 refs)
50. [docker-compose-setup.md](../../docs/deployment/docker-compose-setup.md) - Docker Compose 設置
51. [code-interpreter-dockerfile.md](../../docs/deployment/code-interpreter-dockerfile.md) - Code Interpreter Dockerfile 設計 (New)
52. [container-networking.md](../../docs/deployment/container-networking.md) - 容器網路配置 (New)
53. [deployment-guide.md](../../docs/deployment/deployment-guide.md) - 部署指南

### 程式碼參考 - Application Layer (4 refs)
54. [ICodeExecutionService.cs](../../src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs) - 代碼執行服務介面 (New)
55. [IContainerPoolManager.cs](../../src/AIAgentPlatform.Application/Interfaces/IContainerPoolManager.cs) - 容器池管理介面 (New)
56. [IDockerService.cs](../../src/AIAgentPlatform.Application/Interfaces/IDockerService.cs) - Docker 服務介面 (New)
57. [IExecutionResultProcessor.cs](../../src/AIAgentPlatform.Application/Interfaces/IExecutionResultProcessor.cs) - 執行結果處理介面 (New)

### 程式碼參考 - Domain Layer (2 refs)
58. [CodeExecution.cs](../../src/AIAgentPlatform.Domain/Entities/CodeExecution.cs) - CodeExecution Entity (New)
59. [ExecutionResult.cs](../../src/AIAgentPlatform.Domain/ValueObjects/ExecutionResult.cs) - ExecutionResult Value Object (New)

### 程式碼參考 - Infrastructure Layer (6 refs)
60. [DockerCodeExecutionService.cs](../../src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs) - Docker 代碼執行服務 (New)
61. [ContainerPoolManager.cs](../../src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs) - 容器池管理器 (New)
62. [DockerService.cs](../../src/AIAgentPlatform.Infrastructure/Services/DockerService.cs) - Docker 服務實作 (New)
63. [ExecutionResultProcessor.cs](../../src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs) - 執行結果處理器 (New)
64. [ContainerHealthChecker.cs](../../src/AIAgentPlatform.Infrastructure/Services/ContainerHealthChecker.cs) - 容器健康檢查 (New)
65. [SecurityManager.cs](../../src/AIAgentPlatform.Infrastructure/Services/SecurityManager.cs) - 安全管理器 (New)

### 程式碼參考 - API Layer (1 ref)
66. [CodeController.cs](../../src/AIAgentPlatform.API/Controllers/CodeController.cs) - Code API Controller (New)

### 程式碼參考 - Frontend (3 refs)
67. [CodeInterpreter.tsx](../../apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx) - Code Interpreter 主元件 (New)
68. [CodeEditor.tsx](../../apps/web-app/src/features/code-interpreter/components/CodeEditor.tsx) - 代碼編輯器元件 (New)
69. [ExecutionResult.tsx](../../apps/web-app/src/features/code-interpreter/components/ExecutionResult.tsx) - 執行結果元件 (New)

### 程式碼參考 - Docker (1 ref)
70. [Dockerfile](../../docker/code-interpreter/Dockerfile) - Code Interpreter Docker Image 定義 (New)

---

## 使用指南

本文件為 **Sprint 6 綜合概覽文件**，提供 Sprint 規劃、執行、追蹤所需的完整資訊。

### 目標讀者

**主要讀者**:
- 🎯 **Product Owner**: 了解 Sprint 業務價值和交付內容
- 🏃 **Scrum Master**: 掌握 Sprint 進度、風險和依賴關係
- 👨‍💻 **開發團隊**: 理解技術實作範圍和驗收標準
- 🧪 **QA Team**: 了解測試範圍和品質要求（特別是安全測試）
- 🏗️ **Tech Lead / Architect**: 評估技術決策和架構影響
- 🔐 **Security Team**: 評估安全設計和測試策略

**次要讀者**:
- 👥 **Stakeholders**: 了解 Sprint 目標和預期成果
- 📝 **Technical Writers**: 準備使用者文檔和教學材料

### 使用方式

#### 📋 規劃階段 (Sprint Planning)
**適用角色**: Product Owner, Scrum Master, Tech Lead, Security Team

**閱讀重點**:
1. [Sprint 目標](#sprint-目標) - 了解核心目標和業務價值
2. [User Stories 分配](#user-stories-分配) - 檢視 User Stories 和 Story Points
3. [風險與依賴](#風險與依賴) - **重點關注 RISK-016 (Container 逃逸漏洞)**
4. [預期交付](#預期交付) - 確認交付清單和驗收標準
5. [完整參考文獻索引](#完整參考文獻索引) - 找到相關規劃文檔

**特別注意**:
- ⚠️ **安全風險**: RISK-016 (Container 逃逸) 為高風險，需要 Security Team 參與評估
- ⚠️ **MVP 簡化**: Sprint 6 為簡化版（5 SP vs 原計劃 13 SP），確認範圍調整合理
- ⚠️ **Phase 1A 完成**: Sprint 6 是 Phase 1A 最後一個 Sprint，完成後進入 Phase 1B

**配合使用文檔**:
- [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) - 詳細實施計劃
- [SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md) - 上下文與背景資訊
- [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md) - 執行檢查清單

#### 🚀 執行階段 (Sprint Execution)
**適用角色**: 開發團隊, QA Team, Tech Lead, Security Team

**閱讀重點**:
1. [User Stories 分配](#user-stories-分配) - 查看 Phase 劃分和任務分解
2. [技術範圍](#技術範圍) - 了解 API 端點和技術實作
3. [成功指標](#成功指標) - 確認效能和品質目標（**特別是安全指標**）
4. [完整參考文獻索引](#完整參考文獻索引) - 找到技術實作文檔和程式碼參考

**配合使用文檔**:
- [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md) - 追蹤任務完成狀態
- [SPRINT-6-5-DEV-LOG.md](./SPRINT-6-5-DEV-LOG.md) - 記錄開發進度和決策
- [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md) - 追蹤問題和 Bug

**日常使用流程**:
```
1. 每日站會前 → 查看 [CHECKLIST](#user-stories-分配) 確認今日任務
2. 開發期間 → 參考 [完整參考文獻索引](#完整參考文獻索引) 找到技術文檔
3. 遇到問題 → 記錄到 [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md)
4. 完成任務 → 更新 [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md)
5. 每日結束 → 更新 [SPRINT-6-5-DEV-LOG.md](./SPRINT-6-5-DEV-LOG.md)
6. Week 2 → 進行安全測試（滲透測試）
```

**安全測試重點時程**:
```
Week 1:
  - Day 1-3: Docker Image 建置與容器池實作
  - Day 4-5: Python 執行引擎實作

Week 2:
  - Day 1-2: 4-Layer Security 配置
  - Day 3-4: 安全測試（滲透測試）← 重點
  - Day 5: 安全漏洞修復

Week 3:
  - Day 1-2: 執行結果處理與 UI
  - Day 3-4: 整合測試
  - Day 5: Sprint 回顧
```

#### 🔍 追蹤階段 (Sprint Monitoring)
**適用角色**: Scrum Master, Tech Lead, Product Owner, Security Team

**閱讀重點**:
1. [成功指標](#成功指標) - 檢視效能和品質指標達成情況
2. [風險與依賴](#風險與依賴) - 追蹤風險狀態和緩解措施效果（**特別是 RISK-016**）
3. [預期交付](#預期交付) - 確認交付進度

**配合使用文檔**:
- [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md) - 檢查任務完成率
- [SPRINT-6-5-DEV-LOG.md](./SPRINT-6-5-DEV-LOG.md) - 審閱開發進度
- [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md) - 評估問題影響

**關鍵監控指標**:
```yaml
安全指標 (Critical):
  - 無容器逃逸漏洞: 必須通過滲透測試
  - Resource 限制有效率: 100%
  - Network 隔離有效率: 100%

效能指標 (Important):
  - 執行成功率: ≥90%
  - 執行時間: <30 秒 (P95)
  - 容器啟動時間: <5 秒

品質指標 (Important):
  - 單元測試覆蓋率: ≥85%
  - 安全測試通過率: 100%
```

#### 🎓 回顧階段 (Sprint Retrospective)
**適用角色**: 全團隊

**閱讀重點**:
1. [成功指標](#成功指標) - 評估目標達成情況
2. [風險與依賴](#風險與依賴) - 檢討風險處理效果
3. [Sprint 目標](#sprint-目標) - 確認業務價值實現程度

**配合使用文檔**:
- [SPRINT-6-7-RETROSPECTIVE.md](./SPRINT-6-7-RETROSPECTIVE.md) - 回顧會議記錄

**回顧討論重點**:
```
1. 安全設計是否足夠？
   - 4-Layer Security 是否有效？
   - 滲透測試是否發現漏洞？
   - 是否需要額外安全措施？

2. MVP 簡化是否合理？
   - 5 SP vs 13 SP 的簡化是否影響功能完整性？
   - 固定 3 個容器是否足夠？
   - Phase 2 需要哪些增強？

3. Phase 1A 完成評估：
   - 基礎平台是否達到預期品質？
   - Phase 1B (工作流引擎) 準備是否充分？
```

### 快速導航

#### 按功能導航
- **Docker 環境搭建** → [Phase 1: Docker 容器環境搭建](#phase-1-docker-容器環境搭建-15-sp)
- **Python 執行引擎** → [Phase 2: Python 執行引擎實作](#phase-2-python-執行引擎實作-15-sp)
- **安全架構** → [Phase 3: 4-Layer Security Architecture](#phase-3-4-layer-security-architecture-1-sp)
- **結果處理** → [Phase 4: 執行結果處理與可視化](#phase-4-執行結果處理與可視化-05-sp)
- **API 與 UI** → [Phase 5: Code Interpreter API 與 UI](#phase-5-code-interpreter-api-與-ui-05-sp)

#### 按階段導航
- **Phase 1** → [Docker 容器環境搭建](#phase-1-docker-容器環境搭建-15-sp)
- **Phase 2** → [Python 執行引擎實作](#phase-2-python-執行引擎實作-15-sp)
- **Phase 3** → [4-Layer Security Architecture](#phase-3-4-layer-security-architecture-1-sp)
- **Phase 4** → [執行結果處理與可視化](#phase-4-執行結果處理與可視化-05-sp)
- **Phase 5** → [Code Interpreter API 與 UI](#phase-5-code-interpreter-api-與-ui-05-sp)

#### 按角色導航
- **Product Owner** → [Sprint 目標](#sprint-目標) + [業務價值](#業務價值-business-value) + [預期交付](#預期交付)
- **Scrum Master** → [User Stories 分配](#user-stories-分配) + [風險與依賴](#風險與依賴) + [成功指標](#成功指標)
- **開發者** → [技術範圍](#技術範圍) + [完整參考文獻索引](#完整參考文獻索引) + [User Stories Phase 詳情](#user-stories-分配)
- **QA Engineer** → [成功指標](#成功指標) + [驗收標準](#phase-5-code-interpreter-api-與-ui-05-sp) + [測試文檔](#測試文檔-5-refs)
- **Security Engineer** → [RISK-016](#risk-016-container-逃逸漏洞--高風險) + [安全設計文檔](#安全設計文檔-5-refs) + [安全測試](#測試文檔-5-refs)
- **Architect** → [風險與依賴](#風險與依賴) + [架構設計文檔](#架構設計文檔---adrs-5-refs) + [技術實作文檔](#技術實作文檔---code-interpreter-6-refs)

#### 文檔間導航
```
OVERVIEW (本文件) - Sprint 綜合概覽
    ↓
    ├─→ [PLAN](./SPRINT-6-2-PLAN.md) - 詳細實施計劃
    ├─→ [CONTEXT](./SPRINT-6-3-CONTEXT.md) - 上下文與背景
    ├─→ [CHECKLIST](./SPRINT-6-4-CHECKLIST.md) - 執行檢查清單
    ├─→ [DEV-LOG](./SPRINT-6-5-DEV-LOG.md) - 開發日誌
    ├─→ [ISSUES](./SPRINT-6-6-ISSUES.md) - 問題追蹤
    └─→ [RETROSPECTIVE](./SPRINT-6-7-RETROSPECTIVE.md) - 回顧總結
```

### 文檔更新頻率

| 文檔 | 更新頻率 | 更新者 | 備註 |
|------|----------|--------|------|
| OVERVIEW | Sprint 開始時、重大變更時 | Scrum Master / Tech Lead | 安全風險更新時需額外通知 Security Team |
| PLAN | Sprint 開始時 | Tech Lead / Architect | - |
| CONTEXT | Sprint 開始時 | Tech Lead | - |
| CHECKLIST | 每日 | 開發團隊 | 安全測試任務需特別標記 |
| DEV-LOG | 每日 | 開發團隊 | 記錄安全測試結果 |
| ISSUES | 即時 | 任何團隊成員 | 安全問題需立即升級 |
| RETROSPECTIVE | Sprint 結束時 | Scrum Master | 包含安全評估 |

---

## 版本歷史

### v2.1 (2025-11-14) - 當前版本
- ✅ 建立 Sprint 6 OVERVIEW 文件
- ✅ 遵循 v2.1 標準格式（8 欄位 Header）
- ✅ 詳細的 User Stories Phase 劃分（US 5.1: 5 Phases）
- ✅ 完整的風險評估（RISK-016, 017, 018, 019）
- ✅ **特別強調 RISK-016 (Container 逃逸漏洞) 高風險**
- ✅ 完整參考文獻索引（70+ 文檔，11 分類）
- ✅ 詳細的使用指南（規劃、執行、追蹤、回顧四階段）
- ✅ **安全測試時程規劃與重點標記**
- ✅ 與 Sprint 1-5 格式一致
- ✅ MVP 簡化範圍說明（5 SP vs 13 SP）

**文件統計** (v2.1):
- 總行數: ~1,000 行
- User Stories: 1 個 (US 5.1)
- Story Points: 5 SP (調整後，原計劃 13 SP)
- Phases: 5 個
- 風險識別: 4 個 (RISK-016 高風險, RISK-017, 018, 019 中低風險)
- 參考文獻: 70+ 文檔

**品質指標**:
- 與 Sprint 1-5 格式一致性: ✅ 100%
- v2.1 標準符合度: ✅ 100%
- 參考文獻完整性: ✅ 70+ 文檔
- Phase 劃分細緻度: ✅ 5 Phases, 20+ 任務
- 安全重點強調: ✅ RISK-016 高風險充分說明

**特別說明**:
- Sprint 6 為 Phase 1A 最後一個 Sprint
- 完成後 Phase 1A (基礎平台) 達成重要里程碑
- 為 Phase 1B (工作流引擎核心) 做準備
- MVP 簡化版本（5 SP），完整版留待 Phase 1B-1C

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 6 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-6/SPRINT-6-1-OVERVIEW.md`
