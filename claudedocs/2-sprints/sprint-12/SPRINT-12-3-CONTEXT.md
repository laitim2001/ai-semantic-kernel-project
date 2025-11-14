# Sprint 12 技術上下文 (Technical Context)

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

1. [技術上下文概述](#技術上下文概述)
2. [架構上下文](#架構上下文)
3. [核心功能詳細設計](#核心功能詳細設計)
4. [Phase 1D 啟動技術準備](#phase-1d-啟動技術準備)
5. [技術決策記錄](#技術決策記錄)
6. [依賴與整合](#依賴與整合)
7. [參考文檔](#參考文檔)

---

## 技術上下文概述

### Sprint 12 定位

Sprint 12 是 **Phase 1D (工作流編輯器 Frontend)** 的第一個 Sprint，標誌著從 Phase 1C (Backend API) 到 Phase 1D (Frontend UI) 的重要轉換。

```
Phase 1D Timeline (3 Sprints):
┌─────────────┬─────────────┬─────────────┐
│  Sprint 12  │  Sprint 13  │  Sprint 14  │
│  (Week 34)  │  (Week 37)  │  (Week 40)  │
├─────────────┼─────────────┼─────────────┤
│ 🚀 VueFlow  │ Connection  │ State Mgmt  │
│ 畫布基礎    │ & Validation│ & Persist   │
└─────────────┴─────────────┴─────────────┘
```

### 技術上下文摘要

**主要技術棧**:
```yaml
Frontend Framework:
  - Vue 3 (Composition API)
  - TypeScript 5.2+
  - Vite 5

Workflow Canvas:
  - VueFlow 1.45.0 (核心)
  - @vue-flow/core
  - @vue-flow/background
  - @vue-flow/controls
  - @vue-flow/minimap

Micro-Frontend:
  - Module Federation (Webpack 5)
  - Single-SPA (optional)

State Management:
  - Pinia 2.1+
  - VueUse (composables)

UI Components:
  - Element Plus (primary)
  - Custom Node Components

Build & Dev:
  - Vite 5 (dev server)
  - TypeScript compiler
  - ESLint + Prettier
```

**核心能力**:
1. **VueFlow 集成** (3 SP)
   - Vue 3 微前端架構
   - VueFlow 畫布渲染
   - Module Federation 配置

2. **Drag-drop Node Editor** (3 SP)
   - Node Palette (工具箱)
   - Drag & Drop 功能
   - Canvas Interaction

3. **基礎節點類型** (2 SP)
   - Agent Node
   - Plugin Node
   - Decision Node
   - Start/End Node

---

## 架構上下文

### 1. Micro-Frontend 架構

#### Module Federation 配置

**Host Application (React)**:
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
        workflowEditor: 'http://localhost:3001/assets/remoteEntry.js'
      },
      shared: ['react', 'react-dom', 'react-router-dom']
    })
  ],
  build: {
    modulePreload: false,
    target: 'esnext',
    minify: false,
    cssCodeSplit: false
  }
});
```

**Remote Application (Vue 3)**:
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
        './WorkflowEditor': './src/WorkflowEditor.vue',
        './NodePalette': './src/components/NodePalette.vue'
      },
      shared: ['vue', 'pinia', 'element-plus', '@vue-flow/core']
    })
  ],
  build: {
    modulePreload: false,
    target: 'esnext',
    minify: false,
    cssCodeSplit: false
  },
  server: {
    port: 3001,
    cors: true
  }
});
```

#### 跨框架整合策略

```typescript
// Host (React) 載入 Remote (Vue)
import { lazy, Suspense } from 'react';

const WorkflowEditor = lazy(() => import('workflowEditor/WorkflowEditor'));

function WorkflowPage() {
  return (
    <Suspense fallback={<Loading />}>
      <WorkflowEditor
        workflowId="abc123"
        onSave={handleSave}
        onCancel={handleCancel}
      />
    </Suspense>
  );
}
```

**技術決策**: TD-088 - Module Federation with Vite

---

### 2. VueFlow 架構

#### 核心組件結構

```
packages/remote/src/
├── WorkflowEditor.vue          # 主容器
├── components/
│   ├── NodePalette.vue         # 節點工具箱
│   ├── Canvas.vue              # VueFlow 畫布
│   ├── nodes/
│   │   ├── AgentNode.vue       # Agent 節點
│   │   ├── PluginNode.vue      # Plugin 節點
│   │   ├── DecisionNode.vue    # Decision 節點
│   │   └── StartEndNode.vue    # Start/End 節點
│   └── controls/
│       ├── ZoomControl.vue     # 縮放控制
│       └── MiniMap.vue         # 小地圖
├── composables/
│   ├── useVueFlow.ts           # VueFlow hooks
│   ├── useDragDrop.ts          # Drag & Drop
│   └── useNodeOperations.ts    # 節點操作
└── stores/
    ├── workflow.ts             # 工作流狀態
    └── nodes.ts                # 節點狀態
```

#### VueFlow 基礎配置

```vue
<!-- packages/remote/src/WorkflowEditor.vue -->
<template>
  <div class="workflow-editor">
    <!-- Node Palette -->
    <NodePalette
      @node-drag-start="handleNodeDragStart"
      @node-drag-end="handleNodeDragEnd"
    />

    <!-- Canvas -->
    <VueFlow
      v-model="elements"
      :snap-to-grid="true"
      :snap-grid="[15, 15]"
      :default-zoom="1"
      :min-zoom="0.2"
      :max-zoom="4"
      :delete-key-code="null"
      @node-drag-stop="onNodeDragStop"
      @connect="onConnect"
      @edge-update="onEdgeUpdate"
      @pane-click="onPaneClick"
    >
      <!-- Background Pattern -->
      <Background
        :pattern-color="'#aaa'"
        :gap="15"
      />

      <!-- Controls -->
      <Controls />

      <!-- MiniMap -->
      <MiniMap />

      <!-- Custom Nodes -->
      <template #node-agent="{ data }">
        <AgentNode :data="data" />
      </template>

      <template #node-plugin="{ data }">
        <PluginNode :data="data" />
      </template>

      <template #node-decision="{ data }">
        <DecisionNode :data="data" />
      </template>

      <template #node-start="{ data }">
        <StartEndNode :data="data" type="start" />
      </template>

      <template #node-end="{ data }">
        <StartEndNode :data="data" type="end" />
      </template>
    </VueFlow>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { VueFlow, Background, Controls, MiniMap } from '@vue-flow/core';
import { useVueFlow } from '@vue-flow/core';
import NodePalette from './components/NodePalette.vue';
import AgentNode from './components/nodes/AgentNode.vue';
import PluginNode from './components/nodes/PluginNode.vue';
import DecisionNode from './components/nodes/DecisionNode.vue';
import StartEndNode from './components/nodes/StartEndNode.vue';

const { onConnect, onNodeDragStop, addEdges, removeEdges } = useVueFlow();

const elements = ref([]);

const handleNodeDragStart = (nodeType: string) => {
  // 處理節點拖曳開始
};

const handleNodeDragEnd = (event: DragEvent) => {
  // 處理節點拖曳結束,新增到畫布
};

const onConnect = (params: Connection) => {
  // 處理連線建立
  addEdges([params]);
};

const onEdgeUpdate = (oldEdge: Edge, newConnection: Connection) => {
  // 處理連線更新
  removeEdges([oldEdge]);
  addEdges([newConnection]);
};

const onPaneClick = () => {
  // 處理畫布點擊
};
</script>

<style scoped>
.workflow-editor {
  display: flex;
  height: 100vh;
  width: 100%;
}
</style>
```

**技術決策**: TD-089 - VueFlow as Canvas Engine

---

### 3. State Management 架構

#### Pinia Stores 設計

**Workflow Store**:
```typescript
// packages/remote/src/stores/workflow.ts
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';
import type { WorkflowDefinition, Node, Edge } from '@/types';

export const useWorkflowStore = defineStore('workflow', () => {
  // State
  const currentWorkflow = ref<WorkflowDefinition | null>(null);
  const nodes = ref<Node[]>([]);
  const edges = ref<Edge[]>([]);
  const selectedNodeId = ref<string | null>(null);
  const isDirty = ref(false);

  // Getters
  const selectedNode = computed(() =>
    nodes.value.find(n => n.id === selectedNodeId.value)
  );

  const canSave = computed(() =>
    isDirty.value && validateWorkflow()
  );

  const nodeCount = computed(() => nodes.value.length);
  const edgeCount = computed(() => edges.value.length);

  // Actions
  function loadWorkflow(workflow: WorkflowDefinition) {
    currentWorkflow.value = workflow;
    nodes.value = workflow.nodes || [];
    edges.value = workflow.edges || [];
    isDirty.value = false;
  }

  function addNode(node: Node) {
    nodes.value.push(node);
    isDirty.value = true;
  }

  function updateNode(nodeId: string, updates: Partial<Node>) {
    const index = nodes.value.findIndex(n => n.id === nodeId);
    if (index !== -1) {
      nodes.value[index] = { ...nodes.value[index], ...updates };
      isDirty.value = true;
    }
  }

  function removeNode(nodeId: string) {
    nodes.value = nodes.value.filter(n => n.id !== nodeId);
    edges.value = edges.value.filter(
      e => e.source !== nodeId && e.target !== nodeId
    );
    isDirty.value = true;
  }

  function addEdge(edge: Edge) {
    edges.value.push(edge);
    isDirty.value = true;
  }

  function removeEdge(edgeId: string) {
    edges.value = edges.value.filter(e => e.id !== edgeId);
    isDirty.value = true;
  }

  function selectNode(nodeId: string | null) {
    selectedNodeId.value = nodeId;
  }

  function validateWorkflow(): boolean {
    // 驗證工作流結構
    const hasStart = nodes.value.some(n => n.type === 'start');
    const hasEnd = nodes.value.some(n => n.type === 'end');
    return hasStart && hasEnd && nodes.value.length >= 2;
  }

  async function saveWorkflow() {
    if (!canSave.value) return;

    const workflowData = {
      ...currentWorkflow.value,
      nodes: nodes.value,
      edges: edges.value,
      updatedAt: new Date().toISOString()
    };

    // Call API to save
    // await workflowApi.update(workflowData);

    isDirty.value = false;
  }

  function resetWorkflow() {
    currentWorkflow.value = null;
    nodes.value = [];
    edges.value = [];
    selectedNodeId.value = null;
    isDirty.value = false;
  }

  return {
    // State
    currentWorkflow,
    nodes,
    edges,
    selectedNodeId,
    isDirty,
    // Getters
    selectedNode,
    canSave,
    nodeCount,
    edgeCount,
    // Actions
    loadWorkflow,
    addNode,
    updateNode,
    removeNode,
    addEdge,
    removeEdge,
    selectNode,
    validateWorkflow,
    saveWorkflow,
    resetWorkflow
  };
});
```

**Node Store**:
```typescript
// packages/remote/src/stores/nodes.ts
import { defineStore } from 'pinia';
import { ref } from 'vue';
import type { NodeType, NodeConfig } from '@/types';

export const useNodeStore = defineStore('nodes', () => {
  // Available node types
  const nodeTypes = ref<NodeType[]>([
    {
      type: 'agent',
      label: 'Agent',
      icon: 'robot',
      category: 'execution',
      description: 'Execute an agent'
    },
    {
      type: 'plugin',
      label: 'Plugin',
      icon: 'plugin',
      category: 'execution',
      description: 'Execute a plugin'
    },
    {
      type: 'decision',
      label: 'Decision',
      icon: 'fork',
      category: 'control',
      description: 'Conditional branching'
    },
    {
      type: 'start',
      label: 'Start',
      icon: 'play',
      category: 'flow',
      description: 'Workflow start point'
    },
    {
      type: 'end',
      label: 'End',
      icon: 'stop',
      category: 'flow',
      description: 'Workflow end point'
    }
  ]);

  const nodeConfigs = ref<Record<string, NodeConfig>>({});

  function getNodeType(type: string) {
    return nodeTypes.value.find(nt => nt.type === type);
  }

  function getNodesByCategory(category: string) {
    return nodeTypes.value.filter(nt => nt.category === category);
  }

  function createNodeConfig(type: string): NodeConfig {
    const nodeType = getNodeType(type);
    if (!nodeType) throw new Error(`Unknown node type: ${type}`);

    return {
      type,
      label: nodeType.label,
      data: {},
      position: { x: 0, y: 0 },
      style: {}
    };
  }

  return {
    nodeTypes,
    nodeConfigs,
    getNodeType,
    getNodesByCategory,
    createNodeConfig
  };
});
```

**技術決策**: TD-090 - Pinia for State Management

---

## 核心功能詳細設計

### 功能 1: VueFlow 集成 (3 SP)

#### 1.1 Vue 3 + VueFlow 整合

**目標**: 建立 VueFlow 畫布渲染環境

**技術實現**:

```typescript
// packages/remote/src/composables/useVueFlow.ts
import { ref, computed, watch } from 'vue';
import { useVueFlow as useVF, Position } from '@vue-flow/core';
import { useWorkflowStore } from '@/stores/workflow';

export function useVueFlow() {
  const store = useWorkflowStore();
  const {
    addNodes,
    addEdges,
    removeNodes,
    removeEdges,
    updateNode,
    updateEdge,
    getNodes,
    getEdges,
    project,
    vueFlowRef
  } = useVF();

  // Sync store with VueFlow
  watch(
    () => store.nodes,
    (nodes) => {
      const vfNodes = getNodes.value;
      const toAdd = nodes.filter(
        n => !vfNodes.find(vn => vn.id === n.id)
      );
      const toRemove = vfNodes.filter(
        vn => !nodes.find(n => n.id === vn.id)
      );

      if (toAdd.length) addNodes(toAdd);
      if (toRemove.length) removeNodes(toRemove.map(n => n.id));
    },
    { deep: true }
  );

  watch(
    () => store.edges,
    (edges) => {
      const vfEdges = getEdges.value;
      const toAdd = edges.filter(
        e => !vfEdges.find(ve => ve.id === e.id)
      );
      const toRemove = vfEdges.filter(
        ve => !edges.find(e => e.id === ve.id)
      );

      if (toAdd.length) addEdges(toAdd);
      if (toRemove.length) removeEdges(toRemove.map(e => e.id));
    },
    { deep: true }
  );

  // Add node at specific position
  function addNodeAtPosition(nodeConfig: any, position: { x: number; y: number }) {
    const node = {
      id: generateNodeId(),
      ...nodeConfig,
      position: project(position),
      data: nodeConfig.data || {}
    };

    store.addNode(node);
  }

  // Connect two nodes
  function connectNodes(sourceId: string, targetId: string) {
    const edge = {
      id: `${sourceId}-${targetId}`,
      source: sourceId,
      target: targetId,
      type: 'default'
    };

    store.addEdge(edge);
  }

  // Auto-layout nodes
  function autoLayout() {
    const nodes = getNodes.value;
    // Implement auto-layout algorithm (e.g., Dagre)
  }

  // Zoom to fit all nodes
  function fitView() {
    vueFlowRef.value?.fitView({ padding: 0.2 });
  }

  return {
    addNodeAtPosition,
    connectNodes,
    autoLayout,
    fitView,
    project,
    vueFlowRef
  };
}

function generateNodeId(): string {
  return `node_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
}
```

**驗收標準**:
- ✅ VueFlow 畫布成功渲染
- ✅ 支援縮放 (0.2x ~ 4x)
- ✅ 支援拖曳移動
- ✅ Background grid 顯示
- ✅ Controls 和 MiniMap 正常運作

---

#### 1.2 Module Federation 配置

**目標**: 實現 React (Host) 與 Vue (Remote) 整合

**技術實現**:

```typescript
// packages/remote/src/bootstrap.ts
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import ElementPlus from 'element-plus';
import 'element-plus/dist/index.css';
import '@vue-flow/core/dist/style.css';
import '@vue-flow/core/dist/theme-default.css';

import WorkflowEditor from './WorkflowEditor.vue';

export function mount(el: HTMLElement, props: any) {
  const app = createApp(WorkflowEditor, props);
  const pinia = createPinia();

  app.use(pinia);
  app.use(ElementPlus);
  app.mount(el);

  return app;
}

export function unmount(app: any) {
  app.unmount();
}

// Development mode
if (import.meta.env.DEV) {
  const el = document.getElementById('app');
  if (el) {
    mount(el, {
      workflowId: 'dev-workflow-001',
      onSave: (data: any) => console.log('Save:', data),
      onCancel: () => console.log('Cancel')
    });
  }
}
```

```typescript
// packages/host/src/components/WorkflowEditorWrapper.tsx
import React, { useEffect, useRef } from 'react';

interface WorkflowEditorProps {
  workflowId: string;
  onSave: (data: any) => void;
  onCancel: () => void;
}

const WorkflowEditorWrapper: React.FC<WorkflowEditorProps> = ({
  workflowId,
  onSave,
  onCancel
}) => {
  const containerRef = useRef<HTMLDivElement>(null);
  const appRef = useRef<any>(null);

  useEffect(() => {
    let mounted = false;

    const loadEditor = async () => {
      if (mounted) return;

      try {
        const { mount } = await import('workflowEditor/WorkflowEditor');

        if (containerRef.current && !appRef.current) {
          appRef.current = mount(containerRef.current, {
            workflowId,
            onSave,
            onCancel
          });
          mounted = true;
        }
      } catch (error) {
        console.error('Failed to load workflow editor:', error);
      }
    };

    loadEditor();

    return () => {
      if (appRef.current) {
        const { unmount } = require('workflowEditor/WorkflowEditor');
        unmount(appRef.current);
        appRef.current = null;
        mounted = false;
      }
    };
  }, [workflowId, onSave, onCancel]);

  return <div ref={containerRef} style={{ width: '100%', height: '100%' }} />;
};

export default WorkflowEditorWrapper;
```

**驗收標準**:
- ✅ React Host 成功載入 Vue Remote
- ✅ Props 正確傳遞 (workflowId, onSave, onCancel)
- ✅ 生命週期管理正常 (mount/unmount)
- ✅ 無 CORS 錯誤
- ✅ Hot reload 正常運作

---

### 功能 2: Drag-drop Node Editor (3 SP)

#### 2.1 Node Palette 實現

**目標**: 建立節點工具箱,支援拖曳

**技術實現**:

```vue
<!-- packages/remote/src/components/NodePalette.vue -->
<template>
  <aside class="node-palette">
    <div class="palette-header">
      <h3>節點工具箱</h3>
      <el-input
        v-model="searchQuery"
        placeholder="搜尋節點..."
        :prefix-icon="Search"
      />
    </div>

    <el-collapse v-model="activeCategories" accordion>
      <el-collapse-item
        v-for="category in filteredCategories"
        :key="category.name"
        :name="category.name"
        :title="category.label"
      >
        <div class="node-list">
          <div
            v-for="nodeType in category.nodes"
            :key="nodeType.type"
            class="node-item"
            :draggable="true"
            @dragstart="handleDragStart(nodeType, $event)"
            @dragend="handleDragEnd"
          >
            <el-icon class="node-icon">
              <component :is="getIcon(nodeType.icon)" />
            </el-icon>
            <div class="node-info">
              <span class="node-label">{{ nodeType.label }}</span>
              <span class="node-desc">{{ nodeType.description }}</span>
            </div>
          </div>
        </div>
      </el-collapse-item>
    </el-collapse>
  </aside>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { Search } from '@element-plus/icons-vue';
import { useNodeStore } from '@/stores/nodes';
import type { NodeType } from '@/types';

const emit = defineEmits<{
  (e: 'node-drag-start', nodeType: NodeType): void;
  (e: 'node-drag-end', event: DragEvent): void;
}>();

const nodeStore = useNodeStore();
const searchQuery = ref('');
const activeCategories = ref(['execution']);

const categories = computed(() => {
  const cats = new Map<string, { name: string; label: string; nodes: NodeType[] }>();

  nodeStore.nodeTypes.forEach(nodeType => {
    if (!cats.has(nodeType.category)) {
      cats.set(nodeType.category, {
        name: nodeType.category,
        label: getCategoryLabel(nodeType.category),
        nodes: []
      });
    }
    cats.get(nodeType.category)!.nodes.push(nodeType);
  });

  return Array.from(cats.values());
});

const filteredCategories = computed(() => {
  if (!searchQuery.value) return categories.value;

  const query = searchQuery.value.toLowerCase();
  return categories.value
    .map(cat => ({
      ...cat,
      nodes: cat.nodes.filter(
        n => n.label.toLowerCase().includes(query) ||
             n.description.toLowerCase().includes(query)
      )
    }))
    .filter(cat => cat.nodes.length > 0);
});

function handleDragStart(nodeType: NodeType, event: DragEvent) {
  event.dataTransfer!.effectAllowed = 'copy';
  event.dataTransfer!.setData('application/vueflow', JSON.stringify(nodeType));

  emit('node-drag-start', nodeType);
}

function handleDragEnd(event: DragEvent) {
  emit('node-drag-end', event);
}

function getCategoryLabel(category: string): string {
  const labels: Record<string, string> = {
    execution: '執行節點',
    control: '控制節點',
    flow: '流程節點',
    data: '資料節點'
  };
  return labels[category] || category;
}

function getIcon(iconName: string) {
  // Map icon names to Element Plus icons
  const iconMap: Record<string, any> = {
    robot: 'User',
    plugin: 'Grid',
    fork: 'Share',
    play: 'VideoPlay',
    stop: 'VideoPause'
  };
  return iconMap[iconName] || 'QuestionFilled';
}
</script>

<style scoped>
.node-palette {
  width: 280px;
  height: 100%;
  background: #fff;
  border-right: 1px solid #dcdfe6;
  display: flex;
  flex-direction: column;
}

.palette-header {
  padding: 16px;
  border-bottom: 1px solid #dcdfe6;
}

.palette-header h3 {
  margin: 0 0 12px 0;
  font-size: 16px;
  font-weight: 600;
}

.node-list {
  padding: 8px;
}

.node-item {
  display: flex;
  align-items: center;
  padding: 12px;
  margin-bottom: 8px;
  background: #f5f7fa;
  border: 1px solid #e4e7ed;
  border-radius: 4px;
  cursor: move;
  transition: all 0.2s;
}

.node-item:hover {
  background: #ecf5ff;
  border-color: #409eff;
  box-shadow: 0 2px 4px rgba(64, 158, 255, 0.2);
}

.node-item:active {
  opacity: 0.7;
}

.node-icon {
  font-size: 24px;
  margin-right: 12px;
  color: #409eff;
}

.node-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.node-label {
  font-size: 14px;
  font-weight: 500;
  color: #303133;
  margin-bottom: 4px;
}

.node-desc {
  font-size: 12px;
  color: #909399;
}
</style>
```

**驗收標準**:
- ✅ 節點工具箱正確顯示
- ✅ 節點分類摺疊功能
- ✅ 搜尋功能正常
- ✅ 拖曳視覺回饋正確
- ✅ 拖曳事件正確觸發

---

#### 2.2 Drag & Drop 功能

**目標**: 實現從 Palette 拖曳節點到 Canvas

**技術實現**:

```typescript
// packages/remote/src/composables/useDragDrop.ts
import { ref } from 'vue';
import { useVueFlow } from '@vue-flow/core';
import { useWorkflowStore } from '@/stores/workflow';
import { useNodeStore } from '@/stores/nodes';
import type { NodeType } from '@/types';

export function useDragDrop() {
  const { project } = useVueFlow();
  const workflowStore = useWorkflowStore();
  const nodeStore = useNodeStore();

  const isDragging = ref(false);
  const dragNodeType = ref<NodeType | null>(null);

  function onDragStart(nodeType: NodeType) {
    isDragging.value = true;
    dragNodeType.value = nodeType;
  }

  function onDragOver(event: DragEvent) {
    event.preventDefault();

    if (event.dataTransfer) {
      event.dataTransfer.dropEffect = 'copy';
    }
  }

  function onDrop(event: DragEvent) {
    event.preventDefault();

    if (!dragNodeType.value) return;

    const bounds = (event.target as HTMLElement).getBoundingClientRect();
    const position = project({
      x: event.clientX - bounds.left,
      y: event.clientY - bounds.top
    });

    const nodeConfig = nodeStore.createNodeConfig(dragNodeType.value.type);
    const newNode = {
      ...nodeConfig,
      id: generateNodeId(),
      position,
      data: {
        ...nodeConfig.data,
        label: dragNodeType.value.label
      }
    };

    workflowStore.addNode(newNode);

    isDragging.value = false;
    dragNodeType.value = null;
  }

  function onDragEnd() {
    isDragging.value = false;
    dragNodeType.value = null;
  }

  return {
    isDragging,
    dragNodeType,
    onDragStart,
    onDragOver,
    onDrop,
    onDragEnd
  };
}

function generateNodeId(): string {
  return `node_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
}
```

```vue
<!-- WorkflowEditor.vue 中使用 -->
<template>
  <div class="workflow-editor">
    <NodePalette
      @node-drag-start="onDragStart"
      @node-drag-end="onDragEnd"
    />

    <div
      class="canvas-container"
      @dragover="onDragOver"
      @drop="onDrop"
    >
      <VueFlow v-model="elements">
        <!-- ... -->
      </VueFlow>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useDragDrop } from '@/composables/useDragDrop';

const { onDragStart, onDragOver, onDrop, onDragEnd } = useDragDrop();
</script>
```

**驗收標準**:
- ✅ 拖曳節點到畫布成功建立
- ✅ 節點位置計算正確
- ✅ 拖曳游標視覺回饋
- ✅ 無效拖曳目標正確處理
- ✅ 拖曳取消正確處理

---

#### 2.3 Canvas Interaction

**目標**: 實現畫布互動功能

**技術實現**:

```typescript
// packages/remote/src/composables/useCanvasInteraction.ts
import { ref } from 'vue';
import { useVueFlow } from '@vue-flow/core';
import { useWorkflowStore } from '@/stores/workflow';
import type { Node, Edge, Connection } from '@vue-flow/core';

export function useCanvasInteraction() {
  const {
    addEdges,
    removeNodes,
    removeEdges,
    getSelectedNodes,
    getSelectedEdges
  } = useVueFlow();
  const workflowStore = useWorkflowStore();

  // Node selection
  function onNodeClick(event: MouseEvent, node: Node) {
    workflowStore.selectNode(node.id);
  }

  function onPaneClick() {
    workflowStore.selectNode(null);
  }

  // Node drag
  function onNodeDragStop(event: MouseEvent, node: Node) {
    workflowStore.updateNode(node.id, { position: node.position });
  }

  // Edge connection
  function onConnect(connection: Connection) {
    if (!connection.source || !connection.target) return;

    const edge = {
      id: `${connection.source}-${connection.target}`,
      source: connection.source,
      target: connection.target,
      sourceHandle: connection.sourceHandle,
      targetHandle: connection.targetHandle
    };

    workflowStore.addEdge(edge);
  }

  // Edge update
  function onEdgeUpdate(oldEdge: Edge, newConnection: Connection) {
    workflowStore.removeEdge(oldEdge.id);
    onConnect(newConnection);
  }

  // Delete selection
  function deleteSelection() {
    const selectedNodes = getSelectedNodes.value;
    const selectedEdges = getSelectedEdges.value;

    if (selectedNodes.length > 0) {
      selectedNodes.forEach(node => {
        workflowStore.removeNode(node.id);
      });
    }

    if (selectedEdges.length > 0) {
      selectedEdges.forEach(edge => {
        workflowStore.removeEdge(edge.id);
      });
    }
  }

  // Keyboard shortcuts
  function onKeyDown(event: KeyboardEvent) {
    if (event.key === 'Delete' || event.key === 'Backspace') {
      deleteSelection();
    } else if (event.ctrlKey || event.metaKey) {
      switch (event.key) {
        case 's':
          event.preventDefault();
          workflowStore.saveWorkflow();
          break;
        case 'z':
          event.preventDefault();
          // Undo
          break;
        case 'y':
          event.preventDefault();
          // Redo
          break;
      }
    }
  }

  return {
    onNodeClick,
    onPaneClick,
    onNodeDragStop,
    onConnect,
    onEdgeUpdate,
    deleteSelection,
    onKeyDown
  };
}
```

**驗收標準**:
- ✅ 節點點擊選取
- ✅ 畫布點擊取消選取
- ✅ 節點拖曳移動
- ✅ 連線建立
- ✅ Delete 鍵刪除節點/連線
- ✅ Ctrl+S 儲存快捷鍵

---

### 功能 3: 基礎節點類型 (2 SP)

#### 3.1 Agent Node

**目標**: 實現 Agent 執行節點

**技術實現**:

```vue
<!-- packages/remote/src/components/nodes/AgentNode.vue -->
<template>
  <div
    class="agent-node"
    :class="{ selected: isSelected, error: hasError }"
  >
    <Handle
      type="target"
      :position="Position.Left"
      :is-connectable="isConnectable"
    />

    <div class="node-header">
      <el-icon class="node-icon"><User /></el-icon>
      <span class="node-title">{{ data.label || 'Agent' }}</span>
      <el-dropdown @command="handleCommand">
        <el-icon class="node-menu"><MoreFilled /></el-icon>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item command="edit">編輯</el-dropdown-item>
            <el-dropdown-item command="duplicate">複製</el-dropdown-item>
            <el-dropdown-item command="delete" divided>刪除</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>

    <div class="node-body">
      <div v-if="data.agentId" class="node-field">
        <span class="field-label">Agent:</span>
        <span class="field-value">{{ data.agentName }}</span>
      </div>
      <div v-else class="node-placeholder">
        請選擇 Agent
      </div>

      <div v-if="data.input" class="node-field">
        <span class="field-label">輸入:</span>
        <span class="field-value">{{ truncate(data.input, 30) }}</span>
      </div>
    </div>

    <div v-if="hasError" class="node-error">
      <el-icon><WarningFilled /></el-icon>
      <span>{{ errorMessage }}</span>
    </div>

    <Handle
      type="source"
      :position="Position.Right"
      :is-connectable="isConnectable"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { Handle, Position } from '@vue-flow/core';
import { User, MoreFilled, WarningFilled } from '@element-plus/icons-vue';
import { useWorkflowStore } from '@/stores/workflow';

interface AgentNodeData {
  label?: string;
  agentId?: string;
  agentName?: string;
  input?: string;
  error?: string;
}

interface Props {
  id: string;
  data: AgentNodeData;
  selected?: boolean;
}

const props = defineProps<Props>();
const store = useWorkflowStore();

const isSelected = computed(() => props.selected);
const hasError = computed(() => !!props.data.error);
const errorMessage = computed(() => props.data.error || '');
const isConnectable = computed(() => true);

function handleCommand(command: string) {
  switch (command) {
    case 'edit':
      // Open edit dialog
      break;
    case 'duplicate':
      // Duplicate node
      break;
    case 'delete':
      store.removeNode(props.id);
      break;
  }
}

function truncate(text: string, length: number): string {
  return text.length > length ? text.substring(0, length) + '...' : text;
}
</script>

<style scoped>
.agent-node {
  min-width: 200px;
  background: white;
  border: 2px solid #409eff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.2s;
}

.agent-node:hover {
  box-shadow: 0 4px 12px rgba(64, 158, 255, 0.3);
}

.agent-node.selected {
  border-color: #0066cc;
  box-shadow: 0 4px 16px rgba(0, 102, 204, 0.4);
}

.agent-node.error {
  border-color: #f56c6c;
}

.node-header {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  background: #409eff;
  color: white;
  border-radius: 6px 6px 0 0;
}

.node-icon {
  font-size: 18px;
  margin-right: 8px;
}

.node-title {
  flex: 1;
  font-weight: 500;
  font-size: 14px;
}

.node-menu {
  cursor: pointer;
  opacity: 0.8;
}

.node-menu:hover {
  opacity: 1;
}

.node-body {
  padding: 12px;
}

.node-field {
  margin-bottom: 8px;
  font-size: 12px;
}

.node-field:last-child {
  margin-bottom: 0;
}

.field-label {
  color: #909399;
  margin-right: 4px;
}

.field-value {
  color: #303133;
  font-weight: 500;
}

.node-placeholder {
  color: #c0c4cc;
  font-size: 12px;
  font-style: italic;
}

.node-error {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  background: #fef0f0;
  color: #f56c6c;
  font-size: 12px;
  border-radius: 0 0 6px 6px;
}

.node-error .el-icon {
  margin-right: 4px;
}
</style>
```

**驗收標準**:
- ✅ Agent 節點正確渲染
- ✅ 顯示 Agent 名稱和輸入
- ✅ Handle (連接點) 正常運作
- ✅ 選取狀態視覺回饋
- ✅ 錯誤狀態顯示
- ✅ 右鍵選單功能

---

#### 3.2 Plugin Node

**目標**: 實現 Plugin 執行節點

**技術實現**:

```vue
<!-- packages/remote/src/components/nodes/PluginNode.vue -->
<template>
  <div
    class="plugin-node"
    :class="{ selected: isSelected, error: hasError }"
  >
    <Handle
      type="target"
      :position="Position.Left"
      :is-connectable="isConnectable"
    />

    <div class="node-header">
      <el-icon class="node-icon"><Grid /></el-icon>
      <span class="node-title">{{ data.label || 'Plugin' }}</span>
      <el-dropdown @command="handleCommand">
        <el-icon class="node-menu"><MoreFilled /></el-icon>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item command="edit">編輯</el-dropdown-item>
            <el-dropdown-item command="duplicate">複製</el-dropdown-item>
            <el-dropdown-item command="delete" divided>刪除</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>

    <div class="node-body">
      <div v-if="data.pluginId" class="node-field">
        <span class="field-label">Plugin:</span>
        <span class="field-value">{{ data.pluginName }}</span>
      </div>
      <div v-else class="node-placeholder">
        請選擇 Plugin
      </div>

      <div v-if="data.parameters" class="node-field">
        <span class="field-label">參數:</span>
        <span class="field-value">{{ parameterCount }} 個</span>
      </div>
    </div>

    <Handle
      type="source"
      :position="Position.Right"
      :is-connectable="isConnectable"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { Handle, Position } from '@vue-flow/core';
import { Grid, MoreFilled } from '@element-plus/icons-vue';
import { useWorkflowStore } from '@/stores/workflow';

interface PluginNodeData {
  label?: string;
  pluginId?: string;
  pluginName?: string;
  parameters?: Record<string, any>;
  error?: string;
}

interface Props {
  id: string;
  data: PluginNodeData;
  selected?: boolean;
}

const props = defineProps<Props>();
const store = useWorkflowStore();

const isSelected = computed(() => props.selected);
const hasError = computed(() => !!props.data.error);
const isConnectable = computed(() => true);
const parameterCount = computed(() =>
  props.data.parameters ? Object.keys(props.data.parameters).length : 0
);

function handleCommand(command: string) {
  switch (command) {
    case 'edit':
      // Open edit dialog
      break;
    case 'duplicate':
      // Duplicate node
      break;
    case 'delete':
      store.removeNode(props.id);
      break;
  }
}
</script>

<style scoped>
.plugin-node {
  min-width: 200px;
  background: white;
  border: 2px solid #67c23a;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.2s;
}

.plugin-node:hover {
  box-shadow: 0 4px 12px rgba(103, 194, 58, 0.3);
}

.plugin-node.selected {
  border-color: #529b2e;
  box-shadow: 0 4px 16px rgba(82, 155, 46, 0.4);
}

.node-header {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  background: #67c23a;
  color: white;
  border-radius: 6px 6px 0 0;
}

/* 其他樣式與 AgentNode 相似 */
</style>
```

**驗收標準**:
- ✅ Plugin 節點正確渲染
- ✅ 顯示 Plugin 名稱和參數數量
- ✅ 與 Agent Node 視覺區隔 (綠色)
- ✅ Handle 正常運作
- ✅ 右鍵選單功能

---

#### 3.3 Decision Node

**目標**: 實現條件分支節點

**技術實現**:

```vue
<!-- packages/remote/src/components/nodes/DecisionNode.vue -->
<template>
  <div
    class="decision-node"
    :class="{ selected: isSelected }"
  >
    <Handle
      type="target"
      :position="Position.Left"
      :is-connectable="isConnectable"
    />

    <div class="node-content">
      <el-icon class="node-icon"><Share /></el-icon>
      <span class="node-title">{{ data.label || 'Decision' }}</span>
    </div>

    <div v-if="data.condition" class="node-condition">
      {{ truncate(data.condition, 30) }}
    </div>

    <!-- True branch -->
    <Handle
      id="true"
      type="source"
      :position="Position.Bottom"
      :style="{ left: '30%' }"
      :is-connectable="isConnectable"
    >
      <span class="handle-label">True</span>
    </Handle>

    <!-- False branch -->
    <Handle
      id="false"
      type="source"
      :position="Position.Bottom"
      :style="{ left: '70%' }"
      :is-connectable="isConnectable"
    >
      <span class="handle-label">False</span>
    </Handle>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { Handle, Position } from '@vue-flow/core';
import { Share } from '@element-plus/icons-vue';

interface DecisionNodeData {
  label?: string;
  condition?: string;
}

interface Props {
  id: string;
  data: DecisionNodeData;
  selected?: boolean;
}

const props = defineProps<Props>();

const isSelected = computed(() => props.selected);
const isConnectable = computed(() => true);

function truncate(text: string, length: number): string {
  return text.length > length ? text.substring(0, length) + '...' : text;
}
</script>

<style scoped>
.decision-node {
  min-width: 150px;
  background: white;
  border: 2px solid #e6a23c;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 12px;
  text-align: center;
  position: relative;
}

.decision-node.selected {
  border-color: #cf9236;
  box-shadow: 0 4px 16px rgba(207, 146, 54, 0.4);
}

.node-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 8px;
}

.node-icon {
  font-size: 24px;
  color: #e6a23c;
  margin-bottom: 4px;
}

.node-title {
  font-weight: 500;
  font-size: 14px;
  color: #303133;
}

.node-condition {
  font-size: 12px;
  color: #606266;
  padding: 4px 8px;
  background: #fdf6ec;
  border-radius: 4px;
  margin-bottom: 16px;
}

.handle-label {
  position: absolute;
  top: -20px;
  font-size: 10px;
  color: #909399;
  white-space: nowrap;
}
</style>
```

**驗收標準**:
- ✅ Decision 節點正確渲染
- ✅ 菱形視覺風格
- ✅ 雙輸出 Handle (True/False)
- ✅ 條件顯示
- ✅ Handle 標籤顯示

---

#### 3.4 Start/End Node

**目標**: 實現工作流起點/終點節點

**技術實現**:

```vue
<!-- packages/remote/src/components/nodes/StartEndNode.vue -->
<template>
  <div
    class="start-end-node"
    :class="[nodeClass, { selected: isSelected }]"
  >
    <div class="node-content">
      <el-icon class="node-icon">
        <VideoPlay v-if="type === 'start'" />
        <VideoPause v-else />
      </el-icon>
      <span class="node-title">{{ nodeTitle }}</span>
    </div>

    <Handle
      v-if="type === 'start'"
      type="source"
      :position="Position.Right"
      :is-connectable="isConnectable"
    />

    <Handle
      v-if="type === 'end'"
      type="target"
      :position="Position.Left"
      :is-connectable="isConnectable"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { Handle, Position } from '@vue-flow/core';
import { VideoPlay, VideoPause } from '@element-plus/icons-vue';

interface Props {
  id: string;
  data: any;
  type: 'start' | 'end';
  selected?: boolean;
}

const props = defineProps<Props>();

const isSelected = computed(() => props.selected);
const isConnectable = computed(() => true);

const nodeClass = computed(() =>
  props.type === 'start' ? 'start-node' : 'end-node'
);

const nodeTitle = computed(() =>
  props.type === 'start' ? '開始' : '結束'
);
</script>

<style scoped>
.start-end-node {
  min-width: 120px;
  background: white;
  border: 2px solid;
  border-radius: 30px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 12px 20px;
  text-align: center;
}

.start-node {
  border-color: #67c23a;
}

.end-node {
  border-color: #f56c6c;
}

.start-end-node.selected {
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
}

.node-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.node-icon {
  font-size: 32px;
  margin-bottom: 4px;
}

.start-node .node-icon {
  color: #67c23a;
}

.end-node .node-icon {
  color: #f56c6c;
}

.node-title {
  font-weight: 500;
  font-size: 14px;
  color: #303133;
}
</style>
```

**驗收標準**:
- ✅ Start 節點正確渲染 (綠色圓形)
- ✅ End 節點正確渲染 (紅色圓形)
- ✅ 正確的 Handle 配置 (Start 只有 source, End 只有 target)
- ✅ 視覺區隔明確
- ✅ 不可刪除 (至少各一個)

---

## Phase 1D 啟動技術準備

### Phase 1D 技術棧確認

**Frontend 技術棧**:
```yaml
Core:
  - Vue 3.3+ (Composition API)
  - TypeScript 5.2+
  - Vite 5.0+

Workflow Engine:
  - VueFlow 1.45.0
  - @vue-flow/core
  - @vue-flow/background
  - @vue-flow/controls
  - @vue-flow/minimap

State Management:
  - Pinia 2.1+
  - VueUse 10.0+

UI Framework:
  - Element Plus 2.4+
  - Custom Components

Micro-Frontend:
  - Module Federation (Vite)
  - @originjs/vite-plugin-federation

Testing:
  - Vitest (unit tests)
  - Playwright (E2E tests)

Build & Dev:
  - Vite 5 (dev server, HMR)
  - TypeScript compiler
  - ESLint + Prettier
```

### Phase 1C → Phase 1D 技術銜接

**API 整合準備**:
```typescript
// packages/remote/src/api/workflow.api.ts
import axios from 'axios';
import type { WorkflowDefinition } from '@/types';

const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL || 'http://localhost:5095/api',
  headers: {
    'Content-Type': 'application/json'
  }
});

export const workflowApi = {
  // Get workflow definition
  async get(id: string): Promise<WorkflowDefinition> {
    const response = await apiClient.get(`/workflows/${id}`);
    return response.data;
  },

  // List workflows
  async list(params?: { page?: number; pageSize?: number }) {
    const response = await apiClient.get('/workflows', { params });
    return response.data;
  },

  // Create workflow
  async create(workflow: Partial<WorkflowDefinition>) {
    const response = await apiClient.post('/workflows', workflow);
    return response.data;
  },

  // Update workflow
  async update(id: string, workflow: Partial<WorkflowDefinition>) {
    const response = await apiClient.put(`/workflows/${id}`, workflow);
    return response.data;
  },

  // Delete workflow
  async delete(id: string) {
    await apiClient.delete(`/workflows/${id}`);
  },

  // Validate workflow
  async validate(workflow: WorkflowDefinition) {
    const response = await apiClient.post('/workflows/validate', workflow);
    return response.data;
  }
};
```

### 環境設定

**Development Environment**:
```env
# packages/remote/.env.development
VITE_API_BASE_URL=http://localhost:5095/api
VITE_HOST_URL=http://localhost:3000
VITE_REMOTE_PORT=3001
```

**Production Environment**:
```env
# packages/remote/.env.production
VITE_API_BASE_URL=https://api.example.com/api
VITE_HOST_URL=https://app.example.com
VITE_REMOTE_PORT=3001
```

### Phase 1D 驗收標準

**技術驗收**:
- ✅ Vue 3 開發環境建立
- ✅ VueFlow 成功整合
- ✅ Module Federation 運作
- ✅ Pinia state management 設定
- ✅ Element Plus UI 套用
- ✅ TypeScript 編譯無錯誤
- ✅ ESLint/Prettier 規則套用
- ✅ Vite dev server HMR 正常

**功能驗收**:
- ✅ 畫布渲染
- ✅ 節點拖曳
- ✅ 連線建立
- ✅ 基礎節點類型 (5 種)
- ✅ 狀態管理
- ✅ API 整合準備

---

## 技術決策記錄

### TD-088: Module Federation with Vite

**日期**: 2026-06-02

**狀態**: ✅ 已採納

**背景**:
需要整合 React (Host) 和 Vue (Remote) 應用,實現微前端架構。

**決策**:
採用 Vite 的 Module Federation plugin (`@originjs/vite-plugin-federation`)

**理由**:
1. **Vite 原生支援**: 與 Vite build tool 無縫整合
2. **開發體驗**: 支援 HMR,開發速度快
3. **輕量級**: 相比 Webpack Module Federation 更輕量
4. **TypeScript 支援**: 完整的 TypeScript 類型支援

**替代方案**:
- Webpack Module Federation: 配置複雜,build 速度慢
- Single-SPA: 需要額外的 framework adapter
- iframe: 隔離性太強,通訊複雜

**影響**:
- 需要調整 build 配置
- 需要處理 shared dependencies
- 需要實現 mount/unmount lifecycle

**緩解措施**:
- 詳細文檔化配置過程
- 建立 shared dependencies 清單
- 實現錯誤邊界處理

---

### TD-089: VueFlow as Canvas Engine

**日期**: 2026-06-02

**狀態**: ✅ 已採納

**背景**:
需要選擇工作流編輯器的畫布引擎,參考 ADR-012。

**決策**:
採用 VueFlow 1.45.0 作為畫布引擎

**理由**:
1. **Vue 3 原生**: 完全基於 Vue 3 Composition API
2. **功能完整**: 節點、連線、拖曳、縮放等核心功能
3. **自定義能力**: 支援自定義節點、邊、控制項
4. **活躍維護**: 社群活躍,定期更新
5. **文檔完善**: 官方文檔和範例豐富
6. **效能良好**: 支援大型工作流 (500+ 節點)

**替代方案**:
- React Flow: 需要 React,不適合 Vue 專案
- X6 (AntV): 功能過於複雜,學習曲線陡
- GoJS: 商業授權,成本高

**影響**:
- 需要學習 VueFlow API
- 需要實現自定義節點
- 需要處理 VueFlow 與 Pinia 同步

**緩解措施**:
- 建立 VueFlow composables 封裝
- 詳細文檔化節點開發流程
- 實現完整的範例節點

**參考**:
- ADR-012: Workflow Editor Technology Selection

---

### TD-090: Pinia for State Management

**日期**: 2026-06-02

**狀態**: ✅ 已採納

**背景**:
Vue 3 應用需要狀態管理解決方案。

**決策**:
採用 Pinia 2.1+ 作為狀態管理庫

**理由**:
1. **Vue 3 官方推薦**: Vue 團隊推薦的狀態管理方案
2. **Composition API**: 完全基於 Composition API,語法現代
3. **TypeScript 支援**: 完整的 TypeScript 類型推導
4. **DevTools 整合**: 支援 Vue DevTools
5. **模組化**: 自然支援多個 stores
6. **輕量級**: 相比 Vuex 更輕量,API 更簡潔

**替代方案**:
- Vuex: 較舊的方案,API 複雜
- VueUse: 僅提供 composables,非完整狀態管理

**影響**:
- 需要設計 store 結構
- 需要處理 store 持久化
- 需要實現 store 與 VueFlow 同步

**緩解措施**:
- 建立清晰的 store 分層 (workflow, nodes, edges)
- 使用 watch 實現雙向同步
- 實現 store 單元測試

---

### TD-091: Element Plus as UI Framework

**日期**: 2026-06-02

**狀態**: ✅ 已採納

**背景**:
需要選擇 Vue 3 UI 元件庫。

**決策**:
採用 Element Plus 2.4+ 作為 UI 框架

**理由**:
1. **Vue 3 原生**: 專為 Vue 3 設計
2. **元件豐富**: 60+ 高品質元件
3. **企業級**: 適合企業應用,穩定可靠
4. **可定制**: 支援主題定制
5. **國際化**: 完整的 i18n 支援
6. **文檔完善**: 中英文文檔齊全

**替代方案**:
- Ant Design Vue: 元件較多,bundle size 大
- Naive UI: 較新,生態不夠成熟
- Quasar: 功能過於龐大

**影響**:
- 需要引入 Element Plus 樣式
- 需要配置主題
- 需要處理與自定義元件的樣式衝突

**緩解措施**:
- 按需引入,減少 bundle size
- 建立統一的 theme 配置
- 使用 scoped CSS 避免衝突

---

### TD-092: Vitest for Unit Testing

**日期**: 2026-06-02

**狀態**: ✅ 已採納

**背景**:
需要選擇 Vue 3 單元測試框架。

**決策**:
採用 Vitest 作為單元測試框架

**理由**:
1. **Vite 原生**: 與 Vite 無縫整合
2. **快速**: 基於 Vite,測試執行速度快
3. **Jest 相容**: API 與 Jest 相容,易於遷移
4. **Vue 支援**: 完整支援 Vue 3 測試
5. **Watch Mode**: 優秀的 watch mode 體驗

**替代方案**:
- Jest: 需要額外配置 transform,速度較慢
- Mocha: API 較舊,需要額外的 assertion library

**影響**:
- 需要配置 Vitest
- 需要學習 Vue Testing Library
- 需要撰寫測試用例

**緩解措施**:
- 建立測試範例和 best practices
- 設定 code coverage 目標 (80%)
- 整合 CI/CD pipeline

---

### TD-093: Playwright for E2E Testing

**日期**: 2026-06-02

**狀態**: ✅ 已採納

**背景**:
需要選擇 E2E 測試框架。

**決策**:
採用 Playwright 作為 E2E 測試框架

**理由**:
1. **跨瀏覽器**: 支援 Chromium, Firefox, WebKit
2. **現代化**: 專為現代 Web 應用設計
3. **可靠性**: 自動等待,減少 flaky tests
4. **並行執行**: 支援平行執行,速度快
5. **DevTools**: 強大的 debugging 工具

**替代方案**:
- Cypress: 僅支援 Chromium-based browsers
- Selenium: 較舊,API 複雜

**影響**:
- 需要撰寫 E2E 測試腳本
- 需要設定測試環境
- 需要整合 CI/CD

**緩解措施**:
- 建立 E2E 測試範例
- 使用 Page Object Model 模式
- 設定 test retry 機制

---

## 依賴與整合

### Frontend 依賴清單

```json
{
  "dependencies": {
    "vue": "^3.3.0",
    "@vue-flow/core": "^1.45.0",
    "@vue-flow/background": "^1.3.0",
    "@vue-flow/controls": "^1.1.0",
    "@vue-flow/minimap": "^1.4.0",
    "pinia": "^2.1.0",
    "@vueuse/core": "^10.0.0",
    "element-plus": "^2.4.0",
    "@element-plus/icons-vue": "^2.1.0",
    "axios": "^1.6.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.0",
    "@originjs/vite-plugin-federation": "^1.3.0",
    "vite": "^5.0.0",
    "typescript": "^5.2.0",
    "vitest": "^1.0.0",
    "@vue/test-utils": "^2.4.0",
    "@playwright/test": "^1.40.0",
    "eslint": "^8.50.0",
    "prettier": "^3.0.0"
  }
}
```

### 與 Phase 1C 整合

**API 端點對接**:
```typescript
// Sprint 10-11 (Phase 1C) 提供的 API
GET    /api/workflows              # 列表
GET    /api/workflows/:id          # 詳情
POST   /api/workflows              # 建立
PUT    /api/workflows/:id          # 更新
DELETE /api/workflows/:id          # 刪除
POST   /api/workflows/validate     # 驗證
POST   /api/workflows/:id/execute  # 執行

// Sprint 12 (Phase 1D) 整合
- 列表頁: GET /api/workflows
- 編輯器: GET /api/workflows/:id
- 儲存: PUT /api/workflows/:id
- 驗證: POST /api/workflows/validate
```

**資料格式對接**:
```typescript
// Backend DTO (from Phase 1C)
interface WorkflowDefinitionDto {
  id: string;
  name: string;
  description?: string;
  definition: string; // JSON string
  isActive: boolean;
  version: number;
  createdAt: string;
  updatedAt: string;
}

// Frontend Model (Phase 1D)
interface WorkflowDefinition {
  id: string;
  name: string;
  description?: string;
  nodes: Node[];
  edges: Edge[];
  isActive: boolean;
  version: number;
  createdAt: Date;
  updatedAt: Date;
}

// Transformer
function fromDto(dto: WorkflowDefinitionDto): WorkflowDefinition {
  const definition = JSON.parse(dto.definition);
  return {
    ...dto,
    nodes: definition.nodes || [],
    edges: definition.edges || [],
    createdAt: new Date(dto.createdAt),
    updatedAt: new Date(dto.updatedAt)
  };
}

function toDto(model: WorkflowDefinition): WorkflowDefinitionDto {
  return {
    ...model,
    definition: JSON.stringify({
      nodes: model.nodes,
      edges: model.edges
    }),
    createdAt: model.createdAt.toISOString(),
    updatedAt: model.updatedAt.toISOString()
  };
}
```

### 與 Host Application 整合

**路由整合**:
```typescript
// packages/host/src/routes.tsx
import WorkflowEditorWrapper from '@/components/WorkflowEditorWrapper';

const routes = [
  // ... other routes
  {
    path: '/workflows/:id/edit',
    element: <WorkflowEditorWrapper />
  }
];
```

**狀態整合**:
```typescript
// packages/host/src/App.tsx
import { useNavigate } from 'react-router-dom';

function WorkflowPage() {
  const navigate = useNavigate();
  const { workflowId } = useParams();

  const handleSave = async (data: any) => {
    // Save workflow
    await workflowApi.update(workflowId, data);
    navigate('/workflows');
  };

  const handleCancel = () => {
    navigate('/workflows');
  };

  return (
    <WorkflowEditorWrapper
      workflowId={workflowId}
      onSave={handleSave}
      onCancel={handleCancel}
    />
  );
}
```

---

## 參考文檔

### 架構文檔
1. `docs/architecture/Architecture-Design-Document.md` - 系統架構
2. `docs/architecture/adr/ADR-012-workflow-editor-technology.md` - 工作流編輯器技術選型

### User Stories
3. `docs/user-stories/modules/module-05-workflow-management/US-5.3-workflow-editor.md`
4. `docs/user-stories/modules/module-05-workflow-management/US-5.4-node-library.md`

### UX 設計
5. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part1.md`
6. `docs/ux-design/wireframes/low-fidelity/10-workflow-editor-v2-part2.md`

### Sprint 文檔
7. `claudedocs/2-sprints/sprint-12/SPRINT-12-1-OVERVIEW.md`
8. `claudedocs/2-sprints/sprint-12/SPRINT-12-2-PLAN.md`

### 技術文檔
9. `docs/technical-implementation/1-poc-validation/poc-06-vueflow-crdt-collaboration.md`
10. VueFlow 官方文檔: https://vueflow.dev/
11. Pinia 官方文檔: https://pinia.vuejs.org/
12. Element Plus 官方文檔: https://element-plus.org/
13. Module Federation 文檔: https://github.com/originjs/vite-plugin-federation

### Phase 1C 參考
14. `claudedocs/2-sprints/sprint-10/SPRINT-10-3-CONTEXT.md`
15. `claudedocs/2-sprints/sprint-10/SPRINT-10-7-RETROSPECTIVE.md`
16. `claudedocs/2-sprints/sprint-11/SPRINT-11-3-CONTEXT.md`
17. `claudedocs/2-sprints/sprint-11/SPRINT-11-7-RETROSPECTIVE.md`

### 程式碼範例
18. VueFlow Examples: https://vueflow.dev/examples/
19. Module Federation Examples: https://github.com/originjs/vite-plugin-federation/tree/main/examples

### 測試文檔
20. Vitest 官方文檔: https://vitest.dev/
21. Vue Testing Library: https://testing-library.com/docs/vue-testing-library/intro/
22. Playwright 官方文檔: https://playwright.dev/

### API 文檔
23. `docs/api/workflow-api.md` (Phase 1C API 規格)
24. Axios 文檔: https://axios-http.com/

### 開發工具
25. Vite 官方文檔: https://vitejs.dev/
26. TypeScript 官方文檔: https://www.typescriptlang.org/
27. ESLint 配置指南: https://eslint.org/docs/latest/
28. Prettier 配置指南: https://prettier.io/docs/en/

### Phase 轉換
29. Phase 1C Retrospective (Sprint 11 完成報告)
30. Phase 1D Planning (Sprint 12-14 規劃)

### 設計系統
31. `docs/ux-design/design-system/design-tokens.md`
32. `docs/ux-design/design-system/component-library.md`

### 其他
33. Vue 3 官方文檔: https://vuejs.org/
34. Composition API RFC: https://github.com/vuejs/rfcs/blob/master/active-rfcs/0013-composition-api.md
35. Vue 3 Migration Guide: https://v3-migration.vuejs.org/

---

**文檔維護**:
- 建立日期: 2025-11-14
- 最後更新: 2025-11-14
- 維護者: Development Team
- 審核者: Tech Lead, Architect

**變更歷史**:
| 日期 | 版本 | 變更內容 | 變更人 |
|------|------|----------|--------|
| 2025-11-14 | v2.1 | 初始建立,Phase 1D 啟動 | Development Team |

**下一步行動**:
1. ✅ 完成 SPRINT-12-4-CHECKLIST.md
2. ✅ 完成 SPRINT-12-5-DEV-LOG.md
3. ✅ 完成 SPRINT-12-6-ISSUES.md
4. ✅ 完成 SPRINT-12-7-RETROSPECTIVE.md
5. 🔄 開始 Sprint 12 實施 (2026-06-02)

---

**文檔狀態**: ✅ 完成
