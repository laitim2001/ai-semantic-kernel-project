# SPRINT-5-4-CHECKLIST.md - Sprint 5 任務清單：Knowledge 管理與 RAG 檢索執行追蹤

**版本**: v2.1
**Sprint 編號**: Sprint 5
**Sprint 週期**: Week 13-15 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2026-01-06 ~ 2026-01-26
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📑 目錄 (Table of Contents)

### 核心章節
1. [總體進度統計](#總體進度統計)
2. [項目準備](#項目準備)
3. [US 4.1 - Document Upload & Processing (8 SP, 5 Phases)](#us-41---document-upload--processing-8-sp-5-phases-)
   - 3.1 [Phase 1: 文檔上傳基礎架構](#phase-1-文檔上傳基礎架構-2-sp-)
   - 3.2 [Phase 2: 文檔解析與文字提取](#phase-2-文檔解析與文字提取-2-sp-)
   - 3.3 [Phase 3: 文檔分塊](#phase-3-文檔分塊-15-sp-)
   - 3.4 [Phase 4: 向量化與索引](#phase-4-向量化與索引-2-sp-)
   - 3.5 [Phase 5: 文檔管理 UI 與測試](#phase-5-文檔管理-ui-與測試-05-sp-)
4. [US 4.2 - RAG Retrieval Strategies (5 SP, 4 Phases)](#us-42---rag-retrieval-strategies-5-sp-4-phases-)
   - 4.1 [Phase 1: 語義搜尋實作](#phase-1-語義搜尋實作-15-sp-)
   - 4.2 [Phase 2: 關鍵字搜尋實作](#phase-2-關鍵字搜尋實作-1-sp-)
   - 4.3 [Phase 3: Hybrid Search 與 Re-ranking](#phase-3-hybrid-search-與-re-ranking-15-sp-)
   - 4.4 [Phase 4: 檢索準確率測試框架](#phase-4-檢索準確率測試框架-1-sp-)
5. [測試](#測試)
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
本文件為 Sprint 5 的詳細任務檢查清單，按 **User Story (US)** 組織，追蹤所有待辦事項的執行狀態和完成進度。

### 目標讀者
- **開發團隊**：日常開發任務的核心參考文件，追蹤工作進度
- **Scrum Master / PM**：Sprint 進度追蹤與風險識別
- **QA 團隊**：測試範圍與驗收標準的參考
- **AI Assistant**：任務狀態查詢與進度更新

### 使用方式
1. **每日開發**：查看對應 User Story 的 Phase 任務清單，更新完成狀態
2. **進度追蹤**：查看總體進度統計表，了解 Sprint 整體進度
3. **任務分配**：依據 Phase 劃分合理分配開發任務
4. **完成驗證**：使用 Definition of Done 驗證章節確保質量標準
5. **更新文件**：完成任務後立即更新檢查框狀態 [x]

### 快速導航
- **查看 Sprint 5 整體概覽** → [SPRINT-5-1-OVERVIEW.md](./SPRINT-5-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md)
- **查看技術上下文** → [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md)
- **查看開發日誌** → [SPRINT-5-5-DEV-LOG.md](./SPRINT-5-5-DEV-LOG.md)
- **查看問題追蹤** → [SPRINT-5-6-ISSUES.md](./SPRINT-5-6-ISSUES.md)

### 狀態標記說明
- ✅ **已完成** - 項目已完成並通過驗證
- 🔄 **進行中** - 項目正在執行中
- ⏳ **待開始** - 項目已計劃但尚未開始
- ⚠️ **有問題** - 項目遇到阻礙需要關注
- ❌ **失敗** - 項目未通過驗證需要重做

### 優先級標記
- **P0** - 必須完成的項目，影響 Sprint 交付
- **P1** - 重要但非阻塞的項目，建議完成

---

## 📊 總體進度統計

| 類別 | 已完成 / 總數 | 進度 | 狀態 |
|------|------------|------|------|
| 項目準備 | 0/8 | 0% | ⏳ |
| US 4.1 - Document Upload & Processing | 0/82 | 0% | ⏳ |
| US 4.2 - RAG Retrieval Strategies | 0/65 | 0% | ⏳ |
| 測試 | 0/28 | 0% | ⏳ |
| 文檔 | 0/15 | 0% | ⏳ |
| 部署 | 0/12 | 0% | ⏳ |
| **總計** | **0/210** | **0%** | ⏳ |

**圖例**: ✅ 已完成 | 🔄 進行中 | ⏳ 待開始 | ❌ 阻塞

**Story Points 分配**:
- US 4.1: Document Upload & Processing (8 SP)
- US 4.2: RAG Retrieval Strategies (5 SP)
- **總計**: 13 SP

---

## 項目準備

### 環境設置 (P0)
- [ ] 更新開發環境 (`git pull origin main`)
- [ ] 創建 Feature Branch (`git checkout -b feature/us-4.1-document-upload`)
- [ ] 安裝依賴 (`dotnet restore`, `pnpm install`)
- [ ] 驗證資料庫連接正常 (PostgreSQL, Qdrant)
- [ ] 檢查 Sprint 5 所有文檔
- [ ] 配置 Azure Blob Storage 連接字串
- [ ] 配置 Azure OpenAI API Key (text-embedding-ada-002)
- [ ] 安裝 Hangfire Dashboard (http://localhost:5095/hangfire)

### Sprint Planning (P0)
- [ ] 閱讀 [US 4.1 規格](../../../docs/user-stories/modules/module-05-knowledge-management.md) 與 MVP 範圍
- [ ] 閱讀 [US 4.2 規格](../../../docs/user-stories/modules/module-05-knowledge-management.md) 與 MVP 範圍
- [ ] 確認 Phase 實施順序
- [ ] 規劃 Database Schema (documents, document_chunks tables)
- [ ] 規劃 Qdrant Collection Schema (agent_knowledge collection)
- [ ] 規劃 API 端點 (Document Upload, RAG Search)
- [ ] 規劃測試策略 (Unit, Integration, E2E)
- [ ] 閱讀 ADR-015 至 ADR-019 架構決策

**驗收標準**:
- ✅ 開發環境就緒
- ✅ Feature Branch 創建成功
- ✅ 所有 Sprint 5 規格文檔已閱讀
- ✅ Azure Blob Storage 與 Qdrant 連接正常

---

## US 4.1: Document Upload & Processing (8 SP, 5 Phases)

### Phase 1: 文檔上傳基礎架構 (2 SP) ⏳ 待開始

#### Domain Layer - Entities
- [ ] **創建 Document Entity**: `src/AIAgentPlatform.Domain/Entities/Document.cs`
  - Properties: Id, AgentId, FileName, FileSize, FileExtension, BlobUri, Status, UploadedAt, ProcessedAt
  - Business Rules: 檔案大小限制 (≤10MB), 檔案格式驗證 (PDF/DOCX/TXT/MD)
  - Status Enum: Uploaded, Processing, Completed, Failed
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 200-250

- [ ] **創建 DocumentChunk Entity**: `DocumentChunk.cs`
  - Properties: Id, DocumentId, Content, ChunkIndex, TokenCount, EmbeddingVector, CreatedAt
  - Business Rules: TokenCount 範圍 (512-2048), Content 非空
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 250-300

- [ ] **創建 ProcessingStatus Enum**: `ProcessingStatus.cs`
  - Values: Uploaded, Parsing, Chunking, Embedding, Indexing, Completed, Failed
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 150-200

#### Domain Layer - Value Objects
- [ ] **創建 FileMetadata VO**: `src/AIAgentPlatform.Domain/ValueObjects/FileMetadata.cs`
  - Properties: FileName, FileSize, FileExtension, ContentType, Hash
  - Validation: 檔案格式驗證, 大小限制驗證
  - Immutability: VO 不可變
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 300-350

- [ ] **創建 BlobReference VO**: `BlobReference.cs`
  - Properties: ContainerName, BlobName, BlobUri
  - Validation: URI 格式驗證
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 200-250

#### Infrastructure Layer - Blob Storage
- [ ] **創建 IBlobStorageService Interface**: `src/AIAgentPlatform.Application/Interfaces/IBlobStorageService.cs`
  - Methods: `UploadAsync`, `DownloadAsync`, `DeleteAsync`, `GetSasUriAsync`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 350-400

- [ ] **實作 AzureBlobStorageService**: `src/AIAgentPlatform.Infrastructure/Services/AzureBlobStorageService.cs`
  - 實現所有 CRUD 方法
  - 使用 Azure.Storage.Blobs SDK
  - Hot Tier 配置 (檔案上傳後 30 天內)
  - Cool Tier 配置 (30 天後自動轉換)
  - SAS Token 生成 (讀取權限, 1小時有效期)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 400-500

#### Application Layer - CQRS Commands
- [ ] **創建 UploadDocumentCommand**: `src/AIAgentPlatform.Application/Documents/Commands/UploadDocument/UploadDocumentCommand.cs`
  - Properties: AgentId, Files, Tags, Description
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 500-550

- [ ] **創建 UploadDocumentCommandHandler**: `UploadDocumentCommandHandler.cs`
  - 實現檔案驗證 (格式, 大小)
  - 上傳至 Azure Blob Storage
  - 創建 Document 實體
  - 觸發 Background Job (DocumentProcessingJob)
  - 返回 UploadDocumentResponse (DocumentId, BlobUri, Status)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 550-650

- [ ] **創建 DeleteDocumentCommand**: `DeleteDocument/DeleteDocumentCommand.cs`
  - Properties: DocumentId
  - Handler: 軟刪除 Document, 刪除 Blob, 刪除 Qdrant vectors
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 650-700

#### Application Layer - CQRS Queries
- [ ] **創建 GetDocumentByIdQuery**: `src/AIAgentPlatform.Application/Documents/Queries/GetDocumentById/GetDocumentByIdQuery.cs`
  - Properties: DocumentId
  - Handler: 返回 DocumentDto (包含 SAS URI)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 700-750

- [ ] **創建 GetDocumentsByAgentIdQuery**: `GetDocumentsByAgentId/GetDocumentsByAgentIdQuery.cs`
  - Properties: AgentId, PageNumber, PageSize, SortBy, SortOrder
  - Handler: 分頁查詢, 返回 PaginatedList<DocumentDto>
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 750-800

#### Infrastructure Layer - EF Configuration
- [ ] **創建 DocumentConfiguration**: `src/AIAgentPlatform.Infrastructure/Configurations/DocumentConfiguration.cs`
  - Table: `documents`
  - Indexes: `idx_documents_agent_id`, `idx_documents_status`, `idx_documents_uploaded_at`
  - Unique Constraint: `agent_id + file_name` (when IsDeleted = false)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 800-850

- [ ] **創建 DocumentChunkConfiguration**: `DocumentChunkConfiguration.cs`
  - Table: `document_chunks`
  - Indexes: `idx_document_chunks_document_id`, `idx_document_chunks_chunk_index`
  - Vector Column: `embedding_vector` (PostgreSQL vector extension, dimension 1536)
  - Foreign Key: `document_id` → `documents(id)` (CASCADE DELETE)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 850-900

#### Migration
- [ ] **安裝 pgvector Extension**: PostgreSQL 資料庫
  - SQL: `CREATE EXTENSION IF NOT EXISTS vector;`
  - 驗證: `SELECT * FROM pg_extension WHERE extname = 'vector';`
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 250-300

- [ ] **創建 Migration**: `dotnet ef migrations add AddDocumentManagement`
  - Migration 文件: `20260106_AddDocumentManagement.cs`
  - Up 方法: 創建 2 個 tables + indexes + vector extension
  - Down 方法: 刪除 tables
  - 驗證: Migration 文件正確生成
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 900-950

- [ ] **執行 Migration (Local)**: `dotnet ef database update`
  - 驗證: `documents`, `document_chunks` 表創建成功
  - 驗證: 所有 indexes 創建成功
  - 驗證: pgvector extension 安裝成功

#### Unit Tests (P0)
- [ ] **Document Entity 單元測試**: `tests/AIAgentPlatform.UnitTests/Domain/Entities/DocumentTests.cs`
  - `Create_ValidDocument_ReturnsSuccess`
  - `Create_InvalidFileExtension_ReturnsFailure`
  - `Create_FileSizeTooLarge_ReturnsFailure`
  - `UpdateStatus_ValidTransition_Succeeds`
  - 測試覆蓋率: ≥85%

- [ ] **BlobStorageService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/AzureBlobStorageServiceTests.cs`
  - `UploadAsync_ValidFile_Succeeds`
  - `DownloadAsync_ExistingBlob_ReturnsStream`
  - `DeleteAsync_ExistingBlob_Succeeds`
  - `GetSasUriAsync_ValidBlob_ReturnsUri`
  - 測試覆蓋率: ≥85%

- [ ] **UploadDocumentCommandHandler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/Documents/Commands/UploadDocumentCommandHandlerTests.cs`
  - `Handle_ValidCommand_UploadsAndReturnsDocumentId`
  - `Handle_InvalidFileExtension_ReturnsFailure`
  - `Handle_FileSizeTooLarge_ReturnsFailure`
  - `Handle_BlobUploadFails_ReturnsFailure`
  - 測試覆蓋率: ≥85%

**驗收標準 (Phase 1)**:
- ✅ Domain Entities 定義完整，包含所有必要屬性
- ✅ Blob Storage Service 實作完成
- ✅ Document Upload API 功能正常
- ✅ Database Migration 成功執行
- ✅ 單元測試覆蓋率 ≥85%
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 2: 文檔解析與文字提取 (2 SP) ⏳ 待開始

#### Application Layer - Interfaces
- [ ] **創建 IDocumentParser Interface**: `src/AIAgentPlatform.Application/Interfaces/IDocumentParser.cs`
  - Properties: `SupportedExtensions`
  - Methods: `ParseAsync(Stream fileStream, string fileName, CancellationToken)`
  - Return Type: `DocumentParseResult` (ExtractedText, PageCount, ParsedMetadata)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 950-1000

- [ ] **創建 DocumentParseResult**: `DocumentParseResult.cs`
  - Properties: ExtractedText, PageCount, ParsedMetadata, ParsedAt
  - Success/Failure Factory Methods
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1000-1050

#### Infrastructure Layer - Document Parsers
- [ ] **創建 PdfDocumentParser**: `src/AIAgentPlatform.Infrastructure/Services/DocumentParsers/PdfDocumentParser.cs`
  - 使用 PdfPig NuGet Package
  - 提取所有文字內容
  - 提取頁面數量
  - 提取 Metadata (Title, Author, CreationDate)
  - 處理加密 PDF (返回錯誤)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1050-1150

- [ ] **創建 DocxDocumentParser**: `DocxDocumentParser.cs`
  - 使用 DocumentFormat.OpenXml NuGet Package
  - 提取所有段落文字
  - 提取 Metadata (Title, Author, LastModifiedBy)
  - 處理損壞的 DOCX (返回錯誤)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1150-1250

- [ ] **創建 TextDocumentParser**: `TextDocumentParser.cs`
  - 使用 StreamReader
  - 支援 UTF-8 編碼
  - 支援 TXT 與 MD 檔案
  - 簡單文字讀取
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1250-1300

- [ ] **創建 DocumentParserFactory**: `DocumentParserFactory.cs`
  - Factory Pattern 實現
  - 根據檔案副檔名選擇 Parser
  - 註冊所有 Parser (PDF, DOCX, TXT, MD)
  - 未支援格式拋出異常
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1300-1350

#### Dependency Injection
- [ ] **註冊 Document Parsers**: `src/AIAgentPlatform.Infrastructure/DependencyInjection.cs`
  - `services.AddSingleton<IDocumentParser, PdfDocumentParser>()`
  - `services.AddSingleton<IDocumentParser, DocxDocumentParser>()`
  - `services.AddSingleton<IDocumentParser, TextDocumentParser>()`
  - `services.AddSingleton<DocumentParserFactory>()`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1350-1400

#### Unit Tests (P0)
- [ ] **PdfDocumentParser 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/PdfDocumentParserTests.cs`
  - `ParseAsync_ValidPdf_ExtractsText`
  - `ParseAsync_EncryptedPdf_ReturnsFailure`
  - `ParseAsync_CorruptedPdf_ReturnsFailure`
  - `ParseAsync_ExtractsMetadata_Correctly`
  - 測試覆蓋率: ≥85%

- [ ] **DocxDocumentParser 單元測試**: `DocxDocumentParserTests.cs`
  - `ParseAsync_ValidDocx_ExtractsText`
  - `ParseAsync_CorruptedDocx_ReturnsFailure`
  - `ParseAsync_ExtractsMetadata_Correctly`
  - 測試覆蓋率: ≥85%

- [ ] **TextDocumentParser 單元測試**: `TextDocumentParserTests.cs`
  - `ParseAsync_ValidTxt_ExtractsText`
  - `ParseAsync_ValidMd_ExtractsText`
  - `ParseAsync_Utf8Encoding_HandlesCorrectly`
  - 測試覆蓋率: ≥85%

- [ ] **DocumentParserFactory 單元測試**: `DocumentParserFactoryTests.cs`
  - `GetParser_PdfExtension_ReturnsPdfParser`
  - `GetParser_DocxExtension_ReturnsDocxParser`
  - `GetParser_TxtExtension_ReturnsTextParser`
  - `GetParser_UnsupportedExtension_ThrowsException`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Document Parsing 整合測試**: `tests/AIAgentPlatform.IntegrationTests/DocumentParsing/DocumentParsingTests.cs`
  - `ParsePdfDocument_RealFile_ExtractsText`
  - `ParseDocxDocument_RealFile_ExtractsText`
  - `ParseTextDocument_RealFile_ExtractsText`
  - 使用測試檔案: `test-files/sample.pdf`, `sample.docx`, `sample.txt`
  - 測試覆蓋率: 100% (所有 parsers)

**驗收標準 (Phase 2)**:
- ✅ 3 種 Document Parser 實作完成 (PDF, DOCX, TXT/MD)
- ✅ DocumentParserFactory 正確路由
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過 (使用真實檔案)
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 3: 文檔分塊 (1.5 SP) ⏳ 待開始

#### Application Layer - Interfaces
- [ ] **創建 IDocumentChunker Interface**: `src/AIAgentPlatform.Application/Interfaces/IDocumentChunker.cs`
  - Methods: `ChunkAsync(string content, ChunkingOptions, CancellationToken)`
  - Return Type: `IReadOnlyList<DocumentChunk>`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1400-1450

- [ ] **創建 ChunkingOptions**: `ChunkingOptions.cs`
  - Properties: ChunkSize (512-2048 tokens), OverlapSize (0-200 tokens), MinChunkSize
  - Validation: ChunkSize > OverlapSize, OverlapSize ≥ 0
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1450-1500

#### Infrastructure Layer - Token Counting
- [ ] **安裝 TikToken NuGet Package**: `Microsoft.SemanticKernel.Connectors.OpenAI` (包含 TikToken)
  - Version: ≥1.0.0
  - Encoding: cl100k_base (用於 GPT-4/ada-002)
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 300-350

- [ ] **創建 ITokenCounter Interface**: `src/AIAgentPlatform.Application/Interfaces/ITokenCounter.cs`
  - Methods: `CountTokens(string text)`
  - Return Type: `int`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1500-1550

- [ ] **實作 TikTokenCounter**: `src/AIAgentPlatform.Infrastructure/Services/TikTokenCounter.cs`
  - 使用 cl100k_base encoding
  - 緩存 Tokenizer 實例
  - Thread-safe 實現
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1550-1600

#### Infrastructure Layer - Chunking Strategy
- [ ] **創建 FixedSizeChunker**: `src/AIAgentPlatform.Infrastructure/Services/DocumentChunkers/FixedSizeChunker.cs`
  - 演算法: 固定大小分塊 + 重疊
  - 參數: chunkSize (預設 1024 tokens), overlapSize (預設 100 tokens)
  - 步驟: 計算 token 數量 → 分割 → 添加重疊
  - 邊界處理: 最後一塊若 < minChunkSize 則合併至前一塊
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1600-1750

- [ ] **實作 Chunk 重疊邏輯**:
  - 重疊區域: 前一塊的最後 N tokens
  - 目的: 保持語義連續性
  - 驗證: 重疊不超過 ChunkSize
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 350-400

#### Unit Tests (P0)
- [ ] **TikTokenCounter 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/TikTokenCounterTests.cs`
  - `CountTokens_SimpleText_ReturnsCorrectCount`
  - `CountTokens_EmptyText_ReturnsZero`
  - `CountTokens_ChineseText_ReturnsCorrectCount`
  - 測試覆蓋率: ≥85%

- [ ] **FixedSizeChunker 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/FixedSizeChunkerTests.cs`
  - `ChunkAsync_ShortText_ReturnsSingleChunk`
  - `ChunkAsync_LongText_ReturnsMultipleChunks`
  - `ChunkAsync_WithOverlap_IncludesOverlappingTokens`
  - `ChunkAsync_LastChunkTooSmall_MergesWithPrevious`
  - `ChunkAsync_ChunkCount_IsCorrect`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Chunking 整合測試**: `tests/AIAgentPlatform.IntegrationTests/DocumentChunking/ChunkingTests.cs`
  - `Chunk_RealDocument_ProducesCorrectChunks`
  - `Chunk_OverlapSize_IsAppliedCorrectly`
  - `Chunk_AllChunks_MeetSizeRequirements`
  - 使用真實文檔測試
  - 驗證: 所有 chunk token count 在範圍內

**驗收標準 (Phase 3)**:
- ✅ TikToken 整合成功，token 計數準確
- ✅ FixedSizeChunker 實作完成
- ✅ Chunk 重疊邏輯正確
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 4: 向量化與索引 (2 SP) ⏳ 待開始

#### Application Layer - Interfaces
- [ ] **創建 IEmbeddingService Interface**: `src/AIAgentPlatform.Application/Interfaces/IEmbeddingService.cs`
  - Methods: `GenerateEmbeddingAsync(string text, CancellationToken)`
  - Methods: `GenerateBatchEmbeddingsAsync(IEnumerable<string> texts, CancellationToken)`
  - Return Type: `float[]` (1536 dimensions for ada-002)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1750-1800

- [ ] **創建 IVectorStoreService Interface**: `IVectorStoreService.cs`
  - Methods: `CreateCollectionAsync`, `UpsertVectorsAsync`, `SearchAsync`, `DeleteAsync`
  - Return Type: Collection-specific DTOs
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1800-1850

#### Infrastructure Layer - Embedding Service
- [ ] **實作 OpenAIEmbeddingService**: `src/AIAgentPlatform.Infrastructure/Services/OpenAIEmbeddingService.cs`
  - 使用 Azure OpenAI SDK
  - Model: text-embedding-ada-002
  - Dimension: 1536
  - 實現 Memory Cache (緩存 1 小時)
  - Hash Key: SHA256(text)
  - Batch Processing: 最多 16 個文本/批次
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1850-1950

- [ ] **實作 Embedding Cache**:
  - Cache Key: `embedding:{SHA256(text)}`
  - TTL: 1 小時
  - 使用 MemoryCache
  - Cache Hit Logging
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 400-450

#### Infrastructure Layer - Qdrant Integration
- [ ] **安裝 Qdrant Client NuGet Package**: `Qdrant.Client`
  - Version: ≥1.7.0
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 450-500

- [ ] **實作 QdrantVectorStoreService**: `src/AIAgentPlatform.Infrastructure/Services/QdrantVectorStoreService.cs`
  - Collection Name: `agent_knowledge`
  - Vector Size: 1536
  - Distance Metric: Cosine
  - HNSW Configuration: `m=16, ef_construct=100, ef=128`
  - Full Scan Threshold: 10000
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 1950-2100

- [ ] **實作 CreateCollectionAsync**:
  - 檢查 Collection 是否存在
  - 創建 Collection with HNSW index
  - 設定 Vector 參數 (dimension, distance, hnsw_config)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2000-2050

- [ ] **實作 UpsertVectorsAsync**:
  - Batch Upsert (最多 100 vectors/批次)
  - Payload: `{ documentId, chunkId, content, metadata }`
  - Retry 邏輯 (最多 3 次)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2050-2100

- [ ] **實作 SearchAsync**:
  - Vector Search with Cosine Similarity
  - Top-K 查詢 (預設 10)
  - Score Threshold: 0.7-0.9 (可配置)
  - 返回: `IReadOnlyList<SearchResult>` (ChunkId, Score, Content)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2100-2150

#### Background Jobs - Document Processing
- [ ] **安裝 Hangfire NuGet Packages**:
  - `Hangfire.Core`
  - `Hangfire.AspNetCore`
  - `Hangfire.PostgreSql`
  - Version: ≥1.8.0
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 500-550

- [ ] **配置 Hangfire**: `src/AIAgentPlatform.API/Program.cs`
  - PostgreSQL Storage
  - Dashboard Authorization
  - Job Queue: `knowledge_processing`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2150-2200

- [ ] **創建 DocumentProcessingJob**: `src/AIAgentPlatform.Infrastructure/BackgroundJobs/DocumentProcessingJob.cs`
  - 步驟 1: Download from Blob Storage
  - 步驟 2: Parse document (使用 DocumentParserFactory)
  - 步驟 3: Chunk content (使用 FixedSizeChunker)
  - 步驟 4: Generate embeddings (batch, 使用 OpenAIEmbeddingService)
  - 步驟 5: Store in Qdrant (使用 QdrantVectorStoreService)
  - 步驟 6: Update document status to Completed
  - 錯誤處理: 更新 status to Failed, 記錄錯誤訊息
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2200-2350

- [ ] **註冊 Background Job**: `UploadDocumentCommandHandler.cs`
  - 上傳成功後觸發: `BackgroundJob.Enqueue<DocumentProcessingJob>(x => x.ProcessAsync(documentId))`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2350-2400

#### Unit Tests (P0)
- [ ] **OpenAIEmbeddingService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/OpenAIEmbeddingServiceTests.cs`
  - `GenerateEmbeddingAsync_ValidText_ReturnsEmbedding`
  - `GenerateEmbeddingAsync_CachedText_ReturnsCachedEmbedding`
  - `GenerateBatchEmbeddingsAsync_MultipleTexts_ReturnsEmbeddings`
  - `GenerateEmbeddingAsync_ApiError_ThrowsException`
  - 測試覆蓋率: ≥85%

- [ ] **QdrantVectorStoreService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/QdrantVectorStoreServiceTests.cs`
  - `CreateCollectionAsync_NewCollection_CreatesSuccessfully`
  - `UpsertVectorsAsync_ValidVectors_UpsertsSuccessfully`
  - `SearchAsync_ValidQuery_ReturnsResults`
  - `DeleteAsync_ExistingVector_DeletesSuccessfully`
  - 測試覆蓋率: ≥85%

- [ ] **DocumentProcessingJob 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/BackgroundJobs/DocumentProcessingJobTests.cs`
  - `ProcessAsync_ValidDocument_CompletesSuccessfully`
  - `ProcessAsync_ParsingFails_UpdatesStatusToFailed`
  - `ProcessAsync_EmbeddingFails_UpdatesStatusToFailed`
  - `ProcessAsync_QdrantFails_UpdatesStatusToFailed`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Document Processing Pipeline 整合測試**: `tests/AIAgentPlatform.IntegrationTests/DocumentProcessing/DocumentProcessingPipelineTests.cs`
  - `ProcessDocument_PdfFile_CompletesEndToEnd`
  - `ProcessDocument_DocxFile_CompletesEndToEnd`
  - `ProcessDocument_TxtFile_CompletesEndToEnd`
  - `ProcessDocument_VerifyQdrantIndexing_VectorsStored`
  - 使用真實 Azure OpenAI 與 Qdrant 實例
  - 測試覆蓋率: 100% (整個 pipeline)

**驗收標準 (Phase 4)**:
- ✅ OpenAI Embedding Service 整合成功
- ✅ Qdrant Vector Store 整合成功
- ✅ Hangfire Background Jobs 配置完成
- ✅ Document Processing Pipeline 完整執行
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過 (E2E pipeline)
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 5: 文檔管理 UI 與測試 (0.5 SP) ⏳ 待開始

#### API Layer - Controllers
- [ ] **創建 DocumentsController**: `src/AIAgentPlatform.API/Controllers/DocumentsController.cs`
  - `POST /api/agents/{agentId}/documents` - Upload Document
  - `GET /api/agents/{agentId}/documents` - Get Documents by Agent
  - `GET /api/documents/{documentId}` - Get Document by Id
  - `DELETE /api/documents/{documentId}` - Delete Document
  - `GET /api/documents/{documentId}/download` - Download Document (redirect to SAS URI)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2400-2550

- [ ] **實作 Upload Endpoint**:
  - Request: `IFormFileCollection files`
  - Validation: 檔案格式, 大小 (≤10MB)
  - Response: `UploadDocumentResponse` (DocumentId, Status, BlobUri)
  - Status Code: 201 Created
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2450-2500

- [ ] **實作 Get Documents Endpoint**:
  - Query Params: `pageNumber`, `pageSize`, `sortBy`, `sortOrder`
  - Response: `PaginatedList<DocumentDto>`
  - Status Code: 200 OK
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2500-2550

#### Frontend - Document Management UI
- [ ] **創建 DocumentUpload Component**: `apps/web-app/src/features/documents/components/DocumentUpload.tsx`
  - File Input (支援多檔案上傳)
  - 拖放上傳支援
  - 進度條 (上傳 + 處理狀態)
  - 檔案格式驗證 (client-side)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2550-2650

- [ ] **創建 DocumentList Component**: `DocumentList.tsx`
  - 文檔列表展示 (表格格式)
  - 分頁、排序、篩選
  - 操作按鈕: 下載、刪除
  - 狀態顯示: Uploaded, Processing, Completed, Failed
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2650-2750

- [ ] **創建 DocumentService**: `apps/web-app/src/features/documents/services/documentService.ts`
  - `uploadDocuments(agentId, files)`
  - `getDocuments(agentId, pageNumber, pageSize)`
  - `deleteDocument(documentId)`
  - 使用 Axios HTTP client
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2750-2800

#### E2E Tests (P0)
- [ ] **Document Upload E2E Test**: `tests/AIAgentPlatform.E2ETests/Documents/DocumentUploadTests.cs`
  - `UploadDocument_ValidPdf_Succeeds`
  - `UploadDocument_InvalidFormat_ReturnsError`
  - `UploadDocument_FileTooLarge_ReturnsError`
  - 使用真實 API 與資料庫
  - 測試覆蓋率: 100% (所有端點)

**驗收標準 (Phase 5)**:
- ✅ Document Management API 完整實作
- ✅ Frontend 文檔上傳 UI 完成
- ✅ Frontend 文檔列表 UI 完成
- ✅ E2E 測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

## US 4.2: RAG Retrieval Strategies (5 SP, 4 Phases)

### Phase 1: 語義搜尋實作 (1.5 SP) ⏳ 待開始

#### Application Layer - CQRS Queries
- [ ] **創建 SemanticSearchQuery**: `src/AIAgentPlatform.Application/Knowledge/Queries/SemanticSearch/SemanticSearchQuery.cs`
  - Properties: AgentId, QueryText, TopK (預設 10), ScoreThreshold (預設 0.7)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2800-2850

- [ ] **創建 SemanticSearchQueryHandler**: `SemanticSearchQueryHandler.cs`
  - 步驟 1: Generate query embedding (使用 OpenAIEmbeddingService)
  - 步驟 2: Search in Qdrant (使用 QdrantVectorStoreService)
  - 步驟 3: Filter by ScoreThreshold
  - 步驟 4: Return top-K results
  - Response: `SearchResultDto[]` (ChunkId, DocumentId, Content, Score)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2850-2950

#### Application Layer - DTOs
- [ ] **創建 SearchResultDto**: `SearchResultDto.cs`
  - Properties: ChunkId, DocumentId, Content, Score, FileName, Metadata
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 2950-3000

#### Unit Tests (P0)
- [ ] **SemanticSearchQueryHandler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/Knowledge/Queries/SemanticSearchQueryHandlerTests.cs`
  - `Handle_ValidQuery_ReturnsResults`
  - `Handle_NoResults_ReturnsEmptyList`
  - `Handle_ScoreThreshold_FiltersLowScores`
  - `Handle_TopK_LimitsResults`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Semantic Search 整合測試**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/SemanticSearchTests.cs`
  - `Search_RelevantQuery_ReturnsRelevantChunks`
  - `Search_IrrelevantQuery_ReturnsLowScores`
  - `Search_VerifyCosineSimilarity_IsAccurate`
  - 使用真實 Qdrant 與 indexed documents
  - 測試覆蓋率: 100%

**驗收標準 (Phase 1)**:
- ✅ Semantic Search Query 實作完成
- ✅ Cosine Similarity 計算正確
- ✅ Score Threshold 過濾有效
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 2: 關鍵字搜尋實作 (1 SP) ⏳ 待開始

#### Infrastructure Layer - Full-Text Search
- [ ] **安裝 PostgreSQL FTS Extension**: PostgreSQL 資料庫
  - 檢查: `SELECT * FROM pg_extension WHERE extname = 'pg_trgm';`
  - 安裝: `CREATE EXTENSION IF NOT EXISTS pg_trgm;`
  - 參考: [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) Lines 550-600

- [ ] **創建 Full-Text Search Index**: `document_chunks` table
  - Column: `content_tsv` (tsvector, generated column)
  - Index: `idx_document_chunks_content_fts` (GIN index)
  - SQL: `CREATE INDEX idx_document_chunks_content_fts ON document_chunks USING GIN (to_tsvector('english', content));`
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3000-3050

#### Application Layer - CQRS Queries
- [ ] **創建 KeywordSearchQuery**: `src/AIAgentPlatform.Application/Knowledge/Queries/KeywordSearch/KeywordSearchQuery.cs`
  - Properties: AgentId, Keywords, TopK (預設 10)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3050-3100

- [ ] **創建 KeywordSearchQueryHandler**: `KeywordSearchQueryHandler.cs`
  - 步驟 1: Build FTS Query (`to_tsquery('english', 'keyword1 | keyword2')`)
  - 步驟 2: Execute FTS Query on PostgreSQL
  - 步驟 3: Calculate BM25 Score (ts_rank_cd function)
  - 步驟 4: Order by Score DESC
  - 步驟 5: Return top-K results
  - Response: `SearchResultDto[]` (ChunkId, DocumentId, Content, Score)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3100-3200

#### Unit Tests (P0)
- [ ] **KeywordSearchQueryHandler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/Knowledge/Queries/KeywordSearchQueryHandlerTests.cs`
  - `Handle_ValidQuery_ReturnsResults`
  - `Handle_MultipleKeywords_CombinesWithOR`
  - `Handle_NoMatches_ReturnsEmptyList`
  - `Handle_TopK_LimitsResults`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Keyword Search 整合測試**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/KeywordSearchTests.cs`
  - `Search_ExactKeyword_ReturnsMatches`
  - `Search_PartialKeyword_ReturnsMatches`
  - `Search_MultipleKeywords_ReturnsCombinedResults`
  - `Search_VerifyBM25Score_IsAccurate`
  - 使用真實 PostgreSQL 與 indexed documents
  - 測試覆蓋率: 100%

**驗收標準 (Phase 2)**:
- ✅ PostgreSQL FTS 配置完成
- ✅ Keyword Search Query 實作完成
- ✅ BM25 Score 計算正確
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 3: Hybrid Search 與 Re-ranking (1.5 SP) ⏳ 待開始

#### Application Layer - CQRS Queries
- [ ] **創建 HybridSearchQuery**: `src/AIAgentPlatform.Application/Knowledge/Queries/HybridSearch/HybridSearchQuery.cs`
  - Properties: AgentId, QueryText, TopK (預設 10), SemanticWeight (預設 0.7), KeywordWeight (預設 0.3)
  - Validation: SemanticWeight + KeywordWeight = 1.0
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3200-3250

- [ ] **創建 HybridSearchQueryHandler**: `HybridSearchQueryHandler.cs`
  - 步驟 1: 執行 Semantic Search (並行)
  - 步驟 2: 執行 Keyword Search (並行)
  - 步驟 3: 使用 RRF 融合結果
  - 步驟 4: (Optional) 執行 Cross-Encoder Re-ranking
  - 步驟 5: 返回 top-K 結果
  - Response: `SearchResultDto[]` (ChunkId, Content, Score, RankingMethod)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3250-3400

#### Infrastructure Layer - RRF Fusion
- [ ] **創建 RRFFusionService**: `src/AIAgentPlatform.Infrastructure/Services/RRFFusionService.cs`
  - 演算法: RRF (Reciprocal Rank Fusion)
  - 公式: `score = Σ(weight / (k + rank))`
  - 參數: k = 60 (常數)
  - 輸入: 兩個排序列表 (Semantic, Keyword)
  - 輸出: 融合後的排序列表
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3400-3500

#### Infrastructure Layer - Cross-Encoder Re-ranking
- [ ] **創建 ICrossEncoderService Interface**: `src/AIAgentPlatform.Application/Interfaces/ICrossEncoderService.cs`
  - Methods: `RerankAsync(string query, IEnumerable<string> candidates, CancellationToken)`
  - Return Type: `IReadOnlyList<RerankResult>` (CandidateIndex, Score)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3500-3550

- [ ] **實作 HuggingFaceCrossEncoderService**: `src/AIAgentPlatform.Infrastructure/Services/HuggingFaceCrossEncoderService.cs`
  - 使用 HuggingFace Inference API
  - Model: `cross-encoder/ms-marco-MiniLM-L-6-v2`
  - API Endpoint: `https://api-inference.huggingface.co/models/{model}`
  - 實現 Batch Re-ranking (最多 10 candidates/批次)
  - 實現 Retry 邏輯 (最多 3 次)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3550-3650

#### Unit Tests (P0)
- [ ] **RRFFusionService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/RRFFusionServiceTests.cs`
  - `FuseResults_TwoLists_ReturnsFusedList`
  - `FuseResults_DifferentWeights_AffectsRanking`
  - `FuseResults_EmptyList_HandlesGracefully`
  - `FuseResults_VerifyRRFFormula_IsCorrect`
  - 測試覆蓋率: ≥85%

- [ ] **HuggingFaceCrossEncoderService 單元測試**: `tests/AIAgentPlatform.UnitTests/Infrastructure/Services/HuggingFaceCrossEncoderServiceTests.cs`
  - `RerankAsync_ValidCandidates_ReturnsScores`
  - `RerankAsync_ApiError_RetriesAndSucceeds`
  - `RerankAsync_AllRetriesFail_ThrowsException`
  - 測試覆蓋率: ≥85%

- [ ] **HybridSearchQueryHandler 單元測試**: `tests/AIAgentPlatform.UnitTests/Application/Knowledge/Queries/HybridSearchQueryHandlerTests.cs`
  - `Handle_ValidQuery_ReturnsFusedResults`
  - `Handle_SemanticWeightOnly_ReturnsSemanticResults`
  - `Handle_KeywordWeightOnly_ReturnsKeywordResults`
  - `Handle_WithReranking_ReordersResults`
  - 測試覆蓋率: ≥85%

#### Integration Tests (P0)
- [ ] **Hybrid Search 整合測試**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/HybridSearchTests.cs`
  - `Search_HybridQuery_ReturnsFusedResults`
  - `Search_VerifyRRFFusion_IsAccurate`
  - `Search_WithReranking_ImprovesAccuracy`
  - `Search_DifferentWeights_AffectsRanking`
  - 使用真實 PostgreSQL, Qdrant, HuggingFace API
  - 測試覆蓋率: 100%

**驗收標準 (Phase 3)**:
- ✅ RRF Fusion 演算法實作完成
- ✅ Cross-Encoder Re-ranking 整合成功
- ✅ Hybrid Search Query 實作完成
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試通過
- ✅ Code Review 通過 (無 Critical/High Issues)

---

### Phase 4: 檢索準確率測試框架 (1 SP) ⏳ 待開始

#### Testing Infrastructure - Evaluation Dataset
- [ ] **創建 Evaluation Dataset**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/TestData/evaluation-dataset.json`
  - Structure: `{ "query": "...", "relevantChunks": [...], "irrelevantChunks": [...] }`
  - Size: 至少 50 queries
  - Coverage: 涵蓋各種查詢類型 (Factual, Conceptual, Multi-hop)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3650-3700

#### Testing Infrastructure - Evaluation Metrics
- [ ] **創建 IEvaluationMetric Interface**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/Evaluation/IEvaluationMetric.cs`
  - Methods: `Calculate(query, retrievedChunks, relevantChunks)`
  - Return Type: `double` (metric score)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3700-3750

- [ ] **實作 AccuracyMetric**: `AccuracyMetric.cs`
  - Formula: `Accuracy = CorrectRetrievals / TotalQueries`
  - Threshold: ≥90%
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3750-3800

- [ ] **實作 MRRMetric**: `MRRMetric.cs`
  - Formula: `MRR = (1/n) * Σ(1/rank_i)`
  - Threshold: ≥0.8
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3800-3850

- [ ] **實作 NDCGMetric**: `NDCGMetric.cs`
  - Formula: `NDCG = DCG / IDCG`
  - DCG: `Σ(rel_i / log2(i+1))`
  - Threshold: ≥0.85
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3850-3900

#### Testing Infrastructure - Evaluation Runner
- [ ] **創建 RAGEvaluationRunner**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/Evaluation/RAGEvaluationRunner.cs`
  - 載入 Evaluation Dataset
  - 對每個 query 執行 Hybrid Search
  - 計算 Accuracy, MRR, NDCG
  - 生成評估報告 (JSON/Markdown)
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 3900-4000

#### Integration Tests (P0)
- [ ] **RAG Accuracy 評估測試**: `tests/AIAgentPlatform.IntegrationTests/Knowledge/RAGAccuracyTests.cs`
  - `Evaluate_SemanticSearch_MeetsAccuracyThreshold`
  - `Evaluate_KeywordSearch_MeetsAccuracyThreshold`
  - `Evaluate_HybridSearch_MeetsAccuracyThreshold`
  - `Evaluate_HybridWithReranking_ImprovesMRR`
  - 驗證: Accuracy ≥90%, MRR ≥0.8, NDCG ≥0.85
  - 測試覆蓋率: 100%

#### Documentation
- [ ] **創建 Evaluation Report Template**: `docs/testing/rag-evaluation-report-template.md`
  - Sections: Test Configuration, Metrics Summary, Query Examples, Failure Analysis
  - 參考: [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) Lines 4000-4050

**驗收標準 (Phase 4)**:
- ✅ Evaluation Dataset 建立完成 (≥50 queries)
- ✅ 3 種評估指標實作完成 (Accuracy, MRR, NDCG)
- ✅ Evaluation Runner 實作完成
- ✅ RAG Accuracy 測試通過 (≥90%)
- ✅ MRR 測試通過 (≥0.8)
- ✅ NDCG 測試通過 (≥0.85)
- ✅ Evaluation Report 生成成功
- ✅ Code Review 通過 (無 Critical/High Issues)

---

## 測試

### Unit Tests (P0)
- [ ] **Domain Layer 單元測試**:
  - Document Entity Tests (5 tests)
  - DocumentChunk Entity Tests (4 tests)
  - FileMetadata VO Tests (3 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Application Layer 單元測試**:
  - UploadDocumentCommandHandler Tests (4 tests)
  - DocumentProcessingJob Tests (4 tests)
  - SemanticSearchQueryHandler Tests (4 tests)
  - KeywordSearchQueryHandler Tests (4 tests)
  - HybridSearchQueryHandler Tests (4 tests)
  - 測試覆蓋率: ≥85%

- [ ] **Infrastructure Layer 單元測試**:
  - AzureBlobStorageService Tests (4 tests)
  - PdfDocumentParser Tests (4 tests)
  - DocxDocumentParser Tests (3 tests)
  - TextDocumentParser Tests (3 tests)
  - FixedSizeChunker Tests (5 tests)
  - OpenAIEmbeddingService Tests (4 tests)
  - QdrantVectorStoreService Tests (4 tests)
  - RRFFusionService Tests (4 tests)
  - HuggingFaceCrossEncoderService Tests (3 tests)
  - 測試覆蓋率: ≥85%

### Integration Tests (P0)
- [ ] **Document Parsing 整合測試**:
  - ParsePdfDocument_RealFile_ExtractsText
  - ParseDocxDocument_RealFile_ExtractsText
  - ParseTextDocument_RealFile_ExtractsText
  - 測試覆蓋率: 100%

- [ ] **Document Processing Pipeline 整合測試**:
  - ProcessDocument_PdfFile_CompletesEndToEnd
  - ProcessDocument_DocxFile_CompletesEndToEnd
  - ProcessDocument_TxtFile_CompletesEndToEnd
  - ProcessDocument_VerifyQdrantIndexing_VectorsStored
  - 測試覆蓋率: 100%

- [ ] **RAG Retrieval 整合測試**:
  - SemanticSearch_RelevantQuery_ReturnsRelevantChunks
  - KeywordSearch_ExactKeyword_ReturnsMatches
  - HybridSearch_HybridQuery_ReturnsFusedResults
  - HybridSearch_WithReranking_ImprovesAccuracy
  - 測試覆蓋率: 100%

### E2E Tests (P0)
- [ ] **Document Upload E2E Test**:
  - UploadDocument_ValidPdf_Succeeds
  - UploadDocument_InvalidFormat_ReturnsError
  - UploadDocument_FileTooLarge_ReturnsError
  - 測試覆蓋率: 100%

- [ ] **RAG Search E2E Test**:
  - SearchDocuments_SemanticQuery_ReturnsResults
  - SearchDocuments_KeywordQuery_ReturnsResults
  - SearchDocuments_HybridQuery_ReturnsFusedResults
  - 測試覆蓋率: 100%

### Performance Tests (P1)
- [ ] **Document Processing Performance**:
  - Upload 100 documents (各 1MB) → 完成時間 <5分鐘
  - Parse 100 PDFs → 平均解析時間 <3秒/檔案
  - Chunk 1000 pages → 平均分塊時間 <1秒/頁面

- [ ] **RAG Retrieval Performance**:
  - Semantic Search → 查詢延遲 <200ms
  - Keyword Search → 查詢延遲 <100ms
  - Hybrid Search → 查詢延遲 <300ms
  - Hybrid Search with Re-ranking → 查詢延遲 <500ms

**測試驗收標準**:
- ✅ 單元測試覆蓋率 ≥85%
- ✅ 整合測試覆蓋率 100%
- ✅ E2E 測試覆蓋率 100%
- ✅ 所有測試通過率 100%
- ✅ 性能測試達標

---

## 文檔

### API Documentation (P0)
- [ ] **Swagger Documentation**:
  - 所有 API 端點有完整 XML 註解
  - Request/Response Schema 定義完整
  - Example Requests/Responses
  - Error Codes 說明

- [ ] **API Design Document**:
  - `docs/api/knowledge-management-api.md`
  - 包含所有端點設計
  - Request/Response 範例
  - 錯誤處理策略

### Architecture Documentation (P0)
- [ ] **ADR-015: Document Storage Strategy**:
  - `docs/architecture/adr/ADR-015-document-storage-strategy.md`
  - 決策: Azure Blob + PostgreSQL + Qdrant
  - 理由, 替代方案, 後果

- [ ] **ADR-016: Chunking Strategy**:
  - `docs/architecture/adr/ADR-016-chunking-strategy.md`
  - 決策: Fixed-Size Chunking with Overlap
  - Token-based 分塊, 1024 tokens 預設

- [ ] **ADR-017: RAG Retrieval Strategy**:
  - `docs/architecture/adr/ADR-017-rag-retrieval-strategy.md`
  - 決策: Hybrid Search + RRF + Cross-Encoder
  - 70%/30% 權重配置

- [ ] **ADR-018: Embedding Service Design**:
  - `docs/architecture/adr/ADR-018-embedding-service-design.md`
  - 決策: Azure OpenAI ada-002 + Memory Cache
  - 1 小時 TTL, SHA256 cache key

- [ ] **ADR-019: Background Job Architecture**:
  - `docs/architecture/adr/ADR-019-background-job-architecture.md`
  - 決策: Hangfire + PostgreSQL Storage
  - 6-step processing pipeline

### Technical Implementation Guides (P1)
- [ ] **Document Processing Guide**:
  - `docs/technical-implementation/knowledge-management/document-processing.md`
  - 如何新增 Document Parser
  - 如何配置 Chunking Strategy
  - 如何整合新的 Embedding Model

- [ ] **RAG Retrieval Guide**:
  - `docs/technical-implementation/knowledge-management/rag-retrieval.md`
  - 如何調整 RRF 權重
  - 如何選擇 Cross-Encoder Model
  - 如何優化檢索準確率

### User Documentation (P1)
- [ ] **Knowledge Management User Guide**:
  - `docs/user-guides/knowledge-management.md`
  - 如何上傳文檔
  - 如何搜尋知識庫
  - 如何管理文檔

**文檔驗收標準**:
- ✅ API Documentation 完整
- ✅ 5 個 ADR 文檔完成
- ✅ Technical Implementation Guides 完成
- ✅ User Guide 完成
- ✅ 所有文檔 Review 通過

---

## 部署與DevOps

### Infrastructure Setup (P0)
- [ ] **Azure Blob Storage**:
  - 創建 Storage Account (熱存儲層)
  - 創建 Container: `documents`
  - 配置生命週期管理 (30 天後轉 Cool Tier)
  - 配置 CORS 規則

- [ ] **Qdrant Deployment**:
  - 部署 Qdrant 1.7.4 (Docker 或 Cloud)
  - 創建 Collection: `agent_knowledge`
  - 配置 HNSW Index
  - 配置備份策略

- [ ] **PostgreSQL Extension**:
  - 安裝 pgvector Extension
  - 安裝 pg_trgm Extension (Full-Text Search)
  - 驗證 Extensions 運作正常

- [ ] **Hangfire Dashboard**:
  - 配置 Dashboard Authorization (僅 Admin)
  - 配置 Job Storage (PostgreSQL)
  - 配置 Job Queue: `knowledge_processing`

### Environment Variables (P0)
- [ ] **Azure OpenAI Configuration**:
  - `AZURE_OPENAI_API_KEY`
  - `AZURE_OPENAI_ENDPOINT`
  - `AZURE_OPENAI_EMBEDDING_DEPLOYMENT_NAME` (text-embedding-ada-002)

- [ ] **Azure Blob Storage Configuration**:
  - `AZURE_STORAGE_CONNECTION_STRING`
  - `AZURE_STORAGE_CONTAINER_NAME` (documents)

- [ ] **Qdrant Configuration**:
  - `QDRANT_ENDPOINT` (http://localhost:6333 or cloud URL)
  - `QDRANT_API_KEY` (optional for cloud)

- [ ] **HuggingFace Configuration**:
  - `HUGGINGFACE_API_KEY`
  - `HUGGINGFACE_MODEL` (cross-encoder/ms-marco-MiniLM-L-6-v2)

### CI/CD Pipeline (P0)
- [ ] **Backend CI Pipeline**:
  - Build .NET Solution
  - Run Unit Tests (≥85% coverage)
  - Run Integration Tests
  - Generate Code Coverage Report
  - Run Static Code Analysis (SonarQube)

- [ ] **Frontend CI Pipeline**:
  - Build React App (npm run build)
  - Run TypeScript Type Check
  - Run ESLint
  - Run Unit Tests (Vitest)

- [ ] **CD Pipeline**:
  - Deploy to Staging Environment
  - Run E2E Tests on Staging
  - Deploy to Production (manual approval)
  - Run Smoke Tests on Production

### Monitoring & Alerts (P1)
- [ ] **Application Insights**:
  - Log Document Processing Metrics (processing time, success rate)
  - Log RAG Search Metrics (query latency, accuracy)
  - Log Embedding Service Metrics (cache hit rate, API latency)
  - Log Qdrant Metrics (index size, query performance)

- [ ] **Health Checks**:
  - Azure Blob Storage Health Check
  - Qdrant Health Check
  - PostgreSQL Health Check
  - Azure OpenAI Health Check
  - HuggingFace API Health Check

**部署驗收標準**:
- ✅ Infrastructure 設置完成
- ✅ Environment Variables 配置完成
- ✅ CI/CD Pipeline 運作正常
- ✅ Monitoring & Alerts 配置完成
- ✅ Health Checks 全部通過

---

## Definition of Done 驗證

### Sprint 5 Definition of Done

#### Functionality (P0)
- [ ] ✅ **US 4.1 完成**:
  - 所有 5 個 Phases 完成
  - Document Upload API 正常運作
  - Document Processing Pipeline 正常運作
  - Frontend Document Management UI 完成

- [ ] ✅ **US 4.2 完成**:
  - 所有 4 個 Phases 完成
  - Semantic Search API 正常運作
  - Keyword Search API 正常運作
  - Hybrid Search API 正常運作
  - RAG Accuracy ≥90%

#### Code Quality (P0)
- [ ] ✅ **測試覆蓋率**:
  - Unit Tests ≥85%
  - Integration Tests 100%
  - E2E Tests 100%

- [ ] ✅ **Code Review**:
  - 所有 PR Review 通過
  - 0 Critical Issues
  - 0 High Issues

- [ ] ✅ **Static Analysis**:
  - SonarQube Quality Gate 通過
  - 0 Code Smells (Major)
  - 0 Security Vulnerabilities

#### Performance (P0)
- [ ] ✅ **Document Processing Performance**:
  - Upload API Response Time <2秒
  - Document Parsing <3秒/檔案
  - Chunking <1秒/頁面
  - Embedding Generation <500ms/chunk

- [ ] ✅ **RAG Retrieval Performance**:
  - Semantic Search <200ms
  - Keyword Search <100ms
  - Hybrid Search <300ms
  - Hybrid with Re-ranking <500ms

#### Documentation (P0)
- [ ] ✅ **API Documentation**:
  - Swagger 完整
  - API Design Document 完成

- [ ] ✅ **Architecture Documentation**:
  - 5 個 ADR 完成

- [ ] ✅ **Technical Guides**:
  - Document Processing Guide 完成
  - RAG Retrieval Guide 完成

#### Deployment (P0)
- [ ] ✅ **Infrastructure Ready**:
  - Azure Blob Storage 配置完成
  - Qdrant 部署完成
  - PostgreSQL Extensions 安裝完成
  - Hangfire 配置完成

- [ ] ✅ **CI/CD Pipeline**:
  - Backend CI Pipeline 通過
  - Frontend CI Pipeline 通過
  - CD Pipeline 部署成功

- [ ] ✅ **Monitoring**:
  - Application Insights 配置完成
  - Health Checks 全部通過

**最終驗收**:
- ✅ 所有功能開發完成
- ✅ 所有測試通過
- ✅ 所有文檔完成
- ✅ 部署成功
- ✅ Sprint Review 通過
- ✅ Stakeholder Sign-off

---

## 參考文獻索引

### Sprint 5 核心文檔
1. **SPRINT-5-1-OVERVIEW.md** - Sprint 5 概覽 (1,150 行)
   - Lines 1-100: Sprint 目標與範圍
   - Lines 100-300: US 4.1 Phase Breakdown
   - Lines 300-500: US 4.2 Phase Breakdown
   - Lines 500-800: 風險管理與依賴
   - Lines 800-1150: 參考文獻索引

2. **SPRINT-5-2-PLAN.md** - Sprint 5 實施計劃 (2,500 行)
   - Lines 1-500: US 4.1 Document Upload 實施計劃
   - Lines 500-1400: US 4.1 Document Processing 實施計劃
   - Lines 1400-2400: US 4.1 Vectorization 實施計劃
   - Lines 2400-3200: US 4.2 RAG Retrieval 實施計劃
   - Lines 3200-4000: US 4.2 Evaluation 實施計劃

3. **SPRINT-5-3-CONTEXT.md** - Sprint 5 技術上下文 (1,100 行)
   - Lines 1-200: Core Technology Reference
   - Lines 200-600: US 4.1 Technical Context
   - Lines 600-1000: US 4.2 Technical Context
   - Lines 1000-1100: Reference Index

### User Story 規格文檔
4. **module-05-knowledge-management.md** - US 4.1, 4.2 詳細規格
   - US 5.1 (對應 US 4.1): Document Upload & Processing
   - US 5.2 (對應 US 4.2): RAG Retrieval Strategies

### Architecture Design Documents
5. **ADR-015-document-storage-strategy.md** - 文檔存儲架構決策
6. **ADR-016-chunking-strategy.md** - 分塊策略決策
7. **ADR-017-rag-retrieval-strategy.md** - RAG 檢索策略決策
8. **ADR-018-embedding-service-design.md** - Embedding 服務設計
9. **ADR-019-background-job-architecture.md** - 背景作業架構

### API Design Documents
10. **knowledge-management-api.md** - Knowledge Management API 設計
    - Document Upload API
    - RAG Search API

### Database Design
11. **database-schema.md** - 資料庫 Schema 設計
    - documents table
    - document_chunks table
    - pgvector extension

### Testing Documentation
12. **unit-testing-guidelines.md** - 單元測試指南
13. **integration-testing-strategy.md** - 整合測試策略
14. **rag-evaluation-report-template.md** - RAG 評估報告模板

### 前期 Sprint 文檔 (格式參考)
15. **SPRINT-4-4-CHECKLIST.md** - Sprint 4 Checklist 格式參考
16. **SPRINT-3-4-CHECKLIST.md** - Sprint 3 Checklist 格式參考

---

## 版本歷史

| 版本 | 日期 | 作者 | 變更說明 |
|------|------|------|----------|
| v2.1 | 2025-11-14 | AI Assistant | 初始版本建立，完整 Sprint 5 任務清單 (210 項任務) |
|      |            |              | - US 4.1: 82 項任務 (5 Phases) |
|      |            |              | - US 4.2: 65 項任務 (4 Phases) |
|      |            |              | - 測試: 28 項任務 |
|      |            |              | - 文檔: 15 項任務 |
|      |            |              | - 部署: 12 項任務 |
|      |            |              | 遵循 v2.1 標準 (8 欄位 header, 完整 ToC, 參考索引) |

---

**文件結束** - Sprint 5 任務清單已完整建立 ✅
