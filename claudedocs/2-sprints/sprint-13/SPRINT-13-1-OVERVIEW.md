# Sprint 13 Overview - 28 種節點類型與配置面板

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

**本 Sprint 文檔集** (7 個文件):
1. **SPRINT-13-1-OVERVIEW.md** (本文件) - Sprint 總覽與定位
2. **SPRINT-13-2-PLAN.md** - 詳細實施計劃
3. **SPRINT-13-3-CONTEXT.md** - 節點系統與配置技術上下文
4. **SPRINT-13-4-CHECKLIST.md** - 175+ 任務清單
5. **SPRINT-13-5-DEV-LOG.md** - 3 週開發日誌
6. **SPRINT-13-6-ISSUES.md** - 問題追蹤與解決方案
7. **SPRINT-13-7-RETROSPECTIVE.md** - Sprint 回顧與評估

**相關 Sprint**:
- **Sprint 11**: Canvas 核心與拖放 (Week 31-33)
- **Sprint 12**: 基礎節點與校驗 (Week 34-36) ← 前一個 Sprint
- **Sprint 13**: 28 種節點類型與配置面板 (本 Sprint)
- **Sprint 14**: 工作流執行與調試 (Week 40-42) → 下一個 Sprint

**Phase 1D 完整規劃**:
- `claudedocs/1-planning/PHASE-1D-WORKFLOW-EDITOR-PLAN.md` - Phase 總體計劃

**需求與設計文檔**:
- `docs/user-stories/modules/module-09-workflow-editor.md`
- `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part4.md` (28 種節點)
- `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part2.md` (內聯配置)

---

## 目錄

1. [Sprint 定位與背景](#sprint-定位與背景)
2. [Sprint 目標](#sprint-目標)
3. [核心交付成果](#核心交付成果)
4. [User Stories 分配](#user-stories-分配)
5. [技術範圍](#技術範圍)
6. [技術決策清單](#技術決策清單)
7. [風險與依賴](#風險與依賴)
8. [成功指標](#成功指標)
9. [Phase 1D 進度追蹤](#phase-1d-進度追蹤)
10. [完整參考文獻索引](#完整參考文獻索引)

---

## Sprint 定位與背景

### 在 Phase 1D 中的位置

```yaml
Phase 1D: 工作流編輯器 Frontend
├── Sprint 11 (Week 31-33): Canvas 核心與拖放         [✅ 已完成, 17%]
├── Sprint 12 (Week 34-36): 基礎節點與校驗           [✅ 已完成, 33%]
├── Sprint 13 (Week 37-39): 28 種節點類型與配置面板  [🎯 本 Sprint, +33%]
├── Sprint 14 (Week 40-42): 工作流執行與調試         [📋 計劃中, +17%]
└── Sprint 15 (Week 43-45): 協作與發佈              [📋 計劃中, +100%]

Phase 1D 累計進度: 50% → 83% (本 Sprint 完成後)
```

### Sprint 13 核心特色

**主題**: **28 種節點類型 + 配置面板 = 完整工作流設計能力**

1. **節點類型完整性** (28 種)
   - 基礎節點 (5 種): Agent, Plugin, Decision, Start, End
   - 控制流節點 (6 種): If-Else, Switch, Loop, While, For-Each, Parallel
   - 數據處理節點 (8 種): Transform, Filter, Map, Reduce, Join, Split, Merge, Aggregate
   - 整合節點 (5 種): API Call, Database Query, File I/O, Email, Webhook
   - 進階節點 (4 種): SubWorkflow, Human-in-Loop, Code Execution, Custom

2. **配置能力** (2 種模式)
   - 內聯配置 (In-place editing)
   - 側邊欄配置 (Sidebar panel)

3. **連線繪製**
   - Bezier 曲線渲染
   - 連線規則驗證
   - 連線動畫效果

### 為什麼是 Sprint 13

**前置條件** (Sprint 11-12 已完成):
- ✅ Canvas 核心 (VueFlow 1.45.0)
- ✅ 拖放系統 (Drag & Drop)
- ✅ 5 種基礎節點
- ✅ Connection validation

**後續需求** (Sprint 14-15 需要):
- 📋 工作流執行 → 需要完整節點類型
- 📋 調試工具 → 需要節點配置面板
- 📋 協作編輯 → 需要完整節點系統

**技術準備度**: 95%
- VueFlow 1.45.0 已驗證
- 節點註冊系統已建立
- 配置系統架構已設計

---

## Sprint 目標

### 主要目標 (Must-Have)

```yaml
Goal 1: 實作 28 種節點類型 (4 SP)
  - 基礎節點 5 種 (100%)
  - 控制流節點 6 種 (100%)
  - 數據處理節點 8 種 (100%)
  - 整合節點 5 種 (100%)
  - 進階節點 4 種 (100%)
  成功標準:
    - 所有節點可拖放到畫布
    - 所有節點可正確連線
    - 所有節點有預設配置
    - 測試覆蓋率 ≥85%

Goal 2: Node Configuration Panel (3 SP)
  - 內聯配置實作 (100%)
  - 側邊欄配置實作 (100%)
  - 屬性驗證系統 (100%)
  - 實時預覽功能 (100%)
  成功標準:
    - 所有節點可配置
    - 配置即時生效
    - 驗證錯誤提示
    - 配置持久化

Goal 3: Connection Line Drawing (1 SP)
  - Bezier 曲線渲染 (100%)
  - Connection Rules 驗證 (100%)
  - Port Highlighting (100%)
  - Connection Animation (100%)
  成功標準:
    - 連線流暢繪製
    - 規則正確驗證
    - 視覺反饋清晰
    - 效能符合標準
```

### 次要目標 (Should-Have)

```yaml
Goal 4: 28 種節點測試 (包含在 Goal 1)
  - 單元測試 28 個節點
  - 整合測試節點互動
  - E2E 測試工作流建立

Goal 5: 配置面板測試 (包含在 Goal 2)
  - 配置模式切換測試
  - 驗證邏輯測試
  - 預覽功能測試

Goal 6: 文檔與示範 (0.5 SP)
  - 28 種節點使用文檔
  - 配置面板指南
  - 工作流範例
```

### 延伸目標 (Nice-to-Have)

```yaml
Goal 7: 進階節點功能
  - Custom Node Builder (視時間而定)
  - Node Template 系統 (視時間而定)
  - Node Grouping (延後到 Sprint 15)
```

---

## 核心交付成果

### 1. 28 種節點類型實作

**基礎節點 (5 種)**:
```typescript
// packages/remote/src/components/nodes/basic/

1. AgentNode.vue
   - Icon: 🤖
   - Inputs: [context, parameters]
   - Outputs: [result, error]
   - Configuration: agentId, persona, temperature

2. PluginNode.vue
   - Icon: 🔌
   - Inputs: [input]
   - Outputs: [output, error]
   - Configuration: pluginId, method, parameters

3. DecisionNode.vue
   - Icon: 💭
   - Inputs: [condition]
   - Outputs: [true, false]
   - Configuration: conditionType, expression

4. StartNode.vue
   - Icon: ▶️
   - Inputs: []
   - Outputs: [start]
   - Configuration: triggerType, schedule

5. EndNode.vue
   - Icon: ⏹️
   - Inputs: [result]
   - Outputs: []
   - Configuration: outputType, notification
```

**控制流節點 (6 種)**:
```typescript
// packages/remote/src/components/nodes/control/

6. IfElseNode.vue
   - Icon: 🔀
   - Inputs: [condition]
   - Outputs: [true, false]
   - Configuration: conditionExpression

7. SwitchNode.vue
   - Icon: 🎛️
   - Inputs: [value]
   - Outputs: [case1, case2, ..., default]
   - Configuration: cases[], defaultCase

8. LoopNode.vue
   - Icon: 🔁
   - Inputs: [items]
   - Outputs: [iteration, complete]
   - Configuration: maxIterations, breakCondition

9. WhileNode.vue
   - Icon: ⭕
   - Inputs: [condition]
   - Outputs: [loop, exit]
   - Configuration: condition, maxIterations

10. ForEachNode.vue
    - Icon: 📋
    - Inputs: [array]
    - Outputs: [item, complete]
    - Configuration: itemVariable

11. ParallelNode.vue
    - Icon: ⚡
    - Inputs: [tasks]
    - Outputs: [results]
    - Configuration: maxConcurrency, waitForAll
```

**數據處理節點 (8 種)**:
```typescript
// packages/remote/src/components/nodes/data/

12. TransformNode.vue
    - Icon: 🔄
    - Inputs: [data]
    - Outputs: [transformed]
    - Configuration: transformExpression

13. FilterNode.vue
    - Icon: 🔍
    - Inputs: [items]
    - Outputs: [filtered]
    - Configuration: filterExpression

14. MapNode.vue
    - Icon: 🗺️
    - Inputs: [array]
    - Outputs: [mapped]
    - Configuration: mapExpression

15. ReduceNode.vue
    - Icon: ⬇️
    - Inputs: [array]
    - Outputs: [result]
    - Configuration: reduceExpression, initialValue

16. JoinNode.vue
    - Icon: 🔗
    - Inputs: [left, right]
    - Outputs: [joined]
    - Configuration: joinType, joinKey

17. SplitNode.vue
    - Icon: ✂️
    - Inputs: [data]
    - Outputs: [part1, part2, ...]
    - Configuration: splitBy, partCount

18. MergeNode.vue
    - Icon: 🔀
    - Inputs: [input1, input2, ...]
    - Outputs: [merged]
    - Configuration: mergeStrategy

19. AggregateNode.vue
    - Icon: 📊
    - Inputs: [data]
    - Outputs: [aggregated]
    - Configuration: aggregateFunction, groupBy
```

**整合節點 (5 種)**:
```typescript
// packages/remote/src/components/nodes/integration/

20. ApiCallNode.vue
    - Icon: 🌐
    - Inputs: [request]
    - Outputs: [response, error]
    - Configuration: url, method, headers, body

21. DatabaseQueryNode.vue
    - Icon: 🗄️
    - Inputs: [query, parameters]
    - Outputs: [result, error]
    - Configuration: connectionString, query, timeout

22. FileIONode.vue
    - Icon: 📁
    - Inputs: [path, content]
    - Outputs: [data, error]
    - Configuration: operation, path, encoding

23. EmailNode.vue
    - Icon: 📧
    - Inputs: [to, subject, body]
    - Outputs: [sent, error]
    - Configuration: from, template, attachments

24. WebhookNode.vue
    - Icon: 🪝
    - Inputs: [payload]
    - Outputs: [response]
    - Configuration: url, method, headers, retryPolicy
```

**進階節點 (4 種)**:
```typescript
// packages/remote/src/components/nodes/advanced/

25. SubWorkflowNode.vue
    - Icon: 📦
    - Inputs: [parameters]
    - Outputs: [result, error]
    - Configuration: workflowId, version, mapping

26. HumanInLoopNode.vue
    - Icon: 👤
    - Inputs: [context]
    - Outputs: [approval, rejected]
    - Configuration: approvers, timeout, escalation

27. CodeExecutionNode.vue
    - Icon: 💻
    - Inputs: [code, context]
    - Outputs: [result, error]
    - Configuration: language, timeout, sandbox

28. CustomNode.vue
    - Icon: 🛠️
    - Inputs: [dynamic]
    - Outputs: [dynamic]
    - Configuration: customConfig
```

### 2. Node Configuration Panel

**內聯配置 (In-place Editing)**:
```vue
<!-- packages/remote/src/components/config/InlineConfigPanel.vue -->
<template>
  <div class="inline-config">
    <!-- Quick edit overlay -->
    <div class="config-overlay">
      <!-- 常用屬性快速編輯 -->
      <div class="quick-fields">
        <el-input v-model="config.name" placeholder="Node Name" />
        <el-select v-model="config.type">
          <!-- Type options -->
        </el-select>
      </div>

      <!-- 展開詳細配置按鈕 -->
      <el-button @click="openSidebar">More Settings</el-button>
    </div>
  </div>
</template>
```

**側邊欄配置 (Sidebar Panel)**:
```vue
<!-- packages/remote/src/components/config/SidebarConfigPanel.vue -->
<template>
  <el-drawer v-model="visible" title="Node Configuration" size="400px">
    <!-- Tab navigation -->
    <el-tabs v-model="activeTab">
      <el-tab-pane label="General" name="general">
        <!-- 一般設定 -->
      </el-tab-pane>
      <el-tab-pane label="Advanced" name="advanced">
        <!-- 進階設定 -->
      </el-tab-pane>
      <el-tab-pane label="Preview" name="preview">
        <!-- 配置預覽 -->
      </el-tab-pane>
    </el-tabs>

    <!-- Configuration form -->
    <el-form :model="config" :rules="rules">
      <!-- Dynamic fields based on node type -->
    </el-form>

    <!-- Actions -->
    <template #footer>
      <el-button @click="cancel">Cancel</el-button>
      <el-button type="primary" @click="save">Save</el-button>
    </template>
  </el-drawer>
</template>
```

**屬性驗證系統**:
```typescript
// packages/remote/src/validation/nodeConfigValidation.ts
export const nodeConfigRules = {
  AgentNode: {
    agentId: [
      { required: true, message: 'Agent ID is required' },
      { validator: validateAgentExists }
    ],
    temperature: [
      { type: 'number', min: 0, max: 2, message: 'Temperature must be between 0 and 2' }
    ]
  },
  // ... 其他 27 種節點的驗證規則
}
```

### 3. Connection Line Drawing

**Bezier 曲線渲染**:
```typescript
// packages/remote/src/components/edges/CustomEdge.vue
<script setup lang="ts">
import { getBezierPath } from '@vue-flow/core'

const path = computed(() => {
  return getBezierPath({
    sourceX: props.sourceX,
    sourceY: props.sourceY,
    targetX: props.targetX,
    targetY: props.targetY,
    curvature: 0.25
  })
})
</script>

<template>
  <path
    :d="path"
    :stroke="edgeColor"
    :stroke-width="strokeWidth"
    fill="none"
    :class="edgeClass"
  />
</template>
```

**Connection Rules 驗證**:
```typescript
// packages/remote/src/validation/connectionRules.ts
export const connectionRules = {
  // Type compatibility rules
  isValidConnection(source: Node, target: Node): boolean {
    // Check port types
    if (!arePortTypesCompatible(source.outputType, target.inputType)) {
      return false
    }

    // Check circular dependency
    if (hasCircularDependency(source, target)) {
      return false
    }

    // Check max connections
    if (exceedsMaxConnections(source, target)) {
      return false
    }

    return true
  }
}
```

---

## User Stories 分配

### Sprint 13 包含的 User Stories

```yaml
US 9.3: 工作流節點管理
  Story Points: 8 SP
  Phase: Phase 1D
  Module: Module 09 - Workflow Editor
  Priority: P0 (Must-Have)

  Sub-tasks:
    1. 28 種節點類型實作 (4 SP)
       - 基礎節點 5 種
       - 控制流節點 6 種
       - 數據處理節點 8 種
       - 整合節點 5 種
       - 進階節點 4 種

    2. Node Configuration Panel (3 SP)
       - 內聯配置
       - 側邊欄配置
       - 屬性驗證
       - 實時預覽

    3. Connection Line Drawing (1 SP)
       - Bezier 曲線渲染
       - Connection Rules 驗證
       - Port Highlighting
       - Connection Animation

  Acceptance Criteria:
    ✅ 所有 28 種節點類型實作完成
    ✅ 節點可拖放到畫布並正確連線
    ✅ 配置面板支援內聯和側邊欄兩種模式
    ✅ 配置驗證正確運作
    ✅ 連線繪製流暢且符合規則
    ✅ 測試覆蓋率 ≥85%
    ✅ 文檔完整
```

### 與其他 Stories 的關係

**依賴於** (已完成):
- ✅ US 9.1: 工作流 Canvas (Sprint 11)
- ✅ US 9.2: 基礎節點與校驗 (Sprint 12)

**被依賴於** (後續 Sprint):
- 📋 US 9.4: 工作流執行與調試 (Sprint 14)
- 📋 US 9.5: 工作流協作與發佈 (Sprint 15)

---

## 技術範圍

### 前端開發

**Vue 3 組件** (28 個節點組件):
```yaml
packages/remote/src/components/nodes/
├── basic/           # 5 個基礎節點
│   ├── AgentNode.vue
│   ├── PluginNode.vue
│   ├── DecisionNode.vue
│   ├── StartNode.vue
│   └── EndNode.vue
├── control/         # 6 個控制流節點
│   ├── IfElseNode.vue
│   ├── SwitchNode.vue
│   ├── LoopNode.vue
│   ├── WhileNode.vue
│   ├── ForEachNode.vue
│   └── ParallelNode.vue
├── data/            # 8 個數據處理節點
│   ├── TransformNode.vue
│   ├── FilterNode.vue
│   ├── MapNode.vue
│   ├── ReduceNode.vue
│   ├── JoinNode.vue
│   ├── SplitNode.vue
│   ├── MergeNode.vue
│   └── AggregateNode.vue
├── integration/     # 5 個整合節點
│   ├── ApiCallNode.vue
│   ├── DatabaseQueryNode.vue
│   ├── FileIONode.vue
│   ├── EmailNode.vue
│   └── WebhookNode.vue
└── advanced/        # 4 個進階節點
    ├── SubWorkflowNode.vue
    ├── HumanInLoopNode.vue
    ├── CodeExecutionNode.vue
    └── CustomNode.vue
```

**配置系統**:
```yaml
packages/remote/src/components/config/
├── InlineConfigPanel.vue      # 內聯配置
├── SidebarConfigPanel.vue     # 側邊欄配置
├── ConfigField.vue            # 動態欄位組件
└── ConfigPreview.vue          # 配置預覽
```

**驗證系統**:
```yaml
packages/remote/src/validation/
├── nodeConfigValidation.ts    # 節點配置驗證規則
├── connectionRules.ts         # 連線規則
└── portValidation.ts          # Port 驗證
```

**連線系統**:
```yaml
packages/remote/src/components/edges/
├── CustomEdge.vue             # 自訂連線組件
├── AnimatedEdge.vue           # 動畫連線
└── edgeStyles.ts              # 連線樣式
```

### 狀態管理 (Pinia)

**節點狀態管理**:
```typescript
// packages/remote/src/stores/nodeStore.ts
export const useNodeStore = defineStore('node', {
  state: () => ({
    nodeTypes: new Map<string, NodeTypeDefinition>(),
    selectedNode: null as Node | null,
    configPanel: {
      visible: false,
      mode: 'sidebar' as 'inline' | 'sidebar',
      nodeId: null as string | null
    }
  }),

  actions: {
    registerNodeType(type: string, definition: NodeTypeDefinition) {
      this.nodeTypes.set(type, definition)
    },

    openConfigPanel(nodeId: string, mode: 'inline' | 'sidebar') {
      this.configPanel = { visible: true, mode, nodeId }
    },

    updateNodeConfig(nodeId: string, config: any) {
      // Update node configuration
    }
  }
})
```

### TypeScript 型別定義

**節點型別**:
```typescript
// packages/remote/src/types/nodes.ts
export interface NodeTypeDefinition {
  type: string
  category: 'basic' | 'control' | 'data' | 'integration' | 'advanced'
  label: string
  icon: string
  description: string
  inputs: PortDefinition[]
  outputs: PortDefinition[]
  configSchema: ConfigSchema
  defaultConfig: Record<string, any>
}

export interface PortDefinition {
  id: string
  label: string
  type: 'string' | 'number' | 'boolean' | 'object' | 'array' | 'any'
  required: boolean
  multiple: boolean
}

export interface ConfigSchema {
  fields: ConfigField[]
  validation: ValidationRule[]
}
```

### 測試

**測試範圍**:
```yaml
測試層級:
  1. 單元測試:
     - 28 個節點組件測試
     - 配置驗證邏輯測試
     - 連線規則測試

  2. 整合測試:
     - 節點註冊與初始化
     - 配置面板互動
     - 連線建立與驗證

  3. E2E 測試:
     - 完整工作流建立流程
     - 節點配置流程
     - 連線建立流程

測試工具:
  - Vitest (單元測試)
  - Vue Test Utils (組件測試)
  - Playwright (E2E 測試)

覆蓋率目標:
  - 單元測試: ≥85%
  - 整合測試: ≥75%
  - E2E 測試: 關鍵流程 100%
```

---

## 技術決策清單

### Sprint 13 Technical Decisions

```yaml
TD-094: 節點類型分類策略
  Context: 28 種節點如何組織和分類
  Decision: 5 大類別 + 28 種節點類型
  Rationale:
    - 基礎節點: 最常用的核心節點
    - 控制流: 工作流邏輯控制
    - 數據處理: 數據轉換和處理
    - 整合: 外部系統整合
    - 進階: 複雜功能和自訂
  Status: ✅ Approved
  Date: 2026-06-23
  References:
    - docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part4.md

TD-095: 配置面板模式選擇
  Context: 內聯 vs 側邊欄配置
  Decision: 雙模式支援 (Inline + Sidebar)
  Rationale:
    - Inline: 快速編輯常用屬性
    - Sidebar: 完整配置進階功能
    - 使用者可自由切換
  Alternatives Considered:
    - 單一模式 (僅 Sidebar) → 不夠靈活
    - 彈出視窗 → 遮擋畫布
  Status: ✅ Approved
  Date: 2026-06-23
  References:
    - docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part2.md

TD-096: 節點配置驗證策略
  Context: 如何驗證節點配置
  Decision: Schema-based Validation + Real-time Validation
  Rationale:
    - 每個節點定義 ConfigSchema
    - 實時驗證提供即時反饋
    - 支援同步和非同步驗證
  Implementation:
    - 使用 Zod 或 Yup 進行 schema 驗證
    - 自訂驗證器支援複雜規則
  Status: ✅ Approved
  Date: 2026-06-24
  References:
    - packages/remote/src/validation/nodeConfigValidation.ts

TD-097: Connection Line Rendering
  Context: 連線線條如何繪製
  Decision: Bezier Curves (VueFlow 內建)
  Rationale:
    - Bezier 曲線視覺效果好
    - VueFlow 原生支援
    - 可自訂曲率和樣式
  Configuration:
    - curvature: 0.25
    - strokeWidth: 2px
    - animated: true (可選)
  Status: ✅ Approved
  Date: 2026-06-24
  References:
    - VueFlow documentation

TD-098: Port Type System
  Context: 如何定義和驗證 Port 類型
  Decision: Strong Typing with Runtime Validation
  Rationale:
    - 基本類型: string, number, boolean, object, array
    - 特殊類型: any (通用), void (無輸出)
    - 類型相容性規則
  Rules:
    - any 可連接任何類型
    - object 和 array 可互相連接
    - 基本類型必須精確匹配
  Status: ✅ Approved
  Date: 2026-06-25
  References:
    - packages/remote/src/types/ports.ts

TD-099: Node Configuration Persistence
  Context: 節點配置如何持久化
  Decision: Workflow JSON + Auto-save
  Rationale:
    - 配置儲存在 Workflow JSON
    - 自動儲存每 30 秒
    - 支援版本歷史
  Format:
    {
      "nodes": [
        {
          "id": "node1",
          "type": "AgentNode",
          "config": { ... }
        }
      ]
    }
  Status: ✅ Approved
  Date: 2026-06-25
  References:
    - docs/architecture/ADR-012-workflow-editor-technology.md

TD-100: Custom Node Extensibility
  Context: 如何支援自訂節點
  Decision: Plugin System with Registration API
  Rationale:
    - 使用者可建立自訂節點
    - 透過 registerNodeType API 註冊
    - 支援 Vue 組件和配置 Schema
  API:
    nodeRegistry.registerNodeType({
      type: 'MyCustomNode',
      component: MyCustomNodeVue,
      configSchema: { ... }
    })
  Status: ✅ Approved
  Date: 2026-06-26
  References:
    - packages/remote/src/registry/nodeRegistry.ts

TD-101: Node Icon System
  Context: 節點圖示如何管理
  Decision: Emoji + Icon Library (Element Plus Icons)
  Rationale:
    - Emoji: 快速識別,跨平台一致
    - Element Plus Icons: 專業圖示庫
    - 使用者可自訂圖示
  Usage:
    - 預設使用 Emoji
    - 進階使用 Element Plus Icons
    - 自訂節點可上傳 SVG
  Status: ✅ Approved
  Date: 2026-06-26
  References:
    - docs/ux-design/design-system/design-tokens.md
```

### 相關 Technical Decisions (前幾個 Sprint)

**Sprint 11**:
- TD-082: VueFlow 1.45.0 選型
- TD-083: Canvas 縮放與平移
- TD-084: 拖放系統實作

**Sprint 12**:
- TD-088: 基礎節點設計
- TD-089: Connection validation
- TD-090: 節點樣式系統

---

## 風險與依賴

### 風險評估

```yaml
風險 1: 28 種節點開發時間不足
  嚴重性: 🟡 Medium
  機率: 30%
  影響: Sprint 延期 1 週
  緩解措施:
    - 使用節點模板加速開發
    - 並行開發多個節點類型
    - 準備好測試腳本
  應急計劃:
    - 降低進階節點優先級
    - 延後 Custom Node 到 Sprint 14

風險 2: 配置驗證邏輯複雜度過高
  嚴重性: 🟡 Medium
  機率: 25%
  影響: 測試時間增加
  緩解措施:
    - 使用成熟的驗證庫 (Zod/Yup)
    - 建立驗證測試套件
    - 文檔化驗證規則
  應急計劃:
    - 簡化驗證規則
    - 延後非同步驗證

風險 3: Bezier 曲線效能問題
  嚴重性: 🟢 Low
  機率: 15%
  影響: 大型工作流渲染慢
  緩解措施:
    - 使用 VueFlow 內建優化
    - 虛擬化渲染大量連線
    - 效能測試和監控
  應急計劃:
    - 降低動畫效果
    - 使用簡化的線條樣式

風險 4: 節點配置持久化問題
  嚴重性: 🔴 High
  機率: 20%
  影響: 配置遺失,使用者體驗差
  緩解措施:
    - 實作自動儲存機制
    - 本地儲存備份
    - 版本歷史支援
  應急計劃:
    - 增加手動儲存提示
    - 實作配置匯出功能
```

### 依賴管理

**外部依賴**:
```yaml
VueFlow 1.45.0:
  狀態: ✅ 已驗證 (Sprint 11)
  風險: 🟢 Low
  備註: 核心依賴,已完成 PoC

Element Plus 2.8.0:
  狀態: ✅ 已使用 (Sprint 12)
  風險: 🟢 Low
  備註: UI 組件庫,穩定

Zod 3.22.0 (驗證庫):
  狀態: 📋 待整合
  風險: 🟡 Medium
  備註: 需要學習曲線

Pinia 2.2.0:
  狀態: ✅ 已使用 (Sprint 11)
  風險: 🟢 Low
  備註: 狀態管理
```

**內部依賴**:
```yaml
Sprint 11 成果:
  - Canvas 核心 ✅
  - 拖放系統 ✅
  狀態: 已完成

Sprint 12 成果:
  - 基礎節點 ✅
  - Connection validation ✅
  狀態: 已完成

後端 API (Module 10):
  - Workflow CRUD API
  - 節點執行 API
  狀態: 📋 並行開發中
  風險: 🟡 Medium - 可能需要協調 API 設計
```

---

## 成功指標

### 功能完整性指標

```yaml
節點類型完整性:
  目標: 28/28 節點類型實作完成
  測量: 節點組件數量
  閾值: 100%

配置面板功能:
  目標: 內聯 + 側邊欄兩種模式
  測量: 配置模式切換成功率
  閾值: 100%

連線功能:
  目標: Bezier 曲線渲染 + 規則驗證
  測量: 連線建立成功率
  閾值: ≥95%
```

### 品質指標

```yaml
測試覆蓋率:
  單元測試: ≥85%
  整合測試: ≥75%
  E2E 測試: 關鍵流程 100%

程式碼品質:
  ESLint 錯誤: 0
  TypeScript 錯誤: 0
  重複程式碼: <5%

效能指標:
  節點渲染時間: <50ms/節點
  配置面板開啟: <200ms
  連線繪製: <100ms
  大型工作流 (100 節點): FPS ≥30
```

### 使用者體驗指標

```yaml
配置易用性:
  使用者完成節點配置時間: <2 分鐘
  配置錯誤率: <10%
  配置面板滿意度: ≥4.0/5.0

視覺反饋:
  連線高亮反應時間: <50ms
  驗證錯誤提示時間: <100ms
  配置預覽更新: <200ms
```

### 開發效率指標

```yaml
開發速度:
  平均節點開發時間: <4 小時/節點
  配置面板開發: <16 小時
  連線系統開發: <8 小時

程式碼維護性:
  組件複用率: ≥60%
  文檔完整度: 100%
  技術債務: <10%
```

---

## Phase 1D 進度追蹤

### Sprint-by-Sprint 進度

```yaml
Phase 1D: 工作流編輯器 Frontend
總 Story Points: 24 SP
總時程: 15 週 (5 個 Sprint)

Sprint 11 (Week 31-33): Canvas 核心與拖放
  Story Points: 5 SP
  完成度: ✅ 100%
  Phase 進度: 0% → 17%

Sprint 12 (Week 34-36): 基礎節點與校驗
  Story Points: 5 SP
  完成度: ✅ 100%
  Phase 進度: 17% → 50%

Sprint 13 (Week 37-39): 28 種節點類型與配置面板 [本 Sprint]
  Story Points: 8 SP
  完成度: 📋 0%
  Phase 進度: 50% → 83%

Sprint 14 (Week 40-42): 工作流執行與調試
  Story Points: 4 SP
  完成度: 📋 0%
  Phase 進度: 83% → 100%

Sprint 15 (Week 43-45): 協作與發佈
  Story Points: 2 SP
  完成度: 📋 0%
  Phase 進度: 100% (整合與優化)
```

### Phase 1D 累計進度圖

```
Phase 1D Progress: 50% → 83%

0%      17%        50%              83%              100%
├────────┼──────────┼────────────────┼────────────────┤
│ Sprint │  Sprint  │    Sprint 13   │    Sprint 14   │ Sprint 15
│   11   │    12    │   [本 Sprint]  │   (Planned)    │ (Planned)
│   ✅   │    ✅    │      🎯        │      📋        │    📋
└────────┴──────────┴────────────────┴────────────────┴─────────

完成: ████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 50%
本 Sprint 後: ████████████████████████████████████████░░░░░░░░░░ 83%
```

### Sprint 13 目標與 Phase 1D 關係

```yaml
Sprint 13 完成後,Phase 1D 將達到:
  ✅ 完整的節點系統 (28 種節點類型)
  ✅ 完善的配置能力 (內聯 + 側邊欄)
  ✅ 專業的連線繪製
  ✅ 83% Phase 1D 進度

剩餘工作 (Sprint 14-15):
  📋 工作流執行與調試 (17%)
  📋 協作與發佈 (整合優化)
```

---

## 完整參考文獻索引

### Sprint 文檔

**本 Sprint (Sprint 13)**:
1. `claudedocs/2-sprints/sprint-13/SPRINT-13-1-OVERVIEW.md` (本文件)
2. `claudedocs/2-sprints/sprint-13/SPRINT-13-2-PLAN.md`
3. `claudedocs/2-sprints/sprint-13/SPRINT-13-3-CONTEXT.md`
4. `claudedocs/2-sprints/sprint-13/SPRINT-13-4-CHECKLIST.md`
5. `claudedocs/2-sprints/sprint-13/SPRINT-13-5-DEV-LOG.md`
6. `claudedocs/2-sprints/sprint-13/SPRINT-13-6-ISSUES.md`
7. `claudedocs/2-sprints/sprint-13/SPRINT-13-7-RETROSPECTIVE.md`

**相關 Sprint**:
8. `claudedocs/2-sprints/sprint-11/` (Canvas 核心與拖放)
9. `claudedocs/2-sprints/sprint-12/` (基礎節點與校驗)
10. `claudedocs/2-sprints/sprint-14/` (工作流執行與調試) [計劃中]

### Phase 1D 文檔

11. `claudedocs/1-planning/PHASE-1D-WORKFLOW-EDITOR-PLAN.md` (Phase 總體計劃)
12. `claudedocs/1-planning/SPRINT-ALLOCATION-ANALYSIS.md` (Sprint 分配分析)
13. `claudedocs/1-planning/MODULE-09-IMPLEMENTATION-GUIDE.md` (實施指南)

### User Stories

14. `docs/user-stories/modules/module-09-workflow-editor.md` (Module 總覽)
15. `docs/user-stories/modules/module-09/US-9.3-workflow-node-management.md` (本 Sprint)
16. `docs/user-stories/README.md` (所有 User Stories 索引)

### UX 設計文檔

17. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part1.md` (Canvas)
18. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part2.md` (內聯配置)
19. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part3.md` (側邊欄)
20. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part4.md` (28 種節點)
21. `docs/ux-design/wireframes/low-fidelity/09-workflow-editor-v2-part5.md` (執行調試)
22. `docs/ux-design/information-architecture/sitemap.md`
23. `docs/ux-design/design-system/design-tokens.md`
24. `docs/ux-design/design-system/component-library.md`

### 架構文檔

25. `docs/architecture/Architecture-Design-Document.md` (系統架構)
26. `docs/architecture/C4-architecture-diagrams.md` (C4 模型)
27. `docs/architecture/ADR-012-workflow-editor-technology.md` (工作流編輯器技術選型)
28. `docs/architecture/database-schema.md` (資料庫設計)
29. `docs/architecture/performance-scalability-strategy.md` (效能策略)

### 技術實作文檔

30. `docs/technical-implementation/1-poc-validation/poc-06-vueflow-crdt.md` (VueFlow PoC)
31. `docs/technical-implementation/frontend-architecture.md`
32. `docs/technical-implementation/module-federation-setup.md`
33. `docs/technical-implementation/testing-strategy.md`

### 專案管理文檔

34. `docs/project-management/Project-Management-Plan.md`
35. `docs/project-management/sprint-planning-template.md`
36. `docs/project-management/risk-management.md`

### 程式碼庫文檔

37. `packages/remote/README.md` (工作流編輯器專案)
38. `packages/remote/src/components/nodes/README.md` (節點組件說明)
39. `packages/remote/src/components/config/README.md` (配置系統說明)
40. `packages/remote/src/validation/README.md` (驗證系統說明)

### VueFlow 文檔

41. VueFlow Official Documentation: https://vueflow.dev
42. VueFlow API Reference: https://vueflow.dev/api
43. VueFlow Examples: https://vueflow.dev/examples
44. VueFlow GitHub: https://github.com/bcakmakoglu/vue-flow

### Element Plus 文檔

45. Element Plus Documentation: https://element-plus.org
46. Element Plus Form Components: https://element-plus.org/en-US/component/form.html
47. Element Plus Drawer: https://element-plus.org/en-US/component/drawer.html
48. Element Plus Icons: https://element-plus.org/en-US/component/icon.html

### Vue 3 文檔

49. Vue 3 Documentation: https://vuejs.org
50. Vue 3 Composition API: https://vuejs.org/api/composition-api-setup.html
51. Vue 3 TypeScript: https://vuejs.org/guide/typescript/overview.html
52. Pinia Documentation: https://pinia.vuejs.org

### 驗證庫文檔

53. Zod Documentation: https://zod.dev
54. Yup Documentation: https://github.com/jquense/yup
55. VeeValidate: https://vee-validate.logaretm.com

### 測試文檔

56. Vitest Documentation: https://vitest.dev
57. Vue Test Utils: https://test-utils.vuejs.org
58. Playwright Documentation: https://playwright.dev

### TypeScript 文檔

59. TypeScript Handbook: https://www.typescriptlang.org/docs/handbook
60. TypeScript Best Practices: https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html

### 開發工具文檔

61. Vite Documentation: https://vitejs.dev
62. ESLint Vue Plugin: https://eslint.vuejs.org
63. Prettier: https://prettier.io

### Git & GitHub

64. Git Flow: https://nvie.com/posts/a-successful-git-branching-model/
65. Conventional Commits: https://www.conventionalcommits.org
66. GitHub Actions: https://docs.github.com/en/actions

### 相關標準與最佳實踐

67. WCAG 2.1 無障礙標準: https://www.w3.org/WAI/WCAG21/quickref/
68. Web Content Accessibility Guidelines (WCAG): https://www.w3.org/WAI/standards-guidelines/wcag/
69. Vue.js Style Guide: https://vuejs.org/style-guide/
70. TypeScript Coding Guidelines: https://github.com/microsoft/TypeScript/wiki/Coding-guidelines

### 設計參考

71. Figma Community - Workflow Editors: https://www.figma.com/community
72. n8n Workflow Editor: https://n8n.io
73. Node-RED: https://nodered.org
74. Apache Airflow UI: https://airflow.apache.org

### 內部知識庫

75. `claudedocs/knowledge-base/vueflow-best-practices.md`
76. `claudedocs/knowledge-base/node-configuration-patterns.md`
77. `claudedocs/knowledge-base/connection-validation-examples.md`
78. `claudedocs/knowledge-base/performance-optimization-tips.md`

---

## 附錄

### A. 縮寫與術語

```yaml
通用縮寫:
  SP: Story Point (故事點)
  US: User Story (使用者故事)
  TD: Technical Decision (技術決策)
  PoC: Proof of Concept (概念驗證)
  MVP: Minimum Viable Product (最小可行產品)

技術術語:
  VueFlow: Vue 3 工作流圖形化編輯器庫
  Pinia: Vue 3 官方狀態管理庫
  Element Plus: Vue 3 UI 組件庫
  Vitest: Vite 原生測試框架
  Zod: TypeScript-first schema 驗證庫

專案特定術語:
  Node: 工作流節點
  Edge: 節點間的連線
  Port: 節點的輸入/輸出端口
  Canvas: 工作流編輯畫布
  ConfigPanel: 節點配置面板
```

### B. 快速參考連結

**啟動專案**:
```bash
# 安裝依賴
cd packages/remote
npm install

# 啟動開發伺服器
npm run dev

# 執行測試
npm run test

# 建置
npm run build
```

**常用命令**:
```bash
# 型別檢查
npm run type-check

# Lint
npm run lint

# 格式化
npm run format

# E2E 測試
npm run test:e2e
```

### C. 聯絡資訊

**Sprint 13 負責人**:
- Sprint Master: [待指派]
- Tech Lead: [待指派]
- UX Designer: [待指派]

**支援管道**:
- Slack: #sprint-13-workflow-nodes
- Email: dev-team@company.com
- Office Hours: 每週三 14:00-15:00

---

**文檔版本**: v2.1
**最後更新**: 2025-11-14
**下次審查**: Sprint 13 開始時 (2026-06-23)
**維護者**: AI Agent Platform Team

---

## 變更歷史

```yaml
v2.1 (2025-11-14):
  - 初始建立 Sprint 13 Overview
  - 定義 28 種節點類型
  - 規劃配置面板設計
  - 建立技術決策 TD-094 to TD-101

v2.2 (預計 2026-06-23):
  - Sprint 13 開始時更新狀態
  - 調整風險評估
  - 更新進度追蹤
```

**相關文件**: 請參閱本文件開頭的「文檔導航」章節。
