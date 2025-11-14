# Sprint 12 開發檢查清單 (Development Checklist)

**版本**: v2.1
**Sprint 編號**: Sprint 12
**Sprint 週期**: Week 34-36 (3 週)
**Phase**: Phase 1D - 工作流編輯器 Frontend (Workflow Editor Frontend)
**計劃日期**: 2026-06-02 ~ 2026-06-22
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 目錄

1. [檢查清單概述](#檢查清單概述)
2. [Phase 1: VueFlow 集成](#phase-1-vueflow-集成)
3. [Phase 2: Drag-drop Node Editor](#phase-2-drag-drop-node-editor)
4. [Phase 3: 基礎節點類型](#phase-3-基礎節點類型)
5. [Phase 4: Testing & Documentation](#phase-4-testing--documentation)
6. [Phase 1D 啟動檢查清單](#phase-1d-啟動檢查清單)
7. [進度追蹤](#進度追蹤)

---

## 檢查清單概述

### 總覽

Sprint 12 是 **Phase 1D (工作流編輯器 Frontend)** 的第一個 Sprint，包含 **160+ 任務項目**，分為 4 個主要階段。

**階段劃分**:
```
Phase 1: VueFlow 集成 (50+ tasks)
  └─ Week 34, Day 1-5

Phase 2: Drag-drop Node Editor (40+ tasks)
  └─ Week 35, Day 1-5

Phase 3: 基礎節點類型 (35+ tasks)
  └─ Week 36, Day 1-3

Phase 4: Testing & Documentation (35+ tasks)
  └─ Week 36, Day 4-5
```

### 任務編號規則

**格式**: `T12.XXX`
- `T12`: Sprint 12
- `XXX`: 3 位數序號 (001-999)

**分類前綴**:
- `T12.0XX`: Phase 1 (VueFlow 集成)
- `T12.1XX`: Phase 2 (Drag-drop Editor)
- `T12.2XX`: Phase 3 (基礎節點類型)
- `T12.3XX`: Phase 4 (Testing & Documentation)

### 優先級標記

- 🔴 **P0**: 關鍵任務,必須完成
- 🟡 **P1**: 重要任務,應該完成
- 🟢 **P2**: 次要任務,可選完成

### 狀態標記

- ⏳ **待開始** (Pending)
- 🔄 **進行中** (In Progress)
- ✅ **已完成** (Completed)
- ⚠️ **受阻** (Blocked)
- ❌ **已取消** (Cancelled)

---

## Phase 1: VueFlow 集成

**時程**: Week 34, Day 1-5 (2026-06-02 ~ 2026-06-06)
**Story Points**: 3 SP
**任務數量**: 52 tasks

### 1.1 環境設置 (Environment Setup)

#### T12.001 - 建立 Vue 3 專案結構 🔴 P0
- [ ] ⏳ 建立 `packages/remote` 目錄
- [ ] ⏳ 初始化 npm 專案 (`npm init`)
- [ ] ⏳ 建立基本目錄結構 (src, components, composables, stores, types)
- [ ] ⏳ 設定 `.gitignore`
- [ ] ⏳ 建立 `README.md`

**驗收標準**:
- 目錄結構符合規範
- `package.json` 正確配置
- `.gitignore` 包含必要項目

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

#### T12.002 - 安裝核心依賴 🔴 P0
- [ ] ⏳ 安裝 Vue 3 (`npm install vue@^3.3.0`)
- [ ] ⏳ 安裝 TypeScript (`npm install -D typescript@^5.2.0`)
- [ ] ⏳ 安裝 Vite (`npm install -D vite@^5.0.0`)
- [ ] ⏳ 安裝 Vue plugin for Vite (`npm install -D @vitejs/plugin-vue@^5.0.0`)
- [ ] ⏳ 驗證版本相容性

**驗收標準**:
- 所有依賴正確安裝
- `package.json` 記錄正確版本
- 無版本衝突警告

**負責人**: Frontend Lead
**預估時間**: 30 minutes

---

#### T12.003 - 安裝 VueFlow 🔴 P0
- [ ] ⏳ 安裝 `@vue-flow/core@^1.45.0`
- [ ] ⏳ 安裝 `@vue-flow/background@^1.3.0`
- [ ] ⏳ 安裝 `@vue-flow/controls@^1.1.0`
- [ ] ⏳ 安裝 `@vue-flow/minimap@^1.4.0`
- [ ] ⏳ 驗證 VueFlow 版本

**驗收標準**:
- VueFlow 核心和擴展套件正確安裝
- 版本符合要求 (1.45.0+)
- 可正確 import

**負責人**: Frontend Developer
**預估時間**: 30 minutes

---

#### T12.004 - 安裝 State Management 🔴 P0
- [ ] ⏳ 安裝 Pinia (`npm install pinia@^2.1.0`)
- [ ] ⏳ 安裝 VueUse (`npm install @vueuse/core@^10.0.0`)
- [ ] ⏳ 配置 Pinia 設定
- [ ] ⏳ 建立 store 目錄結構

**驗收標準**:
- Pinia 正確安裝並配置
- VueUse composables 可用
- Store 目錄結構建立

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.005 - 安裝 UI Framework 🔴 P0
- [ ] ⏳ 安裝 Element Plus (`npm install element-plus@^2.4.0`)
- [ ] ⏳ 安裝 Element Plus Icons (`npm install @element-plus/icons-vue@^2.1.0`)
- [ ] ⏳ 配置 Element Plus auto-import
- [ ] ⏳ 設定 Element Plus 樣式

**驗收標準**:
- Element Plus 正確安裝
- Icons 可正常使用
- 樣式正確載入

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.006 - 配置 TypeScript 🔴 P0
- [ ] ⏳ 建立 `tsconfig.json`
- [ ] ⏳ 配置 compiler options (strict mode, module resolution)
- [ ] ⏳ 設定 path aliases (@/ for src)
- [ ] ⏳ 配置 Vue types
- [ ] ⏳ 驗證 TypeScript 編譯

**驗收標準**:
- `tsconfig.json` 配置正確
- Path aliases 運作正常
- 無 TypeScript 錯誤

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

#### T12.007 - 配置 Vite 🔴 P0
- [ ] ⏳ 建立 `vite.config.ts`
- [ ] ⏳ 配置 Vue plugin
- [ ] ⏳ 設定 dev server (port 3001, CORS)
- [ ] ⏳ 配置 build options
- [ ] ⏳ 測試 dev server 啟動

**驗收標準**:
- Vite 配置正確
- Dev server 可啟動
- HMR 正常運作

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

#### T12.008 - 配置 ESLint & Prettier 🟡 P1
- [ ] ⏳ 安裝 ESLint (`npm install -D eslint@^8.50.0`)
- [ ] ⏳ 安裝 Prettier (`npm install -D prettier@^3.0.0`)
- [ ] ⏳ 配置 ESLint rules (`.eslintrc.js`)
- [ ] ⏳ 配置 Prettier options (`.prettierrc`)
- [ ] ⏳ 設定 ESLint + Prettier 整合
- [ ] ⏳ 測試 lint 和 format

**驗收標準**:
- ESLint 規則正確設定
- Prettier 格式化正常
- 無規則衝突

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

### 1.2 Module Federation 配置 (Module Federation Setup)

#### T12.009 - 安裝 Module Federation Plugin 🔴 P0
- [ ] ⏳ 安裝 `@originjs/vite-plugin-federation@^1.3.0`
- [ ] ⏳ 驗證 plugin 版本
- [ ] ⏳ 閱讀 plugin 文檔

**驗收標準**:
- Plugin 正確安裝
- 版本符合要求

**負責人**: Frontend Lead
**預估時間**: 30 minutes

---

#### T12.010 - 配置 Remote Application 🔴 P0
- [ ] ⏳ 在 `vite.config.ts` 中配置 federation plugin
- [ ] ⏳ 設定 remote name (`workflowEditor`)
- [ ] ⏳ 設定 filename (`remoteEntry.js`)
- [ ] ⏳ 配置 exposes (WorkflowEditor, NodePalette)
- [ ] ⏳ 配置 shared dependencies (vue, pinia, element-plus, @vue-flow/core)

**驗收標準**:
- Federation plugin 正確配置
- Exposes 定義正確
- Shared dependencies 設定完整

**負責人**: Frontend Lead
**預估時間**: 2 hours

**配置範例**:
```typescript
federation({
  name: 'workflowEditor',
  filename: 'remoteEntry.js',
  exposes: {
    './WorkflowEditor': './src/WorkflowEditor.vue',
    './NodePalette': './src/components/NodePalette.vue'
  },
  shared: ['vue', 'pinia', 'element-plus', '@vue-flow/core']
})
```

---

#### T12.011 - 建立 Bootstrap Entry 🔴 P0
- [ ] ⏳ 建立 `src/bootstrap.ts`
- [ ] ⏳ 實現 `mount()` 函數
- [ ] ⏳ 實現 `unmount()` 函數
- [ ] ⏳ 設定 development mode entry
- [ ] ⏳ 測試 mount/unmount lifecycle

**驗收標準**:
- Bootstrap 正確實現
- Mount/unmount 運作正常
- Dev mode 可獨立運行

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.012 - 配置 Host Application 🔴 P0
- [ ] ⏳ 在 `packages/host/vite.config.ts` 中配置 federation
- [ ] ⏳ 設定 remotes (`workflowEditor: http://localhost:3001/assets/remoteEntry.js`)
- [ ] ⏳ 配置 shared dependencies
- [ ] ⏳ 測試 remote 載入

**驗收標準**:
- Host 配置正確
- Remote URL 可訪問
- 無 CORS 錯誤

**負責人**: Frontend Lead
**預估時間**: 1.5 hours

---

#### T12.013 - 建立 WorkflowEditorWrapper 🔴 P0
- [ ] ⏳ 在 Host 建立 `WorkflowEditorWrapper.tsx`
- [ ] ⏳ 實現 lazy loading (`React.lazy`)
- [ ] ⏳ 實現 Suspense fallback
- [ ] ⏳ 實現 props 傳遞 (workflowId, onSave, onCancel)
- [ ] ⏳ 實現 lifecycle 管理
- [ ] ⏳ 測試跨框架整合

**驗收標準**:
- Wrapper 正確實現
- Props 正確傳遞
- Lifecycle 管理正常
- 無記憶體洩漏

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.014 - 測試 Module Federation 🔴 P0
- [ ] ⏳ 啟動 Host 和 Remote dev servers
- [ ] ⏳ 測試 Remote 載入
- [ ] ⏳ 測試 Props 傳遞
- [ ] ⏳ 測試 HMR
- [ ] ⏳ 測試錯誤邊界
- [ ] ⏳ 記錄問題和解決方案

**驗收標準**:
- Remote 成功載入到 Host
- Props 正確傳遞
- HMR 正常運作
- 錯誤處理正確

**負責人**: Frontend Team
**預估時間**: 2 hours

---

### 1.3 VueFlow 基礎整合 (VueFlow Basic Integration)

#### T12.015 - 建立 WorkflowEditor.vue 🔴 P0
- [ ] ⏳ 建立 `src/WorkflowEditor.vue`
- [ ] ⏳ 設定 props interface (workflowId, onSave, onCancel)
- [ ] ⏳ 建立基本 template 結構
- [ ] ⏳ 實現 Composition API setup

**驗收標準**:
- 元件檔案建立
- Props 正確定義
- Template 結構清晰

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.016 - 整合 VueFlow 元件 🔴 P0
- [ ] ⏳ Import VueFlow 核心元件
- [ ] ⏳ Import Background, Controls, MiniMap
- [ ] ⏳ 在 template 中使用 VueFlow
- [ ] ⏳ 設定基本配置 (snap-to-grid, zoom)
- [ ] ⏳ 測試畫布渲染

**驗收標準**:
- VueFlow 成功渲染
- Background grid 顯示
- Controls 和 MiniMap 正常

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.017 - 配置 VueFlow Options 🔴 P0
- [ ] ⏳ 設定 `snap-to-grid="true"`
- [ ] ⏳ 設定 `snap-grid="[15, 15]"`
- [ ] ⏳ 設定 `default-zoom="1"`
- [ ] ⏳ 設定 `min-zoom="0.2"`
- [ ] ⏳ 設定 `max-zoom="4"`
- [ ] ⏳ 設定 `delete-key-code="null"` (自定義刪除)
- [ ] ⏳ 測試所有配置

**驗收標準**:
- Snap to grid 正常運作
- Zoom 範圍正確 (0.2x - 4x)
- 預設 zoom 為 1x

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.018 - 實現 useVueFlow Composable 🔴 P0
- [ ] ⏳ 建立 `src/composables/useVueFlow.ts`
- [ ] ⏳ Import VueFlow hooks (`useVueFlow` from @vue-flow/core)
- [ ] ⏳ 實現 `addNodeAtPosition()`
- [ ] ⏳ 實現 `connectNodes()`
- [ ] ⏳ 實現 `autoLayout()`
- [ ] ⏳ 實現 `fitView()`
- [ ] ⏳ 實現 store 同步 (watch nodes/edges)

**驗收標準**:
- Composable 正確實現
- 所有方法運作正常
- Store 同步正確

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.019 - 建立 Workflow Store 🔴 P0
- [ ] ⏳ 建立 `src/stores/workflow.ts`
- [ ] ⏳ 定義 state (currentWorkflow, nodes, edges, selectedNodeId, isDirty)
- [ ] ⏳ 實現 getters (selectedNode, canSave, nodeCount, edgeCount)
- [ ] ⏳ 實現 actions (loadWorkflow, addNode, updateNode, removeNode, etc.)
- [ ] ⏳ 實現 `validateWorkflow()`
- [ ] ⏳ 實現 `saveWorkflow()`
- [ ] ⏳ 測試 store 功能

**驗收標準**:
- Store 正確定義
- 所有 actions 運作正常
- Validation 邏輯正確

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.020 - 建立 Node Store 🟡 P1
- [ ] ⏳ 建立 `src/stores/nodes.ts`
- [ ] ⏳ 定義 nodeTypes array
- [ ] ⏳ 實現 `getNodeType()`
- [ ] ⏳ 實現 `getNodesByCategory()`
- [ ] ⏳ 實現 `createNodeConfig()`
- [ ] ⏳ 測試 store 功能

**驗收標準**:
- Node store 正確實現
- Node types 定義完整
- 方法運作正常

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.021 - 實現 Background Pattern 🟡 P1
- [ ] ⏳ 配置 Background 元件
- [ ] ⏳ 設定 pattern-color (`#aaa`)
- [ ] ⏳ 設定 gap (15)
- [ ] ⏳ 測試 background 顯示
- [ ] ⏳ 調整視覺效果

**驗收標準**:
- Grid pattern 正確顯示
- 顏色和間距符合設計

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.022 - 實現 Controls 🟡 P1
- [ ] ⏳ 配置 Controls 元件
- [ ] ⏳ 測試 zoom in/out
- [ ] ⏳ 測試 fit view
- [ ] ⏳ 測試 lock/unlock
- [ ] ⏳ 自定義控制項樣式 (optional)

**驗收標準**:
- Controls 正常運作
- 所有按鈕功能正確
- 視覺效果符合設計

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.023 - 實現 MiniMap 🟢 P2
- [ ] ⏳ 配置 MiniMap 元件
- [ ] ⏳ 設定位置 (右下角)
- [ ] ⏳ 設定尺寸
- [ ] ⏳ 測試 MiniMap 同步
- [ ] ⏳ 調整視覺效果

**驗收標準**:
- MiniMap 正確顯示
- 與主畫布同步
- 點擊跳轉功能正常

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.024 - 實現 Canvas 樣式 🟡 P1
- [ ] ⏳ 設定 Canvas 容器樣式 (100% width/height)
- [ ] ⏳ 設定 VueFlow 樣式
- [ ] ⏳ Import VueFlow CSS (`@vue-flow/core/dist/style.css`)
- [ ] ⏳ Import theme CSS (`@vue-flow/core/dist/theme-default.css`)
- [ ] ⏳ 自定義樣式變數

**驗收標準**:
- 畫布填滿容器
- 樣式正確套用
- 無視覺錯誤

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.025 - 測試 VueFlow 基礎功能 🔴 P0
- [ ] ⏳ 測試畫布渲染
- [ ] ⏳ 測試縮放 (0.2x - 4x)
- [ ] ⏳ 測試拖曳移動
- [ ] ⏳ 測試 snap to grid
- [ ] ⏳ 測試 fit view
- [ ] ⏳ 記錄問題

**驗收標準**:
- 所有基礎功能正常
- 無效能問題
- 無視覺錯誤

**負責人**: Frontend Team
**預估時間**: 2 hours

---

### 1.4 型別定義 (Type Definitions)

#### T12.026 - 定義核心型別 🔴 P0
- [ ] ⏳ 建立 `src/types/index.ts`
- [ ] ⏳ 定義 `WorkflowDefinition` interface
- [ ] ⏳ 定義 `Node` interface (extends VueFlow Node)
- [ ] ⏳ 定義 `Edge` interface (extends VueFlow Edge)
- [ ] ⏳ 定義 `NodeType` interface
- [ ] ⏳ 定義 `NodeConfig` interface

**驗收標準**:
- 所有核心型別定義完整
- 與 VueFlow 型別相容
- TypeScript 編譯無錯誤

**負責人**: Frontend Lead
**預估時間**: 2 hours

---

#### T12.027 - 定義節點資料型別 🔴 P0
- [ ] ⏳ 定義 `AgentNodeData` interface
- [ ] ⏳ 定義 `PluginNodeData` interface
- [ ] ⏳ 定義 `DecisionNodeData` interface
- [ ] ⏳ 定義 `StartEndNodeData` interface

**驗收標準**:
- 節點資料型別定義正確
- 包含所有必要欄位
- 型別推導正確

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.028 - 定義 API 型別 🔴 P0
- [ ] ⏳ 定義 `WorkflowDefinitionDto` interface (與 Backend 對應)
- [ ] ⏳ 定義 `CreateWorkflowRequest` interface
- [ ] ⏳ 定義 `UpdateWorkflowRequest` interface
- [ ] ⏳ 定義 `ValidationResponse` interface
- [ ] ⏳ 定義 API error types

**驗收標準**:
- API 型別與 Backend DTO 一致
- 包含所有 API 端點型別
- 錯誤型別定義完整

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.029 - 建立型別測試 🟢 P2
- [ ] ⏳ 建立型別測試檔案
- [ ] ⏳ 測試型別相容性
- [ ] ⏳ 測試型別推導
- [ ] ⏳ 驗證 TypeScript strict mode

**驗收標準**:
- 型別測試通過
- 無 TypeScript 錯誤
- Strict mode 啟用

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

### 1.5 API 整合準備 (API Integration Preparation)

#### T12.030 - 建立 Axios Instance 🔴 P0
- [ ] ⏳ 安裝 Axios (`npm install axios@^1.6.0`)
- [ ] ⏳ 建立 `src/api/client.ts`
- [ ] ⏳ 配置 baseURL (環境變數)
- [ ] ⏳ 設定 default headers
- [ ] ⏳ 實現 request interceptor
- [ ] ⏳ 實現 response interceptor
- [ ] ⏳ 實現錯誤處理

**驗收標準**:
- Axios instance 正確配置
- Interceptors 運作正常
- 錯誤處理完整

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.031 - 實現 Workflow API 🔴 P0
- [ ] ⏳ 建立 `src/api/workflow.api.ts`
- [ ] ⏳ 實現 `get(id)` method
- [ ] ⏳ 實現 `list(params)` method
- [ ] ⏳ 實現 `create(workflow)` method
- [ ] ⏳ 實現 `update(id, workflow)` method
- [ ] ⏳ 實現 `delete(id)` method
- [ ] ⏳ 實現 `validate(workflow)` method

**驗收標準**:
- 所有 API methods 實現
- 型別正確
- 錯誤處理完整

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.032 - 實現資料轉換器 🔴 P0
- [ ] ⏳ 建立 `src/api/transformers.ts`
- [ ] ⏳ 實現 `fromDto()` (DTO → Model)
- [ ] ⏳ 實現 `toDto()` (Model → DTO)
- [ ] ⏳ 處理 JSON serialization (definition 欄位)
- [ ] ⏳ 處理日期轉換
- [ ] ⏳ 測試轉換器

**驗收標準**:
- 轉換器正確實現
- 雙向轉換正確
- 邊界情況處理

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.033 - 設定環境變數 🔴 P0
- [ ] ⏳ 建立 `.env.development`
- [ ] ⏳ 建立 `.env.production`
- [ ] ⏳ 設定 `VITE_API_BASE_URL`
- [ ] ⏳ 設定 `VITE_HOST_URL`
- [ ] ⏳ 設定 `VITE_REMOTE_PORT`
- [ ] ⏳ 加入 `.env.example`

**驗收標準**:
- 環境變數正確設定
- Dev 和 prod 環境區分
- `.env.example` 文檔化

**負責人**: Frontend Lead
**預估時間**: 30 minutes

---

#### T12.034 - 測試 API 整合 🟡 P1
- [ ] ⏳ Mock API responses
- [ ] ⏳ 測試 get workflow
- [ ] ⏳ 測試 list workflows
- [ ] ⏳ 測試 create workflow
- [ ] ⏳ 測試 update workflow
- [ ] ⏳ 測試 validation
- [ ] ⏳ 測試錯誤處理

**驗收標準**:
- Mock API 正常運作
- 所有 CRUD 操作測試通過
- 錯誤處理測試通過

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

### 1.6 開發工具與腳本 (Development Tools & Scripts)

#### T12.035 - 配置 npm Scripts 🟡 P1
- [ ] ⏳ 設定 `dev` script (啟動 dev server)
- [ ] ⏳ 設定 `build` script (建置生產版本)
- [ ] ⏳ 設定 `preview` script (預覽建置結果)
- [ ] ⏳ 設定 `lint` script (ESLint)
- [ ] ⏳ 設定 `format` script (Prettier)
- [ ] ⏳ 設定 `type-check` script (TypeScript)

**驗收標準**:
- 所有 scripts 正確配置
- Scripts 可正常執行
- package.json 文檔化

**負責人**: Frontend Lead
**預估時間**: 30 minutes

---

#### T12.036 - 設定 VS Code 配置 🟢 P2
- [ ] ⏳ 建立 `.vscode/settings.json`
- [ ] ⏳ 配置 ESLint 和 Prettier 整合
- [ ] ⏳ 配置 TypeScript settings
- [ ] ⏳ 建立 `.vscode/extensions.json` (推薦擴展)

**驗收標準**:
- VS Code 配置正確
- 推薦擴展列出
- 開發體驗優化

**負責人**: Frontend Lead
**預估時間**: 30 minutes

---

#### T12.037 - 建立 Development Guide 🟢 P2
- [ ] ⏳ 建立 `packages/remote/DEVELOPMENT.md`
- [ ] ⏳ 文檔化環境設置步驟
- [ ] ⏳ 文檔化開發流程
- [ ] ⏳ 文檔化測試指南
- [ ] ⏳ 文檔化 troubleshooting

**驗收標準**:
- Development guide 完整
- 步驟清晰易懂
- 包含常見問題

**負責人**: Frontend Lead
**預估時間**: 1.5 hours

---

### Phase 1 檢查點 (Phase 1 Checkpoint)

#### T12.038 - Phase 1 整合測試 🔴 P0
- [ ] ⏳ 啟動 Host 和 Remote dev servers
- [ ] ⏳ 測試 Module Federation 載入
- [ ] ⏳ 測試 VueFlow 畫布渲染
- [ ] ⏳ 測試 Store 功能
- [ ] ⏳ 測試 API 整合
- [ ] ⏳ 測試所有 npm scripts
- [ ] ⏳ 記錄所有問題

**驗收標準**:
- 所有整合測試通過
- 無阻礙性問題
- 問題清單建立

**負責人**: Frontend Team
**預估時間**: 3 hours

---

#### T12.039 - Phase 1 Code Review 🔴 P0
- [ ] ⏳ Review 所有程式碼
- [ ] ⏳ 檢查型別定義
- [ ] ⏳ 檢查程式碼風格
- [ ] ⏳ 檢查錯誤處理
- [ ] ⏳ 提出改進建議
- [ ] ⏳ 修正 review 問題

**驗收標準**:
- Code review 完成
- 所有問題修正
- 程式碼品質符合標準

**負責人**: Tech Lead, Frontend Lead
**預估時間**: 2 hours

---

#### T12.040 - Phase 1 文檔更新 🟡 P1
- [ ] ⏳ 更新 README.md
- [ ] ⏳ 更新 DEVELOPMENT.md
- [ ] ⏳ 更新 API 文檔
- [ ] ⏳ 更新型別文檔
- [ ] ⏳ 建立 troubleshooting guide

**驗收標準**:
- 所有文檔更新完成
- 文檔與實作一致
- 文檔清晰易懂

**負責人**: Frontend Team
**預估時間**: 2 hours

---

## Phase 2: Drag-drop Node Editor

**時程**: Week 35, Day 1-5 (2026-06-09 ~ 2026-06-13)
**Story Points**: 3 SP
**任務數量**: 42 tasks

### 2.1 Node Palette 實現 (Node Palette Implementation)

#### T12.101 - 建立 NodePalette 元件 🔴 P0
- [ ] ⏳ 建立 `src/components/NodePalette.vue`
- [ ] ⏳ 設定元件結構 (header, search, category list)
- [ ] ⏳ 實現基本樣式
- [ ] ⏳ 測試元件渲染

**驗收標準**:
- 元件檔案建立
- 結構正確
- 樣式符合設計

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.102 - 實現節點分類 🔴 P0
- [ ] ⏳ 使用 Element Plus Collapse
- [ ] ⏳ 實現分類摺疊功能
- [ ] ⏳ 從 Node Store 讀取 node types
- [ ] ⏳ 按 category 分組顯示
- [ ] ⏳ 測試分類顯示

**驗收標準**:
- 分類正確顯示
- 摺疊功能正常
- 節點正確分組

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.103 - 實現節點搜尋 🟡 P1
- [ ] ⏳ 實現搜尋輸入框
- [ ] ⏳ 實現搜尋邏輯 (label, description)
- [ ] ⏳ 實現搜尋結果過濾
- [ ] ⏳ 實現搜尋高亮
- [ ] ⏳ 測試搜尋功能

**驗收標準**:
- 搜尋功能正常
- 過濾邏輯正確
- 高亮顯示正確

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.104 - 實現節點項目 🔴 P0
- [ ] ⏳ 設計節點項目 UI (icon, label, description)
- [ ] ⏳ 實現節點項目樣式
- [ ] ⏳ 實現 hover 效果
- [ ] ⏳ 實現 active 狀態
- [ ] ⏳ 測試視覺效果

**驗收標準**:
- 節點項目正確顯示
- 樣式符合設計
- 互動效果流暢

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.105 - 實現節點圖示 🟡 P1
- [ ] ⏳ 使用 Element Plus Icons
- [ ] ⏳ 建立圖示映射 (icon name → component)
- [ ] ⏳ 實現圖示顯示
- [ ] ⏳ 調整圖示樣式和大小
- [ ] ⏳ 測試圖示顯示

**驗收標準**:
- 圖示正確顯示
- 映射邏輯正確
- 視覺效果良好

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.106 - 實現 Palette 樣式 🟡 P1
- [ ] ⏳ 設定 Palette 寬度 (280px)
- [ ] ⏳ 設定 Palette 高度 (100%)
- [ ] ⏳ 實現 border 和 shadow
- [ ] ⏳ 實現 responsive 樣式
- [ ] ⏳ 測試樣式在不同尺寸下的表現

**驗收標準**:
- Palette 樣式符合設計
- Responsive 正常
- 無視覺錯誤

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.107 - 測試 NodePalette 🔴 P0
- [ ] ⏳ 測試節點列表顯示
- [ ] ⏳ 測試分類摺疊
- [ ] ⏳ 測試搜尋功能
- [ ] ⏳ 測試樣式和互動
- [ ] ⏳ 記錄問題

**驗收標準**:
- 所有功能測試通過
- 無阻礙性問題
- 問題清單建立

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

### 2.2 Drag & Drop 功能 (Drag & Drop Functionality)

#### T12.108 - 實現 useDragDrop Composable 🔴 P0
- [ ] ⏳ 建立 `src/composables/useDragDrop.ts`
- [ ] ⏳ 實現 `onDragStart()` handler
- [ ] ⏳ 實現 `onDragOver()` handler
- [ ] ⏳ 實現 `onDrop()` handler
- [ ] ⏳ 實現 `onDragEnd()` handler
- [ ] ⏳ 實現拖曳狀態管理

**驗收標準**:
- Composable 正確實現
- 所有 handlers 運作正常
- 狀態管理正確

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.109 - 實現節點拖曳開始 🔴 P0
- [ ] ⏳ 在 NodePalette 監聽 `dragstart` 事件
- [ ] ⏳ 設定 `dataTransfer.effectAllowed = 'copy'`
- [ ] ⏳ 設定 `dataTransfer` 資料 (nodeType)
- [ ] ⏳ Emit `node-drag-start` 事件
- [ ] ⏳ 測試拖曳開始

**驗收標準**:
- Dragstart 事件正確觸發
- DataTransfer 資料正確
- 事件 emit 正常

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.110 - 實現畫布拖曳覆蓋 🔴 P0
- [ ] ⏳ 在 Canvas 監聽 `dragover` 事件
- [ ] ⏳ 設定 `event.preventDefault()`
- [ ] ⏳ 設定 `dataTransfer.dropEffect = 'copy'`
- [ ] ⏳ 測試拖曳覆蓋

**驗收標準**:
- Dragover 事件正確處理
- Drop effect 正確
- 游標顯示正確

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.111 - 實現節點放置 🔴 P0
- [ ] ⏳ 在 Canvas 監聽 `drop` 事件
- [ ] ⏳ 計算放置位置 (project to VueFlow coordinates)
- [ ] ⏳ 從 dataTransfer 讀取 nodeType
- [ ] ⏳ 建立新節點 (createNodeConfig)
- [ ] ⏳ 加入節點到 Store
- [ ] ⏳ 測試節點放置

**驗收標準**:
- Drop 事件正確處理
- 位置計算正確
- 節點成功建立

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.112 - 實現拖曳視覺回饋 🟡 P1
- [ ] ⏳ 實現拖曳時的游標樣式
- [ ] ⏳ 實現拖曳陰影 (drag image)
- [ ] ⏳ 實現拖曳到無效區域的視覺回饋
- [ ] ⏳ 測試視覺回饋

**驗收標準**:
- 游標樣式正確
- Drag image 顯示
- 無效區域回饋正確

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.113 - 實現拖曳結束處理 🟡 P1
- [ ] ⏳ 監聽 `dragend` 事件
- [ ] ⏳ 清理拖曳狀態
- [ ] ⏳ Emit `node-drag-end` 事件
- [ ] ⏳ 測試拖曳結束

**驗收標準**:
- Dragend 事件正確處理
- 狀態正確清理
- 事件 emit 正常

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.114 - 實現節點 ID 生成 🔴 P0
- [ ] ⏳ 實現 `generateNodeId()` 函數
- [ ] ⏳ 確保 ID 唯一性 (timestamp + random)
- [ ] ⏳ 測試 ID 生成
- [ ] ⏳ 處理 ID 衝突

**驗收標準**:
- ID 生成正確
- ID 唯一性保證
- 無 ID 衝突

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.115 - 測試 Drag & Drop 🔴 P0
- [ ] ⏳ 測試拖曳節點到畫布
- [ ] ⏳ 測試位置計算
- [ ] ⏳ 測試節點建立
- [ ] ⏳ 測試視覺回饋
- [ ] ⏳ 測試邊界情況
- [ ] ⏳ 記錄問題

**驗收標準**:
- 所有功能測試通過
- 位置計算準確
- 無阻礙性問題

**負責人**: Frontend Team
**預估時間**: 2 hours

---

### 2.3 Canvas Interaction (畫布互動)

#### T12.116 - 實現 useCanvasInteraction Composable 🔴 P0
- [ ] ⏳ 建立 `src/composables/useCanvasInteraction.ts`
- [ ] ⏳ 實現 `onNodeClick()` handler
- [ ] ⏳ 實現 `onPaneClick()` handler
- [ ] ⏳ 實現 `onNodeDragStop()` handler
- [ ] ⏳ 實現 `onConnect()` handler
- [ ] ⏳ 實現 `onEdgeUpdate()` handler
- [ ] ⏳ 實現 `deleteSelection()` handler
- [ ] ⏳ 實現 `onKeyDown()` handler

**驗收標準**:
- Composable 正確實現
- 所有 handlers 運作正常
- 事件處理正確

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.117 - 實現節點選取 🔴 P0
- [ ] ⏳ 監聽節點 click 事件
- [ ] ⏳ 更新 selectedNodeId (Store)
- [ ] ⏳ 更新節點選取狀態 (UI)
- [ ] ⏳ 測試節點選取

**驗收標準**:
- 節點選取正常
- 選取狀態正確
- UI 回饋正確

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.118 - 實現畫布取消選取 🟡 P1
- [ ] ⏳ 監聽畫布 click 事件
- [ ] ⏳ 清除 selectedNodeId
- [ ] ⏳ 清除節點選取狀態
- [ ] ⏳ 測試取消選取

**驗收標準**:
- 畫布點擊取消選取
- 狀態正確清除
- UI 更新正確

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.119 - 實現節點拖曳移動 🔴 P0
- [ ] ⏳ 監聽 `node-drag-stop` 事件
- [ ] ⏳ 更新節點位置 (Store)
- [ ] ⏳ 同步 VueFlow 狀態
- [ ] ⏳ 測試節點移動

**驗收標準**:
- 節點拖曳移動正常
- 位置更新正確
- Store 同步正確

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.120 - 實現連線建立 🔴 P0
- [ ] ⏳ 監聽 `connect` 事件
- [ ] ⏳ 建立 edge 物件
- [ ] ⏳ 加入 edge 到 Store
- [ ] ⏳ 驗證連線合法性
- [ ] ⏳ 測試連線建立

**驗收標準**:
- 連線建立正常
- Edge 物件正確
- 驗證邏輯正確

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.121 - 實現連線更新 🟡 P1
- [ ] ⏳ 監聽 `edge-update` 事件
- [ ] ⏳ 移除舊連線
- [ ] ⏳ 建立新連線
- [ ] ⏳ 測試連線更新

**驗收標準**:
- 連線更新正常
- 舊連線正確移除
- 新連線正確建立

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.122 - 實現刪除功能 🔴 P0
- [ ] ⏳ 實現 `deleteSelection()` 函數
- [ ] ⏳ 取得選取的節點和連線
- [ ] ⏳ 從 Store 移除節點
- [ ] ⏳ 從 Store 移除連線
- [ ] ⏳ 處理級聯刪除 (節點刪除時移除相關連線)
- [ ] ⏳ 測試刪除功能

**驗收標準**:
- 刪除功能正常
- 級聯刪除正確
- Store 更新正確

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.123 - 實現鍵盤快捷鍵 🟡 P1
- [ ] ⏳ 實現 Delete/Backspace 刪除
- [ ] ⏳ 實現 Ctrl+S 儲存
- [ ] ⏳ 實現 Ctrl+Z 復原 (placeholder)
- [ ] ⏳ 實現 Ctrl+Y 重做 (placeholder)
- [ ] ⏳ 測試快捷鍵

**驗收標準**:
- Delete 刪除正常
- Ctrl+S 儲存正常
- Undo/Redo placeholder 建立
- 無快捷鍵衝突

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.124 - 實現多選功能 🟢 P2
- [ ] ⏳ 實現框選 (box selection)
- [ ] ⏳ 實現 Ctrl+Click 多選
- [ ] ⏳ 實現選取狀態管理
- [ ] ⏳ 測試多選功能

**驗收標準**:
- 框選功能正常
- Ctrl+Click 多選正常
- 選取狀態正確

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.125 - 測試 Canvas Interaction 🔴 P0
- [ ] ⏳ 測試節點選取
- [ ] ⏳ 測試節點移動
- [ ] ⏳ 測試連線建立
- [ ] ⏳ 測試刪除功能
- [ ] ⏳ 測試鍵盤快捷鍵
- [ ] ⏳ 記錄問題

**驗收標準**:
- 所有互動測試通過
- 無阻礙性問題
- 問題清單建立

**負責人**: Frontend Team
**預估時間**: 2 hours

---

### Phase 2 檢查點 (Phase 2 Checkpoint)

#### T12.126 - Phase 2 整合測試 🔴 P0
- [ ] ⏳ 測試 NodePalette 顯示
- [ ] ⏳ 測試拖曳節點到畫布
- [ ] ⏳ 測試節點選取和移動
- [ ] ⏳ 測試連線建立
- [ ] ⏳ 測試刪除功能
- [ ] ⏳ 測試鍵盤快捷鍵
- [ ] ⏳ 記錄所有問題

**驗收標準**:
- 所有整合測試通過
- 工作流建立流程順暢
- 無阻礙性問題

**負責人**: Frontend Team
**預估時間**: 2.5 hours

---

#### T12.127 - Phase 2 Code Review 🔴 P0
- [ ] ⏳ Review Drag & Drop 程式碼
- [ ] ⏳ Review Canvas Interaction 程式碼
- [ ] ⏳ 檢查事件處理
- [ ] ⏳ 檢查狀態管理
- [ ] ⏳ 提出改進建議
- [ ] ⏳ 修正 review 問題

**驗收標準**:
- Code review 完成
- 所有問題修正
- 程式碼品質符合標準

**負責人**: Tech Lead, Frontend Lead
**預估時間**: 2 hours

---

## Phase 3: 基礎節點類型

**時程**: Week 36, Day 1-3 (2026-06-16 ~ 2026-06-18)
**Story Points**: 2 SP
**任務數量**: 36 tasks

### 3.1 節點元件開發 (Node Component Development)

#### T12.201 - 建立 AgentNode 元件 🔴 P0
- [ ] ⏳ 建立 `src/components/nodes/AgentNode.vue`
- [ ] ⏳ 定義 props interface
- [ ] ⏳ 實現 template 結構
- [ ] ⏳ 實現節點樣式
- [ ] ⏳ 測試元件渲染

**驗收標準**:
- 元件檔案建立
- Props 正確定義
- 樣式符合設計

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.202 - 實現 AgentNode 視覺設計 🔴 P0
- [ ] ⏳ 實現 header (藍色背景, icon, title, menu)
- [ ] ⏳ 實現 body (agent info, input)
- [ ] ⏳ 實現 error 顯示
- [ ] ⏳ 實現 handles (target, source)
- [ ] ⏳ 調整樣式細節

**驗收標準**:
- 視覺設計符合規格
- Header 和 body 正確顯示
- Handles 位置正確

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.203 - 實現 AgentNode 互動 🟡 P1
- [ ] ⏳ 實現選取狀態
- [ ] ⏳ 實現 hover 效果
- [ ] ⏳ 實現右鍵選單 (edit, duplicate, delete)
- [ ] ⏳ 測試互動功能

**驗收標準**:
- 選取狀態正確
- Hover 效果流暢
- 右鍵選單功能正常

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.204 - 建立 PluginNode 元件 🔴 P0
- [ ] ⏳ 建立 `src/components/nodes/PluginNode.vue`
- [ ] ⏳ 定義 props interface
- [ ] ⏳ 實現 template 結構 (類似 AgentNode)
- [ ] ⏳ 實現節點樣式 (綠色主題)
- [ ] ⏳ 測試元件渲染

**驗收標準**:
- 元件檔案建立
- 視覺與 AgentNode 區隔 (綠色)
- 樣式符合設計

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.205 - 實現 PluginNode 視覺設計 🔴 P0
- [ ] ⏳ 實現 header (綠色背景)
- [ ] ⏳ 實現 body (plugin info, parameters count)
- [ ] ⏳ 實現 handles
- [ ] ⏳ 調整樣式細節

**驗收標準**:
- 視覺設計符合規格
- 與 AgentNode 保持一致性
- Handles 位置正確

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.206 - 實現 PluginNode 互動 🟡 P1
- [ ] ⏳ 實現選取狀態
- [ ] ⏳ 實現 hover 效果
- [ ] ⏳ 實現右鍵選單
- [ ] ⏳ 測試互動功能

**驗收標準**:
- 互動功能與 AgentNode 一致
- 選取和 hover 正常
- 右鍵選單功能正常

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.207 - 建立 DecisionNode 元件 🔴 P0
- [ ] ⏳ 建立 `src/components/nodes/DecisionNode.vue`
- [ ] ⏳ 定義 props interface
- [ ] ⏳ 實現 template 結構 (菱形設計)
- [ ] ⏳ 實現節點樣式 (橙色主題)
- [ ] ⏳ 測試元件渲染

**驗收標準**:
- 元件檔案建立
- 菱形視覺風格
- 樣式符合設計

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.208 - 實現 DecisionNode 雙輸出 🔴 P0
- [ ] ⏳ 實現 target handle (左側)
- [ ] ⏳ 實現 true source handle (下方左側)
- [ ] ⏳ 實現 false source handle (下方右側)
- [ ] ⏳ 實現 handle 標籤 (True/False)
- [ ] ⏳ 測試 handle 連接

**驗收標準**:
- 三個 handles 位置正確
- True/False 標籤顯示
- 連接功能正常

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.209 - 實現 DecisionNode 條件顯示 🟡 P1
- [ ] ⏳ 顯示條件文字
- [ ] ⏳ 實現文字截斷 (truncate)
- [ ] ⏳ 實現 tooltip (完整條件)
- [ ] ⏳ 測試條件顯示

**驗收標準**:
- 條件正確顯示
- 長文字正確截斷
- Tooltip 顯示完整條件

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.210 - 建立 StartEndNode 元件 🔴 P0
- [ ] ⏳ 建立 `src/components/nodes/StartEndNode.vue`
- [ ] ⏳ 定義 props interface (type: 'start' | 'end')
- [ ] ⏳ 實現 template 結構 (圓形設計)
- [ ] ⏳ 實現節點樣式 (Start 綠色, End 紅色)
- [ ] ⏳ 測試元件渲染

**驗收標準**:
- 元件檔案建立
- Start/End 視覺區隔
- 圓形樣式正確

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.211 - 實現 StartEndNode Handle 配置 🔴 P0
- [ ] ⏳ Start 節點: 僅 source handle (右側)
- [ ] ⏳ End 節點: 僅 target handle (左側)
- [ ] ⏳ 實現 handle 樣式
- [ ] ⏳ 測試 handle 連接

**驗收標準**:
- Start 節點僅可作為起點
- End 節點僅可作為終點
- Handle 連接正常

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.212 - 實現節點不可刪除邏輯 🟡 P1
- [ ] ⏳ 檢查工作流至少有一個 Start
- [ ] ⏳ 檢查工作流至少有一個 End
- [ ] ⏳ 防止刪除最後一個 Start/End
- [ ] ⏳ 顯示錯誤訊息
- [ ] ⏳ 測試刪除限制

**驗收標準**:
- 無法刪除最後一個 Start/End
- 錯誤訊息正確顯示
- 可刪除多餘的 Start/End

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

### 3.2 節點註冊與配置 (Node Registration & Configuration)

#### T12.213 - 註冊自定義節點到 VueFlow 🔴 P0
- [ ] ⏳ 在 WorkflowEditor 註冊 AgentNode
- [ ] ⏳ 在 WorkflowEditor 註冊 PluginNode
- [ ] ⏳ 在 WorkflowEditor 註冊 DecisionNode
- [ ] ⏳ 在 WorkflowEditor 註冊 StartEndNode
- [ ] ⏳ 測試節點類型渲染

**驗收標準**:
- 所有節點類型正確註冊
- 節點類型自動渲染
- 無註冊錯誤

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.214 - 更新 Node Store 節點定義 🔴 P0
- [ ] ⏳ 定義 Agent node type
- [ ] ⏳ 定義 Plugin node type
- [ ] ⏳ 定義 Decision node type
- [ ] ⏳ 定義 Start node type
- [ ] ⏳ 定義 End node type
- [ ] ⏳ 設定圖示和分類

**驗收標準**:
- 所有節點類型定義完整
- 圖示和分類正確
- NodePalette 正確顯示

**負責人**: Frontend Developer
**預估時間**: 1 hour

---

#### T12.215 - 實現節點預設配置 🟡 P1
- [ ] ⏳ Agent node 預設配置
- [ ] ⏳ Plugin node 預設配置
- [ ] ⏳ Decision node 預設配置
- [ ] ⏳ Start node 預設配置
- [ ] ⏳ End node 預設配置
- [ ] ⏳ 測試預設配置

**驗收標準**:
- 預設配置合理
- 新建節點使用預設配置
- 配置可覆寫

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.216 - 實現節點驗證規則 🟡 P1
- [ ] ⏳ Agent node 驗證 (必須有 agentId)
- [ ] ⏳ Plugin node 驗證 (必須有 pluginId)
- [ ] ⏳ Decision node 驗證 (必須有 condition)
- [ ] ⏳ Start/End node 驗證
- [ ] ⏳ 測試驗證規則

**驗收標準**:
- 驗證規則正確
- 無效節點標記錯誤
- 錯誤訊息清晰

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

### 3.3 節點測試 (Node Testing)

#### T12.217 - 測試 AgentNode 🔴 P0
- [ ] ⏳ 測試節點渲染
- [ ] ⏳ 測試 props 顯示
- [ ] ⏳ 測試選取狀態
- [ ] ⏳ 測試 hover 效果
- [ ] ⏳ 測試右鍵選單
- [ ] ⏳ 測試 handles 連接
- [ ] ⏳ 記錄問題

**驗收標準**:
- 所有測試通過
- 視覺效果正確
- 互動流暢

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

#### T12.218 - 測試 PluginNode 🔴 P0
- [ ] ⏳ 測試節點渲染
- [ ] ⏳ 測試 props 顯示
- [ ] ⏳ 測試視覺區隔 (綠色)
- [ ] ⏳ 測試互動功能
- [ ] ⏳ 記錄問題

**驗收標準**:
- 所有測試通過
- 與 AgentNode 保持一致性
- 視覺區隔明確

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

#### T12.219 - 測試 DecisionNode 🔴 P0
- [ ] ⏳ 測試節點渲染
- [ ] ⏳ 測試菱形樣式
- [ ] ⏳ 測試雙輸出 handles
- [ ] ⏳ 測試條件顯示
- [ ] ⏳ 測試 True/False 連接
- [ ] ⏳ 記錄問題

**驗收標準**:
- 菱形樣式正確
- 雙輸出功能正常
- 條件顯示正確

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

#### T12.220 - 測試 StartEndNode 🔴 P0
- [ ] ⏳ 測試 Start 節點渲染
- [ ] ⏳ 測試 End 節點渲染
- [ ] ⏳ 測試圓形樣式
- [ ] ⏳ 測試 handle 配置
- [ ] ⏳ 測試刪除限制
- [ ] ⏳ 記錄問題

**驗收標準**:
- Start/End 視覺區隔
- Handle 配置正確
- 刪除限制正常

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

#### T12.221 - 整合測試所有節點類型 🔴 P0
- [ ] ⏳ 建立包含所有節點類型的工作流
- [ ] ⏳ 測試節點建立
- [ ] ⏳ 測試節點連接
- [ ] ⏳ 測試節點移動
- [ ] ⏳ 測試節點刪除
- [ ] ⏳ 測試節點驗證
- [ ] ⏳ 記錄所有問題

**驗收標準**:
- 所有節點類型協同運作
- 工作流建立流暢
- 無阻礙性問題

**負責人**: Frontend Team
**預估時間**: 2 hours

---

### Phase 3 檢查點 (Phase 3 Checkpoint)

#### T12.222 - Phase 3 Code Review 🔴 P0
- [ ] ⏳ Review 所有節點元件
- [ ] ⏳ 檢查程式碼品質
- [ ] ⏳ 檢查樣式一致性
- [ ] ⏳ 檢查型別定義
- [ ] ⏳ 提出改進建議
- [ ] ⏳ 修正 review 問題

**驗收標準**:
- Code review 完成
- 所有問題修正
- 程式碼品質符合標準

**負責人**: Tech Lead, Frontend Lead
**預估時間**: 2 hours

---

## Phase 4: Testing & Documentation

**時程**: Week 36, Day 4-5 (2026-06-19 ~ 2026-06-20)
**Story Points**: N/A (QA & Docs)
**任務數量**: 35 tasks

### 4.1 單元測試 (Unit Testing)

#### T12.301 - 配置 Vitest 🔴 P0
- [ ] ⏳ 安裝 Vitest (`npm install -D vitest@^1.0.0`)
- [ ] ⏳ 安裝 Vue Test Utils (`npm install -D @vue/test-utils@^2.4.0`)
- [ ] ⏳ 建立 `vitest.config.ts`
- [ ] ⏳ 配置測試環境
- [ ] ⏳ 設定 coverage 配置

**驗收標準**:
- Vitest 正確配置
- Vue Test Utils 可用
- Coverage 設定正確

**負責人**: Frontend Lead
**預估時間**: 1.5 hours

---

#### T12.302 - 撰寫 Store 單元測試 🔴 P0
- [ ] ⏳ 測試 Workflow Store actions
- [ ] ⏳ 測試 Workflow Store getters
- [ ] ⏳ 測試 Node Store methods
- [ ] ⏳ 測試 state mutations
- [ ] ⏳ 達成 80%+ coverage

**驗收標準**:
- 所有 store 測試通過
- Coverage ≥ 80%
- 邊界情況測試

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.303 - 撰寫 Composable 單元測試 🔴 P0
- [ ] ⏳ 測試 useVueFlow composable
- [ ] ⏳ 測試 useDragDrop composable
- [ ] ⏳ 測試 useCanvasInteraction composable
- [ ] ⏳ 達成 80%+ coverage

**驗收標準**:
- 所有 composable 測試通過
- Coverage ≥ 80%
- Mock 使用正確

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.304 - 撰寫元件單元測試 🟡 P1
- [ ] ⏳ 測試 NodePalette 元件
- [ ] ⏳ 測試 AgentNode 元件
- [ ] ⏳ 測試 PluginNode 元件
- [ ] ⏳ 測試 DecisionNode 元件
- [ ] ⏳ 測試 StartEndNode 元件
- [ ] ⏳ 達成 70%+ coverage

**驗收標準**:
- 所有元件測試通過
- Coverage ≥ 70%
- 快照測試 (snapshot)

**負責人**: Frontend Developer
**預估時間**: 4 hours

---

#### T12.305 - 撰寫 API 單元測試 🟡 P1
- [ ] ⏳ 測試 API client
- [ ] ⏳ 測試 workflow API methods
- [ ] ⏳ 測試資料轉換器
- [ ] ⏳ 測試錯誤處理
- [ ] ⏳ 達成 80%+ coverage

**驗收標準**:
- 所有 API 測試通過
- Coverage ≥ 80%
- Mock API responses

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.306 - 執行測試覆蓋率報告 🔴 P0
- [ ] ⏳ 執行所有測試
- [ ] ⏳ 生成 coverage report
- [ ] ⏳ 檢查 coverage 達標 (總體 ≥ 75%)
- [ ] ⏳ 標記低 coverage 區域
- [ ] ⏳ 補充測試 (如需要)

**驗收標準**:
- 總體 coverage ≥ 75%
- Store/Composable coverage ≥ 80%
- 報告正確生成

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

### 4.2 E2E 測試 (E2E Testing)

#### T12.307 - 配置 Playwright 🟡 P1
- [ ] ⏳ 安裝 Playwright (`npm install -D @playwright/test@^1.40.0`)
- [ ] ⏳ 建立 `playwright.config.ts`
- [ ] ⏳ 配置 browsers (Chromium, Firefox, WebKit)
- [ ] ⏳ 設定測試 base URL

**驗收標準**:
- Playwright 正確配置
- 多瀏覽器設定
- Base URL 正確

**負責人**: Frontend Lead
**預估時間**: 1.5 hours

---

#### T12.308 - 撰寫工作流建立 E2E 測試 🟡 P1
- [ ] ⏳ 測試載入編輯器
- [ ] ⏳ 測試拖曳節點到畫布
- [ ] ⏳ 測試建立連線
- [ ] ⏳ 測試儲存工作流
- [ ] ⏳ 驗證結果

**驗收標準**:
- E2E 測試通過
- 涵蓋主要流程
- 跨瀏覽器測試

**負責人**: Frontend Developer
**預估時間**: 3 hours

---

#### T12.309 - 撰寫工作流編輯 E2E 測試 🟢 P2
- [ ] ⏳ 測試載入現有工作流
- [ ] ⏳ 測試移動節點
- [ ] ⏳ 測試刪除節點
- [ ] ⏳ 測試更新連線
- [ ] ⏳ 測試儲存變更

**驗收標準**:
- E2E 測試通過
- 編輯流程完整
- 跨瀏覽器測試

**負責人**: Frontend Developer
**預估時間**: 2.5 hours

---

#### T12.310 - 執行 E2E 測試報告 🟡 P1
- [ ] ⏳ 執行所有 E2E 測試
- [ ] ⏳ 生成測試報告
- [ ] ⏳ 記錄失敗測試
- [ ] ⏳ 修正失敗測試

**驗收標準**:
- 所有 E2E 測試通過
- 報告正確生成
- 無 flaky tests

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

### 4.3 文檔撰寫 (Documentation)

#### T12.311 - 撰寫元件文檔 🟡 P1
- [ ] ⏳ 文檔化 WorkflowEditor 元件
- [ ] ⏳ 文檔化 NodePalette 元件
- [ ] ⏳ 文檔化所有節點元件
- [ ] ⏳ 包含 props, events, slots 說明
- [ ] ⏳ 包含使用範例

**驗收標準**:
- 所有元件文檔完整
- Props/Events 說明清晰
- 範例可執行

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.312 - 撰寫 API 文檔 🟡 P1
- [ ] ⏳ 文檔化 workflow API
- [ ] ⏳ 文檔化資料轉換器
- [ ] ⏳ 包含 TypeScript 型別定義
- [ ] ⏳ 包含使用範例

**驗收標準**:
- API 文檔完整
- 型別定義清晰
- 範例可執行

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.313 - 撰寫 Store 文檔 🟡 P1
- [ ] ⏳ 文檔化 Workflow Store
- [ ] ⏳ 文檔化 Node Store
- [ ] ⏳ 包含 state, getters, actions 說明
- [ ] ⏳ 包含使用範例

**驗收標準**:
- Store 文檔完整
- State/Actions 說明清晰
- 範例可執行

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.314 - 撰寫 Composable 文檔 🟡 P1
- [ ] ⏳ 文檔化 useVueFlow
- [ ] ⏳ 文檔化 useDragDrop
- [ ] ⏳ 文檔化 useCanvasInteraction
- [ ] ⏳ 包含參數和返回值說明
- [ ] ⏳ 包含使用範例

**驗收標準**:
- Composable 文檔完整
- 參數/返回值說明清晰
- 範例可執行

**負責人**: Frontend Developer
**預估時間**: 1.5 hours

---

#### T12.315 - 更新 README.md 🔴 P0
- [ ] ⏳ 更新專案描述
- [ ] ⏳ 更新安裝步驟
- [ ] ⏳ 更新使用指南
- [ ] ⏳ 更新開發指南
- [ ] ⏳ 更新 troubleshooting

**驗收標準**:
- README 完整更新
- 安裝步驟正確
- 使用指南清晰

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

#### T12.316 - 建立 CHANGELOG.md 🟢 P2
- [ ] ⏳ 建立 CHANGELOG.md
- [ ] ⏳ 記錄 Sprint 12 變更
- [ ] ⏳ 記錄新增功能
- [ ] ⏳ 記錄已知問題

**驗收標準**:
- CHANGELOG 建立
- 變更記錄完整
- 格式符合 Keep a Changelog

**負責人**: Frontend Lead
**預估時間**: 30 minutes

---

### 4.4 效能優化 (Performance Optimization)

#### T12.317 - 分析 Bundle Size 🟡 P1
- [ ] ⏳ 執行 production build
- [ ] ⏳ 分析 bundle size
- [ ] ⏳ 識別大型依賴
- [ ] ⏳ 優化 bundle size (如需要)

**驗收標準**:
- Bundle size 分析完成
- 識別優化機會
- Bundle size < 500KB (gzipped)

**負責人**: Frontend Lead
**預估時間**: 1.5 hours

---

#### T12.318 - 優化載入效能 🟢 P2
- [ ] ⏳ 實現 code splitting
- [ ] ⏳ 實現 lazy loading
- [ ] ⏳ 優化 asset loading
- [ ] ⏳ 測試載入時間

**驗收標準**:
- Code splitting 實現
- Lazy loading 正常
- 載入時間 < 2s

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

#### T12.319 - 優化渲染效能 🟢 P2
- [ ] ⏳ 分析渲染效能
- [ ] ⏳ 優化節點渲染
- [ ] ⏳ 優化連線渲染
- [ ] ⏳ 測試大型工作流 (100+ 節點)

**驗收標準**:
- 渲染效能分析完成
- 優化實施
- 100 節點工作流流暢

**負責人**: Frontend Developer
**預估時間**: 2 hours

---

### 4.5 部署準備 (Deployment Preparation)

#### T12.320 - 配置 Production Build 🔴 P0
- [ ] ⏳ 設定 production 環境變數
- [ ] ⏳ 配置 build optimization
- [ ] ⏳ 執行 production build
- [ ] ⏳ 驗證 build 結果

**驗收標準**:
- Production build 成功
- 環境變數正確
- Build artifacts 完整

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

#### T12.321 - 測試 Production Build 🔴 P0
- [ ] ⏳ 啟動 preview server
- [ ] ⏳ 測試所有功能
- [ ] ⏳ 測試 Module Federation 載入
- [ ] ⏳ 檢查 console 錯誤
- [ ] ⏳ 記錄問題

**驗收標準**:
- Production build 功能正常
- 無 console 錯誤
- Module Federation 正常

**負責人**: Frontend Team
**預估時間**: 1.5 hours

---

#### T12.322 - 建立部署文檔 🟡 P1
- [ ] ⏳ 文檔化部署流程
- [ ] ⏳ 文檔化環境設定
- [ ] ⏳ 文檔化 troubleshooting
- [ ] ⏳ 建立部署 checklist

**驗收標準**:
- 部署文檔完整
- 流程清晰
- Checklist 可用

**負責人**: Frontend Lead
**預估時間**: 1.5 hours

---

### Phase 4 檢查點 (Phase 4 Checkpoint)

#### T12.323 - 最終整合測試 🔴 P0
- [ ] ⏳ 執行所有單元測試
- [ ] ⏳ 執行所有 E2E 測試
- [ ] ⏳ 測試 production build
- [ ] ⏳ 測試 Module Federation 整合
- [ ] ⏳ 記錄所有問題

**驗收標準**:
- 所有測試通過
- Production build 正常
- 整合測試通過

**負責人**: Frontend Team
**預估時間**: 2 hours

---

#### T12.324 - Sprint 12 最終 Code Review 🔴 P0
- [ ] ⏳ Review 所有程式碼
- [ ] ⏳ 檢查測試覆蓋率
- [ ] ⏳ 檢查文檔完整性
- [ ] ⏳ 提出最終改進建議
- [ ] ⏳ 修正所有問題

**驗收標準**:
- 最終 code review 完成
- 所有問題修正
- 程式碼品質優良

**負責人**: Tech Lead, Architect, Frontend Lead
**預估時間**: 2.5 hours

---

## Phase 1D 啟動檢查清單

### Phase 1D 啟動驗收 (Phase 1D Launch Acceptance)

#### T12.401 - Phase 1C → Phase 1D 轉換驗證 🔴 P0
- [ ] ⏳ 驗證 Phase 1C 完成狀態
  - [ ] Sprint 10 完成率 100%
  - [ ] Sprint 11 完成率 100%
  - [ ] Workflow API 全部測試通過
  - [ ] Execution Engine 驗證通過
- [ ] ⏳ 驗證 Backend API 可用性
  - [ ] Workflow CRUD APIs 正常
  - [ ] Validation API 正常
  - [ ] Execute API 正常
- [ ] ⏳ 確認 API 文檔完整
- [ ] ⏳ 確認測試環境就緒
- [ ] ⏳ 確認開發環境就緒

**驗收標準**:
- Phase 1C 100% 完成
- 所有 Backend APIs 可用
- 環境準備完成

**負責人**: Tech Lead, PM
**預估時間**: 1 hour

---

#### T12.402 - Phase 1D 技術棧驗證 🔴 P0
- [ ] ⏳ 驗證 Vue 3 環境
  - [ ] Node.js 18+ 安裝
  - [ ] npm 9+ 安裝
  - [ ] Vue CLI 可用
- [ ] ⏳ 驗證 VueFlow 可用性
  - [ ] VueFlow 1.45.0 可安裝
  - [ ] 範例專案可運行
- [ ] ⏳ 驗證 Module Federation 可用性
  - [ ] Vite plugin 可用
  - [ ] React Host 準備就緒
- [ ] ⏳ 驗證開發工具
  - [ ] VS Code 配置正確
  - [ ] 擴展安裝完成

**驗收標準**:
- 所有技術棧可用
- 開發環境完整
- 工具配置正確

**負責人**: Frontend Lead
**預估時間**: 1 hour

---

#### T12.403 - Phase 1D 團隊準備 🟡 P1
- [ ] ⏳ Frontend 團隊技能評估
  - [ ] Vue 3 Composition API 熟悉度
  - [ ] TypeScript 熟悉度
  - [ ] VueFlow 學習計劃
- [ ] ⏳ 團隊角色分配
  - [ ] Frontend Lead 指定
  - [ ] Frontend Developers 分配
  - [ ] QA Engineer 協調
- [ ] ⏳ 培訓計劃
  - [ ] VueFlow 培訓 (2 hours)
  - [ ] Module Federation 培訓 (1 hour)
  - [ ] Pinia 培訓 (1 hour)

**驗收標準**:
- 團隊技能評估完成
- 角色分配明確
- 培訓計劃執行

**負責人**: PM, Tech Lead
**預估時間**: 2 hours

---

#### T12.404 - Phase 1D 文檔準備 🟡 P1
- [ ] ⏳ 閱讀 ADR-012 (Workflow Editor Technology)
- [ ] ⏳ 閱讀 US-5.3 (Workflow Editor)
- [ ] ⏳ 閱讀 UX 設計文檔
- [ ] ⏳ 閱讀 PoC-06 (VueFlow + CRDT)
- [ ] ⏳ 建立 Phase 1D 開發指南

**驗收標準**:
- 所有文檔閱讀完成
- 開發指南建立
- 團隊對需求理解一致

**負責人**: Frontend Team
**預估時間**: 2 hours

---

#### T12.405 - Phase 1D 里程碑設定 🔴 P0
- [ ] ⏳ 確認 Sprint 12-14 時程
  - [ ] Sprint 12: VueFlow 畫布基礎 (Week 34-36)
  - [ ] Sprint 13: Connection & Validation (Week 37-39)
  - [ ] Sprint 14: State Management & Persistence (Week 40-42)
- [ ] ⏳ 設定 Phase 1D 交付物
  - [ ] 工作流編輯器 Frontend 完成
  - [ ] Module Federation 整合完成
  - [ ] E2E 測試完成
- [ ] ⏳ 設定 Phase 1D 驗收標準
  - [ ] 功能完整性 100%
  - [ ] 測試覆蓋率 ≥ 75%
  - [ ] 效能指標達標

**驗收標準**:
- 時程確認
- 交付物明確
- 驗收標準清晰

**負責人**: PM, Tech Lead
**預估時間**: 1 hour

---

## 進度追蹤

### 總體進度

| Phase | 任務數 | 完成數 | 進度 | 狀態 |
|-------|--------|--------|------|------|
| Phase 1: VueFlow 集成 | 52 | 0 | 0% | ⏳ 待開始 |
| Phase 2: Drag-drop Editor | 42 | 0 | 0% | ⏳ 待開始 |
| Phase 3: 基礎節點類型 | 36 | 0 | 0% | ⏳ 待開始 |
| Phase 4: Testing & Docs | 35 | 0 | 0% | ⏳ 待開始 |
| Phase 1D 啟動 | 5 | 0 | 0% | ⏳ 待開始 |
| **總計** | **170** | **0** | **0%** | ⏳ 待開始 |

### 每週進度追蹤

#### Week 34 (2026-06-02 ~ 2026-06-06)

**目標**: Phase 1 完成 (VueFlow 集成)

| 日期 | 計劃任務 | 完成任務 | 進度 | 阻礙 |
|------|----------|----------|------|------|
| Day 1 (Mon) | T12.001-T12.010 | - | 0% | - |
| Day 2 (Tue) | T12.011-T12.020 | - | 0% | - |
| Day 3 (Wed) | T12.021-T12.030 | - | 0% | - |
| Day 4 (Thu) | T12.031-T12.037 | - | 0% | - |
| Day 5 (Fri) | T12.038-T12.040 | - | 0% | - |

**Week 34 目標完成率**: 0%

---

#### Week 35 (2026-06-09 ~ 2026-06-13)

**目標**: Phase 2 完成 (Drag-drop Node Editor)

| 日期 | 計劃任務 | 完成任務 | 進度 | 阻礙 |
|------|----------|----------|------|------|
| Day 1 (Mon) | T12.101-T12.110 | - | 0% | - |
| Day 2 (Tue) | T12.111-T12.118 | - | 0% | - |
| Day 3 (Wed) | T12.119-T12.124 | - | 0% | - |
| Day 4 (Thu) | T12.125-T12.126 | - | 0% | - |
| Day 5 (Fri) | T12.127 | - | 0% | - |

**Week 35 目標完成率**: 0%

---

#### Week 36 (2026-06-16 ~ 2026-06-20)

**目標**: Phase 3 & Phase 4 完成

| 日期 | 計劃任務 | 完成任務 | 進度 | 阻礙 |
|------|----------|----------|------|------|
| Day 1 (Mon) | T12.201-T12.210 | - | 0% | - |
| Day 2 (Tue) | T12.211-T12.220 | - | 0% | - |
| Day 3 (Wed) | T12.221-T12.222 | - | 0% | - |
| Day 4 (Thu) | T12.301-T12.315 | - | 0% | - |
| Day 5 (Fri) | T12.316-T12.324 | - | 0% | - |

**Week 36 目標完成率**: 0%

---

### 風險與阻礙追蹤

| ID | 風險描述 | 影響 | 機率 | 緩解措施 | 負責人 | 狀態 |
|----|----------|------|------|----------|--------|------|
| R12-001 | VueFlow 學習曲線陡峭 | 🟡 中 | 🟡 中 | 提前培訓、建立範例 | Frontend Lead | ⏳ 待處理 |
| R12-002 | Module Federation 整合問題 | 🔴 高 | 🟡 中 | PoC 驗證、技術支援 | Tech Lead | ⏳ 待處理 |
| R12-003 | 跨框架 props 傳遞複雜 | 🟡 中 | 🟡 中 | 明確定義 interface | Frontend Lead | ⏳ 待處理 |
| R12-004 | 效能問題 (大型工作流) | 🟡 中 | 🟢 低 | 效能測試、優化 | Frontend Developer | ⏳ 待處理 |
| R12-005 | 測試覆蓋率不足 | 🟡 中 | 🟡 中 | 測試優先、code review | QA Lead | ⏳ 待處理 |

---

### 依賴追蹤

| ID | 依賴項 | 依賴於 | 狀態 | 預計解決 | 負責人 |
|----|--------|--------|------|----------|--------|
| D12-001 | Module Federation 配置 | Host Application 準備 | ⏳ 待開始 | 2026-06-02 | Frontend Lead |
| D12-002 | Workflow API 整合 | Phase 1C 完成 | ⏳ 待開始 | 2026-05-30 | Backend Team |
| D12-003 | UX 設計確認 | UX Designer | ⏳ 待開始 | 2026-06-02 | UX Designer |
| D12-004 | 節點圖示設計 | Design System | ⏳ 待開始 | 2026-06-02 | UX Designer |

---

**文檔維護**:
- 建立日期: 2025-11-14
- 最後更新: 2025-11-14
- 維護者: Frontend Team
- 審核者: Tech Lead, PM

**變更歷史**:
| 日期 | 版本 | 變更內容 | 變更人 |
|------|------|----------|--------|
| 2025-11-14 | v2.1 | 初始建立,Phase 1D 啟動 | Frontend Team |

**下一步行動**:
1. ✅ 完成 SPRINT-12-5-DEV-LOG.md
2. ✅ 完成 SPRINT-12-6-ISSUES.md
3. ✅ 完成 SPRINT-12-7-RETROSPECTIVE.md
4. 🔄 開始 Sprint 12 執行 (2026-06-02)

---

**文檔狀態**: ✅ 完成
