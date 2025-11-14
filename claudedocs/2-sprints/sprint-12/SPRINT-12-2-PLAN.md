# SPRINT 12 - 執行計劃 (Execution Plan)

---

## 📋 文件資訊 (Document Information)

| 欄位 | 內容 |
|------|------|
| **版本** | v2.1 |
| **Sprint 編號** | Sprint 12 |
| **Sprint 週期** | Week 34-36 (3 週) |
| **Phase** | Phase 1D - 工作流編輯器 Frontend (Workflow Editor Frontend) |
| **計劃日期** | 2026-06-02 ~ 2026-06-22 |
| **狀態** | 📋 計劃階段 (Planned) |
| **創建日期** | 2025-11-14 |
| **最後更新** | 2025-11-14 |

---

## 📑 目錄 (Table of Contents)

1. [Sprint 12 Overview](#sprint-12-overview)
2. [Phase 1D 啟動計劃](#phase-1d-啟動計劃)
3. [Feature 1: VueFlow 集成 (3 SP)](#feature-1-vueflow-集成-3-sp)
4. [Feature 2: Drag-drop Node Editor (3 SP)](#feature-2-drag-drop-node-editor-3-sp)
5. [Feature 3: 基礎節點類型 (2 SP)](#feature-3-基礎節點類型-2-sp)
6. [Phase 1D 啟動驗收](#phase-1d-啟動驗收)
7. [Phase 1D 技術準備](#phase-1d-技術準備)

---

## Sprint 12 Overview

### Sprint 目標

**主要目標**: 啟動 Phase 1D Frontend 開發,建立 VueFlow 工作流畫布基礎

**關鍵交付物**:
1. **VueFlow 集成** (3 SP)
   - Module Federation 微前端架構設置
   - VueFlow 基礎配置 (16px grid system)
   - 畫布控制項 (Toolbar, Minimap, ZoomControls)

2. **Drag-drop Node Editor** (3 SP)
   - Node Palette 節點面板
   - HTML5 Drag & Drop 功能
   - Edge Connection 連接線繪製

3. **基礎節點類型** (2 SP)
   - Standard Node (96x96px)
   - Configuration Node (80x80px 圓形)
   - Trigger Node (特殊圓角)
   - 節點狀態系統

**Sprint Points**: 8 SP
**Sprint Duration**: Week 34-36 (3 週, 15 working days)
**Phase**: Phase 1D Kickoff Sprint

---

### Sprint 12 在 Phase 1D 中的定位

**Phase 1D 整體規劃**:
```yaml
Sprint 12 (Week 34-36):
  Status: 🚀 本 Sprint
  Deliverables:
    - VueFlow 集成
    - Drag-drop Node Editor
    - 基礎節點類型
  Story Points: 8 SP
  Phase 1D Progress: 33%

Sprint 13 (Week 37-39):
  Status: ⏳ 計劃中
  Deliverables:
    - 內聯配置系統
    - 表達式編輯器
    - 參數類型系統
  Story Points: 8-9 SP
  Phase 1D Progress: 67%

Sprint 14 (Week 40-42):
  Status: ⏳ 計劃中
  Deliverables:
    - 實時協作 (CRDT)
    - Workflow Template UI
    - Execution Monitoring
  Story Points: 8-9 SP
  Phase 1D Progress: 100%

Phase 1D 總計:
  Duration: 9 weeks
  Story Points: 24-26 SP
  Completion: Sprint 14 結束時達到 100%
```

---

### Sprint 12 關鍵里程碑

| 里程碑 | 目標日期 | 交付物 | 成功標準 |
|--------|----------|--------|----------|
| M12.1: Module Federation 完成 | Week 34 End | React Host + Vue Remote 架構 | 100% 功能實現 + 測試通過 |
| M12.2: VueFlow 畫布完成 | Week 35 Mid | 16px Grid + Controls | 100% 功能實現 + 測試通過 |
| M12.3: Drag-drop 完成 | Week 35 End | Node Palette + Edge Connection | 100% 功能實現 + 測試通過 |
| M12.4: 基礎節點完成 | Week 36 Mid | 3 種節點類型 | 100% 功能實現 + 測試通過 |
| M12.5: Phase 1D Part 1 驗收 | Week 36 End | Sprint 12 100% 交付 | 所有驗收標準通過 |

---

### Sprint 12 成功標準

**功能完整性**:
- ✅ Module Federation 架構正常運作
- ✅ VueFlow 畫布可用 (Grid, Controls)
- ✅ Drag-drop 功能完整 (Node Palette, Edge Connection)
- ✅ 3 種基礎節點類型全部實現

**質量標準**:
- ✅ 代碼覆蓋率 ≥80%
- ✅ 畫布渲染性能 60fps
- ✅ 零 P0/P1 Bugs
- ✅ 所有整合測試通過

**文檔完整性**:
- ✅ Component API 文檔 100% 完整
- ✅ Phase 1D Part 1 交付文檔完成
- ✅ Sprint 13 Handoff Checklist 完成

**Phase 1D 啟動標準**:
- ✅ Vue 3 + VueFlow 技術棧驗證通過
- ✅ Module Federation 架構穩定
- ✅ 無已知 P0/P1 技術債務
- ✅ Sprint 13 準備度 ≥90%

---

## Phase 1D 啟動計劃

### Phase 1D 技術移交接收

**Phase 1C → Phase 1D 移交內容**:

1. **Backend API 文檔**:
   - ✅ Swagger/OpenAPI 3.0 完整文檔
   - ✅ API 使用範例 (TypeScript, cURL)
   - ✅ 錯誤代碼表
   - ✅ 性能基準數據

2. **Database Schema**:
   - ✅ Complete ER diagram
   - ✅ Table definitions (Workflow Definition, Node Registry, Edges)
   - ✅ Sample data for testing

3. **技術決策記錄**:
   - ✅ TD-076 到 TD-087 (12 個技術決策)
   - ✅ ADR-012 (VueFlow 技術選型)
   - ✅ Design rationale

4. **測試報告**:
   - ✅ Integration test report
   - ✅ Performance test report (API P95 <200ms)
   - ✅ API test collection (Postman)

5. **Known Issues & Tech Debt**:
   - ✅ Issue tracking report
   - ✅ Tech debt list (prioritized)
   - ✅ Improvement suggestions

**移交接收時程**:
- Week 33 Day 14: 移交文檔接收完成
- Week 33 Day 15: Phase 1C → Phase 1D Handoff Meeting
- Week 34 Day 1: Phase 1D Sprint 12 Kickoff

---

### Phase 1D 技術棧與架構

**前端技術棧**:
```yaml
核心框架:
  主應用: React 18 + TypeScript 5
  工作流編輯器: Vue 3 + Composition API
  理由:
    - React 主應用已存在 (Module 01-06)
    - Vue 3 專為 Workflow Editor (參考 n8n)
    - Module Federation 整合

畫布引擎:
  核心: VueFlow (@vue-flow/core 1.45.0+)
  擴展:
    - @vue-flow/background (網格背景)
    - @vue-flow/controls (縮放控制)
    - @vue-flow/minimap (小地圖)
  理由:
    - 專為 Vue 3 設計
    - 內置虛擬化渲染
    - n8n 驗證過的成熟方案

狀態管理:
  核心: Pinia (Vue 3)
  理由:
    - Vue 3 官方推薦
    - TypeScript 原生支持
    - 優秀的 DevTools

構建工具:
  核心: Vite 5
  理由:
    - 極快的開發服務器
    - Vue 官方推薦
    - Module Federation 支持

實時協作 (Sprint 14):
  核心: Yjs + y-websocket
  理由:
    - CRDT 算法實現
    - 成熟的協作框架
    - n8n 已整合

UI 組件庫:
  核心: 自研設計系統 (基於 n8n)
  輔助: Element Plus (可選)
  理由:
    - 完全定制化
    - 符合 16px grid system
    - 輕量無依賴
```

**微前端架構 (Module Federation)**:
```yaml
Host Application (React):
  位置: packages/host
  技術: React 18 + TypeScript + Vite
  職責:
    - 主應用容器
    - 路由管理
    - 共享狀態 (Redux Toolkit)
    - Authentication/Authorization

Remote Module (Vue):
  位置: packages/remote
  技術: Vue 3 + VueFlow + Pinia + Vite
  職責:
    - Workflow Editor 完整功能
    - VueFlow 畫布渲染
    - 節點/邊管理
    - 實時協作 (CRDT)

整合策略:
  通訊: Window postMessage API
  狀態: 各自獨立 (Redux vs Pinia)
  樣式: CSS Modules + CSS Variables
  構建: Vite Module Federation Plugin
```

---

## Feature 1: VueFlow 集成 (3 SP)

### Feature Overview

**User Story**: US 7.2 Part 1 - VueFlow 畫布基礎
**Story Points**: 3 SP
**Priority**: P0 (Critical)
**Dependencies**: Phase 1C Backend API (Sprint 10-11)

**Feature Description**:
IT 開發者需要 VueFlow 工作流畫布:
- **Module Federation 架構**: React Host + Vue Remote 微前端設置
- **VueFlow 基礎配置**: 16px grid system, 網格背景
- **畫布控制項**: Toolbar, Minimap, ZoomControls
- **基礎交互**: 縮放、平移、選擇

---

### Phase 1: Module Federation 架構設置 (1 SP)

**Goal**: 建立 React Host + Vue Remote 微前端架構

#### Task Breakdown

**T12.001 - T12.010: Module Federation 設置**

**Host Application 設置** (T12.001 - T12.003):
- [ ] T12.001: React Host 專案初始化
  - 建立 `packages/host` 目錄
  - 初始化 Vite + React + TypeScript
  - 配置 `vite.config.ts` (Module Federation Plugin)

  ```typescript
  // packages/host/vite.config.ts
  import { defineConfig } from 'vite';
  import react from '@vitejs/plugin-react';
  import federation from '@originjs/vite-plugin-federation';

  export default defineConfig({
    plugins: [
      react(),
      federation({
        name: 'host',
        remotes: {
          workflowEditor: 'http://localhost:3001/assets/remoteEntry.js',
        },
        shared: ['react', 'react-dom', 'react-router-dom'],
      }),
    ],
    build: {
      modulePreload: false,
      target: 'esnext',
      minify: false,
      cssCodeSplit: false,
    },
  });
  ```

- [ ] T12.002: Host Application 路由設置
  - 安裝 `react-router-dom`
  - 配置 Workflow Editor 路由
  - 懶加載 Remote Module

  ```typescript
  // packages/host/src/App.tsx
  import { BrowserRouter, Routes, Route } from 'react-router-dom';
  import { lazy, Suspense } from 'react';

  // 懶加載 Vue Remote Module
  const WorkflowEditor = lazy(() => import('workflowEditor/App'));

  function App() {
    return (
      <BrowserRouter>
        <Routes>
          <Route path="/workflows/:id/edit" element={
            <Suspense fallback={<LoadingSpinner />}>
              <WorkflowEditor />
            </Suspense>
          } />
          {/* 其他路由 */}
        </Routes>
      </BrowserRouter>
    );
  }
  ```

- [ ] T12.003: Host ↔ Remote 通訊機制
  - 實現 `postMessage` API wrapper
  - 定義通訊協議 (Workflow CRUD, State Sync)
  - 錯誤處理和重試邏輯

  ```typescript
  // packages/host/src/utils/remoteMessaging.ts
  export interface RemoteMessage {
    type: 'WORKFLOW_LOAD' | 'WORKFLOW_SAVE' | 'STATE_SYNC';
    payload: any;
    requestId: string;
  }

  export class RemoteMessaging {
    private listeners = new Map<string, (data: any) => void>();

    sendToRemote(message: RemoteMessage): Promise<any> {
      return new Promise((resolve, reject) => {
        const requestId = crypto.randomUUID();
        const timeout = setTimeout(() => {
          this.listeners.delete(requestId);
          reject(new Error('Remote call timeout'));
        }, 5000);

        this.listeners.set(requestId, (response) => {
          clearTimeout(timeout);
          this.listeners.delete(requestId);
          resolve(response);
        });

        window.postMessage({
          ...message,
          requestId,
          source: 'host',
        }, '*');
      });
    }

    onRemoteMessage(handler: (message: RemoteMessage) => void) {
      window.addEventListener('message', (event) => {
        if (event.data.source === 'remote') {
          handler(event.data);
        }
      });
    }
  }
  ```

**Remote Module 設置** (T12.004 - T12.006):
- [ ] T12.004: Vue Remote 專案初始化
  - 建立 `packages/remote` 目錄
  - 初始化 Vite + Vue 3 + TypeScript
  - 配置 `vite.config.ts` (Module Federation Plugin)

  ```typescript
  // packages/remote/vite.config.ts
  import { defineConfig } from 'vite';
  import vue from '@vitejs/plugin-vue';
  import federation from '@originjs/vite-plugin-federation';

  export default defineConfig({
    plugins: [
      vue(),
      federation({
        name: 'workflowEditor',
        filename: 'remoteEntry.js',
        exposes: {
          './App': './src/App.vue',
        },
        shared: ['vue', 'pinia', '@vue-flow/core'],
      }),
    ],
    build: {
      modulePreload: false,
      target: 'esnext',
      minify: false,
      cssCodeSplit: false,
    },
    server: {
      port: 3001,
      cors: true,
    },
  });
  ```

- [ ] T12.005: Remote → Host 通訊機制
  - 實現 `postMessage` API wrapper
  - 響應 Host 的 Workflow CRUD 請求
  - 發送狀態更新通知

  ```typescript
  // packages/remote/src/utils/hostMessaging.ts
  export class HostMessaging {
    sendToHost(message: RemoteMessage): void {
      window.parent.postMessage({
        ...message,
        source: 'remote',
      }, '*');
    }

    onHostMessage(handler: (message: RemoteMessage) => void) {
      window.addEventListener('message', (event) => {
        if (event.data.source === 'host') {
          handler(event.data);
        }
      });
    }

    // 響應 Host 請求
    async handleHostRequest(message: RemoteMessage): Promise<void> {
      try {
        let response;
        switch (message.type) {
          case 'WORKFLOW_LOAD':
            response = await this.loadWorkflow(message.payload.workflowId);
            break;
          case 'WORKFLOW_SAVE':
            response = await this.saveWorkflow(message.payload.workflow);
            break;
          default:
            throw new Error(`Unknown message type: ${message.type}`);
        }

        this.sendToHost({
          type: 'RESPONSE',
          payload: response,
          requestId: message.requestId,
        });
      } catch (error) {
        this.sendToHost({
          type: 'ERROR',
          payload: { error: error.message },
          requestId: message.requestId,
        });
      }
    }
  }
  ```

- [ ] T12.006: Remote Module 入口組件
  - 建立 `App.vue` 主組件
  - 初始化 Pinia store
  - 設置 VueFlow provider

  ```vue
  <!-- packages/remote/src/App.vue -->
  <template>
    <div class="workflow-editor">
      <WorkflowCanvas />
    </div>
  </template>

  <script setup lang="ts">
  import { onMounted } from 'vue';
  import { useWorkflowStore } from '@/stores/workflow';
  import { HostMessaging } from '@/utils/hostMessaging';
  import WorkflowCanvas from '@/components/WorkflowCanvas.vue';

  const workflowStore = useWorkflowStore();
  const hostMessaging = new HostMessaging();

  onMounted(() => {
    // 監聽 Host 消息
    hostMessaging.onHostMessage((message) => {
      hostMessaging.handleHostRequest(message);
    });

    // 通知 Host Remote 已準備就緒
    hostMessaging.sendToHost({
      type: 'REMOTE_READY',
      payload: { version: '1.0.0' },
      requestId: crypto.randomUUID(),
    });
  });
  </script>
  ```

**整合測試** (T12.007 - T12.010):
- [ ] T12.007: Host + Remote 並行啟動測試
  - 配置 `package.json` scripts
  - 測試 Module Federation 載入
  - 驗證 Hot Module Replacement (HMR)

  ```json
  // package.json (root)
  {
    "scripts": {
      "dev": "concurrently \"npm run dev:host\" \"npm run dev:remote\"",
      "dev:host": "cd packages/host && npm run dev",
      "dev:remote": "cd packages/remote && npm run dev",
      "build": "npm run build:remote && npm run build:host",
      "build:host": "cd packages/host && npm run build",
      "build:remote": "cd packages/remote && npm run build"
    }
  }
  ```

- [ ] T12.008: Host ↔ Remote 通訊測試
  - 測試 WORKFLOW_LOAD 消息
  - 測試 WORKFLOW_SAVE 消息
  - 測試錯誤處理

- [ ] T12.009: 跨框架狀態同步測試
  - 測試 Redux (Host) → Pinia (Remote)
  - 測試 Pinia (Remote) → Redux (Host)
  - 驗證數據一致性

- [ ] T12.010: 性能基準測試
  - 測試 Remote Module 載入時間 (<2s)
  - 測試 postMessage 延遲 (<50ms)
  - 測試記憶體使用

---

### Phase 2: VueFlow 基礎配置 (1 SP)

**Goal**: 配置 VueFlow 畫布和 16px 網格系統

#### Task Breakdown

**T12.011 - T12.020: VueFlow 配置**

**VueFlow 安裝與配置** (T12.011 - T12.013):
- [ ] T12.011: 安裝 VueFlow 依賴
  - 安裝 `@vue-flow/core` (v1.45.0+)
  - 安裝 `@vue-flow/background`
  - 安裝 `@vue-flow/controls`
  - 安裝 `@vue-flow/minimap`

  ```bash
  cd packages/remote
  npm install @vue-flow/core @vue-flow/background @vue-flow/controls @vue-flow/minimap
  ```

- [ ] T12.012: VueFlow 基礎配置
  - 設置 16px grid system
  - 配置 snap to grid
  - 設置縮放範圍 (0.1 - 4.0)
  - 啟用虛擬化渲染

  ```typescript
  // packages/remote/src/constants/canvas.ts
  export const GRID_SIZE = 16;  // 基礎網格單位（像素）

  export const GRID_MULTIPLIERS = {
    tiny: 1,      // 16px
    small: 2,     // 32px
    medium: 4,    // 64px
    large: 6,     // 96px
    xlarge: 10,   // 160px
    xxlarge: 16,  // 256px
    xxxlarge: 20, // 320px
  };

  export const CANVAS_CONFIG = {
    minZoom: 0.1,
    maxZoom: 4.0,
    snapToGrid: true,
    snapGrid: [GRID_SIZE, GRID_SIZE],
    onlyRenderVisibleElements: true,  // 虛擬化
    deleteKeyCode: 'Delete',
    selectionKeyCode: 'Shift',
    multiSelectionKeyCode: 'Meta',
    zoomActivationKeyCode: 'Meta',
    panOnScroll: true,
    panOnDrag: [1, 2],  // 左鍵 + 中鍵拖拽
    zoomOnScroll: true,
    zoomOnPinch: true,
    zoomOnDoubleClick: false,
  };
  ```

- [ ] T12.013: WorkflowCanvas 主組件
  - 建立 `WorkflowCanvas.vue`
  - 整合 VueFlow
  - 配置 Background, Controls, Minimap

  ```vue
  <!-- packages/remote/src/components/WorkflowCanvas.vue -->
  <template>
    <div class="workflow-canvas-container">
      <VueFlow
        v-model="elements"
        :min-zoom="CANVAS_CONFIG.minZoom"
        :max-zoom="CANVAS_CONFIG.maxZoom"
        :snap-to-grid="CANVAS_CONFIG.snapToGrid"
        :snap-grid="CANVAS_CONFIG.snapGrid"
        :only-render-visible-elements="CANVAS_CONFIG.onlyRenderVisibleElements"
        @nodes-change="onNodesChange"
        @edges-change="onEdgesChange"
        @connect="onConnect"
      >
        <!-- 網格背景 -->
        <Background
          :pattern-color="'var(--color--foreground--tint-2)'"
          :gap="GRID_SIZE * 4"
          :size="1"
        />

        <!-- 縮放控制 -->
        <Controls
          :show-zoom="true"
          :show-fit-view="true"
          :show-interactive="true"
        />

        <!-- 小地圖 -->
        <MiniMap
          :node-color="getNodeColor"
          :position="'bottom-right'"
        />
      </VueFlow>
    </div>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  import { VueFlow, Background, Controls, MiniMap } from '@vue-flow/core';
  import { CANVAS_CONFIG, GRID_SIZE } from '@/constants/canvas';
  import { useWorkflowStore } from '@/stores/workflow';

  const workflowStore = useWorkflowStore();
  const elements = ref([]);

  function onNodesChange(changes) {
    workflowStore.applyNodeChanges(changes);
  }

  function onEdgesChange(changes) {
    workflowStore.applyEdgeChanges(changes);
  }

  function onConnect(params) {
    workflowStore.addEdge(params);
  }

  function getNodeColor(node) {
    // 根據節點狀態返回顏色
    return node.data.status === 'error' ? '#F44336' : '#4CAF50';
  }
  </script>

  <style scoped>
  .workflow-canvas-container {
    width: 100%;
    height: 100vh;
    background-color: var(--color--background);
  }
  </style>
  ```

**網格系統實現** (T12.014 - T12.016):
- [ ] T12.014: Grid Snap 工具函數
  - 實現 `snapToGrid()` 函數
  - 實現 `calculateNodeHeight()` 函數
  - 實現 `calculateNodeWidth()` 函數

  ```typescript
  // packages/remote/src/utils/grid.ts
  import { GRID_SIZE } from '@/constants/canvas';

  /**
   * 將位置對齊到網格
   */
  export function snapToGrid(position: [number, number]): [number, number] {
    return [
      Math.round(position[0] / GRID_SIZE) * GRID_SIZE,
      Math.round(position[1] / GRID_SIZE) * GRID_SIZE
    ];
  }

  /**
   * 計算節點高度（基於端口數量）
   */
  export function calculateNodeHeight(
    baseHeight: number,
    inputCount: number,
    outputCount: number
  ): number {
    const maxPorts = Math.max(inputCount, outputCount, 1);
    // 基礎高度可容納 2 個端口
    // 每增加 1 個端口，增加 2 個網格單位 (32px)
    const additionalHeight = Math.max(0, maxPorts - 2) * GRID_SIZE * 2;
    return baseHeight + additionalHeight;
  }

  /**
   * 計算節點寬度（根據變體）
   */
  export enum NodeWidthVariant {
    COMPACT = 'compact',        // 96px
    CONFIGURATION = 'config',   // 80px
    NORMAL = 'normal',          // 96px
    WIDE = 'wide',              // 320px
  }

  export function calculateNodeWidth(
    variant: NodeWidthVariant,
    isExpanded: boolean = false
  ): number {
    const baseWidths = {
      [NodeWidthVariant.COMPACT]: GRID_SIZE * 6,       // 96px
      [NodeWidthVariant.CONFIGURATION]: GRID_SIZE * 5, // 80px
      [NodeWidthVariant.NORMAL]: GRID_SIZE * 6,        // 96px
      [NodeWidthVariant.WIDE]: GRID_SIZE * 20,         // 320px
    };

    const baseWidth = baseWidths[variant];
    return isExpanded && variant === NodeWidthVariant.WIDE
      ? baseWidth * 1.5  // 480px (展開狀態)
      : baseWidth;
  }
  ```

- [ ] T12.015: 網格背景樣式
  - 實現雙層網格 (主網格 64px, 次網格 16px)
  - 根據縮放級別動態顯示/隱藏

  ```scss
  // packages/remote/src/assets/styles/canvas.scss
  .workflow-canvas {
    background-color: var(--color--background);
    background-image:
      // 主網格線（每 4 個單位）
      linear-gradient(var(--color--foreground--tint-2) 1px, transparent 1px),
      linear-gradient(90deg, var(--color--foreground--tint-2) 1px, transparent 1px),
      // 次網格線（每 1 個單位）
      linear-gradient(var(--color--foreground--tint-3) 1px, transparent 1px),
      linear-gradient(90deg, var(--color--foreground--tint-3) 1px, transparent 1px);
    background-size:
      64px 64px,  // 主網格
      64px 64px,
      16px 16px,  // 次網格
      16px 16px;
    background-position:
      -1px -1px,
      -1px -1px,
      -1px -1px,
      -1px -1px;

    // 縮放 > 1.0: 顯示主網格線和次網格線
    &.zoom-high .vue-flow__background {
      opacity: 1;
    }

    // 縮放 0.5-1.0: 只顯示主網格線
    &.zoom-medium .vue-flow__background {
      opacity: 0.5;
    }

    // 縮放 < 0.5: 隱藏網格線
    &.zoom-low .vue-flow__background {
      opacity: 0.2;
    }
  }
  ```

- [ ] T12.016: 網格對齊驗證
  - 測試節點拖拽對齊
  - 測試節點創建對齊
  - 驗證對齊精度 (±1px 容差)

**Pinia Store 設置** (T12.017 - T12.019):
- [ ] T12.017: Workflow Store
  - 建立 `stores/workflow.ts`
  - 管理 Workflow Definition 狀態
  - 實現 CRUD actions

  ```typescript
  // packages/remote/src/stores/workflow.ts
  import { defineStore } from 'pinia';
  import { ref, computed } from 'vue';
  import type { WorkflowDefinition } from '@/types/workflow';

  export const useWorkflowStore = defineStore('workflow', () => {
    // State
    const currentWorkflow = ref<WorkflowDefinition | null>(null);
    const isLoading = ref(false);
    const error = ref<string | null>(null);

    // Getters
    const workflowId = computed(() => currentWorkflow.value?.id);
    const workflowName = computed(() => currentWorkflow.value?.name);
    const nodes = computed(() => currentWorkflow.value?.nodes || []);
    const edges = computed(() => currentWorkflow.value?.edges || []);

    // Actions
    async function loadWorkflow(id: string) {
      isLoading.value = true;
      error.value = null;
      try {
        const response = await fetch(`/api/v1/workflow-definitions/${id}`);
        if (!response.ok) throw new Error('Failed to load workflow');
        currentWorkflow.value = await response.json();
      } catch (e) {
        error.value = e.message;
        throw e;
      } finally {
        isLoading.value = false;
      }
    }

    async function saveWorkflow() {
      if (!currentWorkflow.value) return;
      isLoading.value = true;
      error.value = null;
      try {
        const response = await fetch(
          `/api/v1/workflow-definitions/${currentWorkflow.value.id}`,
          {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(currentWorkflow.value),
          }
        );
        if (!response.ok) throw new Error('Failed to save workflow');
        currentWorkflow.value = await response.json();
      } catch (e) {
        error.value = e.message;
        throw e;
      } finally {
        isLoading.value = false;
      }
    }

    function applyNodeChanges(changes: any[]) {
      // 應用 VueFlow 節點變更
      // 實現細節略
    }

    function applyEdgeChanges(changes: any[]) {
      // 應用 VueFlow 邊變更
      // 實現細節略
    }

    function addEdge(params: any) {
      // 添加新連接線
      // 實現細節略
    }

    return {
      // State
      currentWorkflow,
      isLoading,
      error,
      // Getters
      workflowId,
      workflowName,
      nodes,
      edges,
      // Actions
      loadWorkflow,
      saveWorkflow,
      applyNodeChanges,
      applyEdgeChanges,
      addEdge,
    };
  });
  ```

- [ ] T12.018: UI Store
  - 管理 UI 狀態 (sidebar, zoom level, selected nodes)

  ```typescript
  // packages/remote/src/stores/ui.ts
  import { defineStore } from 'pinia';
  import { ref } from 'vue';

  export const useUIStore = defineStore('ui', () => {
    const sidebarOpen = ref(true);
    const zoomLevel = ref(1.0);
    const selectedNodes = ref<string[]>([]);
    const selectedEdges = ref<string[]>([]);
    const isDraggingNode = ref(false);
    const isConnectingEdge = ref(false);

    function toggleSidebar() {
      sidebarOpen.value = !sidebarOpen.value;
    }

    function setZoomLevel(level: number) {
      zoomLevel.value = Math.max(0.1, Math.min(4.0, level));
    }

    function selectNode(nodeId: string, addToSelection = false) {
      if (addToSelection) {
        selectedNodes.value.push(nodeId);
      } else {
        selectedNodes.value = [nodeId];
      }
    }

    function clearSelection() {
      selectedNodes.value = [];
      selectedEdges.value = [];
    }

    return {
      sidebarOpen,
      zoomLevel,
      selectedNodes,
      selectedEdges,
      isDraggingNode,
      isConnectingEdge,
      toggleSidebar,
      setZoomLevel,
      selectNode,
      clearSelection,
    };
  });
  ```

- [ ] T12.019: Store Integration 測試
  - 測試 Workflow 載入/保存
  - 測試 Node/Edge 變更
  - 測試 UI 狀態同步

**測試** (T12.020):
- [ ] T12.020: VueFlow 配置測試
  - 測試 Grid snap 功能
  - 測試縮放範圍
  - 測試虛擬化渲染 (50+ nodes)
  - 測試 Controls, Minimap 功能

---

### Phase 3: 畫布控制項 (1 SP)

**Goal**: 實現 Toolbar, Minimap, ZoomControls

#### Task Breakdown

**T12.021 - T12.030: 畫布控制項**

**Toolbar 工具欄** (T12.021 - T12.024):
- [ ] T12.021: CanvasToolbar 組件
  - 建立 `components/CanvasToolbar.vue`
  - 實現工具欄佈局 (頂部固定)
  - 響應式設計

  ```vue
  <!-- packages/remote/src/components/CanvasToolbar.vue -->
  <template>
    <div class="canvas-toolbar">
      <div class="toolbar-left">
        <button @click="handleSave" :disabled="isSaving" class="btn-primary">
          <IconSave :size="16" />
          <span>保存</span>
        </button>
        <button @click="handleUndo" :disabled="!canUndo" class="btn-icon">
          <IconUndo :size="16" />
        </button>
        <button @click="handleRedo" :disabled="!canRedo" class="btn-icon">
          <IconRedo :size="16" />
        </button>
      </div>

      <div class="toolbar-center">
        <span class="workflow-name">{{ workflowName }}</span>
      </div>

      <div class="toolbar-right">
        <button @click="handleZoomIn" class="btn-icon">
          <IconZoomIn :size="16" />
        </button>
        <span class="zoom-level">{{ zoomPercent }}%</span>
        <button @click="handleZoomOut" class="btn-icon">
          <IconZoomOut :size="16" />
        </button>
        <button @click="handleFitView" class="btn-icon">
          <IconMaximize :size="16" />
        </button>
      </div>
    </div>
  </template>

  <script setup lang="ts">
  import { computed } from 'vue';
  import { useVueFlow } from '@vue-flow/core';
  import { useWorkflowStore } from '@/stores/workflow';
  import { useUIStore } from '@/stores/ui';
  import { IconSave, IconUndo, IconRedo, IconZoomIn, IconZoomOut, IconMaximize } from 'lucide-vue-next';

  const { zoomIn, zoomOut, fitView, setViewport } = useVueFlow();
  const workflowStore = useWorkflowStore();
  const uiStore = useUIStore();

  const workflowName = computed(() => workflowStore.workflowName);
  const isSaving = computed(() => workflowStore.isLoading);
  const zoomPercent = computed(() => Math.round(uiStore.zoomLevel * 100));
  const canUndo = computed(() => false); // TODO: 實現 undo/redo
  const canRedo = computed(() => false);

  async function handleSave() {
    await workflowStore.saveWorkflow();
  }

  function handleUndo() {
    // TODO: 實現 undo
  }

  function handleRedo() {
    // TODO: 實現 redo
  }

  function handleZoomIn() {
    zoomIn();
    uiStore.setZoomLevel(uiStore.zoomLevel * 1.2);
  }

  function handleZoomOut() {
    zoomOut();
    uiStore.setZoomLevel(uiStore.zoomLevel / 1.2);
  }

  function handleFitView() {
    fitView({ padding: 0.2 });
  }
  </script>

  <style scoped>
  .canvas-toolbar {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 56px;
    background: var(--color--background--light-2);
    border-bottom: 1px solid var(--color--foreground--tint-2);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 16px;
    z-index: 10;
  }

  .toolbar-left,
  .toolbar-right {
    display: flex;
    gap: 8px;
    align-items: center;
  }

  .workflow-name {
    font-size: 16px;
    font-weight: 600;
    color: var(--color--text);
  }

  .zoom-level {
    min-width: 50px;
    text-align: center;
    font-size: 14px;
    color: var(--color--text--tint-1);
  }
  </style>
  ```

- [ ] T12.022: Zoom Controls 邏輯
  - 實現縮放 (+/-) 功能
  - 實現 Fit View 功能
  - 顯示當前縮放百分比

- [ ] T12.023: Undo/Redo 占位符
  - 按鈕 UI (暫時禁用)
  - TODO: Sprint 13 實現完整 Undo/Redo

- [ ] T12.024: Toolbar 測試
  - 測試保存按鈕
  - 測試縮放控制
  - 測試 Fit View

**MiniMap 小地圖** (T12.025 - T12.027):
- [ ] T12.025: MiniMap 配置
  - 啟用 VueFlow MiniMap
  - 配置位置 (右下角)
  - 設置節點顏色映射

  ```typescript
  // MiniMap 節點顏色函數
  function getNodeColor(node: any): string {
    const statusColors = {
      NEW: '#9E9E9E',
      RUNNING: '#FF6D5A',
      SUCCESS: '#4CAF50',
      ERROR: '#F44336',
      WAITING: '#9C27B0',
      DISABLED: '#E0E0E0',
    };
    return statusColors[node.data.status] || statusColors.NEW;
  }
  ```

- [ ] T12.026: MiniMap 樣式定制
  - 調整大小 (150x100px)
  - 背景顏色
  - 邊框樣式

- [ ] T12.027: MiniMap 測試
  - 測試節點顏色映射
  - 測試點擊導航
  - 測試視口同步

**ZoomControls 縮放控制** (T12.028 - T12.030):
- [ ] T12.028: VueFlow Controls 配置
  - 啟用 Zoom In/Out 按鈕
  - 啟用 Fit View 按鈕
  - 配置位置 (右下角)

- [ ] T12.029: 鍵盤快捷鍵
  - `Ctrl/Cmd + +`: Zoom In
  - `Ctrl/Cmd + -`: Zoom Out
  - `Ctrl/Cmd + 0`: Reset Zoom (100%)
  - `Ctrl/Cmd + 1`: Fit View

  ```typescript
  // packages/remote/src/composables/useKeyboardShortcuts.ts
  import { onMounted, onUnmounted } from 'vue';
  import { useVueFlow } from '@vue-flow/core';

  export function useKeyboardShortcuts() {
    const { zoomIn, zoomOut, fitView, setViewport } = useVueFlow();

    function handleKeyDown(event: KeyboardEvent) {
      const isMeta = event.metaKey || event.ctrlKey;

      if (isMeta && event.key === '=') {
        event.preventDefault();
        zoomIn();
      } else if (isMeta && event.key === '-') {
        event.preventDefault();
        zoomOut();
      } else if (isMeta && event.key === '0') {
        event.preventDefault();
        setViewport({ zoom: 1, x: 0, y: 0 });
      } else if (isMeta && event.key === '1') {
        event.preventDefault();
        fitView({ padding: 0.2 });
      }
    }

    onMounted(() => {
      window.addEventListener('keydown', handleKeyDown);
    });

    onUnmounted(() => {
      window.removeEventListener('keydown', handleKeyDown);
    });
  }
  ```

- [ ] T12.030: ZoomControls 測試
  - 測試按鈕功能
  - 測試鍵盤快捷鍵
  - 測試縮放動畫

---

### Feature 1 驗收標準

**VueFlow 集成完整性**:
- ✅ Module Federation 架構正常運作
- ✅ Host ↔ Remote 通訊穩定 (<50ms 延遲)
- ✅ VueFlow 畫布正常渲染
- ✅ 16px Grid System 正確實現
- ✅ Toolbar, Minimap, ZoomControls 全部可用

**性能目標**:
- ✅ Remote Module 載入時間 <2s
- ✅ 畫布渲染 60fps
- ✅ 虛擬化渲染支持 50+ 節點
- ✅ postMessage 延遲 <50ms

**代碼質量**:
- ✅ TypeScript 類型覆蓋率 100%
- ✅ 單元測試覆蓋率 ≥80%
- ✅ E2E 測試通過 (Playwright)
- ✅ 無 ESLint 錯誤

---

## Feature 2: Drag-drop Node Editor (3 SP)

### Feature Overview

**User Story**: US 7.2 Part 1 - Drag-drop Node Editor
**Story Points**: 3 SP
**Priority**: P0 (Critical)
**Dependencies**: Feature 1 (VueFlow 集成)

**Feature Description**:
IT 開發者需要 Drag-drop 節點編輯功能:
- **Node Palette**: 節點面板,顯示所有可用節點類型
- **Drag & Drop**: HTML5 Drag & Drop API 實現
- **Edge Connection**: 連接線繪製和驗證
- **Node Selection**: 節點選擇和多選

---

### Phase 1: Node Palette 節點面板 (1 SP)

**Goal**: 實現節點面板,顯示所有可用節點類型

#### Task Breakdown

**T12.031 - T12.040: Node Palette**

**Node Registry Service** (T12.031 - T12.033):
- [ ] T12.031: NodeTypeRegistry 服務
  - 建立 `services/nodeTypeRegistry.ts`
  - 從 Backend API 載入 Node Types
  - 緩存 Node Type 定義

  ```typescript
  // packages/remote/src/services/nodeTypeRegistry.ts
  export interface NodeTypeDefinition {
    type: string;
    category: string;
    name: string;
    description: string;
    icon: string;
    color: string;
    defaultConfig: Record<string, any>;
    inputPorts: PortDefinition[];
    outputPorts: PortDefinition[];
  }

  export interface PortDefinition {
    name: string;
    type: string;  // 'string' | 'number' | 'boolean' | 'object' | 'any'
    required: boolean;
  }

  export class NodeTypeRegistry {
    private static instance: NodeTypeRegistry;
    private nodeTypes: Map<string, NodeTypeDefinition> = new Map();
    private categories: Map<string, NodeTypeDefinition[]> = new Map();

    static getInstance(): NodeTypeRegistry {
      if (!NodeTypeRegistry.instance) {
        NodeTypeRegistry.instance = new NodeTypeRegistry();
      }
      return NodeTypeRegistry.instance;
    }

    async loadNodeTypes(): Promise<void> {
      const response = await fetch('/api/v1/node-types');
      const nodeTypes: NodeTypeDefinition[] = await response.json();

      nodeTypes.forEach(nt => {
        this.nodeTypes.set(nt.type, nt);
        if (!this.categories.has(nt.category)) {
          this.categories.set(nt.category, []);
        }
        this.categories.get(nt.category)!.push(nt);
      });
    }

    getNodeType(type: string): NodeTypeDefinition | undefined {
      return this.nodeTypes.get(type);
    }

    getCategories(): string[] {
      return Array.from(this.categories.keys());
    }

    getNodeTypesByCategory(category: string): NodeTypeDefinition[] {
      return this.categories.get(category) || [];
    }

    getAllNodeTypes(): NodeTypeDefinition[] {
      return Array.from(this.nodeTypes.values());
    }
  }
  ```

- [ ] T12.032: 初始化 Node Registry
  - 在 App.vue 初始化時載入
  - 錯誤處理
  - Loading 狀態

- [ ] T12.033: Node Registry Store
  - 建立 `stores/nodeRegistry.ts`
  - 管理 Node Types 狀態

  ```typescript
  // packages/remote/src/stores/nodeRegistry.ts
  import { defineStore } from 'pinia';
  import { ref, computed } from 'vue';
  import { NodeTypeRegistry } from '@/services/nodeTypeRegistry';
  import type { NodeTypeDefinition } from '@/services/nodeTypeRegistry';

  export const useNodeRegistryStore = defineStore('nodeRegistry', () => {
    const isLoading = ref(false);
    const error = ref<string | null>(null);
    const registry = NodeTypeRegistry.getInstance();

    const categories = computed(() => registry.getCategories());
    const allNodeTypes = computed(() => registry.getAllNodeTypes());

    async function initialize() {
      isLoading.value = true;
      error.value = null;
      try {
        await registry.loadNodeTypes();
      } catch (e) {
        error.value = e.message;
        throw e;
      } finally {
        isLoading.value = false;
      }
    }

    function getNodeTypesByCategory(category: string): NodeTypeDefinition[] {
      return registry.getNodeTypesByCategory(category);
    }

    function getNodeType(type: string): NodeTypeDefinition | undefined {
      return registry.getNodeType(type);
    }

    return {
      isLoading,
      error,
      categories,
      allNodeTypes,
      initialize,
      getNodeTypesByCategory,
      getNodeType,
    };
  });
  ```

**NodePalette 組件** (T12.034 - T12.037):
- [ ] T12.034: NodePalette.vue 主組件
  - 建立 `components/NodePalette.vue`
  - 左側固定面板
  - 分類展開/收起

  ```vue
  <!-- packages/remote/src/components/NodePalette.vue -->
  <template>
    <div class="node-palette">
      <div class="palette-header">
        <h3>節點面板</h3>
        <input
          v-model="searchTerm"
          type="text"
          placeholder="搜索節點..."
          class="search-input"
        />
      </div>

      <div class="palette-content">
        <div
          v-for="category in filteredCategories"
          :key="category"
          class="category-section"
        >
          <div
            class="category-header"
            @click="toggleCategory(category)"
          >
            <IconChevron
              :size="16"
              :direction="expandedCategories.has(category) ? 'down' : 'right'"
            />
            <span>{{ category }}</span>
            <span class="node-count">
              {{ getNodeTypesByCategory(category).length }}
            </span>
          </div>

          <div
            v-if="expandedCategories.has(category)"
            class="category-nodes"
          >
            <NodePaletteItem
              v-for="nodeType in getNodeTypesByCategory(category)"
              :key="nodeType.type"
              :node-type="nodeType"
              @dragstart="handleDragStart($event, nodeType)"
            />
          </div>
        </div>
      </div>
    </div>
  </template>

  <script setup lang="ts">
  import { ref, computed } from 'vue';
  import { useNodeRegistryStore } from '@/stores/nodeRegistry';
  import NodePaletteItem from './NodePaletteItem.vue';
  import { IconChevron } from 'lucide-vue-next';

  const nodeRegistryStore = useNodeRegistryStore();
  const searchTerm = ref('');
  const expandedCategories = ref(new Set<string>());

  const filteredCategories = computed(() => {
    if (!searchTerm.value) {
      return nodeRegistryStore.categories;
    }
    return nodeRegistryStore.categories.filter(category => {
      const nodes = nodeRegistryStore.getNodeTypesByCategory(category);
      return nodes.some(n =>
        n.name.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
        n.description.toLowerCase().includes(searchTerm.value.toLowerCase())
      );
    });
  });

  function getNodeTypesByCategory(category: string) {
    const nodes = nodeRegistryStore.getNodeTypesByCategory(category);
    if (!searchTerm.value) return nodes;
    return nodes.filter(n =>
      n.name.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
      n.description.toLowerCase().includes(searchTerm.value.toLowerCase())
    );
  }

  function toggleCategory(category: string) {
    if (expandedCategories.value.has(category)) {
      expandedCategories.value.delete(category);
    } else {
      expandedCategories.value.add(category);
    }
  }

  function handleDragStart(event: DragEvent, nodeType: any) {
    if (!event.dataTransfer) return;
    event.dataTransfer.effectAllowed = 'copy';
    event.dataTransfer.setData('application/vueflow', JSON.stringify({
      type: nodeType.type,
      label: nodeType.name,
    }));
  }
  </script>

  <style scoped>
  .node-palette {
    width: 280px;
    height: 100vh;
    background: var(--color--background--light-2);
    border-right: 1px solid var(--color--foreground--tint-2);
    display: flex;
    flex-direction: column;
  }

  .palette-header {
    padding: 16px;
    border-bottom: 1px solid var(--color--foreground--tint-2);
  }

  .palette-header h3 {
    margin: 0 0 12px 0;
    font-size: 16px;
    font-weight: 600;
  }

  .search-input {
    width: 100%;
    padding: 8px 12px;
    border: 1px solid var(--color--foreground--tint-2);
    border-radius: 4px;
    font-size: 14px;
  }

  .palette-content {
    flex: 1;
    overflow-y: auto;
    padding: 8px 0;
  }

  .category-header {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 8px 16px;
    cursor: pointer;
    user-select: none;
  }

  .category-header:hover {
    background: var(--color--foreground--tint-3);
  }

  .node-count {
    margin-left: auto;
    font-size: 12px;
    color: var(--color--text--tint-1);
  }

  .category-nodes {
    padding: 4px 0;
  }
  </style>
  ```

- [ ] T12.035: NodePaletteItem.vue 節點項
  - 建立 `components/NodePaletteItem.vue`
  - 節點圖標 + 名稱
  - Draggable 屬性

  ```vue
  <!-- packages/remote/src/components/NodePaletteItem.vue -->
  <template>
    <div
      class="node-palette-item"
      draggable="true"
      @dragstart="$emit('dragstart', $event)"
    >
      <div class="node-icon-wrapper" :style="{ background: nodeType.color }">
        <component :is="getIcon(nodeType.icon)" :size="20" />
      </div>
      <div class="node-info">
        <div class="node-name">{{ nodeType.name }}</div>
        <div class="node-description">{{ nodeType.description }}</div>
      </div>
    </div>
  </template>

  <script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';
  import type { NodeTypeDefinition } from '@/services/nodeTypeRegistry';
  import * as LucideIcons from 'lucide-vue-next';

  defineProps<{
    nodeType: NodeTypeDefinition;
  }>();

  defineEmits<{
    (e: 'dragstart', event: DragEvent): void;
  }>();

  function getIcon(iconName: string) {
    return LucideIcons[iconName] || LucideIcons.IconBox;
  }
  </script>

  <style scoped>
  .node-palette-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 8px 16px;
    cursor: grab;
    user-select: none;
    transition: background 0.2s;
  }

  .node-palette-item:hover {
    background: var(--color--foreground--tint-3);
  }

  .node-palette-item:active {
    cursor: grabbing;
  }

  .node-icon-wrapper {
    width: 32px;
    height: 32px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    flex-shrink: 0;
  }

  .node-info {
    flex: 1;
    min-width: 0;
  }

  .node-name {
    font-size: 14px;
    font-weight: 500;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .node-description {
    font-size: 12px;
    color: var(--color--text--tint-1);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  </style>
  ```

- [ ] T12.036: 搜索功能
  - 實現節點搜索過濾
  - 高亮搜索結果
  - 清空搜索

- [ ] T12.037: NodePalette 測試
  - 測試節點列表渲染
  - 測試分類展開/收起
  - 測試搜索過濾

**節點分類定義** (T12.038 - T12.040):
- [ ] T12.038: 定義節點分類
  - Agent Nodes (Agent Invocation, AI Agent, etc.)
  - Tool Nodes (HTTP Request, Database Query, etc.)
  - Logic Nodes (If, Switch, Loop, etc.)
  - Data Nodes (Set Variable, Transform, etc.)
  - Trigger Nodes (Webhook, Schedule, Manual, etc.)

- [ ] T12.039: 節點圖標和顏色
  - 為每個節點類型定義圖標 (Lucide Icons)
  - 定義分類顏色 (#FF6D5A, #4CAF50, #2196F3, etc.)

- [ ] T12.040: Mock Node Types 數據
  - 創建 Mock 數據用於開發
  - 待 Backend API 完成後替換

---

### Phase 2: HTML5 Drag & Drop 實現 (1 SP)

**Goal**: 實現 Drag & Drop 功能,從 Node Palette 拖拽到畫布

#### Task Breakdown

**T12.041 - T12.050: Drag & Drop**

**Drag Source (Node Palette)** (T12.041 - T12.043):
- [ ] T12.041: Draggable 屬性設置
  - NodePaletteItem 添加 `draggable="true"`
  - 設置 `dragstart` 事件
  - 傳遞節點類型數據 (DataTransfer)

  ```typescript
  // NodePaletteItem dragstart handler
  function handleDragStart(event: DragEvent, nodeType: NodeTypeDefinition) {
    if (!event.dataTransfer) return;

    event.dataTransfer.effectAllowed = 'copy';
    event.dataTransfer.setData('application/vueflow', JSON.stringify({
      type: nodeType.type,
      label: nodeType.name,
      icon: nodeType.icon,
      color: nodeType.color,
      defaultConfig: nodeType.defaultConfig,
    }));

    // 設置拖拽圖像
    const dragImage = createDragImage(nodeType);
    event.dataTransfer.setDragImage(dragImage, 50, 50);
  }

  function createDragImage(nodeType: NodeTypeDefinition): HTMLElement {
    const div = document.createElement('div');
    div.className = 'drag-preview';
    div.style.width = '96px';
    div.style.height = '96px';
    div.style.background = nodeType.color;
    div.style.borderRadius = '8px';
    div.style.display = 'flex';
    div.style.alignItems = 'center';
    div.style.justifyContent = 'center';
    div.innerHTML = `<span style="color: white; font-size: 40px;">${nodeType.icon}</span>`;
    document.body.appendChild(div);
    setTimeout(() => document.body.removeChild(div), 0);
    return div;
  }
  ```

- [ ] T12.042: 拖拽視覺反饋
  - 自定義 Drag Image
  - 拖拽時高亮效果
  - 拖拽時鼠標樣式 (cursor: grabbing)

- [ ] T12.043: DataTransfer 數據格式
  - MIME type: `application/vueflow`
  - JSON payload: `{ type, label, icon, color, defaultConfig }`

**Drop Target (Canvas)** (T12.044 - T12.047):
- [ ] T12.044: Canvas Drop 事件處理
  - 監聽 `drop` 事件
  - 解析 DataTransfer 數據
  - 計算 Drop 位置 (Canvas 坐標)

  ```vue
  <!-- WorkflowCanvas.vue -->
  <template>
    <div
      class="workflow-canvas-container"
      @drop="onDrop"
      @dragover="onDragOver"
    >
      <VueFlow ... />
    </div>
  </template>

  <script setup lang="ts">
  import { useVueFlow } from '@vue-flow/core';
  import { snapToGrid } from '@/utils/grid';
  import { useWorkflowStore } from '@/stores/workflow';
  import { useNodeRegistryStore } from '@/stores/nodeRegistry';

  const { project, vueFlowRef } = useVueFlow();
  const workflowStore = useWorkflowStore();
  const nodeRegistryStore = useNodeRegistryStore();

  function onDragOver(event: DragEvent) {
    event.preventDefault();
    if (event.dataTransfer) {
      event.dataTransfer.dropEffect = 'copy';
    }
  }

  function onDrop(event: DragEvent) {
    event.preventDefault();

    if (!event.dataTransfer) return;
    const data = event.dataTransfer.getData('application/vueflow');
    if (!data) return;

    const nodeData = JSON.parse(data);

    // 獲取 Drop 位置 (瀏覽器坐標)
    const { left, top } = vueFlowRef.value.getBoundingClientRect();
    const dropX = event.clientX - left;
    const dropY = event.clientY - top;

    // 轉換為 Canvas 坐標
    const canvasPosition = project({ x: dropX, y: dropY });

    // Snap to grid
    const [x, y] = snapToGrid([canvasPosition.x, canvasPosition.y]);

    // 創建新節點
    addNode({
      type: nodeData.type,
      label: nodeData.label,
      position: { x, y },
      data: {
        icon: nodeData.icon,
        color: nodeData.color,
        config: nodeData.defaultConfig,
        status: 'NEW',
      },
    });
  }

  function addNode(nodeData: any) {
    const newNode = {
      id: `node-${crypto.randomUUID()}`,
      type: nodeData.type,
      position: nodeData.position,
      label: nodeData.label,
      data: nodeData.data,
    };
    workflowStore.addNode(newNode);
  }
  </script>
  ```

- [ ] T12.045: 坐標轉換邏輯
  - 瀏覽器坐標 → Canvas 坐標 (考慮 zoom, pan)
  - Snap to Grid 對齊
  - 避免節點重疊 (可選)

- [ ] T12.046: 創建新節點
  - 生成唯一 Node ID
  - 設置初始 Position
  - 設置初始 Data (icon, color, config)
  - 添加到 Workflow Store

- [ ] T12.047: Drop 視覺反饋
  - Drop Zone 高亮
  - Drop 時動畫效果
  - Grid 對齊指示線 (可選)

**拖拽狀態管理** (T12.048 - T12.050):
- [ ] T12.048: UI Store 拖拽狀態
  - `isDraggingNode: boolean`
  - `draggedNodeType: string | null`
  - 在 dragstart/dragend 更新

- [ ] T12.049: 拖拽時禁用其他操作
  - 禁用縮放
  - 禁用平移
  - 禁用節點選擇

- [ ] T12.050: Drag & Drop 測試
  - 測試從 Palette 拖拽到 Canvas
  - 測試 Grid 對齊
  - 測試多個節點拖拽
  - 測試拖拽取消 (ESC 鍵)

---

### Phase 3: Edge Connection 連接線 (1 SP)

**Goal**: 實現節點之間的連接線繪製和驗證

#### Task Breakdown

**T12.051 - T12.060: Edge Connection**

**Connection Handle** (T12.051 - T12.053):
- [ ] T12.051: NodeHandle 組件
  - 建立 `components/NodeHandle.vue`
  - Input Handle (左側)
  - Output Handle (右側)

  ```vue
  <!-- packages/remote/src/components/NodeHandle.vue -->
  <template>
    <Handle
      :id="id"
      :type="type"
      :position="position"
      :class="['node-handle', `handle-${type}`]"
      :connectable="connectable"
      @connect="handleConnect"
    />
  </template>

  <script setup lang="ts">
  import { Handle, Position } from '@vue-flow/core';

  defineProps<{
    id: string;
    type: 'source' | 'target';
    position: Position;
    connectable?: boolean;
  }>();

  const emit = defineEmits<{
    (e: 'connect', params: any): void;
  }>();

  function handleConnect(params: any) {
    emit('connect', params);
  }
  </script>

  <style scoped>
  .node-handle {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: var(--color--primary);
    border: 2px solid var(--color--background);
    transition: all 0.2s;
  }

  .node-handle:hover {
    width: 16px;
    height: 16px;
    background: var(--color--primary--shade-1);
  }

  .node-handle.connecting {
    background: var(--color--success);
  }

  .node-handle.valid-connection {
    background: var(--color--success);
  }

  .node-handle.invalid-connection {
    background: var(--color--danger);
  }
  </style>
  ```

- [ ] T12.052: Standard Node Handle 位置
  - Input Handle: Left (Position.Left)
  - Output Handle: Right (Position.Right)
  - 動態計算 Handle 數量和位置

- [ ] T12.053: Handle Visibility
  - 默認隱藏
  - Hover 節點時顯示
  - 連接時始終顯示

**Connection 邏輯** (T12.054 - T12.056):
- [ ] T12.054: VueFlow onConnect 處理
  - 監聽 `@connect` 事件
  - 驗證連接有效性
  - 創建 Edge

  ```typescript
  // WorkflowCanvas.vue onConnect
  function onConnect(params: Connection) {
    const { source, target, sourceHandle, targetHandle } = params;

    // 驗證連接
    const isValid = validateConnection(source, target);
    if (!isValid) {
      showToast('Invalid connection: Incompatible node types', 'error');
      return;
    }

    // 檢查循環依賴
    const hasCycle = detectCycle(source, target);
    if (hasCycle) {
      showToast('Invalid connection: Circular dependency detected', 'error');
      return;
    }

    // 創建 Edge
    const newEdge = {
      id: `edge-${crypto.randomUUID()}`,
      source,
      target,
      sourceHandle,
      targetHandle,
      type: 'default',
      animated: false,
      label: '',
      data: {},
    };

    workflowStore.addEdge(newEdge);
  }

  function validateConnection(sourceId: string, targetId: string): boolean {
    const sourceNode = workflowStore.getNodeById(sourceId);
    const targetNode = workflowStore.getNodeById(targetId);

    if (!sourceNode || !targetNode) return false;

    // 獲取節點類型定義
    const sourceType = nodeRegistryStore.getNodeType(sourceNode.type);
    const targetType = nodeRegistryStore.getNodeType(targetNode.type);

    if (!sourceType || !targetType) return false;

    // 檢查輸出類型是否兼容輸入類型
    // 簡化版本: 假設所有類型兼容
    // 完整版本: Sprint 13 實現類型檢查
    return true;
  }

  function detectCycle(sourceId: string, targetId: string): boolean {
    // DFS 檢測循環依賴
    const visited = new Set<string>();
    const stack = [targetId];

    while (stack.length > 0) {
      const nodeId = stack.pop()!;
      if (nodeId === sourceId) return true;
      if (visited.has(nodeId)) continue;

      visited.add(nodeId);
      const outgoingEdges = workflowStore.getOutgoingEdges(nodeId);
      outgoingEdges.forEach(edge => stack.push(edge.target));
    }

    return false;
  }
  ```

- [ ] T12.055: Connection Validation
  - 節點類型兼容性驗證
  - 循環依賴檢測 (DFS)
  - 端口類型匹配 (Sprint 13 完整實現)

- [ ] T12.056: Connection 視覺反饋
  - 連接中顯示臨時線
  - 有效連接: 綠色
  - 無效連接: 紅色
  - 連接完成動畫

**Edge 樣式** (T12.057 - T12.059):
- [ ] T12.057: Default Edge 樣式
  - 使用 Bezier Curve
  - 箭頭指示方向
  - Hover 高亮

  ```vue
  <!-- packages/remote/src/components/CanvasEdge.vue -->
  <template>
    <BaseEdge
      :id="id"
      :path="path"
      :marker-end="markerEnd"
      :style="edgeStyle"
      @click="handleEdgeClick"
    >
      <EdgeLabel
        v-if="label"
        :label="label"
        :label-x="labelX"
        :label-y="labelY"
      />
    </BaseEdge>
  </template>

  <script setup lang="ts">
  import { computed } from 'vue';
  import { BaseEdge, EdgeLabel } from '@vue-flow/core';

  const props = defineProps<{
    id: string;
    source: string;
    target: string;
    sourceX: number;
    sourceY: number;
    targetX: number;
    targetY: number;
    label?: string;
    selected?: boolean;
  }>();

  const path = computed(() => {
    // Bezier curve path
    const { sourceX, sourceY, targetX, targetY } = props;
    const deltaX = targetX - sourceX;
    const controlOffset = Math.abs(deltaX) / 2;

    return `M ${sourceX},${sourceY} C ${sourceX + controlOffset},${sourceY} ${targetX - controlOffset},${targetY} ${targetX},${targetY}`;
  });

  const edgeStyle = computed(() => ({
    stroke: props.selected ? 'var(--color--primary)' : 'var(--color--foreground--shade-2)',
    strokeWidth: props.selected ? 3 : 2,
  }));

  const markerEnd = 'url(#arrow)';
  const labelX = computed(() => (props.sourceX + props.targetX) / 2);
  const labelY = computed(() => (props.sourceY + props.targetY) / 2);

  function handleEdgeClick() {
    // 選擇 Edge
  }
  </script>
  ```

- [ ] T12.058: Edge Marker (箭頭)
  - 定義 SVG Marker
  - 箭頭大小和顏色

- [ ] T12.059: Edge Label
  - 顯示條件表達式 (Sprint 13)
  - Label 位置 (中點)
  - Label 編輯 (Sprint 13)

**Edge 操作** (T12.060):
- [ ] T12.060: Edge 測試
  - 測試連接創建
  - 測試循環依賴檢測
  - 測試連接刪除 (點擊 Edge + Delete 鍵)
  - 測試 Edge 選擇

---

### Feature 2 驗收標準

**Drag-drop 完整性**:
- ✅ Node Palette 正常顯示所有節點類型
- ✅ 搜索過濾功能正常
- ✅ Drag & Drop 從 Palette 到 Canvas 成功
- ✅ Edge Connection 繪製和驗證正常
- ✅ 循環依賴檢測正常

**交互體驗**:
- ✅ 拖拽流暢 (60fps)
- ✅ Grid 對齊自動化
- ✅ 視覺反饋清晰 (有效/無效連接)
- ✅ 鍵盤快捷鍵 (Delete 刪除節點/邊)

**代碼質量**:
- ✅ TypeScript 類型覆蓋率 100%
- ✅ 單元測試覆蓋率 ≥80%
- ✅ E2E 測試通過 (Playwright)

---

## Feature 3: 基礎節點類型 (2 SP)

### Feature Overview

**User Story**: US 7.2 Part 1 - 基礎節點類型
**Story Points**: 2 SP
**Priority**: P0 (Critical)
**Dependencies**: Feature 1, Feature 2

**Feature Description**:
IT 開發者需要 3 種基礎節點類型:
- **Standard Node**: 96x96px 正方形節點 (Agent, Tool, etc.)
- **Configuration Node**: 80x80px 圓形節點 (Credentials, Variables, etc.)
- **Trigger Node**: 96x96px 特殊圓角節點 (Webhook, Schedule, etc.)
- **節點狀態系統**: NEW, RUNNING, SUCCESS, ERROR, WAITING

---

### Phase 1: Standard Node (0.7 SP)

**Goal**: 實現標準執行節點 (96x96px 正方形)

#### Task Breakdown

**T12.061 - T12.070: Standard Node**

**NodeStandard 組件** (T12.061 - T12.063):
- [ ] T12.061: NodeStandard.vue 主組件
  - 建立 `components/nodes/NodeStandard.vue`
  - 96x96px 正方形
  - 圓角矩形 (border-radius: 8px)

  ```vue
  <!-- packages/remote/src/components/nodes/NodeStandard.vue -->
  <template>
    <div
      :class="['node-standard', stateClass]"
      :style="nodeStyle"
    >
      <!-- Input Handles -->
      <NodeHandle
        v-for="(input, index) in inputPorts"
        :key="`input-${index}`"
        :id="`input-${index}`"
        type="target"
        :position="Position.Left"
        :style="{ top: `${calculatePortY(index, inputPorts.length)}px` }"
      />

      <!-- Node Icon -->
      <div class="node-icon">
        <component :is="getIcon(data.icon)" :size="40" />
      </div>

      <!-- Node Label -->
      <div class="node-label">{{ label }}</div>

      <!-- Status Indicator -->
      <div
        v-if="data.status !== 'NEW'"
        class="status-indicator"
        :class="`status-${data.status.toLowerCase()}`"
      />

      <!-- Output Handles -->
      <NodeHandle
        v-for="(output, index) in outputPorts"
        :key="`output-${index}`"
        :id="`output-${index}`"
        type="source"
        :position="Position.Right"
        :style="{ top: `${calculatePortY(index, outputPorts.length)}px` }"
      />
    </div>
  </template>

  <script setup lang="ts">
  import { computed } from 'vue';
  import { Position } from '@vue-flow/core';
  import NodeHandle from '../NodeHandle.vue';
  import * as LucideIcons from 'lucide-vue-next';
  import { NODE_SIZES, GRID_SIZE } from '@/constants/canvas';

  const props = defineProps<{
    id: string;
    label: string;
    data: {
      icon: string;
      color: string;
      status: 'NEW' | 'RUNNING' | 'SUCCESS' | 'ERROR' | 'WAITING' | 'DISABLED';
      inputPorts: any[];
      outputPorts: any[];
    };
    selected?: boolean;
  }>();

  const inputPorts = computed(() => props.data.inputPorts || []);
  const outputPorts = computed(() => props.data.outputPorts || []);

  const stateClass = computed(() => ({
    'selected': props.selected,
    'running': props.data.status === 'RUNNING',
    'error': props.data.status === 'ERROR',
    'success': props.data.status === 'SUCCESS',
    'disabled': props.data.status === 'DISABLED',
  }));

  const nodeStyle = computed(() => ({
    borderColor: props.data.color,
  }));

  function getIcon(iconName: string) {
    return LucideIcons[iconName] || LucideIcons.IconBox;
  }

  function calculatePortY(index: number, totalPorts: number): number {
    const nodeHeight = NODE_SIZES.DEFAULT.height;
    const spacing = nodeHeight / (totalPorts + 1);
    return spacing * (index + 1);
  }
  </script>

  <style scoped>
  .node-standard {
    width: 96px;
    height: 96px;
    border-radius: var(--radius--lg);  /* 8px */
    border: 2px solid var(--color--foreground--shade-2);
    background: var(--color--background--light-3);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    transition: all 0.2s ease;
    cursor: pointer;
  }

  .node-standard:hover {
    border-color: var(--color--primary);
    box-shadow: var(--shadow--md);
  }

  .node-standard.selected {
    border-color: var(--color--primary);
    box-shadow: var(--shadow--lg), 0 0 0 8px var(--color--primary--tint-3);
  }

  .node-standard.running {
    border-color: var(--color--primary);
  }

  .node-standard.error {
    border-color: var(--color--danger);
  }

  .node-standard.success {
    border-color: var(--color--success);
  }

  .node-standard.disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .node-icon {
    width: 40px;
    height: 40px;
    color: var(--color--foreground--shade-1);
  }

  .node-icon svg {
    width: 100%;
    height: 100%;
  }

  .node-standard.running .node-icon {
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  .node-label {
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translateX(-50%);
    margin-top: 8px;
    font-size: 16px;
    font-weight: 500;
    white-space: nowrap;
    text-align: center;
    max-width: 192px;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .status-indicator {
    position: absolute;
    top: 8px;
    right: 8px;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 2px solid var(--color--background);
  }

  .status-indicator.status-running {
    background: var(--color--primary);
    animation: blink 1s ease-in-out infinite;
  }

  .status-indicator.status-success {
    background: var(--color--success);
  }

  .status-indicator.status-error {
    background: var(--color--danger);
  }

  .status-indicator.status-waiting {
    background: var(--color--secondary);
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
  }
  </style>
  ```

- [ ] T12.062: Input/Output Handles 動態計算
  - 根據端口數量計算 Handle 位置
  - 端口數量 1-5 個時均勻分布
  - 超過 5 個時節點高度自動增加

- [ ] T12.063: Node Label 外部顯示
  - Label 在節點底部外側
  - 最大寬度 192px (2 倍節點寬度)
  - 文本溢出省略號

**節點狀態系統** (T12.064 - T12.066):
- [ ] T12.064: 狀態枚舉定義
  - NEW: 灰色,未執行
  - RUNNING: 主色,執行中 (帶動畫)
  - SUCCESS: 綠色,執行成功
  - ERROR: 紅色,執行錯誤
  - WAITING: 紫色,等待中
  - DISABLED: 灰色半透明,禁用

  ```typescript
  // packages/remote/src/types/node.ts
  export enum NodeStatus {
    NEW = 'NEW',
    RUNNING = 'RUNNING',
    SUCCESS = 'SUCCESS',
    ERROR = 'ERROR',
    WAITING = 'WAITING',
    DISABLED = 'DISABLED',
  }

  export const NODE_STATE_COLORS = {
    NEW: '--color--foreground--shade-2',
    RUNNING: '--color--primary',
    SUCCESS: '--color--success',
    ERROR: '--color--danger',
    WAITING: '--color--secondary',
    DISABLED: '--color--foreground',
  } as const;
  ```

- [ ] T12.065: 狀態指示器
  - 節點右上角顯示狀態圓點
  - RUNNING 狀態閃爍動畫
  - SUCCESS/ERROR 狀態靜態顯示

- [ ] T12.066: 狀態切換測試
  - 測試所有狀態顯示正確
  - 測試狀態顏色映射
  - 測試動畫效果

**樣式與動畫** (T12.067 - T12.070):
- [ ] T12.067: Hover 效果
  - 邊框顏色變化
  - 陰影增強 (box-shadow)
  - Transition 動畫 (0.2s ease)

- [ ] T12.068: Selected 效果
  - 邊框高亮 (主色)
  - 外圍光暈 (8px)
  - 陰影增強

- [ ] T12.069: RUNNING 動畫
  - 圖標脈動動畫 (pulse)
  - 邊框閃爍動畫 (可選)

- [ ] T12.070: NodeStandard 測試
  - 測試節點渲染
  - 測試 Handles 位置
  - 測試狀態顯示
  - 測試動畫效果

---

### Phase 2: Configuration Node (0.7 SP)

**Goal**: 實現配置型節點 (80x80px 圓形)

#### Task Breakdown

**T12.071 - T12.080: Configuration Node**

**NodeConfiguration 組件** (T12.071 - T12.073):
- [ ] T12.071: NodeConfiguration.vue 主組件
  - 建立 `components/nodes/NodeConfiguration.vue`
  - 80x80px 圓形
  - border-radius: 50%

  ```vue
  <!-- packages/remote/src/components/nodes/NodeConfiguration.vue -->
  <template>
    <div
      :class="['node-configuration', stateClass]"
      :style="nodeStyle"
    >
      <!-- Node Icon -->
      <div class="node-icon">
        <component :is="getIcon(data.icon)" :size="30" />
      </div>

      <!-- Node Label -->
      <div class="node-label">{{ label }}</div>

      <!-- Optional Handles (配置節點通常無端口) -->
      <NodeHandle
        v-if="showHandles"
        id="output-0"
        type="source"
        :position="Position.Right"
      />
    </div>
  </template>

  <script setup lang="ts">
  import { computed } from 'vue';
  import { Position } from '@vue-flow/core';
  import NodeHandle from '../NodeHandle.vue';
  import * as LucideIcons from 'lucide-vue-next';
  import { NODE_SIZES } from '@/constants/canvas';

  const props = defineProps<{
    id: string;
    label: string;
    data: {
      icon: string;
      color: string;
      status: string;
    };
    selected?: boolean;
  }>();

  const showHandles = computed(() => false);  // 配置節點通常無端口

  const stateClass = computed(() => ({
    'selected': props.selected,
  }));

  const nodeStyle = computed(() => ({
    background: `var(--node-type--supplemental--color--background)`,
    borderColor: props.data.color,
  }));

  function getIcon(iconName: string) {
    return LucideIcons[iconName] || LucideIcons.IconSettings;
  }
  </script>

  <style scoped>
  .node-configuration {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    border: 2px solid var(--color--foreground--shade-1);
    background: var(--node-type--supplemental--color--background);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    transition: all 0.2s ease;
    cursor: pointer;
  }

  .node-configuration:hover {
    border-color: var(--color--primary);
    box-shadow: var(--shadow--md);
  }

  .node-configuration.selected {
    border-color: var(--color--primary);
    box-shadow: var(--shadow--lg), 0 0 0 8px var(--color--primary--tint-3);
  }

  .node-icon {
    width: 30px;
    height: 30px;
    color: var(--color--foreground--shade-1);
  }

  .node-icon svg {
    width: 100%;
    height: 100%;
  }

  .node-label {
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translateX(-50%);
    margin-top: 8px;
    font-size: 16px;
    font-weight: 500;
    white-space: nowrap;
    text-align: center;
    max-width: 160px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  </style>
  ```

- [ ] T12.072: 圓形節點特殊樣式
  - 背景色: 淺灰色 (supplemental color)
  - 圖標大小: 30px (比 Standard 小)
  - 無端口 (通常)

- [ ] T12.073: NodeConfiguration 測試
  - 測試節點渲染
  - 測試圓形樣式
  - 測試 Hover/Selected 效果

**配置節點用途** (T12.074 - T12.076):
- [ ] T12.074: 定義配置節點類型
  - API Credentials (認證憑證)
  - Database Connection (數據庫連接)
  - Environment Variables (環境變數)
  - Global Settings (全局設定)

- [ ] T12.075: 配置節點交互
  - 雙擊打開配置面板 (Sprint 13)
  - 無拖拽連接 (通常)
  - 配置變更通知

- [ ] T12.076: 配置節點示例數據
  - Mock Credentials Node
  - Mock Database Connection Node

**樣式變體** (T12.077 - T12.080):
- [ ] T12.077: 配置節點顏色
  - 區別於執行節點
  - 使用淺色背景

- [ ] T12.078: Icon 選擇
  - Settings, Key, Database, Variable, etc.

- [ ] T12.079: 配置節點動畫
  - Hover 效果
  - Selected 效果

- [ ] T12.080: 配置節點完整測試
  - E2E 測試拖拽創建
  - 測試配置面板打開 (占位符)

---

### Phase 3: Trigger Node (0.6 SP)

**Goal**: 實現觸發型節點 (96x96px 特殊圓角)

#### Task Breakdown

**T12.081 - T12.090: Trigger Node**

**NodeTrigger 組件** (T12.081 - T12.083):
- [ ] T12.081: NodeTrigger.vue 主組件
  - 建立 `components/nodes/NodeTrigger.vue`
  - 96x96px 大小
  - 左側特殊圓角 (36px)
  - 右側普通圓角 (8px)

  ```vue
  <!-- packages/remote/src/components/nodes/NodeTrigger.vue -->
  <template>
    <div
      :class="['node-trigger', stateClass]"
      :style="nodeStyle"
    >
      <!-- Trigger Indicator (左側圓點) -->
      <div class="trigger-indicator" />

      <!-- Node Icon -->
      <div class="node-icon">
        <component :is="getIcon(data.icon)" :size="40" />
      </div>

      <!-- Node Label -->
      <div class="node-label">{{ label }}</div>

      <!-- Output Handle (只有輸出) -->
      <NodeHandle
        id="output-0"
        type="source"
        :position="Position.Right"
        :style="{ top: '50%', transform: 'translateY(-50%)' }"
      />
    </div>
  </template>

  <script setup lang="ts">
  import { computed } from 'vue';
  import { Position } from '@vue-flow/core';
  import NodeHandle from '../NodeHandle.vue';
  import * as LucideIcons from 'lucide-vue-next';

  const props = defineProps<{
    id: string;
    label: string;
    data: {
      icon: string;
      color: string;
      status: string;
    };
    selected?: boolean;
  }>();

  const stateClass = computed(() => ({
    'selected': props.selected,
    'active': props.data.status === 'RUNNING',
  }));

  const nodeStyle = computed(() => ({
    borderColor: 'var(--color--primary)',
    background: `linear-gradient(135deg, var(--color--primary--tint-3) 0%, var(--color--background--light-3) 100%)`,
  }));

  function getIcon(iconName: string) {
    return LucideIcons[iconName] || LucideIcons.IconZap;
  }
  </script>

  <style scoped>
  .node-trigger {
    width: 96px;
    height: 96px;
    /* 特殊的不對稱圓角 */
    border-top-left-radius: 36px;
    border-bottom-left-radius: 36px;
    border-top-right-radius: var(--radius--lg);
    border-bottom-right-radius: var(--radius--lg);
    border: 2px solid var(--color--primary);
    background: linear-gradient(135deg,
      var(--color--primary--tint-3) 0%,
      var(--color--background--light-3) 100%
    );
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    transition: all 0.2s ease;
    cursor: pointer;
  }

  .node-trigger:hover {
    box-shadow: var(--shadow--md);
  }

  .node-trigger.selected {
    box-shadow: var(--shadow--lg), 0 0 0 8px var(--color--primary--tint-3);
  }

  /* 觸發節點的特殊視覺標記 */
  .trigger-indicator {
    position: absolute;
    left: -6px;
    top: 50%;
    transform: translateY(-50%);
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: var(--color--primary);
    border: 2px solid var(--color--background);
  }

  .node-trigger.active .trigger-indicator {
    animation: pulse-trigger 1.5s ease-in-out infinite;
  }

  @keyframes pulse-trigger {
    0%, 100% {
      transform: translateY(-50%) scale(1);
      opacity: 1;
    }
    50% {
      transform: translateY(-50%) scale(1.3);
      opacity: 0.7;
    }
  }

  .node-icon {
    width: 40px;
    height: 40px;
    color: var(--color--primary);
  }

  .node-icon svg {
    width: 100%;
    height: 100%;
  }

  .node-label {
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translateX(-50%);
    margin-top: 8px;
    font-size: 16px;
    font-weight: 500;
    white-space: nowrap;
    text-align: center;
    max-width: 192px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  </style>
  ```

- [ ] T12.082: 不對稱圓角設置
  - border-top-left-radius: 36px
  - border-bottom-left-radius: 36px
  - border-top-right-radius: 8px
  - border-bottom-right-radius: 8px

- [ ] T12.083: Trigger Indicator (左側圓點)
  - 12px 圓形
  - 位置: 左側外部 (-6px)
  - ACTIVE 狀態脈動動畫

**Trigger 節點特性** (T12.084 - T12.086):
- [ ] T12.084: 定義 Trigger 節點類型
  - Webhook Trigger
  - Schedule Trigger (Cron)
  - Manual Trigger
  - Event Trigger

- [ ] T12.085: Trigger 節點交互
  - 只有輸出端口 (無輸入)
  - 作為 Workflow 入口點
  - 雙擊配置 Trigger 參數 (Sprint 13)

- [ ] T12.086: Trigger 節點示例數據
  - Mock Webhook Trigger
  - Mock Schedule Trigger

**樣式與動畫** (T12.087 - T12.090):
- [ ] T12.087: 漸變背景
  - 左側淺色 (primary tint-3)
  - 右側白色
  - 135deg 線性漸變

- [ ] T12.088: ACTIVE 狀態動畫
  - Trigger Indicator 脈動
  - 邊框閃爍 (可選)

- [ ] T12.089: Icon 選擇
  - Zap, Webhook, Clock, Play, etc.

- [ ] T12.090: NodeTrigger 測試
  - 測試節點渲染
  - 測試不對稱圓角
  - 測試 Trigger Indicator 動畫
  - E2E 測試拖拽創建

---

### Phase 4: 節點註冊與類型映射 (0 SP - 整合工作)

**Goal**: 將 3 種節點類型註冊到 VueFlow

#### Task Breakdown

**T12.091 - T12.095: 節點類型註冊**

**VueFlow 節點類型註冊** (T12.091 - T12.093):
- [ ] T12.091: 註冊自定義節點類型
  - 在 VueFlow 配置中註冊
  - 映射節點類型到組件

  ```typescript
  // packages/remote/src/main.ts
  import { createApp } from 'vue';
  import { createPinia } from 'pinia';
  import App from './App.vue';
  import NodeStandard from '@/components/nodes/NodeStandard.vue';
  import NodeConfiguration from '@/components/nodes/NodeConfiguration.vue';
  import NodeTrigger from '@/components/nodes/NodeTrigger.vue';

  const app = createApp(App);
  const pinia = createPinia();

  // 註冊全局組件
  app.component('NodeStandard', NodeStandard);
  app.component('NodeConfiguration', NodeConfiguration);
  app.component('NodeTrigger', NodeTrigger);

  app.use(pinia);
  app.mount('#app');
  ```

  ```vue
  <!-- WorkflowCanvas.vue -->
  <template>
    <VueFlow
      v-model="elements"
      :node-types="nodeTypes"
      ...
    >
      ...
    </VueFlow>
  </template>

  <script setup lang="ts">
  import { NodeStandard, NodeConfiguration, NodeTrigger } from '@/components/nodes';

  const nodeTypes = {
    standard: NodeStandard,
    configuration: NodeConfiguration,
    trigger: NodeTrigger,
  };
  </script>
  ```

- [ ] T12.092: 節點類型映射邏輯
  - 根據 `nodeType` 映射到組件
  - Agent, Tool → `standard`
  - Credentials, Variables → `configuration`
  - Webhook, Schedule → `trigger`

  ```typescript
  // packages/remote/src/utils/nodeTypeMapper.ts
  export function getNodeComponentType(nodeType: string): string {
    const configurationTypes = ['credentials', 'variables', 'database-connection'];
    const triggerTypes = ['webhook', 'schedule', 'manual-trigger', 'event-trigger'];

    if (configurationTypes.includes(nodeType)) {
      return 'configuration';
    } else if (triggerTypes.includes(nodeType)) {
      return 'trigger';
    } else {
      return 'standard';
    }
  }
  ```

- [ ] T12.093: 節點創建工廠函數
  - 統一創建節點的接口

  ```typescript
  // packages/remote/src/utils/nodeFactory.ts
  import { getNodeComponentType } from './nodeTypeMapper';
  import { snapToGrid } from './grid';

  export interface CreateNodeOptions {
    type: string;
    label: string;
    position: { x: number; y: number };
    data?: any;
  }

  export function createNode(options: CreateNodeOptions) {
    const { type, label, position, data = {} } = options;
    const [x, y] = snapToGrid([position.x, position.y]);

    return {
      id: `node-${crypto.randomUUID()}`,
      type: getNodeComponentType(type),
      position: { x, y },
      label,
      data: {
        nodeType: type,  // 原始節點類型
        icon: data.icon || 'IconBox',
        color: data.color || '#9E9E9E',
        status: 'NEW',
        config: data.defaultConfig || {},
        inputPorts: data.inputPorts || [],
        outputPorts: data.outputPorts || [],
        ...data,
      },
    };
  }
  ```

**節點渲染測試** (T12.094 - T12.095):
- [ ] T12.094: 集成測試
  - 測試 3 種節點類型正確渲染
  - 測試節點類型映射邏輯
  - 測試節點創建工廠

- [ ] T12.095: E2E 測試
  - 拖拽創建所有 3 種節點類型
  - 驗證節點樣式正確
  - 驗證節點交互正常

---

### Feature 3 驗收標準

**節點類型完整性**:
- ✅ 3 種基礎節點類型全部實現
- ✅ Standard Node (96x96px 正方形)
- ✅ Configuration Node (80x80px 圓形)
- ✅ Trigger Node (96x96px 特殊圓角)
- ✅ 節點狀態系統正常 (NEW, RUNNING, SUCCESS, ERROR, WAITING, DISABLED)

**視覺設計質量**:
- ✅ 符合 16px grid system
- ✅ 節點樣式符合設計規範
- ✅ Hover/Selected 效果流暢
- ✅ 動畫效果自然 (pulse, blink)

**代碼質量**:
- ✅ TypeScript 類型覆蓋率 100%
- ✅ 單元測試覆蓋率 ≥80%
- ✅ E2E 測試通過 (Playwright)
- ✅ 組件可復用性高

---

## Phase 1D 啟動驗收

### Sprint 12 最終驗收 Checklist

**Feature 1 (VueFlow 集成) 驗收**:
- [ ] ✅ Module Federation 架構穩定運行
- [ ] ✅ Host ↔ Remote 通訊正常 (<50ms 延遲)
- [ ] ✅ VueFlow 畫布正常渲染
- [ ] ✅ 16px Grid System 正確實現
- [ ] ✅ Toolbar, Minimap, ZoomControls 全部可用
- [ ] ✅ 性能達標 (Remote 載入 <2s, 畫布 60fps)

**Feature 2 (Drag-drop) 驗收**:
- [ ] ✅ Node Palette 正常顯示
- [ ] ✅ 搜索過濾功能正常
- [ ] ✅ Drag & Drop 功能完整
- [ ] ✅ Edge Connection 繪製和驗證正常
- [ ] ✅ 循環依賴檢測正常
- [ ] ✅ 交互體驗流暢 (60fps)

**Feature 3 (基礎節點) 驗收**:
- [ ] ✅ 3 種節點類型全部實現
- [ ] ✅ 節點狀態系統正常
- [ ] ✅ 節點樣式符合設計規範
- [ ] ✅ 動畫效果自然
- [ ] ✅ E2E 測試通過

**整體質量驗收**:
- [ ] ✅ 代碼覆蓋率 ≥80%
- [ ] ✅ TypeScript 類型覆蓋率 100%
- [ ] ✅ 零 P0/P1 Bugs
- [ ] ✅ 所有整合測試通過
- [ ] ✅ API 文檔完整 (Component API)

**Phase 1D Part 1 完成標準**:
- [ ] ✅ Sprint 12 所有交付物驗收通過
- [ ] ✅ Phase 1D Critical Path 33% 完成
- [ ] ✅ 無已知 P0/P1 技術債務
- [ ] ✅ Sprint 13 準備度 ≥90%

---

### Phase 1D 啟動回顧與展望

**Sprint 12 成果總結**:
```yaml
Duration: 3 weeks (Week 34-36)
Story Points: 8 SP
Team Size: 3 frontend developers
Features Delivered:
  1. VueFlow 集成 (Module Federation)
  2. Drag-drop Node Editor
  3. 基礎節點類型 (Standard, Configuration, Trigger)

Technical Achievements:
  - React + Vue 微前端架構
  - VueFlow 畫布引擎
  - 16px Grid System
  - 3 種節點類型
  - Drag & Drop 功能
  - >80% code coverage
  - Zero P0 bugs
```

**Phase 1D Part 1 慶祝活動**:
- 🎉 Team Demo Meeting (Week 36 End)
- 📊 VueFlow 畫布 Demo to Stakeholders
- 🏆 Recognition of Key Contributors
- 📝 Sprint 12 Retrospective
- 🚀 Sprint 13 Kickoff Preview (內聯配置系統)

**Sprint 12 Retrospective 重點**:
- 📚 What Went Well (微前端架構, VueFlow 集成)
- 📚 What Could Be Improved (改進建議)
- 📚 Lessons Learned (經驗教訓)
- 🎯 Action Items for Sprint 13

---

## Phase 1D 技術準備

### Sprint 13 Preview

**Sprint 13 範圍**: 內聯配置系統 + 表達式編輯器 (8-9 SP)
**Sprint 13 週期**: Week 37-39 (3 週)

**Sprint 13 關鍵交付物**:
1. **內聯配置系統**:
   - Configurable Node (320x96px 橫長型)
   - 展開/收起功能
   - 參數編輯器組件

2. **表達式編輯器**:
   - `{{$json.xxx}}` 語法支持
   - 智能提示 (Autocomplete)
   - 實時語法驗證

3. **參數類型系統**:
   - String, Number, Boolean, Object, Array
   - Collection (Key-Value pairs)
   - Code Editor (JSON, JavaScript)

**Sprint 13 準備工作** (Sprint 12 結束前):
- [ ] 研究 CodeMirror 6 集成
- [ ] 設計表達式語法解析器
- [ ] 設計參數類型組件架構
- [ ] 準備 Sprint 13 User Stories

---

**文件版本**: v2.1
**最後更新**: 2025-11-14
**狀態**: 📋 計劃階段 (Planned)
**Sprint Story Points**: 8 SP
**Phase 1D 啟動**: Sprint 12 (本 Sprint)
**Phase 1D 進度**: 33% (Sprint 12 完成時)
**下一 Sprint**: Sprint 13 (Week 37-39, 內聯配置系統)
