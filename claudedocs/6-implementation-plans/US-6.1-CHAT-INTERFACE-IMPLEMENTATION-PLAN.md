# US 6.1 - 文字對話 (基礎 Chat 界面) 實施計劃

**User Story**: US 6.1 - 文字對話 (基礎)
**Sprint**: Sprint 2 (延伸)
**計劃天數**: 7 days (2025-12-12 ~ 2025-12-21)
**Story Points**: 3 SP
**優先級**: P0 (MVP 必須)
**狀態**: ⏳ 待開始

**文檔版本**: 1.0.0
**創建日期**: 2025-12-11
**責任人**: Frontend Lead + Tech Lead

---

## 📋 目錄

1. [需求總結](#需求總結)
2. [技術棧確認](#技術棧確認)
3. [組件架構設計](#組件架構設計)
4. [實施階段規劃](#實施階段規劃)
5. [API 集成方案](#api-集成方案)
6. [測試策略](#測試策略)
7. [風險評估](#風險評估)

---

## 📊 需求總結

### 用戶故事

**作為** 業務用戶
**我想要** 通過文字與 Agent 對話
**以便** 完成我的任務

### 驗收標準完整清單

#### ✅ 必須項 (輸入功能) - 5 項

- [ ] **文字輸入框**
  - 支援多行輸入
  - 自動調整高度 (最小 1 行,最大 10 行)
  - Placeholder: "輸入訊息..."

- [ ] **Enter 發送機制**
  - Enter: 發送訊息
  - Shift+Enter: 換行
  - 發送後自動清空輸入框

- [ ] **發送按鈕**
  - 位置: 輸入框右側
  - 圖示: 紙飛機 或 箭頭
  - 狀態: 無內容時禁用,有內容時啟用

- [ ] **輸入字數統計**
  - 顯示: "1250/2000 字"
  - 位置: 輸入框右下角
  - 接近限制時變色提示 (>1800 字變黃,>1950 字變紅)

- [ ] **輸入字數限制**
  - 限制: 2000 字
  - 超過限制時: 禁用發送按鈕 + 提示訊息

#### ✅ 必須項 (Streaming 顯示) - 5 項

- [ ] **逐字顯示 Agent 回應**
  - 打字機效果 (Typewriter effect)
  - 使用 SignalR WebSocket 接收 streaming data
  - 每次接收 1-5 個字符顯示

- [ ] **Streaming 速度自然**
  - 速度: 每秒 20-30 字符 (調整為舒適速度)
  - 不太快 (閱讀困難),不太慢 (等待煩躁)

- [ ] **支援中斷 Streaming**
  - 停止生成按鈕 (Stop generating)
  - 位置: Agent 訊息右上角
  - 功能: 調用 API 取消 streaming

- [ ] **Streaming 過程中可滾動**
  - 對話容器可自由滾動
  - 不鎖定在底部

- [ ] **Streaming 完成後自動捲動**
  - 完成後平滑滾動到底部
  - 使用 `scrollIntoView({ behavior: 'smooth' })`

#### ✅ 必須項 (Markdown 支援) - 6 類

- [ ] **基本格式**
  - 粗體: `**text**` → **text**
  - 斜體: `*text*` → *text*
  - 刪除線: `~~text~~` → ~~text~~

- [ ] **標題** (H1-H6)
  - `# H1` → 大標題
  - `## H2` → 次標題
  - ...

- [ ] **列表**
  - 有序列表: `1. item`
  - 無序列表: `- item` 或 `* item`
  - 嵌套列表支援

- [ ] **連結**
  - `[文字](URL)` → 可點擊連結
  - 新標籤頁打開 (`target="_blank"`)

- [ ] **引用塊**
  - `> quote` → 灰色背景引用樣式

- [ ] **表格**
  - Markdown 表格語法
  - 自動格式化對齊

#### ✅ 必須項 (對話管理) - 6 項

- [ ] **對話歷史自動保存**
  - 每條訊息發送後自動存儲到後端
  - 使用 Conversation API (`POST /api/v1/agents/{id}/conversations`)

- [ ] **會話列表 (Conversation List)**
  - 左側邊欄顯示所有對話
  - 顯示: 對話標題、最後訊息時間、訊息預覽
  - 點擊切換對話

- [ ] **新建對話**
  - "新對話" 按鈕 (左上角)
  - 創建新 Conversation,清空聊天區域

- [ ] **重命名對話**
  - 對話列表中右鍵 → "重命名"
  - 或點擊對話標題編輯

- [ ] **刪除對話 (軟刪除)**
  - 對話列表中右鍵 → "刪除"
  - 調用 API: `DELETE /api/v1/conversations/{id}`
  - 標記為 isDeleted,不實際刪除數據

- [ ] **匯出對話**
  - 匯出格式: Markdown, PDF
  - 按鈕位置: 對話標題右側
  - Markdown: 純文字導出
  - PDF: 使用 jsPDF 或後端生成

#### ✅ 必須項 (上下文管理) - 4 項

- [ ] **多輪對話上下文保持**
  - 每次發送時攜帶 conversationId
  - 後端自動維護對話歷史

- [ ] **上下文 Token 數量顯示**
  - 顯示當前對話已使用 Token 數
  - 位置: 輸入框下方或對話標題旁
  - 格式: "1,250 / 8,000 tokens"

- [ ] **上下文過長時自動截斷**
  - 前端提示: "對話過長,已保留最近 N 輪"
  - 後端邏輯: 保留最近 10-20 輪對話
  - 或使用 sliding window 策略

- [ ] **支援手動清除上下文**
  - "重新開始" 按鈕
  - 清空當前對話上下文
  - 調用 API 創建新 Conversation

### UI 設計要求

#### 對話介面佈局

```
┌─────────────────────────────────────────────────────────────┐
│ [☰] AI Agent Platform        [Agent: GPT-4]     [⚙️] [👤] │  ← Header
├───────────────┬─────────────────────────────────────────────┤
│               │                                             │
│  會話列表      │              對話區域                        │
│  (Sidebar)   │           (Chat Area)                       │
│               │                                             │
│ + 新對話       │  ┌──────────────────────────────────────┐  │
│               │  │ 👤 用戶: 你好                          │  │
│ 📝 對話 1      │  └──────────────────────────────────────┘  │
│    最近訊息... │                                             │
│    2 分鐘前    │  ┌──────────────────────────────────────┐  │
│               │  │ 🤖 Agent: 你好!我是 AI 助手...        │  │
│ 📝 對話 2      │  │ [Streaming...] [■ 停止生成]           │  │
│    標題...     │  └──────────────────────────────────────┘  │
│    1 小時前    │                                             │
│               │  [↓ 更多訊息...]                            │
│               │                                             │
├───────────────┼─────────────────────────────────────────────┤
│               │  ┌──────────────────────────────────────┐  │
│               │  │ 輸入訊息...                           │  │
│               │  │ (多行輸入框,自動調整高度)               │  │
│               │  └──────────────────────────────────────┘  │
│               │  [1250/2000 字]              [📎] [🎤] [🚀] │  ← Input Area
└───────────────┴─────────────────────────────────────────────┘
```

#### 訊息氣泡樣式

**用戶訊息** (右對齊):
```
                            ┌────────────────────────┐
                            │ 你好,請幫我分析數據      │
                            │ 14:35                  │
                            └────────────────────────┘ 👤
                                   (藍色背景)
```

**Agent 訊息** (左對齊):
```
🤖 ┌────────────────────────┐
   │ 當然!我可以幫你分析。    │
   │ 請上傳數據文件。         │
   │ 14:36                  │
   └────────────────────────┘
      (灰色背景)
```

### UX 目標

- **對話延遲**: <500ms (用戶感知)
- **Streaming 流暢度**: 無明顯卡頓
- **輸入響應**: <100ms
- **頁面載入**: <2 秒

---

## 🔧 技術棧確認

### 主要框架 (已確認)

| 技術 | 版本 | 用途 | 來源 |
|-----|------|------|------|
| **React** | **19.0.0** | 主 UI 框架 | TECH-STACK-ANALYSIS.md |
| **TypeScript** | **5.8.3** | 類型系統 | TECH-STACK-ANALYSIS.md |
| **Material-UI (MUI)** | **v6** | UI 組件庫 (Pigment CSS) | TECH-STACK-ANALYSIS.md |
| **Vite** | **7.1.12** | 建構工具 | TECH-STACK-ANALYSIS.md |

### 狀態管理 (待最終選擇)

| 技術 | 優勢 | 劣勢 | 推薦度 |
|-----|------|------|--------|
| **Zustand** | 輕量 (~1kb), 簡單 API, TypeScript 友好 | 生態較小 | ✅ **推薦** |
| Redux Toolkit | 成熟生態, DevTools 強大 | 學習曲線, 代碼量多 | ⚠️ 備選 |
| React Context | 內建,無依賴 | 性能問題,複雜狀態難管理 | ❌ 不推薦 |

**決策**: 使用 **Zustand** (輕量級,適合 Chat 應用簡單狀態管理)

### 即時通訊

| 技術 | 版本 | 用途 | 備註 |
|-----|------|------|------|
| **@microsoft/signalr** | **9.0.10** | SignalR Client | 已確認,US 1.4 已使用 |

### Markdown 與代碼

| 技術 | 版本 | 用途 | 備註 |
|-----|------|------|------|
| **react-markdown** | Latest | Markdown 渲染 | 支援插件系統 |
| **remark-gfm** | Latest | GitHub Flavored Markdown | 表格、刪除線支援 |
| **react-syntax-highlighter** | Latest | 代碼高亮 | 基於 Prism.js 或 Highlight.js |

### 其他依賴

| 技術 | 版本 | 用途 | 備註 |
|-----|------|------|------|
| **React Hook Form** | Latest | 表單管理 | 輸入框驗證 |
| **date-fns** | Latest | 日期格式化 | 時間戳顯示 |
| **jsPDF** (可選) | Latest | PDF 匯出 | 或後端生成 |

---

## 🏗️ 組件架構設計

### 組件樹結構

```
ChatPage (頁面)
├── ChatLayout (佈局)
│   ├── ConversationSidebar (左側邊欄)
│   │   ├── NewConversationButton (新對話按鈕)
│   │   ├── ConversationList (對話列表)
│   │   │   └── ConversationItem × N (對話項目)
│   │   │       ├── ConversationTitle (標題)
│   │   │       ├── ConversationPreview (預覽)
│   │   │       └── ConversationTimestamp (時間)
│   │   └── ConversationActions (操作按鈕)
│   │
│   └── ChatArea (對話區域)
│       ├── ChatHeader (對話標題欄)
│       │   ├── ConversationTitle (對話標題)
│       │   ├── TokenCounter (Token 計數器)
│       │   └── ChatActions (匯出、設置等)
│       │
│       ├── MessageList (訊息列表容器)
│       │   └── Message × N (訊息)
│       │       ├── UserMessage (用戶訊息)
│       │       │   ├── MessageBubble (氣泡)
│       │       │   └── MessageTimestamp (時間)
│       │       └── AgentMessage (Agent 訊息)
│       │           ├── MessageBubble (氣泡)
│       │           ├── MarkdownContent (Markdown 渲染)
│       │           ├── StreamingIndicator (Streaming 狀態)
│       │           ├── StopButton (停止生成按鈕)
│       │           └── MessageTimestamp (時間)
│       │
│       └── ChatInput (輸入區域)
│           ├── TextArea (多行輸入框)
│           ├── CharacterCounter (字數統計)
│           ├── AttachButton (附件按鈕 - Phase 2)
│           └── SendButton (發送按鈕)
```

### 核心組件設計

#### 1. ChatPage (頁面組件)

**職責**:
- 頁面級狀態管理 (當前對話、訊息列表)
- API 調用邏輯
- SignalR 連接管理

**State**:
```typescript
interface ChatPageState {
  currentConversation: Conversation | null;
  conversations: Conversation[];
  messages: Message[];
  isStreaming: boolean;
  streamingMessageId: string | null;
}
```

**Hooks**:
- `useConversations()`: 獲取對話列表
- `useMessages(conversationId)`: 獲取訊息列表
- `useSignalR(agentId, conversationId)`: SignalR 連接

#### 2. MessageList (訊息列表)

**職責**:
- 顯示所有訊息
- 自動滾動到底部
- 虛擬滾動 (Phase 2 優化)

**Props**:
```typescript
interface MessageListProps {
  messages: Message[];
  isStreaming: boolean;
  onStopStreaming: () => void;
}
```

**優化**:
- 使用 `useEffect` 監聽 messages 變化自動滾動
- IntersectionObserver 檢測是否在底部

#### 3. Message (訊息組件)

**職責**:
- 根據角色渲染不同樣式
- Markdown 渲染
- Streaming 動畫

**Props**:
```typescript
interface MessageProps {
  message: Message;
  isStreaming: boolean;
  onStopStreaming?: () => void;
}
```

**Markdown 渲染**:
```typescript
import ReactMarkdown from 'react-markdown';
import remarkGfm from 'remark-gfm';
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';

<ReactMarkdown
  remarkPlugins={[remarkGfm]}
  components={{
    code({ node, inline, className, children, ...props }) {
      const match = /language-(\w+)/.exec(className || '');
      return !inline && match ? (
        <SyntaxHighlighter
          language={match[1]}
          PreTag="div"
          {...props}
        >
          {String(children).replace(/\n$/, '')}
        </SyntaxHighlighter>
      ) : (
        <code className={className} {...props}>
          {children}
        </code>
      );
    }
  }}
>
  {message.content}
</ReactMarkdown>
```

#### 4. ChatInput (輸入組件)

**職責**:
- 多行輸入管理
- 字數統計與限制
- Enter/Shift+Enter 處理
- 發送邏輯

**Props**:
```typescript
interface ChatInputProps {
  onSend: (content: string) => Promise<void>;
  disabled: boolean;
  maxLength: number;
}
```

**實現重點**:
```typescript
const handleKeyDown = (e: React.KeyboardEvent) => {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault(); // 防止換行
    handleSend();
  }
  // Shift+Enter 允許換行 (不需特殊處理)
};

const handleSend = async () => {
  if (content.trim() && content.length <= maxLength) {
    await onSend(content);
    setContent(''); // 清空輸入框
  }
};
```

#### 5. ConversationSidebar (側邊欄)

**職責**:
- 顯示對話列表
- 創建、重命名、刪除對話
- 切換對話

**Props**:
```typescript
interface ConversationSidebarProps {
  conversations: Conversation[];
  currentConversationId: string | null;
  onSelectConversation: (id: string) => void;
  onCreateConversation: () => void;
  onRenameConversation: (id: string, title: string) => void;
  onDeleteConversation: (id: string) => void;
}
```

---

## 📅 實施階段規劃

### Day 18 (2025-12-12): 設計與準備

**時間**: 8 小時

**任務**:
1. ✅ **確認需求** (1 小時)
   - 閱讀 US 6.1 完整需求
   - 確認驗收標準 (26 項)

2. ✅ **技術棧確認** (1 小時)
   - 確認主要框架 (React 19, MUI v6)
   - 選擇狀態管理 (Zustand)
   - 選擇 Markdown 庫 (react-markdown)

3. ✅ **組件架構設計** (2 小時)
   - 繪製組件樹
   - 定義組件職責
   - 設計 Props 和 State

4. 📝 **創建實施計劃文檔** (1 小時) - 當前任務
   - 本文檔

5. 🎨 **UI 原型設計** (2 小時)
   - 使用 Figma 或手繪線框圖
   - 定義樣式變量 (顏色、間距、圓角)
   - 確認響應式斷點

6. 📦 **專案初始化** (1 小時)
   - 創建 React 專案 (Vite)
   - 安裝依賴 (MUI, Zustand, react-markdown, @microsoft/signalr)
   - 配置 TypeScript, ESLint, Prettier

### Day 19 (2025-12-13): 基礎界面實現

**時間**: 8 小時

**Phase 1: 靜態 UI** (4 小時)

1. **Layout 組件** (1 小時)
   - `ChatLayout`: 主佈局 (Sidebar + ChatArea)
   - 使用 MUI Grid/Box 實現響應式佈局

2. **Sidebar 組件** (1.5 小時)
   - `ConversationSidebar`: 側邊欄容器
   - `NewConversationButton`: 新對話按鈕
   - `ConversationList`: 對話列表
   - `ConversationItem`: 對話項目 (標題、預覽、時間)

3. **ChatArea 組件** (1.5 小時)
   - `ChatHeader`: 標題欄 (對話標題、Token 計數、操作按鈕)
   - `MessageList`: 訊息列表容器 (空狀態提示)
   - `ChatInput`: 輸入區域 (TextArea + 發送按鈕)

**Phase 2: Message 組件** (3 小時)

4. **Message 組件實現** (2 小時)
   - `Message`: 訊息基礎組件
   - `UserMessage`: 用戶訊息樣式 (右對齊、藍色氣泡)
   - `AgentMessage`: Agent 訊息樣式 (左對齊、灰色氣泡)
   - Markdown 渲染整合 (react-markdown + remark-gfm)
   - 代碼高亮整合 (react-syntax-highlighter)

5. **輸入功能完善** (1 小時)
   - 多行輸入自動調整高度
   - Enter/Shift+Enter 處理
   - 字數統計與限制 (2000 字)
   - 發送按鈕狀態管理

**測試**: 靜態 UI 完整性檢查

### Day 20 (2025-12-14): API 集成與 Streaming

**時間**: 8 小時

**Phase 1: API 集成** (4 小時)

1. **API Service 層** (1.5 小時)
   - `conversationApi.ts`: Conversation CRUD
   - `agentApi.ts`: Agent 執行 API
   - TypeScript 類型定義

2. **Zustand Store** (1.5 小時)
   - `conversationStore.ts`: 對話狀態管理
   - `messageStore.ts`: 訊息狀態管理
   - `chatStore.ts`: Chat UI 狀態 (isStreaming, etc.)

3. **Hook 封裝** (1 小時)
   - `useConversations()`: 獲取對話列表
   - `useMessages(conversationId)`: 獲取訊息列表
   - `useSendMessage()`: 發送訊息邏輯

**Phase 2: SignalR Streaming** (4 小時)

4. **SignalR Client 設置** (2 小時)
   - 創建 `signalRService.ts`
   - 連接 `/agentHub` (US 1.4 已實現)
   - 監聽 `AgentResponseChunk` 事件

5. **Streaming 實現** (2 小時)
   - 接收 streaming chunks
   - 逐字更新 Agent 訊息
   - Streaming 動畫效果 (打字機)
   - 停止生成按鈕功能

**測試**:
- 發送訊息成功
- 接收 Agent 回應 (Streaming)
- 停止生成功能

### Day 21 (2025-12-15): 對話管理與完善

**時間**: 8 小時

**Phase 1: 對話管理** (3 小時)

1. **創建對話** (1 小時)
   - 新對話按鈕功能
   - 調用 `POST /api/v1/conversations`
   - 切換到新對話

2. **重命名對話** (1 小時)
   - 雙擊標題編輯 或 右鍵菜單
   - 調用 `PUT /api/v1/conversations/{id}`
   - 實時更新 UI

3. **刪除對話** (1 小時)
   - 刪除確認對話框
   - 調用 `DELETE /api/v1/conversations/{id}`
   - 軟刪除 (isDeleted = true)

**Phase 2: 進階功能** (3 小時)

4. **上下文管理** (1 小時)
   - Token 計數器顯示
   - 重新開始按鈕 (清除上下文)
   - 上下文過長提示

5. **匯出對話** (1 小時)
   - Markdown 匯出 (純文字)
   - PDF 匯出 (使用 jsPDF 或後端)

6. **UI 完善** (1 小時)
   - 自動滾動到底部
   - Loading 狀態
   - 錯誤處理
   - 空狀態提示

**Phase 3: 測試與修復** (2 小時)

7. **功能測試** (1 小時)
   - 所有 26 項驗收標準逐一測試
   - Bug 記錄

8. **Bug 修復** (1 小時)
   - 修復發現的問題

### Day 22-23 (2025-12-16 ~ 2025-12-17): 完善與優化

**Phase 1: UX 優化** (8 小時)

1. **響應式設計** (2 小時)
   - 移動端適配 (Sidebar 摺疊)
   - 平板適配
   - 不同螢幕尺寸測試

2. **性能優化** (2 小時)
   - Message 列表虛擬化 (react-window)
   - 圖片懶加載
   - 減少不必要的 re-render

3. **無障礙支援** (2 小時)
   - ARIA labels
   - 鍵盤導航
   - 螢幕閱讀器測試

4. **動畫與過渡** (2 小時)
   - 訊息進入動畫
   - Streaming 動畫優化
   - 頁面切換過渡

**Phase 2: 錯誤處理** (4 小時)

5. **網路錯誤處理** (2 小時)
   - API 調用失敗重試
   - SignalR 斷線重連
   - 錯誤提示 Toast

6. **邊界情況處理** (2 小時)
   - 空對話
   - 空訊息
   - 超長訊息
   - Token 超限

### Day 24-25 (2025-12-18 ~ 2025-12-19): 測試

**Phase 1: 單元測試** (4 小時)

1. **組件測試** (2 小時)
   - `ChatInput` 測試 (輸入、發送、驗證)
   - `Message` 測試 (Markdown 渲染)
   - `ConversationSidebar` 測試 (CRUD 操作)

2. **Hook 測試** (1 小時)
   - `useConversations()` 測試
   - `useMessages()` 測試
   - `useSendMessage()` 測試

3. **Store 測試** (1 小時)
   - Zustand store 狀態變更測試

**Phase 2: 集成測試** (8 小時)

4. **E2E 測試** (4 小時)
   - Playwright 測試腳本
   - 場景 1: 創建對話 → 發送訊息 → 接收回應
   - 場景 2: 切換對話 → 載入歷史訊息
   - 場景 3: 重命名對話 → 刪除對話
   - 場景 4: 匯出對話 (Markdown/PDF)

5. **用戶測試** (2 小時)
   - 邀請團隊成員測試
   - 收集反饋

6. **Bug 修復** (2 小時)
   - 修復測試中發現的問題

### Day 26-27 (2025-12-20 ~ 2025-12-21): 文檔與收尾

**Phase 1: 文檔** (4 小時)

1. **用戶文檔** (2 小時)
   - Chat 界面使用指南
   - 功能說明截圖

2. **技術文檔** (2 小時)
   - 組件 API 文檔
   - SignalR 集成文檔
   - 部署說明

**Phase 2: Sprint 收尾** (8 小時)

3. **代碼審查** (2 小時)
   - 自我審查
   - 同行審查

4. **性能測試** (2 小時)
   - Lighthouse 測試
   - 載入時間測試
   - Streaming 流暢度測試

5. **Sprint 2 文檔更新** (2 小時)
   - 更新 SPRINT-2-OVERVIEW.md
   - 更新 PROJECT-STATUS-REPORT.md
   - 記錄 US 6.1 完成狀態

6. **Sprint Review 準備** (2 小時)
   - Demo 環境準備
   - 展示腳本準備
   - 截圖和錄屏

---

## 🔌 API 集成方案

### 1. Conversation API

#### 獲取對話列表
```typescript
// GET /api/v1/agents/{agentId}/conversations
interface GetConversationsResponse {
  conversations: Conversation[];
  total: number;
}

interface Conversation {
  id: string;
  agentId: string;
  title: string;
  createdAt: string;
  updatedAt: string;
  lastMessageAt: string | null;
  lastMessagePreview: string | null;
  isDeleted: boolean;
}
```

#### 創建對話
```typescript
// POST /api/v1/agents/{agentId}/conversations
interface CreateConversationRequest {
  title: string;
}

interface CreateConversationResponse {
  conversation: Conversation;
}
```

#### 更新對話 (重命名)
```typescript
// PUT /api/v1/conversations/{id}
interface UpdateConversationRequest {
  title: string;
}
```

#### 刪除對話 (軟刪除)
```typescript
// DELETE /api/v1/conversations/{id}
// Response: 204 No Content
```

### 2. Message API

#### 獲取訊息列表
```typescript
// GET /api/v1/conversations/{conversationId}/messages
interface GetMessagesResponse {
  messages: Message[];
  total: number;
}

interface Message {
  id: string;
  conversationId: string;
  role: 'user' | 'agent';
  content: string;
  createdAt: string;
  tokenCount: number;
}
```

### 3. Agent Execution API

#### 發送訊息 (觸發 Agent 執行)
```typescript
// POST /api/v1/agents/{agentId}/invoke
interface InvokeAgentRequest {
  conversationId: string;
  userInput: string;
}

interface InvokeAgentResponse {
  executionId: string;
  conversationId: string;
  // 後續通過 SignalR 接收 streaming response
}
```

### 4. SignalR Streaming

#### 連接 AgentHub
```typescript
import * as signalR from '@microsoft/signalr';

const connection = new signalR.HubConnectionBuilder()
  .withUrl('/agentHub')
  .withAutomaticReconnect()
  .build();

await connection.start();
```

#### 監聽 Streaming 事件
```typescript
// 接收 Agent 回應 Chunk
connection.on('AgentResponseChunk', (data: StreamingChunk) => {
  // data.executionId: string
  // data.chunk: string
  // data.isComplete: boolean

  if (!data.isComplete) {
    // 逐字追加到訊息內容
    appendToMessage(data.executionId, data.chunk);
  } else {
    // Streaming 完成
    markMessageComplete(data.executionId);
  }
});

// 接收執行狀態更新
connection.on('AgentExecutionStatusChanged', (data: StatusChange) => {
  // data.executionId: string
  // data.status: 'pending' | 'running' | 'completed' | 'failed'
});
```

#### 停止 Streaming
```typescript
// 調用後端 API 取消執行
// POST /api/v1/agents/{agentId}/executions/{executionId}/cancel
await connection.invoke('CancelExecution', executionId);
```

### API Service 實現範例

```typescript
// services/conversationApi.ts
import axios from 'axios';

const API_BASE_URL = '/api/v1';

export const conversationApi = {
  // 獲取對話列表
  async getConversations(agentId: string) {
    const response = await axios.get<GetConversationsResponse>(
      `${API_BASE_URL}/agents/${agentId}/conversations`
    );
    return response.data;
  },

  // 創建對話
  async createConversation(agentId: string, title: string) {
    const response = await axios.post<CreateConversationResponse>(
      `${API_BASE_URL}/agents/${agentId}/conversations`,
      { title }
    );
    return response.data;
  },

  // 重命名對話
  async updateConversation(conversationId: string, title: string) {
    await axios.put(
      `${API_BASE_URL}/conversations/${conversationId}`,
      { title }
    );
  },

  // 刪除對話
  async deleteConversation(conversationId: string) {
    await axios.delete(`${API_BASE_URL}/conversations/${conversationId}`);
  },

  // 獲取訊息列表
  async getMessages(conversationId: string) {
    const response = await axios.get<GetMessagesResponse>(
      `${API_BASE_URL}/conversations/${conversationId}/messages`
    );
    return response.data;
  },

  // 發送訊息
  async sendMessage(agentId: string, conversationId: string, userInput: string) {
    const response = await axios.post<InvokeAgentResponse>(
      `${API_BASE_URL}/agents/${agentId}/invoke`,
      { conversationId, userInput }
    );
    return response.data;
  },
};
```

### SignalR Service 實現範例

```typescript
// services/signalRService.ts
import * as signalR from '@microsoft/signalr';

class SignalRService {
  private connection: signalR.HubConnection | null = null;
  private listeners: Map<string, Function[]> = new Map();

  async connect() {
    if (this.connection) {
      return;
    }

    this.connection = new signalR.HubConnectionBuilder()
      .withUrl('/agentHub')
      .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: () => 2000, // 2 秒後重連
      })
      .configureLogging(signalR.LogLevel.Information)
      .build();

    // 設置事件監聽
    this.connection.on('AgentResponseChunk', (data: StreamingChunk) => {
      this.emit('chunk', data);
    });

    this.connection.on('AgentExecutionStatusChanged', (data: StatusChange) => {
      this.emit('status', data);
    });

    // 斷線重連處理
    this.connection.onreconnecting(() => {
      console.log('SignalR reconnecting...');
      this.emit('reconnecting', null);
    });

    this.connection.onreconnected(() => {
      console.log('SignalR reconnected');
      this.emit('reconnected', null);
    });

    await this.connection.start();
    console.log('SignalR connected');
  }

  async disconnect() {
    if (this.connection) {
      await this.connection.stop();
      this.connection = null;
    }
  }

  on(event: string, callback: Function) {
    if (!this.listeners.has(event)) {
      this.listeners.set(event, []);
    }
    this.listeners.get(event)!.push(callback);
  }

  off(event: string, callback: Function) {
    const callbacks = this.listeners.get(event);
    if (callbacks) {
      const index = callbacks.indexOf(callback);
      if (index > -1) {
        callbacks.splice(index, 1);
      }
    }
  }

  private emit(event: string, data: any) {
    const callbacks = this.listeners.get(event);
    if (callbacks) {
      callbacks.forEach((cb) => cb(data));
    }
  }

  async cancelExecution(executionId: string) {
    if (this.connection) {
      await this.connection.invoke('CancelExecution', executionId);
    }
  }
}

export const signalRService = new SignalRService();
```

---

## 🧪 測試策略

### 單元測試 (Jest + React Testing Library)

#### 1. ChatInput 組件測試

```typescript
// components/__tests__/ChatInput.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { ChatInput } from '../ChatInput';

describe('ChatInput', () => {
  it('應該正確渲染輸入框和發送按鈕', () => {
    render(<ChatInput onSend={jest.fn()} disabled={false} maxLength={2000} />);

    expect(screen.getByPlaceholderText('輸入訊息...')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: '發送' })).toBeInTheDocument();
  });

  it('應該在 Enter 時發送訊息', async () => {
    const onSend = jest.fn();
    render(<ChatInput onSend={onSend} disabled={false} maxLength={2000} />);

    const input = screen.getByPlaceholderText('輸入訊息...');
    await userEvent.type(input, 'Hello World{Enter}');

    expect(onSend).toHaveBeenCalledWith('Hello World');
  });

  it('應該在 Shift+Enter 時換行', async () => {
    const onSend = jest.fn();
    render(<ChatInput onSend={onSend} disabled={false} maxLength={2000} />);

    const input = screen.getByPlaceholderText('輸入訊息...') as HTMLTextAreaElement;
    await userEvent.type(input, 'Line 1{Shift>}{Enter}{/Shift}Line 2');

    expect(input.value).toBe('Line 1\nLine 2');
    expect(onSend).not.toHaveBeenCalled();
  });

  it('應該顯示字數統計', async () => {
    render(<ChatInput onSend={jest.fn()} disabled={false} maxLength={2000} />);

    const input = screen.getByPlaceholderText('輸入訊息...');
    await userEvent.type(input, 'Hello');

    expect(screen.getByText('5/2000 字')).toBeInTheDocument();
  });

  it('應該在超過字數限制時禁用發送按鈕', async () => {
    render(<ChatInput onSend={jest.fn()} disabled={false} maxLength={10} />);

    const input = screen.getByPlaceholderText('輸入訊息...');
    await userEvent.type(input, 'This is a very long message');

    const sendButton = screen.getByRole('button', { name: '發送' });
    expect(sendButton).toBeDisabled();
  });
});
```

#### 2. Message 組件測試

```typescript
// components/__tests__/Message.test.tsx
import { render, screen } from '@testing-library/react';
import { Message } from '../Message';

describe('Message', () => {
  it('應該渲染用戶訊息', () => {
    const message = {
      id: '1',
      role: 'user' as const,
      content: 'Hello',
      createdAt: new Date().toISOString(),
    };

    render(<Message message={message} />);
    expect(screen.getByText('Hello')).toBeInTheDocument();
  });

  it('應該渲染 Agent 訊息並支援 Markdown', () => {
    const message = {
      id: '2',
      role: 'agent' as const,
      content: '**Hello** *World*',
      createdAt: new Date().toISOString(),
    };

    render(<Message message={message} />);
    expect(screen.getByText('Hello')).toHaveStyle({ fontWeight: 'bold' });
  });

  it('應該在 Streaming 時顯示停止按鈕', () => {
    const message = {
      id: '3',
      role: 'agent' as const,
      content: 'Streaming...',
      createdAt: new Date().toISOString(),
    };

    const onStopStreaming = jest.fn();
    render(<Message message={message} isStreaming onStopStreaming={onStopStreaming} />);

    expect(screen.getByRole('button', { name: '停止生成' })).toBeInTheDocument();
  });
});
```

### 集成測試 (Playwright)

#### E2E 測試場景

```typescript
// e2e/chat.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Chat Interface', () => {
  test('完整對話流程', async ({ page }) => {
    // 1. 訪問 Chat 頁面
    await page.goto('/chat');

    // 2. 創建新對話
    await page.click('text=新對話');
    expect(await page.locator('.chat-area').textContent()).toContain('開始對話');

    // 3. 發送訊息
    await page.fill('textarea[placeholder="輸入訊息..."]', 'Hello, Agent!');
    await page.press('textarea[placeholder="輸入訊息..."]', 'Enter');

    // 4. 驗證用戶訊息出現
    await expect(page.locator('.user-message')).toContainText('Hello, Agent!');

    // 5. 等待 Agent 回應 (Streaming)
    await expect(page.locator('.agent-message')).toBeVisible({ timeout: 10000 });

    // 6. 驗證 Streaming 完成
    await expect(page.locator('.streaming-indicator')).not.toBeVisible({ timeout: 30000 });

    // 7. 驗證訊息已保存
    await page.reload();
    await expect(page.locator('.message')).toHaveCount(2);
  });

  test('對話管理功能', async ({ page }) => {
    await page.goto('/chat');

    // 1. 創建對話
    await page.click('text=新對話');

    // 2. 重命名對話
    await page.dblclick('.conversation-title');
    await page.fill('input[value="新對話"]', '測試對話');
    await page.press('input[value="測試對話"]', 'Enter');
    await expect(page.locator('.conversation-title')).toContainText('測試對話');

    // 3. 刪除對話
    await page.click('.conversation-item', { button: 'right' });
    await page.click('text=刪除');
    await page.click('button:has-text("確認")');
    await expect(page.locator('.conversation-item:has-text("測試對話")')).not.toBeVisible();
  });

  test('Markdown 渲染', async ({ page }) => {
    await page.goto('/chat');
    await page.fill('textarea[placeholder="輸入訊息..."]', '請用 Markdown 回應');
    await page.press('textarea[placeholder="輸入訊息..."]', 'Enter');

    // 假設 Agent 回應包含 Markdown
    await expect(page.locator('.agent-message strong')).toBeVisible();
    await expect(page.locator('.agent-message code')).toBeVisible();
  });
});
```

### 性能測試

#### Lighthouse 測試

```bash
# 使用 Lighthouse CLI
lighthouse http://localhost:3000/chat --output html --output-path ./lighthouse-report.html

# 目標指標:
# Performance: > 90
# Accessibility: > 95
# Best Practices: > 90
```

#### 載入時間測試

```typescript
// 測試首次載入時間
test('頁面載入時間 < 2 秒', async ({ page }) => {
  const start = Date.now();
  await page.goto('/chat');
  await page.waitForSelector('.chat-area');
  const end = Date.now();

  expect(end - start).toBeLessThan(2000);
});
```

#### Streaming 流暢度測試

```typescript
// 測試 Streaming 是否流暢
test('Streaming 無明顯卡頓', async ({ page }) => {
  await page.goto('/chat');
  await page.fill('textarea', 'Long response please');
  await page.press('textarea', 'Enter');

  // 監聽 DOM 變化頻率
  const updates: number[] = [];
  let lastUpdate = Date.now();

  page.on('console', (msg) => {
    if (msg.text().includes('chunk')) {
      const now = Date.now();
      updates.push(now - lastUpdate);
      lastUpdate = now;
    }
  });

  await page.waitForTimeout(10000);

  // 計算平均更新間隔
  const avgInterval = updates.reduce((a, b) => a + b, 0) / updates.length;
  expect(avgInterval).toBeLessThan(100); // 每 100ms 內應該有更新
});
```

---

## ⚠️ 風險評估

### 技術風險

| 風險 | 影響 | 可能性 | 緩解措施 |
|-----|------|--------|---------|
| **SignalR Streaming 不穩定** | 🔴 高 | 🟡 中 | 實現自動重連機制,降級為輪詢 API |
| **Markdown 渲染性能問題** (長訊息) | 🟡 中 | 🟡 中 | 使用虛擬滾動,限制訊息長度 |
| **React 19 新特性兼容性** | 🟡 中 | 🟢 低 | 充分測試,必要時降級到 React 18 |
| **MUI v6 Pigment CSS 學習曲線** | 🟢 低 | 🟡 中 | 提前學習文檔,使用 styled API 作為備選 |

### 時程風險

| 風險 | 影響 | 可能性 | 緩解措施 |
|-----|------|--------|---------|
| **Day 19-20 實現超時** | 🟡 中 | 🟡 中 | 簡化 UI,優先完成核心功能 |
| **SignalR 集成複雜度超預期** | 🔴 高 | 🟡 中 | 提前與後端對齊 API,準備降級方案 |
| **測試時間不足** | 🟡 中 | 🟢 低 | 並行開發和測試,自動化測試 |

### 需求風險

| 風險 | 影響 | 可能性 | 緩解措施 |
|-----|------|--------|---------|
| **驗收標準理解偏差** | 🔴 高 | 🟢 低 | Day 18 與 PO 確認所有 26 項標準 |
| **UX 設計細節缺失** | 🟡 中 | 🟡 中 | 參考 ChatGPT/Claude UI,補充設計 |
| **後端 API 變更** | 🟡 中 | 🟢 低 | 與 Tech Lead 對齊 API 契約 |

---

## 📚 參考資源

### 文檔

- [US 6.1 詳細需求](../../docs/user-stories/modules/module-06-chat-interface.md)
- [技術棧分析](../../docs/technical-implementation/TECH-STACK-ANALYSIS.md)
- [US 1.4 SignalR 實現](../../docs/user-stories/modules/module-01-agent-management.md)

### 外部資源

- [React 19 文檔](https://react.dev/)
- [Material-UI v6 文檔](https://mui.com/)
- [Zustand 文檔](https://github.com/pmndrs/zustand)
- [@microsoft/signalr 文檔](https://learn.microsoft.com/en-us/aspnet/core/signalr/javascript-client)
- [react-markdown 文檔](https://github.com/remarkjs/react-markdown)
- [ChatGPT UI 參考](https://chat.openai.com/)
- [Claude UI 參考](https://claude.ai/)

---

## ✅ 驗收檢查清單

### 輸入功能 (5/5)

- [ ] 文字輸入框支援多行,自動調整高度
- [ ] Enter 發送,Shift+Enter 換行
- [ ] 發送按鈕狀態正確
- [ ] 輸入字數統計顯示
- [ ] 輸入字數限制 2000 字

### Streaming 顯示 (5/5)

- [ ] 逐字顯示 Agent 回應
- [ ] Streaming 速度自然
- [ ] 支援中斷 Streaming
- [ ] Streaming 過程中可滾動
- [ ] Streaming 完成後自動捲動

### Markdown 支援 (6/6)

- [ ] 粗體、斜體、刪除線渲染正確
- [ ] 標題 (H1-H6) 渲染正確
- [ ] 列表 (有序、無序) 渲染正確
- [ ] 連結可點擊
- [ ] 引用塊渲染正確
- [ ] 表格渲染正確

### 對話管理 (6/6)

- [ ] 對話歷史自動保存
- [ ] 會話列表顯示正確
- [ ] 新建對話功能正常
- [ ] 重命名對話功能正常
- [ ] 刪除對話 (軟刪除) 功能正常
- [ ] 匯出對話 (Markdown/PDF) 功能正常

### 上下文管理 (4/4)

- [ ] 多輪對話上下文保持
- [ ] 上下文 Token 數量顯示
- [ ] 上下文過長時自動截斷
- [ ] 支援手動清除上下文

### 非功能性需求 (5/5)

- [ ] UI 設計符合要求 (類似 ChatGPT/Claude)
- [ ] 對話延遲 <500ms
- [ ] 響應式設計 (桌面/平板/移動端)
- [ ] 無障礙支援 (ARIA labels, 鍵盤導航)
- [ ] 性能測試通過 (Lighthouse > 90)

**總計**: 31/31 項驗收標準

---

**下一步行動**: 開始 Day 18 實施,創建 React 專案並進行 UI 原型設計。

**預估完成日期**: 2025-12-21 (Sprint 2 結束)

**風險等級**: 🟡 中 (主要風險: SignalR Streaming 集成複雜度)

**成功標準**: 所有 31 項驗收標準通過 ✅
