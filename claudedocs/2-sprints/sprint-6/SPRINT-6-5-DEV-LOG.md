# SPRINT-6-5-DEV-LOG.md - Sprint 6 開發日誌：Code Interpreter 安全執行環境實作記錄

**版本**: v2.1
**Sprint 編號**: Sprint 6
**Sprint 週期**: Week 16-18 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2026-01-27 ~ 2026-02-16
**狀態**: 📋 計劃階段 (Planned) - 將在 Sprint 執行時更新
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 文件目的

本文件作為 Sprint 6 執行期間的**每日開發日誌**，記錄：
- 每日開發進度與完成任務
- 技術決策與實作細節
- 遇到的問題與解決方案
- 程式碼變更與 Commit 記錄
- 測試執行結果（特別是安全測試）
- 團隊協作與溝通事項
- 安全漏洞發現與修復記錄

**更新頻率**: 每日更新（開發日結束時）

**重要提示**: Sprint 6 涉及 Code Interpreter 安全執行環境，**安全測試與漏洞修復為本 Sprint 最高優先級**。所有安全相關問題必須在 Sprint 結束前完全解決。

---

## 📑 目錄 (Table of Contents)

1. [使用指南](#-使用指南)
2. [Sprint 6 目標回顧](#-sprint-6-目標回顧)
3. [Week 16 開發日誌](#week-16-開發日誌)
4. [Week 17 開發日誌](#week-17-開發日誌)
5. [Week 18 開發日誌](#week-18-開發日誌)
6. [技術決策記錄](#-技術決策記錄)
7. [問題與解決方案](#-問題與解決方案)
8. [安全測試記錄](#-安全測試記錄)
9. [經驗總結](#-經驗總結)
10. [參考文檔](#-參考文檔)
11. [版本歷史](#-版本歷史)

---

## 📖 使用指南

### 適用對象
- **開發團隊**: 記錄每日進度與技術決策
- **Tech Lead**: 追蹤開發狀況與問題
- **Scrum Master**: 監控 Sprint 進展與阻礙
- **Security Team**: 追蹤安全測試與漏洞修復進度
- **未來團隊成員**: 了解歷史決策與實作脈絡
- **AI Assistant**: 追蹤開發狀態與問題解決

### 更新時機
- **每日站會後**: 更新昨日完成的任務與今日計劃
- **重要決策時**: 即時記錄技術決策與理由
- **遇到問題時**: 記錄問題與暫時/最終解決方案
- **完成任務時**: 記錄實作細節與 Commit ID
- **安全測試後**: 詳細記錄測試結果、發現漏洞與修復方案
- **整合測試後**: 記錄測試結果與問題

### 記錄格式
```markdown
### 📅 YYYY-MM-DD (DayOfWeek) - 標題

#### ✅ 完成任務
- 任務描述與完成細節
- 相關 Commit: `commit-hash`

#### 🔧 技術決策
- 決策內容與理由
- 參考 ADR 文檔

#### ⚠️ 遇到問題
- 問題描述
- 解決方案或暫時處理

#### 🔐 安全測試結果 (如適用)
- 測試項目與結果
- 發現漏洞與嚴重程度
- 修復方案與驗證

#### 📊 測試結果
- 單元測試、整合測試結果
- 覆蓋率統計

#### 📝 備註
- 其他重要資訊
```

### 快速導航
- **查看 Sprint 6 整體概覽** → [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md)
- **查看技術上下文** → [SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md)
- **查看任務檢查清單** → [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md)
- **查看問題追蹤** → [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md)

---

## 🎯 Sprint 6 目標回顧

### 主要交付成果
1. **Code Interpreter - Python 執行引擎 (US 5.1, 5 SP)**
   - Docker 容器池管理（固定 3 個容器）
   - Python 執行引擎（基於 Docker）
   - 4-Layer Security Architecture
   - 執行超時控制（30 秒）
   - 資源限制（CPU, Memory, Disk）
   - 執行結果處理（Text, Image, Table）
   - Code Execution API
   - Code Interpreter UI

### Definition of Done
- [ ] 所有 User Story 的 Acceptance Criteria 滿足
- [ ] 單元測試覆蓋率 ≥85%
- [ ] 整合測試通過率 100%
- [ ] 執行成功率 ≥90%
- [ ] 執行時間 <30 秒 (P95)
- [ ] API 文件完整（Swagger）
- [ ] **安全測試通過率 100%（無容器逃逸漏洞）** ⚠️ 最高優先級
- [ ] 程式碼審查通過
- [ ] 無 Critical/High 等級 Bug
- [ ] Performance 測試達標

### Story Points 分配
- US 5.1: Code Interpreter - Python 執行引擎 (5 SP)
- **總計**: 5 SP

### 核心挑戰
1. **安全性**: 防止容器逃逸、資源濫用、惡意代碼攻擊
2. **效能**: 確保執行時間 <30 秒、容器啟動時間 <5 秒
3. **穩定性**: 容器池管理、健康檢查、自動恢復
4. **可靠性**: 錯誤處理、執行超時、結果處理

---

## Week 16 開發日誌

> **Week 16 目標**: US 5.1 Phase 1-2 - Docker 環境搭建 + Python 執行引擎
> **預期完成**: Phase 1 (1.5 SP) + Phase 2 (1.5 SP) = 3 SP (~60%)

### 📅 2026-01-27 (Monday) - Sprint 6 啟動與 Docker 環境設置

#### ✅ 完成任務
- [ ] **Sprint Planning 會議**
  - 時間: 09:00-11:00
  - 參與者: 全團隊 + Security Team
  - 產出: Sprint 6 Backlog 確認，5 SP 分配
  - US 5.1 拆分為 5 個 Phases
  - **特別討論**: RISK-016 (Container 逃逸漏洞) 緩解措施
  - 安全測試計劃確認（Week 2 進行滲透測試）

- [ ] **環境設置與驗證**
  - Docker Engine 版本確認 (≥20.10)
  - Docker.DotNet NuGet 套件安裝 (版本 3.125.x)
  - Python 3.11 Official Docker Image 拉取測試
  - Docker Compose 配置檔案準備
  - 開發環境 Docker 權限驗證
  - 相關 Commit: `[待填寫]`

- [ ] **Feature Branch 創建**
  - Branch: `feature/us-5.1-code-interpreter`
  - Base: `main`
  - 分支保護規則確認
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-044: Docker Base Image 選擇**
  - 決策: 使用 `python:3.11-slim-bookworm`
  - 理由: 官方 Image、輕量級、安全更新及時
  - 替代方案: `python:3.11-alpine` (過於精簡，缺少部分依賴)
  - Image 大小: 預估 <500MB (含預裝套件)
  - 參考: ADR-008 Code Interpreter Container Pool

- [ ] **TD-045: 容器數量決策**
  - 決策: 固定 3 個容器（MVP 簡化版）
  - 理由: 平衡並發需求與資源消耗，降低複雜度
  - 未來擴展: Phase 2 實作動態擴縮容（5-10 個容器）
  - 預期並發執行: 最多 3 個同時執行

#### 📝 備註
- [ ] 所有基礎設施就緒
- [ ] 環境變數配置完成
- [ ] 團隊對 Sprint 6 目標達成共識
- [ ] Security Team 確認安全測試計劃

---

### 📅 2026-01-28 (Tuesday) - Docker Image 設計與建置

#### ✅ 完成任務
- [ ] **Dockerfile 撰寫**
  - 建立 `docker/code-interpreter/Dockerfile`
  - Base Image: `python:3.11-slim-bookworm`
  - 套件安裝:
    ```dockerfile
    # 預安裝套件
    RUN pip install --no-cache-dir \
        numpy==1.24.3 \
        pandas==2.0.3 \
        matplotlib==3.7.2 \
        seaborn==0.12.2
    ```
  - 安全配置:
    - 非 root 用戶創建: `USER coderunner`
    - Read-only 文件系統（除 /tmp）
    - 最小權限設定
  - 相關 Commit: `[待填寫]`

- [ ] **Docker Image 建置與測試**
  - 建置命令: `docker build -t aiagent/code-interpreter:latest .`
  - Image 大小驗證: 預期 <500MB
  - 基本功能測試:
    ```bash
    docker run --rm aiagent/code-interpreter:latest python -c "import numpy; print(numpy.__version__)"
    docker run --rm aiagent/code-interpreter:latest python -c "import pandas; print(pandas.__version__)"
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **安全掃描**
  - 使用 Trivy 掃描 Docker Image
    ```bash
    trivy image aiagent/code-interpreter:latest
    ```
  - 掃描結果: [待填寫 - Critical/High 漏洞數量]
  - 漏洞修復: [待填寫]
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-046: 套件版本鎖定策略**
  - 決策: 鎖定主要套件版本（numpy, pandas, matplotlib, seaborn）
  - 理由: 確保執行環境一致性，避免套件升級導致 Breaking Changes
  - 更新策略: 每月安全更新一次（Security Patch Only）
  - 版本管理: 使用 `requirements.txt` 鎖定所有依賴

- [ ] **TD-047: Read-only 文件系統配置**
  - 決策: 整個容器文件系統 Read-only，僅 /tmp 可寫
  - 理由: 防止惡意代碼修改系統檔案、持久化後門
  - 例外處理: Python pip cache 需要可寫目錄
  - 實作: Docker run 參數 `--read-only --tmpfs /tmp:rw,size=100m`

#### ⚠️ 遇到問題
- [ ] **問題**: Docker Image 大小超過預期
  - 預期: <500MB
  - 實際: [待填寫]
  - 原因: [待分析 - apt cache, pip cache, 不必要的依賴]
  - 解決方案:
    - 清理 apt cache: `apt-get clean && rm -rf /var/lib/apt/lists/*`
    - 清理 pip cache: `pip install --no-cache-dir`
    - 移除不必要的開發工具
  - 相關 Commit: `[待填寫]`

#### 📊 測試結果
```
Docker Image 測試:
- Image 建置成功: ✅
- Image 大小: [待填寫] MB (Target: <500MB)
- Python 版本驗證: ✅ 3.11.x
- numpy 安裝驗證: ✅
- pandas 安裝驗證: ✅
- matplotlib 安裝驗證: ✅
- seaborn 安裝驗證: ✅

安全掃描 (Trivy):
- Critical 漏洞: [待填寫]
- High 漏洞: [待填寫]
- Medium 漏洞: [待填寫]
```

#### 📝 備註
- [ ] Docker Image 基本功能完成
- [ ] 需要持續優化 Image 大小
- [ ] 安全掃描需要定期執行

---

### 📅 2026-01-29 (Wednesday) - 容器池管理實作

#### ✅ 完成任務
- [ ] **IContainerPoolManager 介面設計**
  - 建立 `src/AIAgentPlatform.Application/Interfaces/IContainerPoolManager.cs`
  - 方法定義:
    ```csharp
    Task InitializeAsync(int poolSize);
    Task<Container> AcquireContainerAsync();
    Task ReleaseContainerAsync(string containerId);
    Task<ContainerHealthStatus> GetHealthStatusAsync();
    Task ShutdownAsync();
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **ContainerPoolManager 實作**
  - 建立 `src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs`
  - 容器池初始化:
    - 啟動 3 個容器
    - 容器命名: `code-interpreter-{1,2,3}`
    - 容器預熱: 執行簡單 Python 代碼驗證
  - 容器分配邏輯:
    - 使用 Semaphore 控制並發
    - FIFO 佇列管理空閒容器
    - 分配超時: 30 秒
  - 容器釋放邏輯:
    - 重置容器狀態（清除 /tmp 檔案）
    - 歸還至空閒池
  - 相關 Commit: `[待填寫]`

- [ ] **ContainerHealthChecker 實作**
  - 建立 `src/AIAgentPlatform.Infrastructure/Services/ContainerHealthChecker.cs`
  - 健康檢查邏輯:
    - 每 30 秒 Ping 所有容器
    - 執行簡單 Python 命令: `python -c "print('ok')"`
    - 超時: 5 秒
  - 異常處理:
    - 健康檢查失敗 → 標記為 Unhealthy
    - 3 次連續失敗 → 重啟容器
    - 重啟失敗 → 創建新容器並移除異常容器
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-048: 容器分配策略**
  - 決策: 使用 Semaphore + FIFO Queue
  - 理由: 簡單可靠、公平分配、避免餓死
  - 替代方案: Round-Robin（過於複雜）
  - 分配超時: 30 秒（與執行超時一致）

- [ ] **TD-049: 容器重置策略**
  - 決策: 釋放時清除 /tmp 檔案，不重啟容器
  - 理由: 提升效能、減少容器啟動時間
  - 風險: 可能殘留 Python 全局變數（風險可控）
  - 完整重置: 每 10 次使用後重啟容器（Phase 2 實作）

- [ ] **TD-050: 健康檢查頻率**
  - 決策: 每 30 秒檢查一次
  - 理由: 平衡檢測及時性與系統負載
  - 異常閾值: 3 次連續失敗才重啟
  - 避免誤判: 使用 Retry with Backoff

#### 📊 測試結果
```
ContainerPoolManager Unit Tests:
- InitializeAsync (3 containers): 1/1 Passed
- AcquireContainerAsync: 2/2 Passed
- ReleaseContainerAsync: 2/2 Passed
- Acquire Timeout (30s): 1/1 Passed
- Coverage: 88%

ContainerHealthChecker Unit Tests:
- Health Check Success: 1/1 Passed
- Health Check Failure: 1/1 Passed
- Container Restart on 3 Failures: 1/1 Passed
- Coverage: 85%

Integration Tests:
- 3 Containers Startup: Passed (Startup Time <15s)
- Concurrent Acquire (3 threads): Passed
- Health Check Loop (5 minutes): Passed
```

#### 📝 備註
- [ ] 容器池基本功能完成
- [ ] 健康檢查機制運作正常
- [ ] 下一步: Python 執行引擎實作

---

### 📅 2026-01-30 (Thursday) - ICodeExecutionService 與 DockerService

#### ✅ 完成任務
- [ ] **ICodeExecutionService 介面設計**
  - 建立 `src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs`
  - 方法定義:
    ```csharp
    Task<ExecutionResult> ExecuteCodeAsync(ExecuteCodeRequest request);
    Task<ExecutionResult> GetExecutionResultAsync(string executionId);
    Task CancelExecutionAsync(string executionId);
    ```
  - 參數定義:
    ```csharp
    public class ExecuteCodeRequest
    {
        public string Code { get; set; }
        public string Language { get; set; } = "python";
        public int TimeoutSeconds { get; set; } = 30;
        public ResourceLimits ResourceLimits { get; set; }
    }
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **IDockerService 介面設計**
  - 建立 `src/AIAgentPlatform.Application/Interfaces/IDockerService.cs`
  - 方法定義:
    ```csharp
    Task<string> CreateContainerAsync(ContainerConfig config);
    Task StartContainerAsync(string containerId);
    Task StopContainerAsync(string containerId);
    Task<ExecResult> ExecCommandAsync(string containerId, string[] command);
    Task<ContainerInspectResponse> InspectContainerAsync(string containerId);
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **DockerService 實作**
  - 建立 `src/AIAgentPlatform.Infrastructure/Services/DockerService.cs`
  - Docker.DotNet 整合:
    ```csharp
    private readonly DockerClient _dockerClient;

    public DockerService()
    {
        _dockerClient = new DockerClientConfiguration(
            new Uri("unix:///var/run/docker.sock")
        ).CreateClient();
    }
    ```
  - 容器建立:
    - Image: `aiagent/code-interpreter:latest`
    - Resource Limits: CPU 1 core, Memory 512MB
    - Network: none (完全隔離)
    - Read-only filesystem (除 /tmp)
  - 命令執行:
    - 使用 `ExecCreateAsync` + `ExecStartAsync`
    - 擷取 stdout, stderr, exit code
    - 超時控制: CancellationTokenSource
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-051: Docker 連接方式**
  - 決策: Unix Socket (`unix:///var/run/docker.sock`)
  - 理由: Linux 環境標準方式、最高效能
  - Windows 替代: Named Pipe (`npipe://./pipe/docker_engine`)
  - 跨平台支援: 根據 OS 自動選擇

- [ ] **TD-052: 命令執行超時機制**
  - 決策: 使用 CancellationTokenSource with Timeout
  - 理由: .NET 原生支援、可靠、易於測試
  - 超時處理: Kill Docker Exec Process → Stop Container → 返回 Timeout Error
  - 預設超時: 30 秒（可配置）

#### ⚠️ 遇到問題
- [ ] **問題**: Docker.DotNet ExecStartAsync 無法設定超時
  - 影響: 無法精確控制執行超時
  - 解決方案: 使用 `Task.WhenAny` + `Task.Delay` 實現超時
    ```csharp
    var execTask = _dockerClient.Exec.StartAndAttachContainerExecAsync(execId, false);
    var timeoutTask = Task.Delay(TimeSpan.FromSeconds(timeoutSeconds));

    var completedTask = await Task.WhenAny(execTask, timeoutTask);
    if (completedTask == timeoutTask)
    {
        // Timeout: Kill exec and stop container
    }
    ```
  - 相關 Commit: `[待填寫]`

#### 📊 測試結果
```
DockerService Unit Tests:
- CreateContainerAsync: 1/1 Passed
- StartContainerAsync: 1/1 Passed
- StopContainerAsync: 1/1 Passed
- ExecCommandAsync (Simple): 2/2 Passed
- ExecCommandAsync (Timeout): 1/1 Passed
- Coverage: 87%

Integration Tests:
- Create and Start Container: Passed (<5s)
- Execute Python Print: Passed (stdout captured)
- Execute Python Error: Passed (stderr captured)
- Execute with Timeout: Passed (timeout after 30s)
```

#### 📝 備註
- [ ] Docker 服務基本功能完成
- [ ] 超時機制運作正常
- [ ] 下一步: DockerCodeExecutionService 實作

---

### 📅 2026-01-31 (Friday) - DockerCodeExecutionService 實作

#### ✅ 完成任務
- [ ] **DockerCodeExecutionService 實作**
  - 建立 `src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs`
  - 執行流程:
    ```csharp
    public async Task<ExecutionResult> ExecuteCodeAsync(ExecuteCodeRequest request)
    {
        // 1. 從容器池獲取容器
        var container = await _containerPoolManager.AcquireContainerAsync();

        try
        {
            // 2. 將代碼寫入容器 /tmp/code.py
            await WriteCodeToContainerAsync(container.Id, request.Code);

            // 3. 執行 Python 代碼
            var result = await _dockerService.ExecCommandAsync(
                container.Id,
                new[] { "python", "/tmp/code.py" },
                request.TimeoutSeconds
            );

            // 4. 處理執行結果
            return ProcessExecutionResult(result);
        }
        finally
        {
            // 5. 釋放容器回池
            await _containerPoolManager.ReleaseContainerAsync(container.Id);
        }
    }
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **ExecutionResult 處理**
  - 建立 `src/AIAgentPlatform.Domain/ValueObjects/ExecutionResult.cs`
  - Properties:
    ```csharp
    public class ExecutionResult
    {
        public string ExecutionId { get; set; }
        public ExecutionStatus Status { get; set; }
        public string Stdout { get; set; }
        public string Stderr { get; set; }
        public int ExitCode { get; set; }
        public long ExecutionTimeMs { get; set; }
        public List<OutputArtifact> Artifacts { get; set; } // 圖表、表格
    }
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **錯誤處理與日誌記錄**
  - 執行失敗: 返回 ExecutionStatus.Failed + Stderr
  - 執行超時: 返回 ExecutionStatus.Timeout
  - 容器異常: 返回 ExecutionStatus.ContainerError
  - 結構化日誌: Serilog 記錄所有執行細節
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-053: 代碼傳遞方式**
  - 決策: 寫入容器 /tmp/code.py 檔案
  - 理由: 簡單可靠、支援多行代碼、易於除錯
  - 替代方案: 透過 stdin 傳遞（不支援複雜代碼）
  - 安全考量: /tmp 目錄配額限制 100MB

- [ ] **TD-054: 執行結果儲存策略**
  - 決策: PostgreSQL 儲存執行記錄，Redis Cache 近期結果（1 小時）
  - 理由: 持久化歷史記錄、快速查詢近期結果
  - 資料保留: 執行記錄保留 30 天
  - Artifacts (圖表): 儲存於 Azure Blob Storage

#### 📊 測試結果
```
DockerCodeExecutionService Unit Tests:
- ExecuteCodeAsync (Simple Print): 1/1 Passed
- ExecuteCodeAsync (Error): 1/1 Passed
- ExecuteCodeAsync (Timeout): 1/1 Passed
- ExecuteCodeAsync (Container Unavailable): 1/1 Passed
- Coverage: 90%

Integration Tests (E2E):
- Execute Python Print: Passed (Execution Time <1s)
- Execute Python Calculation: Passed
- Execute Python Error (SyntaxError): Passed (Stderr captured)
- Execute Python Error (RuntimeError): Passed
- Execute with Timeout: Passed (Timeout after 30s)
- Concurrent Execution (3 threads): Passed
```

#### 📝 備註
- [ ] Week 16 目標達成：Phase 1-2 完成 (3 SP, 60%)
- [ ] Python 執行引擎基本功能完成
- [ ] 下週重點：4-Layer Security Architecture + 安全測試（最高優先級）

---

## Week 17 開發日誌

> **Week 17 目標**: US 5.1 Phase 3 - 4-Layer Security Architecture + 安全測試
> **預期完成**: Phase 3 (1 SP) = 1 SP (~80%)
> **關鍵里程碑**: 安全測試（滲透測試）與漏洞修復 ⚠️ **最高優先級**

### 📅 2026-02-03 (Monday) - Layer 1 & 2: Container 與 Network 隔離

#### ✅ 完成任務
- [ ] **Layer 1: Container 隔離配置**
  - 非 root 用戶執行:
    ```dockerfile
    # Dockerfile
    RUN groupadd -r coderunner && useradd -r -g coderunner coderunner
    USER coderunner
    ```
  - Read-only 文件系統:
    ```csharp
    // DockerService.cs
    HostConfig = new HostConfig
    {
        ReadonlyRootfs = true,
        Tmpfs = new Dictionary<string, string>
        {
            { "/tmp", "rw,size=100m,mode=1777" }
        }
    }
    ```
  - 移除不必要的 Capabilities:
    ```csharp
    CapDrop = new[] { "ALL" },
    CapAdd = new[] { } // 無額外 Capabilities
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **Layer 2: Network 隔離配置**
  - 禁用 Network:
    ```csharp
    NetworkMode = "none" // 完全禁止網路存取
    ```
  - iptables 規則（宿主機層級）:
    ```bash
    # 禁止容器存取外部網路
    iptables -A DOCKER-USER -i docker0 -j DROP
    ```
  - 驗證 Network 隔離:
    - 測試代碼: `requests.get('https://google.com')` → 應失敗
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-055: Network 隔離強度**
  - 決策: 完全禁用 Network (NetworkMode = "none")
  - 理由: 最安全、無任何網路攻擊向量
  - 影響: 無法使用 pip install、無法存取外部 API
  - 解決方案: 所有套件預先安裝於 Docker Image

- [ ] **TD-056: Capabilities 最小化**
  - 決策: Drop ALL Capabilities，無額外授予
  - 理由: 最小權限原則、降低攻擊面
  - 影響: 無法執行特權操作（如 chroot, setuid）
  - 測試: 驗證無法使用 `os.setuid()`, `os.chroot()`

#### 🔐 安全測試結果
```
Layer 1 測試:
- 非 root 用戶執行: ✅ 驗證通過 (whoami = coderunner)
- Read-only 文件系統: ✅ 無法寫入 / 目錄
- /tmp 可寫: ✅ 可寫入 /tmp/test.txt
- Capabilities Drop: ✅ 無特權操作權限

Layer 2 測試:
- Network 禁用: ✅ 無法 ping 外部主機
- requests.get(): ✅ 失敗 (Network unreachable)
- socket.connect(): ✅ 失敗 (No route to host)
```

#### 📝 備註
- [ ] Layer 1 & 2 安全配置完成
- [ ] Network 隔離驗證通過
- [ ] 明日重點: Layer 3 & 4 + 完整安全測試

---

### 📅 2026-02-04 (Tuesday) - Layer 3 & 4: File System 與 Resource 隔離

#### ✅ 完成任務
- [ ] **Layer 3: File System 隔離配置**
  - /tmp 目錄限制:
    ```csharp
    Tmpfs = new Dictionary<string, string>
    {
        { "/tmp", "rw,size=100m,mode=1777,noexec,nosuid,nodev" }
    }
    ```
  - 禁止存取敏感目錄:
    - 驗證無法讀取 /etc/passwd, /proc, /sys
  - 文件數量限制:
    ```bash
    ulimit -n 1024 # 最多 1024 個檔案描述符
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **Layer 4: Resource 限制配置**
  - CPU 限制:
    ```csharp
    NanoCPUs = 1_000_000_000, // 1 CPU core
    CPUQuota = 100_000,       // 100% of 1 core
    CPUPeriod = 100_000
    ```
  - Memory 限制:
    ```csharp
    Memory = 512 * 1024 * 1024,      // 512MB
    MemorySwap = 512 * 1024 * 1024,  // 無 Swap
    MemoryReservation = 256 * 1024 * 1024
    ```
  - Process 限制:
    ```csharp
    PidsLimit = 10 // 最多 10 個 process
    ```
  - Disk I/O 限制:
    ```csharp
    BlkioWeight = 500 // 中等優先級
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **SecurityManager 實作**
  - 建立 `src/AIAgentPlatform.Infrastructure/Services/SecurityManager.cs`
  - 整合所有 4-Layer Security 配置
  - 驗證安全策略有效性
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-057: /tmp 執行權限**
  - 決策: `noexec` 禁止在 /tmp 執行二進制檔案
  - 理由: 防止下載並執行惡意程式
  - 影響: Python .pyc 檔案仍可執行（Python 解釋器執行）
  - 測試: 驗證無法執行 `chmod +x /tmp/script.sh && /tmp/script.sh`

- [ ] **TD-058: Memory Swap 配置**
  - 決策: MemorySwap = Memory (無額外 Swap)
  - 理由: 避免 OOM Killer 影響其他容器
  - 影響: Memory 耗盡時容器會被 Kill（預期行為）
  - OOM Handling: 返回 ExecutionStatus.MemoryExceeded

#### 🔐 安全測試結果
```
Layer 3 測試:
- /tmp 寫入限制 (100MB): ✅ 超過限制時失敗
- /tmp noexec: ✅ 無法執行 binary
- 讀取 /etc/passwd: ✅ 失敗 (Permission denied)
- 讀取 /proc/self: ✅ 失敗 (Permission denied)

Layer 4 測試:
- CPU 限制 (1 core): ✅ CPU 使用不超過 100%
- Memory 限制 (512MB): ✅ 超過時 OOM Kill
- Process 限制 (10): ✅ 無法 fork 超過 10 個 process
- Disk I/O 限制: ✅ 正常
```

#### 📝 備註
- [ ] 4-Layer Security Architecture 全部配置完成
- [ ] 所有安全測試初步驗證通過
- [ ] 明日重點: 完整滲透測試（最高優先級）

---

### 📅 2026-02-05 (Wednesday) - 安全滲透測試（最高優先級）

#### ✅ 完成任務
- [ ] **滲透測試計劃執行**
  - Security Team 參與
  - 測試時間: 全日
  - 測試環境: 獨立測試環境（隔離生產）
  - 測試工具: [待填寫 - Metasploit, 自訂腳本]
  - 相關 Commit: `[待填寫]`

- [ ] **容器逃逸測試 (CRITICAL)**
  - 測試 1: 嘗試掛載宿主機目錄
    ```python
    import os
    os.system('mount /dev/sda1 /mnt')  # 應失敗
    ```
  - 測試 2: 嘗試存取 Docker Socket
    ```python
    import socket
    s = socket.socket(socket.AF_UNIX)
    s.connect('/var/run/docker.sock')  # 應失敗（檔案不存在）
    ```
  - 測試 3: 嘗試特權升級
    ```python
    import os
    os.setuid(0)  # 應失敗（無 CAP_SETUID）
    ```
  - 測試 4: 嘗試內核漏洞利用 (Dirty COW, etc.)
  - 測試結果: [待填寫 - Pass/Fail + 詳細分析]
  - 相關 Issue: [待建立 - 如發現漏洞]

- [ ] **Network 攻擊測試 (CRITICAL)**
  - 測試 5: 嘗試外部網路連線
    ```python
    import requests
    requests.get('https://evil.com')  # 應失敗
    ```
  - 測試 6: 嘗試內網掃描
    ```python
    import socket
    for ip in ['192.168.1.1', '10.0.0.1']:
        socket.create_connection((ip, 22), timeout=1)  # 應失敗
    ```
  - 測試 7: 嘗試 DNS 解析外部域名
    ```python
    import socket
    socket.gethostbyname('google.com')  # 應失敗
    ```
  - 測試結果: [待填寫]

- [ ] **Resource 攻擊測試 (HIGH)**
  - 測試 8: CPU 耗盡攻擊
    ```python
    while True:
        pass  # 應被限制在 1 core
    ```
  - 測試 9: Memory 耗盡攻擊
    ```python
    data = []
    while True:
        data.append(' ' * (1024 * 1024))  # 應在 512MB 時 OOM Kill
    ```
  - 測試 10: Disk 填滿攻擊
    ```python
    with open('/tmp/large_file', 'wb') as f:
        f.write(b'0' * (200 * 1024 * 1024))  # 應在 100MB 時失敗
    ```
  - 測試 11: Process Fork 炸彈
    ```python
    import os
    while True:
        os.fork()  # 應在 10 個 process 時失敗
    ```
  - 測試結果: [待填寫]

- [ ] **File System 攻擊測試 (MEDIUM)**
  - 測試 12: 嘗試讀取敏感檔案
    ```python
    open('/etc/shadow', 'r').read()  # 應失敗
    ```
  - 測試 13: 嘗試寫入系統目錄
    ```python
    with open('/usr/bin/backdoor', 'w') as f:
        f.write('malicious code')  # 應失敗（Read-only）
    ```
  - 測試 14: Symlink 攻擊
    ```python
    import os
    os.symlink('/etc/passwd', '/tmp/passwd')
    open('/tmp/passwd').read()  # 應失敗或僅讀取 symlink
    ```
  - 測試結果: [待填寫]

#### 🔐 滲透測試詳細結果

**測試執行時間**: 2026-02-05 09:00-18:00
**測試人員**: [Security Team 成員名單]
**測試環境**: Isolated Test Environment

##### 測試結果總覽
```yaml
總測試項目: 14
通過: [待填寫]
失敗: [待填寫]
Critical 漏洞: [待填寫]
High 漏洞: [待填寫]
Medium 漏洞: [待填寫]
Low 漏洞: [待填寫]
```

##### 發現漏洞詳情
[待填寫 - 如發現漏洞，需詳細記錄]

```markdown
漏洞 #1: [漏洞名稱]
  嚴重程度: Critical/High/Medium/Low
  描述: [詳細描述]
  攻擊向量: [如何利用]
  影響範圍: [影響程度]
  緩解措施: [修復方案]
  修復狀態: [Pending/In Progress/Fixed]
  相關 Issue: [SPRINT-6-6-ISSUES.md Issue ID]
```

#### ⚠️ 遇到問題
[待填寫 - 如測試發現問題，需立即記錄並升級]

#### 📝 備註
- [ ] 滲透測試完成（或進行中）
- [ ] 所有發現漏洞需在 Week 17 結束前修復
- [ ] 如發現 Critical 漏洞，需立即召集團隊會議討論
- [ ] 明日重點: 漏洞修復

---

### 📅 2026-02-06 (Thursday) - 安全漏洞修復（如適用）

#### ✅ 完成任務
[待填寫 - 根據滲透測試結果填寫]

- [ ] **修復漏洞 #1**
  - 問題: [待填寫]
  - 修復方案: [待填寫]
  - 驗證測試: [待填寫]
  - 相關 Commit: `[待填寫]`

- [ ] **修復漏洞 #2**
  - [同上格式]

#### 🔧 技術決策
[待填寫 - 根據修復方案記錄]

#### 🔐 修復驗證測試
[待填寫 - 重新執行失敗的測試項目]

```
修復驗證:
- 漏洞 #1 Re-test: [待填寫]
- 漏洞 #2 Re-test: [待填寫]
```

#### 📝 備註
- [ ] 所有 Critical/High 漏洞必須修復完成
- [ ] Medium 漏洞需評估是否可延遲至 Phase 2
- [ ] 修復完成後需完整回歸測試

---

### 📅 2026-02-07 (Friday) - 安全驗證與回歸測試

#### ✅ 完成任務
- [ ] **完整安全回歸測試**
  - 重新執行所有 14 項滲透測試
  - 驗證所有漏洞已修復
  - 確認無新的安全問題
  - 測試結果: [待填寫]
  - 相關 Commit: `[待填寫]`

- [ ] **安全測試報告撰寫**
  - 測試執行總結
  - 發現漏洞清單
  - 修復方案與驗證
  - Security Team 簽核
  - 報告位於: `claudedocs/2-sprints/sprint-6/security-test-report.md`

- [ ] **ADR 文檔更新**
  - 更新 ADR-008: Code Interpreter Container Pool
  - 記錄安全設計決策
  - 記錄漏洞修復經驗
  - 相關 Commit: `[待填寫]`

#### 🔐 最終安全測試結果
```yaml
容器逃逸測試 (4 項):
  - 全部通過: ✅
  - 無容器逃逸漏洞

Network 攻擊測試 (3 項):
  - 全部通過: ✅
  - Network 隔離有效

Resource 攻擊測試 (4 項):
  - 全部通過: ✅
  - Resource 限制有效

File System 攻擊測試 (3 項):
  - 全部通過: ✅
  - File System 隔離有效

總體評估:
  - 安全等級: [待評估 - Pass/Fail]
  - 是否達到生產就緒: [待評估 - Yes/No]
  - 殘餘風險: [待評估 - Low/Medium/High]
```

#### 📝 備註
- [ ] Week 17 目標達成：Phase 3 完成 + 安全測試通過 (4 SP, 80%)
- [ ] 4-Layer Security Architecture 驗證完成
- [ ] 下週重點：執行結果處理、UI、整合測試

---

## Week 18 開發日誌

> **Week 18 目標**: US 5.1 Phase 4-5 - 執行結果處理 + UI + 整合測試 + Sprint 回顧
> **預期完成**: Phase 4 (0.5 SP) + Phase 5 (0.5 SP) = 1 SP (100%)

### 📅 2026-02-10 (Monday) - 執行結果處理與可視化

#### ✅ 完成任務
- [ ] **IExecutionResultProcessor 介面設計**
  - 建立 `src/AIAgentPlatform.Application/Interfaces/IExecutionResultProcessor.cs`
  - 方法定義:
    ```csharp
    Task<ProcessedResult> ProcessResultAsync(RawExecutionResult raw);
    Task<List<OutputArtifact>> ExtractArtifactsAsync(string containerId);
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **ExecutionResultProcessor 實作**
  - 建立 `src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs`
  - stdout/stderr 解析:
    - 分離 stdout/stderr stream
    - 錯誤訊息格式化（Python Traceback）
    - Exit code 解釋
  - 執行時間統計
  - 相關 Commit: `[待填寫]`

- [ ] **Matplotlib 圖表輸出支援**
  - Python 代碼範例:
    ```python
    import matplotlib.pyplot as plt
    plt.plot([1, 2, 3], [4, 5, 6])
    plt.savefig('/tmp/plot.png')
    ```
  - 圖表檔案擷取:
    - 從容器 /tmp 目錄擷取 .png 檔案
    - Base64 編碼傳輸
    - 上傳至 Azure Blob Storage
  - OutputArtifact 類型: `image/png`
  - 相關 Commit: `[待填寫]`

- [ ] **Pandas DataFrame 輸出支援**
  - Python 代碼範例:
    ```python
    import pandas as pd
    df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
    print(df.to_json())
    ```
  - DataFrame JSON 解析:
    - 從 stdout 解析 JSON
    - 驗證 JSON 格式
    - 轉換為 OutputArtifact
  - OutputArtifact 類型: `application/json`
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-059: 圖表儲存位置**
  - 決策: Azure Blob Storage
  - 理由: 持久化、可分享、支援大檔案
  - Container: `code-execution-artifacts`
  - Lifecycle: 30 天後自動刪除
  - SAS Token: 1 小時有效期

- [ ] **TD-060: 支援的圖表格式**
  - 決策: 僅支援 PNG 格式
  - 理由: 通用性高、瀏覽器原生支援
  - 未來擴展: SVG (Phase 2), PDF (Phase 3)
  - 大小限制: 單一圖表 <10MB

#### 📊 測試結果
```
ExecutionResultProcessor Unit Tests:
- ProcessResultAsync (Success): 1/1 Passed
- ProcessResultAsync (Error): 1/1 Passed
- Python Traceback Parsing: 1/1 Passed
- Coverage: 88%

Artifact Extraction Tests:
- Extract PNG Image: 1/1 Passed
- Extract Multiple Images: 1/1 Passed
- Extract DataFrame JSON: 1/1 Passed
- No Artifacts: 1/1 Passed
- Coverage: 85%

Integration Tests:
- Execute with Matplotlib: Passed (PNG extracted)
- Execute with Pandas: Passed (JSON extracted)
- Execute with Both: Passed (2 artifacts)
```

#### 📝 備註
- [ ] 執行結果處理完成
- [ ] 圖表與表格輸出支援完成
- [ ] 明日重點: Code Execution API

---

### 📅 2026-02-11 (Tuesday) - Code Execution API 實作

#### ✅ 完成任務
- [ ] **CodeController 實作**
  - 建立 `src/AIAgentPlatform.API/Controllers/CodeController.cs`
  - API 端點實作:
    ```csharp
    [HttpPost("execute")]
    public async Task<ActionResult<ExecutionResponse>> ExecuteCode([FromBody] ExecuteCodeRequest request)

    [HttpGet("executions/{id}")]
    public async Task<ActionResult<ExecutionResult>> GetExecutionResult(string id)

    [HttpDelete("executions/{id}")]
    public async Task<ActionResult> DeleteExecution(string id)

    [HttpGet("status")]
    public async Task<ActionResult<ContainerPoolStatus>> GetStatus()
    ```
  - 相關 Commit: `[待填寫]`

- [ ] **CQRS Commands & Queries**
  - 建立 `ExecuteCodeCommand` + Handler
  - 建立 `GetExecutionResultQuery` + Handler
  - 建立 `DeleteExecutionCommand` + Handler
  - FluentValidation 驗證規則:
    - Code 不為空
    - Language = "python"
    - TimeoutSeconds 範圍 1-300 秒
  - 相關 Commit: `[待填寫]`

- [ ] **Swagger 文檔完成**
  - API 端點文檔
  - Request/Response 範例
  - 錯誤代碼說明
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-061: ExecuteCode 同步 vs 非同步**
  - 決策: 同步執行（等待結果返回）
  - 理由: MVP 簡化版，執行時間短 (<30秒)
  - 未來改進: Phase 2 支援非同步執行（長時間運算）
  - 實作: Polling API 已預留（GetExecutionResult）

#### 📊 測試結果
```
Code API Integration Tests:
- POST /api/v1/code/execute (Success): Passed
- POST /api/v1/code/execute (Error): Passed
- POST /api/v1/code/execute (Timeout): Passed
- GET /api/v1/code/executions/{id}: Passed
- DELETE /api/v1/code/executions/{id}: Passed
- GET /api/v1/code/status: Passed

Validation Tests:
- Empty Code: Passed (400 Bad Request)
- Invalid Language: Passed (400 Bad Request)
- Invalid Timeout: Passed (400 Bad Request)
```

#### 📝 備註
- [ ] Code Execution API 完成
- [ ] Swagger 文檔完整
- [ ] 明日重點: Frontend Code Interpreter UI

---

### 📅 2026-02-12 (Wednesday) - Code Interpreter UI 實作

#### ✅ 完成任務
- [ ] **CodeInterpreter 主元件**
  - 建立 `apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx`
  - 元件結構:
    - CodeEditor (代碼編輯器)
    - ControlPanel (執行按鈕、語言選擇、超時設定)
    - ExecutionResult (執行結果展示)
    - ExecutionHistory (執行歷史)
  - 相關 Commit: `[待填寫]`

- [ ] **CodeEditor 元件**
  - 建立 `apps/web-app/src/features/code-interpreter/components/CodeEditor.tsx`
  - 使用 Monaco Editor:
    ```typescript
    import Editor from '@monaco-editor/react';

    <Editor
      height="400px"
      defaultLanguage="python"
      theme="vs-dark"
      value={code}
      onChange={setCode}
    />
    ```
  - Syntax Highlighting: Python
  - Auto-completion: 基礎 Python 關鍵字
  - 相關 Commit: `[待填寫]`

- [ ] **ExecutionResult 元件**
  - 建立 `apps/web-app/src/features/code-interpreter/components/ExecutionResult.tsx`
  - 結果展示:
    - Text Output (stdout/stderr)
    - Error Messages (Traceback)
    - Images (PNG from Matplotlib)
    - Tables (JSON from Pandas)
  - 相關 Commit: `[待填寫]`

- [ ] **codeInterpreterService API 整合**
  - 建立 `apps/web-app/src/services/codeInterpreterService.ts`
  - API 方法:
    ```typescript
    async executeCode(request: ExecuteCodeRequest): Promise<ExecutionResult>
    async getExecutionResult(id: string): Promise<ExecutionResult>
    async deleteExecution(id: string): Promise<void>
    async getStatus(): Promise<ContainerPoolStatus>
    ```
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-062: 代碼編輯器選擇**
  - 決策: Monaco Editor
  - 理由: VS Code 使用、功能強大、React 整合良好
  - 替代方案: CodeMirror (過於輕量)
  - Bundle Size: +2MB (可接受)

- [ ] **TD-063: 執行狀態 UI 設計**
  - 決策: 執行中顯示 Spinner + 進度條 (模擬)
  - 理由: 提升使用者體驗、避免等待焦慮
  - 進度估算: 基於已執行時間（非真實進度）
  - 超時提示: 接近 30 秒時顯示警告

#### 📊 測試結果
```
Frontend Component Tests (Vitest + Testing Library):
- CodeInterpreter Render: Passed
- CodeEditor Input: Passed
- Execute Button Click: Passed
- ExecutionResult Display (Text): Passed
- ExecutionResult Display (Image): Passed
- ExecutionResult Display (Error): Passed
- Coverage: 82%

E2E Tests (Playwright):
- Execute Simple Code: Passed
- Execute with Matplotlib: Passed (Image displayed)
- Execute with Error: Passed (Error displayed)
- Execute Timeout: Passed (Timeout message)
```

#### 📝 備註
- [ ] Code Interpreter UI 基本功能完成
- [ ] Monaco Editor 整合成功
- [ ] 明日重點: 單元測試與整合測試

---

### 📅 2026-02-13 (Thursday) - 單元測試與整合測試

#### ✅ 完成任務
- [ ] **Backend 單元測試補完**
  - DockerCodeExecutionService Tests
  - ContainerPoolManager Tests
  - ExecutionResultProcessor Tests
  - SecurityManager Tests
  - 測試覆蓋率: [待填寫] (Target: ≥85%)
  - 相關 Commit: `[待填寫]`

- [ ] **Backend 整合測試**
  - E2E 執行流程測試 (10 scenarios)
  - 並發執行測試 (3 concurrent)
  - 安全測試回歸 (14 scenarios)
  - 效能測試 (100 executions)
  - 相關 Commit: `[待填寫]`

- [ ] **Frontend 單元測試補完**
  - CodeInterpreter Component Tests
  - CodeEditor Component Tests
  - ExecutionResult Component Tests
  - codeInterpreterService Tests
  - 測試覆蓋率: [待填寫] (Target: ≥85%)
  - 相關 Commit: `[待填寫]`

#### 📊 最終測試結果

##### Backend 測試
```yaml
Unit Tests:
  - Total: [待填寫]
  - Passed: [待填寫]
  - Failed: [待填寫]
  - Coverage: [待填寫]% (Target: ≥85%)

Integration Tests:
  - E2E Tests: [待填寫]/10 Passed
  - Concurrent Tests: [待填寫]/3 Passed
  - Security Tests: [待填寫]/14 Passed
  - Performance Tests: Passed

Performance Metrics:
  - 執行成功率: [待填寫]% (Target: ≥90%)
  - 平均執行時間: [待填寫]ms (Target: <30s)
  - P95 執行時間: [待填寫]ms (Target: <30s)
  - 容器啟動時間: [待填寫]ms (Target: <5s)
```

##### Frontend 測試
```yaml
Unit Tests:
  - Total: [待填寫]
  - Passed: [待填寫]
  - Failed: [待填寫]
  - Coverage: [待填寫]% (Target: ≥85%)

E2E Tests (Playwright):
  - Total: [待填寫]
  - Passed: [待填寫]
  - Failed: [待填寫]
```

#### 📝 備註
- [ ] 所有測試執行完成
- [ ] 測試覆蓋率達標（或接近）
- [ ] 明日重點: Sprint Review 準備 + Documentation

---

### 📅 2026-02-14 (Friday) - Documentation 與 Sprint Review

#### ✅ 完成任務
- [ ] **ADR 文檔完成**
  - ADR-008: Code Interpreter Container Pool ✅ (已更新)
  - ADR-015: Execution Timeout Strategy ✅
  - ADR-016: Resource Limit Configuration ✅

- [ ] **Technical Implementation Guides 完成**
  - Code Interpreter Implementation Guide ✅
  - 4-Layer Security Architecture Guide ✅
  - Container Pool Management Guide ✅

- [ ] **API Documentation 完成**
  - Swagger 文檔完整 ✅
  - API Design Document 完成 ✅
  - Request/Response 範例完整 ✅

- [ ] **Security Test Report 完成**
  - 滲透測試報告 ✅
  - 漏洞修復記錄 ✅
  - Security Team 簽核 ✅

- [ ] **Sprint Review 會議**
  - 時間: 14:00-16:00
  - 參與者: 全團隊 + Stakeholders + Security Team
  - Demo: Code Interpreter 執行 + 安全展示
  - 成果: [待填寫 - Stakeholders 反饋]
  - 後續: Phase 1A 完成，進入 Phase 1B

#### 📝 備註
- [ ] Sprint 6 成功完成！
- [ ] Definition of Done 全部達成 ✅
- [ ] 安全測試通過，無 Critical 漏洞 ✅
- [ ] Phase 1A (基礎平台) 重要里程碑達成 🎉
- [ ] 下週: Sprint Retrospective

---

### 📅 2026-02-17 (Monday) - Sprint Retrospective

#### Sprint 6 回顧總結

**✅ 完成成果**:
- US 5.1: Code Interpreter - Python 執行引擎 (5 SP) ✅
- 總計: 5 SP 全部完成

**📊 品質指標**:
- 單元測試覆蓋率: [待填寫]% (Target: ≥85%) ✅
- 整合測試通過率: [待填寫]% (Target: 100%) ✅
- 執行成功率: [待填寫]% (Target: ≥90%) ✅
- 執行時間 P95: [待填寫]ms (Target: <30s) ✅
- **安全測試通過率: 100%** ✅✅
- **無容器逃逸漏洞** ✅✅
- Code Review: 0 Critical/High Issues ✅

**🎯 Went Well**:
- 4-Layer Security Architecture 設計完善，滲透測試全數通過
- 容器池管理穩定可靠，健康檢查機制運作良好
- Docker.DotNet 整合順利，執行引擎效能達標
- Security Team 積極參與，安全測試執行嚴謹
- Team 協作順暢，溝通良好

**⚠️ Needs Improvement**:
- Docker Image 大小優化仍有空間 (實際 [待填寫]MB vs 目標 500MB)
- 執行超時機制實作較複雜 (Docker.DotNet API 限制)
- Monaco Editor Bundle Size 較大 (+2MB)
- 滲透測試發現 [待填寫] 個漏洞，修復耗時較長

**🚀 Action Items (Phase 2)**:
- 實作動態容器擴縮容（5-10 個容器）
- 支援非同步執行（長時間運算）
- 增加更多 Python 套件（scipy, scikit-learn）
- 實作完整容器重置（每 10 次使用後）
- 研究更安全的容器 Runtime (gVisor, Kata Containers)
- 持續安全監控與漏洞掃描

**🎉 Phase 1A 完成慶祝**:
- Sprint 6 是 Phase 1A (基礎平台) 最後一個 Sprint
- 成功完成 Agent 管理、Plugin 系統、Persona Framework、Knowledge 管理、Code Interpreter
- 所有核心基礎功能就緒，達到 MVP 里程碑
- 團隊展現出色的技術能力與安全意識
- 為 Phase 1B (工作流引擎核心) 奠定堅實基礎

---

## 📋 技術決策記錄

### TD-044: Docker Base Image 選擇
- **日期**: 2026-01-27
- **決策**: 使用 `python:3.11-slim-bookworm`
- **理由**: 官方 Image、輕量級、安全更新及時
- **影響**: Image 大小 <500MB (含預裝套件)
- **參考**: ADR-008 Code Interpreter Container Pool

### TD-045: 容器數量決策
- **日期**: 2026-01-27
- **決策**: 固定 3 個容器（MVP 簡化版）
- **理由**: 平衡並發需求與資源消耗，降低複雜度
- **影響**: 最多支援 3 個同時執行
- **未來擴展**: Phase 2 實作動態擴縮容（5-10 個容器）

### TD-046: 套件版本鎖定策略
- **日期**: 2026-01-28
- **決策**: 鎖定主要套件版本（numpy, pandas, matplotlib, seaborn）
- **理由**: 確保執行環境一致性，避免套件升級導致 Breaking Changes
- **影響**: 每月安全更新一次（Security Patch Only）

### TD-047: Read-only 文件系統配置
- **日期**: 2026-01-28
- **決策**: 整個容器文件系統 Read-only，僅 /tmp 可寫
- **理由**: 防止惡意代碼修改系統檔案、持久化後門
- **影響**: Python pip cache 需要可寫目錄
- **實作**: `--read-only --tmpfs /tmp:rw,size=100m`

### TD-048: 容器分配策略
- **日期**: 2026-01-29
- **決策**: 使用 Semaphore + FIFO Queue
- **理由**: 簡單可靠、公平分配、避免餓死
- **影響**: 分配超時 30 秒

### TD-049: 容器重置策略
- **日期**: 2026-01-29
- **決策**: 釋放時清除 /tmp 檔案，不重啟容器
- **理由**: 提升效能、減少容器啟動時間
- **影響**: 可能殘留 Python 全局變數（風險可控）

### TD-050: 健康檢查頻率
- **日期**: 2026-01-29
- **決策**: 每 30 秒檢查一次
- **理由**: 平衡檢測及時性與系統負載
- **影響**: 異常閾值 3 次連續失敗才重啟

### TD-051: Docker 連接方式
- **日期**: 2026-01-30
- **決策**: Unix Socket (`unix:///var/run/docker.sock`)
- **理由**: Linux 環境標準方式、最高效能
- **影響**: Windows 需使用 Named Pipe

### TD-052: 命令執行超時機制
- **日期**: 2026-01-30
- **決策**: 使用 CancellationTokenSource with Timeout
- **理由**: .NET 原生支援、可靠、易於測試
- **影響**: 超時處理需 Kill Process + Stop Container

### TD-053: 代碼傳遞方式
- **日期**: 2026-01-31
- **決策**: 寫入容器 /tmp/code.py 檔案
- **理由**: 簡單可靠、支援多行代碼、易於除錯
- **影響**: /tmp 目錄配額限制 100MB

### TD-054: 執行結果儲存策略
- **日期**: 2026-01-31
- **決策**: PostgreSQL 儲存執行記錄，Redis Cache 近期結果（1 小時）
- **理由**: 持久化歷史記錄、快速查詢近期結果
- **影響**: 執行記錄保留 30 天，Artifacts 儲存於 Azure Blob Storage

### TD-055: Network 隔離強度
- **日期**: 2026-02-03
- **決策**: 完全禁用 Network (NetworkMode = "none")
- **理由**: 最安全、無任何網路攻擊向量
- **影響**: 無法使用 pip install、無法存取外部 API

### TD-056: Capabilities 最小化
- **日期**: 2026-02-03
- **決策**: Drop ALL Capabilities，無額外授予
- **理由**: 最小權限原則、降低攻擊面
- **影響**: 無法執行特權操作（如 chroot, setuid）

### TD-057: /tmp 執行權限
- **日期**: 2026-02-04
- **決策**: `noexec` 禁止在 /tmp 執行二進制檔案
- **理由**: 防止下載並執行惡意程式
- **影響**: Python .pyc 檔案仍可執行（Python 解釋器執行）

### TD-058: Memory Swap 配置
- **日期**: 2026-02-04
- **決策**: MemorySwap = Memory (無額外 Swap)
- **理由**: 避免 OOM Killer 影響其他容器
- **影響**: Memory 耗盡時容器會被 Kill（預期行為）

### TD-059: 圖表儲存位置
- **日期**: 2026-02-10
- **決策**: Azure Blob Storage
- **理由**: 持久化、可分享、支援大檔案
- **影響**: Container `code-execution-artifacts`, Lifecycle 30 天

### TD-060: 支援的圖表格式
- **日期**: 2026-02-10
- **決策**: 僅支援 PNG 格式
- **理由**: 通用性高、瀏覽器原生支援
- **影響**: 大小限制 <10MB

### TD-061: ExecuteCode 同步 vs 非同步
- **日期**: 2026-02-11
- **決策**: 同步執行（等待結果返回）
- **理由**: MVP 簡化版，執行時間短 (<30秒)
- **未來改進**: Phase 2 支援非同步執行（長時間運算）

### TD-062: 代碼編輯器選擇
- **日期**: 2026-02-12
- **決策**: Monaco Editor
- **理由**: VS Code 使用、功能強大、React 整合良好
- **影響**: Bundle Size +2MB

### TD-063: 執行狀態 UI 設計
- **日期**: 2026-02-12
- **決策**: 執行中顯示 Spinner + 進度條 (模擬)
- **理由**: 提升使用者體驗、避免等待焦慮
- **影響**: 進度估算基於已執行時間（非真實進度）

---

## ⚠️ 問題與解決方案

### Issue #1: Docker Image 大小超過預期
- **日期**: 2026-01-28
- **問題**: Docker Image 大小超過 500MB 目標
- **影響**: 開發環境下載時間長、儲存空間消耗大
- **解決方案**:
  - 清理 apt cache: `apt-get clean && rm -rf /var/lib/apt/lists/*`
  - 清理 pip cache: `pip install --no-cache-dir`
  - 移除不必要的開發工具
  - 最終大小: [待填寫]MB
- **狀態**: ✅ 已解決

### Issue #2: Docker.DotNet ExecStartAsync 無法設定超時
- **日期**: 2026-01-30
- **問題**: Docker.DotNet API 無法直接設定 Exec 超時
- **影響**: 無法精確控制執行超時
- **解決方案**: 使用 `Task.WhenAny` + `Task.Delay` 實現超時
- **程式碼範例**:
  ```csharp
  var execTask = _dockerClient.Exec.StartAndAttachContainerExecAsync(execId, false);
  var timeoutTask = Task.Delay(TimeSpan.FromSeconds(timeoutSeconds));

  var completedTask = await Task.WhenAny(execTask, timeoutTask);
  if (completedTask == timeoutTask)
  {
      // Timeout: Kill exec and stop container
      await _dockerClient.Containers.StopContainerAsync(containerId, new ContainerStopParameters());
      throw new ExecutionTimeoutException();
  }
  ```
- **狀態**: ✅ 已解決

### Issue #3: 滲透測試發現漏洞（如適用）
- **日期**: 2026-02-05
- **問題**: [待填寫 - 根據實際滲透測試結果]
- **影響**: [待填寫]
- **解決方案**: [待填寫]
- **狀態**: [待填寫 - Pending/Fixed]
- **參考**: [SPRINT-6-6-ISSUES.md Issue ID]

### Issue #4: 容器健康檢查誤判
- **日期**: [待填寫]
- **問題**: [待填寫 - 如發生]
- **影響**: 正常容器被誤判為異常並重啟
- **解決方案**: 增加異常閾值（3 次連續失敗）+ Retry with Backoff
- **狀態**: [待填寫]

---

## 🔐 安全測試記錄

### 滲透測試執行總覽

**測試日期**: 2026-02-05
**測試時間**: 09:00-18:00
**測試環境**: Isolated Test Environment
**測試人員**: [Security Team 成員名單]
**測試工具**: [Metasploit, 自訂腳本, 等]

### 測試項目詳情

#### 容器逃逸測試 (CRITICAL)

**測試 1: 嘗試掛載宿主機目錄**
```yaml
測試代碼:
  import os
  os.system('mount /dev/sda1 /mnt')

預期結果: 失敗 (Permission denied)
實際結果: [待填寫]
嚴重程度: Critical
狀態: [待填寫 - Pass/Fail]
```

**測試 2: 嘗試存取 Docker Socket**
```yaml
測試代碼:
  import socket
  s = socket.socket(socket.AF_UNIX)
  s.connect('/var/run/docker.sock')

預期結果: 失敗 (File not found 或 Permission denied)
實際結果: [待填寫]
嚴重程度: Critical
狀態: [待填寫]
```

**測試 3: 嘗試特權升級**
```yaml
測試代碼:
  import os
  os.setuid(0)

預期結果: 失敗 (Operation not permitted - 無 CAP_SETUID)
實際結果: [待填寫]
嚴重程度: Critical
狀態: [待填寫]
```

**測試 4: 內核漏洞利用 (Dirty COW, etc.)**
```yaml
測試方法: [待填寫 - 使用 Metasploit 或自訂 Exploit]
預期結果: 失敗 (內核漏洞已修補或容器隔離有效)
實際結果: [待填寫]
嚴重程度: Critical
狀態: [待填寫]
```

#### Network 攻擊測試 (CRITICAL)

**測試 5: 嘗試外部網路連線**
```yaml
測試代碼:
  import requests
  requests.get('https://evil.com')

預期結果: 失敗 (Network unreachable)
實際結果: [待填寫]
嚴重程度: Critical
狀態: [待填寫]
```

**測試 6: 嘗試內網掃描**
```yaml
測試代碼:
  import socket
  for ip in ['192.168.1.1', '10.0.0.1']:
      socket.create_connection((ip, 22), timeout=1)

預期結果: 失敗 (Network unreachable)
實際結果: [待填寫]
嚴重程度: Critical
狀態: [待填寫]
```

**測試 7: 嘗試 DNS 解析**
```yaml
測試代碼:
  import socket
  socket.gethostbyname('google.com')

預期結果: 失敗 (Name resolution failed)
實際結果: [待填寫]
嚴重程度: High
狀態: [待填寫]
```

#### Resource 攻擊測試 (HIGH)

**測試 8: CPU 耗盡攻擊**
```yaml
測試代碼:
  while True:
      pass

預期結果: CPU 使用率限制在 100% (1 core)
實際結果: [待填寫]
嚴重程度: High
狀態: [待填寫]
```

**測試 9: Memory 耗盡攻擊**
```yaml
測試代碼:
  data = []
  while True:
      data.append(' ' * (1024 * 1024))

預期結果: 在 512MB 時 OOM Kill
實際結果: [待填寫]
嚴重程度: High
狀態: [待填寫]
```

**測試 10: Disk 填滿攻擊**
```yaml
測試代碼:
  with open('/tmp/large_file', 'wb') as f:
      f.write(b'0' * (200 * 1024 * 1024))

預期結果: 在 100MB 時失敗 (Disk quota exceeded)
實際結果: [待填寫]
嚴重程度: High
狀態: [待填寫]
```

**測試 11: Process Fork 炸彈**
```yaml
測試代碼:
  import os
  while True:
      os.fork()

預期結果: 在 10 個 process 時失敗
實際結果: [待填寫]
嚴重程度: High
狀態: [待填寫]
```

#### File System 攻擊測試 (MEDIUM)

**測試 12: 嘗試讀取敏感檔案**
```yaml
測試代碼:
  open('/etc/shadow', 'r').read()

預期結果: 失敗 (Permission denied)
實際結果: [待填寫]
嚴重程度: Medium
狀態: [待填寫]
```

**測試 13: 嘗試寫入系統目錄**
```yaml
測試代碼:
  with open('/usr/bin/backdoor', 'w') as f:
      f.write('malicious code')

預期結果: 失敗 (Read-only file system)
實際結果: [待填寫]
嚴重程度: Medium
狀態: [待填寫]
```

**測試 14: Symlink 攻擊**
```yaml
測試代碼:
  import os
  os.symlink('/etc/passwd', '/tmp/passwd')
  open('/tmp/passwd').read()

預期結果: 失敗或僅讀取 symlink
實際結果: [待填寫]
嚴重程度: Medium
狀態: [待填寫]
```

### 安全測試總結

```yaml
測試項目總數: 14
通過測試: [待填寫]
失敗測試: [待填寫]

漏洞嚴重程度分佈:
  Critical: [待填寫]
  High: [待填寫]
  Medium: [待填寫]
  Low: [待填寫]

整體安全評級: [待填寫 - Pass/Conditional Pass/Fail]
生產就緒評估: [待填寫 - Ready/Not Ready/Conditional Ready]
```

### 漏洞修復記錄

[待填寫 - 如發現漏洞，需詳細記錄修復過程]

**漏洞 #1**: [漏洞名稱]
```yaml
發現日期: 2026-02-05
嚴重程度: Critical/High/Medium/Low
描述: [詳細描述]
影響: [影響範圍]
修復方案: [修復方法]
修復日期: 2026-02-06
驗證結果: [Re-test 結果]
狀態: ✅ Fixed
相關 Commit: [commit-hash]
```

---

## 💡 經驗總結

### 技術亮點
1. **4-Layer Security Architecture 設計**
   - Layer 1-4 全方位防護
   - 滲透測試驗證有效性
   - 無容器逃逸漏洞

2. **容器池管理設計**
   - 固定 3 個容器穩定可靠
   - 健康檢查機制運作良好
   - Semaphore + FIFO Queue 簡單高效

3. **Docker.DotNet 整合**
   - 克服 API 限制（超時機制）
   - 執行引擎效能達標
   - 資源限制精確控制

4. **執行結果處理**
   - 支援多種輸出格式（Text, Image, Table）
   - Matplotlib/Pandas 整合順利
   - Artifact 管理完善

### 最佳實踐
1. **安全設計優先**
   - Security Team 全程參與
   - 滲透測試嚴謹執行
   - 漏洞修復及時有效

2. **Docker 容器安全**
   - 最小權限原則（Drop ALL Capabilities）
   - 完全網路隔離（NetworkMode = "none"）
   - Read-only 文件系統（僅 /tmp 可寫）

3. **測試驅動開發**
   - 單元測試覆蓋率 [待填寫]% (Target: ≥85%)
   - 整合測試涵蓋所有 E2E Scenarios
   - 安全測試 100% 覆蓋所有攻擊向量

4. **效能優化**
   - Docker Image 大小優化 (<500MB)
   - 容器復用（不重啟）提升效能
   - 健康檢查頻率優化（30 秒）

### 待改進項目
1. **Docker Image 大小**
   - 目標: <500MB
   - 實際: [待填寫]MB
   - 改進: 進一步優化套件、使用 Multi-stage Build

2. **執行超時機制**
   - Docker.DotNet API 限制較多
   - 需要自行實作超時邏輯
   - 考慮貢獻 PR 給 Docker.DotNet

3. **非同步執行支援**
   - MVP 僅支援同步執行
   - 長時間運算需求未滿足
   - Phase 2 實作完整非同步機制

4. **容器擴縮容**
   - MVP 固定 3 個容器
   - 無法應對突發負載
   - Phase 2 實作動態擴縮容（5-10 個容器）

5. **更多 Python 套件支援**
   - MVP 僅支援 numpy, pandas, matplotlib, seaborn
   - 缺少 scipy, scikit-learn, tensorflow
   - Phase 2 評估增加更多套件（考量 Image 大小）

### 安全經驗總結
1. **容器安全不容忽視**
   - 容器逃逸漏洞影響巨大
   - 4-Layer 防護缺一不可
   - 定期安全掃描與滲透測試

2. **最小權限原則**
   - Drop ALL Capabilities 有效降低攻擊面
   - 非 root 用戶執行必要
   - Read-only 文件系統防護有效

3. **Network 隔離最關鍵**
   - 完全禁用 Network 最安全
   - 避免側信道攻擊
   - 無需擔心外部惡意連線

4. **Resource 限制必要性**
   - 防止 DoS 攻擊
   - 保護宿主機資源
   - 確保多租戶公平性

---

## 📚 參考文檔

### Sprint 6 核心文檔
1. [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md) - Sprint 6 概覽
2. [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) - Sprint 6 實施計劃
3. [SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md) - Sprint 6 技術上下文
4. [SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md) - Sprint 6 任務清單
5. [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md) - Sprint 6 問題追蹤
6. [SPRINT-6-7-RETROSPECTIVE.md](./SPRINT-6-7-RETROSPECTIVE.md) - Sprint 6 回顧

### User Story 規格
7. [us-6-code-interpreter.md](../../docs/user-stories/us-6-code-interpreter.md) - Code Interpreter 完整需求 (US 5.1)

### Architecture Design Documents
8. [ADR-008: Code Interpreter Container Pool](../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md) - Docker 容器池設計
9. [ADR-015: Execution Timeout Strategy](../../docs/architecture/decisions/ADR-015-execution-timeout-strategy.md) - 執行超時策略
10. [ADR-016: Resource Limit Configuration](../../docs/architecture/decisions/ADR-016-resource-limit-configuration.md) - 資源限制配置

### Technical Implementation Documents
11. [06-code-interpreter-implementation.md](../../docs/technical-implementation/01-backend-net9/06-code-interpreter-implementation.md) - Code Interpreter 完整實作指南
12. [code-interpreter-architecture.md](../../docs/technical-implementation/code-interpreter/code-interpreter-architecture.md) - 架構設計
13. [container-pool-management.md](../../docs/technical-implementation/code-interpreter/container-pool-management.md) - 容器池管理
14. [docker-image-design.md](../../docs/technical-implementation/code-interpreter/docker-image-design.md) - Docker Image 設計
15. [execution-engine.md](../../docs/technical-implementation/code-interpreter/execution-engine.md) - 執行引擎實作
16. [result-processing.md](../../docs/technical-implementation/code-interpreter/result-processing.md) - 結果處理

### Security Documents
17. [code-execution-security.md](../../docs/security/code-execution-security.md) - 4-Layer Security Architecture
18. [container-security.md](../../docs/security/container-security.md) - 容器安全設計
19. [network-isolation.md](../../docs/security/network-isolation.md) - 網路隔離設計
20. [filesystem-isolation.md](../../docs/security/filesystem-isolation.md) - 文件系統隔離
21. [resource-limits.md](../../docs/security/resource-limits.md) - 資源限制設計

### API & Database Design
22. [code-interpreter-api-design.md](../../docs/api/code-interpreter-api-design.md) - Code Interpreter API 設計
23. [code-execution-entity-design.md](../../docs/database/code-execution-entity-design.md) - CodeExecution Entity 設計

### Testing Documentation
24. [security-testing.md](../../docs/testing/security-testing.md) - 安全測試指南
25. [code-interpreter-testing-strategy.md](../../docs/testing/code-interpreter-testing-strategy.md) - Code Interpreter 測試策略
26. [unit-testing-guidelines.md](../../docs/testing/unit-testing-guidelines.md) - 單元測試指南
27. [integration-testing-guidelines.md](../../docs/testing/integration-testing-guidelines.md) - 整合測試指南

### External Resources
28. [Docker Documentation](https://docs.docker.com/)
29. [Docker.DotNet GitHub](https://github.com/dotnet/Docker.DotNet)
30. [Python 3.11 Documentation](https://docs.python.org/3.11/)
31. [Docker Security Best Practices](https://docs.docker.com/develop/security-best-practices/)
32. [OWASP Container Security](https://owasp.org/www-project-docker-top-10/)
33. [CIS Docker Benchmark](https://www.cisecurity.org/benchmark/docker)

---

## 📝 版本歷史

| 版本 | 日期 | 作者 | 變更說明 |
|------|------|------|----------|
| v2.1 | 2025-11-14 | AI Assistant | 初始版本建立，Sprint 6 開發日誌模板 |
|      |            |              | - 包含 Week 16-18 日誌模板（18 個工作日） |
|      |            |              | - 包含技術決策記錄 (TD-044 至 TD-063) |
|      |            |              | - 包含問題與解決方案 (4+ Issues) |
|      |            |              | - 包含安全測試記錄（14 項滲透測試詳情） |
|      |            |              | - 包含經驗總結（技術亮點、最佳實踐、待改進項目、安全經驗） |
|      |            |              | - 遵循 v2.1 標準 (8 欄位 header, 完整 ToC, 參考索引) |
|      |            |              | - 特別強調安全測試與漏洞修復 |
| v2.2 | 2026-01-27 | [Name] | Sprint 6 執行開始，Week 16 日誌更新 |
| v2.3 | 2026-02-03 | [Name] | Week 17 日誌更新 + 安全測試記錄 |
| v2.4 | 2026-02-05 | Security Team | 滲透測試結果詳細記錄 |
| v2.5 | 2026-02-06 | [Name] | 安全漏洞修復記錄 |
| v2.6 | 2026-02-10 | [Name] | Week 18 日誌更新 |
| v2.7 | 2026-02-17 | [Name] | Sprint Retrospective 完成 |

---

**文件結束** - Sprint 6 開發日誌已完整建立 ✅

**注意**: 本文件為模板，將在 Sprint 6 執行期間每日更新。請開發團隊於每日站會後更新相應日期的任務完成狀態、技術決策、遇到的問題等資訊。

**特別提醒**: Sprint 6 涉及 Code Interpreter 安全執行環境，**安全測試與漏洞修復為本 Sprint 最高優先級**。Week 17 的滲透測試結果必須詳細記錄，所有發現的 Critical/High 漏洞必須在 Sprint 結束前完全修復。
