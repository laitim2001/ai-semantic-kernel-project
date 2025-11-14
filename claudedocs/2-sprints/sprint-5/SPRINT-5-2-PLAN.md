# SPRINT-5-2-PLAN.md - Sprint 5 計劃書：Knowledge 管理與 RAG 檢索實施計劃

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
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-5) - Sprint 5 分析
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Knowledge RAG 系統策略
- 📐 [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 系統架構
- 📖 [Module 05 - Agent Memory](../../../docs/user-stories/modules/module-05-agent-memory.md) - US 5.1-5.2 完整規格

---

## 第一部分：本 Sprint 要建立什麼 (What to Build)

### US 4.1: 文檔上傳與處理 (8 SP)

**User Story 完整規格**: [US 5.1 - 知識庫文件上傳與處理](../../../docs/user-stories/modules/module-05-agent-memory.md#us-51)

#### 一、MVP 範圍定義

**必須實現功能 (P0 - 本 Sprint)**:
- [x] **文檔上傳 API**: 支援 PDF, DOCX, TXT, MD 格式
  - 多檔案上傳（最多 10 個檔案）
  - 單檔大小限制: 50MB
  - 批次上傳進度追蹤
  - 檔案類型驗證與安全檢查
  - **參考**: [Document Upload API Design](../../../docs/api/knowledge-api-design.md#upload)

- [x] **文檔解析引擎**: 提取純文字內容
  - PDF 解析器（支援 OCR）
  - DOCX 解析器（保留結構）
  - TXT/MD 解析器（直接讀取）
  - 元數據提取（標題、作者、日期）
  - **參考**: [Document Parsing Strategy](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md)

- [x] **文檔分塊 (Chunking)**: 智能切分文本
  - Fixed-size Chunking: 512-2048 tokens（可配置）
  - Overlap: 0-200 tokens（可配置）
  - 語義邊界保留（段落、章節）
  - Chunk 元數據記錄（來源文檔、位置）
  - **參考**: [Chunking Strategies](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md)

- [x] **向量化 (Embedding)**: 文本向量轉換
  - Azure OpenAI text-embedding-ada-002
  - Batch Embedding（提升效率）
  - Embedding 快取機制
  - 向量維度: 1536
  - **參考**: [Embedding Service](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md)

- [x] **向量索引 (Qdrant)**: 向量資料庫儲存
  - Qdrant Collection 建立與管理
  - Vector 插入與更新
  - Metadata 過濾支援
  - HNSW 索引優化
  - **參考**: [Qdrant Design](../../../docs/database/qdrant-design.md)

- [x] **Azure Blob Storage 整合**: 原始檔案儲存
  - Blob Container 管理
  - 分層儲存（Hot/Cool）
  - SAS Token 安全訪問
  - 檔案生命週期管理
  - **參考**: [Storage Architecture](../../../docs/architecture/storage-architecture.md)

**明確排除 Phase 2 功能 (延後)**:
- ❌ **進階 OCR**: 複雜圖表、手寫文字識別 - 延後到 Phase 2
- ❌ **多語言支援**: 自動語言檢測 - 延後到 Sprint 6
- ❌ **文檔版本控制**: 完整版本歷史 - 延後到 Sprint 6
- ❌ **增量更新**: 僅更新變更的 Chunks - 延後到 Phase 2
- ❌ **圖片與表格提取**: 圖片描述、表格結構化 - 延後到 Phase 2

**MVP 範圍參考**:
- 📖 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md#knowledge-management) - Knowledge 系統在 Phase 1A 的範圍
- 📖 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-5) - Sprint 5 任務分配

#### 二、詳細技術規格

##### US 4.1.1: 文檔上傳 API

**API Endpoint**:
```http
POST /api/v1/documents/upload
Content-Type: multipart/form-data

Request Body:
- files: File[] (最多 10 個檔案)
- agentId: Guid (關聯的 Agent ID)
- tags: string[] (可選的標籤)
- description: string (可選的描述)

Response:
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

**驗證規則**:
- 檔案類型白名單: `.pdf`, `.docx`, `.txt`, `.md`
- 單檔大小限制: 50MB
- 批次上傳限制: 最多 10 個檔案
- 總大小限制: 500MB
- 檔案名稱驗證: 不允許特殊字元、路徑穿越
- 病毒掃描: 整合 ClamAV 或 Azure Defender

**實作程式碼範例**:
```csharp
// Application/Documents/Commands/UploadDocument/UploadDocumentCommand.cs
public sealed record UploadDocumentCommand : IRequest<Result<UploadDocumentResponse>>
{
    public required Guid AgentId { get; init; }
    public required IFormFileCollection Files { get; init; }
    public string[]? Tags { get; init; }
    public string? Description { get; init; }
}

// Application/Documents/Commands/UploadDocument/UploadDocumentCommandHandler.cs
public sealed class UploadDocumentCommandHandler
    : IRequestHandler<UploadDocumentCommand, Result<UploadDocumentResponse>>
{
    private readonly IDocumentRepository _documentRepo;
    private readonly IBlobStorageService _blobStorage;
    private readonly IBackgroundJobService _backgroundJobs;
    private readonly ILogger<UploadDocumentCommandHandler> _logger;

    public async Task<Result<UploadDocumentResponse>> Handle(
        UploadDocumentCommand request,
        CancellationToken cancellationToken)
    {
        var uploadId = Guid.NewGuid();
        var documentIds = new List<Guid>();

        foreach (var file in request.Files)
        {
            // 1. 驗證檔案
            var validationResult = await ValidateFileAsync(file);
            if (!validationResult.IsSuccess)
            {
                return Result<UploadDocumentResponse>.Failure(validationResult.Error);
            }

            // 2. 上傳到 Azure Blob Storage
            var blobUri = await _blobStorage.UploadAsync(
                file.OpenReadStream(),
                file.FileName,
                file.ContentType,
                cancellationToken);

            // 3. 建立 Document Entity
            var document = Document.Create(
                agentId: request.AgentId,
                fileName: file.FileName,
                fileSize: file.Length,
                contentType: file.ContentType,
                blobUri: blobUri,
                tags: request.Tags,
                description: request.Description);

            await _documentRepo.AddAsync(document, cancellationToken);
            documentIds.Add(document.Id);

            // 4. 排程背景處理任務（解析、分塊、向量化）
            await _backgroundJobs.EnqueueAsync<IDocumentProcessingJob>(
                job => job.ProcessDocumentAsync(document.Id, cancellationToken));
        }

        await _documentRepo.SaveChangesAsync(cancellationToken);

        return Result<UploadDocumentResponse>.Success(new UploadDocumentResponse
        {
            UploadId = uploadId,
            Status = "processing",
            DocumentIds = documentIds,
            TotalFiles = request.Files.Count,
            EstimatedProcessingTime = EstimateProcessingTime(request.Files)
        });
    }

    private async Task<Result> ValidateFileAsync(IFormFile file)
    {
        // 檔案類型驗證
        var allowedExtensions = new[] { ".pdf", ".docx", ".txt", ".md" };
        var extension = Path.GetExtension(file.FileName).ToLowerInvariant();

        if (!allowedExtensions.Contains(extension))
        {
            return Result.Failure($"不支援的檔案類型: {extension}");
        }

        // 檔案大小驗證
        const long maxFileSize = 50 * 1024 * 1024; // 50MB
        if (file.Length > maxFileSize)
        {
            return Result.Failure($"檔案大小超過限制 (50MB): {file.FileName}");
        }

        // 檔案名稱驗證（防止路徑穿越）
        if (file.FileName.Contains("..") || file.FileName.Contains("/") || file.FileName.Contains("\\"))
        {
            return Result.Failure($"檔案名稱包含非法字元: {file.FileName}");
        }

        return Result.Success();
    }

    private string EstimateProcessingTime(IFormFileCollection files)
    {
        // 簡化估算: 1MB = 10 秒
        var totalSizeMB = files.Sum(f => f.Length) / (1024.0 * 1024.0);
        var estimatedSeconds = (int)(totalSizeMB * 10);
        return TimeSpan.FromSeconds(estimatedSeconds).ToString(@"m'm's's'");
    }
}
```

**FluentValidation 驗證器**:
```csharp
// Application/Documents/Commands/UploadDocument/UploadDocumentCommandValidator.cs
public sealed class UploadDocumentCommandValidator
    : AbstractValidator<UploadDocumentCommand>
{
    public UploadDocumentCommandValidator()
    {
        RuleFor(x => x.AgentId)
            .NotEmpty().WithMessage("Agent ID is required");

        RuleFor(x => x.Files)
            .NotEmpty().WithMessage("至少需要上傳一個檔案")
            .Must(files => files.Count <= 10)
            .WithMessage("單次上傳不能超過 10 個檔案");

        RuleFor(x => x.Files)
            .Must(files => files.Sum(f => f.Length) <= 500 * 1024 * 1024)
            .WithMessage("批次上傳總大小不能超過 500MB");

        RuleForEach(x => x.Files)
            .Must(file => file.Length <= 50 * 1024 * 1024)
            .WithMessage("單個檔案大小不能超過 50MB");

        RuleForEach(x => x.Files)
            .Must(file => {
                var allowedExtensions = new[] { ".pdf", ".docx", ".txt", ".md" };
                var extension = Path.GetExtension(file.FileName).ToLowerInvariant();
                return allowedExtensions.Contains(extension);
            })
            .WithMessage("僅支援 PDF, DOCX, TXT, MD 格式");
    }
}
```

**參考文檔**:
- 📐 [Document Upload API](../../../docs/api/knowledge-api-design.md#upload)
- 📐 [Validation Strategy](../../../docs/technical-implementation/01-backend-net9/06-validation-strategy.md)
- 📐 [Azure Blob Storage Integration](../../../docs/technical-implementation/01-backend-net9/13-azure-storage.md)

---

##### US 4.1.2: 文檔解析引擎

**IDocumentParser 介面設計**:
```csharp
// Application/Interfaces/IDocumentParser.cs
public interface IDocumentParser
{
    /// <summary>
    /// 支援的檔案類型
    /// </summary>
    string[] SupportedExtensions { get; }

    /// <summary>
    /// 解析文檔，提取純文字內容和元數據
    /// </summary>
    Task<DocumentParseResult> ParseAsync(
        Stream fileStream,
        string fileName,
        CancellationToken cancellationToken = default);
}

// Application/Documents/Models/DocumentParseResult.cs
public sealed class DocumentParseResult
{
    public required string Content { get; init; }
    public required DocumentMetadata Metadata { get; init; }
    public int PageCount { get; init; }
    public int CharacterCount { get; init; }
    public TimeSpan ParseDuration { get; init; }
}

public sealed class DocumentMetadata
{
    public string? Title { get; init; }
    public string? Author { get; init; }
    public DateTime? CreatedDate { get; init; }
    public DateTime? ModifiedDate { get; init; }
    public string? Language { get; init; }
    public Dictionary<string, string>? CustomProperties { get; init; }
}
```

**PDF 解析器實作**:
```csharp
// Infrastructure/Services/DocumentParsers/PdfDocumentParser.cs
public sealed class PdfDocumentParser : IDocumentParser
{
    private readonly ILogger<PdfDocumentParser> _logger;
    private readonly IOcrService _ocrService; // 可選的 OCR 支援

    public string[] SupportedExtensions => new[] { ".pdf" };

    public async Task<DocumentParseResult> ParseAsync(
        Stream fileStream,
        string fileName,
        CancellationToken cancellationToken = default)
    {
        var startTime = DateTime.UtcNow;

        try
        {
            using var pdfDocument = PdfDocument.Open(fileStream);
            var content = new StringBuilder();
            var metadata = ExtractMetadata(pdfDocument);

            foreach (var page in pdfDocument.GetPages())
            {
                var pageText = page.Text;

                // 如果頁面沒有文字（掃描的 PDF），嘗試 OCR
                if (string.IsNullOrWhiteSpace(pageText) && _ocrService != null)
                {
                    var pageImage = await RenderPageToImageAsync(page);
                    pageText = await _ocrService.RecognizeTextAsync(pageImage, cancellationToken);
                }

                content.AppendLine(pageText);
            }

            return new DocumentParseResult
            {
                Content = content.ToString(),
                Metadata = metadata,
                PageCount = pdfDocument.NumberOfPages,
                CharacterCount = content.Length,
                ParseDuration = DateTime.UtcNow - startTime
            };
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to parse PDF: {FileName}", fileName);
            throw new DocumentParseException($"PDF 解析失敗: {fileName}", ex);
        }
    }

    private DocumentMetadata ExtractMetadata(PdfDocument pdf)
    {
        var info = pdf.Information;

        return new DocumentMetadata
        {
            Title = info.Title,
            Author = info.Author,
            CreatedDate = info.CreationDate,
            ModifiedDate = info.ModifiedDate,
            CustomProperties = new Dictionary<string, string>
            {
                ["Producer"] = info.Producer ?? "",
                ["Creator"] = info.Creator ?? ""
            }
        };
    }

    private async Task<byte[]> RenderPageToImageAsync(Page page)
    {
        // 使用 PdfPig 或其他 PDF 渲染庫
        // 將頁面渲染為圖片供 OCR 使用
        // 實作細節略...
        throw new NotImplementedException("OCR support coming in Phase 2");
    }
}
```

**DOCX 解析器實作**:
```csharp
// Infrastructure/Services/DocumentParsers/DocxDocumentParser.cs
public sealed class DocxDocumentParser : IDocumentParser
{
    private readonly ILogger<DocxDocumentParser> _logger;

    public string[] SupportedExtensions => new[] { ".docx", ".doc" };

    public async Task<DocumentParseResult> ParseAsync(
        Stream fileStream,
        string fileName,
        CancellationToken cancellationToken = default)
    {
        var startTime = DateTime.UtcNow;

        try
        {
            using var document = WordprocessingDocument.Open(fileStream, false);
            var body = document.MainDocumentPart?.Document.Body;

            if (body == null)
            {
                throw new DocumentParseException("DOCX 文件不包含有效內容");
            }

            var content = new StringBuilder();
            var metadata = ExtractMetadata(document);

            // 提取段落文字
            foreach (var paragraph in body.Elements<Paragraph>())
            {
                var text = paragraph.InnerText;
                if (!string.IsNullOrWhiteSpace(text))
                {
                    content.AppendLine(text);
                }
            }

            // 提取表格內容
            foreach (var table in body.Elements<Table>())
            {
                foreach (var row in table.Elements<TableRow>())
                {
                    var rowText = string.Join(" | ",
                        row.Elements<TableCell>().Select(c => c.InnerText));
                    content.AppendLine(rowText);
                }
            }

            return new DocumentParseResult
            {
                Content = content.ToString(),
                Metadata = metadata,
                PageCount = EstimatePageCount(content.Length),
                CharacterCount = content.Length,
                ParseDuration = DateTime.UtcNow - startTime
            };
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to parse DOCX: {FileName}", fileName);
            throw new DocumentParseException($"DOCX 解析失敗: {fileName}", ex);
        }
    }

    private DocumentMetadata ExtractMetadata(WordprocessingDocument document)
    {
        var coreProperties = document.PackageProperties;

        return new DocumentMetadata
        {
            Title = coreProperties.Title,
            Author = coreProperties.Creator,
            CreatedDate = coreProperties.Created,
            ModifiedDate = coreProperties.Modified
        };
    }

    private int EstimatePageCount(int characterCount)
    {
        // 估算: 每頁約 2000 個字元
        return Math.Max(1, (int)Math.Ceiling(characterCount / 2000.0));
    }
}
```

**TXT/MD 解析器實作**:
```csharp
// Infrastructure/Services/DocumentParsers/TextDocumentParser.cs
public sealed class TextDocumentParser : IDocumentParser
{
    private readonly ILogger<TextDocumentParser> _logger;

    public string[] SupportedExtensions => new[] { ".txt", ".md" };

    public async Task<DocumentParseResult> ParseAsync(
        Stream fileStream,
        string fileName,
        CancellationToken cancellationToken = default)
    {
        var startTime = DateTime.UtcNow;

        try
        {
            using var reader = new StreamReader(fileStream, detectEncodingFromByteOrderMarks: true);
            var content = await reader.ReadToEndAsync();

            return new DocumentParseResult
            {
                Content = content,
                Metadata = new DocumentMetadata
                {
                    Title = Path.GetFileNameWithoutExtension(fileName)
                },
                PageCount = 1,
                CharacterCount = content.Length,
                ParseDuration = DateTime.UtcNow - startTime
            };
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to parse text file: {FileName}", fileName);
            throw new DocumentParseException($"文本檔案解析失敗: {fileName}", ex);
        }
    }
}
```

**Document Parser Factory**:
```csharp
// Infrastructure/Services/DocumentParsers/DocumentParserFactory.cs
public sealed class DocumentParserFactory : IDocumentParserFactory
{
    private readonly IEnumerable<IDocumentParser> _parsers;

    public DocumentParserFactory(IEnumerable<IDocumentParser> parsers)
    {
        _parsers = parsers;
    }

    public IDocumentParser GetParser(string fileName)
    {
        var extension = Path.GetExtension(fileName).ToLowerInvariant();

        var parser = _parsers.FirstOrDefault(p =>
            p.SupportedExtensions.Contains(extension));

        if (parser == null)
        {
            throw new UnsupportedFileTypeException(
                $"不支援的檔案類型: {extension}");
        }

        return parser;
    }
}
```

**參考文檔**:
- 📐 [Document Parsing Strategy](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md)
- 📦 [PdfPig Library](https://github.com/UglyToad/PdfPig)
- 📦 [DocumentFormat.OpenXml](https://github.com/OfficeDev/Open-XML-SDK)

---

##### US 4.1.3: 文檔分塊 (Chunking)

**IDocumentChunker 介面設計**:
```csharp
// Application/Interfaces/IDocumentChunker.cs
public interface IDocumentChunker
{
    /// <summary>
    /// 分塊策略名稱
    /// </summary>
    string StrategyName { get; }

    /// <summary>
    /// 將文檔內容切分為多個 Chunks
    /// </summary>
    Task<IReadOnlyList<DocumentChunk>> ChunkAsync(
        string content,
        ChunkingOptions options,
        CancellationToken cancellationToken = default);
}

// Application/Documents/Models/ChunkingOptions.cs
public sealed class ChunkingOptions
{
    public int ChunkSize { get; init; } = 1024; // tokens
    public int OverlapSize { get; init; } = 100; // tokens
    public bool PreserveSentenceBoundary { get; init; } = true;
    public bool PreserveParagraphBoundary { get; init; } = true;
}

// Domain/Entities/DocumentChunk.cs
public sealed class DocumentChunk
{
    public Guid Id { get; private set; }
    public Guid DocumentId { get; private set; }
    public int ChunkIndex { get; private set; }
    public string Content { get; private set; }
    public int TokenCount { get; private set; }
    public int StartPosition { get; private set; }
    public int EndPosition { get; private set; }
    public float[] Embedding { get; private set; } // 1536 維向量
    public DateTime CreatedAt { get; private set; }

    public static DocumentChunk Create(
        Guid documentId,
        int chunkIndex,
        string content,
        int tokenCount,
        int startPosition,
        int endPosition)
    {
        return new DocumentChunk
        {
            Id = Guid.NewGuid(),
            DocumentId = documentId,
            ChunkIndex = chunkIndex,
            Content = content,
            TokenCount = tokenCount,
            StartPosition = startPosition,
            EndPosition = endPosition,
            CreatedAt = DateTime.UtcNow
        };
    }

    public void SetEmbedding(float[] embedding)
    {
        if (embedding.Length != 1536)
        {
            throw new ArgumentException("Embedding 維度必須為 1536", nameof(embedding));
        }

        Embedding = embedding;
    }
}
```

**Fixed-Size Chunker 實作**:
```csharp
// Infrastructure/Services/DocumentChunkers/FixedSizeChunker.cs
public sealed class FixedSizeChunker : IDocumentChunker
{
    private readonly ITokenCounter _tokenCounter;
    private readonly ILogger<FixedSizeChunker> _logger;

    public string StrategyName => "FixedSize";

    public async Task<IReadOnlyList<DocumentChunk>> ChunkAsync(
        string content,
        ChunkingOptions options,
        CancellationToken cancellationToken = default)
    {
        var chunks = new List<DocumentChunk>();
        var sentences = SplitIntoSentences(content);

        var currentChunk = new StringBuilder();
        var currentTokenCount = 0;
        var chunkIndex = 0;
        var startPosition = 0;

        foreach (var sentence in sentences)
        {
            var sentenceTokenCount = await _tokenCounter.CountTokensAsync(sentence);

            // 如果加入這句話會超過 chunk size，則建立新 chunk
            if (currentTokenCount + sentenceTokenCount > options.ChunkSize && currentChunk.Length > 0)
            {
                var chunkContent = currentChunk.ToString().Trim();
                chunks.Add(new DocumentChunk
                {
                    ChunkIndex = chunkIndex++,
                    Content = chunkContent,
                    TokenCount = currentTokenCount,
                    StartPosition = startPosition,
                    EndPosition = startPosition + chunkContent.Length
                });

                // 處理 Overlap
                var overlapContent = GetOverlapContent(chunkContent, options.OverlapSize);
                currentChunk = new StringBuilder(overlapContent);
                currentTokenCount = await _tokenCounter.CountTokensAsync(overlapContent);
                startPosition += chunkContent.Length - overlapContent.Length;
            }

            currentChunk.Append(sentence).Append(" ");
            currentTokenCount += sentenceTokenCount;
        }

        // 最後一個 chunk
        if (currentChunk.Length > 0)
        {
            var chunkContent = currentChunk.ToString().Trim();
            chunks.Add(new DocumentChunk
            {
                ChunkIndex = chunkIndex,
                Content = chunkContent,
                TokenCount = currentTokenCount,
                StartPosition = startPosition,
                EndPosition = startPosition + chunkContent.Length
            });
        }

        _logger.LogInformation(
            "Chunked document into {ChunkCount} chunks with average size {AvgSize} tokens",
            chunks.Count,
            chunks.Count > 0 ? chunks.Average(c => c.TokenCount) : 0);

        return chunks;
    }

    private IEnumerable<string> SplitIntoSentences(string content)
    {
        // 簡化的句子分割邏輯（生產環境可考慮使用 NLP 庫）
        var sentences = content.Split(
            new[] { '.', '!', '?', '。', '！', '？', '\n' },
            StringSplitOptions.RemoveEmptyEntries);

        return sentences.Select(s => s.Trim()).Where(s => !string.IsNullOrEmpty(s));
    }

    private string GetOverlapContent(string content, int overlapTokens)
    {
        if (overlapTokens <= 0) return string.Empty;

        // 從末尾取 overlapTokens 數量的文字
        var words = content.Split(' ', StringSplitOptions.RemoveEmptyEntries);
        var overlapWords = words.TakeLast(Math.Min(overlapTokens, words.Length));
        return string.Join(" ", overlapWords);
    }
}
```

**Token Counter 實作**:
```csharp
// Infrastructure/Services/TikTokenCounter.cs
public sealed class TikTokenCounter : ITokenCounter
{
    private readonly TikTokenizer _tokenizer;

    public TikTokenCounter()
    {
        // 使用 cl100k_base encoding（GPT-3.5, GPT-4）
        _tokenizer = TikTokenizer.CreateForModel("gpt-3.5-turbo");
    }

    public Task<int> CountTokensAsync(string text)
    {
        var tokens = _tokenizer.Encode(text);
        return Task.FromResult(tokens.Count);
    }

    public Task<IReadOnlyList<int>> EncodeAsync(string text)
    {
        var tokens = _tokenizer.Encode(text);
        return Task.FromResult<IReadOnlyList<int>>(tokens);
    }
}
```

**參考文檔**:
- 📐 [Chunking Strategies](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md)
- 📦 [TikToken Library](https://github.com/microsoft/Tokenizer)
- 📚 [Chunking Best Practices](https://www.pinecone.io/learn/chunking-strategies/)

---

##### US 4.1.4: 向量化 (Embedding)

**IEmbeddingService 介面設計**:
```csharp
// Application/Interfaces/IEmbeddingService.cs
public interface IEmbeddingService
{
    /// <summary>
    /// 生成單個文本的向量表示
    /// </summary>
    Task<float[]> GenerateEmbeddingAsync(
        string text,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 批量生成向量（提升效率）
    /// </summary>
    Task<IReadOnlyList<float[]>> GenerateBatchEmbeddingsAsync(
        IEnumerable<string> texts,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 向量維度
    /// </summary>
    int Dimensions { get; }
}
```

**OpenAI Embedding Service 實作**:
```csharp
// Infrastructure/Services/OpenAIEmbeddingService.cs
public sealed class OpenAIEmbeddingService : IEmbeddingService
{
    private readonly OpenAIClient _client;
    private readonly IMemoryCache _cache;
    private readonly ILogger<OpenAIEmbeddingService> _logger;
    private const string ModelName = "text-embedding-ada-002";

    public int Dimensions => 1536;

    public OpenAIEmbeddingService(
        OpenAIClient client,
        IMemoryCache cache,
        ILogger<OpenAIEmbeddingService> logger)
    {
        _client = client;
        _cache = cache;
        _logger = logger;
    }

    public async Task<float[]> GenerateEmbeddingAsync(
        string text,
        CancellationToken cancellationToken = default)
    {
        // 檢查快取
        var cacheKey = $"embedding:{ComputeHash(text)}";
        if (_cache.TryGetValue<float[]>(cacheKey, out var cachedEmbedding))
        {
            _logger.LogDebug("Using cached embedding for text hash: {Hash}", cacheKey);
            return cachedEmbedding!;
        }

        try
        {
            var embeddingOptions = new EmbeddingsOptions(ModelName, new[] { text });
            var response = await _client.GetEmbeddingsAsync(embeddingOptions, cancellationToken);
            var embedding = response.Value.Data[0].Embedding.ToArray();

            // 快取向量（1 小時）
            _cache.Set(cacheKey, embedding, TimeSpan.FromHours(1));

            _logger.LogInformation("Generated embedding for text (length: {Length})", text.Length);
            return embedding;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to generate embedding");
            throw new EmbeddingGenerationException("向量生成失敗", ex);
        }
    }

    public async Task<IReadOnlyList<float[]>> GenerateBatchEmbeddingsAsync(
        IEnumerable<string> texts,
        CancellationToken cancellationToken = default)
    {
        var textList = texts.ToList();
        var embeddings = new List<float[]>();

        // OpenAI API 限制: 每次最多 2048 個輸入
        const int batchSize = 100;

        for (int i = 0; i < textList.Count; i += batchSize)
        {
            var batch = textList.Skip(i).Take(batchSize).ToList();

            try
            {
                var embeddingOptions = new EmbeddingsOptions(ModelName, batch);
                var response = await _client.GetEmbeddingsAsync(embeddingOptions, cancellationToken);

                foreach (var data in response.Value.Data)
                {
                    embeddings.Add(data.Embedding.ToArray());
                }

                _logger.LogInformation(
                    "Generated {Count} embeddings in batch (batch {Current}/{Total})",
                    batch.Count, i / batchSize + 1, (textList.Count + batchSize - 1) / batchSize);

                // 避免超過 Rate Limit
                await Task.Delay(100, cancellationToken);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Failed to generate batch embeddings");
                throw new EmbeddingGenerationException($"批量向量生成失敗 (batch {i / batchSize + 1})", ex);
            }
        }

        return embeddings;
    }

    private static string ComputeHash(string text)
    {
        using var sha256 = SHA256.Create();
        var bytes = Encoding.UTF8.GetBytes(text);
        var hash = sha256.ComputeHash(bytes);
        return Convert.ToBase64String(hash);
    }
}
```

**參考文檔**:
- 📐 [Embedding Service](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md)
- 📚 [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings)
- 📚 [Azure OpenAI Embeddings](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/embeddings)

---

##### US 4.1.5: 向量索引 (Qdrant)

**IVectorStoreService 介面設計**:
```csharp
// Application/Interfaces/IVectorStoreService.cs
public interface IVectorStoreService
{
    /// <summary>
    /// 建立 Collection
    /// </summary>
    Task CreateCollectionAsync(
        string collectionName,
        int vectorSize,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 插入向量
    /// </summary>
    Task UpsertVectorsAsync(
        string collectionName,
        IEnumerable<VectorRecord> records,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 刪除向量
    /// </summary>
    Task DeleteVectorsAsync(
        string collectionName,
        IEnumerable<Guid> ids,
        CancellationToken cancellationToken = default);

    /// <summary>
    /// 向量搜尋
    /// </summary>
    Task<IReadOnlyList<SearchResult>> SearchAsync(
        string collectionName,
        float[] queryVector,
        int limit = 10,
        float scoreThreshold = 0.7f,
        Dictionary<string, object>? filter = null,
        CancellationToken cancellationToken = default);
}

// Application/Documents/Models/VectorRecord.cs
public sealed class VectorRecord
{
    public required Guid Id { get; init; }
    public required float[] Vector { get; init; }
    public required Dictionary<string, object> Metadata { get; init; }
}

// Application/Documents/Models/SearchResult.cs
public sealed class SearchResult
{
    public required Guid Id { get; init; }
    public required float Score { get; init; }
    public required Dictionary<string, object> Metadata { get; init; }
}
```

**Qdrant Service 實作**:
```csharp
// Infrastructure/Services/QdrantVectorStoreService.cs
public sealed class QdrantVectorStoreService : IVectorStoreService
{
    private readonly QdrantClient _client;
    private readonly ILogger<QdrantVectorStoreService> _logger;

    public QdrantVectorStoreService(
        QdrantClient client,
        ILogger<QdrantVectorStoreService> logger)
    {
        _client = client;
        _logger = logger;
    }

    public async Task CreateCollectionAsync(
        string collectionName,
        int vectorSize,
        CancellationToken cancellationToken = default)
    {
        try
        {
            // 檢查 Collection 是否已存在
            var collections = await _client.ListCollectionsAsync(cancellationToken);
            if (collections.Any(c => c.Name == collectionName))
            {
                _logger.LogInformation("Collection {CollectionName} already exists", collectionName);
                return;
            }

            // 建立 Collection（使用 HNSW 索引）
            await _client.CreateCollectionAsync(
                collectionName,
                new VectorParams
                {
                    Size = (ulong)vectorSize,
                    Distance = Distance.Cosine, // Cosine Similarity
                    HnswConfig = new HnswConfigDiff
                    {
                        M = 16,  // 鄰居數量（越大越精確但越慢）
                        EfConstruct = 100  // 索引建立時的搜尋深度
                    }
                },
                cancellationToken: cancellationToken);

            _logger.LogInformation(
                "Created Qdrant collection: {CollectionName} with vector size {VectorSize}",
                collectionName, vectorSize);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to create Qdrant collection: {CollectionName}", collectionName);
            throw new VectorStoreException($"建立 Collection 失敗: {collectionName}", ex);
        }
    }

    public async Task UpsertVectorsAsync(
        string collectionName,
        IEnumerable<VectorRecord> records,
        CancellationToken cancellationToken = default)
    {
        try
        {
            var points = records.Select(r => new PointStruct
            {
                Id = new PointId { Uuid = r.Id.ToString() },
                Vectors = r.Vector,
                Payload = r.Metadata.ToDictionary(
                    kvp => kvp.Key,
                    kvp => new Value { StringValue = kvp.Value?.ToString() })
            }).ToList();

            await _client.UpsertAsync(
                collectionName,
                points,
                cancellationToken: cancellationToken);

            _logger.LogInformation(
                "Upserted {Count} vectors to collection: {CollectionName}",
                points.Count, collectionName);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to upsert vectors to collection: {CollectionName}", collectionName);
            throw new VectorStoreException($"向量插入失敗: {collectionName}", ex);
        }
    }

    public async Task DeleteVectorsAsync(
        string collectionName,
        IEnumerable<Guid> ids,
        CancellationToken cancellationToken = default)
    {
        try
        {
            var pointIds = ids.Select(id => new PointId { Uuid = id.ToString() }).ToList();

            await _client.DeleteAsync(
                collectionName,
                pointIds,
                cancellationToken: cancellationToken);

            _logger.LogInformation(
                "Deleted {Count} vectors from collection: {CollectionName}",
                pointIds.Count, collectionName);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to delete vectors from collection: {CollectionName}", collectionName);
            throw new VectorStoreException($"向量刪除失敗: {collectionName}", ex);
        }
    }

    public async Task<IReadOnlyList<SearchResult>> SearchAsync(
        string collectionName,
        float[] queryVector,
        int limit = 10,
        float scoreThreshold = 0.7f,
        Dictionary<string, object>? filter = null,
        CancellationToken cancellationToken = default)
    {
        try
        {
            var searchParams = new SearchParams
            {
                HnswEf = 128, // 搜尋時的深度（越大越精確但越慢）
                Exact = false // 使用近似搜尋（HNSW）
            };

            Filter? qdrantFilter = null;
            if (filter != null && filter.Any())
            {
                // 建立 Qdrant 過濾條件
                qdrantFilter = BuildFilter(filter);
            }

            var searchResults = await _client.SearchAsync(
                collectionName,
                queryVector,
                limit: (ulong)limit,
                scoreThreshold: scoreThreshold,
                filter: qdrantFilter,
                searchParams: searchParams,
                cancellationToken: cancellationToken);

            var results = searchResults.Select(r => new SearchResult
            {
                Id = Guid.Parse(r.Id.Uuid),
                Score = r.Score,
                Metadata = r.Payload.ToDictionary(
                    kvp => kvp.Key,
                    kvp => (object)(kvp.Value.StringValue ?? ""))
            }).ToList();

            _logger.LogInformation(
                "Searched collection {CollectionName}, found {Count} results",
                collectionName, results.Count);

            return results;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to search collection: {CollectionName}", collectionName);
            throw new VectorStoreException($"向量搜尋失敗: {collectionName}", ex);
        }
    }

    private Filter BuildFilter(Dictionary<string, object> filter)
    {
        // 簡化的過濾條件建立（實際生產環境需要更複雜的邏輯）
        var conditions = filter.Select(kvp => new Condition
        {
            Field = new FieldCondition
            {
                Key = kvp.Key,
                Match = new Match { Keyword = kvp.Value.ToString() }
            }
        }).ToList();

        return new Filter
        {
            Must = { conditions }
        };
    }
}
```

**參考文檔**:
- 📐 [Qdrant Design](../../../docs/database/qdrant-design.md)
- 📚 [Qdrant Documentation](https://qdrant.tech/documentation/)
- 📚 [HNSW Algorithm](https://arxiv.org/abs/1603.09320)

---

##### US 4.1.6: 背景處理任務

**Document Processing Job**:
```csharp
// Infrastructure/BackgroundJobs/DocumentProcessingJob.cs
public sealed class DocumentProcessingJob : IDocumentProcessingJob
{
    private readonly IDocumentRepository _documentRepo;
    private readonly IBlobStorageService _blobStorage;
    private readonly IDocumentParserFactory _parserFactory;
    private readonly IDocumentChunker _chunker;
    private readonly IEmbeddingService _embeddingService;
    private readonly IVectorStoreService _vectorStore;
    private readonly ILogger<DocumentProcessingJob> _logger;

    public async Task ProcessDocumentAsync(
        Guid documentId,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation("Starting document processing for ID: {DocumentId}", documentId);

        try
        {
            // 1. 載入 Document Entity
            var document = await _documentRepo.GetByIdAsync(documentId, cancellationToken);
            if (document == null)
            {
                _logger.LogError("Document not found: {DocumentId}", documentId);
                return;
            }

            document.UpdateStatus(DocumentStatus.Processing);
            await _documentRepo.SaveChangesAsync(cancellationToken);

            // 2. 從 Blob Storage 下載檔案
            _logger.LogInformation("Downloading file from blob storage: {BlobUri}", document.BlobUri);
            await using var fileStream = await _blobStorage.DownloadAsync(
                document.BlobUri, cancellationToken);

            // 3. 解析文檔
            _logger.LogInformation("Parsing document: {FileName}", document.FileName);
            var parser = _parserFactory.GetParser(document.FileName);
            var parseResult = await parser.ParseAsync(fileStream, document.FileName, cancellationToken);

            document.UpdateMetadata(parseResult.Metadata);
            document.UpdatePageCount(parseResult.PageCount);

            // 4. 文檔分塊
            _logger.LogInformation("Chunking document: {DocumentId}", documentId);
            var chunkingOptions = new ChunkingOptions
            {
                ChunkSize = 1024,
                OverlapSize = 100,
                PreserveSentenceBoundary = true
            };

            var chunks = await _chunker.ChunkAsync(
                parseResult.Content, chunkingOptions, cancellationToken);

            _logger.LogInformation("Created {ChunkCount} chunks for document {DocumentId}",
                chunks.Count, documentId);

            // 5. 生成向量（批量）
            _logger.LogInformation("Generating embeddings for {ChunkCount} chunks", chunks.Count);
            var chunkTexts = chunks.Select(c => c.Content).ToList();
            var embeddings = await _embeddingService.GenerateBatchEmbeddingsAsync(
                chunkTexts, cancellationToken);

            // 6. 關聯向量與 Chunks
            for (int i = 0; i < chunks.Count; i++)
            {
                chunks[i].SetEmbedding(embeddings[i]);
            }

            // 7. 儲存到 Qdrant
            _logger.LogInformation("Storing vectors in Qdrant for document {DocumentId}", documentId);
            var collectionName = $"agent_{document.AgentId}";

            // 確保 Collection 存在
            await _vectorStore.CreateCollectionAsync(
                collectionName, _embeddingService.Dimensions, cancellationToken);

            // 插入向量
            var vectorRecords = chunks.Select(chunk => new VectorRecord
            {
                Id = chunk.Id,
                Vector = chunk.Embedding,
                Metadata = new Dictionary<string, object>
                {
                    ["document_id"] = document.Id.ToString(),
                    ["chunk_index"] = chunk.ChunkIndex,
                    ["content"] = chunk.Content,
                    ["token_count"] = chunk.TokenCount
                }
            }).ToList();

            await _vectorStore.UpsertVectorsAsync(collectionName, vectorRecords, cancellationToken);

            // 8. 更新文檔狀態
            document.UpdateStatus(DocumentStatus.Completed);
            document.UpdateChunkCount(chunks.Count);
            await _documentRepo.SaveChangesAsync(cancellationToken);

            _logger.LogInformation(
                "Document processing completed successfully for ID: {DocumentId}, Chunks: {ChunkCount}",
                documentId, chunks.Count);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Document processing failed for ID: {DocumentId}", documentId);

            // 更新文檔狀態為失敗
            var document = await _documentRepo.GetByIdAsync(documentId, cancellationToken);
            if (document != null)
            {
                document.UpdateStatus(DocumentStatus.Failed);
                document.UpdateErrorMessage(ex.Message);
                await _documentRepo.SaveChangesAsync(cancellationToken);
            }

            throw;
        }
    }
}
```

**Hangfire 配置**:
```csharp
// API/Program.cs - Hangfire 註冊
builder.Services.AddHangfire(config =>
{
    config.UsePostgreSqlStorage(builder.Configuration.GetConnectionString("DefaultConnection"));
    config.UseRecommendedSerializerSettings();
});

builder.Services.AddHangfireServer(options =>
{
    options.WorkerCount = 5; // 同時處理 5 個任務
    options.Queues = new[] { "document-processing", "default" };
});

// 註冊背景任務
builder.Services.AddScoped<IDocumentProcessingJob, DocumentProcessingJob>();

// 啟用 Hangfire Dashboard
app.UseHangfireDashboard("/hangfire", new DashboardOptions
{
    Authorization = new[] { new HangfireAuthorizationFilter() }
});
```

**參考文檔**:
- 📐 [Background Jobs](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md)
- 📦 [Hangfire](https://www.hangfire.io/)
- 📚 [Hangfire Best Practices](https://docs.hangfire.io/en/latest/best-practices.html)

---

### US 4.2: RAG 檢索策略配置 (5 SP)

**User Story 完整規格**: [US 5.2 - 精確檢索策略配置](../../../docs/user-stories/modules/module-05-agent-memory.md#us-52)

#### 一、MVP 範圍定義

**必須實現功能 (P0 - 本 Sprint)**:
- [x] **語義搜尋 (Semantic Search)**: 向量相似度檢索
  - Qdrant Vector Search
  - Cosine Similarity
  - 相似度閾值: 0.7-0.9（可配置）
  - Top-K 結果: 1-20（可配置）
  - **參考**: [Semantic Search](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md)

- [x] **關鍵字搜尋 (Keyword Search)**: BM25 演算法
  - 全文檢索（PostgreSQL FTS）
  - BM25 演算法實作
  - 關鍵詞權重調整
  - 同義詞支援
  - **參考**: [Keyword Search](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md)

- [x] **混合檢索 (Hybrid Search)**: Semantic + Keyword
  - RRF (Reciprocal Rank Fusion) 演算法
  - 權重配置: Semantic 70% + Keyword 30%（可調整）
  - 結果去重與排序
  - **參考**: [Hybrid Search](../../../docs/technical-implementation/01-backend-net9/17-hybrid-search.md)

- [x] **Re-ranking**: Cross-Encoder 重排序
  - HuggingFace Cross-Encoder 模型
  - 查詢與文檔的相關性評分
  - 重排序 Top-K 結果
  - **參考**: [Re-ranking Strategy](../../../docs/technical-implementation/01-backend-net9/18-reranking.md)

- [x] **檢索準確率測試框架**: 自動化評估
  - 標準測試集（100+ Q&A pairs）
  - 自動化評估（MRR, NDCG）
  - 準確率目標: ≥90%
  - **參考**: [RAG Testing](../../../docs/testing/rag-accuracy-testing.md)

**明確排除 Phase 2 功能 (延後)**:
- ❌ **Query Expansion**: 查詢擴展與改寫 - 延後到 Phase 2
- ❌ **Contextual Compression**: 上下文壓縮 - 延後到 Phase 2
- ❌ **Multi-Query Retrieval**: 多查詢檢索 - 延後到 Sprint 6
- ❌ **Parent Document Retriever**: 父文檔檢索器 - 延後到 Phase 2

**MVP 範圍參考**:
- 📖 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md#rag-retrieval) - RAG 在 Phase 1A 的範圍
- 📖 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-5) - Sprint 5 RAG 任務

#### 二、詳細技術規格

##### US 4.2.1: 語義搜尋 (Semantic Search)

**ISemanticSearchService 介面設計**:
```csharp
// Application/Interfaces/ISemanticSearchService.cs
public interface ISemanticSearchService
{
    /// <summary>
    /// 執行語義搜尋
    /// </summary>
    Task<IReadOnlyList<SearchResultDto>> SearchAsync(
        string query,
        Guid agentId,
        SemanticSearchOptions options,
        CancellationToken cancellationToken = default);
}

// Application/RAG/Models/SemanticSearchOptions.cs
public sealed class SemanticSearchOptions
{
    public int TopK { get; init; } = 10;
    public float ScoreThreshold { get; init; } = 0.7f;
    public Dictionary<string, object>? Metadata Filter { get; init; }
}

// Application/RAG/DTOs/SearchResultDto.cs
public sealed class SearchResultDto
{
    public required Guid ChunkId { get; init; }
    public required Guid DocumentId { get; init; }
    public required string Content { get; init; }
    public required float Score { get; init; }
    public int ChunkIndex { get; init; }
    public Dictionary<string, object>? Metadata { get; init; }
}
```

**Semantic Search Service 實作**:
```csharp
// Infrastructure/Services/SemanticSearchService.cs
public sealed class SemanticSearchService : ISemanticSearchService
{
    private readonly IEmbeddingService _embeddingService;
    private readonly IVectorStoreService _vectorStore;
    private readonly ILogger<SemanticSearchService> _logger;

    public async Task<IReadOnlyList<SearchResultDto>> SearchAsync(
        string query,
        Guid agentId,
        SemanticSearchOptions options,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation(
            "Performing semantic search for Agent {AgentId}, Query: {Query}",
            agentId, query);

        try
        {
            // 1. 生成查詢向量
            var queryEmbedding = await _embeddingService.GenerateEmbeddingAsync(
                query, cancellationToken);

            // 2. Qdrant 向量搜尋
            var collectionName = $"agent_{agentId}";
            var searchResults = await _vectorStore.SearchAsync(
                collectionName,
                queryEmbedding,
                limit: options.TopK,
                scoreThreshold: options.ScoreThreshold,
                filter: options.MetadataFilter,
                cancellationToken: cancellationToken);

            // 3. 轉換為 DTO
            var results = searchResults.Select(r => new SearchResultDto
            {
                ChunkId = r.Id,
                DocumentId = Guid.Parse(r.Metadata["document_id"]?.ToString() ?? ""),
                Content = r.Metadata["content"]?.ToString() ?? "",
                Score = r.Score,
                ChunkIndex = int.Parse(r.Metadata["chunk_index"]?.ToString() ?? "0"),
                Metadata = r.Metadata
            }).ToList();

            _logger.LogInformation(
                "Semantic search completed, found {Count} results with scores >= {Threshold}",
                results.Count, options.ScoreThreshold);

            return results;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Semantic search failed for Agent {AgentId}", agentId);
            throw new SearchException("語義搜尋失敗", ex);
        }
    }
}
```

**參考文檔**:
- 📐 [Semantic Search](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md)
- 📚 [Vector Search Best Practices](https://qdrant.tech/documentation/concepts/search/)

---

##### US 4.2.2: 關鍵字搜尋 (Keyword Search)

**IKeywordSearchService 介面設計**:
```csharp
// Application/Interfaces/IKeywordSearchService.cs
public interface IKeywordSearchService
{
    /// <summary>
    /// 執行關鍵字搜尋（BM25）
    /// </summary>
    Task<IReadOnlyList<SearchResultDto>> SearchAsync(
        string query,
        Guid agentId,
        KeywordSearchOptions options,
        CancellationToken cancellationToken = default);
}

// Application/RAG/Models/KeywordSearchOptions.cs
public sealed class KeywordSearchOptions
{
    public int TopK { get; init; } = 10;
    public bool UseSynonyms { get; init; } = true;
    public Dictionary<string, float>? TermBoosts { get; init; }
}
```

**PostgreSQL Full-Text Search 實作**:
```csharp
// Infrastructure/Services/KeywordSearchService.cs
public sealed class KeywordSearchService : IKeywordSearchService
{
    private readonly ApplicationDbContext _dbContext;
    private readonly ILogger<KeywordSearchService> _logger;

    public async Task<IReadOnlyList<SearchResultDto>> SearchAsync(
        string query,
        Guid agentId,
        KeywordSearchOptions options,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation(
            "Performing keyword search for Agent {AgentId}, Query: {Query}",
            agentId, query);

        try
        {
            // 1. 預處理查詢（分詞、移除停用詞）
            var processedQuery = PreprocessQuery(query);

            // 2. PostgreSQL FTS 查詢（使用 BM25 演算法）
            var results = await _dbContext.DocumentChunks
                .Where(c => c.Document.AgentId == agentId)
                .Select(c => new
                {
                    Chunk = c,
                    Rank = EF.Functions.FullTextSearchRank(
                        c.ContentVector,
                        EF.Functions.ToTsQuery("english", processedQuery))
                })
                .Where(x => x.Rank > 0)
                .OrderByDescending(x => x.Rank)
                .Take(options.TopK)
                .ToListAsync(cancellationToken);

            // 3. 轉換為 DTO
            var searchResults = results.Select(r => new SearchResultDto
            {
                ChunkId = r.Chunk.Id,
                DocumentId = r.Chunk.DocumentId,
                Content = r.Chunk.Content,
                Score = r.Rank,
                ChunkIndex = r.Chunk.ChunkIndex,
                Metadata = new Dictionary<string, object>
                {
                    ["document_id"] = r.Chunk.DocumentId.ToString(),
                    ["chunk_index"] = r.Chunk.ChunkIndex
                }
            }).ToList();

            _logger.LogInformation(
                "Keyword search completed, found {Count} results",
                searchResults.Count);

            return searchResults;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Keyword search failed for Agent {AgentId}", agentId);
            throw new SearchException("關鍵字搜尋失敗", ex);
        }
    }

    private string PreprocessQuery(string query)
    {
        // 1. 分詞
        var words = query.Split(' ', StringSplitOptions.RemoveEmptyEntries);

        // 2. 移除停用詞
        var stopWords = new HashSet<string> { "the", "a", "an", "is", "are", "of", "in" };
        var filteredWords = words.Where(w => !stopWords.Contains(w.ToLower()));

        // 3. 組合為 tsquery 格式
        return string.Join(" & ", filteredWords);
    }
}
```

**PostgreSQL FTS 索引配置**:
```sql
-- Migration: Add Full-Text Search Index
ALTER TABLE document_chunks
ADD COLUMN content_vector tsvector
GENERATED ALWAYS AS (to_tsvector('english', content)) STORED;

CREATE INDEX idx_document_chunks_content_fts
ON document_chunks USING GIN (content_vector);
```

**參考文檔**:
- 📐 [Keyword Search](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md)
- 📚 [PostgreSQL Full-Text Search](https://www.postgresql.org/docs/current/textsearch.html)
- 📚 [BM25 Algorithm](https://en.wikipedia.org/wiki/Okapi_BM25)

---

##### US 4.2.3: 混合檢索 (Hybrid Search) + Re-ranking

**IHybridSearchService 介面設計**:
```csharp
// Application/Interfaces/IHybridSearchService.cs
public interface IHybridSearchService
{
    /// <summary>
    /// 執行混合檢索（Semantic + Keyword + Re-ranking）
    /// </summary>
    Task<IReadOnlyList<SearchResultDto>> SearchAsync(
        string query,
        Guid agentId,
        HybridSearchOptions options,
        CancellationToken cancellationToken = default);
}

// Application/RAG/Models/HybridSearchOptions.cs
public sealed class HybridSearchOptions
{
    public int TopK { get; init; } = 10;
    public float SemanticWeight { get; init; } = 0.7f;
    public float KeywordWeight { get; init; } = 0.3f;
    public bool EnableReranking { get; init; } = true;
    public float SemanticScoreThreshold { get; init; } = 0.7f;
}
```

**Hybrid Search Service 實作**:
```csharp
// Infrastructure/Services/HybridSearchService.cs
public sealed class HybridSearchService : IHybridSearchService
{
    private readonly ISemanticSearchService _semanticSearch;
    private readonly IKeywordSearchService _keywordSearch;
    private readonly IReranker _reranker;
    private readonly ILogger<HybridSearchService> _logger;

    public async Task<IReadOnlyList<SearchResultDto>> SearchAsync(
        string query,
        Guid agentId,
        HybridSearchOptions options,
        CancellationToken cancellationToken = default)
    {
        _logger.LogInformation(
            "Performing hybrid search for Agent {AgentId}, Query: {Query}",
            agentId, query);

        try
        {
            // 1. 並行執行語義搜尋和關鍵字搜尋
            var semanticTask = _semanticSearch.SearchAsync(
                query, agentId,
                new SemanticSearchOptions
                {
                    TopK = options.TopK * 2, // 取兩倍結果用於融合
                    ScoreThreshold = options.SemanticScoreThreshold
                },
                cancellationToken);

            var keywordTask = _keywordSearch.SearchAsync(
                query, agentId,
                new KeywordSearchOptions { TopK = options.TopK * 2 },
                cancellationToken);

            await Task.WhenAll(semanticTask, keywordTask);

            var semanticResults = await semanticTask;
            var keywordResults = await keywordTask;

            _logger.LogInformation(
                "Retrieved {SemanticCount} semantic results and {KeywordCount} keyword results",
                semanticResults.Count, keywordResults.Count);

            // 2. RRF (Reciprocal Rank Fusion) 融合
            var fusedResults = FuseResultsWithRRF(
                semanticResults,
                keywordResults,
                options.SemanticWeight,
                options.KeywordWeight,
                options.TopK);

            _logger.LogInformation("Fused results using RRF, total: {Count}", fusedResults.Count);

            // 3. Re-ranking（可選）
            if (options.EnableReranking)
            {
                fusedResults = await _reranker.RerankAsync(
                    query, fusedResults, cancellationToken);

                _logger.LogInformation("Re-ranked results using Cross-Encoder");
            }

            return fusedResults.Take(options.TopK).ToList();
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Hybrid search failed for Agent {AgentId}", agentId);
            throw new SearchException("混合檢索失敗", ex);
        }
    }

    private List<SearchResultDto> FuseResultsWithRRF(
        IReadOnlyList<SearchResultDto> semanticResults,
        IReadOnlyList<SearchResultDto> keywordResults,
        float semanticWeight,
        float keywordWeight,
        int topK)
    {
        const int k = 60; // RRF 常數

        // 計算每個結果的 RRF 分數
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
            .Select(kvp => new SearchResultDto
            {
                ChunkId = kvp.Key,
                DocumentId = kvp.Value.Result.DocumentId,
                Content = kvp.Value.Result.Content,
                Score = kvp.Value.Score, // RRF 分數
                ChunkIndex = kvp.Value.Result.ChunkIndex,
                Metadata = kvp.Value.Result.Metadata
            })
            .Take(topK * 2) // 取兩倍結果供 Re-ranking 使用
            .ToList();
    }
}
```

**Cross-Encoder Re-ranker 實作**:
```csharp
// Infrastructure/Services/CrossEncoderReranker.cs
public sealed class CrossEncoderReranker : IReranker
{
    private readonly HttpClient _httpClient;
    private readonly ILogger<CrossEncoderReranker> _logger;

    public async Task<List<SearchResultDto>> RerankAsync(
        string query,
        IReadOnlyList<SearchResultDto> results,
        CancellationToken cancellationToken = default)
    {
        if (!results.Any()) return new List<SearchResultDto>();

        try
        {
            // 呼叫 HuggingFace API 或本地模型
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
}
```

**參考文檔**:
- 📐 [Hybrid Search](../../../docs/technical-implementation/01-backend-net9/17-hybrid-search.md)
- 📐 [Re-ranking Strategy](../../../docs/technical-implementation/01-backend-net9/18-reranking.md)
- 📚 [RRF Algorithm](https://plg.uwaterloo.ca/~gvcormac/cormacksigir09-rrf.pdf)
- 📚 [Cross-Encoder Models](https://www.sbert.net/examples/applications/cross-encoder/README.html)

---

##### US 4.2.4: 檢索準確率測試框架

**測試集格式**:
```json
// test-dataset.json
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
    },
    {
      "id": "q002",
      "question": "Plugin 熱重載的原理是什麼？",
      "expected_answer": "使用 AssemblyLoadContext 實現動態載入與卸載",
      "expected_sources": ["plugin-architecture.md"],
      "difficulty": "hard"
    }
  ]
}
```

**評估指標計算**:
```csharp
// Infrastructure/Services/RAGEvaluationService.cs
public sealed class RAGEvaluationService : IRAGEvaluationService
{
    private readonly IHybridSearchService _hybridSearch;
    private readonly ILogger<RAGEvaluationService> _logger;

    public async Task<EvaluationReport> EvaluateAsync(
        string testDatasetPath,
        Guid agentId,
        HybridSearchOptions searchOptions,
        CancellationToken cancellationToken = default)
    {
        // 1. 載入測試集
        var dataset = await LoadTestDatasetAsync(testDatasetPath);

        var totalQuestions = dataset.Questions.Count;
        var correctResults = 0;
        var mrrSum = 0.0;
        var ndcgSum = 0.0;

        foreach (var question in dataset.Questions)
        {
            // 2. 執行搜尋
            var searchResults = await _hybridSearch.SearchAsync(
                question.Question,
                agentId,
                searchOptions,
                cancellationToken);

            // 3. 評估結果
            var evaluation = EvaluateQuestionResult(question, searchResults);

            if (evaluation.IsCorrect) correctResults++;
            mrrSum += evaluation.MRR;
            ndcgSum += evaluation.NDCG;

            _logger.LogInformation(
                "Question {Id}: MRR={MRR:F3}, NDCG={NDCG:F3}, Correct={IsCorrect}",
                question.Id, evaluation.MRR, evaluation.NDCG, evaluation.IsCorrect);
        }

        // 4. 計算整體指標
        var accuracy = (double)correctResults / totalQuestions * 100;
        var avgMRR = mrrSum / totalQuestions;
        var avgNDCG = ndcgSum / totalQuestions;

        var report = new EvaluationReport
        {
            TotalQuestions = totalQuestions,
            CorrectResults = correctResults,
            Accuracy = accuracy,
            MeanReciprocalRank = avgMRR,
            NormalizedDiscountedCumulativeGain = avgNDCG,
            PassThreshold = accuracy >= 90.0
        };

        _logger.LogInformation(
            "Evaluation completed: Accuracy={Accuracy:F2}%, MRR={MRR:F3}, NDCG={NDCG:F3}",
            accuracy, avgMRR, avgNDCG);

        return report;
    }

    private QuestionEvaluation EvaluateQuestionResult(
        TestQuestion question,
        IReadOnlyList<SearchResultDto> searchResults)
    {
        // MRR: Mean Reciprocal Rank
        double mrr = 0.0;
        for (int i = 0; i < searchResults.Count; i++)
        {
            if (question.ExpectedChunkIds.Contains(searchResults[i].ChunkId))
            {
                mrr = 1.0 / (i + 1);
                break;
            }
        }

        // NDCG: Normalized Discounted Cumulative Gain
        double dcg = 0.0;
        for (int i = 0; i < searchResults.Count; i++)
        {
            int relevance = question.ExpectedChunkIds.Contains(searchResults[i].ChunkId) ? 1 : 0;
            dcg += relevance / Math.Log2(i + 2);
        }

        // IDCG: Ideal DCG
        double idcg = 0.0;
        for (int i = 0; i < Math.Min(question.ExpectedChunkIds.Count, searchResults.Count); i++)
        {
            idcg += 1.0 / Math.Log2(i + 2);
        }

        double ndcg = idcg > 0 ? dcg / idcg : 0.0;

        // 判定為正確：Top-1 或 Top-3 包含預期結果
        bool isCorrect = searchResults.Take(3).Any(r =>
            question.ExpectedChunkIds.Contains(r.ChunkId));

        return new QuestionEvaluation
        {
            MRR = mrr,
            NDCG = ndcg,
            IsCorrect = isCorrect
        };
    }

    private async Task<TestDataset> LoadTestDatasetAsync(string path)
    {
        var json = await File.ReadAllTextAsync(path);
        return JsonSerializer.Deserialize<TestDataset>(json)!;
    }
}
```

**Evaluation Report DTO**:
```csharp
// Application/RAG/DTOs/EvaluationReport.cs
public sealed class EvaluationReport
{
    public int TotalQuestions { get; init; }
    public int CorrectResults { get; init; }
    public double Accuracy { get; init; } // 百分比
    public double MeanReciprocalRank { get; init; } // MRR
    public double NormalizedDiscountedCumulativeGain { get; init; } // NDCG
    public bool PassThreshold { get; init; } // 是否達到 90% 準確率
    public DateTime EvaluatedAt { get; init; } = DateTime.UtcNow;
}
```

**參考文檔**:
- 📐 [RAG Accuracy Testing](../../../docs/testing/rag-accuracy-testing.md)
- 📚 [Information Retrieval Metrics](https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval))
- 📚 [NDCG Calculation](https://en.wikipedia.org/wiki/Discounted_cumulative_gain)

---

## 第二部分：技術實施方案 (How to Build)

**完整技術文檔**: [Backend Technical Guide](../../../docs/technical-implementation/01-backend-net9/README.md)

### Backend 實施

#### Clean Architecture 目錄結構

**架構設計參考**:
- 🏗️ [ADR-001: Clean Architecture](../../../docs/architecture/adr/ADR-001-clean-architecture.md) - 架構分層決策
- 🏗️ [Monorepo Setup](../../../docs/technical-implementation/01-backend-net9/01-monorepo-setup.md) - 專案結構

```
AIAgentPlatform.sln
├── src/
│   ├── AIAgentPlatform.Domain/                     # 領域層
│   │   ├── Entities/
│   │   │   ├── Document.cs                          # 文檔實體 (US 4.1)
│   │   │   └── DocumentChunk.cs                     # Chunk 實體
│   │   ├── ValueObjects/
│   │   │   ├── DocumentMetadata.cs                  # 文檔元數據 VO
│   │   │   └── ChunkingOptions.cs                   # 分塊選項 VO
│   │   ├── Enums/
│   │   │   └── DocumentStatus.cs                    # 文檔狀態 Enum
│   │   └── Interfaces/
│   │       └── IDocumentRepository.cs
│   │
│   ├── AIAgentPlatform.Application/                # 應用層
│   │   ├── Documents/
│   │   │   ├── Commands/
│   │   │   │   ├── UploadDocument/
│   │   │   │   │   ├── UploadDocumentCommand.cs
│   │   │   │   │   ├── UploadDocumentCommandHandler.cs
│   │   │   │   │   └── UploadDocumentCommandValidator.cs
│   │   │   │   └── DeleteDocument/
│   │   │   └── Queries/
│   │   │       ├── GetDocumentById/
│   │   │       └── GetDocumentsList/
│   │   │
│   │   ├── RAG/
│   │   │   ├── Commands/
│   │   │   │   └── PerformHybridSearch/
│   │   │   │       ├── HybridSearchCommand.cs
│   │   │   │       └── HybridSearchCommandHandler.cs
│   │   │   └── Models/
│   │   │       ├── SemanticSearchOptions.cs
│   │   │       ├── KeywordSearchOptions.cs
│   │   │       └── HybridSearchOptions.cs
│   │   │
│   │   └── Interfaces/
│   │       ├── IDocumentParser.cs
│   │       ├── IDocumentChunker.cs
│   │       ├── IEmbeddingService.cs
│   │       ├── IVectorStoreService.cs
│   │       ├── ISemanticSearchService.cs
│   │       ├── IKeywordSearchService.cs
│   │       ├── IHybridSearchService.cs
│   │       └── IReranker.cs
│   │
│   ├── AIAgentPlatform.Infrastructure/             # 基礎設施層
│   │   ├── Persistence/
│   │   │   ├── ApplicationDbContext.cs
│   │   │   ├── Configurations/
│   │   │   │   ├── DocumentConfiguration.cs
│   │   │   │   └── DocumentChunkConfiguration.cs
│   │   │   └── Repositories/
│   │   │       └── DocumentRepository.cs
│   │   │
│   │   ├── Services/
│   │   │   ├── DocumentParsers/
│   │   │   │   ├── PdfDocumentParser.cs
│   │   │   │   ├── DocxDocumentParser.cs
│   │   │   │   ├── TextDocumentParser.cs
│   │   │   │   └── DocumentParserFactory.cs
│   │   │   ├── DocumentChunkers/
│   │   │   │   └── FixedSizeChunker.cs
│   │   │   ├── OpenAIEmbeddingService.cs
│   │   │   ├── QdrantVectorStoreService.cs
│   │   │   ├── SemanticSearchService.cs
│   │   │   ├── KeywordSearchService.cs
│   │   │   ├── HybridSearchService.cs
│   │   │   ├── CrossEncoderReranker.cs
│   │   │   ├── BlobStorageService.cs
│   │   │   └── TikTokenCounter.cs
│   │   │
│   │   ├── BackgroundJobs/
│   │   │   └── DocumentProcessingJob.cs
│   │   │
│   │   └── Migrations/
│   │       └── 20260106_AddDocumentAndChunk.cs
│   │
│   └── AIAgentPlatform.API/                        # API 層
│       ├── Controllers/
│       │   ├── DocumentsController.cs              # 文檔 CRUD API
│       │   └── RAGController.cs                    # RAG 搜尋 API
│       └── Program.cs
│
└── tests/
    ├── AIAgentPlatform.UnitTests/
    │   ├── Domain/
    │   │   ├── DocumentTests.cs
    │   │   └── DocumentChunkTests.cs
    │   ├── Application/
    │   │   ├── Documents/
    │   │   │   └── UploadDocumentCommandHandlerTests.cs
    │   │   └── RAG/
    │   │       └── HybridSearchCommandHandlerTests.cs
    │   └── Infrastructure/
    │       ├── Services/
    │       │   ├── PdfDocumentParserTests.cs
    │       │   ├── FixedSizeChunkerTests.cs
    │       │   ├── OpenAIEmbeddingServiceTests.cs
    │       │   ├── QdrantVectorStoreServiceTests.cs
    │       │   └── HybridSearchServiceTests.cs
    │       └── Repositories/
    │           └── DocumentRepositoryTests.cs
    │
    └── AIAgentPlatform.IntegrationTests/
        ├── Documents/
        │   └── DocumentsControllerTests.cs
        ├── RAG/
        │   ├── RAGControllerTests.cs
        │   └── RAGAccuracyTests.cs
        └── BackgroundJobs/
            └── DocumentProcessingJobTests.cs
```

**分層設計參考**:
- 📐 [Domain Layer Design](../../../docs/architecture/layered-architecture/Domain-Layer.md)
- 📐 [Application Layer Design](../../../docs/architecture/layered-architecture/Application-Layer.md)
- 📐 [Infrastructure Layer Design](../../../docs/architecture/layered-architecture/Infrastructure-Layer.md)
- 📐 [API Layer Design](../../../docs/architecture/layered-architecture/API-Layer.md)

---

#### 核心實施策略

**架構模式參考**:
- 📚 [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - 完整架構設計

**1. CQRS with MediatR**:
- Commands: UploadDocument, DeleteDocument, ProcessDocument (寫入操作)
- Queries: GetDocumentById, GetDocumentsList, HybridSearch (讀取操作)
- 好處: 關注點分離, 易於測試, 支援背景任務

**參考**:
- 📐 [ADR-002: CQRS Pattern](../../../docs/architecture/adr/ADR-002-cqrs-pattern.md) - CQRS 架構決策
- 📐 [CQRS Implementation](../../../docs/technical-implementation/01-backend-net9/05-cqrs-implementation.md) - MediatR 實作指南

**2. Repository Pattern**:
- DocumentRepository: 封裝 Document 和 DocumentChunk 資料存取
- 支援 PostgreSQL 和 Qdrant 雙資料來源
- 好處: 隔離業務邏輯與資料存取, 易於測試

**參考**:
- 📐 [ADR-003: Repository Pattern](../../../docs/architecture/adr/ADR-003-repository-pattern.md) - Repository 架構決策
- 📐 [Data Access Layer](../../../docs/technical-implementation/01-backend-net9/04-data-access-layer.md) - Repository 實作

**3. Background Jobs (Hangfire)**:
- DocumentProcessingJob: 文檔解析 → 分塊 → 向量化 → 索引
- 支援重試機制（最多 3 次）
- 支援任務進度追蹤

**參考**:
- 📐 [Background Jobs](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md)
- 📦 [Hangfire](https://www.hangfire.io/)

**4. Dependency Injection**:
- 所有服務透過 DI 容器註冊
- 使用 Interface 定義契約
- 支援多種實作切換（例如：不同的 Embedding 服務）

**DI 配置範例**:
```csharp
// Program.cs - 服務註冊
builder.Services.AddScoped<IDocumentRepository, DocumentRepository>();
builder.Services.AddScoped<IDocumentParserFactory, DocumentParserFactory>();
builder.Services.AddScoped<IDocumentParser, PdfDocumentParser>();
builder.Services.AddScoped<IDocumentParser, DocxDocumentParser>();
builder.Services.AddScoped<IDocumentParser, TextDocumentParser>();
builder.Services.AddScoped<IDocumentChunker, FixedSizeChunker>();
builder.Services.AddScoped<IEmbeddingService, OpenAIEmbeddingService>();
builder.Services.AddScoped<IVectorStoreService, QdrantVectorStoreService>();
builder.Services.AddScoped<ISemanticSearchService, SemanticSearchService>();
builder.Services.AddScoped<IKeywordSearchService, KeywordSearchService>();
builder.Services.AddScoped<IHybridSearchService, HybridSearchService>();
builder.Services.AddScoped<IReranker, CrossEncoderReranker>();
builder.Services.AddScoped<IBlobStorageService, AzureBlobStorageService>();
builder.Services.AddScoped<ITokenCounter, TikTokenCounter>();

// Qdrant Client 註冊
builder.Services.AddSingleton<QdrantClient>(sp =>
{
    var config = sp.GetRequiredService<IConfiguration>();
    var endpoint = config["Qdrant:Endpoint"]!;
    return new QdrantClient(endpoint);
});

// Azure OpenAI Client 註冊
builder.Services.AddSingleton<OpenAIClient>(sp =>
{
    var config = sp.GetRequiredService<IConfiguration>();
    var endpoint = config["AzureOpenAI:Endpoint"]!;
    var apiKey = config["AzureOpenAI:ApiKey"]!;
    return new OpenAIClient(new Uri(endpoint), new AzureKeyCredential(apiKey));
});
```

---

### 開發週計劃 (Week by Week)

#### Week 1: US 4.1 - 文檔上傳與處理 (Day 1-5)

**Day 1: Domain Layer + Infrastructure Setup**
- 建立 Document Entity 和 DocumentChunk Entity
- 建立 DocumentMetadata Value Object
- 建立 DocumentStatus Enum
- EF Core Migration: Add Document and DocumentChunk tables
- **參考**: [Domain Layer Design](../../../docs/architecture/layered-architecture/Domain-Layer.md)

**Day 2: 文檔上傳 API**
- 實作 UploadDocumentCommand + Handler
- 實作 FluentValidation 驗證器
- 實作 DocumentsController (Upload endpoint)
- Azure Blob Storage 整合
- **參考**: [Document Upload API](../../../docs/api/knowledge-api-design.md#upload)

**Day 3: 文檔解析引擎**
- 實作 IDocumentParser 介面
- 實作 PdfDocumentParser
- 實作 DocxDocumentParser
- 實作 TextDocumentParser
- 實作 DocumentParserFactory
- **參考**: [Document Parsing Strategy](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md)

**Day 4: 文檔分塊與向量化**
- 實作 IDocumentChunker 介面
- 實作 FixedSizeChunker
- 實作 TikTokenCounter
- 實作 IEmbeddingService 介面
- 實作 OpenAIEmbeddingService（含快取）
- **參考**: [Chunking Strategies](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md)
- **參考**: [Embedding Service](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md)

**Day 5: Qdrant 整合與背景任務**
- 實作 IVectorStoreService 介面
- 實作 QdrantVectorStoreService
- 實作 DocumentProcessingJob（Hangfire）
- Hangfire Dashboard 配置
- **參考**: [Qdrant Design](../../../docs/database/qdrant-design.md)
- **參考**: [Background Jobs](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md)

---

#### Week 2: US 4.2 - RAG 檢索策略 (Day 6-10)

**Day 6: 語義搜尋**
- 實作 ISemanticSearchService 介面
- 實作 SemanticSearchService
- 實作 SemanticSearchOptions
- 測試向量搜尋流程
- **參考**: [Semantic Search](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md)

**Day 7: 關鍵字搜尋**
- PostgreSQL FTS Migration（Add tsvector column）
- 實作 IKeywordSearchService 介面
- 實作 KeywordSearchService（BM25）
- 測試 FTS 查詢效能
- **參考**: [Keyword Search](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md)

**Day 8: 混合檢索**
- 實作 IHybridSearchService 介面
- 實作 HybridSearchService（RRF 融合）
- 實作 HybridSearchOptions
- 測試 Hybrid Search 準確率
- **參考**: [Hybrid Search](../../../docs/technical-implementation/01-backend-net9/17-hybrid-search.md)

**Day 9: Re-ranking**
- 實作 IReranker 介面
- 實作 CrossEncoderReranker（HuggingFace API）
- 整合 Re-ranking 到 HybridSearchService
- 測試 Re-ranking 效果
- **參考**: [Re-ranking Strategy](../../../docs/technical-implementation/01-backend-net9/18-reranking.md)

**Day 10: RAG API 與測試框架**
- 實作 RAGController（Search endpoint）
- 實作 RAGEvaluationService
- 建立標準測試集（test-dataset.json）
- 執行準確率評估（目標 ≥90%）
- **參考**: [RAG Accuracy Testing](../../../docs/testing/rag-accuracy-testing.md)

---

#### Week 3: 整合測試與優化 (Day 11-15)

**Day 11: 單元測試**
- DocumentTests, DocumentChunkTests
- UploadDocumentCommandHandlerTests
- PdfDocumentParserTests, DocxDocumentParserTests
- FixedSizeChunkerTests
- OpenAIEmbeddingServiceTests
- **參考**: [Unit Testing Strategy](../../../docs/testing/unit-testing-strategy.md)

**Day 12: 整合測試**
- DocumentsControllerTests（API 端點測試）
- RAGControllerTests（搜尋 API 測試）
- DocumentProcessingJobTests（背景任務測試）
- QdrantVectorStoreServiceTests（Qdrant 整合測試）
- **參考**: [Integration Testing](../../../docs/testing/integration-testing.md)

**Day 13: 效能優化**
- Qdrant 索引優化（HNSW 參數調整）
- Embedding 快取策略優化
- PostgreSQL FTS 索引優化
- Batch Embedding 並行處理
- **參考**: [Performance Optimization](../../../docs/technical-implementation/01-backend-net9/19-performance-optimization.md)

**Day 14: 文檔與 Swagger**
- API 文檔完善（Swagger 註解）
- README 更新（RAG 使用指南）
- 技術文檔完善
- 程式碼註解補充
- **參考**: [API Documentation Standards](../../../docs/api/api-documentation-standards.md)

**Day 15: 最終驗收與發布**
- 執行完整測試套件（Unit + Integration）
- 執行 RAG 準確率評估（確認 ≥90%）
- Code Review
- Sprint 5 Retrospective
- 準備 Sprint 6 規劃

---

## 第三部分：編碼規範

**完整編碼規範**: [C# Coding Standards](../../../docs/technical-implementation/01-backend-net9/02-coding-standards.md)

### 1. 命名規範

**類別與介面**:
```csharp
// ✅ 正確
public sealed class DocumentProcessingJob { }
public interface IDocumentParser { }

// ❌ 錯誤
public class documentprocessingjob { }
public interface DocumentParser { }
```

**方法與屬性**:
```csharp
// ✅ 正確
public async Task<Result> ProcessDocumentAsync(Guid documentId, CancellationToken cancellationToken)
{
    // ...
}

public string FileName { get; private set; }

// ❌ 錯誤
public async Task<Result> process_document(Guid documentId) { }
public string filename;
```

**區域變數與參數**:
```csharp
// ✅ 正確
var documentId = Guid.NewGuid();
var chunkingOptions = new ChunkingOptions();

// ❌ 錯誤
var DocumentId = Guid.NewGuid();
var ChunkingOptions = new ChunkingOptions();
```

---

### 2. 程式碼組織

**檔案結構**:
```csharp
// ✅ 正確的檔案結構
namespace AIAgentPlatform.Application.Documents.Commands.UploadDocument;

// 1. Usings
using AIAgentPlatform.Domain.Entities;
using MediatR;

// 2. Namespace 宣告
namespace AIAgentPlatform.Application.Documents.Commands.UploadDocument;

// 3. 類別定義
public sealed record UploadDocumentCommand : IRequest<Result<UploadDocumentResponse>>
{
    public required Guid AgentId { get; init; }
    public required IFormFileCollection Files { get; init; }
}

// 4. Handler 定義
public sealed class UploadDocumentCommandHandler
    : IRequestHandler<UploadDocumentCommand, Result<UploadDocumentResponse>>
{
    private readonly IDocumentRepository _documentRepo;
    private readonly IBlobStorageService _blobStorage;

    public UploadDocumentCommandHandler(
        IDocumentRepository documentRepo,
        IBlobStorageService blobStorage)
    {
        _documentRepo = documentRepo;
        _blobStorage = blobStorage;
    }

    public async Task<Result<UploadDocumentResponse>> Handle(
        UploadDocumentCommand request,
        CancellationToken cancellationToken)
    {
        // 實作
    }
}
```

---

### 3. 錯誤處理

**使用 Result Pattern**:
```csharp
// ✅ 正確
public async Task<Result<Document>> GetDocumentAsync(Guid id, CancellationToken cancellationToken)
{
    var document = await _documentRepo.GetByIdAsync(id, cancellationToken);

    if (document == null)
    {
        return Result<Document>.Failure($"Document not found: {id}");
    }

    return Result<Document>.Success(document);
}

// ❌ 錯誤：直接拋出例外
public async Task<Document> GetDocumentAsync(Guid id)
{
    var document = await _documentRepo.GetByIdAsync(id);

    if (document == null)
    {
        throw new NotFoundException($"Document not found: {id}");
    }

    return document;
}
```

**自訂例外**:
```csharp
// ✅ 正確
public sealed class DocumentParseException : Exception
{
    public DocumentParseException(string message, Exception? innerException = null)
        : base(message, innerException)
    {
    }
}

// 使用
throw new DocumentParseException($"Failed to parse PDF: {fileName}", ex);
```

---

### 4. 非同步程式設計

**Async/Await 規範**:
```csharp
// ✅ 正確
public async Task<IReadOnlyList<SearchResult>> SearchAsync(
    string query,
    CancellationToken cancellationToken = default)
{
    var embedding = await _embeddingService.GenerateEmbeddingAsync(query, cancellationToken);
    var results = await _vectorStore.SearchAsync(embedding, cancellationToken);
    return results;
}

// ❌ 錯誤：缺少 CancellationToken
public async Task<IReadOnlyList<SearchResult>> SearchAsync(string query)
{
    var embedding = await _embeddingService.GenerateEmbeddingAsync(query);
    var results = await _vectorStore.SearchAsync(embedding);
    return results;
}
```

**ConfigureAwait**:
```csharp
// ✅ 正確（Library 程式碼）
public async Task<Document> GetDocumentAsync(Guid id, CancellationToken cancellationToken)
{
    return await _dbContext.Documents
        .FirstOrDefaultAsync(d => d.Id == id, cancellationToken)
        .ConfigureAwait(false);
}

// ✅ 正確（API Controller 可省略）
public async Task<IActionResult> GetDocument(Guid id, CancellationToken cancellationToken)
{
    var document = await _mediator.Send(new GetDocumentByIdQuery { Id = id }, cancellationToken);
    return Ok(document);
}
```

---

### 5. Dependency Injection

**建構子注入**:
```csharp
// ✅ 正確
public sealed class DocumentProcessingJob : IDocumentProcessingJob
{
    private readonly IDocumentRepository _documentRepo;
    private readonly IBlobStorageService _blobStorage;
    private readonly IDocumentParserFactory _parserFactory;
    private readonly ILogger<DocumentProcessingJob> _logger;

    public DocumentProcessingJob(
        IDocumentRepository documentRepo,
        IBlobStorageService blobStorage,
        IDocumentParserFactory parserFactory,
        ILogger<DocumentProcessingJob> logger)
    {
        _documentRepo = documentRepo;
        _blobStorage = blobStorage;
        _parserFactory = parserFactory;
        _logger = logger;
    }
}

// ❌ 錯誤：Service Locator Anti-Pattern
public sealed class DocumentProcessingJob
{
    public DocumentProcessingJob(IServiceProvider serviceProvider)
    {
        _documentRepo = serviceProvider.GetRequiredService<IDocumentRepository>();
    }
}
```

---

### 6. Logging

**Structured Logging**:
```csharp
// ✅ 正確
_logger.LogInformation(
    "Document processing started for ID: {DocumentId}, Agent: {AgentId}",
    documentId, agentId);

_logger.LogWarning(
    "Document parsing took longer than expected: {Duration}ms for file {FileName}",
    duration, fileName);

_logger.LogError(ex,
    "Failed to generate embedding for document {DocumentId}",
    documentId);

// ❌ 錯誤：字串插值
_logger.LogInformation($"Document processing started for ID: {documentId}");
```

---

### 7. 註解與文檔

**XML 文檔註解**:
```csharp
/// <summary>
/// 解析文檔並提取純文字內容和元數據
/// </summary>
/// <param name="fileStream">文檔檔案流</param>
/// <param name="fileName">文檔檔案名稱</param>
/// <param name="cancellationToken">取消令牌</param>
/// <returns>包含內容和元數據的解析結果</returns>
/// <exception cref="DocumentParseException">當文檔解析失敗時拋出</exception>
Task<DocumentParseResult> ParseAsync(
    Stream fileStream,
    string fileName,
    CancellationToken cancellationToken = default);
```

---

## 第四部分：質量保證

**完整測試策略**: [Testing Strategy](../../../docs/testing/testing-strategy.md)

### 1. 單元測試

**測試覆蓋率目標**: ≥80% (Domain + Application 層)

**測試範例**:
```csharp
// tests/AIAgentPlatform.UnitTests/Infrastructure/Services/FixedSizeChunkerTests.cs
public sealed class FixedSizeChunkerTests
{
    private readonly Mock<ITokenCounter> _tokenCounterMock;
    private readonly FixedSizeChunker _sut;

    public FixedSizeChunkerTests()
    {
        _tokenCounterMock = new Mock<ITokenCounter>();
        _sut = new FixedSizeChunker(_tokenCounterMock.Object, Mock.Of<ILogger<FixedSizeChunker>>());
    }

    [Fact]
    public async Task ChunkAsync_WithSmallContent_ShouldReturnSingleChunk()
    {
        // Arrange
        var content = "This is a short document.";
        var options = new ChunkingOptions { ChunkSize = 1024, OverlapSize = 0 };

        _tokenCounterMock
            .Setup(x => x.CountTokensAsync(It.IsAny<string>()))
            .ReturnsAsync(5);

        // Act
        var chunks = await _sut.ChunkAsync(content, options);

        // Assert
        chunks.Should().HaveCount(1);
        chunks[0].Content.Should().Contain(content);
    }

    [Fact]
    public async Task ChunkAsync_WithLargeContent_ShouldReturnMultipleChunks()
    {
        // Arrange
        var content = string.Join(" ", Enumerable.Repeat("word", 2000));
        var options = new ChunkingOptions { ChunkSize = 512, OverlapSize = 100 };

        _tokenCounterMock
            .Setup(x => x.CountTokensAsync(It.IsAny<string>()))
            .ReturnsAsync((string text) => text.Split(' ').Length);

        // Act
        var chunks = await _sut.ChunkAsync(content, options);

        // Assert
        chunks.Should().HaveCountGreaterThan(1);
        chunks.Should().OnlyContain(c => c.TokenCount <= 512);
    }

    [Fact]
    public async Task ChunkAsync_WithOverlap_ShouldHaveOverlappingContent()
    {
        // Arrange
        var content = string.Join(" ", Enumerable.Repeat("word", 1000));
        var options = new ChunkingOptions { ChunkSize = 300, OverlapSize = 50 };

        _tokenCounterMock
            .Setup(x => x.CountTokensAsync(It.IsAny<string>()))
            .ReturnsAsync((string text) => text.Split(' ').Length);

        // Act
        var chunks = await _sut.ChunkAsync(content, options);

        // Assert
        chunks.Should().HaveCountGreaterThan(1);

        // 驗證相鄰 chunks 有重疊
        for (int i = 0; i < chunks.Count - 1; i++)
        {
            var chunk1End = chunks[i].Content.Split(' ').TakeLast(50);
            var chunk2Start = chunks[i + 1].Content.Split(' ').Take(50);
            chunk1End.Intersect(chunk2Start).Should().NotBeEmpty();
        }
    }
}
```

---

### 2. 整合測試

**測試範例**:
```csharp
// tests/AIAgentPlatform.IntegrationTests/RAG/RAGAccuracyTests.cs
public sealed class RAGAccuracyTests : IClassFixture<IntegrationTestWebAppFactory>
{
    private readonly IntegrationTestWebAppFactory _factory;
    private readonly IRAGEvaluationService _evaluationService;

    public RAGAccuracyTests(IntegrationTestWebAppFactory factory)
    {
        _factory = factory;
        _evaluationService = _factory.Services.GetRequiredService<IRAGEvaluationService>();
    }

    [Fact]
    public async Task HybridSearch_WithStandardTestSet_ShouldAchieve90PercentAccuracy()
    {
        // Arrange
        var testDatasetPath = "test-data/standard-qa-dataset.json";
        var agentId = await SetupTestAgentWithKnowledgeBase();

        var searchOptions = new HybridSearchOptions
        {
            TopK = 10,
            SemanticWeight = 0.7f,
            KeywordWeight = 0.3f,
            EnableReranking = true,
            SemanticScoreThreshold = 0.7f
        };

        // Act
        var report = await _evaluationService.EvaluateAsync(
            testDatasetPath,
            agentId,
            searchOptions,
            CancellationToken.None);

        // Assert
        report.Accuracy.Should().BeGreaterOrEqualTo(90.0);
        report.MeanReciprocalRank.Should().BeGreaterOrEqualTo(0.8);
        report.PassThreshold.Should().BeTrue();

        _factory.TestOutputHelper.WriteLine($"Accuracy: {report.Accuracy:F2}%");
        _factory.TestOutputHelper.WriteLine($"MRR: {report.MeanReciprocalRank:F3}");
        _factory.TestOutputHelper.WriteLine($"NDCG: {report.NormalizedDiscountedCumulativeGain:F3}");
    }

    private async Task<Guid> SetupTestAgentWithKnowledgeBase()
    {
        // 建立測試 Agent
        // 上傳測試文檔
        // 等待背景處理完成
        // 返回 Agent ID
        // 實作略...
        return Guid.NewGuid();
    }
}
```

---

### 3. 效能測試

**效能基準**:
- 文檔上傳: <2 秒（10MB PDF）
- 文檔解析: <5 秒（100 頁 PDF）
- 文檔分塊: <1 秒（1000 chunks）
- Embedding 生成: <3 秒（批次 100 chunks）
- 向量搜尋: <200ms（100K 向量）
- Hybrid Search: <500ms（含 Re-ranking）

**效能測試範例**:
```csharp
[Fact]
public async Task QdrantVectorSearch_With100KVectors_ShouldComplete InLessThan200Ms()
{
    // Arrange
    var collectionName = "perf_test_collection";
    await SetupCollectionWith100KVectors(collectionName);

    var queryVector = await _embeddingService.GenerateEmbeddingAsync("test query");
    var stopwatch = Stopwatch.StartNew();

    // Act
    var results = await _vectorStore.SearchAsync(
        collectionName,
        queryVector,
        limit: 10,
        scoreThreshold: 0.7f);

    stopwatch.Stop();

    // Assert
    stopwatch.ElapsedMilliseconds.Should().BeLessThan(200);
    results.Should().HaveCountLessOrEqualTo(10);
}
```

---

### 4. 程式碼審查檢查清單

**Domain Layer**:
- [ ] Entity 包含明確的業務規則
- [ ] Value Object 實作 Equality 比較
- [ ] Enum 使用有意義的名稱
- [ ] 沒有外部依賴（純 Domain 邏輯）

**Application Layer**:
- [ ] Command/Query 使用 MediatR
- [ ] Handler 包含完整的驗證邏輯
- [ ] FluentValidation 規則完整
- [ ] DTO 與 Entity 分離

**Infrastructure Layer**:
- [ ] Repository 實作正確的 Interface
- [ ] EF Core Configuration 使用 Fluent API
- [ ] 外部服務使用 Interface 抽象
- [ ] 錯誤處理完整（try-catch + logging）

**API Layer**:
- [ ] Controller 僅負責路由和回應
- [ ] 使用 MediatR 呼叫 Application Layer
- [ ] 包含 Swagger 註解
- [ ] 回應格式統一（Result Pattern）

---

## 第五部分：參考文檔

### 核心規劃文檔

**規劃文檔**:
- 📋 [MVP Scope Definition](../../1-planning/MVP-SCOPE-DEFINITION.md) - Phase 1A 範圍定義
- 📊 [Sprint Allocation Analysis](../../1-planning/SPRINT-ALLOCATION-ANALYSIS.md#sprint-5) - Sprint 5 詳細分析 (US 4.1-4.2, 13 SP)
- 🎯 [Development Strategy](../../1-planning/DEVELOPMENT-STRATEGY.md) - Knowledge RAG 系統開發策略
- 📐 [Architecture Design Document](../../../docs/architecture/Architecture-Design-Document.md) - Clean Architecture 總覽

**Sprint 5 文檔**:
- 📖 [SPRINT-5-1-OVERVIEW.md](./SPRINT-5-1-OVERVIEW.md) - Sprint 5 概覽
- 📖 [SPRINT-5-3-CONTEXT.md](./SPRINT-5-3-CONTEXT.md) - Sprint 5 技術背景
- 📖 [SPRINT-5-4-CHECKLIST.md](./SPRINT-5-4-CHECKLIST.md) - Sprint 5 任務清單

---

### User Stories

**Knowledge Management (Module 05)**:
- 📖 [US 5.1 - 知識庫文件上傳與處理](../../../docs/user-stories/modules/module-05-agent-memory.md#us-51)
- 📖 [US 5.2 - 精確檢索策略配置](../../../docs/user-stories/modules/module-05-agent-memory.md#us-52)

---

### 架構設計 ADR

**核心架構決策**:
- 🏗️ [ADR-001: Clean Architecture](../../../docs/architecture/adr/ADR-001-clean-architecture.md) - 架構分層決策
- 🏗️ [ADR-002: CQRS Pattern](../../../docs/architecture/adr/ADR-002-cqrs-pattern.md) - CQRS 架構決策
- 🏗️ [ADR-003: Repository Pattern](../../../docs/architecture/adr/ADR-003-repository-pattern.md) - Repository 架構決策
- 🏗️ [ADR-006: Hybrid State Management](../../../docs/architecture/adr/ADR-006-hybrid-state-management.md) - Redis + PostgreSQL

---

### 技術實作指南

**Backend 技術實作**:
- 📐 [10-document-parsing.md](../../../docs/technical-implementation/01-backend-net9/10-document-parsing.md) - 文檔解析策略
- 📐 [11-chunking-strategies.md](../../../docs/technical-implementation/01-backend-net9/11-chunking-strategies.md) - 分塊策略
- 📐 [12-embedding-service.md](../../../docs/technical-implementation/01-backend-net9/12-embedding-service.md) - Embedding 服務
- 📐 [13-azure-storage.md](../../../docs/technical-implementation/01-backend-net9/13-azure-storage.md) - Azure Blob Storage
- 📐 [14-background-jobs.md](../../../docs/technical-implementation/01-backend-net9/14-background-jobs.md) - 背景任務（Hangfire）
- 📐 [15-semantic-search.md](../../../docs/technical-implementation/01-backend-net9/15-semantic-search.md) - 語義搜尋
- 📐 [16-keyword-search.md](../../../docs/technical-implementation/01-backend-net9/16-keyword-search.md) - 關鍵字搜尋（BM25）
- 📐 [17-hybrid-search.md](../../../docs/technical-implementation/01-backend-net9/17-hybrid-search.md) - 混合檢索（RRF）
- 📐 [18-reranking.md](../../../docs/technical-implementation/01-backend-net9/18-reranking.md) - Re-ranking 策略
- 📐 [19-performance-optimization.md](../../../docs/technical-implementation/01-backend-net9/19-performance-optimization.md) - 效能優化

---

### API 設計

**Knowledge API 設計**:
- 📘 [knowledge-api-design.md](../../../docs/api/knowledge-api-design.md) - Document Upload, Search API
- 📘 [api-documentation-standards.md](../../../docs/api/api-documentation-standards.md) - API 文檔標準

---

### 資料庫設計

**資料庫 Schema**:
- 🗄️ [document-schema.md](../../../docs/database/document-schema.md) - Document & DocumentChunk Tables
- 🗄️ [qdrant-design.md](../../../docs/database/qdrant-design.md) - Qdrant Collection 設計

---

### 測試文檔

**測試策略與指南**:
- 🧪 [testing-strategy.md](../../../docs/testing/testing-strategy.md) - 完整測試策略
- 🧪 [unit-testing-strategy.md](../../../docs/testing/unit-testing-strategy.md) - 單元測試指南
- 🧪 [integration-testing.md](../../../docs/testing/integration-testing.md) - 整合測試指南
- 🧪 [rag-accuracy-testing.md](../../../docs/testing/rag-accuracy-testing.md) - RAG 準確率測試

---

### 相關 Sprint 參考

**Previous Sprints**:
- 📖 [Sprint 1 - Agent CRUD](../sprint-1/SPRINT-1-2-PLAN.md) - Agent 基礎架構參考
- 📖 [Sprint 2 - Execution Engine](../sprint-2/SPRINT-2-2-PLAN.md) - Semantic Kernel 整合參考
- 📖 [Sprint 3 - Plugin System](../sprint-3/SPRINT-3-2-PLAN.md) - Plugin 系統參考
- 📖 [Sprint 4 - Persona Framework](../sprint-4/SPRINT-4-2-PLAN.md) - Persona & Prompt 參考

---

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

---

## 使用指南

### 如何使用此文件

**開發階段使用**:
1. **開發前**: 閱讀「第一部分」了解需求範圍
2. **開發中**: 參考「第二部分」的技術實作方案和程式碼範例
3. **Coding 時**: 遵循「第三部分」的編碼規範
4. **測試前**: 查閱「第四部分」的質量保證標準
5. **疑問時**: 查找「第五部分」的相關參考文檔

**任務追蹤使用**:
- 配合 [SPRINT-5-4-CHECKLIST.md](./SPRINT-5-4-CHECKLIST.md) 追蹤進度
- 配合 [SPRINT-5-5-DEV-LOG.md](./SPRINT-5-5-DEV-LOG.md) 記錄開發日誌
- 配合 [SPRINT-5-6-ISSUES.md](./SPRINT-5-6-ISSUES.md) 追蹤問題

**Code Review 使用**:
- 使用「第四部分」的檢查清單進行審查
- 確保符合「第三部分」的編碼規範
- 驗證「第一部分」的驗收標準

---

## 版本歷史

**v2.1** (2025-11-13)
- 初始版本建立
- 完整 US 4.1 (文檔上傳與處理) 技術規格
- 完整 US 4.2 (RAG 檢索策略) 技術規格
- 開發週計劃（Week 1-3）
- 編碼規範與質量保證標準
- 完整參考文檔索引

---

**文件維護**:
- 📅 下次審查日期: Sprint 5 開始前 (2026-01-06)
- 👤 負責人: Backend Tech Lead
- 🔄 更新頻率: Sprint 進行中每週更新實作細節

**相關文檔**:
- ⬆️ 上一層: [Sprint 5 Overview](./SPRINT-5-1-OVERVIEW.md)
- ➡️ 下一步: [Sprint 5 Context](./SPRINT-5-3-CONTEXT.md)
- 📋 任務清單: [Sprint 5 Checklist](./SPRINT-5-4-CHECKLIST.md)
