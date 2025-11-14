# SPRINT-5-3-CONTEXT.md - Sprint 5 開發上下文：Knowledge 管理與 RAG 檢索系統

**版本**: v2.1
**Sprint 編號**: Sprint 5
**Sprint 週期**: Week 13-15 (3 週)
**Phase**: Phase 1A - 基礎平台 (Foundation Platform)
**計劃日期**: 2026-01-06 ~ 2026-01-26
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-13
**最後更新**: 2025-11-13

---

## 📑 目錄 (Table of Contents)

- [使用說明](#-使用說明-how-to-use-this-document)
- [一、核心技術參考層 (Reference Layer)](#一核心技術參考層-reference-layer)
  - [1.1 Module & Epic 引用](#11-module--epic-引用)
  - [1.2 架構設計文檔](#12-架構設計文檔)
  - [1.3 技術決策記錄 (ADR)](#13-技術決策記錄-adr)
  - [1.4 API 設計引用](#14-api-設計引用)
  - [1.5 資料庫設計引用](#15-資料庫設計引用)
- [二、US 4.1: 文檔上傳與處理 - 詳細技術上下文](#二us-41-文檔上傳與處理---詳細技術上下文)
  - [2.1 MVP 範圍定義與技術決策](#21-mvp-範圍定義與技術決策)
  - [2.2 文檔解析引擎架構](#22-文檔解析引擎架構)
  - [2.3 文檔分塊策略設計](#23-文檔分塊策略設計)
  - [2.4 向量化服務設計](#24-向量化服務設計)
  - [2.5 Qdrant 向量儲存架構](#25-qdrant-向量儲存架構)
  - [2.6 背景處理任務設計](#26-背景處理任務設計)
- [三、US 4.2: RAG 檢索策略配置 - 詳細技術上下文](#三us-42-rag-檢索策略配置---詳細技術上下文)
  - [3.1 語義搜尋實作細節](#31-語義搜尋實作細節)
  - [3.2 關鍵字搜尋 (BM25) 實作](#32-關鍵字搜尋-bm25-實作)
  - [3.3 混合檢索 (RRF) 融合策略](#33-混合檢索-rrf-融合策略)
  - [3.4 Cross-Encoder Re-ranking 機制](#34-cross-encoder-re-ranking-機制)
  - [3.5 RAG 準確率評估框架](#35-rag-準確率評估框架)
- [四、完整文檔引用清單 (按功能分類)](#四完整文檔引用清單-按功能分類)
- [使用指南](#-使用指南)
- [更新日誌](#-更新日誌)

---

## 📖 使用說明 (How to Use This Document)

**本文檔定位**: "Just-in-Time" 技術參考手冊

**適用場景**:
- ✅ 開發 US 4.1 時，需要查詢文檔解析器設計 → 第二章
- ✅ 開發 US 4.2 時，需要查詢 RAG 檢索策略 → 第三章
- ✅ 需要快速找到 Qdrant 配置參數 → 2.5 章節
- ✅ 需要理解 RRF 融合演算法 → 3.3 章節
- ✅ 需要快速找到架構設計文檔 → 第一章 Reference Layer
- ✅ 需要理解技術決策背景 → 各章節的「關鍵技術決策」部分

**不適用場景**:
- ❌ 尋找 Sprint 整體進度與成果 → 使用 [SPRINT-5-1-OVERVIEW.md](./SPRINT-5-1-OVERVIEW.md)
- ❌ 尋找詳細開發計劃與任務清單 → 使用 [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md)
- ❌ 追蹤開發進度與 Checklist → 使用 [SPRINT-5-4-CHECKLIST.md](./SPRINT-5-4-CHECKLIST.md)

---

## 一、核心技術參考層 (Reference Layer)

### 1.1 Module & Epic 引用

**User Story 完整規格**:
- 📖 [Module 05: Agent Memory & Knowledge Management](../../../docs/user-stories/modules/module-05-agent-memory.md) - Knowledge 系統完整規格
  - **Section**: [US 5.1 - 知識庫文件上傳與處理](../../../docs/user-stories/modules/module-05-agent-memory.md#us-51) - 文檔處理驗收標準
  - **Section**: [US 5.2 - 精確檢索策略配置](../../../docs/user-stories/modules/module-05-agent-memory.md#us-52) - RAG 檢索規格
  - **關鍵內容**: PDF/DOCX/TXT/MD 解析、Chunking 策略、Semantic/Keyword/Hybrid Search、90%+ 準確率目標

**Epic 文檔**:
- 📖 [Epic 05: Knowledge Management & RAG](../../../docs/user-stories/epics/epic-05.md) - Knowledge 功能完整策略
  - **用途**: 理解 Knowledge Management 在整體系統中的定位
  - **關鍵內容**: BDD 場景、驗收標準、Phase 1A → Phase 2 演進路徑
  - **Phase 1A 範圍**: 基礎文檔上傳、向量化、Hybrid Search（本 Sprint）
  - **Phase 2 延後**: OCR、多語言、Query Expansion、Parent Document Retriever

### 1.2 架構設計文檔

**核心架構決策記錄 (ADR)**:

**1. Document Storage Strategy** - [ADR-015](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md)
- **決策**: Azure Blob Storage（原始文件）+ PostgreSQL（元數據）+ Qdrant（向量）
- **關鍵原則**: 分層儲存策略，原始文件與向量索引分離，支援未來的 Azure AI Search 遷移
- **Section**: [Blob Storage Configuration](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md#blob-storage-configuration) - Container 結構與 SAS Token 策略
- **Section**: [Metadata in PostgreSQL](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md#metadata-in-postgresql) - Document & DocumentChunk 表設計
- **Section**: [Vector Store Selection](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md#vector-store-selection) - Qdrant vs Pinecone vs Weaviate 比較
- **實施影響**: US 4.1 的儲存架構基於此決策，三層儲存設計確保靈活性與效能

**2. Chunking Strategy** - [ADR-016](../../../docs/architecture/adr/ADR-016-chunking-strategy.md)
- **決策**: Fixed-Size Chunking 為主（512-2048 tokens），支援可配置 Overlap（0-200 tokens）
- **關鍵設計**: Token-based 切分（不是字元或單詞），保留語義邊界（句子、段落）
- **Section**: [Chunk Size Optimization](../../../docs/architecture/adr/ADR-016-chunking-strategy.md#chunk-size-optimization) - 512/1024/2048 tokens 效能比較
- **Section**: [Overlap Strategy](../../../docs/architecture/adr/ADR-016-chunking-strategy.md#overlap-strategy) - Overlap 對檢索準確率的影響分析
- **Section**: [Future: Semantic Chunking](../../../docs/architecture/adr/ADR-016-chunking-strategy.md#future-semantic-chunking) - Phase 2 進階分塊策略
- **實施影響**: US 4.1 的 FixedSizeChunker 實作基於此架構，延後 Semantic Chunking 到 Phase 2

**3. RAG Retrieval Strategy** - [ADR-017](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md)
- **決策**: Hybrid Search (Semantic 70% + Keyword 30%) + Cross-Encoder Re-ranking
- **關鍵設計**: RRF (Reciprocal Rank Fusion) 融合演算法，Cross-Encoder 精確重排序
- **Section**: [Semantic Search Design](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md#semantic-search-design) - Qdrant Vector Search 配置
- **Section**: [Keyword Search Design](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md#keyword-search-design) - PostgreSQL FTS + BM25 演算法
- **Section**: [RRF Fusion Algorithm](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md#rrf-fusion-algorithm) - RRF 公式與權重配置
- **Section**: [Re-ranking Strategy](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md#re-ranking-strategy) - Cross-Encoder 模型選擇
- **實施影響**: US 4.2 的 HybridSearchService 實作基於此架構，目標準確率 ≥90%

**4. Embedding Service Design** - [ADR-018](../../../docs/architecture/adr/ADR-018-embedding-service-design.md)
- **決策**: Azure OpenAI text-embedding-ada-002（1536 維度）+ Memory Cache（1 小時）
- **關鍵設計**: Batch Embedding（提升效率），Hash-based 快取（避免重複計算）
- **Section**: [Model Selection](../../../docs/architecture/adr/ADR-018-embedding-service-design.md#model-selection) - ada-002 vs ada-003 vs Multilingual 比較
- **Section**: [Caching Strategy](../../../docs/architecture/adr/ADR-018-embedding-service-design.md#caching-strategy) - SHA256 Hash 快取機制
- **Section**: [Batch Processing](../../../docs/architecture/adr/ADR-018-embedding-service-design.md#batch-processing) - 批次處理優化（100 chunks/batch）
- **實施影響**: US 4.1 的 OpenAIEmbeddingService 實作基於此設計，支援 Phase 2 多語言模型切換

**5. Background Job Architecture** - [ADR-019](../../../docs/architecture/adr/ADR-019-background-job-architecture.md)
- **決策**: Hangfire（持久化背景任務）+ PostgreSQL Storage
- **關鍵設計**: DocumentProcessingJob（Upload → Parse → Chunk → Embed → Index），支援重試與進度追蹤
- **Section**: [Job Design](../../../docs/architecture/adr/ADR-019-background-job-architecture.md#job-design) - 文檔處理 Pipeline 設計
- **Section**: [Error Handling](../../../docs/architecture/adr/ADR-019-background-job-architecture.md#error-handling) - 失敗重試機制（最多 3 次）
- **Section**: [Progress Tracking](../../../docs/architecture/adr/ADR-019-background-job-architecture.md#progress-tracking) - 實時進度更新策略
- **實施影響**: US 4.1 的背景處理任務基於 Hangfire 實施

**完整架構設計文檔**:
- 🏗️ [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 系統架構總覽
  - **Section**: [Knowledge Management Architecture](../../../docs/architecture/Architecture-Design-Document.md#knowledge-management-architecture) - Knowledge 系統架構圖
  - **Section**: [RAG Pipeline](../../../docs/architecture/Architecture-Design-Document.md#rag-pipeline) - 完整 RAG 檢索流程
  - **Section**: [Vector Store Integration](../../../docs/architecture/Architecture-Design-Document.md#vector-store-integration) - Qdrant 整合架構
  - **Section**: [Document Processing Pipeline](../../../docs/architecture/Architecture-Design-Document.md#document-processing-pipeline) - 文檔處理流程圖

### 1.3 技術決策記錄 (ADR)

**Knowledge Management 相關 ADR 完整清單**:

| ADR 編號 | 標題 | 關鍵決策 | 實施影響 |
|---------|------|---------|---------|
| [ADR-015](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md) | Document Storage Strategy | Azure Blob + PostgreSQL + Qdrant 三層儲存 | US 4.1 儲存架構 |
| [ADR-016](../../../docs/architecture/adr/ADR-016-chunking-strategy.md) | Chunking Strategy | Fixed-Size Chunking (512-2048 tokens) | US 4.1 分塊實作 |
| [ADR-017](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md) | RAG Retrieval Strategy | Hybrid Search + RRF + Cross-Encoder | US 4.2 檢索策略 |
| [ADR-018](../../../docs/architecture/adr/ADR-018-embedding-service-design.md) | Embedding Service Design | Azure OpenAI ada-002 + Cache | US 4.1 向量化服務 |
| [ADR-019](../../../docs/architecture/adr/ADR-019-background-job-architecture.md) | Background Job Architecture | Hangfire + PostgreSQL Storage | US 4.1 背景任務 |
| [ADR-001](../../../docs/architecture/adr/ADR-001-clean-architecture.md) | Clean Architecture | Domain → Application → Infrastructure → API | 整體分層架構 |
| [ADR-002](../../../docs/architecture/adr/ADR-002-cqrs-pattern.md) | CQRS Pattern | MediatR Commands/Queries 分離 | US 4.1-4.2 業務邏輯 |
| [ADR-003](../../../docs/architecture/adr/ADR-003-repository-pattern.md) | Repository Pattern | IDocumentRepository 抽象 | US 4.1 資料存取 |

### 1.4 API 設計引用

**Knowledge API 設計規範**:
- 📄 [Knowledge API Design](../../../docs/api/knowledge-api-design.md) - Knowledge Management RESTful API 完整設計
  - **Section**: [Document Upload Endpoints](../../../docs/api/knowledge-api-design.md#document-upload-endpoints) - 文檔上傳 API 詳細設計
    - `POST /api/v1/documents/upload` - 批次文檔上傳（最多 10 檔案，50MB/檔案）(US 4.1)
    - `GET /api/v1/documents/{id}` - 獲取文檔詳情與處理狀態 (US 4.1)
    - `GET /api/v1/documents` - 查詢文檔列表（分頁、篩選）(US 4.1)
    - `DELETE /api/v1/documents/{id}` - 刪除文檔（軟刪除）(US 4.1)
    - `GET /api/v1/documents/{id}/chunks` - 獲取文檔的所有 Chunks (US 4.1)

  - **Section**: [RAG Search Endpoints](../../../docs/api/knowledge-api-design.md#rag-search-endpoints) - RAG 檢索 API 設計
    - `POST /api/v1/agents/{agentId}/search` - Hybrid Search（語義 + 關鍵字）(US 4.2)
    - `POST /api/v1/agents/{agentId}/search/semantic` - 純語義搜尋 (US 4.2)
    - `POST /api/v1/agents/{agentId}/search/keyword` - 純關鍵字搜尋 (US 4.2)
    - `POST /api/v1/agents/{agentId}/search/evaluate` - RAG 準確率評估 (US 4.2)

  - **Section**: [Request/Response Format](../../../docs/api/knowledge-api-design.md#request-response-format) - 標準格式定義
  - **Section**: [Error Handling](../../../docs/api/knowledge-api-design.md#error-handling) - 統一錯誤處理策略
  - **Section**: [Validation Rules](../../../docs/api/knowledge-api-design.md#validation-rules) - API 驗證規則

**API 請求/回應範例**:

**1. 文檔上傳 API**:
```http
POST /api/v1/documents/upload
Content-Type: multipart/form-data

Request Body:
- files: File[] (最多 10 個檔案)
- agentId: Guid
- tags: string[]
- description: string

Response (201 Created):
{
  "uploadId": "uuid",
  "status": "processing",
  "files": [
    {
      "fileName": "document.pdf",
      "fileSize": 1024000,
      "status": "uploaded",
      "documentId": "uuid"
    }
  ],
  "totalFiles": 1,
  "estimatedProcessingTime": "2m30s"
}
```

**2. Hybrid Search API**:
```http
POST /api/v1/agents/{agentId}/search
Content-Type: application/json

Request Body:
{
  "query": "如何設定 Agent 的 Persona 配置？",
  "topK": 10,
  "semanticWeight": 0.7,
  "keywordWeight": 0.3,
  "enableReranking": true,
  "scoreThreshold": 0.7
}

Response (200 OK):
{
  "results": [
    {
      "chunkId": "uuid",
      "documentId": "uuid",
      "content": "Persona 配置可以透過 JSON/YAML 檔案設定...",
      "score": 0.92,
      "chunkIndex": 5,
      "metadata": {
        "document_name": "persona-config-guide.md",
        "page": 3
      }
    }
  ],
  "totalResults": 10,
  "queryTime": "450ms"
}
```

### 1.5 資料庫設計引用

**Database Schema 設計**:
- 🗄️ [Knowledge Database Schema](../../../docs/database/knowledge-schema.md) - Knowledge 系統資料庫 Schema 設計
  - **Section**: [Documents Table](../../../docs/database/knowledge-schema.md#documents-table) - documents 表結構定義
    ```sql
    CREATE TABLE documents (
        id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
        agent_id UUID NOT NULL REFERENCES agents(id),
        file_name VARCHAR(255) NOT NULL,
        file_size BIGINT NOT NULL,
        content_type VARCHAR(100) NOT NULL,
        blob_uri TEXT NOT NULL,
        status VARCHAR(50) NOT NULL, -- Processing, Completed, Failed
        page_count INT,
        chunk_count INT,
        error_message TEXT,
        tags TEXT[],
        description TEXT,
        created_at TIMESTAMP NOT NULL DEFAULT NOW(),
        updated_at TIMESTAMP,
        INDEX idx_documents_agent_id (agent_id),
        INDEX idx_documents_status (status),
        INDEX idx_documents_created_at (created_at DESC)
    );
    ```

  - **Section**: [DocumentChunks Table](../../../docs/database/knowledge-schema.md#document-chunks-table) - document_chunks 表結構
    ```sql
    CREATE TABLE document_chunks (
        id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
        document_id UUID NOT NULL REFERENCES documents(id) ON DELETE CASCADE,
        chunk_index INT NOT NULL,
        content TEXT NOT NULL,
        token_count INT NOT NULL,
        start_position INT NOT NULL,
        end_position INT NOT NULL,
        created_at TIMESTAMP NOT NULL DEFAULT NOW(),
        content_vector TSVECTOR GENERATED ALWAYS AS (to_tsvector('english', content)) STORED,
        INDEX idx_chunks_document_id (document_id),
        INDEX idx_chunks_content_fts USING GIN (content_vector),
        UNIQUE (document_id, chunk_index)
    );
    ```

  - **Section**: [Indexing Strategy](../../../docs/database/knowledge-schema.md#indexing-strategy) - 索引設計策略
  - **Section**: [Full-Text Search Configuration](../../../docs/database/knowledge-schema.md#full-text-search-configuration) - PostgreSQL FTS 設定

**Qdrant Collection 設計**:
- 🗄️ [Qdrant Design](../../../docs/database/qdrant-design.md) - Qdrant Vector Database 設計
  - **Section**: [Collection Structure](../../../docs/database/qdrant-design.md#collection-structure) - Collection 命名與結構
    - Collection 命名: `agent_{agentId}` (每個 Agent 獨立 Collection)
    - Vector 維度: 1536 (Azure OpenAI ada-002)
    - Distance Metric: Cosine Similarity
    - Index Type: HNSW (Hierarchical Navigable Small World)

  - **Section**: [HNSW Configuration](../../../docs/database/qdrant-design.md#hnsw-configuration) - HNSW 索引參數
    ```json
    {
      "hnsw_config": {
        "m": 16,              // 鄰居數量（越大越精確但越慢）
        "ef_construct": 100,  // 索引建立時的搜尋深度
        "ef": 128             // 搜尋時的深度
      }
    }
    ```

  - **Section**: [Metadata Schema](../../../docs/database/qdrant-design.md#metadata-schema) - Vector Payload 設計
    ```json
    {
      "document_id": "uuid",
      "chunk_index": 5,
      "content": "chunk text content",
      "token_count": 512,
      "document_name": "file.pdf",
      "created_at": "2026-01-06T10:00:00Z"
    }
    ```

  - **Section**: [Performance Optimization](../../../docs/database/qdrant-design.md#performance-optimization) - 效能優化策略

---

## 二、US 4.1: 文檔上傳與處理 - 詳細技術上下文

### 2.1 MVP 範圍定義與技術決策

**完整 User Story 規格**: [US 5.1 - 知識庫文件上傳與處理](../../../docs/user-stories/modules/module-05-agent-memory.md#us-51)

**MVP 範圍邊界**:

✅ **本 Sprint 必須實現 (P0)**:
1. **文檔上傳 API** - 支援批次上傳（最多 10 檔案，50MB/檔案）
   - 支援格式: PDF, DOCX, TXT, MD
   - 檔案驗證: 類型、大小、病毒掃描
   - Azure Blob Storage 上傳
   - 背景任務排程
   - **參考**: [Document Upload API](../../../docs/api/knowledge-api-design.md#document-upload-endpoints)

2. **文檔解析引擎** - 提取純文字內容與元數據
   - PDF 解析器（PdfPig 庫）
   - DOCX 解析器（DocumentFormat.OpenXml）
   - TXT/MD 解析器（直接讀取）
   - 元數據提取（標題、作者、日期）
   - **參考**: [Document Parsing Strategy](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md)

3. **文檔分塊 (Chunking)** - 智能切分文本
   - Fixed-Size Chunking: 512-2048 tokens（可配置）
   - Overlap: 0-200 tokens（可配置）
   - 語義邊界保留（句子、段落）
   - Token 計數（TikToken）
   - **參考**: [Chunking Strategies](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md)

4. **向量化 (Embedding)** - 文本向量轉換
   - Azure OpenAI text-embedding-ada-002
   - Batch Embedding（批次處理）
   - Memory Cache（1 小時快取）
   - 向量維度: 1536
   - **參考**: [Embedding Service](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md)

5. **向量索引 (Qdrant)** - 向量資料庫儲存
   - Qdrant Collection 管理
   - HNSW 索引建立
   - Metadata 過濾支援
   - Batch Upsert（批次插入）
   - **參考**: [Qdrant Design](../../../docs/database/qdrant-design.md)

6. **背景處理任務** - Hangfire 背景任務
   - DocumentProcessingJob（Upload → Parse → Chunk → Embed → Index）
   - 進度追蹤
   - 失敗重試（最多 3 次）
   - **參考**: [Background Jobs](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md)

❌ **明確排除 Phase 2 功能 (延後)**:
- 進階 OCR（複雜圖表、手寫文字） - Phase 2
- 多語言支援（自動語言檢測） - Sprint 6
- 文檔版本控制（完整版本歷史） - Sprint 6
- 增量更新（僅更新變更的 Chunks） - Phase 2
- 圖片與表格提取 - Phase 2

**關鍵技術決策**:

**決策 1: 文檔儲存架構**
- **選擇**: Azure Blob Storage（原始文件）+ PostgreSQL（元數據）+ Qdrant（向量）
- **理由**: 分層儲存確保靈活性，原始文件與向量索引分離，支援未來 Azure AI Search 遷移
- **替代方案**: 全部存 PostgreSQL（BLOB 欄位佔用大量空間），純 NoSQL（缺乏關聯查詢）
- **參考**: [ADR-015: Document Storage Strategy](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md)

**決策 2: 文檔解析庫選擇**
- **選擇**: PDF → PdfPig, DOCX → DocumentFormat.OpenXml, TXT/MD → StreamReader
- **理由**: PdfPig 開源且功能強大，OpenXml 官方 SDK 穩定，TXT/MD 無需額外庫
- **替代方案**: iTextSharp（商業授權限制），Aspose（價格昂貴）
- **參考**: [Document Parsing Strategy](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md#library-selection)

**決策 3: Chunking 策略**
- **選擇**: Fixed-Size Chunking（512-2048 tokens）+ Configurable Overlap（0-200 tokens）
- **理由**: 簡單高效，支援 80%+ 場景，Overlap 提升檢索準確率 10-15%
- **替代方案**: Semantic Chunking（複雜度高，延後到 Phase 2），Sentence-based（chunk 大小不一致）
- **參考**: [ADR-016: Chunking Strategy](../../../docs/architecture/adr/ADR-016-chunking-strategy.md)

**決策 4: Embedding 模型選擇**
- **選擇**: Azure OpenAI text-embedding-ada-002（1536 維度）
- **理由**: 穩定可靠，官方支援，成本合理（$0.0001/1K tokens），準確率高
- **替代方案**: ada-003（更貴），Multilingual 模型（Phase 2 多語言需求）
- **參考**: [ADR-018: Embedding Service Design](../../../docs/architecture/adr/ADR-018-embedding-service-design.md)

**決策 5: 背景任務框架**
- **選擇**: Hangfire + PostgreSQL Storage
- **理由**: 持久化任務佇列，支援重試與進度追蹤，Dashboard 監控
- **替代方案**: Azure Queue（雲端依賴），RabbitMQ（額外基礎設施），In-Memory Queue（不持久化）
- **參考**: [ADR-019: Background Job Architecture](../../../docs/architecture/adr/ADR-019-background-job-architecture.md)

### 2.2 文檔解析引擎架構

**完整設計文檔**: [Document Parsing Strategy](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md)

**IDocumentParser 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IDocumentParser.cs`
- **設計模式**: Strategy Pattern（每種格式一個解析器）
- **Factory Pattern**: DocumentParserFactory 根據副檔名選擇解析器

**核心方法簽名**:
```csharp
public interface IDocumentParser
{
    string[] SupportedExtensions { get; }
    Task<DocumentParseResult> ParseAsync(
        Stream fileStream,
        string fileName,
        CancellationToken cancellationToken = default);
}

public sealed class DocumentParseResult
{
    public required string Content { get; init; }
    public required DocumentMetadata Metadata { get; init; }
    public int PageCount { get; init; }
    public int CharacterCount { get; init; }
    public TimeSpan ParseDuration { get; init; }
}
```

**解析器實作**:

**1. PDF 解析器 (PdfDocumentParser)**:
- **庫**: PdfPig (NuGet: `UglyToad.PdfPig`)
- **功能**: 提取文字內容、元數據、頁面數量
- **OCR 支援**: Phase 2 實作（掃描的 PDF）
- **參考**: [PDF Parsing Guide](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md#pdf-parser)
- **實作位置**: `AIAgentPlatform.Infrastructure/Services/DocumentParsers/PdfDocumentParser.cs`

**2. DOCX 解析器 (DocxDocumentParser)**:
- **庫**: DocumentFormat.OpenXml (NuGet: `DocumentFormat.OpenXml`)
- **功能**: 提取段落文字、表格內容、元數據
- **結構保留**: 保留段落邊界、表格結構
- **參考**: [DOCX Parsing Guide](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md#docx-parser)
- **實作位置**: `AIAgentPlatform.Infrastructure/Services/DocumentParsers/DocxDocumentParser.cs`

**3. TXT/MD 解析器 (TextDocumentParser)**:
- **庫**: System.IO.StreamReader（.NET 內建）
- **功能**: 直接讀取文字內容
- **編碼檢測**: 自動偵測 UTF-8, Big5, GB2312
- **參考**: [Text Parsing Guide](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md#text-parser)
- **實作位置**: `AIAgentPlatform.Infrastructure/Services/DocumentParsers/TextDocumentParser.cs`

**錯誤處理策略**:
- **DocumentParseException**: 自訂例外類型
- **重試機制**: Hangfire 自動重試（最多 3 次）
- **日誌記錄**: Structured Logging（記錄檔案名稱、大小、錯誤訊息）

### 2.3 文檔分塊策略設計

**完整設計文檔**: [Chunking Strategies](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md)

**IDocumentChunker 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IDocumentChunker.cs`
- **策略**: Fixed-Size Chunking（Phase 1A），Semantic Chunking（Phase 2）

**ChunkingOptions 配置**:
```csharp
public sealed class ChunkingOptions
{
    public int ChunkSize { get; init; } = 1024; // tokens
    public int OverlapSize { get; init; } = 100; // tokens
    public bool PreserveSentenceBoundary { get; init; } = true;
    public bool PreserveParagraphBoundary { get; init; } = true;
}
```

**FixedSizeChunker 實作細節**:
- **演算法**:
  1. 將內容按句子分割
  2. 累積句子直到達到 ChunkSize
  3. 建立 Chunk（包含 Overlap）
  4. 記錄 Chunk 位置（StartPosition, EndPosition）
- **Token 計數**: 使用 TikToken (cl100k_base encoding)
- **Overlap 處理**: 從上一個 Chunk 末尾取 OverlapSize 個 tokens
- **參考**: [Fixed-Size Chunking Algorithm](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md#fixed-size-chunking)

**Chunk Size 選擇指南**:
| Chunk Size | 適用場景 | 優點 | 缺點 |
|-----------|---------|------|------|
| 512 tokens | 精確問答、短文本 | 精確定位、低噪音 | 可能切斷上下文 |
| 1024 tokens | 通用場景（預設）| 平衡準確率與上下文 | 中等噪音 |
| 2048 tokens | 長篇文章、技術文檔 | 豐富上下文、完整語義 | 高噪音、效能影響 |

**Overlap 影響分析**:
- **無 Overlap (0 tokens)**: 可能遺失跨 Chunk 的重要資訊
- **小 Overlap (50-100 tokens)**: 提升 10-15% 準確率（推薦）
- **大 Overlap (200 tokens)**: 提升 15-20% 準確率，但增加 30%+ 儲存成本

**參考**: [Chunking Best Practices](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md#best-practices)

### 2.4 向量化服務設計

**完整設計文檔**: [Embedding Service](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md)

**IEmbeddingService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IEmbeddingService.cs`
- **方法**:
  - `GenerateEmbeddingAsync(string text)` - 單個文本向量化
  - `GenerateBatchEmbeddingsAsync(IEnumerable<string> texts)` - 批次向量化

**OpenAIEmbeddingService 實作**:
- **模型**: text-embedding-ada-002
- **維度**: 1536
- **API**: Azure OpenAI Embeddings API
- **實作位置**: `AIAgentPlatform.Infrastructure/Services/OpenAIEmbeddingService.cs`

**快取策略**:
- **快取鍵**: SHA256 Hash（文本內容）
- **快取時間**: 1 小時（IMemoryCache）
- **快取命中率**: 預期 60-70%（相似文本重複出現）
- **記憶體管理**: LRU（Least Recently Used）自動清理

**批次處理優化**:
- **Batch Size**: 100 chunks/batch（OpenAI API 限制: 2048）
- **並行處理**: 最多 3 個並行請求（避免 Rate Limit）
- **Rate Limit 處理**:
  - 429 錯誤 → 指數退避重試（1s, 2s, 4s）
  - 每 batch 間隔 100ms（避免超過限額）
- **參考**: [Batch Embedding Optimization](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md#batch-optimization)

**成本估算**:
- **價格**: $0.0001 / 1K tokens
- **範例**: 100 頁 PDF（~50K tokens）→ 50 chunks（1024 tokens/chunk）→ $0.005
- **月成本**: 1000 文檔 * $0.005 = $5/月

### 2.5 Qdrant 向量儲存架構

**完整設計文檔**: [Qdrant Design](../../../docs/database/qdrant-design.md)

**Collection 架構設計**:
- **命名策略**: `agent_{agentId}` (每個 Agent 獨立 Collection)
- **好處**: 資料隔離、獨立配置、刪除方便
- **Vector 維度**: 1536（Azure OpenAI ada-002）
- **Distance Metric**: Cosine Similarity（最常用於文本向量）

**HNSW 索引配置**:
```json
{
  "hnsw_config": {
    "m": 16,              // 鄰居數量（建議 8-32）
    "ef_construct": 100,  // 索引建立深度（建議 100-200）
    "ef": 128,            // 搜尋深度（建議 64-256）
    "full_scan_threshold": 10000
  }
}
```

**HNSW 參數調整指南**:
| 參數 | 值範圍 | 效能影響 | 建議值 |
|-----|-------|---------|-------|
| m | 8-32 | 越大越精確但越慢 | 16（通用） |
| ef_construct | 100-400 | 索引建立時間 | 100（平衡） |
| ef | 64-512 | 搜尋精確度 | 128（通用） |

**Payload (Metadata) 設計**:
```json
{
  "document_id": "uuid",
  "chunk_index": 5,
  "content": "chunk text content",
  "token_count": 512,
  "document_name": "file.pdf",
  "page": 3,
  "created_at": "2026-01-06T10:00:00Z"
}
```

**IVectorStoreService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IVectorStoreService.cs`
- **主要方法**:
  - `CreateCollectionAsync()` - 建立 Collection
  - `UpsertVectorsAsync()` - 批次插入/更新向量
  - `DeleteVectorsAsync()` - 刪除向量
  - `SearchAsync()` - 向量搜尋（支援 Metadata 過濾）

**效能優化策略**:
- **Batch Upsert**: 批次插入（100 vectors/batch）
- **Async Operations**: 所有操作非同步執行
- **Connection Pooling**: Qdrant Client 連接池
- **參考**: [Qdrant Performance Tuning](../../../docs/database/qdrant-design.md#performance-optimization)

### 2.6 背景處理任務設計

**完整設計文檔**: [Background Jobs](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md)

**DocumentProcessingJob Pipeline**:
```
1. Upload → Azure Blob Storage
2. Parse → Extract text + metadata
3. Chunk → Split into chunks (1024 tokens)
4. Embed → Generate vectors (ada-002)
5. Index → Store in Qdrant
6. Update → Document status = Completed
```

**IDocumentProcessingJob 介面**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IDocumentProcessingJob.cs`
- **方法**: `ProcessDocumentAsync(Guid documentId, CancellationToken cancellationToken)`

**實作細節**:
- **位置**: `AIAgentPlatform.Infrastructure/BackgroundJobs/DocumentProcessingJob.cs`
- **依賴注入**:
  - IDocumentRepository
  - IBlobStorageService
  - IDocumentParserFactory
  - IDocumentChunker
  - IEmbeddingService
  - IVectorStoreService

**Hangfire 配置**:
```csharp
// Hangfire Server Configuration
builder.Services.AddHangfireServer(options =>
{
    options.WorkerCount = 5; // 同時處理 5 個任務
    options.Queues = new[] { "document-processing", "default" };
});

// Job 排程
BackgroundJob.Enqueue<IDocumentProcessingJob>(
    job => job.ProcessDocumentAsync(documentId, CancellationToken.None));
```

**錯誤處理與重試**:
- **自動重試**: Hangfire 自動重試（最多 3 次）
- **指數退避**: 1 min, 5 min, 15 min
- **失敗處理**:
  - 更新 Document Status = Failed
  - 記錄錯誤訊息（ErrorMessage 欄位）
  - 發送通知（可選）

**進度追蹤**:
- **Document Status**: Processing, Completed, Failed
- **Chunk Count**: 記錄已處理的 Chunk 數量
- **Estimated Time**: 根據檔案大小估算（1MB = 10 秒）

**參考**: [Hangfire Best Practices](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md#best-practices)

---

## 三、US 4.2: RAG 檢索策略配置 - 詳細技術上下文

### 3.1 語義搜尋實作細節

**完整設計文檔**: [Semantic Search](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md)

**ISemanticSearchService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/ISemanticSearchService.cs`
- **核心方法**: `SearchAsync(string query, Guid agentId, SemanticSearchOptions options)`

**SemanticSearchOptions 配置**:
```csharp
public sealed class SemanticSearchOptions
{
    public int TopK { get; init; } = 10;
    public float ScoreThreshold { get; init; } = 0.7f; // Cosine Similarity
    public Dictionary<string, object>? MetadataFilter { get; init; }
}
```

**搜尋流程**:
```
1. Query Embedding → Generate query vector (1536 維度)
2. Vector Search → Qdrant.SearchAsync(queryVector, topK, threshold)
3. Result Filtering → Filter by score threshold (≥0.7)
4. DTO Mapping → Convert to SearchResultDto
```

**實作細節**:
- **位置**: `AIAgentPlatform.Infrastructure/Services/SemanticSearchService.cs`
- **依賴**:
  - IEmbeddingService（生成查詢向量）
  - IVectorStoreService（Qdrant 搜尋）

**Cosine Similarity 閾值選擇**:
| 閾值 | 適用場景 | 準確率 | 召回率 |
|-----|---------|-------|-------|
| 0.9+ | 精確匹配、低誤報 | 95%+ | 60-70% |
| 0.8-0.9 | 高品質結果 | 90-95% | 70-80% |
| 0.7-0.8 | 平衡準確率與召回率（預設）| 85-90% | 80-90% |
| 0.6-0.7 | 高召回率、可能有噪音 | 75-85% | 90%+ |

**效能目標**:
- **搜尋延遲**: <200ms（100K 向量）
- **吞吐量**: >50 queries/sec
- **HNSW ef**: 128（平衡精確度與速度）

**參考**: [Semantic Search Optimization](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md#optimization)

### 3.2 關鍵字搜尋 (BM25) 實作

**完整設計文檔**: [Keyword Search](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md)

**IKeywordSearchService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IKeywordSearchService.cs`
- **核心方法**: `SearchAsync(string query, Guid agentId, KeywordSearchOptions options)`

**PostgreSQL Full-Text Search 實作**:
- **tsvector 欄位**: `content_vector TSVECTOR GENERATED ALWAYS AS (to_tsvector('english', content)) STORED`
- **GIN 索引**: `CREATE INDEX idx_chunks_content_fts USING GIN (content_vector);`
- **查詢語法**: `ts_query('english', 'word1 & word2 & word3')`

**BM25 演算法**:
```sql
-- PostgreSQL ts_rank_cd 使用 BM25 變體
SELECT
    id,
    content,
    ts_rank_cd(content_vector, to_tsquery('english', query)) AS rank
FROM document_chunks
WHERE content_vector @@ to_tsquery('english', query)
ORDER BY rank DESC
LIMIT 10;
```

**查詢預處理**:
```csharp
private string PreprocessQuery(string query)
{
    // 1. 分詞
    var words = query.Split(' ', StringSplitOptions.RemoveEmptyEntries);

    // 2. 移除停用詞（the, a, an, is, are, of, in）
    var stopWords = new HashSet<string> { "the", "a", "an", "is", "are", "of", "in" };
    var filteredWords = words.Where(w => !stopWords.Contains(w.ToLower()));

    // 3. 組合為 tsquery 格式（& = AND）
    return string.Join(" & ", filteredWords);
}
```

**KeywordSearchOptions 配置**:
```csharp
public sealed class KeywordSearchOptions
{
    public int TopK { get; init; } = 10;
    public bool UseSynonyms { get; init; } = true; // Phase 2 實作
    public Dictionary<string, float>? TermBoosts { get; init; } // Phase 2 實作
}
```

**效能優化**:
- **GIN 索引**: 提升 10-100x 查詢速度
- **Index Only Scan**: 儘可能使用索引掃描
- **參考**: [PostgreSQL FTS Performance](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md#performance)

### 3.3 混合檢索 (RRF) 融合策略

**完整設計文檔**: [Hybrid Search](../../../docs/technical-implementation/01-backend-net9/17-hybrid-search.md)

**IHybridSearchService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IHybridSearchService.cs`
- **核心方法**: `SearchAsync(string query, Guid agentId, HybridSearchOptions options)`

**HybridSearchOptions 配置**:
```csharp
public sealed class HybridSearchOptions
{
    public int TopK { get; init; } = 10;
    public float SemanticWeight { get; init; } = 0.7f;
    public float KeywordWeight { get; init; } = 0.3f;
    public bool EnableReranking { get; init; } = true;
    public float SemanticScoreThreshold { get; init; } = 0.7f;
}
```

**RRF (Reciprocal Rank Fusion) 演算法**:
```csharp
// RRF 公式: score = Σ(weight / (k + rank))
// k = 60（常數，根據實驗調整）
private List<SearchResultDto> FuseResultsWithRRF(
    IReadOnlyList<SearchResultDto> semanticResults,
    IReadOnlyList<SearchResultDto> keywordResults,
    float semanticWeight,
    float keywordWeight,
    int topK)
{
    const int k = 60;
    var scoresDict = new Dictionary<Guid, (SearchResultDto Result, float Score)>();

    // 語義搜尋結果
    for (int i = 0; i < semanticResults.Count; i++)
    {
        var result = semanticResults[i];
        var rrfScore = semanticWeight / (k + i + 1);

        if (scoresDict.ContainsKey(result.ChunkId))
        {
            scoresDict[result.ChunkId] = (
                result,
                scoresDict[result.ChunkId].Score + rrfScore
            );
        }
        else
        {
            scoresDict[result.ChunkId] = (result, rrfScore);
        }
    }

    // 關鍵字搜尋結果
    for (int i = 0; i < keywordResults.Count; i++)
    {
        var result = keywordResults[i];
        var rrfScore = keywordWeight / (k + i + 1);

        if (scoresDict.ContainsKey(result.ChunkId))
        {
            scoresDict[result.ChunkId] = (
                scoresDict[result.ChunkId].Result,
                scoresDict[result.ChunkId].Score + rrfScore
            );
        }
        else
        {
            scoresDict[result.ChunkId] = (result, rrfScore);
        }
    }

    // 根據 RRF 分數排序
    return scoresDict
        .OrderByDescending(kvp => kvp.Value.Score)
        .Take(topK * 2) // 取兩倍結果供 Re-ranking 使用
        .Select(kvp => new SearchResultDto
        {
            ChunkId = kvp.Key,
            DocumentId = kvp.Value.Result.DocumentId,
            Content = kvp.Value.Result.Content,
            Score = kvp.Value.Score,
            ChunkIndex = kvp.Value.Result.ChunkIndex,
            Metadata = kvp.Value.Result.Metadata
        })
        .ToList();
}
```

**權重配置建議**:
| 場景 | Semantic Weight | Keyword Weight | 說明 |
|-----|----------------|---------------|------|
| 概念性問題 | 0.8 | 0.2 | 強調語義理解 |
| 通用場景（預設）| 0.7 | 0.3 | 平衡語義與關鍵字 |
| 精確匹配 | 0.5 | 0.5 | 平等權重 |
| 術語查詢 | 0.3 | 0.7 | 強調關鍵字匹配 |

**實作流程**:
```
1. Parallel Execution → Semantic Search || Keyword Search
2. RRF Fusion → Combine results with weighted RRF scores
3. Re-ranking (Optional) → Cross-Encoder re-rank top results
4. Return → Top-K final results
```

**參考**: [RRF Algorithm Paper](https://plg.uwaterloo.ca/~gvcormac/cormacksigir09-rrf.pdf)

### 3.4 Cross-Encoder Re-ranking 機制

**完整設計文檔**: [Re-ranking Strategy](../../../docs/technical-implementation/01-backend-net9/18-reranking.md)

**IReranker 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IReranker.cs`
- **核心方法**: `RerankAsync(string query, IReadOnlyList<SearchResultDto> results)`

**Cross-Encoder 模型選擇**:
| 模型 | 參數量 | 速度 | 準確率 | 適用場景 |
|-----|-------|------|-------|---------|
| ms-marco-MiniLM-L-6-v2 | 22M | 快 | 85-90% | 通用場景（推薦）|
| ms-marco-MiniLM-L-12-v2 | 33M | 中 | 90-92% | 高準確率需求 |
| ms-marco-electra-base | 110M | 慢 | 92-95% | 最高準確率（生產環境謹慎使用）|

**推薦**: `cross-encoder/ms-marco-MiniLM-L-6-v2`（平衡速度與準確率）

**Re-ranking 流程**:
```
1. Input → Query + Candidate passages (Top 20-30 from RRF)
2. Cross-Encoder Scoring → Query-passage relevance scores (0-1)
3. Sort → Descending order by Cross-Encoder scores
4. Return → Top-K re-ranked results
```

**實作方式**:
- **選項 1**: HuggingFace API（雲端推理）
  - 優點: 無需本地部署、自動更新
  - 缺點: API 延遲、成本、依賴外部服務
  - 適用: MVP、快速驗證

- **選項 2**: ONNX Runtime（本地推理）
  - 優點: 低延遲、無外部依賴、成本低
  - 缺點: 需要模型部署、更新複雜
  - 適用: 生產環境（Phase 2）

**CrossEncoderReranker 實作**（HuggingFace API）:
```csharp
public async Task<List<SearchResultDto>> RerankAsync(
    string query,
    IReadOnlyList<SearchResultDto> results,
    CancellationToken cancellationToken = default)
{
    if (!results.Any()) return new List<SearchResultDto>();

    try
    {
        // 呼叫 HuggingFace Inference API
        var requestPayload = new
        {
            inputs = new
            {
                query,
                passages = results.Select(r => r.Content).ToArray()
            }
        };

        var response = await _httpClient.PostAsJsonAsync(
            "https://api-inference.huggingface.co/models/cross-encoder/ms-marco-MiniLM-L-6-v2",
            requestPayload,
            cancellationToken);

        response.EnsureSuccessStatusCode();

        var scores = await response.Content.ReadFromJsonAsync<float[]>(cancellationToken);

        // 根據 Cross-Encoder 分數重新排序
        var rankedResults = results
            .Select((result, index) => new SearchResultDto
            {
                ChunkId = result.ChunkId,
                DocumentId = result.DocumentId,
                Content = result.Content,
                Score = scores![index], // Cross-Encoder 分數
                ChunkIndex = result.ChunkIndex,
                Metadata = result.Metadata
            })
            .OrderByDescending(r => r.Score)
            .ToList();

        _logger.LogInformation("Re-ranked {Count} results using Cross-Encoder", rankedResults.Count);

        return rankedResults;
    }
    catch (Exception ex)
    {
        _logger.LogWarning(ex, "Re-ranking failed, returning original order");
        return results.ToList();
    }
}
```

**效能考量**:
- **Re-ranking 數量**: 建議 20-30 個候選（過多會影響速度）
- **API 延遲**: ~200-500ms（HuggingFace API）
- **本地推理延遲**: ~50-100ms（ONNX Runtime，Phase 2）
- **總搜尋時間**: Semantic (100ms) + Keyword (50ms) + RRF (10ms) + Re-rank (200ms) = ~360ms

**參考**: [Cross-Encoder Models](https://www.sbert.net/examples/applications/cross-encoder/README.html)

### 3.5 RAG 準確率評估框架

**完整設計文檔**: [RAG Accuracy Testing](../../../docs/testing/rag-accuracy-testing.md)

**IRAGEvaluationService 介面設計**:
- **位置**: `AIAgentPlatform.Application/Interfaces/IRAGEvaluationService.cs`
- **核心方法**: `EvaluateAsync(string testDatasetPath, Guid agentId, HybridSearchOptions searchOptions)`

**測試集格式**:
```json
{
  "dataset_name": "agent_knowledge_qa",
  "version": "1.0",
  "questions": [
    {
      "id": "q001",
      "question": "如何設定 Agent 的 Persona 配置？",
      "expected_answer": "透過 Persona 配置檔（JSON/YAML）設定角色、溝通風格、專業領域等",
      "expected_sources": ["persona-config-guide.md", "api-documentation.pdf"],
      "expected_chunk_ids": ["chunk_uuid_1", "chunk_uuid_2"],
      "difficulty": "easy"
    }
  ]
}
```

**評估指標**:

**1. Accuracy（準確率）**:
```
Accuracy = (返回正確文檔的問題數 / 總問題數) × 100%

評分標準:
- Top-1 結果包含正確答案: 1 分
- Top-3 結果包含正確答案: 0.5 分
- 未返回正確答案: 0 分

目標: ≥90%（80% 是最低可接受門檻）
```

**2. MRR (Mean Reciprocal Rank)**:
```
MRR = (1/N) × Σ(1 / rank_i)

rank_i = 正確答案在結果中的排名（1-based）

範例:
Q1: 正確答案排名 1 → 1/1 = 1.0
Q2: 正確答案排名 3 → 1/3 = 0.33
Q3: 正確答案排名 5 → 1/5 = 0.2
MRR = (1.0 + 0.33 + 0.2) / 3 = 0.51

目標: ≥0.8
```

**3. NDCG (Normalized Discounted Cumulative Gain)**:
```
DCG = Σ(relevance_i / log2(i + 1))
NDCG = DCG / IDCG (Ideal DCG)

relevance_i = 1 if 正確答案, else 0
IDCG = 理想情況下的 DCG（所有正確答案排在最前面）

目標: ≥0.85
```

**評估流程**:
```
1. Load Test Dataset → 載入標準測試集
2. For each question:
   a. Execute Hybrid Search
   b. Evaluate result (Accuracy, MRR, NDCG)
   c. Log results
3. Calculate overall metrics
4. Generate evaluation report
```

**EvaluationReport DTO**:
```csharp
public sealed class EvaluationReport
{
    public int TotalQuestions { get; init; }
    public int CorrectResults { get; init; }
    public double Accuracy { get; init; } // 百分比
    public double MeanReciprocalRank { get; init; } // MRR
    public double NormalizedDiscountedCumulativeGain { get; init; } // NDCG
    public bool PassThreshold { get; init; } // 是否達到 90% 準確率
    public DateTime EvaluatedAt { get; init; }
}
```

**標準測試集要求**:
- **問題數量**: ≥100 個問題
- **難度分佈**: Easy (40%), Medium (40%), Hard (20%)
- **覆蓋範圍**: 所有核心領域（Agent, Persona, Plugin, Knowledge, Workflow）
- **維護**: 每個 Sprint 更新測試集

**參考**: [Information Retrieval Metrics](https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval))

---

## 四、完整文檔引用清單 (按功能分類)

### 核心規劃文檔

**Sprint 5 規劃**:
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1A Knowledge 範圍
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-5) - Sprint 5 詳細分析
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Knowledge RAG 開發策略

**Sprint 5 執行文檔**:
- 📖 [SPRINT-5-1-OVERVIEW.md](./SPRINT-5-1-OVERVIEW.md) - Sprint 5 概覽
- 📖 [SPRINT-5-2-PLAN.md](./SPRINT-5-2-PLAN.md) - Sprint 5 詳細實施計劃
- 📖 [SPRINT-5-4-CHECKLIST.md](./SPRINT-5-4-CHECKLIST.md) - Sprint 5 任務清單

### User Stories & Epics

**Module 05: Knowledge Management**:
- 📖 [Module 05](../../../docs/user-stories/modules/module-05-agent-memory.md) - Complete specification
  - [US 5.1 - 知識庫文件上傳與處理](../../../docs/user-stories/modules/module-05-agent-memory.md#us-51)
  - [US 5.2 - 精確檢索策略配置](../../../docs/user-stories/modules/module-05-agent-memory.md#us-52)

**Epic 05: Knowledge Management & RAG**:
- 📖 [Epic 05](../../../docs/user-stories/epics/epic-05.md) - Strategic overview

### 架構設計 ADR

**Knowledge 相關 ADR**:
- 🏗️ [ADR-015: Document Storage Strategy](../../../docs/architecture/adr/ADR-015-document-storage-strategy.md)
- 🏗️ [ADR-016: Chunking Strategy](../../../docs/architecture/adr/ADR-016-chunking-strategy.md)
- 🏗️ [ADR-017: RAG Retrieval Strategy](../../../docs/architecture/adr/ADR-017-rag-retrieval-strategy.md)
- 🏗️ [ADR-018: Embedding Service Design](../../../docs/architecture/adr/ADR-018-embedding-service-design.md)
- 🏗️ [ADR-019: Background Job Architecture](../../../docs/architecture/adr/ADR-019-background-job-architecture.md)

**Core ADRs**:
- 🏗️ [ADR-001: Clean Architecture](../../../docs/architecture/adr/ADR-001-clean-architecture.md)
- 🏗️ [ADR-002: CQRS Pattern](../../../docs/architecture/adr/ADR-002-cqrs-pattern.md)
- 🏗️ [ADR-003: Repository Pattern](../../../docs/architecture/adr/ADR-003-repository-pattern.md)

### 技術實作指南

**Backend 技術實作**:
- 📐 [10-document-parsing.md](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md)
- 📐 [11-chunking-strategies.md](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md)
- 📐 [12-embedding-service.md](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md)
- 📐 [13-azure-storage.md](../../../docs/technical-implementation/01-backend-net9/13-azure-storage.md)
- 📐 [14-background-jobs.md](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md)
- 📐 [15-semantic-search.md](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md)
- 📐 [16-keyword-search.md](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md)
- 📐 [17-hybrid-search.md](../../../docs/technical-implementation/01-backend-net9/17-hybrid-search.md)
- 📐 [18-reranking.md](../../../docs/technical-implementation/01-backend-net9/18-reranking.md)
- 📐 [19-performance-optimization.md](../../../docs/technical-implementation/01-backend-net9/19-performance-optimization.md)

### API 設計

**Knowledge API**:
- 📘 [knowledge-api-design.md](../../../docs/api/knowledge-api-design.md)
- 📘 [api-documentation-standards.md](../../../docs/api/api-documentation-standards.md)

### 資料庫設計

**Database Schema**:
- 🗄️ [knowledge-schema.md](../../../docs/database/knowledge-schema.md)
- 🗄️ [qdrant-design.md](../../../docs/database/qdrant-design.md)

### 測試文檔

**Testing Strategy**:
- 🧪 [testing-strategy.md](../../../docs/testing/testing-strategy.md)
- 🧪 [unit-testing-strategy.md](../../../docs/testing/unit-testing-strategy.md)
- 🧪 [integration-testing.md](../../../docs/testing/integration-testing.md)
- 🧪 [rag-accuracy-testing.md](../../../docs/testing/rag-accuracy-testing.md)

### 外部資源

**Azure 服務**:
- 🔗 [Azure OpenAI Embeddings](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/embeddings)
- 🔗 [Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/)

**向量資料庫**:
- 🔗 [Qdrant Documentation](https://qdrant.tech/documentation/)
- 🔗 [HNSW Algorithm Paper](https://arxiv.org/abs/1603.09320)

**RAG 最佳實踐**:
- 🔗 [Pinecone RAG Guide](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- 🔗 [LangChain RAG Documentation](https://python.langchain.com/docs/use_cases/question_answering/)

**NuGet 套件**:
- 🔗 [PdfPig](https://github.com/UglyToad/PdfPig) - PDF 解析
- 🔗 [DocumentFormat.OpenXml](https://github.com/OfficeDev/Open-XML-SDK) - DOCX 解析
- 🔗 [TikToken](https://github.com/microsoft/Tokenizer) - Token 計數
- 🔗 [Hangfire](https://www.hangfire.io/) - 背景任務
- 🔗 [Qdrant.Client](https://github.com/qdrant/qdrant-dotnet) - Qdrant .NET Client

---

## 📖 使用指南

### 如何使用此文件

**開發階段使用**:
1. **開發 US 4.1 文檔處理**: 閱讀「第二章」了解解析器、分塊、向量化、背景任務實作
2. **開發 US 4.2 RAG 檢索**: 閱讀「第三章」了解語義搜尋、關鍵字搜尋、混合檢索、Re-ranking 實作
3. **查詢架構決策**: 查閱「第一章 1.2-1.3」的 ADR 文檔
4. **查詢 API 規格**: 查閱「第一章 1.4」的 API 設計文檔
5. **查詢資料庫設計**: 查閱「第一章 1.5」的 Database Schema

**疑難排解使用**:
- **Qdrant 配置問題**: 查閱「2.5 Qdrant 向量儲存架構」
- **Chunking 策略問題**: 查閱「2.3 文檔分塊策略設計」
- **RAG 準確率問題**: 查閱「3.5 RAG 準確率評估框架」
- **效能優化問題**: 查閱各章節的「效能優化」部分

**Code Review 使用**:
- 驗證實作是否符合「關鍵技術決策」
- 檢查介面設計是否與文檔一致
- 確認效能目標是否達成

---

## 📝 更新日誌

**v2.1** (2025-11-13)
- 初始版本建立
- 完整 US 4.1 (文檔上傳與處理) 技術上下文
- 完整 US 4.2 (RAG 檢索策略) 技術上下文
- 6 個 ADR 文檔引用與說明
- 10 個技術實作指南引用
- 完整 API、資料庫、測試文檔引用

---

**文件維護**:
- 📅 下次審查日期: Sprint 5 開始前 (2026-01-06)
- 👤 負責人: Backend Tech Lead
- 🔄 更新頻率: Sprint 進行中每週更新技術細節

**相關文檔**:
- ⬆️ 上一層: [Sprint 5 Overview](./SPRINT-5-1-OVERVIEW.md)
- ⬅️ 上一步: [Sprint 5 Plan](./SPRINT-5-2-PLAN.md)
- ➡️ 下一步: [Sprint 5 Checklist](./SPRINT-5-4-CHECKLIST.md)
