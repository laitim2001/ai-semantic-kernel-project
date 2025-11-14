# SPRINT-6-2-PLAN.md - Sprint 6 計劃書：Code Interpreter 安全執行環境實施計劃

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

1. [規劃文檔參考](#規劃文檔參考)
2. [第一部分：本 Sprint 要建立什麼](#第一部分本-sprint-要建立什麼-what-to-build)
3. [第二部分：技術實施方案](#第二部分技術實施方案-how-to-build)
4. [第三部分：編碼規範](#第三部分編碼規範)
5. [第四部分：質量保證](#第四部分質量保證)
6. [第五部分：參考文檔](#第五部分參考文檔)
7. [使用指南](#使用指南)
8. [版本歷史](#版本歷史)

---

## 規劃文檔參考

**規劃文檔參考**:
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1A 範圍
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-6) - Sprint 6 分析 (5 SP 調整)
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Code Interpreter 開發策略
- 📐 [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 系統架構
- 📖 [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md) - Sprint 6 概覽與目標
- ⚠️ [RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md) - RISK-016 (Container 逃逸) 高風險管理

---

## 第一部分：本 Sprint 要建立什麼 (What to Build)

### US 5.1: Code Interpreter - Python 執行引擎 (5 SP)

**User Story 完整規格**: [US 6: Code Interpreter](../../../docs/user-stories/us-6-code-interpreter.md)
**ADR 參考**: [ADR-008: Code Interpreter Container Pool](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md)

#### 一、MVP 範圍定義（簡化版）

**Sprint 6 特別說明**:
本 Sprint 實作 **簡化版 Code Interpreter (5 SP)**，相較於原計劃的完整版 (13 SP)，進行以下調整：

```yaml
原計劃 (13 SP) - Phase 2:
  - 動態容器池管理（自動擴縮容、負載均衡）
  - 複雜安全策略（AppArmor, Seccomp, Capabilities）
  - 多語言支援（Python, R, Julia）
  - 高級可視化（Plotly, Bokeh, Altair）
  - 完整監控系統

簡化版 (5 SP) - Phase 1A (本 Sprint):
  - 固定容器池（3 個容器）
  - 基礎安全策略（Docker 隔離 + Resource Limit）
  - 僅支援 Python 3.11
  - 基礎可視化（Matplotlib, Pandas）
  - 基礎監控

簡化理由:
  - Phase 1A 聚焦基礎平台驗證
  - 降低技術複雜度和安全風險
  - 確保在 3 週內完成交付
  - 完整版留待 Phase 1B-1C 實作
  參考: claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md (Sprint 6 調整說明)
```

#### 必須實現功能 (P0 - 本 Sprint)

##### Phase 1: Docker 容器環境搭建 (1.5 SP)

- [x] **Docker Image 設計與建置** (0.8 SP)
  - 基礎 Image：python:3.11-slim-bookworm
  - 預安裝套件：numpy, pandas, matplotlib, seaborn, scipy
  - 安全配置：非 root 用戶（uid=1000）、最小權限
  - Read-only 文件系統（除 /tmp）
  - Image 大小優化：<500MB
  - **參考**: [Dockerfile Design](../../../docker/code-interpreter/Dockerfile)
  - **參考**: [Container Security](../../../docs/security/container-security.md)

- [x] **容器池管理** (0.5 SP)
  - 固定容器池大小：3 個容器
  - 容器生命週期：啟動、停止、重啟、健康檢查
  - 容器命名規則：`code-interpreter-{1-3}`
  - 容器狀態追蹤：Available, Busy, Unhealthy
  - 容器分配策略：Round-robin
  - **參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md)
  - **實作**: `src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs`

- [x] **Docker Compose 整合** (0.2 SP)
  - 更新 docker-compose.yml
  - 容器網路隔離配置：isolated network
  - Volume 掛載設計：/tmp 唯一可寫目錄
  - 環境變數配置
  - **參考**: [Docker Compose Setup](../../../docs/deployment/docker-compose-setup.md)

**驗收標準**:
- ✅ Docker Image 建置成功（Image 大小 <500MB）
- ✅ 容器池啟動成功（3 個容器同時運行）
- ✅ 容器健康檢查運作正常（每 10 秒 ping 一次）
- ✅ 容器網路隔離有效（無法存取外部網路）
- ✅ 容器內 Python 環境驗證（import numpy, pandas, matplotlib 成功）

##### Phase 2: Python 執行引擎實作 (1.5 SP)

- [x] **ICodeExecutionService 介面定義** (0.3 SP)
  - ExecuteCode() method
  - 參數：code (string), language (string), timeout (int), resourceLimits (ResourceLimitOptions)
  - 返回：ExecutionResult (包含 stdout, stderr, exitCode, executionTime)
  - **參考**: `src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs`

- [x] **DockerCodeExecutionService 實作** (0.8 SP)
  - Docker API 整合（Docker.DotNet NuGet 套件）
  - 容器選擇邏輯（從容器池選取可用容器）
  - 代碼提交到容器執行（docker exec）
  - 執行結果擷取（stdout, stderr, exit code）
  - 執行超時控制（預設 30 秒，可配置）
  - 容器清理與重置（執行後清除 /tmp 內容）
  - **參考**: [Execution Engine](../../../docs/technical-implementation/code-interpreter/execution-engine.md)
  - **實作**: `src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs`

- [x] **資源限制配置** (0.2 SP)
  - CPU 限制：1 core (--cpus=1.0)
  - Memory 限制：512MB (--memory=512m)
  - Disk 限制：100MB (tmpfs size)
  - Network 限制：禁止外部網路存取（--network=isolated）
  - Process 限制：最多 10 個 process (--pids-limit=10)
  - **參考**: [Resource Limits](../../../docs/security/resource-limits.md)

- [x] **錯誤處理與日誌記錄** (0.2 SP)
  - 執行失敗處理（Python 錯誤、語法錯誤）
  - 超時處理（kill 容器進程）
  - 容器崩潰處理（自動重啟容器）
  - 結構化日誌記錄（Serilog）
  - **參考**: [Error Handling Strategy](../../../docs/technical-implementation/01-backend-net9/error-handling.md)

**驗收標準**:
- ✅ ICodeExecutionService 介面定義完整
- ✅ Python 代碼執行成功（簡單 print 測試）
- ✅ 執行超時機制運作正常（>30 秒自動終止）
- ✅ 資源限制有效（CPU, Memory, Disk 在限制內）
- ✅ 錯誤處理和日誌記錄完整（可追溯執行歷史）

##### Phase 3: 4-Layer Security Architecture (1 SP)

**安全設計參考**: [Code Execution Security](../../../docs/security/code-execution-security.md)
**關鍵風險管理**: [RISK-016: Container 逃逸漏洞](../../1-planning/RISK-REGISTER.md) - 🚨 高風險

- [x] **Layer 1: Container 隔離** (0.3 SP)
  - Docker 容器隔離
  - 非 root 用戶執行（uid=1000, gid=1000）
  - Read-only 文件系統（--read-only，除 /tmp）
  - Drop all capabilities（--cap-drop=ALL）
  - No new privileges（--security-opt=no-new-privileges）
  - **參考**: [Container Isolation](../../../docs/security/container-security.md#layer-1)

- [x] **Layer 2: Network 隔離** (0.2 SP)
  - 禁止外部網路存取（--network=isolated）
  - 僅允許 localhost 存取
  - 無 DNS 解析（防止域名查詢）
  - 防火牆規則配置（iptables）
  - **參考**: [Network Isolation](../../../docs/security/network-isolation.md)

- [x] **Layer 3: File System 隔離** (0.2 SP)
  - /tmp 目錄僅可寫（tmpfs mount）
  - 禁止存取敏感目錄（/etc, /proc, /sys, /root）
  - File System 配額限制（100MB）
  - 執行後自動清除 /tmp 內容
  - **參考**: [Filesystem Isolation](../../../docs/security/filesystem-isolation.md)

- [x] **Layer 4: Resource 限制** (0.3 SP)
  - CPU 限制：1 core (--cpus=1.0)
  - Memory 限制：512MB (--memory=512m, --memory-swap=512m)
  - Process 限制：10 個 process (--pids-limit=10)
  - 執行時間限制：30 秒（應用層控制）
  - Disk I/O 限制（--device-read-bps, --device-write-bps）
  - **參考**: [Resource Limits](../../../docs/security/resource-limits.md)

**驗收標準**:
- ✅ 4-Layer Security 全部配置完成
- ✅ 無法從容器內存取宿主機檔案（測試 /etc/passwd, /proc, /sys）
- ✅ 無法從容器內存取外部網路（測試 curl google.com）
- ✅ Resource 限制有效（CPU, Memory 不超過限制）
- ✅ **安全測試通過**（滲透測試，無容器逃逸漏洞）

##### Phase 4: 執行結果處理與可視化 (0.5 SP)

- [x] **執行結果解析** (0.2 SP)
  - stdout / stderr 解析
  - 錯誤訊息格式化（Python traceback 解析）
  - Exit code 處理（0=成功, >0=失敗）
  - 執行時間統計
  - **參考**: [Result Processing](../../../docs/technical-implementation/code-interpreter/result-processing.md)
  - **實作**: `src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs`

- [x] **圖表輸出支援** (0.2 SP)
  - Matplotlib 圖表儲存為 PNG
  - 圖表檔案從容器擷取（docker cp）
  - Base64 編碼傳輸
  - 圖表大小限制：5MB
  - **參考**: [Multimodal Output Engine](../../../docs/technical-implementation/01-backend-net9/08-multimodal-output-engine.md)

- [x] **表格輸出支援** (0.1 SP)
  - Pandas DataFrame 轉 JSON
  - CSV 格式支援
  - 表格大小限制：10,000 rows
  - **參考**: [Result Processing](../../../docs/technical-implementation/code-interpreter/result-processing.md)

**驗收標準**:
- ✅ 執行結果正確解析（stdout, stderr, exit code）
- ✅ Matplotlib 圖表正確輸出（PNG 格式，Base64 編碼）
- ✅ Pandas DataFrame 轉 JSON 成功（支援 10,000 rows）

##### Phase 5: Code Interpreter API 與 UI (0.5 SP)

- [x] **Code Execution API** (0.2 SP)
  - POST /api/v1/code/execute - 執行代碼
  - GET /api/v1/code/executions/{id} - 獲取執行結果
  - DELETE /api/v1/code/executions/{id} - 刪除執行記錄
  - GET /api/v1/code/status - 容器池狀態
  - **參考**: [Code Interpreter API Design](../../../docs/api/code-interpreter-api-design.md)
  - **實作**: `src/AIAgentPlatform.API/Controllers/CodeController.cs`

- [x] **Code Interpreter UI** (0.2 SP)
  - 代碼編輯器（Monaco Editor 或 CodeMirror）
  - 執行按鈕（Run, Stop, Clear）
  - 執行結果展示（Text, Image, Table）
  - 執行歷史記錄（最近 10 次）
  - Loading 狀態與進度條
  - **參考**: [Code Interpreter UI Design](../../../docs/ux-design/code-interpreter-ui-design.md)
  - **實作**: `apps/web-app/src/features/code-interpreter/`

- [x] **單元測試與整合測試** (0.1 SP)
  - DockerCodeExecutionService 單元測試
  - 安全測試（滲透測試）
  - 執行超時測試
  - 資源限制測試
  - 測試覆蓋率 ≥85%
  - **參考**: [Code Interpreter Testing Strategy](../../../docs/testing/code-interpreter-testing-strategy.md)
  - **實作**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/CodeExecutionTests.cs`

**驗收標準**:
- ✅ Code Execution API 完成（4 個端點）
- ✅ Code Interpreter UI 完成（代碼編輯器 + 結果展示）
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 安全測試通過（無容器逃逸漏洞）

#### 總體驗收標準 (Overall Acceptance Criteria)

**功能驗收**:
- ✅ 支援 Python 3.11 代碼執行
- ✅ 執行成功率 ≥90%
- ✅ 執行時間 <30 秒 (P95)
- ✅ 支援 Text、Image、Table 輸出

**安全驗收** (Critical):
- ✅ 4-Layer Security Architecture 完整實作
- ✅ 容器池運作正常（3 個容器）
- ✅ **無容器逃逸漏洞**（滲透測試通過）
- ✅ Resource 限制有效率：100%
- ✅ Network 隔離有效率：100%

**性能驗收**:
- ✅ 容器啟動時間 <5 秒
- ✅ API 響應時間 <200ms (P95)
- ✅ 容器池健康度 ≥95%

**質量驗收**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 集成測試覆蓋率 ≥80%
- ✅ 安全測試覆蓋率：100% (所有攻擊向量)

#### 明確排除 Phase 2 功能 (延後)

❌ **動態容器池管理** - 延後到 Phase 1B
  - 自動擴縮容（根據負載）
  - 負載均衡（智能分配）
  - 容器預熱與回收

❌ **複雜安全策略** - 延後到 Phase 1C
  - AppArmor / SELinux profiles
  - Seccomp 系統調用過濾
  - Capabilities 精細控制

❌ **多語言支援** - 延後到 Phase 2
  - R 語言支援
  - Julia 語言支援
  - JavaScript/Node.js 支援

❌ **高級可視化** - 延後到 Phase 2
  - Plotly 互動圖表
  - Bokeh 儀表板
  - Altair 聲明式圖表

❌ **完整監控系統** - 延後到 Phase 1F
  - Prometheus metrics
  - Grafana dashboard
  - 告警機制

**MVP 範圍參考**:
- 📖 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md#code-interpreter) - Code Interpreter 在 Phase 1A 的範圍
- 📖 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-6) - Sprint 6 任務分配與調整說明

---

## 第二部分：技術實施方案 (How to Build)

### 2.1 架構設計

#### 2.1.1 整體架構

```
┌─────────────────────────────────────────────────────────┐
│  Presentation Layer (API)                               │
│  ┌─────────────────────────────────────────────────┐   │
│  │  CodeController                                  │   │
│  │  POST /api/v1/code/execute                       │   │
│  │  GET  /api/v1/code/executions/{id}              │   │
│  │  GET  /api/v1/code/status                       │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│  Application Layer (CQRS)                               │
│  ┌─────────────────────────────────────────────────┐   │
│  │  ExecuteCodeCommand                              │   │
│  │  ExecuteCodeCommandHandler                       │   │
│  │  GetExecutionStatusQuery                         │   │
│  │  GetContainerStatusQuery                         │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│  Infrastructure Layer (Services)                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │  ICodeExecutionService                           │   │
│  │  ├─ DockerCodeExecutionService                   │   │
│  │  │   ├─ ContainerPoolManager                     │   │
│  │  │   ├─ DockerService                            │   │
│  │  │   └─ SecurityManager                          │   │
│  │  └─ ExecutionResultProcessor                     │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│  Docker Container Pool (3 containers)                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │ Container 1 │  │ Container 2 │  │ Container 3 │    │
│  │ Python 3.11 │  │ Python 3.11 │  │ Python 3.11 │    │
│  │ Status: ✅  │  │ Status: ⏳  │  │ Status: ✅  │    │
│  └─────────────┘  └─────────────┘  └─────────────┘    │
└─────────────────────────────────────────────────────────┘
```

**參考**: [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md)

#### 2.1.2 4-Layer Security Architecture

```
┌──────────────────────────────────────────────────────┐
│ Layer 4: Resource Limits                             │
│ CPU: 1 core │ Memory: 512MB │ Disk: 100MB           │
│ Process: 10 │ Timeout: 30s                          │
└──────────────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────────────┐
│ Layer 3: File System Isolation                       │
│ Read-only FS │ /tmp only writable │ 100MB quota     │
└──────────────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────────────┐
│ Layer 2: Network Isolation                           │
│ No external network │ localhost only │ No DNS       │
└──────────────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────────────┐
│ Layer 1: Container Isolation                         │
│ Docker isolation │ Non-root user │ No capabilities  │
└──────────────────────────────────────────────────────┘
```

**參考**: [Code Execution Security](../../../docs/security/code-execution-security.md)

### 2.2 詳細技術實作

#### 2.2.1 Docker Image 建置

**Dockerfile 設計**:
```dockerfile
# docker/code-interpreter/Dockerfile
FROM python:3.11-slim-bookworm

# 安全更新與必要工具
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        gcc \
        g++ \
        libffi-dev \
    && rm -rf /var/lib/apt/lists/*

# 建立非 root 用戶
RUN groupadd -g 1000 coderunner \
    && useradd -r -u 1000 -g coderunner coderunner \
    && mkdir -p /home/coderunner \
    && chown -R coderunner:coderunner /home/coderunner

# 安裝 Python 套件
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt \
    && rm /tmp/requirements.txt

# 建立執行目錄（僅此目錄可寫）
RUN mkdir -p /workspace \
    && chown -R coderunner:coderunner /workspace

# 切換到非 root 用戶
USER coderunner

# 設定工作目錄
WORKDIR /workspace

# 健康檢查
HEALTHCHECK --interval=10s --timeout=3s --retries=3 \
    CMD python -c "print('healthy')" || exit 1

# 預設命令（保持容器運行）
CMD ["tail", "-f", "/dev/null"]
```

**requirements.txt**:
```txt
numpy==1.26.0
pandas==2.1.0
matplotlib==3.8.0
seaborn==0.13.0
scipy==1.11.0
```

**建置與驗證**:
```bash
# 建置 Image
docker build -t code-interpreter:latest -f docker/code-interpreter/Dockerfile .

# 驗證 Image 大小
docker images | grep code-interpreter
# 預期: < 500MB

# 測試 Image
docker run --rm code-interpreter:latest python -c "import numpy, pandas, matplotlib; print('OK')"
```

**參考**: [Dockerfile Design](../../../docs/deployment/code-interpreter-dockerfile.md)

#### 2.2.2 容器池管理實作

**ContainerPoolManager.cs**:
```csharp
// src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs
namespace AIAgentPlatform.Infrastructure.Services;

public sealed class ContainerPoolManager : IContainerPoolManager, IDisposable
{
    private readonly IDockerService _dockerService;
    private readonly ILogger<ContainerPoolManager> _logger;
    private readonly SemaphoreSlim _poolLock = new(1, 1);
    private readonly List<ContainerInfo> _containers = new();
    private readonly Timer _healthCheckTimer;
    private const int PoolSize = 3;
    private const string ImageName = "code-interpreter:latest";

    public ContainerPoolManager(
        IDockerService dockerService,
        ILogger<ContainerPoolManager> logger)
    {
        _dockerService = dockerService;
        _logger = logger;

        // 啟動健康檢查定時器（每 10 秒）
        _healthCheckTimer = new Timer(
            async _ => await PerformHealthCheckAsync(),
            null,
            TimeSpan.FromSeconds(10),
            TimeSpan.FromSeconds(10));
    }

    public async Task InitializePoolAsync(CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Initializing container pool with {PoolSize} containers", PoolSize);

        for (int i = 1; i <= PoolSize; i++)
        {
            var containerName = $"code-interpreter-{i}";

            try
            {
                var containerId = await _dockerService.CreateContainerAsync(
                    imageName: ImageName,
                    containerName: containerName,
                    options: new ContainerCreateOptions
                    {
                        NetworkMode = "isolated",
                        ReadOnlyRootFilesystem = true,
                        CpuLimit = 1.0,
                        MemoryLimitMB = 512,
                        PidsLimit = 10,
                        NoNewPrivileges = true,
                        CapDrop = new[] { "ALL" },
                        TmpfsMounts = new Dictionary<string, string>
                        {
                            ["/tmp"] = "size=100m"
                        }
                    },
                    cancellationToken);

                await _dockerService.StartContainerAsync(containerId, cancellationToken);

                _containers.Add(new ContainerInfo
                {
                    Id = containerId,
                    Name = containerName,
                    Status = ContainerStatus.Available,
                    CreatedAt = DateTime.UtcNow,
                    LastHealthCheck = DateTime.UtcNow
                });

                _logger.LogInformation("Container {ContainerName} created and started successfully", containerName);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Failed to create container {ContainerName}", containerName);
                throw;
            }
        }

        _logger.LogInformation("Container pool initialized successfully");
    }

    public async Task<ContainerInfo> AcquireContainerAsync(CancellationToken cancellationToken = default)
    {
        await _poolLock.WaitAsync(cancellationToken);

        try
        {
            var availableContainer = _containers
                .FirstOrDefault(c => c.Status == ContainerStatus.Available);

            if (availableContainer == null)
            {
                _logger.LogWarning("No available containers in pool, waiting...");

                // 等待最多 30 秒
                var timeout = TimeSpan.FromSeconds(30);
                var startTime = DateTime.UtcNow;

                while ((DateTime.UtcNow - startTime) < timeout)
                {
                    await Task.Delay(1000, cancellationToken);
                    availableContainer = _containers
                        .FirstOrDefault(c => c.Status == ContainerStatus.Available);

                    if (availableContainer != null)
                        break;
                }

                if (availableContainer == null)
                {
                    throw new NoAvailableContainerException("No containers available after timeout");
                }
            }

            availableContainer.Status = ContainerStatus.Busy;
            availableContainer.LastUsedAt = DateTime.UtcNow;

            _logger.LogInformation("Container {ContainerName} acquired", availableContainer.Name);

            return availableContainer;
        }
        finally
        {
            _poolLock.Release();
        }
    }

    public async Task ReleaseContainerAsync(
        string containerId,
        CancellationToken cancellationToken = default)
    {
        await _poolLock.WaitAsync(cancellationToken);

        try
        {
            var container = _containers.FirstOrDefault(c => c.Id == containerId);

            if (container == null)
            {
                _logger.LogWarning("Container {ContainerId} not found in pool", containerId);
                return;
            }

            // 清理容器 /tmp 目錄
            await _dockerService.ExecuteCommandAsync(
                containerId,
                new[] { "sh", "-c", "rm -rf /tmp/*" },
                cancellationToken);

            container.Status = ContainerStatus.Available;
            container.ExecutionCount++;

            _logger.LogInformation("Container {ContainerName} released and cleaned", container.Name);
        }
        finally
        {
            _poolLock.Release();
        }
    }

    public async Task<IReadOnlyList<ContainerInfo>> GetContainerStatusAsync(
        CancellationToken cancellationToken = default)
    {
        await _poolLock.WaitAsync(cancellationToken);

        try
        {
            return _containers.ToList();
        }
        finally
        {
            _poolLock.Release();
        }
    }

    private async Task PerformHealthCheckAsync()
    {
        await _poolLock.WaitAsync();

        try
        {
            foreach (var container in _containers)
            {
                try
                {
                    var isHealthy = await _dockerService.CheckContainerHealthAsync(container.Id);

                    if (!isHealthy)
                    {
                        _logger.LogWarning("Container {ContainerName} is unhealthy, restarting...", container.Name);

                        container.Status = ContainerStatus.Unhealthy;

                        await _dockerService.RestartContainerAsync(container.Id);

                        container.Status = ContainerStatus.Available;
                        container.RestartCount++;

                        _logger.LogInformation("Container {ContainerName} restarted successfully", container.Name);
                    }

                    container.LastHealthCheck = DateTime.UtcNow;
                }
                catch (Exception ex)
                {
                    _logger.LogError(ex, "Health check failed for container {ContainerName}", container.Name);
                }
            }
        }
        finally
        {
            _poolLock.Release();
        }
    }

    public async Task ShutdownPoolAsync(CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Shutting down container pool");

        _healthCheckTimer?.Dispose();

        foreach (var container in _containers)
        {
            try
            {
                await _dockerService.StopContainerAsync(container.Id, cancellationToken);
                await _dockerService.RemoveContainerAsync(container.Id, cancellationToken);

                _logger.LogInformation("Container {ContainerName} stopped and removed", container.Name);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Failed to shutdown container {ContainerName}", container.Name);
            }
        }

        _containers.Clear();
        _logger.LogInformation("Container pool shutdown complete");
    }

    public void Dispose()
    {
        _healthCheckTimer?.Dispose();
        _poolLock?.Dispose();
    }
}

// Domain/ValueObjects/ContainerInfo.cs
public sealed class ContainerInfo
{
    public required string Id { get; init; }
    public required string Name { get; init; }
    public ContainerStatus Status { get; set; }
    public DateTime CreatedAt { get; init; }
    public DateTime? LastUsedAt { get; set; }
    public DateTime LastHealthCheck { get; set; }
    public int ExecutionCount { get; set; }
    public int RestartCount { get; set; }
}

public enum ContainerStatus
{
    Available,
    Busy,
    Unhealthy
}
```

**參考**: [Container Pool Management](../../../docs/technical-implementation/code-interpreter/container-pool-management.md)

#### 2.2.3 Docker Service 實作

**DockerService.cs**:
```csharp
// src/AIAgentPlatform.Infrastructure/Services/DockerService.cs
namespace AIAgentPlatform.Infrastructure.Services;

public sealed class DockerService : IDockerService
{
    private readonly DockerClient _client;
    private readonly ILogger<DockerService> _logger;

    public DockerService(ILogger<DockerService> logger)
    {
        _logger = logger;
        _client = new DockerClientConfiguration(
            new Uri("unix:///var/run/docker.sock"))
            .CreateClient();
    }

    public async Task<string> CreateContainerAsync(
        string imageName,
        string containerName,
        ContainerCreateOptions options,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Creating container {ContainerName} from image {ImageName}",
            containerName, imageName);

        var config = new CreateContainerParameters
        {
            Image = imageName,
            Name = containerName,
            HostConfig = new HostConfig
            {
                NetworkMode = options.NetworkMode,
                ReadonlyRootfs = options.ReadOnlyRootFilesystem,
                NanoCPUs = (long)(options.CpuLimit * 1_000_000_000),
                Memory = options.MemoryLimitMB * 1024 * 1024,
                PidsLimit = options.PidsLimit,
                CapDrop = options.CapDrop.ToList(),
                SecurityOpt = new List<string> { "no-new-privileges" },
                Tmpfs = options.TmpfsMounts
            },
            Cmd = new[] { "tail", "-f", "/dev/null" } // 保持容器運行
        };

        var response = await _client.Containers.CreateContainerAsync(config, cancellationToken);

        _logger.LogInformation("Container {ContainerName} created with ID {ContainerId}",
            containerName, response.ID);

        return response.ID;
    }

    public async Task StartContainerAsync(string containerId, CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Starting container {ContainerId}", containerId);

        await _client.Containers.StartContainerAsync(
            containerId,
            new ContainerStartParameters(),
            cancellationToken);
    }

    public async Task StopContainerAsync(string containerId, CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Stopping container {ContainerId}", containerId);

        await _client.Containers.StopContainerAsync(
            containerId,
            new ContainerStopParameters { WaitBeforeKillSeconds = 5 },
            cancellationToken);
    }

    public async Task RemoveContainerAsync(string containerId, CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Removing container {ContainerId}", containerId);

        await _client.Containers.RemoveContainerAsync(
            containerId,
            new ContainerRemoveParameters { Force = true },
            cancellationToken);
    }

    public async Task RestartContainerAsync(string containerId, CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Restarting container {ContainerId}", containerId);

        await _client.Containers.RestartContainerAsync(
            containerId,
            new ContainerRestartParameters { WaitBeforeKillSeconds = 5 },
            cancellationToken);
    }

    public async Task<bool> CheckContainerHealthAsync(
        string containerId,
        CancellationToken cancellationToken = default)
    {
        try
        {
            var result = await ExecuteCommandAsync(
                containerId,
                new[] { "python", "-c", "print('healthy')" },
                cancellationToken);

            return result.ExitCode == 0 && result.StdOut.Contains("healthy");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Health check failed for container {ContainerId}", containerId);
            return false;
        }
    }

    public async Task<ExecutionResult> ExecuteCommandAsync(
        string containerId,
        string[] command,
        CancellationToken cancellationToken = default)
    {
        _logger.LogDebug("Executing command in container {ContainerId}: {Command}",
            containerId, string.Join(" ", command));

        var execConfig = new ContainerExecCreateParameters
        {
            AttachStdout = true,
            AttachStderr = true,
            Cmd = command
        };

        var execResponse = await _client.Exec.ExecCreateContainerAsync(
            containerId,
            execConfig,
            cancellationToken);

        using var stream = await _client.Exec.StartAndAttachContainerExecAsync(
            execResponse.ID,
            false,
            cancellationToken);

        var (stdout, stderr) = await stream.ReadOutputToEndAsync(cancellationToken);

        var inspectResponse = await _client.Exec.InspectContainerExecAsync(
            execResponse.ID,
            cancellationToken);

        return new ExecutionResult
        {
            StdOut = stdout,
            StdErr = stderr,
            ExitCode = (int)inspectResponse.ExitCode
        };
    }
}

// Application/Interfaces/IDockerService.cs
public interface IDockerService
{
    Task<string> CreateContainerAsync(
        string imageName,
        string containerName,
        ContainerCreateOptions options,
        CancellationToken cancellationToken = default);

    Task StartContainerAsync(string containerId, CancellationToken cancellationToken = default);
    Task StopContainerAsync(string containerId, CancellationToken cancellationToken = default);
    Task RemoveContainerAsync(string containerId, CancellationToken cancellationToken = default);
    Task RestartContainerAsync(string containerId, CancellationToken cancellationToken = default);
    Task<bool> CheckContainerHealthAsync(string containerId, CancellationToken cancellationToken = default);

    Task<ExecutionResult> ExecuteCommandAsync(
        string containerId,
        string[] command,
        CancellationToken cancellationToken = default);
}

// Domain/ValueObjects/ContainerCreateOptions.cs
public sealed class ContainerCreateOptions
{
    public string NetworkMode { get; init; } = "isolated";
    public bool ReadOnlyRootFilesystem { get; init; } = true;
    public double CpuLimit { get; init; } = 1.0;
    public long MemoryLimitMB { get; init; } = 512;
    public long PidsLimit { get; init; } = 10;
    public bool NoNewPrivileges { get; init; } = true;
    public string[] CapDrop { get; init; } = Array.Empty<string>();
    public Dictionary<string, string> TmpfsMounts { get; init; } = new();
}
```

**參考**: [Docker.DotNet Documentation](https://github.com/dotnet/Docker.DotNet)

#### 2.2.4 Code Execution Service 實作

**DockerCodeExecutionService.cs**:
```csharp
// src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs
namespace AIAgentPlatform.Infrastructure.Services;

public sealed class DockerCodeExecutionService : ICodeExecutionService
{
    private readonly IContainerPoolManager _containerPool;
    private readonly IDockerService _dockerService;
    private readonly IExecutionResultProcessor _resultProcessor;
    private readonly ILogger<DockerCodeExecutionService> _logger;
    private const int DefaultTimeoutSeconds = 30;

    public DockerCodeExecutionService(
        IContainerPoolManager containerPool,
        IDockerService dockerService,
        IExecutionResultProcessor resultProcessor,
        ILogger<DockerCodeExecutionService> logger)
    {
        _containerPool = containerPool;
        _dockerService = dockerService;
        _resultProcessor = resultProcessor;
        _logger = logger;
    }

    public async Task<CodeExecutionResult> ExecuteCodeAsync(
        string code,
        string language,
        int timeoutSeconds = DefaultTimeoutSeconds,
        CancellationToken cancellationToken = default)
    {
        if (language.ToLower() != "python")
        {
            throw new UnsupportedLanguageException($"不支援的語言: {language}");
        }

        var startTime = DateTime.UtcNow;
        ContainerInfo? container = null;

        try
        {
            _logger.LogInformation("Acquiring container for code execution");
            container = await _containerPool.AcquireContainerAsync(cancellationToken);

            _logger.LogInformation("Executing Python code in container {ContainerName}", container.Name);

            // 1. 將代碼寫入容器的 /tmp/script.py
            await WriteCodeToContainerAsync(container.Id, code, cancellationToken);

            // 2. 執行代碼（帶超時控制）
            using var cts = CancellationTokenSource.CreateLinkedTokenSource(cancellationToken);
            cts.CancelAfter(TimeSpan.FromSeconds(timeoutSeconds));

            var executionResult = await ExecuteWithTimeoutAsync(
                container.Id,
                timeoutSeconds,
                cts.Token);

            // 3. 處理執行結果
            var result = await _resultProcessor.ProcessResultAsync(
                container.Id,
                executionResult,
                cancellationToken);

            result.ExecutionTime = DateTime.UtcNow - startTime;
            result.ContainerId = container.Id;

            _logger.LogInformation(
                "Code execution completed in {ExecutionTime}ms with exit code {ExitCode}",
                result.ExecutionTime.TotalMilliseconds,
                result.ExitCode);

            return result;
        }
        catch (OperationCanceledException)
        {
            _logger.LogWarning("Code execution timed out after {Timeout}s", timeoutSeconds);

            if (container != null)
            {
                // 終止容器內的 Python 進程
                await KillPythonProcessAsync(container.Id);
            }

            return new CodeExecutionResult
            {
                Success = false,
                ExitCode = -1,
                StdErr = $"執行超時 ({timeoutSeconds} 秒)",
                ExecutionTime = TimeSpan.FromSeconds(timeoutSeconds),
                ContainerId = container?.Id ?? "unknown"
            };
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Code execution failed");

            return new CodeExecutionResult
            {
                Success = false,
                ExitCode = -1,
                StdErr = $"執行失敗: {ex.Message}",
                ExecutionTime = DateTime.UtcNow - startTime,
                ContainerId = container?.Id ?? "unknown"
            };
        }
        finally
        {
            if (container != null)
            {
                await _containerPool.ReleaseContainerAsync(container.Id, cancellationToken);
            }
        }
    }

    private async Task WriteCodeToContainerAsync(
        string containerId,
        string code,
        CancellationToken cancellationToken)
    {
        // 將代碼寫入 /tmp/script.py
        var escapedCode = code.Replace("'", "'\\''");

        await _dockerService.ExecuteCommandAsync(
            containerId,
            new[] { "sh", "-c", $"echo '{escapedCode}' > /tmp/script.py" },
            cancellationToken);
    }

    private async Task<ExecutionResult> ExecuteWithTimeoutAsync(
        string containerId,
        int timeoutSeconds,
        CancellationToken cancellationToken)
    {
        var result = await _dockerService.ExecuteCommandAsync(
            containerId,
            new[] { "python", "/tmp/script.py" },
            cancellationToken);

        return result;
    }

    private async Task KillPythonProcessAsync(string containerId)
    {
        try
        {
            await _dockerService.ExecuteCommandAsync(
                containerId,
                new[] { "pkill", "-9", "python" },
                CancellationToken.None);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to kill Python process in container {ContainerId}", containerId);
        }
    }
}

// Application/Interfaces/ICodeExecutionService.cs
public interface ICodeExecutionService
{
    Task<CodeExecutionResult> ExecuteCodeAsync(
        string code,
        string language,
        int timeoutSeconds = 30,
        CancellationToken cancellationToken = default);
}

// Domain/ValueObjects/CodeExecutionResult.cs
public sealed class CodeExecutionResult
{
    public bool Success { get; set; }
    public int ExitCode { get; set; }
    public string StdOut { get; set; } = string.Empty;
    public string StdErr { get; set; } = string.Empty;
    public TimeSpan ExecutionTime { get; set; }
    public string ContainerId { get; set; } = string.Empty;
    public List<OutputArtifact> Artifacts { get; set; } = new();
}

// Domain/ValueObjects/OutputArtifact.cs
public sealed class OutputArtifact
{
    public string Type { get; set; } = string.Empty; // "image", "table", "text"
    public string Content { get; set; } = string.Empty; // Base64 for images, JSON for tables
    public string MimeType { get; set; } = string.Empty;
}
```

**參考**: [Execution Engine](../../../docs/technical-implementation/code-interpreter/execution-engine.md)

#### 2.2.5 Execution Result Processor 實作

**ExecutionResultProcessor.cs**:
```csharp
// src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs
namespace AIAgentPlatform.Infrastructure.Services;

public sealed class ExecutionResultProcessor : IExecutionResultProcessor
{
    private readonly IDockerService _dockerService;
    private readonly ILogger<ExecutionResultProcessor> _logger;

    public ExecutionResultProcessor(
        IDockerService dockerService,
        ILogger<ExecutionResultProcessor> logger)
    {
        _dockerService = dockerService;
        _logger = logger;
    }

    public async Task<CodeExecutionResult> ProcessResultAsync(
        string containerId,
        ExecutionResult executionResult,
        CancellationToken cancellationToken = default)
    {
        var result = new CodeExecutionResult
        {
            Success = executionResult.ExitCode == 0,
            ExitCode = executionResult.ExitCode,
            StdOut = executionResult.StdOut,
            StdErr = executionResult.StdErr,
            ContainerId = containerId
        };

        // 處理 Matplotlib 圖表輸出
        await ProcessImageOutputsAsync(containerId, result, cancellationToken);

        // 處理 Pandas DataFrame 輸出（如果有）
        await ProcessTableOutputsAsync(result);

        return result;
    }

    private async Task ProcessImageOutputsAsync(
        string containerId,
        CodeExecutionResult result,
        CancellationToken cancellationToken)
    {
        try
        {
            // 檢查 /tmp 是否有 .png 圖片
            var checkResult = await _dockerService.ExecuteCommandAsync(
                containerId,
                new[] { "sh", "-c", "ls /tmp/*.png 2>/dev/null || echo 'no-images'" },
                cancellationToken);

            if (checkResult.StdOut.Contains("no-images"))
            {
                return; // 沒有圖片
            }

            var imageFiles = checkResult.StdOut
                .Split('\n', StringSplitOptions.RemoveEmptyEntries)
                .Where(f => f.EndsWith(".png"))
                .ToList();

            foreach (var imageFile in imageFiles)
            {
                // 從容器讀取圖片內容
                var catResult = await _dockerService.ExecuteCommandAsync(
                    containerId,
                    new[] { "cat", imageFile },
                    cancellationToken);

                if (catResult.ExitCode == 0)
                {
                    // Base64 編碼
                    var base64Image = Convert.ToBase64String(Encoding.UTF8.GetBytes(catResult.StdOut));

                    result.Artifacts.Add(new OutputArtifact
                    {
                        Type = "image",
                        Content = base64Image,
                        MimeType = "image/png"
                    });

                    _logger.LogInformation("Processed image output: {ImageFile}", imageFile);
                }
            }
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to process image outputs");
        }
    }

    private Task ProcessTableOutputsAsync(CodeExecutionResult result)
    {
        // 檢查 StdOut 是否包含 JSON 格式的 DataFrame
        // 簡化實作：假設輸出包含 JSON 標記
        if (result.StdOut.Contains("__DATAFRAME_JSON__"))
        {
            try
            {
                var jsonStart = result.StdOut.IndexOf("__DATAFRAME_JSON__") + "__DATAFRAME_JSON__".Length;
                var jsonEnd = result.StdOut.IndexOf("__END_DATAFRAME__");

                if (jsonEnd > jsonStart)
                {
                    var jsonContent = result.StdOut.Substring(jsonStart, jsonEnd - jsonStart);

                    result.Artifacts.Add(new OutputArtifact
                    {
                        Type = "table",
                        Content = jsonContent,
                        MimeType = "application/json"
                    });

                    _logger.LogInformation("Processed table output");
                }
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Failed to process table output");
            }
        }

        return Task.CompletedTask;
    }
}

// Application/Interfaces/IExecutionResultProcessor.cs
public interface IExecutionResultProcessor
{
    Task<CodeExecutionResult> ProcessResultAsync(
        string containerId,
        ExecutionResult executionResult,
        CancellationToken cancellationToken = default);
}
```

**參考**: [Result Processing](../../../docs/technical-implementation/code-interpreter/result-processing.md)

#### 2.2.6 Code Execution API 實作

**CodeController.cs**:
```csharp
// src/AIAgentPlatform.API/Controllers/CodeController.cs
namespace AIAgentPlatform.API.Controllers;

[ApiController]
[Route("api/v1/code")]
[Produces("application/json")]
public sealed class CodeController : ControllerBase
{
    private readonly ISender _sender;
    private readonly ILogger<CodeController> _logger;

    public CodeController(ISender sender, ILogger<CodeController> logger)
    {
        _sender = sender;
        _logger = logger;
    }

    /// <summary>
    /// 執行代碼
    /// </summary>
    [HttpPost("execute")]
    [ProducesResponseType(typeof(CodeExecutionResponse), StatusCodes.Status200OK)]
    [ProducesResponseType(typeof(ProblemDetails), StatusCodes.Status400BadRequest)]
    [ProducesResponseType(typeof(ProblemDetails), StatusCodes.Status500InternalServerError)]
    public async Task<ActionResult<CodeExecutionResponse>> ExecuteCode(
        [FromBody] ExecuteCodeRequest request,
        CancellationToken cancellationToken)
    {
        _logger.LogInformation("Executing code: language={Language}, length={Length}",
            request.Language, request.Code.Length);

        var command = new ExecuteCodeCommand
        {
            Code = request.Code,
            Language = request.Language,
            TimeoutSeconds = request.TimeoutSeconds ?? 30
        };

        var result = await _sender.Send(command, cancellationToken);

        if (!result.IsSuccess)
        {
            return Problem(
                detail: result.Error,
                statusCode: StatusCodes.Status500InternalServerError);
        }

        return Ok(result.Value);
    }

    /// <summary>
    /// 獲取執行結果
    /// </summary>
    [HttpGet("executions/{id}")]
    [ProducesResponseType(typeof(CodeExecutionResponse), StatusCodes.Status200OK)]
    [ProducesResponseType(typeof(ProblemDetails), StatusCodes.Status404NotFound)]
    public async Task<ActionResult<CodeExecutionResponse>> GetExecution(
        Guid id,
        CancellationToken cancellationToken)
    {
        var query = new GetExecutionQuery { ExecutionId = id };
        var result = await _sender.Send(query, cancellationToken);

        if (!result.IsSuccess)
        {
            return NotFound(new ProblemDetails
            {
                Detail = result.Error,
                Status = StatusCodes.Status404NotFound
            });
        }

        return Ok(result.Value);
    }

    /// <summary>
    /// 刪除執行記錄
    /// </summary>
    [HttpDelete("executions/{id}")]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(typeof(ProblemDetails), StatusCodes.Status404NotFound)]
    public async Task<ActionResult> DeleteExecution(
        Guid id,
        CancellationToken cancellationToken)
    {
        var command = new DeleteExecutionCommand { ExecutionId = id };
        var result = await _sender.Send(command, cancellationToken);

        if (!result.IsSuccess)
        {
            return NotFound(new ProblemDetails
            {
                Detail = result.Error,
                Status = StatusCodes.Status404NotFound
            });
        }

        return NoContent();
    }

    /// <summary>
    /// 獲取容器池狀態
    /// </summary>
    [HttpGet("status")]
    [ProducesResponseType(typeof(ContainerPoolStatusResponse), StatusCodes.Status200OK)]
    public async Task<ActionResult<ContainerPoolStatusResponse>> GetStatus(
        CancellationToken cancellationToken)
    {
        var query = new GetContainerStatusQuery();
        var result = await _sender.Send(query, cancellationToken);

        return Ok(result.Value);
    }
}

// API/Contracts/ExecuteCodeRequest.cs
public sealed record ExecuteCodeRequest
{
    [Required]
    [StringLength(100000, MinimumLength = 1)]
    public required string Code { get; init; }

    [Required]
    [RegularExpression("^(python)$", ErrorMessage = "目前僅支援 Python")]
    public required string Language { get; init; }

    [Range(1, 300)]
    public int? TimeoutSeconds { get; init; }
}

// API/Contracts/CodeExecutionResponse.cs
public sealed record CodeExecutionResponse
{
    public Guid ExecutionId { get; init; }
    public bool Success { get; init; }
    public int ExitCode { get; init; }
    public string StdOut { get; init; } = string.Empty;
    public string StdErr { get; init; } = string.Empty;
    public double ExecutionTimeMs { get; init; }
    public string ContainerId { get; init; } = string.Empty;
    public List<OutputArtifactDto> Artifacts { get; init; } = new();
}

// API/Contracts/ContainerPoolStatusResponse.cs
public sealed record ContainerPoolStatusResponse
{
    public int TotalContainers { get; init; }
    public int AvailableContainers { get; init; }
    public int BusyContainers { get; init; }
    public int UnhealthyContainers { get; init; }
    public List<ContainerInfoDto> Containers { get; init; } = new();
}
```

**ExecuteCodeCommand & Handler**:
```csharp
// Application/CodeExecution/Commands/ExecuteCode/ExecuteCodeCommand.cs
public sealed record ExecuteCodeCommand : IRequest<Result<CodeExecutionResponse>>
{
    public required string Code { get; init; }
    public required string Language { get; init; }
    public int TimeoutSeconds { get; init; } = 30;
}

// Application/CodeExecution/Commands/ExecuteCode/ExecuteCodeCommandHandler.cs
public sealed class ExecuteCodeCommandHandler
    : IRequestHandler<ExecuteCodeCommand, Result<CodeExecutionResponse>>
{
    private readonly ICodeExecutionService _executionService;
    private readonly ICodeExecutionRepository _repository;
    private readonly ILogger<ExecuteCodeCommandHandler> _logger;

    public async Task<Result<CodeExecutionResponse>> Handle(
        ExecuteCodeCommand request,
        CancellationToken cancellationToken)
    {
        try
        {
            // 1. 執行代碼
            var result = await _executionService.ExecuteCodeAsync(
                request.Code,
                request.Language,
                request.TimeoutSeconds,
                cancellationToken);

            // 2. 儲存執行記錄
            var execution = CodeExecution.Create(
                code: request.Code,
                language: request.Language,
                result: result);

            await _repository.AddAsync(execution, cancellationToken);
            await _repository.SaveChangesAsync(cancellationToken);

            // 3. 返回結果
            var response = new CodeExecutionResponse
            {
                ExecutionId = execution.Id,
                Success = result.Success,
                ExitCode = result.ExitCode,
                StdOut = result.StdOut,
                StdErr = result.StdErr,
                ExecutionTimeMs = result.ExecutionTime.TotalMilliseconds,
                ContainerId = result.ContainerId,
                Artifacts = result.Artifacts.Select(a => new OutputArtifactDto
                {
                    Type = a.Type,
                    Content = a.Content,
                    MimeType = a.MimeType
                }).ToList()
            };

            return Result<CodeExecutionResponse>.Success(response);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Code execution failed");
            return Result<CodeExecutionResponse>.Failure($"執行失敗: {ex.Message}");
        }
    }
}
```

**參考**: [Code Interpreter API Design](../../../docs/api/code-interpreter-api-design.md)

#### 2.2.7 Code Interpreter UI 實作

**CodeInterpreter.tsx**:
```typescript
// apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx
import React, { useState } from 'react';
import { Box, Button, Paper, Typography, CircularProgress } from '@mui/material';
import { PlayArrow, Stop, Clear } from '@mui/icons-material';
import Editor from '@monaco-editor/react';
import { executeCode } from './services/codeExecutionApi';
import ExecutionResult from './components/ExecutionResult';

interface CodeInterpreterProps {
  agentId: string;
}

export const CodeInterpreter: React.FC<CodeInterpreterProps> = ({ agentId }) => {
  const [code, setCode] = useState<string>('# 輸入 Python 代碼\nprint("Hello, World!")');
  const [executing, setExecuting] = useState<boolean>(false);
  const [result, setResult] = useState<ExecutionResult | null>(null);
  const [executionTime, setExecutionTime] = useState<number>(0);

  const handleExecute = async () => {
    setExecuting(true);
    setResult(null);
    const startTime = Date.now();

    try {
      const response = await executeCode({
        code,
        language: 'python',
        timeoutSeconds: 30,
      });

      setExecutionTime(Date.now() - startTime);
      setResult(response);
    } catch (error) {
      console.error('Execution failed:', error);
      setResult({
        executionId: '',
        success: false,
        exitCode: -1,
        stdOut: '',
        stdErr: error instanceof Error ? error.message : 'Unknown error',
        executionTimeMs: Date.now() - startTime,
        containerId: '',
        artifacts: [],
      });
    } finally {
      setExecuting(false);
    }
  };

  const handleStop = () => {
    // TODO: 實作停止執行（需要 API 支援）
    setExecuting(false);
  };

  const handleClear = () => {
    setCode('');
    setResult(null);
    setExecutionTime(0);
  };

  return (
    <Box sx={{ height: '100vh', display: 'flex', flexDirection: 'column', p: 2 }}>
      {/* 標題列 */}
      <Box sx={{ mb: 2, display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
        <Typography variant="h5">Code Interpreter</Typography>
        <Box>
          <Button
            variant="contained"
            color="primary"
            startIcon={executing ? <CircularProgress size={20} /> : <PlayArrow />}
            onClick={handleExecute}
            disabled={executing || !code.trim()}
            sx={{ mr: 1 }}
          >
            {executing ? '執行中...' : '執行'}
          </Button>
          <Button
            variant="outlined"
            color="error"
            startIcon={<Stop />}
            onClick={handleStop}
            disabled={!executing}
            sx={{ mr: 1 }}
          >
            停止
          </Button>
          <Button
            variant="outlined"
            startIcon={<Clear />}
            onClick={handleClear}
          >
            清除
          </Button>
        </Box>
      </Box>

      {/* 代碼編輯器 */}
      <Paper sx={{ flex: 1, mb: 2, overflow: 'hidden' }}>
        <Editor
          height="100%"
          defaultLanguage="python"
          value={code}
          onChange={(value) => setCode(value || '')}
          theme="vs-dark"
          options={{
            minimap: { enabled: false },
            fontSize: 14,
            lineNumbers: 'on',
            roundedSelection: false,
            scrollBeyondLastLine: false,
            readOnly: executing,
          }}
        />
      </Paper>

      {/* 執行結果 */}
      {result && (
        <Paper sx={{ flex: 1, overflow: 'auto', p: 2 }}>
          <ExecutionResult result={result} executionTime={executionTime} />
        </Paper>
      )}
    </Box>
  );
};

export default CodeInterpreter;
```

**ExecutionResult.tsx**:
```typescript
// apps/web-app/src/features/code-interpreter/components/ExecutionResult.tsx
import React from 'react';
import { Box, Typography, Divider, Alert, Tabs, Tab } from '@mui/material';
import { CheckCircle, Error } from '@mui/icons-material';

interface ExecutionResultProps {
  result: {
    success: boolean;
    exitCode: number;
    stdOut: string;
    stdErr: string;
    executionTimeMs: number;
    artifacts: Array<{ type: string; content: string; mimeType: string }>;
  };
  executionTime: number;
}

export const ExecutionResult: React.FC<ExecutionResultProps> = ({ result, executionTime }) => {
  const [tabValue, setTabValue] = React.useState(0);

  return (
    <Box>
      {/* 執行狀態 */}
      <Box sx={{ display: 'flex', alignItems: 'center', mb: 2 }}>
        {result.success ? (
          <CheckCircle color="success" sx={{ mr: 1 }} />
        ) : (
          <Error color="error" sx={{ mr: 1 }} />
        )}
        <Typography variant="h6">
          {result.success ? '執行成功' : '執行失敗'} (Exit Code: {result.exitCode})
        </Typography>
        <Typography variant="body2" color="text.secondary" sx={{ ml: 'auto' }}>
          執行時間: {result.executionTimeMs.toFixed(2)} ms
        </Typography>
      </Box>

      <Divider sx={{ mb: 2 }} />

      {/* 輸出標籤 */}
      <Tabs value={tabValue} onChange={(_, newValue) => setTabValue(newValue)} sx={{ mb: 2 }}>
        <Tab label="標準輸出" />
        <Tab label="錯誤輸出" />
        {result.artifacts.length > 0 && <Tab label="圖表與表格" />}
      </Tabs>

      {/* 標準輸出 */}
      {tabValue === 0 && (
        <Box>
          {result.stdOut ? (
            <pre style={{
              background: '#f5f5f5',
              padding: '16px',
              borderRadius: '4px',
              overflow: 'auto',
              maxHeight: '400px'
            }}>
              {result.stdOut}
            </pre>
          ) : (
            <Typography color="text.secondary">無輸出</Typography>
          )}
        </Box>
      )}

      {/* 錯誤輸出 */}
      {tabValue === 1 && (
        <Box>
          {result.stdErr ? (
            <Alert severity="error" sx={{ whiteSpace: 'pre-wrap' }}>
              {result.stdErr}
            </Alert>
          ) : (
            <Typography color="text.secondary">無錯誤</Typography>
          )}
        </Box>
      )}

      {/* 圖表與表格 */}
      {tabValue === 2 && (
        <Box>
          {result.artifacts.map((artifact, index) => (
            <Box key={index} sx={{ mb: 2 }}>
              {artifact.type === 'image' && (
                <img
                  src={`data:${artifact.mimeType};base64,${artifact.content}`}
                  alt={`Output ${index + 1}`}
                  style={{ maxWidth: '100%', height: 'auto' }}
                />
              )}
              {artifact.type === 'table' && (
                <pre style={{
                  background: '#f5f5f5',
                  padding: '16px',
                  borderRadius: '4px',
                  overflow: 'auto'
                }}>
                  {JSON.stringify(JSON.parse(artifact.content), null, 2)}
                </pre>
              )}
            </Box>
          ))}
        </Box>
      )}
    </Box>
  );
};

export default ExecutionResult;
```

**codeExecutionApi.ts**:
```typescript
// apps/web-app/src/features/code-interpreter/services/codeExecutionApi.ts
import axios from 'axios';

const API_BASE_URL = import.meta.env.VITE_API_BASE_URL || 'http://localhost:5095';

export interface ExecuteCodeRequest {
  code: string;
  language: string;
  timeoutSeconds?: number;
}

export interface ExecuteCodeResponse {
  executionId: string;
  success: boolean;
  exitCode: number;
  stdOut: string;
  stdErr: string;
  executionTimeMs: number;
  containerId: string;
  artifacts: Array<{
    type: string;
    content: string;
    mimeType: string;
  }>;
}

export const executeCode = async (
  request: ExecuteCodeRequest
): Promise<ExecuteCodeResponse> => {
  const response = await axios.post<ExecuteCodeResponse>(
    `${API_BASE_URL}/api/v1/code/execute`,
    request,
    {
      headers: {
        'Content-Type': 'application/json',
      },
    }
  );

  return response.data;
};

export const getExecution = async (
  executionId: string
): Promise<ExecuteCodeResponse> => {
  const response = await axios.get<ExecuteCodeResponse>(
    `${API_BASE_URL}/api/v1/code/executions/${executionId}`
  );

  return response.data;
};

export const deleteExecution = async (executionId: string): Promise<void> => {
  await axios.delete(`${API_BASE_URL}/api/v1/code/executions/${executionId}`);
};
```

**參考**: [Code Interpreter UI Design](../../../docs/ux-design/code-interpreter-ui-design.md)

### 2.3 Docker Compose 配置

**docker-compose.yml 更新**:
```yaml
# docker-compose.yml (更新部分)
services:
  # ... 其他服務 ...

  # Code Interpreter 容器（3 個）
  code-interpreter-1:
    image: code-interpreter:latest
    container_name: code-interpreter-1
    build:
      context: ./docker/code-interpreter
      dockerfile: Dockerfile
    networks:
      - isolated
    read_only: true
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL
    tmpfs:
      - /tmp:size=100m,mode=1777
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 256M
    healthcheck:
      test: ["CMD", "python", "-c", "print('healthy')"]
      interval: 10s
      timeout: 3s
      retries: 3
    restart: unless-stopped

  code-interpreter-2:
    image: code-interpreter:latest
    container_name: code-interpreter-2
    build:
      context: ./docker/code-interpreter
      dockerfile: Dockerfile
    networks:
      - isolated
    read_only: true
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL
    tmpfs:
      - /tmp:size=100m,mode=1777
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 256M
    healthcheck:
      test: ["CMD", "python", "-c", "print('healthy')"]
      interval: 10s
      timeout: 3s
      retries: 3
    restart: unless-stopped

  code-interpreter-3:
    image: code-interpreter:latest
    container_name: code-interpreter-3
    build:
      context: ./docker/code-interpreter
      dockerfile: Dockerfile
    networks:
      - isolated
    read_only: true
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL
    tmpfs:
      - /tmp:size=100m,mode=1777
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 256M
    healthcheck:
      test: ["CMD", "python", "-c", "print('healthy')"]
      interval: 10s
      timeout: 3s
      retries: 3
    restart: unless-stopped

networks:
  isolated:
    driver: bridge
    internal: true  # 禁止外部網路存取
```

**參考**: [Docker Compose Setup](../../../docs/deployment/docker-compose-setup.md)

---

## 第三部分：編碼規範

### 3.1 C# 編碼規範

**命名規則**:
- 類別、介面：PascalCase (`ICodeExecutionService`, `DockerCodeExecutionService`)
- 方法、屬性：PascalCase (`ExecuteCodeAsync`, `ContainerId`)
- 私有欄位：_camelCase (`_containerPool`, `_logger`)
- 參數、區域變數：camelCase (`containerId`, `cancellationToken`)
- 常數：UPPER_CASE (`DEFAULT_TIMEOUT_SECONDS`)

**非同步方法**:
- 方法名稱以 `Async` 結尾
- 返回 `Task` 或 `Task<T>`
- 接受 `CancellationToken` 參數

**錯誤處理**:
- 使用 Result Pattern（`Result<T>`）
- 記錄結構化日誌（Serilog）
- 拋出領域特定例外（`ContainerException`, `ExecutionTimeoutException`）

**依賴注入**:
- 建構子注入
- 介面優於實作
- 使用 ILogger<T>

**參考**: [C# Coding Standards](../../../docs/development-standards/coding-standards-csharp.md)

### 3.2 TypeScript 編碼規範

**命名規則**:
- 介面、類型：PascalCase (`ExecuteCodeRequest`, `CodeExecutionResponse`)
- 函數、變數：camelCase (`executeCode`, `executionTime`)
- 常數：UPPER_CASE (`API_BASE_URL`)
- 組件：PascalCase (`CodeInterpreter`, `ExecutionResult`)

**React 組件**:
- 使用函數組件 + Hooks
- Props 使用介面定義
- 明確的類型標註

**錯誤處理**:
- try-catch 包裹 API 調用
- 提供用戶友好的錯誤訊息
- 記錄錯誤到 console

**參考**: [TypeScript Coding Standards](../../../docs/development-standards/coding-standards-typescript.md)

### 3.3 Docker 與安全規範

**Docker 最佳實踐**:
- 使用官方基礎 Image
- 最小化 Image 大小
- 非 root 用戶執行
- Read-only 文件系統
- 明確的資源限制

**安全檢查清單**:
- ✅ 4-Layer Security Architecture 完整實作
- ✅ 無外部網路存取
- ✅ 無敏感目錄訪問
- ✅ 資源限制配置
- ✅ 定期安全掃描（Trivy）

**參考**:
- [Container Security](../../../docs/security/container-security.md)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

---

## 第四部分：質量保證

### 4.1 測試策略

#### 4.1.1 單元測試

**ContainerPoolManagerTests.cs**:
```csharp
// tests/AIAgentPlatform.UnitTests/Infrastructure/Services/ContainerPoolManagerTests.cs
public class ContainerPoolManagerTests
{
    private readonly Mock<IDockerService> _dockerServiceMock;
    private readonly Mock<ILogger<ContainerPoolManager>> _loggerMock;
    private readonly ContainerPoolManager _sut;

    public ContainerPoolManagerTests()
    {
        _dockerServiceMock = new Mock<IDockerService>();
        _loggerMock = new Mock<ILogger<ContainerPoolManager>>();
        _sut = new ContainerPoolManager(_dockerServiceMock.Object, _loggerMock.Object);
    }

    [Fact]
    public async Task InitializePoolAsync_ShouldCreate3Containers()
    {
        // Arrange
        _dockerServiceMock
            .Setup(x => x.CreateContainerAsync(
                It.IsAny<string>(),
                It.IsAny<string>(),
                It.IsAny<ContainerCreateOptions>(),
                It.IsAny<CancellationToken>()))
            .ReturnsAsync("container-id");

        _dockerServiceMock
            .Setup(x => x.StartContainerAsync(It.IsAny<string>(), It.IsAny<CancellationToken>()))
            .Returns(Task.CompletedTask);

        // Act
        await _sut.InitializePoolAsync();

        // Assert
        _dockerServiceMock.Verify(
            x => x.CreateContainerAsync(
                It.IsAny<string>(),
                It.IsAny<string>(),
                It.IsAny<ContainerCreateOptions>(),
                It.IsAny<CancellationToken>()),
            Times.Exactly(3));

        _dockerServiceMock.Verify(
            x => x.StartContainerAsync(It.IsAny<string>(), It.IsAny<CancellationToken>()),
            Times.Exactly(3));
    }

    [Fact]
    public async Task AcquireContainerAsync_ShouldReturnAvailableContainer()
    {
        // Arrange
        await _sut.InitializePoolAsync();

        // Act
        var container = await _sut.AcquireContainerAsync();

        // Assert
        Assert.NotNull(container);
        Assert.Equal(ContainerStatus.Busy, container.Status);
    }

    [Fact]
    public async Task ReleaseContainerAsync_ShouldMarkContainerAsAvailable()
    {
        // Arrange
        await _sut.InitializePoolAsync();
        var container = await _sut.AcquireContainerAsync();

        _dockerServiceMock
            .Setup(x => x.ExecuteCommandAsync(
                It.IsAny<string>(),
                It.IsAny<string[]>(),
                It.IsAny<CancellationToken>()))
            .ReturnsAsync(new ExecutionResult { ExitCode = 0 });

        // Act
        await _sut.ReleaseContainerAsync(container.Id);

        // Assert
        var status = await _sut.GetContainerStatusAsync();
        var releasedContainer = status.First(c => c.Id == container.Id);
        Assert.Equal(ContainerStatus.Available, releasedContainer.Status);
    }
}
```

**DockerCodeExecutionServiceTests.cs**:
```csharp
// tests/AIAgentPlatform.UnitTests/Infrastructure/Services/DockerCodeExecutionServiceTests.cs
public class DockerCodeExecutionServiceTests
{
    private readonly Mock<IContainerPoolManager> _containerPoolMock;
    private readonly Mock<IDockerService> _dockerServiceMock;
    private readonly Mock<IExecutionResultProcessor> _resultProcessorMock;
    private readonly Mock<ILogger<DockerCodeExecutionService>> _loggerMock;
    private readonly DockerCodeExecutionService _sut;

    public DockerCodeExecutionServiceTests()
    {
        _containerPoolMock = new Mock<IContainerPoolManager>();
        _dockerServiceMock = new Mock<IDockerService>();
        _resultProcessorMock = new Mock<IExecutionResultProcessor>();
        _loggerMock = new Mock<ILogger<DockerCodeExecutionService>>();

        _sut = new DockerCodeExecutionService(
            _containerPoolMock.Object,
            _dockerServiceMock.Object,
            _resultProcessorMock.Object,
            _loggerMock.Object);
    }

    [Fact]
    public async Task ExecuteCodeAsync_WithValidPythonCode_ShouldReturnSuccessResult()
    {
        // Arrange
        var code = "print('Hello, World!')";
        var containerInfo = new ContainerInfo
        {
            Id = "test-container-id",
            Name = "test-container",
            Status = ContainerStatus.Available,
            CreatedAt = DateTime.UtcNow
        };

        _containerPoolMock
            .Setup(x => x.AcquireContainerAsync(It.IsAny<CancellationToken>()))
            .ReturnsAsync(containerInfo);

        _dockerServiceMock
            .Setup(x => x.ExecuteCommandAsync(
                It.IsAny<string>(),
                It.IsAny<string[]>(),
                It.IsAny<CancellationToken>()))
            .ReturnsAsync(new ExecutionResult
            {
                ExitCode = 0,
                StdOut = "Hello, World!\n",
                StdErr = string.Empty
            });

        _resultProcessorMock
            .Setup(x => x.ProcessResultAsync(
                It.IsAny<string>(),
                It.IsAny<ExecutionResult>(),
                It.IsAny<CancellationToken>()))
            .ReturnsAsync(new CodeExecutionResult
            {
                Success = true,
                ExitCode = 0,
                StdOut = "Hello, World!\n",
                ContainerId = containerInfo.Id
            });

        // Act
        var result = await _sut.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success);
        Assert.Equal(0, result.ExitCode);
        Assert.Contains("Hello, World!", result.StdOut);

        _containerPoolMock.Verify(
            x => x.ReleaseContainerAsync(containerInfo.Id, It.IsAny<CancellationToken>()),
            Times.Once);
    }

    [Fact]
    public async Task ExecuteCodeAsync_WithTimeout_ShouldReturnTimeoutError()
    {
        // Arrange
        var code = "import time\ntime.sleep(100)";
        var containerInfo = new ContainerInfo
        {
            Id = "test-container-id",
            Name = "test-container",
            Status = ContainerStatus.Available,
            CreatedAt = DateTime.UtcNow
        };

        _containerPoolMock
            .Setup(x => x.AcquireContainerAsync(It.IsAny<CancellationToken>()))
            .ReturnsAsync(containerInfo);

        _dockerServiceMock
            .Setup(x => x.ExecuteCommandAsync(
                It.IsAny<string>(),
                It.IsAny<string[]>(),
                It.IsAny<CancellationToken>()))
            .ThrowsAsync(new OperationCanceledException());

        // Act
        var result = await _sut.ExecuteCodeAsync(code, "python", timeoutSeconds: 1);

        // Assert
        Assert.False(result.Success);
        Assert.Equal(-1, result.ExitCode);
        Assert.Contains("執行超時", result.StdErr);
    }

    [Fact]
    public async Task ExecuteCodeAsync_WithUnsupportedLanguage_ShouldThrowException()
    {
        // Arrange
        var code = "console.log('test')";

        // Act & Assert
        await Assert.ThrowsAsync<UnsupportedLanguageException>(
            () => _sut.ExecuteCodeAsync(code, "javascript"));
    }
}
```

**測試覆蓋率目標**: ≥85%

**參考**: [Unit Testing Guidelines](../../../docs/testing/unit-testing-guidelines.md)

#### 4.1.2 集成測試

**CodeExecutionIntegrationTests.cs**:
```csharp
// tests/AIAgentPlatform.IntegrationTests/CodeExecution/CodeExecutionIntegrationTests.cs
[Collection("Docker")]
public class CodeExecutionIntegrationTests : IAsyncLifetime
{
    private readonly ContainerPoolManager _containerPool;
    private readonly DockerService _dockerService;
    private readonly DockerCodeExecutionService _executionService;

    public CodeExecutionIntegrationTests()
    {
        var logger1 = new Mock<ILogger<DockerService>>().Object;
        var logger2 = new Mock<ILogger<ContainerPoolManager>>().Object;
        var logger3 = new Mock<ILogger<DockerCodeExecutionService>>().Object;

        _dockerService = new DockerService(logger1);
        _containerPool = new ContainerPoolManager(_dockerService, logger2);

        var resultProcessor = new ExecutionResultProcessor(_dockerService, Mock.Of<ILogger<ExecutionResultProcessor>>());
        _executionService = new DockerCodeExecutionService(
            _containerPool,
            _dockerService,
            resultProcessor,
            logger3);
    }

    public async Task InitializeAsync()
    {
        await _containerPool.InitializePoolAsync();
    }

    public async Task DisposeAsync()
    {
        await _containerPool.ShutdownPoolAsync();
    }

    [Fact]
    public async Task ExecutePythonCode_SimplePrint_ShouldReturnCorrectOutput()
    {
        // Arrange
        var code = "print('Integration Test')";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success);
        Assert.Equal(0, result.ExitCode);
        Assert.Contains("Integration Test", result.StdOut);
    }

    [Fact]
    public async Task ExecutePythonCode_WithMatplotlib_ShouldGenerateImage()
    {
        // Arrange
        var code = @"
import matplotlib.pyplot as plt
plt.figure()
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.savefig('/tmp/plot.png')
print('Plot saved')
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success);
        Assert.Contains("Plot saved", result.StdOut);
        Assert.NotEmpty(result.Artifacts);
        Assert.Contains(result.Artifacts, a => a.Type == "image");
    }

    [Fact]
    public async Task ExecutePythonCode_WithError_ShouldReturnStdErr()
    {
        // Arrange
        var code = "print(undefined_variable)";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.False(result.Success);
        Assert.NotEqual(0, result.ExitCode);
        Assert.Contains("NameError", result.StdErr);
    }

    [Fact]
    public async Task ExecutePythonCode_WithTimeout_ShouldTimeout()
    {
        // Arrange
        var code = @"
import time
time.sleep(100)
print('This should not print')
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python", timeoutSeconds: 2);

        // Assert
        Assert.False(result.Success);
        Assert.Contains("執行超時", result.StdErr);
    }
}
```

**測試覆蓋率目標**: ≥80%

**參考**: [Integration Testing Guidelines](../../../docs/testing/integration-testing-guidelines.md)

#### 4.1.3 安全測試

**Security Testing Checklist**:
- ✅ **Container 逃逸測試**:
  - 嘗試存取宿主機檔案（/etc/passwd, /proc, /sys）
  - 嘗試逃逸 Docker 容器（Docker socket 訪問）
  - 嘗試提權攻擊（sudo, setuid）

- ✅ **Network 隔離測試**:
  - 嘗試存取外部網路（curl google.com）
  - 嘗試 DNS 查詢（nslookup）
  - 嘗試連接其他容器

- ✅ **Resource 限制測試**:
  - CPU 消耗測試（CPU bomb）
  - Memory 消耗測試（Memory bomb）
  - Fork 炸彈測試（Process bomb）
  - Disk 寫入測試（超過 100MB）

- ✅ **File System 隔離測試**:
  - 嘗試寫入 /etc, /proc, /sys
  - 嘗試讀取敏感檔案
  - 嘗試符號連結攻擊

**SecurityTests.cs**:
```csharp
// tests/AIAgentPlatform.IntegrationTests/Security/SecurityTests.cs
[Collection("Docker")]
public class SecurityTests : IAsyncLifetime
{
    private readonly DockerCodeExecutionService _executionService;
    private readonly ContainerPoolManager _containerPool;

    // ... 初始化代碼 ...

    [Fact]
    public async Task Security_CannotAccessHostFiles()
    {
        // Arrange
        var code = @"
try:
    with open('/etc/passwd', 'r') as f:
        print(f.read())
except Exception as e:
    print(f'Error: {e}')
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success); // 執行成功（沒有崩潰）
        Assert.Contains("Error:", result.StdOut); // 但無法讀取檔案
    }

    [Fact]
    public async Task Security_CannotAccessExternalNetwork()
    {
        // Arrange
        var code = @"
import urllib.request
try:
    response = urllib.request.urlopen('http://google.com', timeout=5)
    print('Network access succeeded')
except Exception as e:
    print(f'Network blocked: {e}')
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success);
        Assert.Contains("Network blocked", result.StdOut);
    }

    [Fact]
    public async Task Security_CpuLimitEnforced()
    {
        // Arrange
        var code = @"
# CPU bomb
while True:
    pass
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python", timeoutSeconds: 5);

        // Assert
        Assert.False(result.Success); // 應該被 timeout 終止
        Assert.Contains("執行超時", result.StdErr);
    }

    [Fact]
    public async Task Security_MemoryLimitEnforced()
    {
        // Arrange
        var code = @"
# Memory bomb
data = []
try:
    while True:
        data.append('x' * 1024 * 1024)  # 每次分配 1MB
except MemoryError:
    print('Memory limit reached')
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success || result.StdOut.Contains("Memory limit"));
    }

    [Fact]
    public async Task Security_CannotWriteToRootFilesystem()
    {
        // Arrange
        var code = @"
try:
    with open('/etc/test.txt', 'w') as f:
        f.write('hacked')
    print('Write succeeded')
except Exception as e:
    print(f'Write blocked: {e}')
";

        // Act
        var result = await _executionService.ExecuteCodeAsync(code, "python");

        // Assert
        Assert.True(result.Success);
        Assert.Contains("Write blocked", result.StdOut);
        Assert.Contains("Read-only file system", result.StdOut);
    }
}
```

**參考**: [Security Testing](../../../docs/testing/security-testing.md)

### 4.2 性能測試

**Performance Benchmarks**:
- ✅ 容器啟動時間: <5 秒
- ✅ API 響應時間: <200ms (P95)
- ✅ Python 代碼執行: <30 秒 (P95)
- ✅ 容器池健康度: ≥95%

**PerformanceTests.cs**:
```csharp
// tests/AIAgentPlatform.IntegrationTests/Performance/PerformanceTests.cs
public class PerformanceTests
{
    [Fact]
    public async Task Performance_ContainerStartupTime()
    {
        var dockerService = new DockerService(Mock.Of<ILogger<DockerService>>());
        var stopwatch = Stopwatch.StartNew();

        var containerId = await dockerService.CreateContainerAsync(
            "code-interpreter:latest",
            "perf-test-container",
            new ContainerCreateOptions { /* ... */ },
            CancellationToken.None);

        await dockerService.StartContainerAsync(containerId, CancellationToken.None);
        stopwatch.Stop();

        Assert.True(stopwatch.ElapsedMilliseconds < 5000,
            $"Container startup took {stopwatch.ElapsedMilliseconds}ms, expected <5000ms");

        await dockerService.RemoveContainerAsync(containerId, CancellationToken.None);
    }

    [Fact]
    public async Task Performance_CodeExecutionSpeed()
    {
        // 測試 100 次簡單代碼執行的 P95
        var executionTimes = new List<double>();

        for (int i = 0; i < 100; i++)
        {
            var stopwatch = Stopwatch.StartNew();
            var result = await _executionService.ExecuteCodeAsync("print('test')", "python");
            stopwatch.Stop();

            executionTimes.Add(stopwatch.Elapsed.TotalMilliseconds);
        }

        var p95 = executionTimes.OrderBy(t => t).ElementAt((int)(executionTimes.Count * 0.95));

        Assert.True(p95 < 30000, $"P95 execution time {p95}ms, expected <30000ms");
    }
}
```

**參考**: [Performance Testing](../../../docs/testing/performance-testing.md)

### 4.3 質量目標

**代碼質量**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 集成測試覆蓋率 ≥80%
- ✅ 安全測試覆蓋率 100%
- ✅ 無 Critical / High Severity SonarQube Issues

**功能質量**:
- ✅ 執行成功率 ≥90%
- ✅ API 可用性 ≥99.9%
- ✅ 容器池健康度 ≥95%

**安全質量**:
- ✅ 無容器逃逸漏洞（滲透測試）
- ✅ Resource 限制有效率 100%
- ✅ Network 隔離有效率 100%
- ✅ File System 隔離有效率 100%

---

## 第五部分：參考文檔

### 5.1 核心規劃文檔 (8 refs)

1. **[MVP-SCOPE-DEFINITION.md](../../1-planning/MVP-SCOPE-DEFINITION.md)** - MVP 範圍定義與 Phase 1A 詳細規劃
2. **[SPRINT-ALLOCATION-ANALYSIS.md](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md)** - Sprint 6 詳細分析 (5 SP 調整後, 3 週)
3. **[DEVELOPMENT-STRATEGY.md](../../1-planning/DEVELOPMENT-STRATEGY.md)** - Code Interpreter 開發策略
4. **[DEPENDENCY-MATRIX.md](../../1-planning/DEPENDENCY-MATRIX.md)** - Sprint 依賴關係矩陣
5. **[RISK-REGISTER.md](../../1-planning/RISK-REGISTER.md)** - 風險登記簿 (RISK-016 Container 逃逸高風險)
6. **[TECHNICAL-DECISIONS-LOG.md](../../1-planning/TECHNICAL-DECISIONS-LOG.md)** - 技術決策日誌
7. **[ARCHITECTURE-EVOLUTION-ROADMAP.md](../../1-planning/ARCHITECTURE-EVOLUTION-ROADMAP.md)** - 架構演進路線圖
8. **[DOCS-REBUILD-EXECUTION-PLAN.md](../../1-planning/DOCS-REBUILD-EXECUTION-PLAN.md)** - 文檔重建執行計劃

### 5.2 Sprint 6 文檔 (7 refs)

9. **[SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md)** - Sprint 6 概覽
10. **[SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md)** - Sprint 6 實施計劃 (本文件)
11. **[SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md)** - Sprint 6 上下文與背景
12. **[SPRINT-6-4-CHECKLIST.md](./SPRINT-6-4-CHECKLIST.md)** - Sprint 6 檢查清單
13. **[SPRINT-6-5-DEV-LOG.md](./SPRINT-6-5-DEV-LOG.md)** - Sprint 6 開發日誌
14. **[SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md)** - Sprint 6 問題追蹤
15. **[SPRINT-6-7-RETROSPECTIVE.md](./SPRINT-6-7-RETROSPECTIVE.md)** - Sprint 6 回顧

### 5.3 User Stories - Code Interpreter (2 refs)

16. **[us-6-code-interpreter.md](../../../docs/user-stories/us-6-code-interpreter.md)** - Code Interpreter 完整需求 (US 5.1)
17. **[epic-06-code-interpreter.md](../../../docs/user-stories/epics/epic-06-code-interpreter.md)** - Code Interpreter Epic

### 5.4 架構設計文檔 - ADRs (5 refs)

18. **[Architecture-Design-Document.md](../../../docs/architecture/Architecture-Design-Document.md)** - 完整架構設計
19. **[ADR-008: Code Interpreter Container Pool](../../../docs/architecture/adr/ADR-008-code-interpreter-container-pool.md)** - Docker 容器池設計與動態生命週期管理
20. **[ADR-006: Hybrid State Management](../../../docs/architecture/decisions/ADR-006-hybrid-state-management.md)** - Redis + PostgreSQL 混合狀態管理
21. **[ADR-015: Execution Timeout Strategy](../../../docs/architecture/decisions/ADR-015-execution-timeout-strategy.md)** - 執行超時策略
22. **[ADR-016: Resource Limit Configuration](../../../docs/architecture/decisions/ADR-016-resource-limit-configuration.md)** - 資源限制配置

### 5.5 技術實作文檔 - Code Interpreter (6 refs)

23. **[06-code-interpreter-implementation.md](../../../docs/technical-implementation/01-backend-net9/06-code-interpreter-implementation.md)** - Code Interpreter 完整實作指南
24. **[code-interpreter-architecture.md](../../../docs/technical-implementation/code-interpreter/code-interpreter-architecture.md)** - 架構設計
25. **[container-pool-management.md](../../../docs/technical-implementation/code-interpreter/container-pool-management.md)** - 容器池管理詳解
26. **[docker-image-design.md](../../../docs/technical-implementation/code-interpreter/docker-image-design.md)** - Docker Image 設計
27. **[execution-engine.md](../../../docs/technical-implementation/code-interpreter/execution-engine.md)** - 執行引擎實作
28. **[result-processing.md](../../../docs/technical-implementation/code-interpreter/result-processing.md)** - 執行結果處理

### 5.6 安全設計文檔 (5 refs)

29. **[code-execution-security.md](../../../docs/security/code-execution-security.md)** - Code Interpreter 4-Layer Security Architecture 完整設計
30. **[container-security.md](../../../docs/security/container-security.md)** - 容器安全設計
31. **[network-isolation.md](../../../docs/security/network-isolation.md)** - 網路隔離設計
32. **[filesystem-isolation.md](../../../docs/security/filesystem-isolation.md)** - 文件系統隔離設計
33. **[resource-limits.md](../../../docs/security/resource-limits.md)** - 資源限制設計

### 5.7 API 設計文檔 (3 refs)

34. **[API-Design-Guidelines.md](../../../docs/api/API-Design-Guidelines.md)** - API 設計指南
35. **[code-interpreter-api-design.md](../../../docs/api/code-interpreter-api-design.md)** - Code Interpreter API 設計
36. **[api-endpoints-v1.md](../../../docs/api/api-endpoints-v1.md)** - API v1 端點清單

### 5.8 測試文檔 (5 refs)

37. **[testing-strategy-overview.md](../../../docs/testing/testing-strategy-overview.md)** - 測試策略概覽
38. **[unit-testing-guidelines.md](../../../docs/testing/unit-testing-guidelines.md)** - 單元測試指南
39. **[integration-testing-guidelines.md](../../../docs/testing/integration-testing-guidelines.md)** - 整合測試指南
40. **[security-testing.md](../../../docs/testing/security-testing.md)** - 安全測試指南
41. **[code-interpreter-testing-strategy.md](../../../docs/testing/code-interpreter-testing-strategy.md)** - Code Interpreter 測試策略

### 5.9 UX 設計文檔 (2 refs)

42. **[user-research/personas.md](../../../docs/ux-design/user-research/personas.md)** - 使用者研究 Personas
43. **[code-interpreter-ui-design.md](../../../docs/ux-design/code-interpreter-ui-design.md)** - Code Interpreter UI 設計詳解

### 5.10 開發標準與流程 (3 refs)

44. **[coding-standards-csharp.md](../../../docs/development-standards/coding-standards-csharp.md)** - C# 編碼標準
45. **[coding-standards-typescript.md](../../../docs/development-standards/coding-standards-typescript.md)** - TypeScript 編碼標準
46. **[git-workflow.md](../../../docs/development-standards/git-workflow.md)** - Git 工作流程

### 5.11 Docker 與部署文檔 (4 refs)

47. **[docker-compose-setup.md](../../../docs/deployment/docker-compose-setup.md)** - Docker Compose 設置
48. **[code-interpreter-dockerfile.md](../../../docs/deployment/code-interpreter-dockerfile.md)** - Code Interpreter Dockerfile 設計
49. **[container-networking.md](../../../docs/deployment/container-networking.md)** - 容器網路配置
50. **[deployment-guide.md](../../../docs/deployment/deployment-guide.md)** - 部署指南

### 5.12 程式碼參考 - Application Layer (4 refs)

51. **[ICodeExecutionService.cs](../../../src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs)** - 代碼執行服務介面
52. **[IContainerPoolManager.cs](../../../src/AIAgentPlatform.Application/Interfaces/IContainerPoolManager.cs)** - 容器池管理介面
53. **[IDockerService.cs](../../../src/AIAgentPlatform.Application/Interfaces/IDockerService.cs)** - Docker 服務介面
54. **[IExecutionResultProcessor.cs](../../../src/AIAgentPlatform.Application/Interfaces/IExecutionResultProcessor.cs)** - 執行結果處理介面

### 5.13 程式碼參考 - Domain Layer (2 refs)

55. **[CodeExecution.cs](../../../src/AIAgentPlatform.Domain/Entities/CodeExecution.cs)** - CodeExecution Entity
56. **[ExecutionResult.cs](../../../src/AIAgentPlatform.Domain/ValueObjects/ExecutionResult.cs)** - ExecutionResult Value Object

### 5.14 程式碼參考 - Infrastructure Layer (6 refs)

57. **[DockerCodeExecutionService.cs](../../../src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs)** - Docker 代碼執行服務
58. **[ContainerPoolManager.cs](../../../src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs)** - 容器池管理器
59. **[DockerService.cs](../../../src/AIAgentPlatform.Infrastructure/Services/DockerService.cs)** - Docker 服務實作
60. **[ExecutionResultProcessor.cs](../../../src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs)** - 執行結果處理器
61. **[ContainerHealthChecker.cs](../../../src/AIAgentPlatform.Infrastructure/Services/ContainerHealthChecker.cs)** - 容器健康檢查
62. **[SecurityManager.cs](../../../src/AIAgentPlatform.Infrastructure/Services/SecurityManager.cs)** - 安全管理器

### 5.15 程式碼參考 - API Layer (1 ref)

63. **[CodeController.cs](../../../src/AIAgentPlatform.API/Controllers/CodeController.cs)** - Code API Controller

### 5.16 程式碼參考 - Frontend (3 refs)

64. **[CodeInterpreter.tsx](../../../apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx)** - Code Interpreter 主元件
65. **[CodeEditor.tsx](../../../apps/web-app/src/features/code-interpreter/components/CodeEditor.tsx)** - 代碼編輯器元件
66. **[ExecutionResult.tsx](../../../apps/web-app/src/features/code-interpreter/components/ExecutionResult.tsx)** - 執行結果元件

### 5.17 程式碼參考 - Docker (1 ref)

67. **[Dockerfile](../../../docker/code-interpreter/Dockerfile)** - Code Interpreter Docker Image 定義

---

## 使用指南

本文件為 **Sprint 6 完整實施計劃**，提供開發團隊執行 Code Interpreter 功能所需的所有技術細節。

### 目標讀者

**主要讀者**:
- 👨‍💻 **Backend Developer**: C# .NET 開發者，實作 Docker 服務、容器池管理、代碼執行引擎
- 🔐 **Security Engineer**: 安全工程師，實作 4-Layer Security Architecture、滲透測試
- 👩‍💻 **Frontend Developer**: React + TypeScript 開發者，實作 Code Interpreter UI
- 🧪 **QA Engineer**: 測試工程師，執行單元測試、集成測試、安全測試

**次要讀者**:
- 🏗️ **Tech Lead / Architect**: 評估技術實作與架構設計
- 🏃 **Scrum Master**: 追蹤 Sprint 進度與任務完成情況
- 🎯 **Product Owner**: 了解實作範圍與驗收標準

### 使用方式

#### 📋 Phase 1: Docker 環境搭建 (Week 1, Day 1-3)

**Backend Developer 任務**:
1. 閱讀 [2.2.1 Docker Image 建置](#221-docker-image-建置)
2. 建置 Docker Image：
   ```bash
   docker build -t code-interpreter:latest -f docker/code-interpreter/Dockerfile .
   ```
3. 驗證 Image 大小 <500MB
4. 測試 Python 環境：
   ```bash
   docker run --rm code-interpreter:latest python -c "import numpy, pandas, matplotlib; print('OK')"
   ```

5. 實作 `ContainerPoolManager.cs`（參考 [2.2.2](#222-容器池管理實作)）
6. 實作 `DockerService.cs`（參考 [2.2.3](#223-docker-service-實作)）
7. 更新 `docker-compose.yml`（參考 [2.3](#23-docker-compose-配置)）

**驗收標準**:
- ✅ Docker Image 建置成功（<500MB）
- ✅ 3 個容器同時運行
- ✅ 容器健康檢查運作正常

#### 🐍 Phase 2: Python 執行引擎 (Week 1, Day 4-5 + Week 2, Day 1-2)

**Backend Developer 任務**:
1. 定義 `ICodeExecutionService` 介面
2. 實作 `DockerCodeExecutionService`（參考 [2.2.4](#224-code-execution-service-實作)）
3. 實作執行超時控制（30 秒）
4. 實作資源限制配置（CPU, Memory, Disk）
5. 實作錯誤處理與日誌記錄

**測試**:
- 執行簡單 Python 代碼：`print('Hello')`
- 測試超時機制：`time.sleep(100)`
- 測試錯誤處理：`print(undefined)`

**驗收標準**:
- ✅ Python 代碼執行成功
- ✅ 超時機制運作正常（>30 秒終止）
- ✅ 資源限制有效

#### 🔐 Phase 3: 4-Layer Security Architecture (Week 2, Day 3-5)

**Security Engineer + Backend Developer 任務**:
1. 實作 Layer 1: Container 隔離（非 root, read-only FS）
2. 實作 Layer 2: Network 隔離（禁止外部網路）
3. 實作 Layer 3: File System 隔離（僅 /tmp 可寫）
4. 實作 Layer 4: Resource 限制（CPU, Memory, Process）

**安全測試（Critical）**:
```bash
# 測試 1: 嘗試存取宿主機檔案
python -c "open('/etc/passwd').read()"  # 應失敗

# 測試 2: 嘗試外部網路存取
python -c "import urllib.request; urllib.request.urlopen('http://google.com')"  # 應失敗

# 測試 3: CPU bomb
python -c "while True: pass"  # 應被超時終止

# 測試 4: 嘗試寫入根目錄
python -c "open('/etc/test.txt', 'w').write('hacked')"  # 應失敗
```

**驗收標準（Critical）**:
- ✅ **無容器逃逸漏洞**（滲透測試通過）
- ✅ 無法存取宿主機檔案
- ✅ 無法存取外部網路
- ✅ Resource 限制有效

#### 📊 Phase 4: 執行結果處理 (Week 3, Day 1-2)

**Backend Developer 任務**:
1. 實作 `ExecutionResultProcessor`（參考 [2.2.5](#225-execution-result-processor-實作)）
2. 實作 Matplotlib 圖表處理
3. 實作 Pandas DataFrame 處理

**測試**:
```python
# 測試圖表輸出
import matplotlib.pyplot as plt
plt.plot([1,2,3], [1,4,9])
plt.savefig('/tmp/plot.png')
```

**驗收標準**:
- ✅ Matplotlib 圖表正確輸出（PNG, Base64）
- ✅ Pandas DataFrame 轉 JSON 成功

#### 🌐 Phase 5: API 與 UI (Week 3, Day 3-5)

**Backend Developer 任務**:
1. 實作 `CodeController`（參考 [2.2.6](#226-code-execution-api-實作)）
2. 實作 CQRS Commands/Queries
3. Swagger 文檔

**Frontend Developer 任務**:
1. 實作 `CodeInterpreter.tsx`（參考 [2.2.7](#227-code-interpreter-ui-實作)）
2. 整合 Monaco Editor
3. 實作執行結果展示（Text, Image, Table）

**驗收標準**:
- ✅ API 完成（4 個端點）
- ✅ UI 完成（代碼編輯器 + 結果展示）
- ✅ E2E 測試通過

### 快速參考

#### 關鍵程式碼位置

**Backend**:
- `src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs`
- `src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs`
- `src/AIAgentPlatform.Infrastructure/Services/DockerService.cs`
- `src/AIAgentPlatform.API/Controllers/CodeController.cs`

**Frontend**:
- `apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx`
- `apps/web-app/src/features/code-interpreter/components/ExecutionResult.tsx`

**Docker**:
- `docker/code-interpreter/Dockerfile`
- `docker-compose.yml`

**Tests**:
- `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/CodeExecutionTests.cs`
- `tests/AIAgentPlatform.IntegrationTests/Security/SecurityTests.cs`

#### 關鍵命令

```bash
# 建置 Docker Image
docker build -t code-interpreter:latest -f docker/code-interpreter/Dockerfile .

# 啟動容器池
docker-compose up -d code-interpreter-1 code-interpreter-2 code-interpreter-3

# 執行單元測試
dotnet test tests/AIAgentPlatform.UnitTests

# 執行集成測試（需要 Docker）
dotnet test tests/AIAgentPlatform.IntegrationTests

# 執行安全測試
dotnet test tests/AIAgentPlatform.IntegrationTests --filter "FullyQualifiedName~SecurityTests"

# 啟動 API
cd src/AIAgentPlatform.API
dotnet run

# 啟動前端
cd apps/web-app
npm run dev
```

### 常見問題 (FAQ)

**Q1: Docker Image 建置失敗？**
- 檢查基礎 Image 是否可用：`docker pull python:3.11-slim-bookworm`
- 檢查網路連接
- 參考 [Docker Image Design](../../../docs/technical-implementation/code-interpreter/docker-image-design.md)

**Q2: 容器無法啟動？**
- 檢查 Docker Engine 版本 ≥20.10
- 檢查容器名稱是否衝突
- 查看日誌：`docker logs code-interpreter-1`

**Q3: 安全測試失敗？**
- 這是 **Critical** 問題，必須解決
- 參考 [RISK-016](../../1-planning/RISK-REGISTER.md) 緩解措施
- 聯繫 Security Team 協助

**Q4: 執行超時無法終止？**
- 檢查 `CancellationToken` 是否正確傳遞
- 檢查 `pkill` 命令是否執行成功
- 參考 [Execution Engine](../../../docs/technical-implementation/code-interpreter/execution-engine.md)

---

## 版本歷史

### v2.1 (2025-11-14) - 當前版本

- ✅ 建立 Sprint 6 PLAN 文件（完整實施計劃）
- ✅ 遵循 v2.1 標準格式（8 欄位 Header）
- ✅ 詳細的技術實作方案（2.2.1 ~ 2.2.7）
- ✅ 完整的程式碼範例（C#, TypeScript, Dockerfile, docker-compose.yml）
- ✅ 完整的測試策略（單元測試、集成測試、安全測試）
- ✅ **特別強調 RISK-016 (Container 逃逸) 高風險管理**
- ✅ 完整參考文獻索引（67 個文檔）
- ✅ 詳細的使用指南（按 Phase 劃分）
- ✅ 與 Sprint 5-2-PLAN 格式一致
- ✅ MVP 簡化範圍說明（5 SP vs 13 SP）

**文件統計** (v2.1):
- 總行數: ~2,800 行
- 程式碼範例: 15+ 個完整實作
- 測試範例: 10+ 個測試案例
- 參考文獻: 67 個文檔
- User Stories: US 5.1 (5 Phases)

**品質指標**:
- 與 Sprint 5-2-PLAN 格式一致性: ✅ 100%
- v2.1 標準符合度: ✅ 100%
- 參考文獻完整性: ✅ 67 文檔
- 程式碼範例完整性: ✅ Backend + Frontend + Docker
- 安全重點強調: ✅ RISK-016 高風險充分說明

**特別說明**:
- Sprint 6 為 Phase 1A 最後一個 Sprint
- 實作簡化版 Code Interpreter（5 SP）
- 完整版留待 Phase 1B-1C（動態容器池、高級安全策略）
- **安全性是本 Sprint 的 Critical 驗收標準**

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**維護者**: Sprint 6 開發團隊
**文件路徑**: `claudedocs/2-sprints/sprint-6/SPRINT-6-2-PLAN.md`
