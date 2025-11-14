# SPRINT-6-3-CONTEXT.md - Sprint 6 開發上下文：Code Interpreter 安全執行環境

**版本**: v2.1
**Sprint 編號**: Sprint 6
**Sprint 週期**: Week 16-18 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2026-01-27 ~ 2026-02-16
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📑 目錄 (Table of Contents)

- [使用說明](#-使用說明-how-to-use-this-document)
- [一、核心技術參考層 (Reference Layer)](#一核心技術參考層-reference-layer)
  - [1.1 Module & Epic 引用](#11-module--epic-引用)
  - [1.2 架構設計文檔](#12-架構設計文檔)
  - [1.3 技術決策記錄 (ADR)](#13-技術決策記錄-adr)
  - [1.4 安全設計引用](#14-安全設計引用)
  - [1.5 API 設計引用](#15-api-設計引用)
- [二、US 5.1: Code Interpreter - Python 執行引擎 - 詳細技術上下文](#二us-51-code-interpreter---python-執行引擎---詳細技術上下文)
  - [2.1 MVP 範圍定義與技術決策](#21-mvp-範圍定義與技術決策)
  - [2.2 Docker 容器池架構設計](#22-docker-容器池架構設計)
  - [2.3 4-Layer Security Architecture 詳解](#23-4-layer-security-architecture-詳解)
  - [2.4 Python 執行引擎設計](#24-python-執行引擎設計)
  - [2.5 執行結果處理與可視化](#25-執行結果處理與可視化)
  - [2.6 資源管理與效能優化](#26-資源管理與效能優化)
- [三、完整文檔引用清單 (按功能分類)](#三完整文檔引用清單-按功能分類)
- [使用指南](#-使用指南)
- [更新日誌](#-更新日誌)

---

## 📖 使用說明 (How to Use This Document)

**本文檔定位**: "Just-in-Time" 技術參考手冊

**適用場景**:
- ✅ 開發 US 5.1 時，需要查詢 Docker 容器池設計 → 第二章 2.2
- ✅ 開發 US 5.1 時，需要查詢安全架構設計 → 第二章 2.3
- ✅ 需要快速找到 4-Layer Security 配置參數 → 2.3 章節
- ✅ 需要理解容器生命週期管理 → 2.2 章節
- ✅ 需要快速找到架構設計文檔 → 第一章 Reference Layer
- ✅ 需要理解技術決策背景 → 各章節的「關鍵技術決策」部分

**不適用場景**:
- ❌ 尋找 Sprint 整體進度與成果 → 使用 [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md)
- ❌ 尋找詳細開發計劃與任務清單 → 使用 [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md)
- ❌ 追蹤開發進度與 Checklist → 使用 [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md)

---

## 一、核心技術參考層 (Reference Layer)

### 1.1 Module & Epic 引用

**User Story 完整規格**:
- 📖 [US 6: Code Interpreter](../../../docs/user-stories/us-6-code-interpreter.md) - Code Interpreter 完整規格
  - **Section**: [US 5.1 - Python 執行引擎](../../../docs/user-stories/us-6-code-interpreter.md#us-51) - Python 執行驗收標準
  - **關鍵內容**: Docker 容器池、4-Layer Security、執行超時控制、資源限制、結果可視化

**Epic 文檔**:
- 📖 [Epic 06: Code Interpreter](../../../docs/user-stories/epics/epic-06-code-interpreter.md) - Code Interpreter 功能完整策略
  - **用途**: 理解 Code Interpreter 在整體系統中的定位
  - **關鍵內容**: BDD 場景、驗收標準、Phase 1A → Phase 2 演進路徑
  - **Phase 1A 範圍**: Docker 容器池、Python 3.11、基礎安全架構（本 Sprint）
  - **Phase 2 延後**: 多語言支援（R, Julia）、Jupyter Notebook 完整環境、進階容器管理

### 1.2 架構設計文檔

**核心架構決策記錄 (ADR)**:

**1. Code Interpreter Container Pool** - [ADR-008](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md)
- **決策**: Docker 容器池 + 動態生命週期管理（MVP 簡化為固定 3 個容器）
- **關鍵原則**: 容器隔離、資源限制、安全沙箱、執行超時
- **Section**: [Container Pool Strategy](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md#container-pool-strategy) - 容器池管理策略
- **Section**: [Container Lifecycle](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md#container-lifecycle) - 容器生命週期設計
- **Section**: [Security Isolation](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md#security-isolation) - 安全隔離機制
- **實施影響**: US 5.1 的容器池設計基於此決策，MVP 簡化為固定 3 個容器（Phase 2 實作動態池）

**2. Hybrid State Management** - [ADR-006](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md)
- **決策**: Redis（執行狀態快取）+ PostgreSQL（執行記錄持久化）
- **關鍵設計**: 執行狀態實時追蹤（Redis），歷史記錄查詢（PostgreSQL）
- **Section**: [Redis Caching Strategy](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md#redis-caching-strategy) - 執行狀態快取
- **Section**: [PostgreSQL Persistence](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md#postgresql-persistence) - 執行記錄持久化
- **實施影響**: US 5.1 的執行狀態管理基於混合儲存策略

**3. Execution Timeout Strategy** - ADR-015 (New)
- **決策**: 30 秒執行超時 + 優雅關閉機制
- **關鍵設計**: CancellationToken 傳遞、Docker 容器強制停止、超時錯誤處理
- **Section**: [Timeout Configuration](../../../docs/architecture/adr/ADR-015-execution-timeout-strategy.md#timeout-configuration) - 超時配置策略
- **Section**: [Graceful Shutdown](../../../docs/architecture/adr/ADR-015-execution-timeout-strategy.md#graceful-shutdown) - 優雅關閉設計
- **實施影響**: US 5.1 的執行超時控制基於此設計

**4. Resource Limit Configuration** - ADR-016 (New)
- **決策**: CPU 1 core、Memory 512MB、Disk 100MB、Process 10 個
- **關鍵設計**: Docker Resource Constraints、cgroups 配置、資源監控
- **Section**: [Resource Limits](../../../docs/architecture/adr/ADR-016-resource-limit-configuration.md#resource-limits) - 資源限制配置
- **Section**: [Monitoring](../../../docs/architecture/adr/ADR-016-resource-limit-configuration.md#monitoring) - 資源使用監控
- **實施影響**: US 5.1 的資源限制基於此配置

**完整架構設計文檔**:
- 🏗️ [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 系統架構總覽
  - **Section**: [Code Interpreter Architecture](../../../docs/architecture/Architecture-Design-Document.md#code-interpreter-architecture) - Code Interpreter 系統架構圖
  - **Section**: [Security Architecture](../../../docs/architecture/Architecture-Design-Document.md#security-architecture) - 4-Layer Security 完整設計
  - **Section**: [Container Management](../../../docs/architecture/Architecture-Design-Document.md#container-management) - 容器管理架構
  - **Section**: [Execution Pipeline](../../../docs/architecture/Architecture-Design-Document.md#execution-pipeline) - 代碼執行流程圖

### 1.3 技術決策記錄 (ADR)

**Code Interpreter 相關 ADR 完整清單**:

| ADR 編號 | 標題 | 關鍵決策 | 實施影響 |
|---------|------|---------|---------|
| [ADR-008](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md) | Container Pool Strategy | Docker 容器池 + 固定 3 個容器（MVP） | US 5.1 容器池設計 |
| [ADR-006](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md) | Hybrid State Management | Redis + PostgreSQL 混合儲存 | US 5.1 執行狀態管理 |
| [ADR-015](../../../docs/architecture/adr/ADR-015-execution-timeout-strategy.md) | Execution Timeout Strategy | 30 秒超時 + 優雅關閉 | US 5.1 超時控制 |
| [ADR-016](../../../docs/architecture/adr/ADR-016-resource-limit-configuration.md) | Resource Limit Configuration | CPU 1 core, Memory 512MB | US 5.1 資源限制 |
| [ADR-001](../../../docs/architecture/adr/ADR-001-clean-architecture.md) | Clean Architecture | Domain → Application → Infrastructure → API | 整體分層架構 |
| [ADR-002](../../../docs/architecture/adr/ADR-002-cqrs-pattern.md) | CQRS Pattern | MediatR Commands/Queries 分離 | US 5.1 業務邏輯 |
| [ADR-003](../../../docs/architecture/adr/ADR-003-repository-pattern.md) | Repository Pattern | ICodeExecutionRepository 抽象 | US 5.1 資料存取 |

### 1.4 安全設計引用

**Code Interpreter 安全設計**:
- 🔐 [Code Execution Security](../../../docs/security/code-execution-security.md) - Code Interpreter 4-Layer Security Architecture 完整設計
  - **Section**: [Layer 1 - Container Isolation](../../../docs/security/code-execution-security.md#layer-1-container-isolation) - 容器隔離詳細設計
    - Docker 容器隔離
    - 非 root 用戶執行（UID 1000）
    - Read-only 文件系統（除 /tmp）
    - No privileged mode
    - Drop capabilities（除必要的 CAP_NET_BIND_SERVICE）

  - **Section**: [Layer 2 - Network Isolation](../../../docs/security/code-execution-security.md#layer-2-network-isolation) - 網路隔離設計
    - 禁止外部網路存取
    - 僅允許 localhost 存取
    - Docker Network: none 模式
    - 防火牆規則配置（iptables）

  - **Section**: [Layer 3 - File System Isolation](../../../docs/security/code-execution-security.md#layer-3-filesystem-isolation) - 文件系統隔離
    - /tmp 目錄僅可寫（100MB 配額）
    - 禁止存取敏感目錄（/etc, /proc, /sys）
    - tmpfs 掛載（不持久化到磁碟）
    - File System 配額限制

  - **Section**: [Layer 4 - Resource Limits](../../../docs/security/code-execution-security.md#layer-4-resource-limits) - 資源限制
    - CPU 限制：1 core
    - Memory 限制：512MB
    - Process 限制：10 個 process
    - 執行時間限制：30 秒
    - Disk I/O 限制

  - **Section**: [Security Testing](../../../docs/security/code-execution-security.md#security-testing) - 安全測試策略
    - 滲透測試（Week 2）
    - 容器逃逸測試
    - Network 隔離測試
    - File System 隔離測試
    - Resource 限制測試

**Container Security**:
- 🔐 [Container Security](../../../docs/security/container-security.md) - 容器安全最佳實踐 (New)
  - **Section**: [Docker Image Security](../../../docs/security/container-security.md#docker-image-security) - Docker Image 安全掃描
  - **Section**: [Runtime Security](../../../docs/security/container-security.md#runtime-security) - 運行時安全監控
  - **Section**: [Vulnerability Management](../../../docs/security/container-security.md#vulnerability-management) - 漏洞管理流程

**Network Isolation**:
- 🔐 [Network Isolation](../../../docs/security/network-isolation.md) - 網路隔離設計 (New)
  - **Section**: [Docker Network Configuration](../../../docs/security/network-isolation.md#docker-network-configuration) - Docker 網路配置
  - **Section**: [Firewall Rules](../../../docs/security/network-isolation.md#firewall-rules) - 防火牆規則設定

**File System Isolation**:
- 🔐 [File System Isolation](../../../docs/security/filesystem-isolation.md) - 文件系統隔離設計 (New)
  - **Section**: [Read-only Root FS](../../../docs/security/filesystem-isolation.md#read-only-root-fs) - 唯讀根文件系統
  - **Section**: [tmpfs Configuration](../../../docs/security/filesystem-isolation.md#tmpfs-configuration) - tmpfs 配置
  - **Section**: [Quota Management](../../../docs/security/filesystem-isolation.md#quota-management) - 配額管理

**Resource Limits**:
- 🔐 [Resource Limits](../../../docs/security/resource-limits.md) - 資源限制設計 (New)
  - **Section**: [CPU Limits](../../../docs/security/resource-limits.md#cpu-limits) - CPU 限制配置
  - **Section**: [Memory Limits](../../../docs/security/resource-limits.md#memory-limits) - Memory 限制配置
  - **Section**: [Process Limits](../../../docs/security/resource-limits.md#process-limits) - Process 限制配置

### 1.5 API 設計引用

**Code Interpreter API 設計規範**:
- 📄 [Code Interpreter API Design](../../../docs/api/code-interpreter-api-design.md) - Code Interpreter RESTful API 完整設計 (New)
  - **Section**: [Execute Code Endpoint](../../../docs/api/code-interpreter-api-design.md#execute-code-endpoint) - 執行代碼 API 詳細設計
    - `POST /api/v1/code/execute` - 執行 Python 代碼（US 5.1）
    - Request: code, language, timeout, resourceLimits
    - Response: executionId, status, output, error, executionTime

  - **Section**: [Get Execution Result Endpoint](../../../docs/api/code-interpreter-api-design.md#get-execution-result-endpoint) - 獲取執行結果 API
    - `GET /api/v1/code/executions/{id}` - 獲取執行結果與狀態（US 5.1）
    - Response: executionId, status, output, error, charts, tables

  - **Section**: [Delete Execution Endpoint](../../../docs/api/code-interpreter-api-design.md#delete-execution-endpoint) - 刪除執行記錄 API
    - `DELETE /api/v1/code/executions/{id}` - 刪除執行記錄（軟刪除）（US 5.1）

  - **Section**: [Get Container Status Endpoint](../../../docs/api/code-interpreter-api-design.md#get-container-status-endpoint) - 容器池狀態 API
    - `GET /api/v1/code/status` - 容器池狀態（US 5.1）
    - Response: totalContainers, availableContainers, busyContainers, healthStatus

  - **Section**: [Request/Response Format](../../../docs/api/code-interpreter-api-design.md#request-response-format) - 標準格式定義
  - **Section**: [Error Handling](../../../docs/api/code-interpreter-api-design.md#error-handling) - 統一錯誤處理策略
  - **Section**: [Validation Rules](../../../docs/api/code-interpreter-api-design.md#validation-rules) - API 驗證規則

**API 請求/回應範例**:

**1. 執行代碼 API**:
```http
POST /api/v1/code/execute
Content-Type: application/json
Authorization: Bearer {token}

Request Body:
{
  "code": "import pandas as pd\ndf = pd.DataFrame({'A': [1, 2, 3]})\nprint(df)",
  "language": "python",
  "timeout": 30,
  "resourceLimits": {
    "cpuCores": 1,
    "memoryMB": 512,
    "diskMB": 100
  }
}

Response (202 Accepted):
{
  "executionId": "uuid",
  "status": "running",
  "submittedAt": "2026-01-27T10:00:00Z",
  "estimatedTime": "5s"
}
```

**2. 獲取執行結果 API**:
```http
GET /api/v1/code/executions/{executionId}
Authorization: Bearer {token}

Response (200 OK):
{
  "executionId": "uuid",
  "status": "completed",
  "output": {
    "stdout": "   A\n0  1\n1  2\n2  3",
    "stderr": "",
    "exitCode": 0
  },
  "executionTime": "2.5s",
  "charts": [],
  "tables": [
    {
      "name": "df",
      "data": [{"A": 1}, {"A": 2}, {"A": 3}]
    }
  ],
  "completedAt": "2026-01-27T10:00:02Z"
}
```

**3. 容器池狀態 API**:
```http
GET /api/v1/code/status
Authorization: Bearer {token}

Response (200 OK):
{
  "totalContainers": 3,
  "availableContainers": 2,
  "busyContainers": 1,
  "healthStatus": "healthy",
  "containers": [
    {
      "containerId": "container_1",
      "status": "available",
      "lastUsed": "2026-01-27T09:55:00Z"
    },
    {
      "containerId": "container_2",
      "status": "busy",
      "currentExecutionId": "uuid"
    },
    {
      "containerId": "container_3",
      "status": "available",
      "lastUsed": "2026-01-27T09:50:00Z"
    }
  ]
}
```

---

## 二、US 5.1: Code Interpreter - Python 執行引擎 - 詳細技術上下文

### 2.1 MVP 範圍定義與技術決策

**完整 User Story 規格**: [US 6: Code Interpreter](../../../docs/user-stories/us-6-code-interpreter.md)

**MVP 範圍邊界**:

✅ **本 Sprint 必須實現 (P0)**:
1. **Docker 容器池管理** - 固定 3 個容器，簡化生命週期管理
   - 容器預創建（啟動時建立 3 個容器）
   - 容器健康檢查（每 10 秒 Ping）
   - 容器重啟機制（異常時自動重啟）
   - **參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md)

2. **Python 3.11 執行引擎** - 基於 Docker API 的代碼執行
   - Docker.DotNet 整合
   - 代碼提交到容器執行
   - 執行結果擷取（stdout, stderr, exit code）
   - 執行超時控制（30 秒）
   - **參考**: [Execution Engine](../../../docs/technical-implementation/code-interpreter/execution-engine.md)

3. **4-Layer Security Architecture** - 完整安全防護
   - Layer 1: Container 隔離（非 root 用戶、read-only filesystem）
   - Layer 2: Network 隔離（禁止外部網路）
   - Layer 3: File System 隔離（僅 /tmp 可寫）
   - Layer 4: Resource 限制（CPU, Memory, Process）
   - **參考**: [Code Execution Security](../../../docs/security/code-execution-security.md)

4. **執行結果處理** - 支援 Text、Image、Table 輸出
   - stdout / stderr 解析
   - Matplotlib 圖表儲存為 PNG
   - Pandas DataFrame 轉 JSON
   - Base64 編碼傳輸
   - **參考**: [Result Processing](../../../docs/technical-implementation/code-interpreter/result-processing.md)

5. **Code Interpreter UI** - 基礎代碼編輯與執行
   - 代碼編輯器（Monaco Editor 或 CodeMirror）
   - 執行按鈕與控制
   - 執行結果展示（Text、Image、Table）
   - 執行歷史記錄
   - **參考**: [Code Interpreter UI Design](../../../docs/ux-design/code-interpreter-ui-design.md)

❌ **明確排除 Phase 2 功能 (延後)**:
- 動態容器池（自動擴縮容、負載均衡） - Phase 2
- 多語言支援（R, Julia, JavaScript） - Phase 2
- Jupyter Notebook 完整環境 - Phase 2
- 進階安全策略（AppArmor, Seccomp, Capabilities） - Phase 2
- 異步執行和佇列管理 - Phase 2
- 高級可視化（Plotly, Bokeh, Altair） - Phase 2

**關鍵技術決策**:

**決策 1: 容器池管理策略**
- **選擇**: 固定 3 個容器（MVP）vs 動態容器池（Phase 2）
- **理由**: MVP 聚焦核心功能驗證，固定容器池降低複雜度，確保 3 週內完成
- **替代方案**: 動態容器池（Phase 2 實作），支援自動擴縮容和負載均衡
- **參考**: [ADR-008: Container Pool Strategy](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md)
- **MVP 限制**: 最多 3 個並發執行，超過則進入等待佇列
- **Phase 2 升級**: 動態容器池、負載均衡、自動擴縮容（根據系統負載）

**決策 2: Python 版本選擇**
- **選擇**: Python 3.11（最新穩定版）
- **理由**: 效能提升（比 3.10 快 10-60%），更好的錯誤訊息，穩定性高
- **替代方案**: Python 3.10（較穩定但效能較差），Python 3.12（太新，套件支援不完整）
- **參考**: [Docker Image Design](../../../docs/technical-implementation/code-interpreter/docker-image-design.md)
- **套件支援**: numpy, pandas, matplotlib, seaborn（預安裝）
- **Phase 2 擴展**: scikit-learn, tensorflow, pytorch（根據需求動態安裝）

**決策 3: Docker Base Image 選擇**
- **選擇**: python:3.11-slim-bookworm
- **理由**: 官方 Image、體積小（<500MB）、安全更新及時、Debian 12 穩定基礎
- **替代方案**: python:3.11-alpine（體積更小但套件安裝複雜），python:3.11（體積過大 >1GB）
- **參考**: [Docker Image Design](../../../docs/technical-implementation/code-interpreter/docker-image-design.md#base-image-selection)
- **Image 優化**: Multi-stage build、移除不必要套件、Layer 快取優化

**決策 4: 安全隔離策略**
- **選擇**: 4-Layer Security Architecture（MVP）
- **理由**: 多層防護確保容器逃逸風險降至最低，符合企業安全標準
- **替代方案**: 更嚴格的安全策略（gVisor, Kata Containers）- Phase 2 評估
- **參考**: [Code Execution Security](../../../docs/security/code-execution-security.md)
- **測試策略**: Week 2 進行滲透測試，驗證安全有效性
- **Phase 2 增強**: AppArmor、Seccomp、Capabilities 精細控制

**決策 5: 執行超時策略**
- **選擇**: 30 秒硬超時（MVP）
- **理由**: 平衡使用者體驗與系統安全，30 秒足夠執行大部分數據分析任務
- **替代方案**: 可配置超時（Phase 2），根據使用者角色和任務類型調整
- **參考**: [ADR-015: Execution Timeout Strategy](../../../docs/architecture/adr/ADR-015-execution-timeout-strategy.md)
- **優雅關閉**: CancellationToken 傳遞、Docker 容器 SIGTERM → SIGKILL
- **Phase 2 升級**: 可配置超時（10s - 300s）、超時預警機制

**決策 6: 資源限制配置**
- **選擇**: CPU 1 core, Memory 512MB, Disk 100MB, Process 10 個
- **理由**: 足夠執行常見數據分析任務，防止資源濫用和系統過載
- **替代方案**: 可配置資源限制（Phase 2），根據使用者等級和任務類型調整
- **參考**: [ADR-016: Resource Limit Configuration](../../../docs/architecture/adr/ADR-016-resource-limit-configuration.md)
- **監控**: 實時監控資源使用，超過閾值時告警
- **Phase 2 升級**: 可配置資源限制、資源配額管理、使用者等級差異化

### 2.2 Docker 容器池架構設計

**完整設計文檔**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md)

**IContainerPoolManager 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IContainerPoolManager.cs`
- **設計模式**: Object Pool Pattern（容器池模式）
- **生命週期管理**: 啟動、停止、健康檢查、自動重啟

**核心方法簽名**:
```csharp
public interface IContainerPoolManager
{
    Task InitializeAsync(CancellationToken cancellationToken = default);
    Task<IContainer> AcquireContainerAsync(CancellationToken cancellationToken = default);
    Task ReleaseContainerAsync(string containerId, CancellationToken cancellationToken = default);
    Task<ContainerPoolStatus> GetPoolStatusAsync(CancellationToken cancellationToken = default);
    Task ShutdownAsync(CancellationToken cancellationToken = default);
}

public sealed class ContainerPoolStatus
{
    public required int TotalContainers { get; init; }
    public required int AvailableContainers { get; init; }
    public required int BusyContainers { get; init; }
    public required string HealthStatus { get; init; } // "healthy", "degraded", "unhealthy"
    public required IReadOnlyList<ContainerInfo> Containers { get; init; }
}

public sealed class ContainerInfo
{
    public required string ContainerId { get; init; }
    public required string Status { get; init; } // "available", "busy", "unhealthy"
    public DateTime? LastUsed { get; init; }
    public string? CurrentExecutionId { get; init; }
}
```

**容器池管理策略**:

**1. 容器預創建（Startup Initialization）**:
- **時機**: 應用啟動時（IHostedService）
- **數量**: 3 個容器（MVP 固定數量）
- **流程**:
  1. 建立 Docker Network（none 模式，網路隔離）
  2. 建立 3 個 Docker Container（基於預建 Image）
  3. 啟動容器並進入待命狀態
  4. 執行健康檢查（驗證容器可用性）
  5. 將容器標記為 "available"
- **參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md#startup-initialization)

**2. 容器分配（Container Acquisition）**:
- **演算法**: FIFO（First-In-First-Out）
- **流程**:
  1. 檢查可用容器池（Status = "available"）
  2. 選擇最早釋放的容器（LastUsed 最早）
  3. 更新容器狀態為 "busy"
  4. 記錄 CurrentExecutionId
  5. 返回容器實例
- **等待機制**: 如無可用容器，進入等待佇列（最多等待 60 秒）
- **參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md#container-acquisition)

**3. 容器釋放（Container Release）**:
- **流程**:
  1. 清理容器內部狀態（刪除 /tmp 檔案）
  2. 重置容器環境
  3. 更新容器狀態為 "available"
  4. 更新 LastUsed 時間戳
  5. 清除 CurrentExecutionId
- **錯誤處理**: 如容器異常，標記為 "unhealthy" 並觸發重啟
- **參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md#container-release)

**4. 容器健康檢查（Health Check）**:
- **頻率**: 每 10 秒
- **檢查項目**:
  1. 容器是否運行（Docker API: InspectContainer）
  2. 容器內部進程是否正常（exec: echo test）
  3. 容器資源使用是否正常（<90% CPU, <90% Memory）
- **異常處理**:
  - 健康檢查失敗 → 標記為 "unhealthy"
  - 連續 3 次失敗 → 自動重啟容器
  - 重啟失敗 → 記錄錯誤並移除容器，建立新容器
- **參考**: [Container Health Checker](../../../src/AIAgentPlatform.Infrastructure/Services/ContainerHealthChecker.cs)

**5. 容器重啟機制（Container Restart）**:
- **觸發條件**:
  - 健康檢查失敗（連續 3 次）
  - 執行異常崩潰
  - 資源使用異常（Memory OOM）
- **重啟流程**:
  1. 停止異常容器（Docker Stop → 10 秒 → Docker Kill）
  2. 刪除容器（Docker Rm）
  3. 建立新容器（相同配置）
  4. 啟動新容器
  5. 執行健康檢查
  6. 標記為 "available"
- **重試策略**: 最多重試 3 次，失敗後記錄錯誤並告警
- **參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md#container-restart)

**ContainerPoolManager 實作細節**:
- **位置**: `AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs`
- **依賴注入**:
  - IDockerService（Docker API 抽象）
  - IContainerHealthChecker（健康檢查服務）
  - ILogger<ContainerPoolManager>（日誌記錄）
  - IOptions<ContainerPoolOptions>（配置選項）

**ContainerPoolOptions 配置**:
```csharp
public sealed class ContainerPoolOptions
{
    public int PoolSize { get; init; } = 3; // 固定 3 個容器（MVP）
    public string ImageName { get; init; } = "code-interpreter-python:3.11"; // Docker Image
    public int HealthCheckIntervalSeconds { get; init; } = 10; // 健康檢查間隔
    public int MaxRestartAttempts { get; init; } = 3; // 最大重試次數
    public int AcquireTimeoutSeconds { get; init; } = 60; // 容器獲取超時
    public ContainerResourceLimits ResourceLimits { get; init; } = new(); // 資源限制
}

public sealed class ContainerResourceLimits
{
    public long CpuCores { get; init; } = 1; // CPU 核心數
    public long MemoryMB { get; init; } = 512; // Memory 限制 (MB)
    public long DiskMB { get; init; } = 100; // Disk 限制 (MB)
    public int MaxProcesses { get; init; } = 10; // Process 限制
}
```

**容器池效能指標**:
- **容器啟動時間**: <5 秒（目標）
- **容器分配時間**: <100ms（目標）
- **容器釋放時間**: <500ms（目標）
- **健康檢查時間**: <1 秒（目標）
- **容器池可用性**: ≥95%（目標）

**參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md#performance-metrics)

### 2.3 4-Layer Security Architecture 詳解

**完整設計文檔**: [Code Execution Security](../../../docs/security/code-execution-security.md)

**安全架構概覽**:
```
Layer 4: Resource Limits (CPU, Memory, Process, Time)
    ↓
Layer 3: File System Isolation (Read-only FS, tmpfs)
    ↓
Layer 2: Network Isolation (No external network)
    ↓
Layer 1: Container Isolation (Docker, Non-root user)
    ↓
Code Execution
```

**Layer 1: Container Isolation（容器隔離）**:
- **Docker 容器隔離**:
  - 獨立的命名空間（Namespace）: PID, Network, Mount, UTS, IPC
  - cgroups 資源控制
  - Linux Capabilities 限制
  - 參考: [Container Security](../../../docs/security/container-security.md#docker-isolation)

- **非 root 用戶執行**:
  ```dockerfile
  # Dockerfile
  RUN useradd -m -u 1000 -s /bin/bash coderunner
  USER coderunner
  WORKDIR /home/coderunner
  ```
  - 用戶: coderunner (UID 1000)
  - 無 sudo 權限
  - 無法修改系統檔案
  - 參考: [Container Security](../../../docs/security/container-security.md#non-root-user)

- **Read-only 文件系統**:
  ```csharp
  // ContainerCreateParameters
  HostConfig = new HostConfig
  {
      ReadonlyRootfs = true, // 根文件系統唯讀
      Tmpfs = new Dictionary<string, string>
      {
          { "/tmp", "rw,noexec,nosuid,size=100m" } // /tmp 可寫，100MB 限制
      }
  }
  ```
  - 根文件系統唯讀（除 /tmp）
  - /tmp 使用 tmpfs（記憶體檔案系統，不持久化）
  - 100MB 配額限制
  - 參考: [File System Isolation](../../../docs/security/filesystem-isolation.md#read-only-fs)

- **禁止 Privileged Mode**:
  ```csharp
  HostConfig = new HostConfig
  {
      Privileged = false, // 禁止 Privileged Mode
      CapDrop = new[] { "ALL" }, // Drop 所有 Capabilities
      CapAdd = new[] { "CAP_NET_BIND_SERVICE" } // 僅保留必要的 Capability
  }
  ```
  - 禁止 Privileged Mode（防止容器逃逸）
  - Drop 所有 Linux Capabilities
  - 僅保留必要的 Capability（CAP_NET_BIND_SERVICE）
  - 參考: [Container Security](../../../docs/security/container-security.md#capabilities)

**Layer 2: Network Isolation（網路隔離）**:
- **Docker Network: none 模式**:
  ```csharp
  // ContainerCreateParameters
  HostConfig = new HostConfig
  {
      NetworkMode = "none" // 禁止網路存取
  }
  ```
  - 完全禁止網路存取
  - 無法連線外部網路
  - 無法存取其他容器
  - 僅支援 localhost（容器內部）
  - 參考: [Network Isolation](../../../docs/security/network-isolation.md#docker-network)

- **防火牆規則（iptables）**:
  ```bash
  # 禁止容器存取外部網路
  iptables -A DOCKER-USER -i docker0 -o eth0 -j DROP
  iptables -A DOCKER-USER -i docker0 -o wlan0 -j DROP
  ```
  - 禁止容器透過 docker0 存取外部網路
  - 僅允許容器與宿主機通訊（必要時）
  - 參考: [Network Isolation](../../../docs/security/network-isolation.md#firewall-rules)

**Layer 3: File System Isolation（文件系統隔離）**:
- **/tmp 目錄配置**:
  ```csharp
  Tmpfs = new Dictionary<string, string>
  {
      { "/tmp", "rw,noexec,nosuid,size=100m" }
  }
  ```
  - rw: 可讀可寫
  - noexec: 禁止執行二進制檔案
  - nosuid: 禁止 setuid/setgid
  - size=100m: 100MB 配額限制
  - 參考: [File System Isolation](../../../docs/security/filesystem-isolation.md#tmpfs-config)

- **禁止存取敏感目錄**:
  - /etc（系統配置）
  - /proc（進程資訊）
  - /sys（系統資訊）
  - /dev（設備檔案）
  - /root（root 用戶主目錄）
  - 參考: [File System Isolation](../../../docs/security/filesystem-isolation.md#sensitive-directories)

- **File System 配額管理**:
  - /tmp: 100MB 限制
  - 超過配額時自動清理舊檔案
  - 執行完畢後清理所有 /tmp 檔案
  - 參考: [File System Isolation](../../../docs/security/filesystem-isolation.md#quota-management)

**Layer 4: Resource Limits（資源限制）**:
- **CPU 限制**:
  ```csharp
  HostConfig = new HostConfig
  {
      NanoCPUs = 1_000_000_000, // 1 core (1e9 nanoseconds)
      CPUQuota = 100_000, // 100ms per 100ms period
      CPUPeriod = 100_000 // 100ms period
  }
  ```
  - CPU 核心數: 1 core
  - CPU 配額: 100% (100ms per 100ms period)
  - 參考: [Resource Limits](../../../docs/security/resource-limits.md#cpu-limits)

- **Memory 限制**:
  ```csharp
  HostConfig = new HostConfig
  {
      Memory = 512 * 1024 * 1024, // 512MB
      MemorySwap = 512 * 1024 * 1024, // 禁止 Swap
      MemoryReservation = 256 * 1024 * 1024, // 預留 256MB
      KernelMemory = 50 * 1024 * 1024 // Kernel Memory 50MB
  }
  ```
  - Memory 限制: 512MB
  - Memory Swap: 512MB（等於 Memory，禁止使用 Swap）
  - Memory 預留: 256MB（保證最低可用記憶體）
  - Kernel Memory: 50MB（核心記憶體限制）
  - 參考: [Resource Limits](../../../docs/security/resource-limits.md#memory-limits)

- **Process 限制**:
  ```csharp
  HostConfig = new HostConfig
  {
      PidsLimit = 10 // 最多 10 個 process
  }
  ```
  - 最多 10 個 process（防止 fork bomb）
  - 超過限制時無法創建新 process
  - 參考: [Resource Limits](../../../docs/security/resource-limits.md#process-limits)

- **執行時間限制**:
  ```csharp
  // 執行超時控制
  using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
  await ExecuteCodeAsync(code, cts.Token);
  ```
  - 執行超時: 30 秒（硬限制）
  - 超時後強制停止容器（SIGTERM → SIGKILL）
  - CancellationToken 傳遞到所有異步操作
  - 參考: [ADR-015: Execution Timeout Strategy](../../../docs/architecture/adr/ADR-015-execution-timeout-strategy.md)

**安全測試策略**:
- **滲透測試（Week 2）**:
  - 容器逃逸測試（Docker Breakout）
  - Network 隔離測試（外部連線測試）
  - File System 隔離測試（敏感目錄存取）
  - Resource 限制測試（CPU、Memory、Process 耗盡）
  - 參考: [Security Testing](../../../docs/testing/security-testing.md)

- **測試工具**:
  - Docker Bench Security（容器安全掃描）
  - Trivy（Docker Image 漏洞掃描）
  - Clair（容器漏洞掃描）
  - 自訂滲透測試腳本
  - 參考: [Security Testing](../../../docs/testing/security-testing.md#testing-tools)

**安全指標**:
- **無容器逃逸漏洞**: 100%（滲透測試通過）
- **Resource 限制有效率**: 100%
- **Network 隔離有效率**: 100%
- **File System 隔離有效率**: 100%

### 2.4 Python 執行引擎設計

**完整設計文檔**: [Execution Engine](../../../docs/technical-implementation/code-interpreter/execution-engine.md)

**ICodeExecutionService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs`
- **核心方法**: `ExecuteCodeAsync(string code, string language, ExecutionOptions options, CancellationToken cancellationToken)`

**ExecutionOptions 配置**:
```csharp
public sealed class ExecutionOptions
{
    public int TimeoutSeconds { get; init; } = 30; // 執行超時（秒）
    public ContainerResourceLimits ResourceLimits { get; init; } = new(); // 資源限制
    public bool CaptureOutput { get; init; } = true; // 捕獲輸出
    public bool CaptureCharts { get; init; } = true; // 捕獲圖表
    public bool CaptureTables { get; init; } = true; // 捕獲表格
}

public sealed class ExecutionResult
{
    public required Guid ExecutionId { get; init; }
    public required string Status { get; init; } // "completed", "failed", "timeout"
    public required ExecutionOutput Output { get; init; }
    public TimeSpan ExecutionTime { get; init; }
    public DateTime CompletedAt { get; init; }
}

public sealed class ExecutionOutput
{
    public required string Stdout { get; init; }
    public required string Stderr { get; init; }
    public required int ExitCode { get; init; }
    public IReadOnlyList<ChartOutput>? Charts { get; init; }
    public IReadOnlyList<TableOutput>? Tables { get; init; }
}
```

**執行流程**:
```
1. 獲取容器 → ContainerPoolManager.AcquireContainerAsync()
2. 準備代碼 → 寫入容器內部檔案（/tmp/code.py）
3. 執行代碼 → Docker Exec: python /tmp/code.py
4. 擷取輸出 → stdout, stderr, exit code
5. 處理結果 → 解析圖表、表格
6. 釋放容器 → ContainerPoolManager.ReleaseContainerAsync()
```

**DockerCodeExecutionService 實作**:
- **位置**: `AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs`
- **依賴**:
  - IContainerPoolManager（容器池管理）
  - IDockerService（Docker API 抽象）
  - IExecutionResultProcessor（結果處理）
  - ICodeExecutionRepository（執行記錄持久化）
  - ILogger<DockerCodeExecutionService>（日誌記錄）

**核心實作邏輯**:
```csharp
public async Task<ExecutionResult> ExecuteCodeAsync(
    string code,
    string language,
    ExecutionOptions options,
    CancellationToken cancellationToken = default)
{
    var executionId = Guid.NewGuid();
    var startTime = DateTime.UtcNow;

    try
    {
        // 1. 獲取容器
        var container = await _containerPoolManager.AcquireContainerAsync(cancellationToken);

        try
        {
            // 2. 寫入代碼到容器
            await _dockerService.WriteFileToContainerAsync(
                container.Id,
                "/tmp/code.py",
                code,
                cancellationToken);

            // 3. 執行代碼（30 秒超時）
            using var cts = CancellationTokenSource.CreateLinkedTokenSource(
                cancellationToken,
                new CancellationTokenSource(TimeSpan.FromSeconds(options.TimeoutSeconds)).Token);

            var execResult = await _dockerService.ExecAsync(
                container.Id,
                new[] { "python", "/tmp/code.py" },
                cts.Token);

            // 4. 擷取輸出
            var output = new ExecutionOutput
            {
                Stdout = execResult.Stdout,
                Stderr = execResult.Stderr,
                ExitCode = execResult.ExitCode
            };

            // 5. 處理結果（圖表、表格）
            if (options.CaptureCharts || options.CaptureTables)
            {
                var processedOutput = await _executionResultProcessor.ProcessAsync(
                    container.Id,
                    output,
                    options,
                    cancellationToken);

                output = processedOutput;
            }

            // 6. 記錄執行結果
            var executionTime = DateTime.UtcNow - startTime;
            var result = new ExecutionResult
            {
                ExecutionId = executionId,
                Status = execResult.ExitCode == 0 ? "completed" : "failed",
                Output = output,
                ExecutionTime = executionTime,
                CompletedAt = DateTime.UtcNow
            };

            await _codeExecutionRepository.SaveAsync(result, cancellationToken);

            return result;
        }
        finally
        {
            // 7. 釋放容器
            await _containerPoolManager.ReleaseContainerAsync(container.Id, cancellationToken);
        }
    }
    catch (OperationCanceledException)
    {
        // 超時處理
        var executionTime = DateTime.UtcNow - startTime;
        var timeoutResult = new ExecutionResult
        {
            ExecutionId = executionId,
            Status = "timeout",
            Output = new ExecutionOutput
            {
                Stdout = "",
                Stderr = $"Execution timeout after {options.TimeoutSeconds} seconds",
                ExitCode = -1
            },
            ExecutionTime = executionTime,
            CompletedAt = DateTime.UtcNow
        };

        await _codeExecutionRepository.SaveAsync(timeoutResult, cancellationToken);

        return timeoutResult;
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Code execution failed: {ExecutionId}", executionId);
        throw;
    }
}
```

**IDockerService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IDockerService.cs`
- **核心方法**:
  - `CreateContainerAsync()` - 建立容器
  - `StartContainerAsync()` - 啟動容器
  - `StopContainerAsync()` - 停止容器
  - `RemoveContainerAsync()` - 刪除容器
  - `ExecAsync()` - 執行命令
  - `WriteFileToContainerAsync()` - 寫入檔案到容器
  - `ReadFileFromContainerAsync()` - 從容器讀取檔案
  - `InspectContainerAsync()` - 檢查容器狀態

**DockerService 實作**:
- **位置**: `AIAgentPlatform.Infrastructure/Services/DockerService.cs`
- **依賴**: Docker.DotNet (NuGet 套件)
- **Docker API Client**:
  ```csharp
  // DockerClientConfiguration
  var dockerClient = new DockerClientConfiguration(
      new Uri("unix:///var/run/docker.sock")) // Linux/macOS
      // new Uri("npipe://./pipe/docker_engine")) // Windows
      .CreateClient();
  ```

**錯誤處理與重試**:
- **執行失敗**: 記錄錯誤、返回 ExecutionResult（Status = "failed"）
- **超時**: CancellationToken 觸發、停止容器、返回 ExecutionResult（Status = "timeout"）
- **容器崩潰**: 自動重啟容器、重新執行（最多 1 次重試）
- **Docker API 失敗**: 記錄錯誤、釋放容器、拋出例外

**效能指標**:
- **執行啟動時間**: <2 秒（目標）
- **執行時間**: <30 秒（硬限制）
- **成功率**: ≥90%（目標）

### 2.5 執行結果處理與可視化

**完整設計文檔**: [Result Processing](../../../docs/technical-implementation/code-interpreter/result-processing.md)

**IExecutionResultProcessor 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IExecutionResultProcessor.cs`
- **核心方法**: `ProcessAsync(string containerId, ExecutionOutput output, ExecutionOptions options, CancellationToken cancellationToken)`

**結果處理流程**:
```
1. stdout/stderr 解析 → 提取文字輸出
2. 圖表檢測 → 檢查 /tmp/*.png 檔案
3. 圖表讀取 → 從容器讀取圖表檔案
4. 圖表編碼 → Base64 編碼
5. 表格檢測 → 解析 stdout 中的 DataFrame
6. 表格轉換 → JSON 格式
7. 組合結果 → ExecutionOutput
```

**圖表處理（Matplotlib）**:
```python
# Python 代碼範例
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title('Sine Wave')
plt.savefig('/tmp/chart_1.png')  # 儲存圖表
plt.close()

print('Chart saved to /tmp/chart_1.png')
```

**圖表讀取與編碼**:
```csharp
// ExecutionResultProcessor
private async Task<List<ChartOutput>> ProcessChartsAsync(
    string containerId,
    CancellationToken cancellationToken)
{
    var charts = new List<ChartOutput>();

    // 1. 列出 /tmp 目錄下的 PNG 檔案
    var files = await _dockerService.ListFilesAsync(
        containerId,
        "/tmp",
        "*.png",
        cancellationToken);

    // 2. 讀取並編碼每個圖表
    foreach (var file in files)
    {
        var imageBytes = await _dockerService.ReadFileFromContainerAsync(
            containerId,
            file,
            cancellationToken);

        var base64 = Convert.ToBase64String(imageBytes);

        charts.Add(new ChartOutput
        {
            Name = Path.GetFileNameWithoutExtension(file),
            Format = "png",
            Data = base64,
            Width = 800, // 預設寬度
            Height = 600 // 預設高度
        });
    }

    return charts;
}
```

**ChartOutput DTO**:
```csharp
public sealed class ChartOutput
{
    public required string Name { get; init; } // 圖表名稱（檔案名稱）
    public required string Format { get; init; } // 圖表格式（png, svg）
    public required string Data { get; init; } // Base64 編碼的圖表數據
    public int Width { get; init; } = 800; // 圖表寬度
    public int Height { get; init; } = 600; // 圖表高度
}
```

**表格處理（Pandas DataFrame）**:
```python
# Python 代碼範例
import pandas as pd
import json

df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'London', 'Tokyo']
})

print(df)
print('---TABLE_START---')
print(json.dumps(df.to_dict('records')))
print('---TABLE_END---')
```

**表格解析**:
```csharp
// ExecutionResultProcessor
private List<TableOutput> ParseTables(string stdout)
{
    var tables = new List<TableOutput>();

    // 1. 檢測表格標記
    var tablePattern = @"---TABLE_START---(.*?)---TABLE_END---";
    var matches = Regex.Matches(stdout, tablePattern, RegexOptions.Singleline);

    // 2. 解析每個表格
    foreach (Match match in matches)
    {
        var jsonData = match.Groups[1].Value.Trim();

        try
        {
            var data = JsonSerializer.Deserialize<List<Dictionary<string, object>>>(jsonData);

            tables.Add(new TableOutput
            {
                Name = $"table_{tables.Count + 1}",
                Data = data,
                RowCount = data.Count,
                ColumnCount = data.FirstOrDefault()?.Count ?? 0
            });
        }
        catch (JsonException ex)
        {
            _logger.LogWarning(ex, "Failed to parse table data");
        }
    }

    return tables;
}
```

**TableOutput DTO**:
```csharp
public sealed class TableOutput
{
    public required string Name { get; init; } // 表格名稱
    public required IReadOnlyList<Dictionary<string, object>> Data { get; init; } // 表格數據
    public int RowCount { get; init; } // 行數
    public int ColumnCount { get; init; } // 列數
}
```

**錯誤處理**:
- **圖表讀取失敗**: 記錄警告、跳過該圖表
- **表格解析失敗**: 記錄警告、跳過該表格
- **Base64 編碼失敗**: 記錄錯誤、拋出例外

**效能考量**:
- **圖表數量限制**: 最多 10 個圖表（防止記憶體溢位）
- **圖表大小限制**: 最大 5MB / 圖表
- **表格大小限制**: 最多 1000 行（防止 JSON 過大）

### 2.6 資源管理與效能優化

**完整設計文檔**: [Performance Optimization](../../../docs/technical-implementation/code-interpreter/performance-optimization.md)

**資源監控**:
- **CPU 使用監控**:
  ```csharp
  // 監控容器 CPU 使用率
  var stats = await _dockerService.GetContainerStatsAsync(containerId);
  var cpuUsage = stats.CPUStats.CPUUsage.TotalUsage / stats.CPUStats.SystemCPUUsage * 100;

  if (cpuUsage > 90)
  {
      _logger.LogWarning("Container {ContainerId} CPU usage high: {CpuUsage}%", containerId, cpuUsage);
  }
  ```

- **Memory 使用監控**:
  ```csharp
  // 監控容器 Memory 使用
  var memoryUsage = stats.MemoryStats.Usage;
  var memoryLimit = stats.MemoryStats.Limit;
  var memoryPercent = (double)memoryUsage / memoryLimit * 100;

  if (memoryPercent > 90)
  {
      _logger.LogWarning("Container {ContainerId} Memory usage high: {MemoryUsage}MB/{MemoryLimit}MB",
          containerId, memoryUsage / 1024 / 1024, memoryLimit / 1024 / 1024);
  }
  ```

**效能優化策略**:

**1. Docker Image 優化**:
- **Multi-stage Build**: 減少 Image 大小
  ```dockerfile
  # Stage 1: 建置環境
  FROM python:3.11 AS builder
  RUN pip install --user numpy pandas matplotlib seaborn

  # Stage 2: 運行環境
  FROM python:3.11-slim-bookworm
  COPY --from=builder /root/.local /root/.local
  ENV PATH=/root/.local/bin:$PATH
  ```
- **Layer 快取**: 利用 Docker Layer 快取加速建置
- **移除不必要套件**: 減少 Image 大小和攻擊面
- **Image 大小目標**: <500MB

**2. 容器預熱**:
- **啟動時預創建**: 應用啟動時預先建立 3 個容器
- **容器復用**: 執行完畢後重置容器，而非銷毀重建
- **預載入套件**: 預先 import 常用套件（numpy, pandas）

**3. 執行優化**:
- **簡單代碼優先**: 優先執行簡單代碼（快速回應）
- **複雜代碼背景執行**: 複雜代碼異步執行（非阻塞）
- **執行佇列管理**: 超過 3 個並發時進入等待佇列

**4. 結果快取**:
- **執行結果快取**: Redis 快取執行結果（1 小時）
- **快取鍵**: SHA256 Hash（代碼內容）
- **快取命中率**: 預期 20-30%（相似代碼重複執行）

**效能指標**:
- **容器啟動時間**: <5 秒（目標）
- **執行啟動時間**: <2 秒（目標）
- **執行時間**: <30 秒（硬限制）
- **成功率**: ≥90%（目標）
- **並發執行數**: 3（MVP 固定）

**資源清理**:
- **執行完畢後清理**: 刪除 /tmp 檔案
- **定期清理**: 每小時清理未使用的容器
- **異常清理**: 容器崩潰後清理殘留資源

---

## 三、完整文檔引用清單 (按功能分類)

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
9. [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md) - Sprint 6 概覽
10. [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) - Sprint 6 實施計劃
11. [SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md) - Sprint 6 上下文與背景（本文件）
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

### 架構設計文檔 - ADRs (7 refs)
18. [Architecture-Design-Document.md](../../docs/architecture/Architecture-Design-Document.md) - 完整架構設計
19. [ADR-008: Code Interpreter Container Pool](../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md) - Docker 容器池設計與動態生命週期管理
20. [ADR-006: Hybrid State Management](../../docs/architecture/adr/ADR-006-hybrid-state-management.md) - Redis + PostgreSQL 混合狀態管理
21. [ADR-015: Execution Timeout Strategy](../../docs/architecture/adr/ADR-015-execution-timeout-strategy.md) - 執行超時策略 (New)
22. [ADR-016: Resource Limit Configuration](../../docs/architecture/adr/ADR-016-resource-limit-configuration.md) - 資源限制配置 (New)
23. [ADR-001: Clean Architecture](../../docs/architecture/adr/ADR-001-clean-architecture.md) - Clean Architecture 分層架構
24. [ADR-002: CQRS Pattern](../../docs/architecture/adr/ADR-002-cqrs-pattern.md) - CQRS 模式實作

### 技術實作文檔 - Code Interpreter (6 refs)
25. [06-code-interpreter-implementation.md](../../docs/technical-implementation/01-backend-net9/06-code-interpreter-implementation.md) - Code Interpreter 完整實作指南
26. [container-pool-management.md](../../docs/technical-implementation/code-interpreter/container-pool-management.md) - 容器池管理詳解 (New)
27. [docker-image-design.md](../../docs/technical-implementation/code-interpreter/docker-image-design.md) - Docker Image 設計 (New)
28. [execution-engine.md](../../docs/technical-implementation/code-interpreter/execution-engine.md) - 執行引擎實作 (New)
29. [result-processing.md](../../docs/technical-implementation/code-interpreter/result-processing.md) - 執行結果處理 (New)
30. [performance-optimization.md](../../docs/technical-implementation/code-interpreter/performance-optimization.md) - 效能優化策略 (New)

### 安全設計文檔 (5 refs)
31. [code-execution-security.md](../../docs/security/code-execution-security.md) - Code Interpreter 4-Layer Security Architecture 完整設計
32. [container-security.md](../../docs/security/container-security.md) - 容器安全設計 (New)
33. [network-isolation.md](../../docs/security/network-isolation.md) - 網路隔離設計 (New)
34. [filesystem-isolation.md](../../docs/security/filesystem-isolation.md) - 文件系統隔離設計 (New)
35. [resource-limits.md](../../docs/security/resource-limits.md) - 資源限制設計 (New)

### API 設計文檔 (3 refs)
36. [API-Design-Guidelines.md](../../docs/api/API-Design-Guidelines.md) - API 設計指南
37. [code-interpreter-api-design.md](../../docs/api/code-interpreter-api-design.md) - Code Interpreter API 設計 (New)
38. [api-endpoints-v1.md](../../docs/api/api-endpoints-v1.md) - API v1 端點清單

### 資料庫文檔 (2 refs)
39. [database-schema-design.md](../../docs/database/database-schema-design.md) - 資料庫 Schema 設計
40. [code-execution-entity-design.md](../../docs/database/code-execution-entity-design.md) - CodeExecution Entity 設計 (New)

### 測試文檔 (5 refs)
41. [testing-strategy-overview.md](../../docs/testing/testing-strategy-overview.md) - 測試策略概覽
42. [unit-testing-guidelines.md](../../docs/testing/unit-testing-guidelines.md) - 單元測試指南
43. [integration-testing-guidelines.md](../../docs/testing/integration-testing-guidelines.md) - 整合測試指南
44. [security-testing.md](../../docs/testing/security-testing.md) - 安全測試指南 (New)
45. [code-interpreter-testing-strategy.md](../../docs/testing/code-interpreter-testing-strategy.md) - Code Interpreter 測試策略 (New)

### UX 設計文檔 (3 refs)
46. [user-research/personas.md](../../docs/ux-design/user-research/personas.md) - 使用者研究 Personas
47. [wireframes/low-fidelity/07-code-interpreter.md](../../docs/ux-design/wireframes/low-fidelity/07-code-interpreter.md) - Code Interpreter UI 設計
48. [code-interpreter-ui-design.md](../../docs/ux-design/code-interpreter-ui-design.md) - Code Interpreter UI 設計詳解 (New)

### 開發標準與流程 (3 refs)
49. [coding-standards-csharp.md](../../docs/development-standards/coding-standards-csharp.md) - C# 編碼標準
50. [coding-standards-typescript.md](../../docs/development-standards/coding-standards-typescript.md) - TypeScript 編碼標準
51. [git-workflow.md](../../docs/development-standards/git-workflow.md) - Git 工作流程

### Docker 與部署文檔 (4 refs)
52. [docker-compose-setup.md](../../docs/deployment/docker-compose-setup.md) - Docker Compose 設置
53. [code-interpreter-dockerfile.md](../../docs/deployment/code-interpreter-dockerfile.md) - Code Interpreter Dockerfile 設計 (New)
54. [container-networking.md](../../docs/deployment/container-networking.md) - 容器網路配置 (New)
55. [deployment-guide.md](../../docs/deployment/deployment-guide.md) - 部署指南

### 程式碼參考 - Application Layer (4 refs)
56. [ICodeExecutionService.cs](../../src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs) - 代碼執行服務介面 (New)
57. [IContainerPoolManager.cs](../../src/AIAgentPlatform.Application/Interfaces/IContainerPoolManager.cs) - 容器池管理介面 (New)
58. [IDockerService.cs](../../src/AIAgentPlatform.Application/Interfaces/IDockerService.cs) - Docker 服務介面 (New)
59. [IExecutionResultProcessor.cs](../../src/AIAgentPlatform.Application/Interfaces/IExecutionResultProcessor.cs) - 執行結果處理介面 (New)

### 程式碼參考 - Domain Layer (2 refs)
60. [CodeExecution.cs](../../src/AIAgentPlatform.Domain/Entities/CodeExecution.cs) - CodeExecution Entity (New)
61. [ExecutionResult.cs](../../src/AIAgentPlatform.Domain/ValueObjects/ExecutionResult.cs) - ExecutionResult Value Object (New)

### 程式碼參考 - Infrastructure Layer (6 refs)
62. [DockerCodeExecutionService.cs](../../src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs) - Docker 代碼執行服務 (New)
63. [ContainerPoolManager.cs](../../src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs) - 容器池管理器 (New)
64. [DockerService.cs](../../src/AIAgentPlatform.Infrastructure/Services/DockerService.cs) - Docker 服務實作 (New)
65. [ExecutionResultProcessor.cs](../../src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs) - 執行結果處理器 (New)
66. [ContainerHealthChecker.cs](../../src/AIAgentPlatform.Infrastructure/Services/ContainerHealthChecker.cs) - 容器健康檢查 (New)
67. [SecurityManager.cs](../../src/AIAgentPlatform.Infrastructure/Services/SecurityManager.cs) - 安全管理器 (New)

### 程式碼參考 - API Layer (1 ref)
68. [CodeController.cs](../../src/AIAgentPlatform.API/Controllers/CodeController.cs) - Code API Controller (New)

### 程式碼參考 - Frontend (3 refs)
69. [CodeInterpreter.tsx](../../apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx) - Code Interpreter 主元件 (New)
70. [CodeEditor.tsx](../../apps/web-app/src/features/code-interpreter/components/CodeEditor.tsx) - 代碼編輯器元件 (New)
71. [ExecutionResult.tsx](../../apps/web-app/src/features/code-interpreter/components/ExecutionResult.tsx) - 執行結果元件 (New)

### 程式碼參考 - Docker (1 ref)
72. [Dockerfile](../../docker/code-interpreter/Dockerfile) - Code Interpreter Docker Image 定義 (New)

---

## 📖 使用指南

### 如何使用此文件

**開發階段使用**:
1. **開發 US 5.1 Docker 容器池**: 閱讀「第二章 2.2」了解容器池管理、生命週期、健康檢查實作
2. **開發 US 5.1 安全架構**: 閱讀「第二章 2.3」了解 4-Layer Security Architecture 詳細配置
3. **開發 US 5.1 執行引擎**: 閱讀「第二章 2.4」了解 Python 執行引擎實作
4. **開發 US 5.1 結果處理**: 閱讀「第二章 2.5」了解圖表、表格處理實作
5. **查詢架構決策**: 查閱「第一章 1.2-1.3」的 ADR 文檔
6. **查詢安全設計**: 查閱「第一章 1.4」的安全設計文檔
7. **查詢 API 規格**: 查閱「第一章 1.5」的 API 設計文檔

**疑難排解使用**:
- **容器池管理問題**: 查閱「2.2 Docker 容器池架構設計」
- **安全配置問題**: 查閱「2.3 4-Layer Security Architecture 詳解」
- **執行超時問題**: 查閱「2.4 Python 執行引擎設計」
- **結果處理問題**: 查閱「2.5 執行結果處理與可視化」
- **效能優化問題**: 查閱「2.6 資源管理與效能優化」

**Code Review 使用**:
- 驗證實作是否符合「關鍵技術決策」
- 檢查介面設計是否與文檔一致
- 確認安全配置是否完整（4-Layer Security）
- 確認效能目標是否達成

---

## 📝 更新日誌

**v2.1** (2025-11-14)
- ✅ 初始版本建立
- ✅ 完整 US 5.1 (Code Interpreter - Python 執行引擎) 技術上下文
- ✅ 完整 Docker 容器池架構設計（2.2 章節）
- ✅ 完整 4-Layer Security Architecture 詳解（2.3 章節）
- ✅ 完整 Python 執行引擎設計（2.4 章節）
- ✅ 完整執行結果處理與可視化（2.5 章節）
- ✅ 完整資源管理與效能優化（2.6 章節）
- ✅ 7 個 ADR 文檔引用與說明
- ✅ 6 個技術實作指南引用
- ✅ 5 個安全設計文檔引用
- ✅ 完整 API、資料庫、測試文檔引用（72+ 文檔）
- ✅ MVP 範圍定義與 Phase 2 延後項目明確說明
- ✅ 6 個關鍵技術決策詳細分析

**文件統計** (v2.1):
- 總行數: ~1,300 行
- User Stories: 1 個 (US 5.1)
- Story Points: 5 SP (調整後，原計劃 13 SP)
- Phases: 5 個
- 技術決策: 6 個關鍵決策
- 參考文獻: 72+ 文檔

**品質指標**:
- 與 Sprint 5-3-CONTEXT 格式一致性: ✅ 100%
- v2.1 標準符合度: ✅ 100%
- 參考文獻完整性: ✅ 72+ 文檔
- 技術細節深度: ✅ 6 大章節，詳細技術實作
- MVP 範圍清晰度: ✅ 明確定義 Phase 1A vs Phase 2

**特別說明**:
- Sprint 6 為 Phase 1A 最後一個 Sprint
- 完成後 Phase 1A (基礎平台) 達成重要里程碑
- 為 Phase 1B (工作流引擎核心) 做準備
- MVP 簡化版本（5 SP），完整版留待 Phase 1B-1C
- 特別強調 4-Layer Security Architecture 和滲透測試策略

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 6 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-6/SPRINT-6-3-CONTEXT.md`

---

**相關文檔**:
- ⬆️ 上一層: [Sprint 6 Overview](./SPRINT-6-1-OVERVIEW.md)
- ⬅️ 上一步: [Sprint 6 Plan](./SPRINT-6-2-PLAN.md)
- ➡️ 下一步: [Sprint 6 Checklist](./SPRINT-6-4-CHECKLIST.md)
