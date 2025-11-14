# Sprint 13 Implementation Plan - 28 種節點類型與配置面板

**版本**: v2.1
**Sprint 編號**: Sprint 13
**Sprint 週期**: Week 37-39 (3 週)
**Phase**: Phase 1D - 工作流編輯器 Frontend (Workflow Editor Frontend)
**計劃日期**: 2026-06-23 ~ 2026-07-13
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 文檔導航

**本文件位置**: Part 2 of 7 - 詳細實施計劃

**Sprint 13 完整文檔集**:
1. SPRINT-13-1-OVERVIEW.md - Sprint 總覽與定位
2. **SPRINT-13-2-PLAN.md** (本文件) - 詳細實施計劃
3. SPRINT-13-3-CONTEXT.md - 節點系統與配置技術上下文
4. SPRINT-13-4-CHECKLIST.md - 175+ 任務清單
5. SPRINT-13-5-DEV-LOG.md - 3 週開發日誌
6. SPRINT-13-6-ISSUES.md - 問題追蹤與解決方案
7. SPRINT-13-7-RETROSPECTIVE.md - Sprint 回顧與評估

**規劃文檔參考**:
- `claudedocs/1-planning/PHASE-1D-WORKFLOW-EDITOR-PLAN.md`
- `claudedocs/1-planning/MODULE-09-IMPLEMENTATION-GUIDE.md`
- `docs/user-stories/modules/module-09-workflow-editor.md`

---

## 目錄

### 第一部分: 本 Sprint 要建立什麼
1. [28 種節點類型詳細說明](#第一部分-本-sprint-要建立什麼)
2. [Node Configuration Panel 設計](#node-configuration-panel-設計)
3. [Connection Line Drawing 設計](#connection-line-drawing-設計)

### 第二部分: 如何實施
4. [Phase 1: 28 種節點類型實作](#phase-1-28-種節點類型實作)
5. [Phase 2: Node Configuration Panel](#phase-2-node-configuration-panel)
6. [Phase 3: Connection Line Drawing](#phase-3-connection-line-drawing)
7. [Phase 4: Testing & Documentation](#phase-4-testing--documentation)

### 第三部分: 編碼規範
8. [編碼標準與最佳實踐](#編碼標準與最佳實踐)
9. [TypeScript 型別定義](#typescript-型別定義)
10. [Vue 3 組件規範](#vue-3-組件規範)

### 第四部分: 質量保證
11. [測試策略](#測試策略)
12. [程式碼審查標準](#程式碼審查標準)
13. [效能優化指南](#效能優化指南)

### 第五部分: 參考文檔
14. [完整參考文獻](#完整參考文獻)

---

# 第一部分: 本 Sprint 要建立什麼

## 28 種節點類型詳細說明

### Category 1: 基礎節點 (5 種)

#### 1.1 AgentNode (Agent 節點)

**視覺設計**:
```yaml
Icon: 🤖
Color: #4CAF50 (綠色)
Size: 180px × 120px
Border: 2px solid #4CAF50
Background: #E8F5E9 (淺綠)
```

**Port 定義**:
```typescript
Inputs:
  - context: object (required) - 執行上下文
  - parameters: object (optional) - 額外參數

Outputs:
  - result: any (always) - 執行結果
  - error: string (conditional) - 錯誤訊息
```

**Configuration Schema**:
```typescript
interface AgentNodeConfig {
  agentId: string           // Agent ID (required)
  persona: string           // Persona 設定 (required)
  temperature: number       // 溫度 0-2 (default: 0.7)
  maxTokens: number         // 最大 Token 數 (default: 2000)
  timeout: number           // 超時時間秒數 (default: 30)
  retryPolicy: {
    maxRetries: number      // 最大重試次數 (default: 3)
    backoffMs: number       // 重試間隔毫秒 (default: 1000)
  }
}
```

**Validation Rules**:
```typescript
{
  agentId: [
    { required: true, message: 'Agent ID is required' },
    { validator: async (val) => await agentExists(val), message: 'Agent not found' }
  ],
  temperature: [
    { type: 'number', min: 0, max: 2, message: 'Temperature must be 0-2' }
  ],
  maxTokens: [
    { type: 'number', min: 1, max: 32000, message: 'Max tokens must be 1-32000' }
  ]
}
```

**組件檔案**:
- `packages/remote/src/components/nodes/basic/AgentNode.vue`
- `packages/remote/src/components/nodes/basic/AgentNode.spec.ts`

---

#### 1.2 PluginNode (Plugin 節點)

**視覺設計**:
```yaml
Icon: 🔌
Color: #2196F3 (藍色)
Size: 160px × 100px
Border: 2px solid #2196F3
Background: #E3F2FD (淺藍)
```

**Port 定義**:
```typescript
Inputs:
  - input: any (required) - Plugin 輸入

Outputs:
  - output: any (always) - Plugin 輸出
  - error: string (conditional) - 錯誤訊息
```

**Configuration Schema**:
```typescript
interface PluginNodeConfig {
  pluginId: string          // Plugin ID (required)
  method: string            // 方法名稱 (required)
  parameters: Record<string, any> // 參數 (optional)
  timeout: number           // 超時時間 (default: 10)
  cacheResults: boolean     // 是否快取結果 (default: false)
}
```

**Validation Rules**:
```typescript
{
  pluginId: [
    { required: true, message: 'Plugin ID is required' },
    { validator: async (val) => await pluginExists(val), message: 'Plugin not found' }
  ],
  method: [
    { required: true, message: 'Method name is required' }
  ]
}
```

---

#### 1.3 DecisionNode (Decision 節點)

**視覺設計**:
```yaml
Icon: 💭
Color: #FF9800 (橙色)
Shape: 菱形 (Diamond)
Size: 140px × 140px
Border: 2px solid #FF9800
Background: #FFF3E0 (淺橙)
```

**Port 定義**:
```typescript
Inputs:
  - condition: boolean | any (required) - 條件值

Outputs:
  - true: any (conditional) - 條件為真時輸出
  - false: any (conditional) - 條件為假時輸出
```

**Configuration Schema**:
```typescript
interface DecisionNodeConfig {
  conditionType: 'boolean' | 'expression' | 'function'
  expression?: string       // JavaScript 表達式
  function?: string         // 自訂函數
  defaultBranch: 'true' | 'false' // 預設分支
}
```

**Validation Rules**:
```typescript
{
  conditionType: [
    { required: true, message: 'Condition type is required' }
  ],
  expression: [
    {
      validator: (val, config) => {
        if (config.conditionType === 'expression' && !val) {
          return false
        }
        return true
      },
      message: 'Expression is required when type is expression'
    }
  ]
}
```

---

#### 1.4 StartNode (Start 節點)

**視覺設計**:
```yaml
Icon: ▶️
Color: #4CAF50 (綠色)
Shape: 圓形 (Circle)
Size: 80px × 80px
Border: 3px solid #4CAF50
Background: #E8F5E9 (淺綠)
```

**Port 定義**:
```typescript
Inputs: [] // 無輸入

Outputs:
  - start: void (always) - 啟動訊號
```

**Configuration Schema**:
```typescript
interface StartNodeConfig {
  triggerType: 'manual' | 'scheduled' | 'webhook' | 'event'
  schedule?: string         // Cron expression (for scheduled)
  webhookUrl?: string       // Webhook URL (for webhook)
  eventType?: string        // Event type (for event)
}
```

**Validation Rules**:
```typescript
{
  triggerType: [
    { required: true, message: 'Trigger type is required' }
  ],
  schedule: [
    {
      validator: (val, config) => {
        if (config.triggerType === 'scheduled') {
          return validateCronExpression(val)
        }
        return true
      },
      message: 'Invalid cron expression'
    }
  ]
}
```

---

#### 1.5 EndNode (End 節點)

**視覺設計**:
```yaml
Icon: ⏹️
Color: #F44336 (紅色)
Shape: 圓形 (Circle)
Size: 80px × 80px
Border: 3px solid #F44336
Background: #FFEBEE (淺紅)
```

**Port 定義**:
```typescript
Inputs:
  - result: any (required) - 工作流結果

Outputs: [] // 無輸出
```

**Configuration Schema**:
```typescript
interface EndNodeConfig {
  outputType: 'success' | 'error' | 'custom'
  notification: {
    enabled: boolean        // 是否啟用通知
    recipients: string[]    // 通知對象
    template: string        // 通知模板
  }
  saveResult: boolean       // 是否儲存結果
  resultPath?: string       // 結果儲存路徑
}
```

---

### Category 2: 控制流節點 (6 種)

#### 2.1 IfElseNode (If-Else 節點)

**視覺設計**:
```yaml
Icon: 🔀
Color: #9C27B0 (紫色)
Shape: 菱形 (Diamond)
Size: 160px × 120px
Border: 2px solid #9C27B0
Background: #F3E5F5 (淺紫)
```

**Port 定義**:
```typescript
Inputs:
  - condition: boolean | any (required) - 條件

Outputs:
  - true: any (conditional) - True 分支
  - false: any (conditional) - False 分支
```

**Configuration Schema**:
```typescript
interface IfElseNodeConfig {
  conditionExpression: string  // 條件表達式
  trueLabel?: string           // True 分支標籤
  falseLabel?: string          // False 分支標籤
  evaluationMode: 'strict' | 'loose' // 評估模式
}
```

---

#### 2.2 SwitchNode (Switch 節點)

**視覺設計**:
```yaml
Icon: 🎛️
Color: #9C27B0 (紫色)
Size: 180px × 140px
Border: 2px solid #9C27B0
Background: #F3E5F5 (淺紫)
```

**Port 定義**:
```typescript
Inputs:
  - value: any (required) - 比對值

Outputs:
  - case[1-N]: any (conditional) - 各個 case 輸出
  - default: any (conditional) - 預設輸出
```

**Configuration Schema**:
```typescript
interface SwitchNodeConfig {
  cases: Array<{
    value: any              // Case 值
    label: string           // 標籤
    outputPort: string      // 輸出 Port ID
  }>
  defaultCase: {
    enabled: boolean        // 是否啟用預設 case
    label: string
  }
  comparisonMode: 'strict' | 'loose' // 比對模式
}
```

---

#### 2.3 LoopNode (Loop 節點)

**視覺設計**:
```yaml
Icon: 🔁
Color: #9C27B0 (紫色)
Size: 160px × 120px
Border: 2px solid #9C27B0
Background: #F3E5F5 (淺紫)
```

**Port 定義**:
```typescript
Inputs:
  - items: any[] (required) - 迭代項目

Outputs:
  - iteration: any (每次迭代) - 當前迭代值
  - complete: any[] (迭代完成) - 所有結果
```

**Configuration Schema**:
```typescript
interface LoopNodeConfig {
  maxIterations: number       // 最大迭代次數 (default: 1000)
  breakCondition?: string     // 中斷條件
  itemVariable: string        // 項目變數名稱 (default: 'item')
  indexVariable: string       // 索引變數名稱 (default: 'index')
  collectResults: boolean     // 是否收集結果 (default: true)
}
```

---

#### 2.4 WhileNode (While 節點)

**視覺設計**:
```yaml
Icon: ⭕
Color: #9C27B0 (紫色)
Shape: 六邊形
Size: 160px × 120px
Border: 2px solid #9C27B0
Background: #F3E5F5 (淺紫)
```

**Port 定義**:
```typescript
Inputs:
  - condition: boolean (required) - 迴圈條件

Outputs:
  - loop: any (每次迭代) - 迴圈體輸出
  - exit: any (條件為假) - 迴圈結束輸出
```

**Configuration Schema**:
```typescript
interface WhileNodeConfig {
  condition: string           // 條件表達式
  maxIterations: number       // 最大迭代次數 (防止無限迴圈)
  timeout: number             // 超時時間 (秒)
  collectResults: boolean     // 是否收集所有迭代結果
}
```

---

#### 2.5 ForEachNode (For-Each 節點)

**視覺設計**:
```yaml
Icon: 📋
Color: #9C27B0 (紫色)
Size: 180px × 120px
Border: 2px solid #9C27B0
Background: #F3E5F5 (淺紫)
```

**Port 定義**:
```typescript
Inputs:
  - array: any[] (required) - 陣列

Outputs:
  - item: any (每個項目) - 當前項目
  - complete: any[] (完成) - 處理後的陣列
```

**Configuration Schema**:
```typescript
interface ForEachNodeConfig {
  itemVariable: string        // 項目變數名稱
  parallel: boolean           // 是否並行處理
  maxConcurrency?: number     // 最大並行數 (if parallel)
  orderPreserved: boolean     // 是否保持順序
  filterExpression?: string   // 過濾表達式
}
```

---

#### 2.6 ParallelNode (Parallel 節點)

**視覺設計**:
```yaml
Icon: ⚡
Color: #9C27B0 (紫色)
Size: 200px × 140px
Border: 2px solid #9C27B0
Background: #F3E5F5 (淺紫)
```

**Port 定義**:
```typescript
Inputs:
  - tasks: any[] (required) - 並行任務

Outputs:
  - results: any[] (always) - 所有結果
  - errors: any[] (conditional) - 錯誤列表
```

**Configuration Schema**:
```typescript
interface ParallelNodeConfig {
  maxConcurrency: number      // 最大並行數
  waitForAll: boolean         // 是否等待所有任務完成
  failFast: boolean           // 是否快速失敗
  timeout: number             // 整體超時時間
  individualTimeout: number   // 單一任務超時時間
}
```

---

### Category 3: 數據處理節點 (8 種)

#### 3.1 TransformNode (Transform 節點)

**視覺設計**:
```yaml
Icon: 🔄
Color: #00BCD4 (青色)
Size: 160px × 100px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - data: any (required) - 原始資料

Outputs:
  - transformed: any (always) - 轉換後資料
```

**Configuration Schema**:
```typescript
interface TransformNodeConfig {
  transformExpression: string // JavaScript 轉換表達式
  transformType: 'map' | 'custom' | 'template'
  template?: string           // 模板 (for template type)
  validateOutput: boolean     // 是否驗證輸出
  outputSchema?: object       // 輸出 Schema (Zod)
}
```

**範例**:
```javascript
// Transform Expression
(data) => ({
  id: data.userId,
  name: `${data.firstName} ${data.lastName}`,
  email: data.email.toLowerCase()
})
```

---

#### 3.2 FilterNode (Filter 節點)

**視覺設計**:
```yaml
Icon: 🔍
Color: #00BCD4 (青色)
Size: 160px × 100px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - items: any[] (required) - 項目陣列

Outputs:
  - filtered: any[] (always) - 過濾後陣列
  - rejected: any[] (optional) - 被過濾掉的項目
```

**Configuration Schema**:
```typescript
interface FilterNodeConfig {
  filterExpression: string    // 過濾表達式
  outputRejected: boolean     // 是否輸出被過濾項目
  caseSensitive: boolean      // 是否區分大小寫 (for string)
}
```

**範例**:
```javascript
// Filter Expression
(item) => item.age >= 18 && item.active === true
```

---

#### 3.3 MapNode (Map 節點)

**視覺設計**:
```yaml
Icon: 🗺️
Color: #00BCD4 (青色)
Size: 160px × 100px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - array: any[] (required) - 輸入陣列

Outputs:
  - mapped: any[] (always) - 映射後陣列
```

**Configuration Schema**:
```typescript
interface MapNodeConfig {
  mapExpression: string       // 映射表達式
  includeIndex: boolean       // 是否包含索引
  async: boolean              // 是否非同步映射
}
```

**範例**:
```javascript
// Map Expression
(item, index) => ({
  ...item,
  id: index + 1,
  processedAt: new Date().toISOString()
})
```

---

#### 3.4 ReduceNode (Reduce 節點)

**視覺設計**:
```yaml
Icon: ⬇️
Color: #00BCD4 (青色)
Size: 160px × 100px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - array: any[] (required) - 輸入陣列

Outputs:
  - result: any (always) - 聚合結果
```

**Configuration Schema**:
```typescript
interface ReduceNodeConfig {
  reduceExpression: string    // Reduce 表達式
  initialValue: any           // 初始值
  reduceType: 'sum' | 'average' | 'min' | 'max' | 'custom'
}
```

**範例**:
```javascript
// Reduce Expression
(accumulator, current) => accumulator + current.amount
```

---

#### 3.5 JoinNode (Join 節點)

**視覺設計**:
```yaml
Icon: 🔗
Color: #00BCD4 (青色)
Size: 180px × 120px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - left: any[] (required) - 左側資料
  - right: any[] (required) - 右側資料

Outputs:
  - joined: any[] (always) - 合併後資料
```

**Configuration Schema**:
```typescript
interface JoinNodeConfig {
  joinType: 'inner' | 'left' | 'right' | 'full'
  joinKey: {
    left: string              // 左側 key
    right: string             // 右側 key
  }
  outputFormat: 'merged' | 'separate'
}
```

---

#### 3.6 SplitNode (Split 節點)

**視覺設計**:
```yaml
Icon: ✂️
Color: #00BCD4 (青色)
Size: 160px × 100px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - data: any (required) - 輸入資料

Outputs:
  - part[1-N]: any (dynamic) - 分割部分
```

**Configuration Schema**:
```typescript
interface SplitNodeConfig {
  splitBy: 'size' | 'condition' | 'key'
  partCount?: number          // 分割數量 (for size)
  partSize?: number           // 每部分大小 (for size)
  conditions?: string[]       // 條件陣列 (for condition)
  key?: string                // 分組 key (for key)
}
```

---

#### 3.7 MergeNode (Merge 節點)

**視覺設計**:
```yaml
Icon: 🔀
Color: #00BCD4 (青色)
Size: 180px × 120px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - input[1-N]: any (dynamic) - 多個輸入

Outputs:
  - merged: any (always) - 合併結果
```

**Configuration Schema**:
```typescript
interface MergeNodeConfig {
  mergeStrategy: 'concat' | 'deep' | 'shallow' | 'custom'
  customMerge?: string        // 自訂合併邏輯
  conflictResolution: 'first' | 'last' | 'all'
  removeDuplicates: boolean
}
```

---

#### 3.8 AggregateNode (Aggregate 節點)

**視覺設計**:
```yaml
Icon: 📊
Color: #00BCD4 (青色)
Size: 180px × 120px
Border: 2px solid #00BCD4
Background: #E0F7FA (淺青)
```

**Port 定義**:
```typescript
Inputs:
  - data: any[] (required) - 輸入資料

Outputs:
  - aggregated: any (always) - 聚合結果
```

**Configuration Schema**:
```typescript
interface AggregateNodeConfig {
  aggregateFunction: 'count' | 'sum' | 'avg' | 'min' | 'max' | 'custom'
  groupBy?: string            // 分組欄位
  customFunction?: string     // 自訂聚合函數
  outputFormat: 'object' | 'array'
}
```

---

### Category 4: 整合節點 (5 種)

#### 4.1 ApiCallNode (API Call 節點)

**視覺設計**:
```yaml
Icon: 🌐
Color: #FF5722 (深橙)
Size: 180px × 120px
Border: 2px solid #FF5722
Background: #FBE9E7 (淺橙)
```

**Port 定義**:
```typescript
Inputs:
  - request: {
      url?: string
      method?: string
      headers?: object
      body?: any
    } (optional) - 動態請求參數

Outputs:
  - response: any (success) - API 回應
  - error: any (failure) - 錯誤資訊
```

**Configuration Schema**:
```typescript
interface ApiCallNodeConfig {
  url: string                 // API URL
  method: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH'
  headers: Record<string, string> // 請求標頭
  body?: any                  // 請求本文
  timeout: number             // 超時時間 (ms)
  retryPolicy: {
    maxRetries: number
    backoffMs: number
    retryOn: number[]         // HTTP 狀態碼
  }
  authentication: {
    type: 'none' | 'basic' | 'bearer' | 'apiKey'
    credentials?: object
  }
}
```

---

#### 4.2 DatabaseQueryNode (Database Query 節點)

**視覺設計**:
```yaml
Icon: 🗄️
Color: #FF5722 (深橙)
Size: 200px × 140px
Border: 2px solid #FF5722
Background: #FBE9E7 (淺橙)
```

**Port 定義**:
```typescript
Inputs:
  - query: string (optional) - 動態查詢
  - parameters: any[] (optional) - 查詢參數

Outputs:
  - result: any[] (success) - 查詢結果
  - error: any (failure) - 錯誤資訊
```

**Configuration Schema**:
```typescript
interface DatabaseQueryNodeConfig {
  connectionString: string    // 連線字串 (加密)
  query: string               // SQL 查詢
  queryType: 'select' | 'insert' | 'update' | 'delete'
  timeout: number             // 查詢超時 (秒)
  maxRows: number             // 最大回傳行數
  parameters: Array<{
    name: string
    type: 'string' | 'number' | 'boolean' | 'date'
    defaultValue?: any
  }>
}
```

**安全性**:
```yaml
- 使用參數化查詢防止 SQL Injection
- 連線字串加密儲存
- 唯讀連線 (for SELECT)
- Schema 白名單驗證
```

---

#### 4.3 FileIONode (File I/O 節點)

**視覺設計**:
```yaml
Icon: 📁
Color: #FF5722 (深橙)
Size: 180px × 120px
Border: 2px solid #FF5722
Background: #FBE9E7 (淺橙)
```

**Port 定義**:
```typescript
Inputs:
  - path: string (optional) - 檔案路徑
  - content: any (for write) - 寫入內容

Outputs:
  - data: any (for read) - 讀取資料
  - success: boolean (for write) - 寫入成功
  - error: any (failure) - 錯誤資訊
```

**Configuration Schema**:
```typescript
interface FileIONodeConfig {
  operation: 'read' | 'write' | 'append' | 'delete'
  path: string                // 檔案路徑
  encoding: 'utf8' | 'base64' | 'binary'
  format: 'json' | 'csv' | 'xml' | 'text'
  createDirIfNotExists: boolean
  overwrite: boolean          // for write
}
```

**安全性**:
```yaml
- 路徑白名單驗證
- 檔案大小限制
- 病毒掃描整合
- 存取權限檢查
```

---

#### 4.4 EmailNode (Email 節點)

**視覺設計**:
```yaml
Icon: 📧
Color: #FF5722 (深橙)
Size: 180px × 120px
Border: 2px solid #FF5722
Background: #FBE9E7 (淺橙)
```

**Port 定義**:
```typescript
Inputs:
  - to: string | string[] (optional) - 收件人
  - subject: string (optional) - 主題
  - body: string (optional) - 內文

Outputs:
  - sent: boolean (always) - 是否發送成功
  - messageId: string (success) - 訊息 ID
  - error: any (failure) - 錯誤資訊
```

**Configuration Schema**:
```typescript
interface EmailNodeConfig {
  from: string                // 寄件人
  to: string[]                // 收件人
  cc?: string[]               // 副本
  bcc?: string[]              // 密件副本
  subject: string             // 主題
  body: string                // 內文
  bodyType: 'text' | 'html'   // 內文格式
  template?: string           // 範本 ID
  attachments?: Array<{
    filename: string
    path: string
  }>
  priority: 'low' | 'normal' | 'high'
}
```

---

#### 4.5 WebhookNode (Webhook 節點)

**視覺設計**:
```yaml
Icon: 🪝
Color: #FF5722 (深橙)
Size: 180px × 120px
Border: 2px solid #FF5722
Background: #FBE9E7 (淺橙)
```

**Port 定義**:
```typescript
Inputs:
  - payload: any (required) - Webhook 資料

Outputs:
  - response: any (success) - Webhook 回應
  - error: any (failure) - 錯誤資訊
```

**Configuration Schema**:
```typescript
interface WebhookNodeConfig {
  url: string                 // Webhook URL
  method: 'POST' | 'PUT' | 'PATCH'
  headers: Record<string, string>
  authentication: {
    type: 'none' | 'basic' | 'bearer' | 'signature'
    credentials?: object
  }
  retryPolicy: {
    maxRetries: number
    backoffMs: number
    exponentialBackoff: boolean
  }
  validateResponse: boolean
  expectedStatus: number[]
}
```

---

### Category 5: 進階節點 (4 種)

#### 5.1 SubWorkflowNode (Sub-Workflow 節點)

**視覺設計**:
```yaml
Icon: 📦
Color: #607D8B (藍灰)
Size: 200px × 140px
Border: 2px solid #607D8B
Background: #ECEFF1 (淺藍灰)
```

**Port 定義**:
```typescript
Inputs:
  - parameters: any (required) - 子工作流參數

Outputs:
  - result: any (success) - 子工作流結果
  - error: any (failure) - 錯誤資訊
```

**Configuration Schema**:
```typescript
interface SubWorkflowNodeConfig {
  workflowId: string          // 子工作流 ID
  version?: string            // 版本 (optional, default: latest)
  mapping: {
    inputs: Record<string, string>  // 輸入映射
    outputs: Record<string, string> // 輸出映射
  }
  timeout: number             // 超時時間
  async: boolean              // 是否非同步執行
  waitForCompletion: boolean  // 是否等待完成
}
```

---

#### 5.2 HumanInLoopNode (Human-in-Loop 節點)

**視覺設計**:
```yaml
Icon: 👤
Color: #607D8B (藍灰)
Size: 180px × 140px
Border: 2px solid #607D8B
Background: #ECEFF1 (淺藍灰)
```

**Port 定義**:
```typescript
Inputs:
  - context: any (required) - 審批上下文

Outputs:
  - approved: any (approval) - 批准輸出
  - rejected: any (rejection) - 拒絕輸出
  - timeout: any (timeout) - 超時輸出
```

**Configuration Schema**:
```typescript
interface HumanInLoopNodeConfig {
  approvers: string[]         // 審批者
  approvalType: 'any' | 'all' | 'majority'
  timeout: number             // 超時時間 (小時)
  escalation: {
    enabled: boolean
    escalateTo: string[]
    escalateAfter: number     // 小時
  }
  form: {
    fields: Array<{
      name: string
      type: string
      label: string
      required: boolean
    }>
  }
  notification: {
    email: boolean
    slack: boolean
  }
}
```

---

#### 5.3 CodeExecutionNode (Code Execution 節點)

**視覺設計**:
```yaml
Icon: 💻
Color: #607D8B (藍灰)
Size: 200px × 140px
Border: 2px solid #607D8B
Background: #ECEFF1 (淺藍灰)
```

**Port 定義**:
```typescript
Inputs:
  - code: string (optional) - 動態程式碼
  - context: any (optional) - 執行上下文

Outputs:
  - result: any (success) - 執行結果
  - error: any (failure) - 錯誤資訊
  - stdout: string (always) - 標準輸出
  - stderr: string (always) - 標準錯誤
```

**Configuration Schema**:
```typescript
interface CodeExecutionNodeConfig {
  language: 'javascript' | 'python' | 'sql'
  code: string                // 程式碼
  timeout: number             // 執行超時 (秒)
  sandbox: {
    enabled: boolean
    memory: number            // MB
    cpu: number               // 核心數
  }
  allowedModules: string[]    // 允許的模組
  environmentVariables: Record<string, string>
}
```

**安全性**:
```yaml
- 沙箱執行 (Docker 隔離)
- 資源限制 (CPU, Memory, Time)
- 模組白名單
- 網絡隔離
- 程式碼審查 (AST 分析)
```

---

#### 5.4 CustomNode (Custom 節點)

**視覺設計**:
```yaml
Icon: 🛠️
Color: #607D8B (藍灰)
Size: 180px × 120px
Border: 2px dashed #607D8B
Background: #ECEFF1 (淺藍灰)
```

**Port 定義**:
```typescript
Inputs: 動態定義 (由使用者配置)
Outputs: 動態定義 (由使用者配置)
```

**Configuration Schema**:
```typescript
interface CustomNodeConfig {
  customType: string          // 自訂類型名稱
  icon?: string               // 自訂圖示
  color?: string              // 自訂顏色
  inputs: Array<{
    id: string
    label: string
    type: string
    required: boolean
  }>
  outputs: Array<{
    id: string
    label: string
    type: string
  }>
  handler: string             // 處理函數 (JavaScript)
  customConfig: any           // 自訂配置
}
```

**使用案例**:
```yaml
- 整合特定第三方 API
- 實作自訂業務邏輯
- 建立可重用的複合節點
- 擴展平台功能
```

---

## Node Configuration Panel 設計

### 內聯配置 (In-place Editing)

**觸發方式**:
```yaml
1. 雙擊節點
2. 選中節點後按 Enter
3. 節點右鍵選單 → Quick Edit
```

**UI 設計**:
```vue
<template>
  <div class="inline-config-overlay" v-show="visible">
    <!-- Quick Edit Form -->
    <el-form :model="config" size="small" label-position="top">
      <!-- 動態欄位 -->
      <template v-for="field in quickFields" :key="field.name">
        <el-form-item :label="field.label">
          <component
            :is="getFieldComponent(field.type)"
            v-model="config[field.name]"
            :placeholder="field.placeholder"
            :options="field.options"
          />
        </el-form-item>
      </template>

      <!-- Actions -->
      <div class="actions">
        <el-button size="small" @click="cancel">Cancel</el-button>
        <el-button size="small" type="primary" @click="save">Save</el-button>
        <el-button size="small" text @click="openSidebar">More Settings →</el-button>
      </div>
    </el-form>
  </div>
</template>

<style scoped>
.inline-config-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  z-index: 1000;
  min-width: 300px;
  max-width: 400px;
}
</style>
```

**快速欄位定義**:
```typescript
// 每個節點類型定義最多 3-5 個快速欄位
const quickFieldsMap = {
  AgentNode: [
    { name: 'agentId', type: 'select', label: 'Agent', required: true },
    { name: 'temperature', type: 'number', label: 'Temperature', min: 0, max: 2, step: 0.1 }
  ],
  PluginNode: [
    { name: 'pluginId', type: 'select', label: 'Plugin', required: true },
    { name: 'method', type: 'text', label: 'Method', required: true }
  ],
  // ... 其他節點類型
}
```

---

### 側邊欄配置 (Sidebar Panel)

**觸發方式**:
```yaml
1. 內聯配置的 "More Settings" 按鈕
2. 選中節點後點擊右側 Properties 面板
3. 節點右鍵選單 → Full Settings
```

**UI 結構**:
```vue
<template>
  <el-drawer
    v-model="visible"
    title="Node Configuration"
    :size="450"
    direction="rtl"
  >
    <!-- Node Info Header -->
    <div class="node-info-header">
      <span class="node-icon">{{ nodeIcon }}</span>
      <div class="node-details">
        <h3>{{ nodeLabel }}</h3>
        <p class="node-type">{{ nodeType }}</p>
      </div>
    </div>

    <!-- Tab Navigation -->
    <el-tabs v-model="activeTab" class="config-tabs">
      <!-- General Tab -->
      <el-tab-pane label="General" name="general">
        <el-form
          ref="formRef"
          :model="config"
          :rules="validationRules"
          label-position="top"
        >
          <template v-for="field in generalFields" :key="field.name">
            <el-form-item
              :label="field.label"
              :prop="field.name"
              :required="field.required"
            >
              <component
                :is="getFieldComponent(field.type)"
                v-model="config[field.name]"
                v-bind="field.props"
              />
              <div class="field-help" v-if="field.help">
                {{ field.help }}
              </div>
            </el-form-item>
          </template>
        </el-form>
      </el-tab-pane>

      <!-- Advanced Tab -->
      <el-tab-pane label="Advanced" name="advanced">
        <el-form :model="config" label-position="top">
          <template v-for="field in advancedFields" :key="field.name">
            <!-- Advanced fields -->
          </template>
        </el-form>
      </el-tab-pane>

      <!-- Preview Tab -->
      <el-tab-pane label="Preview" name="preview">
        <div class="config-preview">
          <h4>Configuration JSON</h4>
          <pre><code>{{ JSON.stringify(config, null, 2) }}</code></pre>

          <h4>Validation Status</h4>
          <el-alert
            v-if="validationErrors.length === 0"
            type="success"
            :closable="false"
          >
            Configuration is valid
          </el-alert>
          <el-alert
            v-else
            type="error"
            :closable="false"
          >
            <ul>
              <li v-for="error in validationErrors" :key="error.field">
                {{ error.message }}
              </li>
            </ul>
          </el-alert>
        </div>
      </el-tab-pane>

      <!-- Help Tab -->
      <el-tab-pane label="Help" name="help">
        <div class="node-help">
          <h4>Description</h4>
          <p>{{ nodeDescription }}</p>

          <h4>Usage Example</h4>
          <pre><code>{{ usageExample }}</code></pre>

          <h4>Common Issues</h4>
          <ul>
            <li v-for="issue in commonIssues" :key="issue.id">
              <strong>{{ issue.problem }}</strong>: {{ issue.solution }}
            </li>
          </ul>
        </div>
      </el-tab-pane>
    </el-tabs>

    <!-- Footer Actions -->
    <template #footer>
      <div class="drawer-footer">
        <el-button @click="cancel">Cancel</el-button>
        <el-button @click="reset">Reset</el-button>
        <el-button type="primary" @click="save" :loading="saving">
          Save Changes
        </el-button>
      </div>
    </template>
  </el-drawer>
</template>
```

**欄位組件映射**:
```typescript
const fieldComponentMap = {
  text: 'el-input',
  number: 'el-input-number',
  select: 'el-select',
  multiSelect: 'el-select',
  checkbox: 'el-checkbox',
  switch: 'el-switch',
  textarea: 'el-input',
  date: 'el-date-picker',
  time: 'el-time-picker',
  color: 'el-color-picker',
  slider: 'el-slider',
  radio: 'el-radio-group',
  code: 'CodeEditor',        // Monaco Editor
  json: 'JsonEditor',
  expression: 'ExpressionEditor',
  file: 'FileUpload'
}
```

---

### 屬性驗證系統

**驗證架構**:
```typescript
// packages/remote/src/validation/nodeConfigValidation.ts

import { z } from 'zod'

// 1. 定義基礎 Schema
const agentNodeSchema = z.object({
  agentId: z.string().min(1, 'Agent ID is required'),
  persona: z.string().min(1, 'Persona is required'),
  temperature: z.number().min(0).max(2),
  maxTokens: z.number().int().min(1).max(32000),
  timeout: z.number().int().min(1).max(300),
  retryPolicy: z.object({
    maxRetries: z.number().int().min(0).max(10),
    backoffMs: z.number().int().min(100).max(60000)
  })
})

// 2. 建立驗證規則映射
export const nodeValidationSchemas = {
  AgentNode: agentNodeSchema,
  PluginNode: pluginNodeSchema,
  // ... 其他節點
}

// 3. 驗證函數
export function validateNodeConfig(nodeType: string, config: any) {
  const schema = nodeValidationSchemas[nodeType]
  if (!schema) {
    throw new Error(`No validation schema for node type: ${nodeType}`)
  }

  try {
    schema.parse(config)
    return { valid: true, errors: [] }
  } catch (error) {
    if (error instanceof z.ZodError) {
      return {
        valid: false,
        errors: error.errors.map(e => ({
          field: e.path.join('.'),
          message: e.message
        }))
      }
    }
    throw error
  }
}

// 4. 實時驗證 Composable
export function useNodeConfigValidation(nodeType: string, config: Ref<any>) {
  const errors = ref<ValidationError[]>([])
  const isValid = computed(() => errors.value.length === 0)

  // 監聽配置變更
  watch(config, (newConfig) => {
    const result = validateNodeConfig(nodeType, newConfig)
    errors.value = result.errors
  }, { deep: true })

  return { errors, isValid }
}
```

**非同步驗證**:
```typescript
// 驗證 Agent ID 是否存在
async function validateAgentExists(agentId: string): Promise<boolean> {
  try {
    const response = await fetch(`/api/agents/${agentId}`)
    return response.ok
  } catch {
    return false
  }
}

// 整合到 Schema
const agentNodeSchema = z.object({
  agentId: z.string()
    .min(1, 'Agent ID is required')
    .refine(
      async (id) => await validateAgentExists(id),
      { message: 'Agent not found' }
    )
})
```

---

### 實時預覽功能

**配置預覽組件**:
```vue
<template>
  <div class="config-preview">
    <!-- JSON 預覽 -->
    <section class="json-preview">
      <h4>Configuration JSON</h4>
      <pre class="json-code"><code>{{ formattedConfig }}</code></pre>
      <el-button size="small" @click="copyToClipboard">
        Copy JSON
      </el-button>
    </section>

    <!-- 視覺預覽 -->
    <section class="visual-preview">
      <h4>Node Preview</h4>
      <div class="node-preview-container">
        <!-- 渲染節點預覽 -->
        <component
          :is="nodeComponent"
          :data="previewData"
          :config="config"
          preview-mode
        />
      </div>
    </section>

    <!-- 驗證狀態 -->
    <section class="validation-status">
      <h4>Validation</h4>
      <el-alert
        v-if="isValid"
        type="success"
        :closable="false"
      >
        ✅ Configuration is valid
      </el-alert>
      <el-alert
        v-else
        type="error"
        :closable="false"
      >
        <ul class="error-list">
          <li v-for="error in errors" :key="error.field">
            <strong>{{ error.field }}</strong>: {{ error.message }}
          </li>
        </ul>
      </el-alert>
    </section>

    <!-- 範例輸出 -->
    <section class="example-output" v-if="exampleOutput">
      <h4>Example Output</h4>
      <pre><code>{{ exampleOutput }}</code></pre>
    </section>
  </div>
</template>

<script setup lang="ts">
const formattedConfig = computed(() => {
  return JSON.stringify(props.config, null, 2)
})

const { errors, isValid } = useNodeConfigValidation(props.nodeType, toRef(props, 'config'))

function copyToClipboard() {
  navigator.clipboard.writeText(formattedConfig.value)
  ElMessage.success('Copied to clipboard')
}
</script>
```

---

## Connection Line Drawing 設計

### Bezier 曲線渲染

**Edge 組件**:
```vue
<!-- packages/remote/src/components/edges/CustomEdge.vue -->
<script setup lang="ts">
import { computed } from 'vue'
import { EdgeProps, getBezierPath } from '@vue-flow/core'

const props = defineProps<EdgeProps>()

// 計算 Bezier 路徑
const path = computed(() => {
  const [bezierPath] = getBezierPath({
    sourceX: props.sourceX,
    sourceY: props.sourceY,
    sourcePosition: props.sourcePosition,
    targetX: props.targetX,
    targetY: props.targetY,
    targetPosition: props.targetPosition,
    curvature: 0.25
  })
  return bezierPath
})

// Edge 樣式
const edgeStyle = computed(() => ({
  stroke: props.data?.color || '#b1b1b7',
  strokeWidth: props.selected ? 3 : 2,
  strokeDasharray: props.data?.animated ? '5,5' : 'none'
}))
</script>

<template>
  <g class="custom-edge">
    <!-- 主線條 -->
    <path
      :id="`edge-${props.id}`"
      :d="path"
      v-bind="edgeStyle"
      fill="none"
      class="edge-path"
    />

    <!-- 箭頭 -->
    <defs>
      <marker
        :id="`arrow-${props.id}`"
        markerWidth="10"
        markerHeight="10"
        refX="9"
        refY="3"
        orient="auto"
        markerUnits="strokeWidth"
      >
        <path d="M0,0 L0,6 L9,3 z" :fill="edgeStyle.stroke" />
      </marker>
    </defs>

    <!-- 動畫效果 (可選) -->
    <path
      v-if="data?.animated"
      :d="path"
      v-bind="edgeStyle"
      fill="none"
      class="edge-animation"
    >
      <animate
        attributeName="stroke-dashoffset"
        from="0"
        to="10"
        dur="0.5s"
        repeatCount="indefinite"
      />
    </path>

    <!-- 邊緣標籤 -->
    <EdgeLabelRenderer>
      <div
        v-if="props.label"
        class="edge-label"
        :style="edgeLabelStyle"
      >
        {{ props.label }}
      </div>
    </EdgeLabelRenderer>
  </g>
</template>
```

**曲率配置**:
```typescript
// 不同連線類型的曲率
const curvatureMap = {
  default: 0.25,        // 預設曲率
  straight: 0,          // 直線
  smooth: 0.5,          // 平滑曲線
  sharp: 0.1            // 銳利曲線
}
```

---

### Connection Rules 驗證

**連線規則引擎**:
```typescript
// packages/remote/src/validation/connectionRules.ts

export interface ConnectionRule {
  name: string
  validate: (source: Node, target: Node, connection: Connection) => boolean
  errorMessage: string
}

// 1. Port 類型相容性規則
const portTypeCompatibilityRule: ConnectionRule = {
  name: 'portTypeCompatibility',
  validate: (source, target, connection) => {
    const sourcePort = source.data.outputs.find(p => p.id === connection.sourceHandle)
    const targetPort = target.data.inputs.find(p => p.id === connection.targetHandle)

    if (!sourcePort || !targetPort) return false

    return arePortTypesCompatible(sourcePort.type, targetPort.type)
  },
  errorMessage: 'Port types are not compatible'
}

// 2. 循環依賴檢查
const circularDependencyRule: ConnectionRule = {
  name: 'circularDependency',
  validate: (source, target, connection) => {
    // 使用 DFS 檢查是否會形成循環
    return !hasCircularDependency(source, target)
  },
  errorMessage: 'This connection would create a circular dependency'
}

// 3. 最大連線數限制
const maxConnectionsRule: ConnectionRule = {
  name: 'maxConnections',
  validate: (source, target, connection) => {
    const sourcePort = source.data.outputs.find(p => p.id === connection.sourceHandle)
    const targetPort = target.data.inputs.find(p => p.id === connection.targetHandle)

    // 檢查是否超過最大連線數
    if (sourcePort.maxConnections && sourcePort.connections >= sourcePort.maxConnections) {
      return false
    }
    if (targetPort.maxConnections && targetPort.connections >= targetPort.maxConnections) {
      return false
    }

    return true
  },
  errorMessage: 'Maximum connections exceeded'
}

// 4. 必填輸入檢查
const requiredInputRule: ConnectionRule = {
  name: 'requiredInput',
  validate: (source, target, connection) => {
    const targetPort = target.data.inputs.find(p => p.id === connection.targetHandle)

    if (!targetPort.required) return true

    // 如果是必填輸入,檢查是否已有連線
    return targetPort.connections === 0
  },
  errorMessage: 'This input already has a connection'
}

// 整合所有規則
const connectionRules: ConnectionRule[] = [
  portTypeCompatibilityRule,
  circularDependencyRule,
  maxConnectionsRule,
  requiredInputRule
]

// 驗證連線
export function validateConnection(source: Node, target: Node, connection: Connection) {
  for (const rule of connectionRules) {
    if (!rule.validate(source, target, connection)) {
      return {
        valid: false,
        error: rule.errorMessage,
        ruleName: rule.name
      }
    }
  }

  return { valid: true }
}
```

**Port 類型相容性**:
```typescript
// Port 類型定義
type PortType = 'string' | 'number' | 'boolean' | 'object' | 'array' | 'any' | 'void'

// 類型相容性矩陣
const typeCompatibilityMatrix: Record<PortType, PortType[]> = {
  any: ['string', 'number', 'boolean', 'object', 'array', 'any'],
  string: ['string', 'any'],
  number: ['number', 'any'],
  boolean: ['boolean', 'any'],
  object: ['object', 'array', 'any'],
  array: ['array', 'object', 'any'],
  void: []
}

function arePortTypesCompatible(sourceType: PortType, targetType: PortType): boolean {
  return typeCompatibilityMatrix[sourceType]?.includes(targetType) ?? false
}
```

**循環依賴檢查**:
```typescript
function hasCircularDependency(source: Node, target: Node): boolean {
  const visited = new Set<string>()
  const stack = [target.id]

  while (stack.length > 0) {
    const currentId = stack.pop()!

    if (currentId === source.id) {
      return true  // 發現循環
    }

    if (visited.has(currentId)) {
      continue
    }

    visited.add(currentId)

    // 獲取當前節點的所有輸出連線
    const outgoingConnections = getOutgoingConnections(currentId)
    outgoingConnections.forEach(conn => {
      stack.push(conn.target)
    })
  }

  return false
}
```

---

### Port Highlighting

**Port 高亮系統**:
```vue
<script setup lang="ts">
import { ref, computed } from 'vue'
import { useVueFlow } from '@vue-flow/core'

const { onConnect, onConnectStart, onConnectEnd } = useVueFlow()

const connectingPort = ref<{ nodeId: string, portId: string } | null>(null)

onConnectStart((event) => {
  connectingPort.value = {
    nodeId: event.nodeId,
    portId: event.handleId
  }
})

onConnectEnd(() => {
  connectingPort.value = null
})

// 計算 Port 樣式
function getPortStyle(node: Node, port: Port) {
  if (!connectingPort.value) {
    return { opacity: 1 }
  }

  // 檢查是否可連線
  const canConnect = validateConnection(
    connectingPort.value.nodeId,
    node.id,
    connectingPort.value.portId,
    port.id
  )

  return {
    opacity: canConnect ? 1 : 0.3,
    borderColor: canConnect ? '#4CAF50' : '#F44336',
    borderWidth: canConnect ? '2px' : '1px',
    cursor: canConnect ? 'pointer' : 'not-allowed'
  }
}
</script>

<template>
  <div class="node-port" :style="getPortStyle(node, port)">
    <div class="port-indicator" />
    <div class="port-label">{{ port.label }}</div>
  </div>
</template>
```

**Port 樣式**:
```scss
.node-port {
  position: relative;
  padding: 4px 8px;
  transition: all 0.2s ease;

  .port-indicator {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    border: 2px solid #b1b1b7;
    background: white;
    transition: all 0.2s ease;
  }

  &:hover .port-indicator {
    transform: scale(1.2);
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.2);
  }

  &.compatible .port-indicator {
    border-color: #4CAF50;
    background: #E8F5E9;
  }

  &.incompatible .port-indicator {
    border-color: #F44336;
    background: #FFEBEE;
  }
}
```

---

### Connection Animation

**動畫效果**:
```vue
<template>
  <g class="animated-edge">
    <!-- 背景線條 -->
    <path
      :d="path"
      stroke="#b1b1b7"
      stroke-width="2"
      fill="none"
    />

    <!-- 動畫線條 -->
    <path
      :d="path"
      stroke="#4CAF50"
      stroke-width="2"
      fill="none"
      stroke-dasharray="5,5"
      class="animated-line"
    >
      <animate
        attributeName="stroke-dashoffset"
        from="0"
        to="10"
        dur="0.5s"
        repeatCount="indefinite"
      />
    </path>

    <!-- 移動的點 -->
    <circle r="3" fill="#4CAF50">
      <animateMotion
        :path="path"
        dur="2s"
        repeatCount="indefinite"
      />
    </circle>
  </g>
</template>
```

**動畫配置**:
```typescript
interface EdgeAnimationConfig {
  enabled: boolean
  type: 'dash' | 'dot' | 'pulse'
  speed: 'slow' | 'normal' | 'fast'
  color: string
}

const animationSpeedMap = {
  slow: '3s',
  normal: '2s',
  fast: '1s'
}
```

---

# 第二部分: 如何實施

## Phase 1: 28 種節點類型實作

### Week 37 (Day 1-5): 基礎與控制流節點 (11 種)

**Day 1-2: 基礎節點 (5 種)**

**任務分配**:
```yaml
Task T13.001: AgentNode 實作
  Time: 4 hours
  Steps:
    1. 建立組件檔案 AgentNode.vue (1h)
    2. 實作 Port 定義和配置 Schema (1h)
    3. 建立驗證規則 (1h)
    4. 撰寫單元測試 (1h)

Task T13.002: PluginNode 實作
  Time: 3 hours

Task T13.003: DecisionNode 實作
  Time: 3 hours

Task T13.004: StartNode 實作
  Time: 2 hours

Task T13.005: EndNode 實作
  Time: 2 hours
```

**實作範本**:
```vue
<!-- packages/remote/src/components/nodes/basic/AgentNode.vue -->
<script setup lang="ts">
import { computed } from 'vue'
import { Handle, Position } from '@vue-flow/core'
import type { NodeProps } from '@vue-flow/core'
import type { AgentNodeConfig } from '@/types/nodes'

const props = defineProps<NodeProps<AgentNodeConfig>>()

const config = computed(() => props.data.config)
const isValid = computed(() => props.data.isValid)
</script>

<template>
  <div class="agent-node" :class="{ invalid: !isValid }">
    <!-- Node Header -->
    <div class="node-header">
      <span class="node-icon">🤖</span>
      <span class="node-label">{{ data.label || 'Agent' }}</span>
    </div>

    <!-- Node Body -->
    <div class="node-body">
      <div class="node-info">
        <span class="info-label">Agent:</span>
        <span class="info-value">{{ config.agentId }}</span>
      </div>
    </div>

    <!-- Input Handles -->
    <Handle
      id="context"
      type="target"
      :position="Position.Left"
      class="node-handle input-handle"
    />

    <!-- Output Handles -->
    <Handle
      id="result"
      type="source"
      :position="Position.Right"
      class="node-handle output-handle"
    />
  </div>
</template>

<style scoped>
.agent-node {
  width: 180px;
  min-height: 120px;
  border: 2px solid #4CAF50;
  border-radius: 8px;
  background: #E8F5E9;
  padding: 12px;
}

.node-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 8px;
}

.node-icon {
  font-size: 20px;
}

.node-label {
  font-weight: 600;
  color: #2E7D32;
}

.node-body {
  font-size: 12px;
}

.invalid {
  border-color: #F44336;
}
</style>
```

**測試範本**:
```typescript
// packages/remote/src/components/nodes/basic/AgentNode.spec.ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import AgentNode from './AgentNode.vue'

describe('AgentNode', () => {
  it('renders correctly', () => {
    const wrapper = mount(AgentNode, {
      props: {
        id: 'node1',
        data: {
          label: 'Test Agent',
          config: {
            agentId: 'agent1',
            temperature: 0.7
          }
        }
      }
    })

    expect(wrapper.find('.agent-node').exists()).toBe(true)
    expect(wrapper.find('.node-label').text()).toBe('Test Agent')
  })

  it('displays validation error when invalid', () => {
    const wrapper = mount(AgentNode, {
      props: {
        id: 'node1',
        data: {
          config: {},
          isValid: false
        }
      }
    })

    expect(wrapper.find('.invalid').exists()).toBe(true)
  })
})
```

---

**Day 3-5: 控制流節點 (6 種)**

**任務分配**:
```yaml
Task T13.006: IfElseNode 實作 (4h)
Task T13.007: SwitchNode 實作 (5h)
Task T13.008: LoopNode 實作 (5h)
Task T13.009: WhileNode 實作 (4h)
Task T13.010: ForEachNode 實作 (5h)
Task T13.011: ParallelNode 實作 (5h)
```

**動態 Port 處理 (SwitchNode 範例)**:
```vue
<script setup lang="ts">
const outputPorts = computed(() => {
  const ports = []

  // 動態 case ports
  config.value.cases.forEach((caseItem, index) => {
    ports.push({
      id: `case${index + 1}`,
      label: caseItem.label,
      type: 'any'
    })
  })

  // Default port
  if (config.value.defaultCase.enabled) {
    ports.push({
      id: 'default',
      label: config.value.defaultCase.label,
      type: 'any'
    })
  }

  return ports
})
</script>

<template>
  <!-- Dynamic Output Handles -->
  <Handle
    v-for="port in outputPorts"
    :key="port.id"
    :id="port.id"
    type="source"
    :position="Position.Right"
    :style="{ top: `${getPortYPosition(port.id)}px` }"
  >
    <span class="port-label">{{ port.label }}</span>
  </Handle>
</template>
```

---

### Week 38 (Day 6-10): 數據處理與整合節點 (13 種)

**Day 6-8: 數據處理節點 (8 種)**

**任務分配**:
```yaml
Task T13.012: TransformNode (4h)
Task T13.013: FilterNode (4h)
Task T13.014: MapNode (4h)
Task T13.015: ReduceNode (5h)
Task T13.016: JoinNode (6h)
Task T13.017: SplitNode (5h)
Task T13.018: MergeNode (5h)
Task T13.019: AggregateNode (5h)
```

**表達式編輯器整合**:
```vue
<!-- 用於 Transform/Filter/Map 等節點的配置 -->
<template>
  <el-form-item label="Transform Expression">
    <CodeEditor
      v-model="config.transformExpression"
      language="javascript"
      :height="200"
      :options="{
        minimap: { enabled: false },
        lineNumbers: 'on',
        theme: 'vs-light'
      }"
    />
  </el-form-item>
</template>
```

---

**Day 9-10: 整合節點 (5 種)**

**任務分配**:
```yaml
Task T13.020: ApiCallNode (6h)
Task T13.021: DatabaseQueryNode (7h)
Task T13.022: FileIONode (5h)
Task T13.023: EmailNode (5h)
Task T13.024: WebhookNode (5h)
```

**安全性實作 (DatabaseQueryNode)**:
```typescript
// SQL Injection 防護
function sanitizeQuery(query: string, parameters: any[]): { query: string, params: any[] } {
  // 使用參數化查詢
  const parameterizedQuery = query.replace(/\$(\d+)/g, (match, index) => {
    return `?`
  })

  return {
    query: parameterizedQuery,
    params: parameters
  }
}

// Schema 白名單驗證
function validateSchemaAccess(query: string, allowedSchemas: string[]): boolean {
  const schemaPattern = /FROM\s+([a-zA-Z0-9_]+)\./gi
  const matches = query.matchAll(schemaPattern)

  for (const match of matches) {
    const schema = match[1]
    if (!allowedSchemas.includes(schema)) {
      return false
    }
  }

  return true
}
```

---

### Week 39 (Day 11-15): 進階節點與整合測試 (4 種)

**Day 11-12: 進階節點 (4 種)**

**任務分配**:
```yaml
Task T13.025: SubWorkflowNode (6h)
Task T13.026: HumanInLoopNode (6h)
Task T13.027: CodeExecutionNode (8h)
Task T13.028: CustomNode (6h)
```

**CodeExecutionNode 沙箱整合**:
```typescript
// Code Execution with Sandbox
async function executeCodeInSandbox(code: string, context: any, config: CodeExecutionNodeConfig) {
  const payload = {
    language: config.language,
    code: code,
    context: context,
    timeout: config.timeout,
    sandbox: config.sandbox
  }

  try {
    const response = await fetch('/api/sandbox/execute', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    })

    const result = await response.json()

    return {
      result: result.output,
      stdout: result.stdout,
      stderr: result.stderr,
      error: null
    }
  } catch (error) {
    return {
      result: null,
      stdout: '',
      stderr: '',
      error: error.message
    }
  }
}
```

---

**Day 13-15: 整合測試與文檔**

**任務分配**:
```yaml
Task T13.029: 節點註冊系統整合測試 (4h)
Task T13.030: 28 種節點拖放測試 (6h)
Task T13.031: 節點連線測試 (5h)
Task T13.032: 配置持久化測試 (4h)
Task T13.033: 28 種節點使用文檔 (8h)
```

---

## Phase 2: Node Configuration Panel

### Week 37 (Day 1-3): 內聯配置實作

**Task T13.034: InlineConfigPanel 組件** (8h)

**實作步驟**:
```yaml
Step 1: 建立 InlineConfigPanel 組件 (2h)
  - 設計 overlay UI
  - 實作開啟/關閉邏輯

Step 2: 動態欄位渲染 (3h)
  - 根據節點類型載入快速欄位
  - 實作欄位組件映射

Step 3: 配置儲存與取消 (2h)
  - 實作儲存邏輯
  - 實作取消與重置

Step 4: 切換到側邊欄 (1h)
  - "More Settings" 按鈕
  - 狀態傳遞
```

**組件程式碼**:
```vue
<!-- packages/remote/src/components/config/InlineConfigPanel.vue -->
<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import { useNodeStore } from '@/stores/nodeStore'

const props = defineProps<{
  nodeId: string
  visible: boolean
}>()

const emit = defineEmits(['update:visible', 'open-sidebar'])

const nodeStore = useNodeStore()
const node = computed(() => nodeStore.getNodeById(props.nodeId))
const config = ref({ ...node.value?.data.config })

const quickFields = computed(() => {
  return nodeStore.getQuickFields(node.value?.type)
})

function save() {
  nodeStore.updateNodeConfig(props.nodeId, config.value)
  emit('update:visible', false)
}

function cancel() {
  config.value = { ...node.value?.data.config }
  emit('update:visible', false)
}

function openSidebar() {
  emit('open-sidebar')
}
</script>

<template>
  <Teleport to="body">
    <div v-show="visible" class="inline-config-overlay">
      <div class="overlay-backdrop" @click="cancel" />

      <div class="overlay-content">
        <h3 class="overlay-title">Quick Edit</h3>

        <el-form :model="config" size="small" label-position="top">
          <template v-for="field in quickFields" :key="field.name">
            <el-form-item :label="field.label" :required="field.required">
              <component
                :is="getFieldComponent(field.type)"
                v-model="config[field.name]"
                v-bind="field.props"
              />
            </el-form-item>
          </template>
        </el-form>

        <div class="overlay-actions">
          <el-button size="small" @click="cancel">Cancel</el-button>
          <el-button size="small" type="primary" @click="save">Save</el-button>
          <el-button size="small" text @click="openSidebar">
            More Settings →
          </el-button>
        </div>
      </div>
    </div>
  </Teleport>
</template>
```

---

### Week 37-38 (Day 4-8): 側邊欄配置實作

**Task T13.035: SidebarConfigPanel 組件** (16h)

**實作步驟**:
```yaml
Step 1: Drawer 基礎結構 (2h)
Step 2: Tab Navigation 實作 (2h)
Step 3: General Tab - 動態表單 (4h)
Step 4: Advanced Tab 實作 (3h)
Step 5: Preview Tab 實作 (2h)
Step 6: Help Tab 實作 (2h)
Step 7: Footer Actions 整合 (1h)
```

**組件程式碼** (詳見前述 Sidebar Panel 設計章節)

---

### Week 38 (Day 9-10): 屬性驗證系統

**Task T13.036: Zod Schema 定義** (12h)

**實作步驟**:
```yaml
Step 1: 安裝與配置 Zod (1h)
Step 2: 定義 28 種節點的驗證 Schema (8h)
Step 3: 建立驗證函數 (2h)
Step 4: 實時驗證 Composable (1h)
```

**非同步驗證整合**:
```typescript
// Task T13.037: 非同步驗證 (4h)
const agentNodeSchema = z.object({
  agentId: z.string()
    .min(1)
    .refine(
      async (id) => {
        const exists = await checkAgentExists(id)
        return exists
      },
      { message: 'Agent not found' }
    ),
  persona: z.string().min(1),
  temperature: z.number().min(0).max(2)
})
```

---

### Week 39 (Day 11-12): 實時預覽功能

**Task T13.038: ConfigPreview 組件** (8h)

**實作內容**:
```vue
<template>
  <div class="config-preview">
    <!-- JSON Preview -->
    <section>
      <h4>Configuration JSON</h4>
      <pre><code>{{ formattedConfig }}</code></pre>
    </section>

    <!-- Visual Preview -->
    <section>
      <h4>Node Preview</h4>
      <div class="preview-canvas">
        <component
          :is="nodeComponent"
          :data="previewData"
          preview-mode
        />
      </div>
    </section>

    <!-- Validation Status -->
    <section>
      <h4>Validation</h4>
      <ValidationStatus :errors="errors" />
    </section>
  </div>
</template>
```

---

## Phase 3: Connection Line Drawing

### Week 39 (Day 13-14): Bezier 曲線與規則驗證

**Task T13.039: CustomEdge 組件** (8h)

**實作步驟**:
```yaml
Step 1: 基礎 Bezier 路徑計算 (2h)
Step 2: 動態樣式系統 (2h)
Step 3: 箭頭標記實作 (2h)
Step 4: Edge 標籤渲染 (2h)
```

**Task T13.040: Connection Rules Engine** (12h)

**實作步驟**:
```yaml
Step 1: Port 類型相容性 (3h)
Step 2: 循環依賴檢查 (4h)
Step 3: 最大連線數限制 (2h)
Step 4: 必填輸入檢查 (2h)
Step 5: 規則整合與測試 (1h)
```

---

### Week 39 (Day 15): Port Highlighting 與動畫

**Task T13.041: Port Highlighting** (6h)

**實作內容**:
```typescript
// Port 高亮邏輯
const portStyle = computed(() => {
  if (!connecting.value) return {}

  const canConnect = validateConnection(...)

  return {
    borderColor: canConnect ? '#4CAF50' : '#F44336',
    borderWidth: '2px',
    transform: canConnect ? 'scale(1.2)' : 'scale(1)'
  }
})
```

**Task T13.042: Connection Animation** (6h)

**動畫類型**:
```yaml
1. Dash Animation: 虛線流動動畫
2. Dot Animation: 移動的點
3. Pulse Animation: 脈衝效果
```

---

## Phase 4: Testing & Documentation

### Week 39 (Day 15): 整合測試

**Task T13.043: E2E 測試** (8h)

**測試場景**:
```typescript
// packages/remote/tests/e2e/workflow-node-management.spec.ts

describe('Workflow Node Management E2E', () => {
  test('should drag and drop all 28 node types', async ({ page }) => {
    // 測試所有節點類型的拖放
    for (const nodeType of ALL_NODE_TYPES) {
      await dragNodeToCanvas(page, nodeType)
      await expect(page.locator(`.${nodeType}`)).toBeVisible()
    }
  })

  test('should configure node using inline panel', async ({ page }) => {
    await dragNodeToCanvas(page, 'AgentNode')
    await page.dblclick('.agent-node')

    // 配置節點
    await page.fill('[name="agentId"]', 'agent1')
    await page.click('button:has-text("Save")')

    // 驗證配置已保存
    const config = await getNodeConfig(page, 'node1')
    expect(config.agentId).toBe('agent1')
  })

  test('should validate connection rules', async ({ page }) => {
    // 建立不相容的連線
    await createConnection(page, 'node1-output', 'node2-input')

    // 應顯示錯誤
    await expect(page.locator('.error-message')).toContainText('Port types are not compatible')
  })
})
```

---

### Week 39 (Day 15): 文檔撰寫

**Task T13.044: 使用文檔** (8h)

**文檔結構**:
```markdown
# 工作流節點使用指南

## 1. 基礎節點
### 1.1 Agent 節點
- 用途: 執行 AI Agent
- 配置項目:
  - Agent ID
  - Persona
  - Temperature
- 使用範例

### 1.2 Plugin 節點
...

## 2. 控制流節點
...

## 3. 數據處理節點
...

## 4. 整合節點
...

## 5. 進階節點
...

## 常見問題 (FAQ)
Q: 如何建立自訂節點?
A: ...
```

---

# 第三部分: 編碼規範

## 編碼標準與最佳實踐

### Vue 3 組件規範

**組件結構**:
```vue
<script setup lang="ts">
// 1. Imports
import { ref, computed, watch } from 'vue'
import type { NodeProps } from '@vue-flow/core'

// 2. Types
interface Props extends NodeProps<AgentNodeConfig> {}

// 3. Props & Emits
const props = defineProps<Props>()
const emit = defineEmits<{
  'update:data': [data: any]
}>()

// 4. Reactive State
const config = ref(props.data.config)

// 5. Computed
const isValid = computed(() => validateConfig(config.value))

// 6. Watchers
watch(config, (newConfig) => {
  emit('update:data', { config: newConfig })
}, { deep: true })

// 7. Methods
function handleConfigChange() {
  // ...
}

// 8. Lifecycle (if needed)
onMounted(() => {
  // ...
})
</script>

<template>
  <!-- Template -->
</template>

<style scoped>
/* Styles */
</style>
```

**命名規範**:
```yaml
組件檔案: PascalCase (AgentNode.vue)
組件 class: kebab-case (agent-node)
Props: camelCase (nodeData)
Events: kebab-case (update-config)
CSS Variables: --prefix-name
```

---

### TypeScript 型別定義

**檔案組織**:
```typescript
// packages/remote/src/types/nodes.ts

// 1. 基礎型別
export type PortType = 'string' | 'number' | 'boolean' | 'object' | 'array' | 'any' | 'void'

// 2. 介面定義
export interface PortDefinition {
  id: string
  label: string
  type: PortType
  required: boolean
  multiple: boolean
  maxConnections?: number
}

export interface NodeTypeDefinition {
  type: string
  category: NodeCategory
  label: string
  icon: string
  description: string
  inputs: PortDefinition[]
  outputs: PortDefinition[]
  configSchema: ConfigSchema
  defaultConfig: Record<string, any>
}

// 3. 配置型別 (每個節點)
export interface AgentNodeConfig {
  agentId: string
  persona: string
  temperature: number
  maxTokens: number
  timeout: number
  retryPolicy: RetryPolicy
}

// ... 其他 27 種節點配置型別
```

---

### Pinia Store 規範

**Store 結構**:
```typescript
// packages/remote/src/stores/nodeStore.ts

export const useNodeStore = defineStore('node', {
  // 1. State
  state: () => ({
    nodeTypes: new Map<string, NodeTypeDefinition>(),
    nodes: new Map<string, Node>(),
    selectedNode: null as Node | null,
    configPanel: {
      visible: false,
      mode: 'sidebar' as 'inline' | 'sidebar',
      nodeId: null as string | null
    }
  }),

  // 2. Getters
  getters: {
    getNodeById: (state) => (id: string) => {
      return state.nodes.get(id)
    },

    nodesByCategory: (state) => (category: NodeCategory) => {
      return Array.from(state.nodeTypes.values())
        .filter(node => node.category === category)
    }
  },

  // 3. Actions
  actions: {
    registerNodeType(definition: NodeTypeDefinition) {
      this.nodeTypes.set(definition.type, definition)
    },

    addNode(node: Node) {
      this.nodes.set(node.id, node)
    },

    updateNodeConfig(nodeId: string, config: any) {
      const node = this.nodes.get(nodeId)
      if (node) {
        node.data.config = config
      }
    },

    openConfigPanel(nodeId: string, mode: 'inline' | 'sidebar') {
      this.configPanel = { visible: true, mode, nodeId }
    },

    closeConfigPanel() {
      this.configPanel.visible = false
    }
  }
})
```

---

### CSS/SCSS 規範

**樣式組織**:
```scss
// 1. Variables
$node-border-radius: 8px;
$node-padding: 12px;
$transition-speed: 0.2s;

// 2. Component Root
.agent-node {
  width: 180px;
  min-height: 120px;
  border-radius: $node-border-radius;
  padding: $node-padding;
  transition: all $transition-speed ease;

  // 3. Nested Elements
  .node-header {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .node-icon {
    font-size: 20px;
  }

  // 4. Modifiers
  &.selected {
    box-shadow: 0 0 0 3px #4CAF50;
  }

  &.invalid {
    border-color: #F44336;
  }

  // 5. States
  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
}
```

---

### Git Commit 規範

**Commit Message 格式**:
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
```yaml
feat: 新功能 (feat(nodes): add AgentNode component)
fix: 錯誤修復 (fix(config): fix validation error)
docs: 文檔變更 (docs(nodes): add usage examples)
style: 程式碼格式 (style: format AgentNode.vue)
refactor: 重構 (refactor(validation): simplify connection rules)
test: 測試相關 (test(nodes): add AgentNode tests)
chore: 建置/工具 (chore: update dependencies)
```

**範例**:
```
feat(nodes): implement AgentNode with configuration panel

- Add AgentNode component with drag & drop support
- Implement inline and sidebar configuration
- Add Zod validation schema
- Write unit and integration tests

Closes #T13.001
```

---

# 第四部分: 質量保證

## 測試策略

### 測試金字塔

```
       /\
      /E2E\       10% - End-to-End Tests
     /------\
    /Integr-\    30% - Integration Tests
   /----------\
  /Unit Tests \  60% - Unit Tests
 /--------------\
```

### 單元測試 (60%)

**測試範圍**:
```yaml
1. 節點組件 (28 個):
   - Props 渲染測試
   - 配置變更測試
   - 驗證狀態測試
   - Port 渲染測試

2. 配置面板:
   - 欄位渲染測試
   - 驗證邏輯測試
   - 儲存/取消測試

3. 驗證系統:
   - Schema 驗證測試
   - 非同步驗證測試
   - 錯誤訊息測試

4. Connection Rules:
   - 類型相容性測試
   - 循環依賴測試
   - 連線限制測試
```

**測試範例**:
```typescript
// packages/remote/src/components/nodes/basic/AgentNode.spec.ts

import { describe, it, expect, beforeEach } from 'vitest'
import { mount } from '@vue/test-utils'
import { createPinia } from 'pinia'
import AgentNode from './AgentNode.vue'

describe('AgentNode', () => {
  let wrapper
  const pinia = createPinia()

  beforeEach(() => {
    wrapper = mount(AgentNode, {
      global: {
        plugins: [pinia]
      },
      props: {
        id: 'node1',
        data: {
          label: 'Test Agent',
          config: {
            agentId: 'agent1',
            persona: 'Assistant',
            temperature: 0.7,
            maxTokens: 2000,
            timeout: 30,
            retryPolicy: {
              maxRetries: 3,
              backoffMs: 1000
            }
          }
        }
      }
    })
  })

  it('renders correctly', () => {
    expect(wrapper.find('.agent-node').exists()).toBe(true)
    expect(wrapper.find('.node-label').text()).toBe('Test Agent')
    expect(wrapper.find('.node-icon').text()).toBe('🤖')
  })

  it('displays agent configuration', () => {
    expect(wrapper.find('.info-value').text()).toBe('agent1')
  })

  it('renders input and output handles', () => {
    const handles = wrapper.findAllComponents({ name: 'Handle' })
    expect(handles.length).toBeGreaterThanOrEqual(2)
  })

  it('applies invalid class when validation fails', async () => {
    await wrapper.setProps({
      data: {
        ...wrapper.props('data'),
        isValid: false
      }
    })

    expect(wrapper.find('.invalid').exists()).toBe(true)
  })

  it('emits config change event', async () => {
    const newConfig = { ...wrapper.props('data').config, temperature: 0.5 }

    // Trigger config change (implementation depends on component)
    await wrapper.vm.updateConfig(newConfig)

    expect(wrapper.emitted('update:data')).toBeTruthy()
    expect(wrapper.emitted('update:data')[0][0].config.temperature).toBe(0.5)
  })
})
```

---

### 整合測試 (30%)

**測試範圍**:
```yaml
1. 節點註冊與初始化:
   - 28 種節點類型註冊
   - 預設配置載入
   - Store 整合

2. 配置面板互動:
   - 開啟/關閉流程
   - 模式切換 (inline ↔ sidebar)
   - 配置儲存與驗證

3. 連線建立:
   - 拖放連線
   - 規則驗證
   - 連線儲存

4. 工作流持久化:
   - 配置儲存到 JSON
   - JSON 載入還原
   - 自動儲存機制
```

**測試範例**:
```typescript
// packages/remote/tests/integration/node-configuration.spec.ts

import { describe, it, expect, beforeEach } from 'vitest'
import { mount } from '@vue/test-utils'
import { createPinia } from 'pinia'
import { useNodeStore } from '@/stores/nodeStore'
import WorkflowCanvas from '@/components/WorkflowCanvas.vue'
import InlineConfigPanel from '@/components/config/InlineConfigPanel.vue'

describe('Node Configuration Integration', () => {
  let pinia
  let nodeStore

  beforeEach(() => {
    pinia = createPinia()
    nodeStore = useNodeStore(pinia)
  })

  it('should open inline config panel on double click', async () => {
    const wrapper = mount(WorkflowCanvas, {
      global: { plugins: [pinia] }
    })

    // Add a node
    const node = {
      id: 'node1',
      type: 'AgentNode',
      data: {
        label: 'Test Agent',
        config: { agentId: 'agent1' }
      }
    }
    nodeStore.addNode(node)

    // Double click node
    await wrapper.find('.agent-node').trigger('dblclick')

    // Config panel should be visible
    expect(nodeStore.configPanel.visible).toBe(true)
    expect(nodeStore.configPanel.mode).toBe('inline')
    expect(nodeStore.configPanel.nodeId).toBe('node1')
  })

  it('should switch from inline to sidebar config', async () => {
    const wrapper = mount(InlineConfigPanel, {
      global: { plugins: [pinia] },
      props: {
        nodeId: 'node1',
        visible: true
      }
    })

    // Click "More Settings"
    await wrapper.find('button:contains("More Settings")').trigger('click')

    // Should emit open-sidebar event
    expect(wrapper.emitted('open-sidebar')).toBeTruthy()
  })

  it('should validate and save configuration', async () => {
    const wrapper = mount(InlineConfigPanel, {
      global: { plugins: [pinia] },
      props: {
        nodeId: 'node1',
        visible: true
      }
    })

    // Change configuration
    await wrapper.find('[name="temperature"]').setValue(1.5)

    // Click save
    await wrapper.find('button:contains("Save")').trigger('click')

    // Configuration should be updated in store
    const node = nodeStore.getNodeById('node1')
    expect(node.data.config.temperature).toBe(1.5)
  })
})
```

---

### E2E 測試 (10%)

**關鍵流程測試**:
```typescript
// packages/remote/tests/e2e/complete-workflow-creation.spec.ts

import { test, expect } from '@playwright/test'

test.describe('Complete Workflow Creation', () => {
  test('should create a complete workflow with all node types', async ({ page }) => {
    await page.goto('http://localhost:3001/workflow/new')

    // Step 1: Drag Agent Node
    await page.dragAndDrop(
      '[data-node-type="AgentNode"]',
      '.workflow-canvas',
      { targetPosition: { x: 200, y: 100 } }
    )

    // Step 2: Configure Agent Node
    await page.dblclick('.agent-node')
    await page.fill('[name="agentId"]', 'agent1')
    await page.fill('[name="temperature"]', '0.7')
    await page.click('button:has-text("Save")')

    // Step 3: Drag Plugin Node
    await page.dragAndDrop(
      '[data-node-type="PluginNode"]',
      '.workflow-canvas',
      { targetPosition: { x: 450, y: 100 } }
    )

    // Step 4: Create Connection
    await page.hover('.agent-node [data-handleid="result"]')
    await page.mouse.down()
    await page.hover('.plugin-node [data-handleid="input"]')
    await page.mouse.up()

    // Verify connection created
    await expect(page.locator('.vue-flow__edge')).toBeVisible()

    // Step 5: Save Workflow
    await page.click('button:has-text("Save Workflow")')
    await page.fill('[name="workflowName"]', 'Test Workflow')
    await page.click('button:has-text("Confirm")')

    // Verify save success
    await expect(page.locator('.success-message')).toContainText('Workflow saved')
  })

  test('should validate connection rules', async ({ page }) => {
    await page.goto('http://localhost:3001/workflow/new')

    // Add incompatible nodes
    await page.dragAndDrop('[data-node-type="StartNode"]', '.workflow-canvas')
    await page.dragAndDrop('[data-node-type="EndNode"]', '.workflow-canvas')

    // Try to create invalid connection
    await page.hover('.start-node [data-handleid="start"]')
    await page.mouse.down()
    await page.hover('.end-node [data-handleid="result"]')  // Wrong direction
    await page.mouse.up()

    // Should show error
    await expect(page.locator('.error-toast')).toContainText('Invalid connection')
  })
})
```

---

## 程式碼審查標準

### 審查清單

**功能性** (30%):
```yaml
□ 功能符合需求規格
□ 所有驗收標準已滿足
□ 邊界條件已處理
□ 錯誤處理完整
□ 無已知 Bug
```

**程式碼品質** (25%):
```yaml
□ 遵循編碼規範
□ 無程式碼重複 (DRY)
□ 函數單一職責
□ 變數命名清晰
□ 註解適當且有意義
```

**TypeScript 型別** (15%):
```yaml
□ 型別定義完整
□ 無 any 濫用
□ 介面設計合理
□ 泛型使用適當
```

**效能** (10%):
```yaml
□ 無明顯效能瓶頸
□ 避免不必要的重新渲染
□ 大列表使用虛擬化
□ 圖片/資源優化
```

**測試** (15%):
```yaml
□ 單元測試覆蓋率 ≥85%
□ 關鍵路徑有整合測試
□ E2E 測試覆蓋核心流程
□ 所有測試通過
```

**文檔** (5%):
```yaml
□ README 更新
□ API 文檔完整
□ 使用範例提供
□ 變更歷史記錄
```

---

## 效能優化指南

### 節點渲染優化

**虛擬化大型工作流**:
```typescript
// 當節點數量 > 50 時啟用虛擬化
const useVirtualization = computed(() => nodes.value.length > 50)

// 只渲染可見區域的節點
const visibleNodes = computed(() => {
  if (!useVirtualization.value) return nodes.value

  const viewport = canvasViewport.value
  return nodes.value.filter(node => {
    return isNodeInViewport(node, viewport)
  })
})
```

**懶加載節點組件**:
```typescript
// 使用 defineAsyncComponent 懶加載
const AgentNode = defineAsyncComponent(() =>
  import('./components/nodes/basic/AgentNode.vue')
)
```

**Memoization**:
```typescript
// 快取昂貴的計算
const nodeValidation = computed(() => {
  return memoize((nodeId: string) => {
    const node = nodes.value.find(n => n.id === nodeId)
    return validateNodeConfig(node.type, node.data.config)
  })
})
```

---

### 連線渲染優化

**減少重新計算**:
```typescript
// 使用 shallowRef 避免深層響應
const edges = shallowRef<Edge[]>([])

// 批次更新
function addEdges(newEdges: Edge[]) {
  edges.value = [...edges.value, ...newEdges]
}
```

**Canvas 層級優化**:
```yaml
Layer 1: Background Grid (static, cached)
Layer 2: Edges (update only when changed)
Layer 3: Nodes (virtual rendering)
Layer 4: Overlay (config panels, selection)
```

---

### 配置面板優化

**防抖輸入**:
```typescript
import { useDebounceFn } from '@vueuse/core'

const debouncedValidation = useDebounceFn((config) => {
  validateNodeConfig(nodeType, config)
}, 300)

watch(config, (newConfig) => {
  debouncedValidation(newConfig)
}, { deep: true })
```

**按需載入欄位組件**:
```typescript
const fieldComponent = computed(() => {
  return defineAsyncComponent(() => {
    switch (field.type) {
      case 'code':
        return import('./fields/CodeEditor.vue')
      case 'json':
        return import('./fields/JsonEditor.vue')
      default:
        return import('./fields/TextField.vue')
    }
  })
})
```

---

# 第五部分: 參考文檔

## 完整參考文獻

### Sprint 文檔 (7 個)
1. `claudedocs/2-sprints/sprint-13/SPRINT-13-1-OVERVIEW.md`
2. `claudedocs/2-sprints/sprint-13/SPRINT-13-2-PLAN.md` (本文件)
3. `claudedocs/2-sprints/sprint-13/SPRINT-13-3-CONTEXT.md`
4. `claudedocs/2-sprints/sprint-13/SPRINT-13-4-CHECKLIST.md`
5. `claudedocs/2-sprints/sprint-13/SPRINT-13-5-DEV-LOG.md`
6. `claudedocs/2-sprints/sprint-13/SPRINT-13-6-ISSUES.md`
7. `claudedocs/2-sprints/sprint-13/SPRINT-13-7-RETROSPECTIVE.md`

### 相關 Sprint (6 個)
8. `claudedocs/2-sprints/sprint-11/` - Canvas 核心與拖放
9. `claudedocs/2-sprints/sprint-12/` - 基礎節點與校驗
10. `claudedocs/2-sprints/sprint-14/` - 工作流執行與調試 [計劃中]

### Phase 1D 文檔 (3 個)
11. `claudedocs/1-planning/PHASE-1D-WORKFLOW-EDITOR-PLAN.md`
12. `claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md`
13. `claudedocs/1-planning/MODULE-09-IMPLEMENTATION-GUIDE.md`

### User Stories (5 個)
14. `docs/user-stories/modules/module-09-workflow-editor.md`
15. `docs/user-stories/modules/module-09/US-9.3-workflow-node-management.md`
16. `docs/user-stories/README.md`

### UX 設計文檔 (10 個)
17. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part1.md` - Canvas
18. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part2.md` - 內聯配置
19. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part3.md` - 側邊欄
20. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part4.md` - 28 種節點
21. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part5.md` - 執行調試
22. `docs/ux-design/information-architecture/sitemap.md`
23. `docs/ux-design/design-system/design-tokens.md`
24. `docs/ux-design/design-system/component-library.md`
25. `docs/ux-design/design-system/accessibility-guidelines.md`

### 架構文檔 (7 個)
26. `docs/architecture/Architecture-Design-Document.md`
27. `docs/architecture/C4-architecture-diagrams.md`
28. `docs/architecture/ADR-012-workflow-editor-technology.md`
29. `docs/architecture/database-schema.md`
30. `docs/architecture/performance-scalability-strategy.md`

### 技術實作文檔 (6 個)
31. `docs/technical-implementation/1-poc-validation/poc-06-vueflow-crdt.md`
32. `docs/technical-implementation/frontend-architecture.md`
33. `docs/technical-implementation/module-federation-setup.md`
34. `docs/technical-implementation/testing-strategy.md`

### 專案管理文檔 (4 個)
35. `docs/project-management/Project-Management-Plan.md`
36. `docs/project-management/sprint-planning-template.md`
37. `docs/project-management/risk-management.md`

### 程式碼庫文檔 (6 個)
38. `packages/remote/README.md`
39. `packages/remote/src/components/nodes/README.md`
40. `packages/remote/src/components/config/README.md`
41. `packages/remote/src/validation/README.md`

### 外部技術文檔 (30+ 個)
42-70. [詳見 SPRINT-13-1-OVERVIEW.md 的完整參考文獻索引]

---

## 快速連結

**開發環境**:
```bash
# 啟動開發伺服器
cd packages/remote
npm run dev

# 執行測試
npm run test
npm run test:watch

# 建置
npm run build
```

**常用命令**:
```bash
# 型別檢查
npm run type-check

# Lint
npm run lint

# E2E 測試
npm run test:e2e
```

---

**文檔版本**: v2.1
**最後更新**: 2025-11-14
**下次審查**: Sprint 13 開始時 (2026-06-23)
**維護者**: AI Agent Platform Team

---

## 變更歷史

```yaml
v2.1 (2025-11-14):
  - 初始建立 Sprint 13 Implementation Plan
  - 詳細定義 28 種節點類型
  - 規劃 4 個實施階段
  - 建立完整編碼規範
  - 定義測試策略與效能優化指南

v2.2 (預計 2026-06-23):
  - Sprint 13 開始時更新
  - 調整實施計劃細節
  - 更新風險評估
```
