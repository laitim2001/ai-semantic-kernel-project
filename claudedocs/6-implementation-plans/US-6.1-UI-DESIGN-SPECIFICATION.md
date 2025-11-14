# US 6.1 - Chat UI 設計規範

**文檔編號**: US-6.1-UI-SPEC
**版本**: 1.0.0
**創建日期**: 2025-12-12
**設計風格參考**: ChatGPT, Claude AI
**技術棧**: React 19 + Material-UI v6 + TypeScript

---

## 目錄

1. [設計原則](#設計原則)
2. [Design Tokens](#design-tokens)
3. [Layout 設計](#layout-設計)
4. [Component 設計](#component-設計)
5. [互動設計](#互動設計)
6. [響應式設計](#響應式設計)
7. [Accessibility 設計](#accessibility-設計)

---

## 設計原則

### 核心設計理念
1. **簡潔優先**: 減少視覺干擾,專注對話內容
2. **清晰層次**: 明確的訊息發送者區分 (用戶 vs Agent)
3. **流暢體驗**: Streaming 打字機效果自然流暢
4. **快速響應**: 輸入、發送、滾動均需即時反饋
5. **親和易用**: 符合 WCAG 2.1 AA 無障礙標準

### 參考設計 (Best Practices)
- **ChatGPT**: 簡潔左右分區,訊息氣泡清晰
- **Claude AI**: 優雅的 Markdown 渲染,代碼高亮
- **Material Design 3**: 現代化色彩系統,圓潤設計語言

---

## Design Tokens

### Color Palette

#### Primary Colors (主色調)
```typescript
const colors = {
  primary: {
    main: '#1976d2',      // MUI Blue 700 - 主要操作按鈕
    light: '#42a5f5',     // MUI Blue 400 - Hover 狀態
    dark: '#1565c0',      // MUI Blue 800 - Active 狀態
    contrastText: '#fff'  // 按鈕文字顏色
  },

  secondary: {
    main: '#9c27b0',      // MUI Purple 500 - 次要操作
    light: '#ba68c8',     // MUI Purple 300 - Hover
    dark: '#7b1fa2',      // MUI Purple 700 - Active
    contrastText: '#fff'
  }
}
```

#### Message Colors (訊息顏色)
```typescript
const messageColors = {
  user: {
    background: '#1976d2',    // 用戶訊息背景 (藍色)
    text: '#ffffff',          // 用戶訊息文字 (白色)
    timestamp: 'rgba(255, 255, 255, 0.7)'  // 時間戳 (半透明白)
  },

  agent: {
    background: '#f5f5f5',    // Agent 訊息背景 (淺灰)
    text: '#212121',          // Agent 訊息文字 (深灰)
    timestamp: 'rgba(0, 0, 0, 0.5)'  // 時間戳 (半透明黑)
  }
}
```

#### Neutral Colors (中性色)
```typescript
const neutralColors = {
  background: {
    default: '#ffffff',       // 頁面背景 (白色)
    paper: '#f9f9f9',         // Sidebar 背景 (極淺灰)
    chat: '#ffffff',          // 對話區域背景 (白色)
    input: '#f5f5f5'          // 輸入框背景 (淺灰)
  },

  text: {
    primary: '#212121',       // 主要文字 (深灰)
    secondary: '#757575',     // 次要文字 (中灰)
    disabled: '#bdbdbd',      // 禁用文字 (淺灰)
    hint: '#9e9e9e'           // 提示文字 (淺灰)
  },

  divider: '#e0e0e0',         // 分隔線 (淺灰)
  border: '#e0e0e0'           // 邊框 (淺灰)
}
```

#### Status Colors (狀態色)
```typescript
const statusColors = {
  success: '#4caf50',         // 成功 (綠色)
  warning: '#ff9800',         // 警告 (橙色)
  error: '#f44336',           // 錯誤 (紅色)
  info: '#2196f3',            // 資訊 (藍色)
  streaming: '#1976d2'        // Streaming 狀態 (藍色)
}
```

### Spacing Scale (間距系統)

```typescript
const spacing = {
  xs: 4,    // 0.25rem - 極小間距 (icon padding)
  sm: 8,    // 0.5rem  - 小間距 (button padding)
  md: 16,   // 1rem    - 中間距 (component margin)
  lg: 24,   // 1.5rem  - 大間距 (section padding)
  xl: 32,   // 2rem    - 超大間距 (page padding)
  xxl: 48   // 3rem    - 極大間距 (header height)
}
```

**使用範例**:
- Message bubble 內邊距: `${spacing.md}px` (16px)
- Conversation item 間距: `${spacing.sm}px` (8px)
- Sidebar padding: `${spacing.lg}px` (24px)
- Chat area padding: `${spacing.xl}px` (32px)

### Typography (字體系統)

```typescript
const typography = {
  fontFamily: {
    base: '"Roboto", "Noto Sans TC", "Microsoft JhengHei", sans-serif',
    code: '"Fira Code", "Consolas", "Monaco", monospace'
  },

  fontSize: {
    xs: '0.75rem',    // 12px - 時間戳、字數統計
    sm: '0.875rem',   // 14px - 次要文字
    md: '1rem',       // 16px - 主要文字 (訊息內容)
    lg: '1.125rem',   // 18px - 標題
    xl: '1.25rem',    // 20px - 大標題
    xxl: '1.5rem'     // 24px - 頁面標題
  },

  fontWeight: {
    regular: 400,     // 一般文字
    medium: 500,      // 按鈕、強調文字
    semibold: 600,    // 標題
    bold: 700         // 重要強調
  },

  lineHeight: {
    tight: 1.2,       // 標題行高
    normal: 1.5,      // 正文行高
    relaxed: 1.75     // 段落行高
  }
}
```

### Border Radius (圓角系統)

```typescript
const borderRadius = {
  none: 0,           // 無圓角
  sm: '4px',         // 小圓角 (按鈕、輸入框)
  md: '8px',         // 中圓角 (Card、訊息氣泡)
  lg: '12px',        // 大圓角 (對話容器)
  xl: '16px',        // 超大圓角 (特殊容器)
  full: '9999px'     // 完全圓角 (圓形按鈕)
}
```

### Shadows (陰影系統)

```typescript
const shadows = {
  none: 'none',

  sm: '0 1px 2px 0 rgba(0, 0, 0, 0.05)',           // 微陰影 (輸入框)
  md: '0 4px 6px -1px rgba(0, 0, 0, 0.1)',         // 中陰影 (Card)
  lg: '0 10px 15px -3px rgba(0, 0, 0, 0.1)',       // 大陰影 (Modal)
  xl: '0 20px 25px -5px rgba(0, 0, 0, 0.1)',       // 超大陰影 (Dropdown)

  inner: 'inset 0 2px 4px 0 rgba(0, 0, 0, 0.06)'   // 內陰影 (Active input)
}
```

### Animation (動畫系統)

```typescript
const animation = {
  duration: {
    fast: '150ms',      // 快速動畫 (Hover 效果)
    normal: '250ms',    // 標準動畫 (Transition)
    slow: '350ms',      // 慢速動畫 (Modal 進出)
    typing: '20ms'      // 打字機效果 (每個字符)
  },

  easing: {
    easeInOut: 'cubic-bezier(0.4, 0, 0.2, 1)',     // 標準緩動
    easeOut: 'cubic-bezier(0.0, 0, 0.2, 1)',       // 出場緩動
    easeIn: 'cubic-bezier(0.4, 0, 1, 1)',          // 入場緩動
    sharp: 'cubic-bezier(0.4, 0, 0.6, 1)'          // 銳利緩動
  }
}
```

---

## Layout 設計

### 整體佈局結構

```
┌─────────────────────────────────────────────────────────────┐
│ ChatPage (Full Viewport)                                    │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ ChatLayout (Flex Container)                             │ │
│ │ ┌──────────────┬────────────────────────────────────────┐│ │
│ │ │              │                                        ││ │
│ │ │ Conversation │ ChatArea                               ││ │
│ │ │ Sidebar      │                                        ││ │
│ │ │ (280px)      │ ┌────────────────────────────────────┐││ │
│ │ │              │ │ ChatHeader (64px)                  │││ │
│ │ │ ┌──────────┐ │ └────────────────────────────────────┘││ │
│ │ │ │ New Conv │ │ ┌────────────────────────────────────┐││ │
│ │ │ └──────────┘ │ │                                    │││ │
│ │ │              │ │ MessageList (Flex-grow)            │││ │
│ │ │ ┌──────────┐ │ │ - Scrollable                       │││ │
│ │ │ │ Conv 1   │ │ │ - Auto-scroll to bottom            │││ │
│ │ │ └──────────┘ │ │                                    │││ │
│ │ │ ┌──────────┐ │ └────────────────────────────────────┘││ │
│ │ │ │ Conv 2   │ │ ┌────────────────────────────────────┐││ │
│ │ │ └──────────┘ │ │ ChatInput (Auto-height, max 200px) │││ │
│ │ │              │ │ - Textarea                         │││ │
│ │ │ ...          │ │ - Send button                      │││ │
│ │ │              │ │ - Character counter                │││ │
│ │ │              │ └────────────────────────────────────┘││ │
│ │ └──────────────┴────────────────────────────────────────┘│ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### 尺寸規格

```typescript
const layoutSizes = {
  sidebar: {
    width: 280,                  // Sidebar 固定寬度
    minWidth: 240,               // 最小寬度 (響應式收縮)
    maxWidth: 320                // 最大寬度 (響應式放大)
  },

  chatHeader: {
    height: 64                   // Header 固定高度
  },

  chatInput: {
    minHeight: 56,               // 輸入框最小高度 (單行)
    maxHeight: 200,              // 輸入框最大高度 (多行滾動)
    padding: 16                  // 輸入框內邊距
  },

  message: {
    maxWidth: 720,               // 訊息氣泡最大寬度
    minHeight: 48,               // 訊息氣泡最小高度
    padding: 16,                 // 訊息氣泡內邊距
    gap: 12                      // 訊息間距
  }
}
```

---

## Component 設計

### 1. ConversationSidebar (對話側邊欄)

#### 視覺設計
```
┌─────────────────────────┐
│ ┌─────────────────────┐ │  ← New Conversation Button
│ │ ➕ 新對話            │ │    (Full width, primary color)
│ └─────────────────────┘ │
│                         │
│ ┌─────────────────────┐ │  ← Conversation Item (Active)
│ │ 💬 對話標題 1        │ │    (Background: primary.light + opacity 0.1)
│ │ 最後訊息...          │ │    (Border-left: 3px primary)
│ │ 2 分鐘前            │ │
│ └─────────────────────┘ │
│                         │
│ ┌─────────────────────┐ │  ← Conversation Item (Inactive)
│ │ 💬 對話標題 2        │ │    (Background: transparent)
│ │ 最後訊息...          │ │    (Hover: background.paper)
│ │ 1 小時前            │ │
│ └─────────────────────┘ │
│                         │
│ ...                     │
└─────────────────────────┘
```

#### Design Specs
```typescript
const conversationSidebar = {
  background: neutralColors.background.paper,    // #f9f9f9
  padding: spacing.lg,                           // 24px
  borderRight: `1px solid ${neutralColors.divider}`,

  newButton: {
    height: 48,
    borderRadius: borderRadius.md,               // 8px
    background: colors.primary.main,
    color: '#fff',
    fontWeight: typography.fontWeight.medium,
    fontSize: typography.fontSize.md,
    '&:hover': {
      background: colors.primary.light
    }
  },

  conversationItem: {
    padding: spacing.md,                         // 16px
    marginBottom: spacing.sm,                    // 8px
    borderRadius: borderRadius.md,               // 8px
    cursor: 'pointer',
    transition: `background ${animation.duration.fast}`,

    '&.active': {
      background: 'rgba(25, 118, 210, 0.08)',    // primary + opacity
      borderLeft: `3px solid ${colors.primary.main}`
    },

    '&:hover:not(.active)': {
      background: neutralColors.background.paper
    }
  },

  itemTitle: {
    fontSize: typography.fontSize.md,            // 16px
    fontWeight: typography.fontWeight.medium,    // 500
    color: neutralColors.text.primary,
    overflow: 'hidden',
    textOverflow: 'ellipsis',
    whiteSpace: 'nowrap'
  },

  itemPreview: {
    fontSize: typography.fontSize.sm,            // 14px
    color: neutralColors.text.secondary,
    overflow: 'hidden',
    textOverflow: 'ellipsis',
    whiteSpace: 'nowrap',
    marginTop: spacing.xs                        // 4px
  },

  itemTimestamp: {
    fontSize: typography.fontSize.xs,            // 12px
    color: neutralColors.text.hint,
    marginTop: spacing.xs                        // 4px
  }
}
```

### 2. ChatHeader (對話標題欄)

#### 視覺設計
```
┌─────────────────────────────────────────────────────────┐
│ 💬 對話標題 1                                  ⋯ ⚙️     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

#### Design Specs
```typescript
const chatHeader = {
  height: layoutSizes.chatHeader.height,         // 64px
  padding: `0 ${spacing.xl}px`,                  // 0 32px
  borderBottom: `1px solid ${neutralColors.divider}`,
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'space-between',
  background: neutralColors.background.default,  // #fff

  title: {
    fontSize: typography.fontSize.xl,            // 20px
    fontWeight: typography.fontWeight.semibold,  // 600
    color: neutralColors.text.primary
  },

  actions: {
    display: 'flex',
    gap: spacing.sm,                             // 8px

    iconButton: {
      width: 40,
      height: 40,
      borderRadius: borderRadius.sm,             // 4px
      color: neutralColors.text.secondary,

      '&:hover': {
        background: neutralColors.background.paper
      }
    }
  }
}
```

### 3. MessageList (訊息列表)

#### 視覺設計
```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│                   ┌─────────────────────┐              │  ← User Message
│                   │ 你好，我想問一個問題  │              │    (Right aligned, blue)
│                   │ 2:30 PM            │              │
│                   └─────────────────────┘              │
│                                                         │
│ ┌─────────────────────────────────────────┐            │  ← Agent Message
│ │ 您好!我很樂意幫助您。請問有什麼問題呢?   │            │    (Left aligned, gray)
│ │                                         │            │
│ │ 2:30 PM                                │            │
│ └─────────────────────────────────────────┘            │
│                                                         │
│                   ┌─────────────────────┐              │
│                   │ 請解釋 React Hooks   │              │
│                   │ 2:31 PM            │              │
│                   └─────────────────────┘              │
│                                                         │
│ ┌─────────────────────────────────────────┐            │
│ │ React Hooks 是 React 16.8 引入的新功能... │          │  ← Streaming
│ │ █                                       │            │    (Cursor blink)
│ │ 2:31 PM                 ⏹️ 停止生成      │            │
│ └─────────────────────────────────────────┘            │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

#### Design Specs
```typescript
const messageList = {
  flexGrow: 1,
  overflowY: 'auto',
  padding: spacing.xl,                           // 32px
  background: neutralColors.background.chat,     // #fff
  display: 'flex',
  flexDirection: 'column',
  gap: spacing.md,                               // 16px

  // Scrollbar styling
  '&::-webkit-scrollbar': {
    width: 8
  },
  '&::-webkit-scrollbar-thumb': {
    background: neutralColors.border,
    borderRadius: borderRadius.sm
  }
}
```

### 4. Message (訊息氣泡)

#### User Message Design
```typescript
const userMessage = {
  container: {
    display: 'flex',
    justifyContent: 'flex-end',                  // 右對齊
    marginBottom: spacing.md                     // 16px
  },

  bubble: {
    maxWidth: layoutSizes.message.maxWidth,      // 720px
    padding: spacing.md,                         // 16px
    borderRadius: borderRadius.md,               // 8px
    background: messageColors.user.background,   // #1976d2 (藍色)
    color: messageColors.user.text,              // #fff
    boxShadow: shadows.sm
  },

  content: {
    fontSize: typography.fontSize.md,            // 16px
    lineHeight: typography.lineHeight.normal,    // 1.5
    whiteSpace: 'pre-wrap',                      // 保留換行
    wordBreak: 'break-word'                      // 長單詞換行
  },

  timestamp: {
    fontSize: typography.fontSize.xs,            // 12px
    color: messageColors.user.timestamp,         // rgba(255,255,255,0.7)
    marginTop: spacing.xs,                       // 4px
    textAlign: 'right'
  }
}
```

#### Agent Message Design
```typescript
const agentMessage = {
  container: {
    display: 'flex',
    justifyContent: 'flex-start',                // 左對齊
    marginBottom: spacing.md                     // 16px
  },

  bubble: {
    maxWidth: layoutSizes.message.maxWidth,      // 720px
    padding: spacing.md,                         // 16px
    borderRadius: borderRadius.md,               // 8px
    background: messageColors.agent.background,  // #f5f5f5 (淺灰)
    color: messageColors.agent.text,             // #212121
    boxShadow: shadows.sm,
    border: `1px solid ${neutralColors.border}`
  },

  content: {
    fontSize: typography.fontSize.md,            // 16px
    lineHeight: typography.lineHeight.normal,    // 1.5

    // Markdown 樣式
    '& h1, & h2, & h3': {
      marginTop: spacing.md,
      marginBottom: spacing.sm,
      fontWeight: typography.fontWeight.semibold
    },

    '& h1': { fontSize: typography.fontSize.xxl },  // 24px
    '& h2': { fontSize: typography.fontSize.xl },   // 20px
    '& h3': { fontSize: typography.fontSize.lg },   // 18px

    '& p': {
      marginBottom: spacing.sm
    },

    '& ul, & ol': {
      paddingLeft: spacing.lg,
      marginBottom: spacing.sm
    },

    '& code': {
      background: 'rgba(0, 0, 0, 0.05)',
      padding: '2px 6px',
      borderRadius: borderRadius.sm,
      fontFamily: typography.fontFamily.code,
      fontSize: '0.9em'
    },

    '& pre': {
      background: '#1e1e1e',                       // 深色代碼背景
      padding: spacing.md,
      borderRadius: borderRadius.md,
      overflow: 'auto',
      marginBottom: spacing.sm,

      '& code': {
        background: 'transparent',
        color: '#d4d4d4',
        padding: 0
      }
    },

    '& blockquote': {
      borderLeft: `4px solid ${colors.primary.main}`,
      paddingLeft: spacing.md,
      marginLeft: 0,
      fontStyle: 'italic',
      color: neutralColors.text.secondary
    },

    '& table': {
      width: '100%',
      borderCollapse: 'collapse',
      marginBottom: spacing.sm,

      '& th, & td': {
        border: `1px solid ${neutralColors.border}`,
        padding: spacing.sm,
        textAlign: 'left'
      },

      '& th': {
        background: neutralColors.background.paper,
        fontWeight: typography.fontWeight.semibold
      }
    }
  },

  footer: {
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginTop: spacing.sm
  },

  timestamp: {
    fontSize: typography.fontSize.xs,            // 12px
    color: messageColors.agent.timestamp         // rgba(0,0,0,0.5)
  },

  stopButton: {
    fontSize: typography.fontSize.sm,
    padding: `${spacing.xs}px ${spacing.sm}px`,  // 4px 8px
    borderRadius: borderRadius.sm,
    background: statusColors.warning,
    color: '#fff',
    border: 'none',
    cursor: 'pointer',
    display: 'flex',
    alignItems: 'center',
    gap: spacing.xs,

    '&:hover': {
      background: '#f57c00'                      // Darker orange
    }
  }
}
```

### 5. ChatInput (輸入區域)

#### 視覺設計
```
┌─────────────────────────────────────────────────────────┐
│ ┌─────────────────────────────────────────────────────┐ │
│ │ 輸入訊息... (Placeholder)                            │ │
│ │                                                     │ │  ← Textarea
│ │                                                     │ │    (Auto-height)
│ │                                                     │ │
│ └─────────────────────────────────────────────────────┘ │
│ 236 / 2000                                      [發送] │  ← Footer
└─────────────────────────────────────────────────────────┘
```

#### Design Specs
```typescript
const chatInput = {
  container: {
    padding: spacing.lg,                         // 24px
    borderTop: `1px solid ${neutralColors.divider}`,
    background: neutralColors.background.default
  },

  textarea: {
    width: '100%',
    minHeight: layoutSizes.chatInput.minHeight,  // 56px
    maxHeight: layoutSizes.chatInput.maxHeight,  // 200px
    padding: spacing.md,                         // 16px
    border: `1px solid ${neutralColors.border}`,
    borderRadius: borderRadius.md,               // 8px
    fontSize: typography.fontSize.md,            // 16px
    fontFamily: typography.fontFamily.base,
    lineHeight: typography.lineHeight.normal,    // 1.5
    resize: 'none',
    overflowY: 'auto',
    background: neutralColors.background.input,  // #f5f5f5

    '&:focus': {
      outline: 'none',
      borderColor: colors.primary.main,
      boxShadow: `0 0 0 3px rgba(25, 118, 210, 0.1)`  // Focus ring
    },

    '&::placeholder': {
      color: neutralColors.text.hint
    }
  },

  footer: {
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginTop: spacing.sm                        // 8px
  },

  charCounter: {
    fontSize: typography.fontSize.xs,            // 12px
    color: neutralColors.text.secondary,

    '&.warning': {                               // > 1800 字時警告
      color: statusColors.warning
    },

    '&.error': {                                 // > 2000 字時錯誤
      color: statusColors.error,
      fontWeight: typography.fontWeight.medium
    }
  },

  sendButton: {
    height: 40,
    padding: `0 ${spacing.lg}px`,                // 0 24px
    borderRadius: borderRadius.sm,               // 4px
    background: colors.primary.main,
    color: '#fff',
    fontSize: typography.fontSize.md,
    fontWeight: typography.fontWeight.medium,
    border: 'none',
    cursor: 'pointer',
    transition: `background ${animation.duration.fast}`,

    '&:hover:not(:disabled)': {
      background: colors.primary.light
    },

    '&:disabled': {
      background: neutralColors.text.disabled,
      cursor: 'not-allowed'
    }
  }
}
```

---

## 互動設計

### 1. 輸入與發送

#### Enter / Shift+Enter 行為
```typescript
// 使用者按鍵行為
const keyboardBehavior = {
  'Enter': '發送訊息 (如果內容不為空)',
  'Shift + Enter': '插入換行符號',
  'Ctrl/Cmd + Enter': '發送訊息 (替代方案)',
  'Esc': '清空輸入框 (可選)'
}
```

#### 輸入狀態
- **正常狀態**: 字數 0-1800,黑色字數顯示
- **警告狀態**: 字數 1801-2000,橙色字數顯示
- **錯誤狀態**: 字數 >2000,紅色字數顯示,發送按鈕禁用

### 2. Streaming 效果

#### 打字機效果實現
```typescript
const streamingEffect = {
  speed: '20-30 字符/秒',
  chunkSize: '1-3 字符',             // 每次顯示 1-3 個字符
  smoothness: '流暢,無明顯卡頓',

  cursorBlink: {
    animation: 'blink 1s infinite',
    '@keyframes blink': {
      '0%, 49%': { opacity: 1 },
      '50%, 100%': { opacity: 0 }
    }
  }
}
```

#### 停止生成按鈕
- **位置**: Agent 訊息右下角
- **顏色**: 橙色 (警告色)
- **文字**: "⏹️ 停止生成"
- **Hover**: 深橙色
- **顯示時機**: Streaming 進行中

### 3. 對話管理

#### 新建對話
- **觸發**: 點擊 "➕ 新對話" 按鈕
- **行為**: 創建新對話,自動切換到新對話,清空 MessageList

#### 重命名對話
- **觸發**: 點擊對話項目右側 "⋯" → "重命名"
- **行為**: 彈出 Modal,顯示輸入框,預填當前標題

#### 刪除對話
- **觸發**: 點擊對話項目右側 "⋯" → "刪除"
- **行為**: 彈出確認 Dialog,"確定要刪除此對話嗎?"

#### 匯出對話
- **觸發**: 點擊 ChatHeader 右側 "⋯" → "匯出"
- **選項**: Markdown / PDF
- **行為**: 下載檔案 "對話標題_2025-12-12.md"

### 4. 滾動行為

#### Auto-scroll 邏輯
```typescript
const scrollBehavior = {
  onNewMessage: {
    userMessage: '立即滾動到底部',          // 用戶發送後
    agentMessageStart: '立即滾動到底部',    // Agent 開始回覆
    streaming: '保持當前位置 (可手動滾動)',  // Streaming 進行中
    streamingComplete: '滾動到底部'         // Streaming 完成
  },

  scrollOptions: {
    behavior: 'smooth',                    // 平滑滾動
    block: 'end'                           // 滾動到底部
  }
}
```

---

## 響應式設計

### Breakpoints

```typescript
const breakpoints = {
  xs: 0,        // 0-599px    - Mobile (320-599px)
  sm: 600,      // 600-959px  - Tablet (600-959px)
  md: 960,      // 960-1279px - Desktop Small (960-1279px)
  lg: 1280,     // 1280-1919px - Desktop Medium (1280-1919px)
  xl: 1920      // 1920px+    - Desktop Large (1920px+)
}
```

### 響應式佈局

#### Mobile (< 600px)
```typescript
const mobileLayout = {
  sidebar: {
    display: 'none',                       // 隱藏 Sidebar
    position: 'fixed',                     // 切換為 Drawer
    left: 0,
    top: 0,
    width: '80vw',
    height: '100vh',
    zIndex: 1300,
    transform: 'translateX(-100%)',        // 預設隱藏
    transition: 'transform 0.3s ease',

    '&.open': {
      transform: 'translateX(0)'           // 打開時滑入
    }
  },

  chatArea: {
    width: '100%'                          // 全寬
  },

  chatHeader: {
    padding: `0 ${spacing.md}px`,          // 減少 padding

    menuButton: {
      display: 'block'                     // 顯示 Hamburger 選單
    }
  },

  messageList: {
    padding: spacing.md                    // 減少 padding
  },

  messageBubble: {
    maxWidth: '85vw'                       // 減少最大寬度
  }
}
```

#### Tablet (600-959px)
```typescript
const tabletLayout = {
  sidebar: {
    width: 240,                            // 縮小 Sidebar
    minWidth: 240
  },

  chatArea: {
    width: 'calc(100% - 240px)'
  },

  messageBubble: {
    maxWidth: 600                          // 縮小訊息氣泡
  }
}
```

#### Desktop (960px+)
```typescript
const desktopLayout = {
  // 使用預設佈局規格 (上述 layoutSizes)
}
```

---

## Accessibility 設計

### WCAG 2.1 AA 標準

#### 1. 色彩對比 (Contrast Ratio)
```typescript
const contrastRatios = {
  normalText: {
    minimum: 4.5,                          // WCAG AA 標準
    primary: 7.2,                          // #212121 on #fff
    secondary: 4.6                         // #757575 on #fff
  },

  largeText: {
    minimum: 3.0,                          // WCAG AA 標準 (18px+)
    headers: 7.2
  },

  userMessage: {
    text: 4.8                              // #fff on #1976d2
  },

  agentMessage: {
    text: 12.6                             // #212121 on #f5f5f5
  }
}
```

#### 2. 鍵盤導航 (Keyboard Navigation)
```typescript
const keyboardNavigation = {
  'Tab': '在可聚焦元素間移動 (按鈕、輸入框)',
  'Shift + Tab': '反向移動',
  'Enter': '激活按鈕或發送訊息',
  'Escape': '關閉 Modal/Dialog',
  'Arrow Keys': '在對話列表中移動 (可選)',
  'Ctrl/Cmd + K': '聚焦到輸入框 (可選快捷鍵)'
}

// Focus 樣式
const focusStyles = {
  outline: `2px solid ${colors.primary.main}`,
  outlineOffset: 2,
  borderRadius: borderRadius.sm
}
```

#### 3. ARIA 屬性
```typescript
const ariaAttributes = {
  conversationSidebar: {
    'role': 'navigation',
    'aria-label': '對話列表'
  },

  conversationItem: {
    'role': 'button',
    'aria-label': '切換到對話: {conversationTitle}',
    'aria-current': 'page'                 // Active conversation
  },

  messageList: {
    'role': 'log',
    'aria-live': 'polite',                 // Announce new messages
    'aria-atomic': 'false'
  },

  userMessage: {
    'role': 'article',
    'aria-label': '您的訊息: {messageContent}'
  },

  agentMessage: {
    'role': 'article',
    'aria-label': 'Agent 回覆: {messageContent}',
    'aria-busy': 'true'                    // Streaming 進行中
  },

  chatInput: {
    'aria-label': '輸入訊息',
    'aria-describedby': 'char-counter',    // 關聯字數統計
    'aria-multiline': 'true'
  },

  sendButton: {
    'aria-label': '發送訊息',
    'aria-disabled': 'true'                // 輸入為空時
  },

  stopButton: {
    'aria-label': '停止生成回覆'
  }
}
```

#### 4. 屏幕閱讀器支援
```typescript
const screenReaderSupport = {
  liveRegions: {
    messageList: 'polite',                 // 新訊息到達時通知
    charCounter: 'polite',                 // 接近字數限制時通知
    errorMessages: 'assertive'             // 錯誤訊息立即通知
  },

  hiddenText: {
    conversationActions: '顯示對話操作選單',
    messageTimestamp: '訊息時間: {timestamp}'
  }
}
```

---

## 設計驗收標準

### 視覺設計 ✅
- [ ] 所有顏色符合 WCAG AA 對比度標準 (4.5:1 最低)
- [ ] Spacing 系統一致應用 (xs/sm/md/lg/xl)
- [ ] Typography 清晰易讀,字體大小適當
- [ ] Border radius 統一,視覺流暢
- [ ] Shadows 自然,不過度突出

### 互動設計 ✅
- [ ] Enter 發送,Shift+Enter 換行正常運作
- [ ] 字數統計實時更新,超過限制時禁用發送
- [ ] Streaming 打字機效果流暢 (20-30 字符/秒)
- [ ] 停止生成按鈕顯示時機正確
- [ ] Auto-scroll 邏輯符合預期

### 響應式設計 ✅
- [ ] Mobile (<600px) 佈局正確,Sidebar 切換為 Drawer
- [ ] Tablet (600-959px) 佈局適配良好
- [ ] Desktop (960px+) 完整顯示所有功能
- [ ] 跨設備一致性,無佈局破損

### Accessibility ✅
- [ ] 所有互動元素可鍵盤訪問 (Tab/Enter/Escape)
- [ ] Focus 樣式清晰可見
- [ ] ARIA 屬性正確標記
- [ ] 屏幕閱讀器測試通過 (NVDA/VoiceOver)
- [ ] 色盲友好 (不僅依賴顏色傳遞資訊)

---

## 附錄: Design Token 導出

### CSS Variables
```css
:root {
  /* Colors */
  --color-primary: #1976d2;
  --color-primary-light: #42a5f5;
  --color-primary-dark: #1565c0;

  --color-message-user-bg: #1976d2;
  --color-message-user-text: #ffffff;
  --color-message-agent-bg: #f5f5f5;
  --color-message-agent-text: #212121;

  --color-background: #ffffff;
  --color-background-paper: #f9f9f9;
  --color-text-primary: #212121;
  --color-text-secondary: #757575;
  --color-divider: #e0e0e0;

  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;

  /* Typography */
  --font-family-base: "Roboto", "Noto Sans TC", "Microsoft JhengHei", sans-serif;
  --font-family-code: "Fira Code", "Consolas", "Monaco", monospace;
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;

  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);

  /* Animation */
  --duration-fast: 150ms;
  --duration-normal: 250ms;
  --easing: cubic-bezier(0.4, 0, 0.2, 1);
}
```

### TypeScript Type Definitions
```typescript
// src/theme/tokens.ts
export const designTokens = {
  colors: { /* ... */ },
  spacing: { /* ... */ },
  typography: { /* ... */ },
  borderRadius: { /* ... */ },
  shadows: { /* ... */ },
  animation: { /* ... */ }
} as const;

export type DesignTokens = typeof designTokens;
```

---

**文檔版本**: 1.0.0
**最後更新**: 2025-12-12
**設計者**: Claude Code
**審核狀態**: ⏳ 待審核
