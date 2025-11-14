# SPRINT-6-4-CHECKLIST.md - Sprint 6 任務清單：Code Interpreter 安全執行環境

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

### 核心章節
1. [總體進度統計](#總體進度統計)
2. [項目準備](#項目準備)
3. [US 5.1 - Code Interpreter (5 SP, 5 Phases)](#us-51---code-interpreter-5-sp-5-phases)
   - 3.1 [Phase 1: Docker 環境搭建](#phase-1-docker-環境搭建-15-sp-)
   - 3.2 [Phase 2: Python 執行引擎實作](#phase-2-python-執行引擎實作-15-sp-)
   - 3.3 [Phase 3: 4-Layer Security Architecture](#phase-3-4-layer-security-architecture-1-sp-)
   - 3.4 [Phase 4: 執行結果處理與可視化](#phase-4-執行結果處理與可視化-05-sp-)
   - 3.5 [Phase 5: Code Interpreter API 與 UI](#phase-5-code-interpreter-api-與-ui-05-sp-)
4. [測試](#測試)
5. [安全驗證](#安全驗證)
6. [文檔](#文檔)
7. [部署與DevOps](#部署與devops)
8. [Definition of Done 驗證](#definition-of-done-驗證)

### 輔助章節
- [使用指南](#使用指南)
- [參考文獻索引](#參考文獻索引)
- [版本歷史](#版本歷史)

---

## 📖 使用指南

### 文件目的
本文件為 Sprint 6 的詳細任務檢查清單，按 **User Story (US)** 組織，追蹤所有待辦事項的執行狀態和完成進度。

### 目標讀者
- **開發團隊**：日常開發任務的核心參考文件，追蹤工作進度
- **Scrum Master / PM**：Sprint 進度追蹤與風險識別
- **QA 團隊**：測試範圍與驗收標準的參考（特別是安全測試）
- **Security Team**：安全配置與滲透測試的執行清單
- **AI Assistant**：任務狀態查詢與進度更新

### 使用方式
1. **每日開發**：查看對應 User Story 的 Phase 任務清單，更新完成狀態
2. **進度追蹤**：查看總體進度統計表，了解 Sprint 整體進度
3. **任務分配**：依據 Phase 劃分合理分配開發任務
4. **完成驗證**：使用 Definition of Done 驗證章節確保質量標準
5. **更新文件**：完成任務後立即更新檢查框狀態 [x]
6. **安全測試重點**：Week 2 必須完成滲透測試，確保無容器逃逸漏洞

### 快速導航
- **查看 Sprint 6 整體概覽** → [SPRINT-6-1-OVERVIEW.md](./SPRINT-6-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md)
- **查看技術上下文** → [SPRINT-6-3-CONTEXT.md](./SPRINT-6-3-CONTEXT.md)
- **查看開發日誌** → [SPRINT-6-5-DEV-LOG.md](./SPRINT-6-5-DEV-LOG.md)
- **查看問題追蹤** → [SPRINT-6-6-ISSUES.md](./SPRINT-6-6-ISSUES.md)

### 狀態標記說明
- ✅ **已完成** - 項目已完成並通過驗證
- 🔄 **進行中** - 項目正在執行中
- ⏳ **待開始** - 項目已計劃但尚未開始
- ⚠️ **有問題** - 項目遇到阻礙需要關注
- ❌ **失敗** - 項目未通過驗證需要重做
- 🔒 **安全重點** - 安全相關任務，需特別注意

### 優先級標記
- **P0** - 必須完成的項目，影響 Sprint 交付
- **P1** - 重要但非阻塞的項目，建議完成
- **SECURITY** - 安全相關任務，必須通過審核

---

## 📊 總體進度統計

| 類別 | 已完成 / 總數 | 進度 | 狀態 |
|------|------------|------|------|
| 項目準備 | 0/10 | 0% | ⏳ |
| US 5.1 - Code Interpreter | 0/78 | 0% | ⏳ |
| 測試 | 0/25 | 0% | ⏳ |
| 安全驗證 | 0/12 | 0% | ⏳ |
| 文檔 | 0/8 | 0% | ⏳ |
| 部署 | 0/10 | 0% | ⏳ |
| **總計** | **0/143** | **0%** | ⏳ |

**圖例**: ✅ 已完成 | 🔄 進行中 | ⏳ 待開始 | ❌ 阻塞 | 🔒 安全重點

**Story Points 分配**:
- US 5.1: Code Interpreter (5 SP, 簡化版)
- **總計**: 5 SP

**重要里程碑**:
- Week 1 完成：Docker 環境 + Python 執行引擎
- Week 2 完成：4-Layer Security + 滲透測試 🔒
- Week 3 完成：API/UI + 整合測試

---

## 項目準備

### 環境設置 (P0)
- [ ] 更新開發環境 (`git pull origin main`)
- [ ] 創建 Feature Branch (`git checkout -b feature/us-5.1-code-interpreter`)
- [ ] 安裝依賴 (`dotnet restore`, `pnpm install`)
- [ ] 驗證資料庫連接正常 (PostgreSQL, Redis)
- [ ] 檢查 Sprint 6 所有文檔
- [ ] 驗證 Docker Engine 版本 (≥20.10)
- [ ] 安裝 Docker.DotNet NuGet 套件
- [ ] 準備測試用 Python 腳本
- [ ] 配置 Azure Blob Storage (圖片存儲)
- [ ] 檢查網路安全政策 (Docker network isolation)

### Sprint Planning (P0)
- [ ] 閱讀 [US 5.1 規格](../../docs/user-stories/modules/module-03-code-interpreter.md) 與 MVP 範圍
- [ ] 閱讀 [ADR-008: Code Interpreter Container Pool](../../docs/architecture/ADR-008-code-interpreter-execution-model.md)
- [ ] 閱讀 [Code Execution Security](../../docs/technical-implementation/08-SECURITY-ARCHITECTURE.md)
- [ ] 閱讀 [PoC3 Security Hardening](../../docs/technical-implementation/10-POC3-SECURITY-HARDENING.md)
- [ ] 確認 Phase 實施順序
- [ ] 規劃 Docker Image 設計 (python:3.11-slim-bookworm)
- [ ] 規劃容器池大小 (固定 3 個容器)
- [ ] 規劃 4-Layer Security 配置
- [ ] 規劃測試策略 (Unit, Integration, Security Tests)
- [ ] 規劃滲透測試時程 (Week 2) 🔒

**驗收標準**:
- ✅ 開發環境就緒
- ✅ Feature Branch 創建成功
- ✅ 所有 Sprint 6 規格文檔已閱讀
- ✅ Docker Engine 與 Docker.DotNet 可用
- ✅ 安全測試計劃已制定 🔒

---

## US 5.1: Code Interpreter (5 SP, 5 Phases)

### Phase 1: Docker 環境搭建 (1.5 SP) ⏳ 待開始

#### Docker Image 設計與建置 (P0)
- [ ] **創建 Dockerfile**: `docker/code-interpreter/Dockerfile`
  - Base Image: `python:3.11-slim-bookworm`
  - 預安裝套件: numpy, pandas, matplotlib, seaborn
  - 非 root 用戶: `sandbox` (UID 1000)
  - 工作目錄: `/home/sandbox`
  - 參考: [ADR-008](../../docs/architecture/ADR-008-code-interpreter-execution-model.md) Lines 148-202

- [ ] **配置 Python 環境**:
  - 安裝 NumPy 1.24+
  - 安裝 Pandas 2.0+
  - 安裝 Matplotlib 3.7+
  - 安裝 Seaborn 0.12+
  - 配置中文字體支援 (Microsoft YaHei 或 Noto Sans CJK)
  - 禁用 pip install (移除 pip 或限制權限)

- [ ] **優化 Image 大小**:
  - 使用 slim 基礎鏡像
  - 清理 apt cache
  - 移除不必要的套件
  - 目標 Image 大小: <500MB
  - 參考: [SPRINT-6-2-PLAN.md](./SPRINT-6-2-PLAN.md) Lines 150-250

- [ ] **建置與測試 Image**:
  - 執行 `docker build -t code-sandbox-python:latest .`
  - 驗證 Image 大小 <500MB
  - 驗證 Python 版本 3.11+
  - 驗證預安裝套件可用
  - 驗證非 root 用戶 (UID 1000)

#### 容器池設計與實作 (P0)
- [ ] **創建 IContainerPoolManager Interface**: `src/AIAgentPlatform.Application/Interfaces/IContainerPoolManager.cs`
  - Methods: `AcquireAsync`, `ReleaseAsync`, `GetAvailableCountAsync`
  - 參考: [ADR-008](../../docs/architecture/ADR-008-code-interpreter-execution-model.md) Lines 43-48

- [ ] **實作 ContainerPoolManager**: `src/AIAgentPlatform.Infrastructure/Services/ContainerPoolManager.cs`
  - 固定容器池大小: 3 個容器
  - 預熱啟動: 初始化時創建 3 個容器
  - 容器獲取: <2 秒 (從池中)
  - 容器釋放: 銷毀後補充池
  - 健康檢查: 每 30 秒檢查容器狀態
  - 參考: [ADR-008](../../docs/architecture/ADR-008-code-interpreter-execution-model.md) Lines 52-229

- [ ] **實作容器生命週期管理**:
  - 容器啟動邏輯
  - 容器停止邏輯
  - 容器移除邏輯
  - 容器健康檢查
  - 異常容器自動替換

- [ ] **實作容器補充邏輯**:
  - 異步非阻塞補充
  - 保持池中始終有 3 個容器
  - 補充失敗重試 (最多 3 次)
  - 記錄補充日誌

#### Docker Compose 整合 (P0)
- [ ] **更新 docker-compose.yml**:
  - 添加 code-interpreter 服務
  - 配置網路隔離 (network: none)
  - 配置 Volume 掛載 (tmpfs for /tmp)
  - 參考: `docker-compose.yml` 現有配置

- [ ] **配置網路隔離**:
  - 創建獨立網路: `code-sandbox-network`
  - 配置 network mode: none
  - 禁止外部網路訪問

- [ ] **配置 Volume 掛載**:
  - /tmp: tmpfs (rw, noexec, nosuid, size=100m)
  - /home/sandbox: bind mount (rw)

#### Unit Tests (P0)
- [ ] **ContainerPoolManager 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/ContainerPoolManagerTests.cs`
  - `InitializeAsync_CreatesThreeContainers_Success`
  - `AcquireAsync_FromPool_ReturnsContainerInTwoSeconds`
  - `ReleaseAsync_DestroysContainer_AndReplenishesPool`
  - `GetAvailableCountAsync_ReturnsCorrectCount`
  - `HealthCheck_ReplacesUnhealthyContainer`
  - 測試覆蓋率: ≥85%

- [ ] **Docker Image 測試**: `tests/AIAgentPlatform.IntegrationTests/Docker/DockerImageTests.cs`
  - `DockerImage_PythonVersion_Is311OrHigher`
  - `DockerImage_PreinstalledPackages_AreAvailable`
  - `DockerImage_Size_IsLessThan500MB`
  - `DockerImage_NonRootUser_IsConfigured`
  - 測試覆蓋率: 100%

**驗收標準 (Phase 1)**:
- ✅ Docker Image 建置成功 (Image 大小 <500MB)
- ✅ 容器池啟動成功 (3 個容器同時運行)
- ✅ 容器健康檢查運作正常
- ✅ 容器網路隔離有效
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 2: Python 執行引擎實作 (1.5 SP) ⏳ 待開始

#### Application Layer - Interfaces (P0)
- [ ] **創建 ICodeExecutionService Interface**: `src/AIAgentPlatform.Application/Interfaces/ICodeExecutionService.cs`
  - Method: `ExecuteCodeAsync(code, language, timeout, resourceLimits)`
  - Return Type: `CodeExecutionResult`
  - 參考: [ADR-008](../../docs/architecture/ADR-008-code-interpreter-execution-model.md) Lines 248-302

- [ ] **創建 IDockerService Interface**: `IDockerService.cs`
  - Methods: `CreateContainerAsync`, `StartContainerAsync`, `StopContainerAsync`, `RemoveContainerAsync`
  - Return Type: Container-specific DTOs

- [ ] **創建 CodeExecutionResult DTO**: `CodeExecutionResult.cs`
  - Properties: Success, Output, Error, ExitCode, ExecutionTimeMs, Files
  - Success/Failure Factory Methods
  - 參考: [ADR-008](../../docs/architecture/ADR-008-code-interpreter-execution-model.md) Lines 415-423

#### Infrastructure Layer - Docker Integration (P0)
- [ ] **安裝 Docker.DotNet NuGet Package**: `Docker.DotNet` ≥3.125.0
  - 驗證套件安裝成功
  - 配置 Docker Client

- [ ] **實作 DockerCodeExecutionService**: `src/AIAgentPlatform.Infrastructure/Services/DockerCodeExecutionService.cs`
  - 整合 Docker.DotNet API
  - 實現代碼提交到容器
  - 實現執行結果擷取 (stdout, stderr, exit code)
  - 實現執行超時控制 (預設 30 秒)
  - 實現錯誤處理與日誌記錄
  - 參考: [ADR-008](../../docs/architecture/ADR-008-code-interpreter-execution-model.md) Lines 235-412

- [ ] **實作執行流程**:
  - 步驟 1: 從 ContainerPool 獲取容器
  - 步驟 2: 將代碼寫入容器 /tmp/script.py
  - 步驟 3: 執行 `python /tmp/script.py`
  - 步驟 4: 收集 stdout, stderr, exit code
  - 步驟 5: 收集生成的檔案 (圖片等)
  - 步驟 6: 釋放容器 (銷毀)

- [ ] **實作超時控制**:
  - 使用 CancellationTokenSource
  - 預設超時: 30 秒
  - 超時時強制終止容器
  - 返回超時錯誤訊息

#### Resource Limits Configuration (P0)
- [ ] **配置 CPU 限制**:
  - CPU 限制: 1 core
  - 配置: `--cpus=1` 或 `NanoCPUs=1_000_000_000`

- [ ] **配置 Memory 限制**:
  - Memory 限制: 512MB
  - Memory Swap: 512MB (禁用 swap)
  - 配置: `--memory=512m --memory-swap=512m`

- [ ] **配置 Disk 限制**:
  - Disk 限制: 100MB
  - /tmp 大小: 100MB
  - 配置: `--tmpfs /tmp:size=100m`

- [ ] **配置 Process 限制**:
  - Process 限制: 100 個
  - 配置: `--pids-limit=100`

#### Error Handling & Logging (P0)
- [ ] **實作錯誤處理**:
  - 執行失敗處理 (exit code != 0)
  - 超時處理 (OperationCanceledException)
  - 容器崩潰處理
  - 資源超限處理

- [ ] **實作結構化日誌記錄**:
  - 記錄執行開始 (containerId, code)
  - 記錄執行結果 (success, output, error, executionTime)
  - 記錄執行失敗 (errorType, errorMessage, stackTrace)
  - 使用 Serilog 結構化日誌

#### Unit Tests (P0)
- [ ] **DockerCodeExecutionService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/DockerCodeExecutionServiceTests.cs`
  - `ExecuteCodeAsync_SimplePrint_ReturnsOutput`
  - `ExecuteCodeAsync_SyntaxError_ReturnsError`
  - `ExecuteCodeAsync_Timeout_ReturnsTimeoutError`
  - `ExecuteCodeAsync_ExceedsMemoryLimit_ReturnsError`
  - `ExecuteCodeAsync_GeneratesPlot_ReturnsFile`
  - 測試覆蓋率: ≥85%

- [ ] **Resource Limits 測試**: `ResourceLimitsTests.cs`
  - `ExecuteCode_ExceedsCpuLimit_IsThrottled`
  - `ExecuteCode_ExceedsMemoryLimit_IsKilled`
  - `ExecuteCode_ExceedsDiskLimit_FailsToWrite`
  - `ExecuteCode_ExceedsProcessLimit_CannotFork`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Code Execution 整合測試**: `tests/AIAgentPlatform.IntegrationTests/CodeExecution/CodeExecutionTests.cs`
  - `ExecuteCode_SimplePython_Success`
  - `ExecuteCode_DataAnalysis_WithPandas_Success`
  - `ExecuteCode_Visualization_WithMatplotlib_Success`
  - `ExecuteCode_Timeout_ReturnsError`
  - 使用真實 Docker 容器
  - 測試覆蓋率: 100%

**驗收標準 (Phase 2)**:
- ✅ ICodeExecutionService 介面定義完整
- ✅ Python 代碼執行成功 (簡單 print 測試)
- ✅ 執行超時機制運作正常
- ✅ 資源限制有效 (CPU, Memory, Disk, Process)
- ✅ 錯誤處理和日誌記錄完整
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 3: 4-Layer Security Architecture (1 SP) ⏳ 待開始 🔒

#### Layer 1: Network Isolation (P0, SECURITY)
- [ ] **配置 Network Isolation**:
  - Docker Network Mode: `none`
  - 禁止外部網路訪問
  - 僅允許 localhost 存取
  - 參考: [Code Execution Security](../../docs/technical-implementation/08-SECURITY-ARCHITECTURE.md) Lines 468-484

- [ ] **驗證 Network Isolation**:
  - 測試無法訪問外網 (google.com)
  - 測試無法訪問內網資源
  - 測試無法創建 Socket
  - 參考: [PoC3 Security](../../docs/technical-implementation/10-POC3-SECURITY-HARDENING.md) Lines 795-802

#### Layer 2: Resource Limits (P0, SECURITY)
- [ ] **配置 Resource Limits**:
  - CPU 限制: 1 core
  - Memory 限制: 512MB
  - Disk 限制: 100MB
  - Process 限制: 100 個
  - 參考: [Code Execution Security](../../docs/technical-implementation/08-SECURITY-ARCHITECTURE.md) Lines 486-505

- [ ] **驗證 Resource Limits**:
  - 測試 CPU 耗盡攻擊 (死循環)
  - 測試 Memory 炸彈
  - 測試 Disk 寫入攻擊
  - 測試 Fork Bomb

#### Layer 3: Execution Timeout (P0, SECURITY)
- [ ] **配置 Execution Timeout**:
  - 預設超時: 30 秒
  - 強制終止容器
  - 返回超時錯誤
  - 參考: [Code Execution Security](../../docs/technical-implementation/08-SECURITY-ARCHITECTURE.md) Lines 507-523

- [ ] **驗證 Execution Timeout**:
  - 測試無限循環
  - 測試長時間運算
  - 測試超時後容器已終止

#### Layer 4: Read-Only Filesystem (P0, SECURITY)
- [ ] **配置 Read-Only Filesystem**:
  - ReadonlyRootfs: `true`
  - /tmp: tmpfs (rw, noexec, nosuid, size=100m)
  - 禁止存取敏感目錄 (/etc, /proc, /sys)
  - 參考: [Code Execution Security](../../docs/technical-implementation/08-SECURITY-ARCHITECTURE.md) Lines 525-544

- [ ] **驗證 Read-Only Filesystem**:
  - 測試無法寫入 /etc
  - 測試無法寫入 /usr
  - 測試 /tmp 可寫
  - 測試無法執行二進制文件 (noexec)

#### Advanced Security (Optional - Phase 2 完整版) 🔒
- [ ] **Seccomp Profile (Optional)**:
  - 創建 Seccomp Profile JSON
  - 允許 40 個核心 syscalls
  - 阻斷危險 syscalls (mount, ptrace, reboot)
  - 參考: [PoC3 Security](../../docs/technical-implementation/10-POC3-SECURITY-HARDENING.md) Lines 89-312

- [ ] **AppArmor Profile (Optional)**:
  - 創建 AppArmor Profile
  - 阻斷 /proc/1/cgroup 讀取
  - 阻斷 /sys 讀取
  - 參考: [PoC3 Security](../../docs/technical-implementation/10-POC3-SECURITY-HARDENING.md) Lines 384-467

#### Security Tests (P0, SECURITY) 🔒
- [ ] **Security Test Suite**: `tests/AIAgentPlatform.IntegrationTests/Security/SecurityTests.cs`
  - `Test_NetworkIsolation_CannotAccessInternet`
  - `Test_NetworkIsolation_CannotCreateSocket`
  - `Test_ResourceLimit_CpuThrottled`
  - `Test_ResourceLimit_MemoryKilled`
  - `Test_ExecutionTimeout_ForcedTermination`
  - `Test_ReadOnlyFilesystem_CannotWriteToEtc`
  - `Test_ReadOnlyFilesystem_CanWriteToTmp`
  - `Test_NoExec_CannotExecuteBinary`
  - 測試覆蓋率: 100%
  - 參考: [PoC3 Security](../../docs/technical-implementation/10-POC3-SECURITY-HARDENING.md) Lines 757-823

- [ ] **Penetration Tests (Week 2) (P0, SECURITY)** 🔒:
  - 測試容器逃逸攻擊 (訪問 Docker Socket)
  - 測試特權提升攻擊
  - 測試網路攻擊 (DDoS, 下載惡意代碼)
  - 測試系統篡改攻擊 (修改 /etc/passwd)
  - 測試 /proc/1/cgroup 讀取 (信息洩露)
  - 測試 /sys 讀取 (信息洩露)
  - 參考: [PoC3 Security](../../docs/technical-implementation/10-POC3-SECURITY-HARDENING.md) Lines 1-73

**驗收標準 (Phase 3)** 🔒:
- ✅ 4-Layer Security 全部配置完成
- ✅ 無法從容器內存取宿主機檔案
- ✅ 無法從容器內存取外部網路
- ✅ Resource 限制有效
- ✅ 安全測試通過 (100% 通過率)
- ✅ 滲透測試通過 (無容器逃逸漏洞) 🔒
- ✅ 安全評分: ≥90/100 (目標 98/100 if Seccomp+AppArmor)
- ✅ Code Review 通過 (安全專家審核) 🔒

---

### Phase 4: 執行結果處理與可視化 (0.5 SP) ⏳ 待開始

#### Execution Result Processing (P0)
- [ ] **創建 IExecutionResultProcessor Interface**: `src/AIAgentPlatform.Application/Interfaces/IExecutionResultProcessor.cs`
  - Methods: `ParseOutput`, `ExtractFiles`, `FormatError`

- [ ] **實作 ExecutionResultProcessor**: `src/AIAgentPlatform.Infrastructure/Services/ExecutionResultProcessor.cs`
  - 解析 stdout / stderr
  - 格式化錯誤訊息
  - 處理 Exit code
  - 提取執行時間

#### Chart Output Support (P0)
- [ ] **實作 Matplotlib 圖表支援**:
  - 圖表儲存為 PNG (`plt.savefig('/tmp/chart.png')`)
  - 圖表檔案擷取 (從容器複製到宿主機)
  - Base64 編碼傳輸
  - 上傳至 Azure Blob Storage

- [ ] **創建 IBlobStorageService Interface**: `IBlobStorageService.cs`
  - Methods: `UploadFileAsync`, `GetFileUrlAsync`, `DeleteFileAsync`

- [ ] **實作 AzureBlobStorageService**: `src/AIAgentPlatform.Infrastructure/Services/AzureBlobStorageService.cs`
  - 整合 Azure.Storage.Blobs SDK
  - 上傳圖片到 Blob Storage
  - 生成 SAS URL (有效期 1 小時)
  - 刪除過期圖片

#### Table Output Support (P0)
- [ ] **實作 Pandas DataFrame 支援**:
  - DataFrame 轉 JSON (`df.to_json()`)
  - DataFrame 轉 CSV (`df.to_csv()`)
  - 自動偵測輸出格式

#### File Management (P0)
- [ ] **實作檔案管理**:
  - 從容器複製檔案 (`docker cp`)
  - 識別檔案類型 (PNG, SVG, CSV, TXT)
  - 計算檔案大小
  - 生成檔案列表

#### Unit Tests (P0)
- [ ] **ExecutionResultProcessor 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/ExecutionResultProcessorTests.cs`
  - `ParseOutput_SimplePrint_ReturnsText`
  - `ParseOutput_DataFrameSummary_ReturnsFormattedTable`
  - `ExtractFiles_MatplotlibPng_ReturnsFileInfo`
  - `FormatError_SyntaxError_ReturnsFormattedMessage`
  - 測試覆蓋率: ≥85%

- [ ] **AzureBlobStorageService 單元測試**: `AzureBlobStorageServiceTests.cs`
  - `UploadFileAsync_ValidFile_ReturnsUrl`
  - `GetFileUrlAsync_ExistingFile_ReturnsSasUrl`
  - `DeleteFileAsync_ExistingFile_Succeeds`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Chart Output 整合測試**: `tests/AIAgentPlatform.IntegrationTests/CodeExecution/ChartOutputTests.cs`
  - `ExecuteCode_MatplotlibPlot_ReturnsImage`
  - `ExecuteCode_SeabornPlot_ReturnsImage`
  - `ExecuteCode_MultiplePlots_ReturnsMultipleImages`
  - 使用真實 Azure Blob Storage
  - 測試覆蓋率: 100%

**驗收標準 (Phase 4)**:
- ✅ 執行結果正確解析
- ✅ Matplotlib 圖表正確輸出
- ✅ Pandas DataFrame 轉 JSON 成功
- ✅ 圖片上傳至 Azure Blob Storage
- ✅ SAS URL 生成成功
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 5: Code Interpreter API 與 UI (0.5 SP) ⏳ 待開始

#### API Layer - Controllers (P0)
- [ ] **創建 CodeController**: `src/AIAgentPlatform.API/Controllers/CodeController.cs`
  - `POST /api/v1/code/execute` - 執行 Python 代碼
  - `GET /api/v1/code/executions/{id}` - 獲取執行結果
  - `DELETE /api/v1/code/executions/{id}` - 刪除執行記錄
  - `GET /api/v1/code/status` - 容器池狀態
  - 參考: [API Design Guidelines](../../docs/api/API-Design-Guidelines.md)

- [ ] **實作 Execute Endpoint**:
  - Request: `ExecuteCodeRequest` (code, language, timeout)
  - Validation: 代碼長度限制 (≤10KB), language 驗證
  - Response: `ExecuteCodeResponse` (executionId, status, output, files)
  - Status Code: 202 Accepted (異步執行)

- [ ] **實作 Get Execution Result Endpoint**:
  - Request: `executionId`
  - Response: `ExecutionResultDto` (status, output, error, files)
  - Status Code: 200 OK

- [ ] **實作 Container Pool Status Endpoint**:
  - Response: `ContainerPoolStatusDto` (availableCount, totalCount, usageRate)
  - Status Code: 200 OK

#### Application Layer - CQRS Commands (P0)
- [ ] **創建 ExecuteCodeCommand**: `src/AIAgentPlatform.Application/CodeExecution/Commands/ExecuteCode/ExecuteCodeCommand.cs`
  - Properties: Code, Language, Timeout, UserId
  - Validation: Code 非空, Language 支援

- [ ] **創建 ExecuteCodeCommandHandler**: `ExecuteCodeCommandHandler.cs`
  - 步驟 1: 驗證代碼
  - 步驟 2: 從容器池獲取容器
  - 步驟 3: 執行代碼
  - 步驟 4: 處理結果 (上傳圖片等)
  - 步驟 5: 釋放容器
  - 步驟 6: 返回執行結果

#### Application Layer - CQRS Queries (P0)
- [ ] **創建 GetExecutionResultQuery**: `src/AIAgentPlatform.Application/CodeExecution/Queries/GetExecutionResult/GetExecutionResultQuery.cs`
  - Properties: ExecutionId
  - Handler: 從 Redis 或 Database 獲取結果

- [ ] **創建 GetContainerPoolStatusQuery**: `GetContainerPoolStatus/GetContainerPoolStatusQuery.cs`
  - Handler: 調用 ContainerPoolManager.GetAvailableCountAsync()

#### Frontend - Code Interpreter UI (P0)
- [ ] **創建 CodeInterpreter Component**: `apps/web-app/src/features/code-interpreter/CodeInterpreter.tsx`
  - 代碼編輯器 (Monaco Editor 或 CodeMirror)
  - 執行按鈕
  - 執行狀態顯示 (執行中、成功、失敗)
  - 執行結果展示 (Text, Image, Table)

- [ ] **創建 CodeEditor Component**: `components/CodeEditor.tsx`
  - Syntax Highlighting (Python)
  - Line Numbers
  - Copy Code Button
  - Code Folding (超過 20 行)

- [ ] **創建 ExecutionResult Component**: `components/ExecutionResult.tsx`
  - Text Output (格式化)
  - Error Output (紅色標註)
  - Image Display (內嵌 + Lightbox)
  - Execution Time Display

- [ ] **創建 codeExecutionService**: `apps/web-app/src/features/code-interpreter/services/codeExecutionService.ts`
  - `executeCode(code, language)`
  - `getExecutionResult(executionId)`
  - `getContainerPoolStatus()`
  - 使用 Axios HTTP client

#### Unit Tests (P0)
- [ ] **CodeController 單元測試**: `tests/AIAgentPlatform.UnitTests/API/Controllers/CodeControllerTests.cs`
  - `ExecuteCode_ValidRequest_ReturnsAccepted`
  - `ExecuteCode_InvalidLanguage_ReturnsBadRequest`
  - `GetExecutionResult_ExistingId_ReturnsResult`
  - `GetContainerPoolStatus_ReturnsStatus`
  - 測試覆蓋率: ≥85%

- [ ] **ExecuteCodeCommandHandler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/CodeExecution/Commands/ExecuteCodeCommandHandlerTests.cs`
  - `Handle_ValidCommand_ExecutesSuccessfully`
  - `Handle_CodeExecutionFails_ReturnsError`
  - `Handle_ContainerPoolEmpty_WaitsForContainer`
  - 測試覆蓋率: ≥85%

#### E2E Tests (P0)
- [ ] **Code Interpreter E2E Test**: `tests/AIAgentPlatform.E2ETests/CodeInterpreter/CodeInterpreterTests.cs`
  - `ExecuteCode_SimplePython_ReturnsOutput`
  - `ExecuteCode_Matplotlib_ReturnsImage`
  - `ExecuteCode_Pandas_ReturnsFormattedTable`
  - `ExecuteCode_SyntaxError_ReturnsFormattedError`
  - 使用真實 API 與資料庫
  - 測試覆蓋率: 100%

**驗收標準 (Phase 5)**:
- ✅ Code Execution API 完成
- ✅ Code Interpreter UI 完成
- ✅ 代碼編輯器功能完整
- ✅ 執行結果展示正確
- ✅ 單元測試覆蓋率 ≥85%
- ✅ E2E 測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

## 測試

### Unit Tests (P0)
- [ ] **Domain Layer 單元測試**:
  - CodeExecution Entity Tests (4 tests)
  - ExecutionResult VO Tests (3 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Application Layer 單元測試**:
  - ExecuteCodeCommandHandler Tests (4 tests)
  - GetExecutionResultQueryHandler Tests (3 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Infrastructure Layer 單元測試**:
  - ContainerPoolManager Tests (5 tests)
  - DockerCodeExecutionService Tests (5 tests)
  - ExecutionResultProcessor Tests (4 tests)
  - AzureBlobStorageService Tests (3 tests)
  - 測試覆蓋率: ≥85%

### Integration Tests (P0)
- [ ] **Code Execution 整合測試**:
  - ExecuteCode_SimplePython_Success
  - ExecuteCode_DataAnalysis_WithPandas_Success
  - ExecuteCode_Visualization_WithMatplotlib_Success
  - ExecuteCode_Timeout_ReturnsError
  - 測試覆蓋率: 100%

- [ ] **Container Pool 整合測試**:
  - ContainerPool_Initialize_CreatesThreeContainers
  - ContainerPool_Acquire_FromPool_UnderTwoSeconds
  - ContainerPool_Release_DestroysAndReplenishes
  - 測試覆蓋率: 100%

### E2E Tests (P0)
- [ ] **Code Interpreter E2E Test**:
  - ExecuteCode_SimplePython_ReturnsOutput
  - ExecuteCode_Matplotlib_ReturnsImage
  - ExecuteCode_Pandas_ReturnsFormattedTable
  - ExecuteCode_SyntaxError_ReturnsFormattedError
  - 測試覆蓋率: 100%

### Performance Tests (P1)
- [ ] **Execution Performance**:
  - Simple Code Execution → <3 秒 (P95)
  - Data Analysis with Pandas → <10 秒 (P95)
  - Visualization with Matplotlib → <8 秒 (P95)
  - Container Acquisition → <2 秒 (P95)

- [ ] **Concurrent Execution Performance**:
  - 3 並發執行 → 所有 <10 秒 (P95)
  - 5 並發執行 → 有 2 個需等待容器 (可接受)

**測試驗收標準**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋率 100%
- ✅ E2E 測試覆蓋率 100%
- ✅ 所有測試通過率 100%
- ✅ 性能測試達標

---

## 安全驗證

### Security Test Suite (P0, SECURITY) 🔒
- [ ] **Network Isolation Tests**:
  - 測試無法訪問外網 (google.com)
  - 測試無法訪問內網資源
  - 測試無法創建 Socket
  - 通過率: 100%

- [ ] **Resource Limit Tests**:
  - 測試 CPU 限制 (死循環)
  - 測試 Memory 限制 (Memory 炸彈)
  - 測試 Disk 限制 (大量寫入)
  - 測試 Process 限制 (Fork Bomb)
  - 通過率: 100%

- [ ] **Execution Timeout Tests**:
  - 測試無限循環
  - 測試長時間運算
  - 測試超時後容器已終止
  - 通過率: 100%

- [ ] **Read-Only Filesystem Tests**:
  - 測試無法寫入 /etc
  - 測試無法寫入 /usr
  - 測試 /tmp 可寫
  - 測試無法執行二進制文件 (noexec)
  - 通過率: 100%

### Penetration Tests (P0, SECURITY) 🔒
- [ ] **Container Escape Tests (Week 2)**:
  - 測試訪問 Docker Socket (`/var/run/docker.sock`)
  - 測試訪問宿主機檔案 (`/etc/passwd`)
  - 測試特權提升攻擊
  - 測試 /proc/1/cgroup 讀取 (信息洩露)
  - 測試 /sys 讀取 (信息洩露)
  - 通過率: 100% (無任何成功攻擊)

- [ ] **Network Attack Tests**:
  - 測試 DDoS 攻擊
  - 測試下載惡意代碼
  - 測試掃描內網
  - 通過率: 100% (所有攻擊被阻擋)

- [ ] **System Tampering Tests**:
  - 測試修改系統檔案
  - 測試植入後門
  - 測試篡改 Python 解釋器
  - 通過率: 100% (所有攻擊被阻擋)

### Security Score (P0, SECURITY) 🔒
- [ ] **Security Score Calculation**:
  - Layer 1 (Network Isolation): 25 points
  - Layer 2 (Resource Limits): 25 points
  - Layer 3 (Execution Timeout): 20 points
  - Layer 4 (Read-Only Filesystem): 20 points
  - Penetration Test Pass: 10 points
  - **目標總分**: ≥90/100 (簡化版)
  - **進階總分**: ≥98/100 (Seccomp + AppArmor)

**安全驗收標準** 🔒:
- ✅ 安全測試通過率 100%
- ✅ 滲透測試通過率 100%
- ✅ 安全評分 ≥90/100
- ✅ 無容器逃逸漏洞
- ✅ 安全專家審核通過

---

## 文檔

### API Documentation (P0)
- [ ] **Swagger Documentation**:
  - 所有 API 端點有完整 XML 註解
  - Request/Response Schema 定義完整
  - Example Requests/Responses
  - Error Codes 說明

- [ ] **API Design Document**:
  - `docs/api/code-interpreter-api-design.md`
  - 包含所有端點設計
  - Request/Response 範例
  - 錯誤處理策略

### Architecture Documentation (P0)
- [ ] **ADR-008 更新**:
  - 更新 Container Pool 實作細節
  - 記錄簡化版本 (3 containers vs 5)
  - 記錄 MVP 範圍調整

### Technical Implementation Guides (P1)
- [ ] **Code Interpreter Implementation Guide**:
  - `docs/technical-implementation/code-interpreter-implementation.md`
  - 如何新增支援的語言 (Python → R, Julia)
  - 如何調整資源限制
  - 如何優化容器池大小

### Security Documentation (P0, SECURITY) 🔒
- [ ] **Security Configuration Guide**:
  - `docs/security/code-interpreter-security-config.md`
  - 4-Layer Security 配置詳解
  - Seccomp Profile 使用指南 (Optional)
  - AppArmor Profile 使用指南 (Optional)
  - 滲透測試報告模板

### User Documentation (P1)
- [ ] **Code Interpreter User Guide**:
  - `docs/user-guides/code-interpreter.md`
  - 如何執行 Python 代碼
  - 如何生成可視化圖表
  - 常見錯誤與解決方法

**文檔驗收標準**:
- ✅ API Documentation 完整
- ✅ ADR-008 更新完成
- ✅ Security Configuration Guide 完成 🔒
- ✅ User Guide 完成
- ✅ 所有文檔 Review 通過

---

## 部署與DevOps

### Infrastructure Setup (P0)
- [ ] **Docker Engine**:
  - 驗證 Docker Engine 版本 ≥20.10
  - 配置 Docker Daemon (資源限制)
  - 配置 Docker Network (none 模式)
  - 配置 Docker Storage (tmpfs)

- [ ] **Azure Blob Storage**:
  - 創建 Storage Account
  - 創建 Container: `code-execution-outputs`
  - 配置 CORS 規則
  - 配置生命週期管理 (7 天後刪除)

- [ ] **Redis**:
  - 配置執行結果緩存 (TTL: 1 小時)
  - 配置容器池狀態緩存

### Environment Variables (P0)
- [ ] **Docker Configuration**:
  - `DOCKER_HOST` (Docker Daemon URL)
  - `CODE_SANDBOX_IMAGE` (code-sandbox-python:latest)
  - `CONTAINER_POOL_SIZE` (3)

- [ ] **Azure Blob Storage Configuration**:
  - `AZURE_STORAGE_CONNECTION_STRING`
  - `AZURE_STORAGE_CONTAINER_NAME` (code-execution-outputs)

- [ ] **Execution Configuration**:
  - `CODE_EXECUTION_TIMEOUT` (30)
  - `CODE_EXECUTION_MAX_CPU` (1)
  - `CODE_EXECUTION_MAX_MEMORY` (512)

### CI/CD Pipeline (P0)
- [ ] **Docker Image Build**:
  - 自動建置 Docker Image
  - 掃描漏洞 (Trivy / Snyk)
  - 推送至 Azure Container Registry

- [ ] **Backend CI Pipeline**:
  - Build .NET Solution
  - Run Unit Tests (≥85% coverage)
  - Run Integration Tests
  - Run Security Tests 🔒
  - Generate Code Coverage Report
  - Run Static Code Analysis (SonarQube)

- [ ] **CD Pipeline**:
  - Deploy to Staging Environment
  - Run E2E Tests on Staging
  - Run Penetration Tests on Staging 🔒
  - Deploy to Production (manual approval)
  - Run Smoke Tests on Production

### Monitoring & Alerts (P1)
- [ ] **Application Insights**:
  - Log Code Execution Metrics (execution time, success rate)
  - Log Container Pool Metrics (usage rate, available count)
  - Log Security Violations 🔒

- [ ] **Health Checks**:
  - Docker Engine Health Check
  - Container Pool Health Check
  - Azure Blob Storage Health Check

**部署驗收標準**:
- ✅ Infrastructure 設置完成
- ✅ Environment Variables 配置完成
- ✅ CI/CD Pipeline 運作正常
- ✅ Monitoring & Alerts 配置完成
- ✅ Health Checks 全部通過

---

## Definition of Done 驗證

### Sprint 6 Definition of Done

#### Functionality (P0)
- [ ] ✅ **US 5.1 完成**:
  - 所有 5 個 Phases 完成
  - Code Interpreter API 正常運作
  - Code Interpreter UI 完成
  - 執行成功率 ≥90%

#### Code Quality (P0)
- [ ] ✅ **測試覆蓋率**:
  - Unit Tests ≥85%
  - Integration Tests 100%
  - E2E Tests 100%
  - Security Tests 100% 🔒

- [ ] ✅ **Code Review**:
  - 所有 PR Review 通過
  - 0 Critical Issues
  - 0 High Issues

- [ ] ✅ **Static Analysis**:
  - SonarQube Quality Gate 通過
  - 0 Code Smells (Major)
  - 0 Security Vulnerabilities

#### Security (P0, SECURITY) 🔒
- [ ] ✅ **4-Layer Security Architecture**:
  - Layer 1 (Network Isolation): ✅ 配置完成
  - Layer 2 (Resource Limits): ✅ 配置完成
  - Layer 3 (Execution Timeout): ✅ 配置完成
  - Layer 4 (Read-Only Filesystem): ✅ 配置完成

- [ ] ✅ **Security Tests**:
  - 安全測試通過率 100%
  - 滲透測試通過率 100% 🔒
  - 安全評分 ≥90/100
  - 無容器逃逸漏洞

- [ ] ✅ **Security Review**:
  - 安全專家審核通過
  - 安全配置文檔完整

#### Performance (P0)
- [ ] ✅ **Execution Performance**:
  - Simple Code Execution <3 秒 (P95)
  - Data Analysis <10 秒 (P95)
  - Visualization <8 秒 (P95)
  - Container Acquisition <2 秒 (P95)

- [ ] ✅ **Container Pool Performance**:
  - 容器池啟動時間 <10 秒
  - 容器池健康度 ≥95%
  - 容器獲取成功率 100%

#### Documentation (P0)
- [ ] ✅ **API Documentation**:
  - Swagger 完整
  - API Design Document 完成

- [ ] ✅ **Architecture Documentation**:
  - ADR-008 更新完成

- [ ] ✅ **Security Documentation**:
  - Security Configuration Guide 完成 🔒
  - Penetration Test Report 完成 🔒

#### Deployment (P0)
- [ ] ✅ **Infrastructure Ready**:
  - Docker Engine 配置完成
  - Azure Blob Storage 配置完成
  - Redis 配置完成

- [ ] ✅ **CI/CD Pipeline**:
  - Backend CI Pipeline 通過
  - Docker Image Build 成功
  - CD Pipeline 部署成功

- [ ] ✅ **Monitoring**:
  - Application Insights 配置完成
  - Health Checks 全部通過

**最終驗收**:
- ✅ 所有功能開發完成
- ✅ 所有測試通過
- ✅ 所有安全測試通過 🔒
- ✅ 所有文檔完成
- ✅ 部署成功
- ✅ Sprint Review 通過
- ✅ Stakeholder Sign-off

---

## 參考文獻索引

### Sprint 6 核心文檔 (7 refs)
1. **SPRINT-6-1-OVERVIEW.md** - Sprint 6 概覽 (963 行)
   - Lines 1-100: Sprint 目標與範圍
   - Lines 100-300: US 5.1 Phase Breakdown
   - Lines 300-600: 技術範圍與預期交付
   - Lines 600-963: 完整參考文獻索引 (70+ 文檔)

2. **SPRINT-6-2-PLAN.md** - Sprint 6 實施計劃 (預計 2,000+ 行)
   - Lines 1-500: US 5.1 Docker 環境搭建實施計劃
   - Lines 500-1000: US 5.1 Python 執行引擎實施計劃
   - Lines 1000-1500: US 5.1 Security 實施計劃
   - Lines 1500-2000: US 5.1 API/UI 實施計劃

3. **SPRINT-6-3-CONTEXT.md** - Sprint 6 技術上下文 (預計 1,000+ 行)
   - Core Technology Reference
   - US 5.1 Technical Context
   - Reference Index

4. **SPRINT-6-4-CHECKLIST.md** - Sprint 6 檢查清單 (本文件, 1,400+ 行)

5. **SPRINT-6-5-DEV-LOG.md** - Sprint 6 開發日誌
6. **SPRINT-6-6-ISSUES.md** - Sprint 6 問題追蹤
7. **SPRINT-6-7-RETROSPECTIVE.md** - Sprint 6 回顧

### User Story 規格文檔 (2 refs)
8. **module-03-code-interpreter.md** - US 3.1-3.3 詳細規格
   - US 3.1 (對應 US 5.1): Python 代碼執行（4 層安全沙箱）
   - US 3.2: Agent 自動生成可視化代碼
   - US 3.3: Code Interpreter 結果展示（UI 增強）

9. **epic-06-code-interpreter.md** - Code Interpreter Epic

### Architecture Design Documents - ADRs (3 refs)
10. **Architecture-Design-Document.md** - 完整架構設計
11. **ADR-008: Code Interpreter Execution Model** - Docker 容器池設計與動態生命週期管理 (725 行)
    - Lines 12-36: Hybrid Container Pool 決策
    - Lines 39-229: Container Pool 管理實現
    - Lines 232-412: Code Interpreter 實現
    - Lines 465-561: 4-Layer Security Architecture
    - Lines 564-625: 理由與影響分析
12. **ADR-006: Hybrid State Management** - Redis + PostgreSQL 混合狀態管理

### Technical Implementation Documents (3 refs)
13. **08-SECURITY-ARCHITECTURE.md** - 安全架構總覽 (1,103 行)
    - Lines 1-105: 安全架構概覽
    - Lines 107-294: 多層防禦策略 (7 層)
    - Lines 296-433: 威脅模型分析 (STRIDE)
    - Lines 477-507: 數據安全
    - Lines 619-684: 網絡安全
    - Lines 741-937: 應用安全 (OWASP Top 10)

14. **10-POC3-SECURITY-HARDENING.md** - Code Interpreter 安全強化 (1,005 行)
    - Lines 1-73: 當前安全狀態 (90/100)
    - Lines 75-365: Seccomp Profile 設計
    - Lines 367-535: AppArmor Profile 設計
    - Lines 537-592: SELinux Policy 設計
    - Lines 594-749: 完整安全配置 (6-Layer)
    - Lines 751-839: 安全驗證測試套件
    - Lines 841-998: 生產部署建議

15. **06-code-interpreter-implementation.md** - Code Interpreter 完整實作指南 (預計創建)

### API Design Documents (2 refs)
16. **API-Design-Guidelines.md** - API 設計指南
17. **code-interpreter-api-design.md** - Code Interpreter API 設計 (New)

### Database Design (1 ref)
18. **code-execution-entity-design.md** - CodeExecution Entity 設計 (New)

### Testing Documentation (3 refs)
19. **testing-strategy-overview.md** - 測試策略概覽
20. **unit-testing-guidelines.md** - 單元測試指南
21. **security-testing.md** - 安全測試指南 (包含滲透測試) 🔒

### UX Design Documents (2 refs)
22. **user-research/personas.md** - 使用者研究 Personas
23. **wireframes/low-fidelity/07-code-interpreter.md** - Code Interpreter UI 設計

### Development Standards (3 refs)
24. **coding-standards-csharp.md** - C# 編碼標準
25. **coding-standards-typescript.md** - TypeScript 編碼標準
26. **git-workflow.md** - Git 工作流程

### Docker & Deployment Documents (3 refs)
27. **docker-compose-setup.md** - Docker Compose 設置
28. **code-interpreter-dockerfile.md** - Code Interpreter Dockerfile 設計 (New)
29. **deployment-guide.md** - 部署指南

### Code References - Application Layer (4 refs)
30. **ICodeExecutionService.cs** - 代碼執行服務介面 (New)
31. **IContainerPoolManager.cs** - 容器池管理介面 (New)
32. **IDockerService.cs** - Docker 服務介面 (New)
33. **IExecutionResultProcessor.cs** - 執行結果處理介面 (New)

### Code References - Domain Layer (2 refs)
34. **CodeExecution.cs** - CodeExecution Entity (New)
35. **ExecutionResult.cs** - ExecutionResult Value Object (New)

### Code References - Infrastructure Layer (6 refs)
36. **DockerCodeExecutionService.cs** - Docker 代碼執行服務 (New)
37. **ContainerPoolManager.cs** - 容器池管理器 (New)
38. **DockerService.cs** - Docker 服務實作 (New)
39. **ExecutionResultProcessor.cs** - 執行結果處理器 (New)
40. **ContainerHealthChecker.cs** - 容器健康檢查 (New)
41. **SecurityManager.cs** - 安全管理器 (New)

### Code References - API Layer (1 ref)
42. **CodeController.cs** - Code API Controller (New)

### Code References - Frontend (3 refs)
43. **CodeInterpreter.tsx** - Code Interpreter 主元件 (New)
44. **CodeEditor.tsx** - 代碼編輯器元件 (New)
45. **ExecutionResult.tsx** - 執行結果元件 (New)

### Code References - Docker (1 ref)
46. **Dockerfile** - Code Interpreter Docker Image 定義 (New)

### Planning Documents (8 refs)
47. **MVP-SCOPE-DEFINITION.md** - MVP 範圍定義與 Phase 1A 詳細規劃
48. **SPRINT-ALLOCATION-ANALYSIS.md** - Sprint 6 詳細分析 (5 SP 調整後, 3 週)
49. **DEVELOPMENT-STRATEGY.md** - Code Interpreter 開發策略
50. **DEPENDENCY-MATRIX.md** - Sprint 依賴關係矩陣
51. **RISK-REGISTER.md** - 風險登記簿 (RISK-016: Container 逃逸, RISK-017: 效能不穩定, RISK-018: 容器池管理複雜度, RISK-019: Docker 依賴風險)
52. **TECHNICAL-DECISIONS-LOG.md** - 技術決策日誌
53. **ARCHITECTURE-EVOLUTION-ROADMAP.md** - 架構演進路線圖
54. **DOCS-REBUILD-EXECUTION-PLAN.md** - 文檔重建執行計劃

### Previous Sprint References (35 refs - Sprint 1-5 各 7 files)
**Sprint 1**: OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE
**Sprint 2**: OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE
**Sprint 3**: OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE
**Sprint 4**: OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE
**Sprint 5**: OVERVIEW, PLAN, CONTEXT, CHECKLIST, DEV-LOG, ISSUES, RETROSPECTIVE (格式參考, 1,282 行)

---

## 版本歷史

| 版本 | 日期 | 作者 | 變更說明 |
|------|------|------|----------|
| v2.1 | 2025-11-14 | AI Assistant | 初始版本建立，完整 Sprint 6 任務清單 (143 項任務) |
|      |            |              | - US 5.1: 78 項任務 (5 Phases) |
|      |            |              | - 測試: 25 項任務 |
|      |            |              | - 安全驗證: 12 項任務 🔒 |
|      |            |              | - 文檔: 8 項任務 |
|      |            |              | - 部署: 10 項任務 |
|      |            |              | - 項目準備: 10 項任務 |
|      |            |              | 遵循 v2.1 標準 (8 欄位 header, 完整 ToC, 參考索引) |
|      |            |              | 特別強調安全測試 (Week 2 滲透測試) 🔒 |
|      |            |              | MVP 簡化版本 (5 SP, 3 containers, 4-Layer Security) |

---

**文件結束** - Sprint 6 任務清單已完整建立 ✅

**重要提醒** 🔒:
- Week 2 必須完成滲透測試
- 安全評分目標: ≥90/100 (簡化版)
- 無容器逃逸漏洞為必須通過條件
- 所有安全相關任務標記 🔒 需特別關注
