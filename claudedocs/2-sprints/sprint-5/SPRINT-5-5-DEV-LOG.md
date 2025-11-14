# SPRINT-5-5-DEV-LOG.md - Sprint 5 開發日誌：Knowledge 管理與 RAG 檢索實作記錄

**版本**: v2.1
**Sprint 編號**: Sprint 5
**Sprint 週期**: Week 13-15 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2026-01-06 ~ 2026-01-26
**狀態**: 📋 計劃階段 (Planned) - 將在 Sprint 執行時更新
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 📋 文件目的

本文件作為 Sprint 5 執行期間的**每日開發日誌**，記錄：
- 每日開發進度與完成任務
- 技術決策與實作細節
- 遇到的問題與解決方案
- 程式碼變更與 Commit 記錄
- 測試執行結果
- 團隊協作與溝通事項

**更新頻率**: 每日更新（開發日結束時）

---

## 📑 目錄 (Table of Contents)

1. [使用指南](#-使用指南)
2. [Sprint 5 目標回顧](#-sprint-5-目標回顧)
3. [Week 13 開發日誌](#week-13-開發日誌)
4. [Week 14 開發日誌](#week-14-開發日誌)
5. [Week 15 開發日誌](#week-15-開發日誌)
6. [技術決策記錄](#-技術決策記錄)
7. [問題與解決方案](#-問題與解決方案)
8. [經驗總結](#-經驗總結)
9. [參考文檔](#-參考文檔)
10. [版本歷史](#-版本歷史)

---

## 📖 使用指南

### 適用對象
- **開發團隊**: 記錄每日進度與技術決策
- **Tech Lead**: 追蹤開發狀況與問題
- **Scrum Master**: 監控 Sprint 進展與阻礙
- **未來團隊成員**: 了解歷史決策與實作脈絡
- **AI Assistant**: 追蹤開發狀態與問題解決

### 更新時機
- **每日站會後**: 更新昨日完成的任務與今日計劃
- **重要決策時**: 即時記錄技術決策與理由
- **遇到問題時**: 記錄問題與暫時/最終解決方案
- **完成任務時**: 記錄實作細節與 Commit ID
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

#### 📊 測試結果
- 單元測試、整合測試結果
- 覆蓋率統計

#### 📝 備註
- 其他重要資訊
```

### 快速導航
- **查看 Sprint 5 整體概覽** → [SPRINT-5-1-OVERVIEW.md](./SPRINT-5-1-OVERVIEW.md)
- **查看詳細實施計劃** → [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md)
- **查看技術上下文** → [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md)
- **查看任務檢查清單** → [SPRINT-5-4-CHECKLIST.md](./SPRINT-5-4-CHECKLIST.md)
- **查看問題追蹤** → [SPRINT-5-6-ISSUES.md](./SPRINT-5-6-ISSUES.md)

---

## 🎯 Sprint 5 目標回顧

### 主要交付成果
1. **Document Upload & Processing (US 4.1, 8 SP)**
   - 文檔上傳 API (PDF/DOCX/TXT/MD)
   - 文檔解析引擎 (3 種 Parser)
   - 文檔分塊 (Fixed-Size Chunking)
   - 向量化與索引 (Azure OpenAI + Qdrant)
   - Background Processing Pipeline (Hangfire)

2. **RAG Retrieval Strategies (US 4.2, 5 SP)**
   - 語義搜尋 (Qdrant Vector Search)
   - 關鍵字搜尋 (PostgreSQL FTS + BM25)
   - Hybrid Search (RRF Fusion)
   - Cross-Encoder Re-ranking (HuggingFace)
   - RAG Accuracy Evaluation (≥90%)

### Definition of Done
- [ ] 所有 User Story 的 Acceptance Criteria 滿足
- [ ] 單元測試覆蓋率 ≥85%
- [ ] 整合測試通過率 100%
- [ ] RAG 檢索準確率 ≥90%
- [ ] API 文件完整（Swagger）
- [ ] 5 個 ADR 文檔完成
- [ ] 程式碼審查通過
- [ ] 無 Critical/High 等級 Bug
- [ ] Performance 測試達標

### Story Points 分配
- US 4.1: Document Upload & Processing (8 SP)
- US 4.2: RAG Retrieval Strategies (5 SP)
- **總計**: 13 SP

---

## Week 13 開發日誌

> **Week 13 目標**: US 4.1 文檔上傳基礎架構 + 文檔解析與分塊
> **預期完成**: US 4.1 Phase 1-3 (5.5 SP, ~42%)

### 📅 2026-01-06 (Monday) - Sprint 5 啟動與環境設置

#### ✅ 完成任務
- [ ] **Sprint Planning 會議**
  - 時間: 09:00-11:00
  - 參與者: 全團隊
  - 產出: Sprint 5 Backlog 確認，13 SP 分配
  - US 4.1 拆分為 5 個 Phases
  - US 4.2 拆分為 4 個 Phases

- [ ] **環境設置與驗證**
  - Azure Blob Storage 創建與配置
    - Storage Account: `aiagentplatformstorage`
    - Container: `documents`
    - Tier: Hot Storage
    - CORS 規則配置完成
  - Qdrant 部署 (Docker)
    - Version: 1.7.4
    - Port: 6333
    - Dashboard: http://localhost:6333/dashboard
  - PostgreSQL Extensions 安裝
    - `pgvector` extension 安裝成功
    - `pg_trgm` extension 安裝成功
  - Hangfire Dashboard 配置
    - URL: http://localhost:5095/hangfire
    - Authorization: Admin Only
  - 相關 Commit: `[待填寫]`

- [ ] **Feature Branch 創建**
  - Branch: `feature/us-4.1-document-upload`
  - Base: `main`
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-024: Azure Blob Storage Tier 策略**
  - 決策: 使用 Hot Tier + Lifecycle Management (30 天後轉 Cool Tier)
  - 理由: 新上傳文檔頻繁訪問，舊文檔訪問頻率低
  - 成本優化: 預估節省 50% 儲存成本
  - 參考: ADR-015 Document Storage Strategy

- [ ] **TD-025: Qdrant Deployment 策略**
  - 決策: 使用 Docker 部署 (Development), Qdrant Cloud (Production)
  - 理由: 開發環境靈活，生產環境穩定
  - 配置: HNSW Index (m=16, ef_construct=100, ef=128)

#### 📝 備註
- [ ] 所有基礎設施就緒
- [ ] 環境變數配置完成
- [ ] 團隊對 Sprint 5 目標達成共識

---

### 📅 2026-01-07 (Tuesday) - Domain Entities 與 Blob Storage

#### ✅ 完成任務
- [ ] **Document Domain Entities 設計**
  - 建立 `Document.cs` Entity
    ```csharp
    // src/AIAgentPlatform.Domain/Entities/Document.cs
    public class Document : BaseEntity
    {
        public Guid AgentId { get; private set; }
        public string FileName { get; private set; }
        public long FileSize { get; private set; }
        public string FileExtension { get; private set; }
        public string BlobUri { get; private set; }
        public ProcessingStatus Status { get; private set; }
        public DateTime UploadedAt { get; private set; }
        public DateTime? ProcessedAt { get; private set; }
    }
    ```
  - 建立 `DocumentChunk.cs` Entity
  - 建立 `ProcessingStatus` Enum
  - 相關 Commit: `[待填寫]`

- [ ] **Value Objects 設計**
  - 建立 `FileMetadata` VO
    - Properties: FileName, FileSize, FileExtension, ContentType, Hash
    - Validation: 檔案格式驗證, 大小限制 (≤10MB)
  - 建立 `BlobReference` VO
    - Properties: ContainerName, BlobName, BlobUri
    - Validation: URI 格式驗證
  - 實作 Equality 與 Validation
  - 相關 Commit: `[待填寫]`

- [ ] **AzureBlobStorageService 實作**
  - 實作 `IBlobStorageService` Interface
  - 實作 `UploadAsync` 方法
  - 實作 `DownloadAsync` 方法
  - 實作 `DeleteAsync` 方法
  - 實作 `GetSasUriAsync` 方法 (1 小時有效期)
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-026: Document 檔案大小限制**
  - 決策: 10MB 上限
  - 理由: 平衡處理速度與使用需求
  - 超大檔案處理: 未來可考慮分段上傳

- [ ] **TD-027: Blob SAS Token 有效期**
  - 決策: 1 小時有效期
  - 理由: 安全性考量，避免長期暴露
  - 替代方案: 動態延長（未採用，過於複雜）

#### ⚠️ 遇到問題
- [ ] **問題**: Azure Blob Storage SDK 版本衝突
  - 影響: 編譯失敗
  - 解決方案: 統一使用 `Azure.Storage.Blobs` v12.x
  - 參考: [Azure SDK Migration Guide](https://learn.microsoft.com/en-us/dotnet/azure/sdk/azure-sdk-for-dotnet)

#### 📊 測試結果
```
Domain Layer Unit Tests:
- Document Entity: 5/5 Passed
- FileMetadata VO: 3/3 Passed
- Coverage: 92%

Blob Storage Service Unit Tests:
- UploadAsync: 2/2 Passed
- DownloadAsync: 2/2 Passed
- DeleteAsync: 1/1 Passed
- GetSasUriAsync: 1/1 Passed
- Coverage: 88%
```

#### 📝 備註
- [ ] Domain Entities 設計完成
- [ ] Blob Storage Service 基本功能完成

---

### 📅 2026-01-08 (Wednesday) - CQRS Commands 與 EF Configuration

#### ✅ 完成任務
- [ ] **UploadDocumentCommand + Handler**
  - 建立 `UploadDocumentCommand.cs`
  - 建立 `UploadDocumentCommandHandler.cs`
    - 檔案驗證 (格式, 大小)
    - 上傳至 Azure Blob Storage
    - 創建 Document Entity
    - 觸發 Background Job
  - FluentValidation 驗證規則
  - 相關 Commit: `[待填寫]`

- [ ] **DeleteDocumentCommand + Handler**
  - 建立 `DeleteDocumentCommand.cs`
  - 建立 `DeleteDocumentCommandHandler.cs`
    - 軟刪除 Document
    - 刪除 Blob
    - 刪除 Qdrant Vectors
  - 相關 Commit: `[待填寫]`

- [ ] **EF Core Configuration**
  - 建立 `DocumentConfiguration.cs`
    - Table: `documents`
    - Indexes: `idx_documents_agent_id`, `idx_documents_status`, `idx_documents_uploaded_at`
    - Unique Constraint: `agent_id + file_name`
  - 建立 `DocumentChunkConfiguration.cs`
    - Table: `document_chunks`
    - Indexes: `idx_document_chunks_document_id`, `idx_document_chunks_chunk_index`
    - Vector Column: `embedding_vector` (dimension 1536)
  - 相關 Commit: `[待填寫]`

- [ ] **Database Migration**
  - 創建 Migration: `dotnet ef migrations add AddDocumentManagement`
  - 執行 Migration: `dotnet ef database update`
  - 驗證: Tables 創建成功
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-028: Document 唯一性約束**
  - 決策: `agent_id + file_name` 唯一約束 (僅未刪除記錄)
  - 理由: 避免重複上傳相同檔案
  - 實作: Partial Unique Index (WHERE IsDeleted = false)

#### 📊 測試結果
```
Application Layer Unit Tests:
- UploadDocumentCommandHandler: 4/4 Passed
- DeleteDocumentCommandHandler: 3/3 Passed
- Coverage: 90%

Migration Tests:
- Migration Up: Passed
- Migration Down: Passed
- Index Creation: Passed
```

#### 📝 備註
- [ ] Document Upload Command 功能完成
- [ ] Database Schema 建立成功

---

### 📅 2026-01-09 (Thursday) - Document Parsers 實作

#### ✅ 完成任務
- [ ] **IDocumentParser Interface 設計**
  - 定義 `ParseAsync` 方法
  - 定義 `SupportedExtensions` 屬性
  - 建立 `DocumentParseResult` DTO
  - 相關 Commit: `[待填寫]`

- [ ] **PdfDocumentParser 實作**
  - 使用 PdfPig NuGet Package
  - 提取所有文字內容
  - 提取頁面數量
  - 提取 Metadata (Title, Author, CreationDate)
  - 處理加密 PDF (返回錯誤)
  - 相關 Commit: `[待填寫]`

- [ ] **DocxDocumentParser 實作**
  - 使用 DocumentFormat.OpenXml NuGet Package
  - 提取所有段落文字
  - 提取 Metadata (Title, Author)
  - 處理損壞的 DOCX (返回錯誤)
  - 相關 Commit: `[待填寫]`

- [ ] **TextDocumentParser 實作**
  - 使用 StreamReader
  - 支援 UTF-8 編碼
  - 支援 TXT 與 MD 檔案
  - 相關 Commit: `[待填寫]`

- [ ] **DocumentParserFactory 實作**
  - Factory Pattern 實現
  - 根據檔案副檔名選擇 Parser
  - 註冊所有 Parser
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-029: PDF Parser 選擇**
  - 決策: 使用 PdfPig 而非 iTextSharp
  - 理由: 開源、MIT License、社群活躍
  - 效能: 解析速度 <3秒/檔案 (10頁 PDF)

- [ ] **TD-030: DOCX Parser 選擇**
  - 決策: 使用 DocumentFormat.OpenXml (官方套件)
  - 理由: Microsoft 官方支援、穩定可靠
  - 注意: 僅提取純文字，不保留格式

#### ⚠️ 遇到問題
- [ ] **問題**: PdfPig 無法解析某些 PDF 檔案
  - 原因: PDF 格式損壞或加密
  - 解決方案: 實作 try-catch，返回友善錯誤訊息
  - 後續: 考慮使用 Tesseract OCR 處理掃描 PDF (Phase 2)

#### 📊 測試結果
```
Document Parser Unit Tests:
- PdfDocumentParser: 4/4 Passed
- DocxDocumentParser: 3/3 Passed
- TextDocumentParser: 3/3 Passed
- DocumentParserFactory: 4/4 Passed
- Coverage: 87%

Integration Tests (Real Files):
- Parse PDF: Passed (sample.pdf, 10 pages)
- Parse DOCX: Passed (sample.docx, 5 pages)
- Parse TXT: Passed (sample.txt, 1000 lines)
```

#### 📝 備註
- [ ] 3 種 Document Parser 實作完成
- [ ] DocumentParserFactory 正確路由
- [ ] 下一步: 文檔分塊

---

### 📅 2026-01-10 (Friday) - Document Chunking 實作

#### ✅ 完成任務
- [ ] **TikToken Integration**
  - 安裝 `Microsoft.SemanticKernel.Connectors.OpenAI` (包含 TikToken)
  - 建立 `TikTokenCounter.cs`
  - 使用 cl100k_base encoding (GPT-4/ada-002)
  - 相關 Commit: `[待填寫]`

- [ ] **FixedSizeChunker 實作**
  - 建立 `FixedSizeChunker.cs`
  - 實作固定大小分塊演算法
  - 實作重疊邏輯 (Overlap Tokens)
  - 邊界處理: 最後一塊若 < minChunkSize 則合併至前一塊
  - 相關 Commit: `[待填寫]`

- [ ] **ChunkingOptions 設計**
  - Properties: ChunkSize (512-2048), OverlapSize (0-200), MinChunkSize
  - Validation: ChunkSize > OverlapSize
  - 預設值: ChunkSize=1024, OverlapSize=100
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-031: Chunking 策略選擇**
  - 決策: Fixed-Size Chunking with Overlap
  - 理由: 簡單可靠、可控性高、效能穩定
  - 替代方案: Semantic Chunking (過於複雜，準確率不穩定)
  - 參考: ADR-016 Chunking Strategy

- [ ] **TD-032: 預設 Chunk Size**
  - 決策: 1024 tokens (預設)
  - 理由: 平衡檢索準確率與上下文完整性
  - 範圍: 512-2048 tokens (可配置)
  - 測試: 不同 Chunk Size 對準確率的影響

#### 📊 測試結果
```
Chunking Unit Tests:
- TikTokenCounter: 3/3 Passed
- FixedSizeChunker: 5/5 Passed
- ChunkingOptions Validation: 3/3 Passed
- Coverage: 91%

Integration Tests:
- Chunk Short Text: Passed (1 chunk)
- Chunk Long Text: Passed (10 chunks)
- Overlap Verification: Passed (100 tokens overlap)
- Chunk Size Verification: Passed (all within 512-2048)
```

#### 📝 備註
- [ ] Week 13 目標達成：US 4.1 Phase 1-3 完成
- [ ] 下週重點：向量化與索引 (Phase 4)

---

## Week 14 開發日誌

> **Week 14 目標**: US 4.1 向量化與索引 + US 4.2 語義搜尋與關鍵字搜尋
> **預期完成**: US 4.1 Phase 4-5 + US 4.2 Phase 1-2 (4.5 SP, ~77%)

### 📅 2026-01-13 (Monday) - Embedding Service 與 Qdrant Integration

#### ✅ 完成任務
- [ ] **OpenAIEmbeddingService 實作**
  - 建立 `OpenAIEmbeddingService.cs`
  - 整合 Azure OpenAI SDK
  - Model: text-embedding-ada-002
  - Dimension: 1536
  - 實作 Memory Cache (1 小時 TTL)
  - Cache Key: `embedding:{SHA256(text)}`
  - Batch Processing: 最多 16 個文本/批次
  - 相關 Commit: `[待填寫]`

- [ ] **QdrantVectorStoreService 實作**
  - 安裝 Qdrant.Client NuGet Package
  - 建立 `QdrantVectorStoreService.cs`
  - 實作 `CreateCollectionAsync` (HNSW configuration)
  - 實作 `UpsertVectorsAsync` (Batch upsert, 最多 100 vectors)
  - 實作 `SearchAsync` (Cosine similarity)
  - 相關 Commit: `[待填寫]`

- [ ] **Qdrant Collection 創建**
  - Collection Name: `agent_knowledge`
  - Vector Size: 1536
  - Distance Metric: Cosine
  - HNSW Config: m=16, ef_construct=100, ef=128
  - Full Scan Threshold: 10000
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-033: Embedding Cache 策略**
  - 決策: Memory Cache with 1 小時 TTL
  - 理由: 減少 API 呼叫、降低成本、提升速度
  - Cache Hit Rate 預估: 30-40%
  - 成本節省: 預估節省 35% API 成本
  - 參考: ADR-018 Embedding Service Design

- [ ] **TD-034: HNSW 參數調校**
  - 決策: m=16, ef_construct=100, ef=128
  - 理由: 平衡檢索速度與準確率
  - 測試: 不同參數對 recall 的影響
  - Recall@10: ≥95%

#### ⚠️ 遇到問題
- [ ] **問題**: Azure OpenAI API Rate Limit
  - 影響: Batch embedding 處理速度慢
  - 解決方案: 實作 Retry with Exponential Backoff
  - 最大重試: 3 次
  - 後續: 考慮申請提高 Rate Limit

#### 📊 測試結果
```
Embedding Service Unit Tests:
- GenerateEmbeddingAsync: 4/4 Passed
- Cache Hit/Miss: 2/2 Passed
- Batch Processing: 2/2 Passed
- Coverage: 89%

Qdrant Service Unit Tests:
- CreateCollectionAsync: 1/1 Passed
- UpsertVectorsAsync: 2/2 Passed
- SearchAsync: 3/3 Passed
- Coverage: 86%
```

#### 📝 備註
- [ ] Embedding Service 與 Qdrant Integration 完成
- [ ] Cache 機制運作良好

---

### 📅 2026-01-14 (Tuesday) - Background Processing Pipeline

#### ✅ 完成任務
- [ ] **Hangfire 配置**
  - 安裝 Hangfire NuGet Packages (Core, AspNetCore, PostgreSql)
  - 配置 PostgreSQL Storage
  - 配置 Dashboard Authorization (Admin Only)
  - Job Queue: `knowledge_processing`
  - 相關 Commit: `[待填寫]`

- [ ] **DocumentProcessingJob 實作**
  - 建立 `DocumentProcessingJob.cs`
  - 步驟 1: Download from Blob Storage
  - 步驟 2: Parse document
  - 步驟 3: Chunk content
  - 步驟 4: Generate embeddings (batch)
  - 步驟 5: Store in Qdrant
  - 步驟 6: Update document status
  - 錯誤處理: Update status to Failed
  - 相關 Commit: `[待填寫]`

- [ ] **Job Trigger Integration**
  - 在 `UploadDocumentCommandHandler` 中觸發 Job
  - `BackgroundJob.Enqueue<DocumentProcessingJob>(x => x.ProcessAsync(documentId))`
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-035: Background Job 架構選擇**
  - 決策: Hangfire + PostgreSQL Storage
  - 理由: 可靠性高、持久化、Dashboard 友善
  - 替代方案: Azure Service Bus (成本高，過於複雜)
  - 參考: ADR-019 Background Job Architecture

- [ ] **TD-036: Job Retry 策略**
  - 決策: 最多重試 3 次，Exponential Backoff
  - 理由: 處理暫時性錯誤 (網路問題, API Rate Limit)
  - Delay: 1 min, 5 min, 15 min

#### 📊 測試結果
```
DocumentProcessingJob Unit Tests:
- ProcessAsync (Success): 1/1 Passed
- ProcessAsync (Parsing Fails): 1/1 Passed
- ProcessAsync (Embedding Fails): 1/1 Passed
- ProcessAsync (Qdrant Fails): 1/1 Passed
- Coverage: 92%

Integration Tests (E2E Pipeline):
- Process PDF Document: Passed (10 pages → 25 chunks)
- Process DOCX Document: Passed (5 pages → 12 chunks)
- Process TXT Document: Passed (1000 lines → 8 chunks)
- Verify Qdrant Indexing: Passed (all vectors stored)
```

#### 📝 備註
- [ ] Background Processing Pipeline 完整運作
- [ ] 平均處理時間: ~30秒/檔案 (10頁 PDF)

---

### 📅 2026-01-15 (Wednesday) - Document Management API 與 Frontend

#### ✅ 完成任務
- [ ] **DocumentsController 實作**
  - POST `/api/agents/{agentId}/documents` - Upload Document
  - GET `/api/agents/{agentId}/documents` - Get Documents by Agent (分頁)
  - GET `/api/documents/{documentId}` - Get Document by Id
  - DELETE `/api/documents/{documentId}` - Delete Document
  - GET `/api/documents/{documentId}/download` - Download Document (SAS URI redirect)
  - Swagger 文檔完整
  - 相關 Commit: `[待填寫]`

- [ ] **Frontend - DocumentUpload Component**
  - 建立 `DocumentUpload.tsx`
  - File Input (支援多檔案上傳)
  - 拖放上傳支援
  - 進度條 (上傳 + 處理狀態)
  - 檔案格式驗證 (client-side)
  - 相關 Commit: `[待填寫]`

- [ ] **Frontend - DocumentList Component**
  - 建立 `DocumentList.tsx`
  - 文檔列表展示 (表格格式)
  - 分頁、排序、篩選
  - 操作按鈕: 下載、刪除
  - 狀態顯示: Uploaded, Processing, Completed, Failed
  - 相關 Commit: `[待填寫]`

- [ ] **Frontend - DocumentService**
  - 建立 `documentService.ts`
  - `uploadDocuments(agentId, files)`
  - `getDocuments(agentId, pageNumber, pageSize)`
  - `deleteDocument(documentId)`
  - 使用 Axios HTTP client
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-037: 檔案上傳進度追蹤**
  - 決策: 使用 Axios Upload Progress Event
  - 理由: 提供即時上傳進度反饋
  - 實作: Progress Bar + Percentage Display

#### 📊 測試結果
```
E2E Tests (Document Upload):
- Upload Valid PDF: Passed
- Upload Invalid Format: Passed (Error handling)
- Upload File Too Large: Passed (Error handling)
- Download Document: Passed (SAS URI redirect)
- Delete Document: Passed
```

#### 📝 備註
- [ ] US 4.1 完成！Document Upload & Processing 功能完整
- [ ] 下一步: US 4.2 RAG Retrieval Strategies

---

### 📅 2026-01-16 (Thursday) - Semantic Search 實作

#### ✅ 完成任務
- [ ] **SemanticSearchQuery + Handler**
  - 建立 `SemanticSearchQuery.cs`
  - 建立 `SemanticSearchQueryHandler.cs`
    - 步驟 1: Generate query embedding
    - 步驟 2: Search in Qdrant
    - 步驟 3: Filter by ScoreThreshold (0.7)
    - 步驟 4: Return top-K results (10)
  - 相關 Commit: `[待填寫]`

- [ ] **SearchResultDto 設計**
  - Properties: ChunkId, DocumentId, Content, Score, FileName, Metadata
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-038: Semantic Search Score Threshold**
  - 決策: 預設 0.7 (可配置 0.7-0.9)
  - 理由: 平衡準確率與召回率
  - 測試: 不同 Threshold 對準確率的影響
  - 參考: ADR-017 RAG Retrieval Strategy

#### 📊 測試結果
```
Semantic Search Unit Tests:
- Handle Valid Query: 1/1 Passed
- Handle No Results: 1/1 Passed
- Score Threshold Filtering: 1/1 Passed
- TopK Limiting: 1/1 Passed
- Coverage: 90%

Integration Tests:
- Search Relevant Query: Passed (8/10 relevant results)
- Search Irrelevant Query: Passed (low scores)
- Cosine Similarity Accuracy: Passed
```

#### 📝 備註
- [ ] Semantic Search 實作完成
- [ ] 檢索準確率: 初步測試 ~85%

---

### 📅 2026-01-17 (Friday) - Keyword Search 實作

#### ✅ 完成任務
- [ ] **PostgreSQL FTS Extension 安裝**
  - 安裝 `pg_trgm` extension
  - 創建 Full-Text Search Index
    - Column: `content_tsv` (tsvector, generated column)
    - Index: `idx_document_chunks_content_fts` (GIN index)
  - 相關 Commit: `[待填寫]`

- [ ] **KeywordSearchQuery + Handler**
  - 建立 `KeywordSearchQuery.cs`
  - 建立 `KeywordSearchQueryHandler.cs`
    - 步驟 1: Build FTS Query (`to_tsquery`)
    - 步驟 2: Execute FTS Query
    - 步驟 3: Calculate BM25 Score (`ts_rank_cd`)
    - 步驟 4: Order by Score DESC
    - 步驟 5: Return top-K results
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-039: BM25 參數調校**
  - 決策: 使用 PostgreSQL `ts_rank_cd` 預設參數
  - 理由: PostgreSQL 內建 BM25 實作穩定
  - 未來優化: 考慮自訂 BM25 參數 (k1, b)

#### 📊 測試結果
```
Keyword Search Unit Tests:
- Handle Valid Query: 1/1 Passed
- Multiple Keywords (OR): 1/1 Passed
- No Matches: 1/1 Passed
- TopK Limiting: 1/1 Passed
- Coverage: 88%

Integration Tests:
- Search Exact Keyword: Passed (10/10 matches)
- Search Partial Keyword: Passed (7/10 matches)
- Multiple Keywords: Passed (combined results)
- BM25 Score Accuracy: Passed
```

#### 📝 備註
- [ ] Week 14 目標達成：US 4.1 完成 + US 4.2 Phase 1-2 完成
- [ ] 下週重點：Hybrid Search 與 Re-ranking + Evaluation

---

## Week 15 開發日誌

> **Week 15 目標**: US 4.2 Hybrid Search + Re-ranking + Evaluation + Sprint 回顧
> **預期完成**: US 4.2 Phase 3-4 + Sprint Review (2.5 SP, 100%)

### 📅 2026-01-20 (Monday) - Hybrid Search (RRF Fusion)

#### ✅ 完成任務
- [ ] **RRFFusionService 實作**
  - 建立 `RRFFusionService.cs`
  - 實作 RRF 演算法: `score = Σ(weight / (k + rank))`
  - 參數: k = 60 (常數)
  - 輸入: 兩個排序列表 (Semantic, Keyword)
  - 輸出: 融合後的排序列表
  - 相關 Commit: `[待填寫]`

- [ ] **HybridSearchQuery + Handler**
  - 建立 `HybridSearchQuery.cs`
  - 建立 `HybridSearchQueryHandler.cs`
    - 步驟 1: 執行 Semantic Search (並行)
    - 步驟 2: 執行 Keyword Search (並行)
    - 步驟 3: 使用 RRF 融合結果
    - 步驟 4: 返回 top-K 結果
  - Properties: SemanticWeight (0.7), KeywordWeight (0.3)
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-040: RRF Fusion 權重配置**
  - 決策: Semantic 70%, Keyword 30% (預設)
  - 理由: Semantic Search 準確率較高
  - 測試: 不同權重對準確率的影響
  - 可配置: 允許使用者調整權重
  - 參考: ADR-017 RAG Retrieval Strategy

#### 📊 測試結果
```
RRF Fusion Unit Tests:
- Fuse Two Lists: 1/1 Passed
- Different Weights: 1/1 Passed
- Empty List Handling: 1/1 Passed
- RRF Formula Verification: 1/1 Passed
- Coverage: 93%

Hybrid Search Unit Tests:
- Handle Valid Query: 1/1 Passed
- Semantic Weight Only: 1/1 Passed
- Keyword Weight Only: 1/1 Passed
- Coverage: 91%
```

#### 📝 備註
- [ ] RRF Fusion 實作完成
- [ ] Hybrid Search 基本功能完成

---

### 📅 2026-01-21 (Tuesday) - Cross-Encoder Re-ranking

#### ✅ 完成任務
- [ ] **HuggingFaceCrossEncoderService 實作**
  - 建立 `HuggingFaceCrossEncoderService.cs`
  - 使用 HuggingFace Inference API
  - Model: `cross-encoder/ms-marco-MiniLM-L-6-v2`
  - API Endpoint: `https://api-inference.huggingface.co/models/{model}`
  - 實作 Batch Re-ranking (最多 10 candidates/批次)
  - 實作 Retry 邏輯 (最多 3 次)
  - 相關 Commit: `[待填寫]`

- [ ] **HybridSearchQueryHandler 整合 Re-ranking**
  - 步驟 4: (Optional) 執行 Cross-Encoder Re-ranking
  - 步驟 5: 返回 re-ranked top-K 結果
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-041: Cross-Encoder Model 選擇**
  - 決策: `cross-encoder/ms-marco-MiniLM-L-6-v2`
  - 理由: 輕量級、速度快、準確率高
  - 效能: Re-ranking 延遲 <200ms (10 candidates)
  - 替代方案: `cross-encoder/ms-marco-MiniLM-L-12-v2` (更準確但更慢)

#### ⚠️ 遇到問題
- [ ] **問題**: HuggingFace API Cold Start 慢
  - 影響: 首次呼叫延遲 >5秒
  - 解決方案: 實作 Warmup Request (啟動時)
  - 後續: 考慮自建 Model Inference Service

#### 📊 測試結果
```
Cross-Encoder Service Unit Tests:
- RerankAsync Valid Candidates: 1/1 Passed
- Retry on API Error: 1/1 Passed
- All Retries Fail: 1/1 Passed
- Coverage: 87%

Integration Tests (Hybrid Search with Re-ranking):
- Search Hybrid Query: Passed
- With Re-ranking Improves Accuracy: Passed (85% → 92%)
- Different Weights Affect Ranking: Passed
```

#### 📝 備註
- [ ] Cross-Encoder Re-ranking 實作完成
- [ ] Re-ranking 顯著提升準確率 (+7%)

---

### 📅 2026-01-22 (Wednesday) - RAG Accuracy Evaluation Framework

#### ✅ 完成任務
- [ ] **Evaluation Dataset 建立**
  - 建立 `evaluation-dataset.json`
  - 50 個查詢 (Factual, Conceptual, Multi-hop)
  - 每個查詢標註相關 Chunks
  - 相關 Commit: `[待填寫]`

- [ ] **Evaluation Metrics 實作**
  - 建立 `AccuracyMetric.cs`
    - Formula: `Accuracy = CorrectRetrievals / TotalQueries`
    - Threshold: ≥90%
  - 建立 `MRRMetric.cs`
    - Formula: `MRR = (1/n) * Σ(1/rank_i)`
    - Threshold: ≥0.8
  - 建立 `NDCGMetric.cs`
    - Formula: `NDCG = DCG / IDCG`
    - Threshold: ≥0.85
  - 相關 Commit: `[待填寫]`

- [ ] **RAGEvaluationRunner 實作**
  - 建立 `RAGEvaluationRunner.cs`
  - 載入 Evaluation Dataset
  - 對每個 query 執行 Hybrid Search
  - 計算 Accuracy, MRR, NDCG
  - 生成評估報告 (JSON + Markdown)
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-042: Evaluation Dataset 設計**
  - 決策: 50 個查詢，涵蓋 3 種類型
  - 理由: 平衡評估成本與準確性
  - 未來擴展: 可增加至 100+ 查詢

#### 📊 測試結果
```
RAG Accuracy Evaluation Results:

Semantic Search:
- Accuracy: 86.0% (Target: ≥90%)
- MRR: 0.82 (Target: ≥0.8) ✅
- NDCG: 0.84 (Target: ≥0.85)

Keyword Search:
- Accuracy: 78.0% (Target: ≥90%)
- MRR: 0.75 (Target: ≥0.8)
- NDCG: 0.77 (Target: ≥0.85)

Hybrid Search (RRF 70/30):
- Accuracy: 91.0% (Target: ≥90%) ✅
- MRR: 0.85 (Target: ≥0.8) ✅
- NDCG: 0.87 (Target: ≥0.85) ✅

Hybrid Search with Re-ranking:
- Accuracy: 94.0% (Target: ≥90%) ✅✅
- MRR: 0.89 (Target: ≥0.8) ✅✅
- NDCG: 0.91 (Target: ≥0.85) ✅✅

✅ All Targets Met with Hybrid Search + Re-ranking!
```

#### 📝 備註
- [ ] RAG Evaluation Framework 完成
- [ ] Hybrid Search with Re-ranking 達標！
- [ ] Accuracy: 94% (超越 90% 目標)

---

### 📅 2026-01-23 (Thursday) - Performance Testing 與優化

#### ✅ 完成任務
- [ ] **Document Processing Performance Test**
  - Upload 100 documents (各 1MB)
    - 完成時間: 4 分 32 秒 (Target: <5分鐘) ✅
  - Parse 100 PDFs
    - 平均解析時間: 2.8 秒/檔案 (Target: <3秒) ✅
  - Chunk 1000 pages
    - 平均分塊時間: 0.7 秒/頁面 (Target: <1秒) ✅

- [ ] **RAG Retrieval Performance Test**
  - Semantic Search: 182ms (Target: <200ms) ✅
  - Keyword Search: 87ms (Target: <100ms) ✅
  - Hybrid Search: 276ms (Target: <300ms) ✅
  - Hybrid with Re-ranking: 458ms (Target: <500ms) ✅

- [ ] **Performance 優化**
  - Qdrant Index 優化 (ef_construct 提升至 200)
  - Embedding Cache Hit Rate 提升至 42%
  - PostgreSQL FTS Index 優化
  - 相關 Commit: `[待填寫]`

#### 🔧 技術決策
- [ ] **TD-043: Performance 優化策略**
  - 決策: 優先優化 Qdrant HNSW Index
  - 理由: Semantic Search 是 Bottleneck
  - 效果: 延遲降低 18% (220ms → 182ms)

#### 📊 測試結果
```
Performance Test Summary:

Document Processing:
- Upload 100 docs: 4m 32s ✅
- Parse 100 PDFs: 2.8s/file ✅
- Chunk 1000 pages: 0.7s/page ✅

RAG Retrieval:
- Semantic Search: 182ms ✅
- Keyword Search: 87ms ✅
- Hybrid Search: 276ms ✅
- Hybrid + Re-ranking: 458ms ✅

✅ All Performance Targets Met!
```

#### 📝 備註
- [ ] Performance 測試通過
- [ ] 所有指標達標

---

### 📅 2026-01-24 (Friday) - Documentation 與 Sprint Review

#### ✅ 完成任務
- [ ] **ADR 文檔完成**
  - ADR-015: Document Storage Strategy ✅
  - ADR-016: Chunking Strategy ✅
  - ADR-017: RAG Retrieval Strategy ✅
  - ADR-018: Embedding Service Design ✅
  - ADR-019: Background Job Architecture ✅

- [ ] **API Documentation 完成**
  - Swagger 文檔完整 ✅
  - API Design Document 完成 ✅
  - Request/Response 範例完整 ✅

- [ ] **Technical Implementation Guides 完成**
  - Document Processing Guide ✅
  - RAG Retrieval Guide ✅

- [ ] **Sprint Review 會議**
  - 時間: 14:00-16:00
  - 參與者: 全團隊 + Stakeholders
  - Demo: Document Upload & RAG Search
  - 成果: Stakeholders 滿意，提供正面反饋
  - 後續: US 4.3 (Multi-turn Conversation) 排入 Sprint 6

#### 📝 備註
- [ ] Sprint 5 成功完成！
- [ ] Definition of Done 全部達成 ✅
- [ ] 下週: Sprint Retrospective

---

### 📅 2026-01-27 (Monday) - Sprint Retrospective

#### Sprint 5 回顧總結

**✅ 完成成果**:
- US 4.1: Document Upload & Processing (8 SP) ✅
- US 4.2: RAG Retrieval Strategies (5 SP) ✅
- 總計: 13 SP 全部完成

**📊 品質指標**:
- 單元測試覆蓋率: 89% (Target: ≥85%) ✅
- 整合測試通過率: 100% ✅
- RAG Accuracy: 94% (Target: ≥90%) ✅
- Performance 測試: 全部達標 ✅
- Code Review: 0 Critical/High Issues ✅

**🎯 Went Well**:
- Background Processing Pipeline 設計良好，穩定可靠
- RRF Fusion + Cross-Encoder Re-ranking 顯著提升準確率
- Evaluation Framework 設計完善，有效驗證 RAG 準確率
- Team 協作順暢，溝通良好

**⚠️ Needs Improvement**:
- Azure OpenAI API Rate Limit 需要提前規劃
- HuggingFace API Cold Start 問題需要解決 (考慮自建 Service)
- Evaluation Dataset 需要持續擴充 (目前 50 queries，目標 100+)

**🚀 Action Items (Sprint 6)**:
- 申請提高 Azure OpenAI Rate Limit
- 研究自建 Cross-Encoder Inference Service
- 擴充 Evaluation Dataset 至 100+ queries
- 實作 Multi-turn Conversation (US 4.3)

---

## 📋 技術決策記錄

### TD-024: Azure Blob Storage Tier 策略
- **日期**: 2026-01-06
- **決策**: 使用 Hot Tier + Lifecycle Management (30 天後轉 Cool Tier)
- **理由**: 新上傳文檔頻繁訪問，舊文檔訪問頻率低
- **影響**: 預估節省 50% 儲存成本
- **參考**: ADR-015 Document Storage Strategy

### TD-025: Qdrant Deployment 策略
- **日期**: 2026-01-06
- **決策**: 使用 Docker 部署 (Development), Qdrant Cloud (Production)
- **理由**: 開發環境靈活，生產環境穩定
- **影響**: 開發效率提升，生產穩定性保證

### TD-026: Document 檔案大小限制
- **日期**: 2026-01-07
- **決策**: 10MB 上限
- **理由**: 平衡處理速度與使用需求
- **影響**: 超大檔案需要分段上傳（未來功能）

### TD-027: Blob SAS Token 有效期
- **日期**: 2026-01-07
- **決策**: 1 小時有效期
- **理由**: 安全性考量，避免長期暴露
- **影響**: 使用者需要在 1 小時內完成下載

### TD-028: Document 唯一性約束
- **日期**: 2026-01-08
- **決策**: `agent_id + file_name` 唯一約束 (僅未刪除記錄)
- **理由**: 避免重複上傳相同檔案
- **影響**: Partial Unique Index 實作

### TD-029: PDF Parser 選擇
- **日期**: 2026-01-09
- **決策**: 使用 PdfPig 而非 iTextSharp
- **理由**: 開源、MIT License、社群活躍
- **影響**: 解析速度 <3秒/檔案

### TD-030: DOCX Parser 選擇
- **日期**: 2026-01-09
- **決策**: 使用 DocumentFormat.OpenXml (官方套件)
- **理由**: Microsoft 官方支援、穩定可靠
- **影響**: 僅提取純文字，不保留格式

### TD-031: Chunking 策略選擇
- **日期**: 2026-01-10
- **決策**: Fixed-Size Chunking with Overlap
- **理由**: 簡單可靠、可控性高、效能穩定
- **影響**: Chunk Size 1024 tokens (預設)
- **參考**: ADR-016 Chunking Strategy

### TD-032: 預設 Chunk Size
- **日期**: 2026-01-10
- **決策**: 1024 tokens (預設)
- **理由**: 平衡檢索準確率與上下文完整性
- **影響**: 範圍 512-2048 tokens (可配置)

### TD-033: Embedding Cache 策略
- **日期**: 2026-01-13
- **決策**: Memory Cache with 1 小時 TTL
- **理由**: 減少 API 呼叫、降低成本、提升速度
- **影響**: Cache Hit Rate 42%, 成本節省 35%
- **參考**: ADR-018 Embedding Service Design

### TD-034: HNSW 參數調校
- **日期**: 2026-01-13
- **決策**: m=16, ef_construct=100, ef=128
- **理由**: 平衡檢索速度與準確率
- **影響**: Recall@10 ≥95%

### TD-035: Background Job 架構選擇
- **日期**: 2026-01-14
- **決策**: Hangfire + PostgreSQL Storage
- **理由**: 可靠性高、持久化、Dashboard 友善
- **影響**: 平均處理時間 ~30秒/檔案
- **參考**: ADR-019 Background Job Architecture

### TD-036: Job Retry 策略
- **日期**: 2026-01-14
- **決策**: 最多重試 3 次，Exponential Backoff
- **理由**: 處理暫時性錯誤 (網路問題, API Rate Limit)
- **影響**: Delay 1min, 5min, 15min

### TD-037: 檔案上傳進度追蹤
- **日期**: 2026-01-15
- **決策**: 使用 Axios Upload Progress Event
- **理由**: 提供即時上傳進度反饋
- **影響**: 使用者體驗提升

### TD-038: Semantic Search Score Threshold
- **日期**: 2026-01-16
- **決策**: 預設 0.7 (可配置 0.7-0.9)
- **理由**: 平衡準確率與召回率
- **影響**: 檢索準確率 ~85%
- **參考**: ADR-017 RAG Retrieval Strategy

### TD-039: BM25 參數調校
- **日期**: 2026-01-17
- **決策**: 使用 PostgreSQL `ts_rank_cd` 預設參數
- **理由**: PostgreSQL 內建 BM25 實作穩定
- **影響**: Keyword Search 準確率 ~78%

### TD-040: RRF Fusion 權重配置
- **日期**: 2026-01-20
- **決策**: Semantic 70%, Keyword 30% (預設)
- **理由**: Semantic Search 準確率較高
- **影響**: Hybrid Search 準確率 91%
- **參考**: ADR-017 RAG Retrieval Strategy

### TD-041: Cross-Encoder Model 選擇
- **日期**: 2026-01-21
- **決策**: `cross-encoder/ms-marco-MiniLM-L-6-v2`
- **理由**: 輕量級、速度快、準確率高
- **影響**: Re-ranking 延遲 <200ms, 準確率提升至 94%

### TD-042: Evaluation Dataset 設計
- **日期**: 2026-01-22
- **決策**: 50 個查詢，涵蓋 3 種類型
- **理由**: 平衡評估成本與準確性
- **影響**: 未來可擴展至 100+ 查詢

### TD-043: Performance 優化策略
- **日期**: 2026-01-23
- **決策**: 優先優化 Qdrant HNSW Index
- **理由**: Semantic Search 是 Bottleneck
- **影響**: 延遲降低 18% (220ms → 182ms)

---

## ⚠️ 問題與解決方案

### Issue #1: Azure Blob Storage SDK 版本衝突
- **日期**: 2026-01-07
- **問題**: 編譯失敗，NuGet Package 版本衝突
- **影響**: 開發進度延遲 1 小時
- **解決方案**: 統一使用 `Azure.Storage.Blobs` v12.x
- **參考**: [Azure SDK Migration Guide](https://learn.microsoft.com/en-us/dotnet/azure/sdk/azure-sdk-for-dotnet)
- **狀態**: ✅ 已解決

### Issue #2: PdfPig 無法解析某些 PDF 檔案
- **日期**: 2026-01-09
- **問題**: PDF 格式損壞或加密導致解析失敗
- **影響**: 部分 PDF 檔案無法處理
- **解決方案**: 實作 try-catch，返回友善錯誤訊息
- **後續**: 考慮使用 Tesseract OCR 處理掃描 PDF (Phase 2)
- **狀態**: ✅ 已解決 (Workaround)

### Issue #3: Azure OpenAI API Rate Limit
- **日期**: 2026-01-13
- **問題**: Batch embedding 處理速度慢，達到 Rate Limit
- **影響**: 處理延遲增加，使用者體驗下降
- **解決方案**: 實作 Retry with Exponential Backoff (最大重試 3 次)
- **後續**: 申請提高 Rate Limit
- **狀態**: ⚠️ Workaround 完成，待提高 Rate Limit

### Issue #4: HuggingFace API Cold Start 慢
- **日期**: 2026-01-21
- **問題**: 首次呼叫延遲 >5秒
- **影響**: 首次 Re-ranking 體驗差
- **解決方案**: 實作 Warmup Request (啟動時)
- **後續**: 考慮自建 Model Inference Service
- **狀態**: ⚠️ Workaround 完成，待自建 Service

---

## 💡 經驗總結

### 技術亮點
1. **Background Processing Pipeline 設計**
   - Hangfire + PostgreSQL Storage 穩定可靠
   - 6-step Pipeline 清晰易維護
   - 平均處理時間 ~30秒/檔案

2. **RRF Fusion + Cross-Encoder Re-ranking**
   - RRF Fusion 提升 Hybrid Search 準確率至 91%
   - Cross-Encoder Re-ranking 進一步提升至 94%
   - 演算法簡單高效，易於調整權重

3. **Evaluation Framework 設計**
   - 3 種評估指標 (Accuracy, MRR, NDCG) 全面評估
   - Evaluation Dataset 設計合理，涵蓋多種查詢類型
   - 自動化評估，快速驗證 RAG 準確率

### 最佳實踐
1. **Domain-Driven Design**
   - Document, DocumentChunk Entities 設計清晰
   - Value Objects (FileMetadata, BlobReference) 封裝良好
   - Repository Pattern 實作完整

2. **CQRS Pattern**
   - Commands 與 Queries 分離清晰
   - MediatR Handler 實作簡潔
   - FluentValidation 驗證規則完整

3. **測試策略**
   - 單元測試覆蓋率 89% (Target: ≥85%)
   - 整合測試涵蓋所有 E2E Scenarios
   - Performance 測試驗證所有指標

### 待改進項目
1. **API Rate Limit 管理**
   - 需要更好的 Rate Limit 監控
   - 考慮實作 Rate Limiter Middleware
   - 提前規劃 API Quota

2. **Cold Start 優化**
   - HuggingFace API Cold Start 需要優化
   - 考慮自建 Model Inference Service
   - 實作 Model Warmup 策略

3. **Evaluation Dataset 擴充**
   - 目前 50 queries，需要擴充至 100+
   - 涵蓋更多查詢類型 (Comparison, Aggregation, etc.)
   - 持續優化 Relevant Chunks 標註

---

## 📚 參考文檔

### Sprint 5 核心文檔
1. [SPRINT-5-1-OVERVIEW.md](./SPRINT-5-1-OVERVIEW.md) - Sprint 5 概覽
2. [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) - Sprint 5 實施計劃
3. [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) - Sprint 5 技術上下文
4. [SPRINT-5-4-CHECKLIST.md](./SPRINT-5-4-CHECKLIST.md) - Sprint 5 任務清單
5. [SPRINT-5-6-ISSUES.md](./SPRINT-5-6-ISSUES.md) - Sprint 5 問題追蹤
6. [SPRINT-5-7-RETROSPECTIVE.md](./SPRINT-5-7-RETROSPECTIVE.md) - Sprint 5 回顧

### User Story 規格
7. [module-05-knowledge-management.md](../../../docs/user-stories/modules/module-05-knowledge-management.md) - US 4.1, 4.2 規格

### Architecture Design Documents
8. [ADR-015-document-storage-strategy.md](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md)
9. [ADR-016-chunking-strategy.md](../../../docs/architecture/adr/ADR-016-chunking-strategy.md)
10. [ADR-017-rag-retrieval-strategy.md](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md)
11. [ADR-018-embedding-service-design.md](../../../docs/architecture/adr/ADR-018-embedding-service-design.md)
12. [ADR-019-background-job-architecture.md](../../../docs/architecture/adr/ADR-019-background-job-architecture.md)

### API & Database Design
13. [knowledge-management-api.md](../../../docs/api/knowledge-management-api.md) - API 設計文檔
14. [database-schema.md](../../../docs/database/database-schema.md) - 資料庫 Schema

### Testing Documentation
15. [unit-testing-guidelines.md](../../../docs/testing/unit-testing-guidelines.md)
16. [integration-testing-strategy.md](../../../docs/testing/integration-testing-strategy.md)
17. [rag-evaluation-report-template.md](../../../docs/testing/rag-evaluation-report-template.md)

### External Resources
18. [Azure Blob Storage Documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/)
19. [Qdrant Documentation](https://qdrant.tech/documentation/)
20. [Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/)
21. [Hangfire Documentation](https://docs.hangfire.io/)
22. [PdfPig GitHub](https://github.com/UglyToad/PdfPig)
23. [DocumentFormat.OpenXml](https://learn.microsoft.com/en-us/office/open-xml/open-xml-sdk)

---

## 📝 版本歷史

| 版本 | 日期 | 作者 | 變更說明 |
|------|------|------|----------|
| v2.1 | 2025-11-14 | AI Assistant | 初始版本建立，Sprint 5 開發日誌模板 |
|      |            |              | - 包含 Week 13-15 日誌模板 |
|      |            |              | - 包含技術決策記錄 (TD-024 至 TD-043) |
|      |            |              | - 包含問題與解決方案 (4 個 Issues) |
|      |            |              | - 包含經驗總結 (技術亮點、最佳實踐、待改進項目) |
|      |            |              | - 遵循 v2.1 標準 (8 欄位 header, 完整 ToC, 參考索引) |
| v2.2 | 2026-01-06 | [Name] | Sprint 5 執行開始，Week 13 日誌更新 |
| v2.3 | 2026-01-13 | [Name] | Week 14 日誌更新 |
| v2.4 | 2026-01-20 | [Name] | Week 15 日誌更新 |
| v2.5 | 2026-01-27 | [Name] | Sprint Retrospective 完成 |

---

**文件結束** - Sprint 5 開發日誌已完整建立 ✅

**注意**: 本文件為模板，將在 Sprint 5 執行期間每日更新。請開發團隊於每日站會後更新相應日期的任務完成狀態、技術決策、遇到的問題等資訊。
