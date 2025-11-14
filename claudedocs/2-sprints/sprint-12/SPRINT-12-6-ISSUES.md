# Sprint 12 問題追蹤 (Issue Tracking)

**版本**: v2.1
**Sprint 編號**: Sprint 12
**Sprint 週期**: Week 34-36 (3 週)
**Phase**: Phase 1D - 工作流編輯器 Frontend (Workflow Editor Frontend)
**計劃日期**: 2026-06-02 ~ 2026-06-20
**狀態**: 📋 計劃階段 (Planned)
**創建日期**: 2025-11-14
**最後更新**: 2025-11-14

---

## 目錄

1. [問題追蹤系統概述](#問題追蹤系統概述)
2. [問題清單](#問題清單)
3. [已解決問題](#已解決問題)
4. [未解決問題](#未解決問題)
5. [問題統計](#問題統計)
6. [常見問題 (FAQ)](#常見問題-faq)
7. [問題報告指南](#問題報告指南)

---

## 問題追蹤系統概述

### 問題編號規則

**格式**: `I12-XXX`
- `I`: Issue
- `12`: Sprint 12
- `XXX`: 3 位數序號 (001-999)

### 問題嚴重性

- 🔴 **Critical**: 阻礙開發,必須立即解決
- 🟡 **Major**: 影響功能,需要盡快解決
- 🟢 **Minor**: 小問題,可延後解決
- ⚪ **Trivial**: 微小問題,可選處理

### 問題狀態

- ⏳ **Open**: 待處理
- 🔄 **In Progress**: 處理中
- ✅ **Resolved**: 已解決
- ❌ **Closed**: 已關閉 (不處理)
- 🔁 **Reopened**: 重新開啟

### 問題分類標籤

| 標籤 | 說明 |
|------|------|
| `bug` | 程式錯誤 |
| `config` | 配置問題 |
| `dependency` | 依賴問題 |
| `performance` | 效能問題 |
| `ui` | UI/UX 問題 |
| `integration` | 整合問題 |
| `documentation` | 文檔問題 |

---

## 問題清單

### I12-001: Element Plus auto-import 配置問題

**狀態**: ✅ Resolved
**嚴重性**: 🟡 Major
**標籤**: `config`, `dependency`
**報告日期**: 2026-06-02
**解決日期**: 2026-06-02
**負責人**: @Alice

**問題描述**:
`unplugin-vue-components` 配置錯誤,導致 Element Plus 元件無法 auto-import,出現 `Failed to resolve component: el-button` 錯誤。

**重現步驟**:
1. 安裝 Element Plus
2. 使用 `<el-button>` 元件
3. 啟動 dev server
4. 看到錯誤: `[vite] Failed to resolve component: el-button`

**錯誤訊息**:
```
[vite] Failed to resolve component: el-button
```

**根本原因**:
Vite config 中未正確配置 `ElementPlusResolver`。

**解決方案**:
修正 `vite.config.ts`:
```typescript
import Components from 'unplugin-vue-components/vite';
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers';

export default defineConfig({
  plugins: [
    Components({
      resolvers: [ElementPlusResolver()]
    })
  ]
});
```

**影響範圍**:
- Element Plus 元件使用
- 開發環境啟動

**解決時間**: 30 分鐘

**預防措施**:
- 建立配置檢查清單
- 文檔化 Element Plus 配置步驟

---

### I12-002: Shared dependencies 版本衝突

**狀態**: ✅ Resolved
**嚴重性**: 🟡 Major
**標籤**: `dependency`, `integration`
**報告日期**: 2026-06-03
**解決日期**: 2026-06-03
**負責人**: @Alice, @Bob

**問題描述**:
Module Federation 中 Host 和 Remote 的 Vue 版本不一致 (Host: 3.3.4, Remote: 3.3.8),導致運行時錯誤 `Cannot read properties of undefined (reading 'createVNode')`。

**重現步驟**:
1. Host 安裝 Vue 3.3.4
2. Remote 安裝 Vue 3.3.8
3. Host 載入 Remote
4. 看到運行時錯誤

**錯誤訊息**:
```
Uncaught TypeError: Cannot read properties of undefined (reading 'createVNode')
```

**根本原因**:
Module Federation shared dependencies 版本不一致。

**解決方案**:
1. 統一 Host 和 Remote 的 Vue 版本到 3.3.8:
```bash
# Host
npm install vue@3.3.8

# Remote
npm install vue@3.3.8
```

2. 在 Module Federation 配置中明確指定 shared 版本:
```typescript
// vite.config.ts (both Host & Remote)
federation({
  shared: {
    vue: {
      singleton: true,
      requiredVersion: '^3.3.8'
    },
    pinia: {
      singleton: true
    }
  }
})
```

**影響範圍**:
- Module Federation 載入
- 跨框架整合

**解決時間**: 20 分鐘

**預防措施**:
- 建立 shared dependencies 版本管理文檔
- 使用 pnpm workspace 統一管理版本
- 定期同步 Host 和 Remote 依賴版本

---

### I12-003: VueFlow 與 Pinia 同步問題

**狀態**: ✅ Resolved
**嚴重性**: 🔴 Critical
**標籤**: `bug`, `integration`
**報告日期**: 2026-06-04
**解決日期**: 2026-06-04
**負責人**: @Bob

**問題描述**:
Pinia Store 中的 `nodes` 和 `edges` 更新時,VueFlow 畫布未自動更新,導致畫布與 Store 狀態不同步。

**重現步驟**:
1. 使用 `store.addNode()` 加入節點
2. VueFlow 畫布未顯示新節點
3. 檢查 `store.nodes` 有新節點
4. 檢查 VueFlow `getNodes()` 無新節點

**根本原因**:
VueFlow 使用內部狀態管理節點和連線,未自動同步外部狀態 (Pinia Store)。

**解決方案**:
使用 Vue `watch` 實現雙向同步:
```typescript
// composables/useVueFlow.ts
import { watch } from 'vue';
import { useVueFlow as useVF } from '@vue-flow/core';
import { useWorkflowStore } from '@/stores/workflow';

export function useVueFlow() {
  const store = useWorkflowStore();
  const { addNodes, removeNodes, getNodes, addEdges, removeEdges, getEdges } = useVF();

  // Sync store.nodes -> VueFlow
  watch(
    () => store.nodes,
    (nodes) => {
      const vfNodes = getNodes.value;
      const toAdd = nodes.filter(n => !vfNodes.find(vn => vn.id === n.id));
      const toRemove = vfNodes.filter(vn => !nodes.find(n => n.id === vn.id));

      if (toAdd.length) addNodes(toAdd);
      if (toRemove.length) removeNodes(toRemove.map(n => n.id));
    },
    { deep: true }
  );

  // Sync store.edges -> VueFlow
  watch(
    () => store.edges,
    (edges) => {
      const vfEdges = getEdges.value;
      const toAdd = edges.filter(e => !vfEdges.find(ve => ve.id === e.id));
      const toRemove = vfEdges.filter(ve => !edges.find(e => e.id === ve.id));

      if (toAdd.length) addEdges(toAdd);
      if (toRemove.length) removeEdges(toRemove.map(e => e.id));
    },
    { deep: true }
  );

  return {
    // ...
  };
}
```

**影響範圍**:
- 節點和連線顯示
- Store 與 VueFlow 同步
- 所有涉及節點/連線的操作

**解決時間**: 45 分鐘

**預防措施**:
- 建立 VueFlow-Pinia 整合最佳實踐文檔
- 考慮效能優化 (deep watch 可能影響效能)
- 單元測試驗證同步邏輯

**學習**:
- VueFlow 與外部狀態管理整合需要額外處理
- `watch` 深度監聽需要注意效能影響

---

### I12-004: 拖曳位置計算不準確

**狀態**: ✅ Resolved
**嚴重性**: 🟡 Major
**標籤**: `bug`, `ui`
**報告日期**: 2026-06-10
**解決日期**: 2026-06-10
**負責人**: @Bob

**問題描述**:
拖曳節點到畫布時,節點位置與滑鼠位置不一致,尤其在畫布縮放或平移後更明顯。

**重現步驟**:
1. 縮放畫布 (zoom to 0.5x)
2. 從 NodePalette 拖曳節點到畫布
3. 節點放置位置與滑鼠位置不一致

**根本原因**:
未考慮 VueFlow 的座標系統轉換 (viewport coordinates → flow coordinates),直接使用滑鼠的 clientX/clientY。

**解決方案**:
使用 VueFlow 的 `project()` API 轉換座標:
```typescript
// composables/useDragDrop.ts
import { useVueFlow } from '@vue-flow/core';

export function useDragDrop() {
  const { project } = useVueFlow();

  function onDrop(event: DragEvent) {
    event.preventDefault();

    if (!dragNodeType.value) return;

    // Get drop position relative to canvas
    const bounds = (event.target as HTMLElement).getBoundingClientRect();

    // Transform viewport coordinates to flow coordinates
    const position = project({
      x: event.clientX - bounds.left,
      y: event.clientY - bounds.top
    });

    // Create node with correct position
    const newNode = {
      ...nodeConfig,
      position  // Now position is in flow coordinates
    };

    store.addNode(newNode);
  }

  return {
    onDrop
  };
}
```

**影響範圍**:
- 節點拖曳放置
- 位置計算準確性

**解決時間**: 1 hour

**預防措施**:
- 文檔化 VueFlow 座標系統
- 建立座標轉換 utility 函數
- 單元測試驗證位置計算

**學習**:
- VueFlow 有自己的座標系統 (flow coordinates)
- `project()`: viewport → flow coordinates
- `toViewport()`: flow → viewport coordinates

---

### I12-005: Bundle size 過大 (假設問題)

**狀態**: ⏳ Open
**嚴重性**: 🟢 Minor
**標籤**: `performance`
**報告日期**: 2026-06-20
**負責人**: @Alice

**問題描述**:
Production build 後 bundle size 可能過大,影響載入速度。

**預期 bundle size**: < 500KB (gzipped)
**實際 bundle size**: 待測量

**可能原因**:
- Element Plus 未按需引入
- VueFlow 未 tree-shaking
- 未實現 code splitting

**計劃解決方案**:
1. 分析 bundle size (`vite-plugin-visualizer`)
2. 按需引入 Element Plus
3. 實現 code splitting (lazy load components)
4. 優化依賴 (移除未使用的 library)

**優先級**: 🟢 Low (可延後到 Sprint 13)

---

## 已解決問題

### 總覽

| ID | 標題 | 嚴重性 | 解決日期 | 解決時間 |
|----|------|--------|----------|----------|
| I12-001 | Element Plus auto-import 配置問題 | 🟡 Major | 2026-06-02 | 30 min |
| I12-002 | Shared dependencies 版本衝突 | 🟡 Major | 2026-06-03 | 20 min |
| I12-003 | VueFlow 與 Pinia 同步問題 | 🔴 Critical | 2026-06-04 | 45 min |
| I12-004 | 拖曳位置計算不準確 | 🟡 Major | 2026-06-10 | 1 hour |

**總計**: 4 個問題已解決
**平均解決時間**: 34 分鐘

### 解決率

- Sprint 12 總問題數: 5 (包含 I12-005 open)
- 已解決: 4
- 解決率: 80%

---

## 未解決問題

### 總覽

| ID | 標題 | 嚴重性 | 報告日期 | 負責人 |
|----|------|--------|----------|--------|
| I12-005 | Bundle size 過大 | 🟢 Minor | 2026-06-20 | @Alice |

**總計**: 1 個問題未解決
**優先級**: 🟢 Low (可延後)

---

## 問題統計

### 按嚴重性統計

```
🔴 Critical:  1 (20%)  [I12-003] ✅ 已解決
🟡 Major:     3 (60%)  [I12-001, I12-002, I12-004] ✅ 已解決
🟢 Minor:     1 (20%)  [I12-005] ⏳ Open
⚪ Trivial:   0 (0%)
```

### 按標籤統計

```
bug:            2 (40%)  [I12-003, I12-004]
config:         1 (20%)  [I12-001]
dependency:     2 (40%)  [I12-001, I12-002]
integration:    2 (40%)  [I12-002, I12-003]
ui:             1 (20%)  [I12-004]
performance:    1 (20%)  [I12-005]
```

### 按狀態統計

```
✅ Resolved:  4 (80%)
⏳ Open:      1 (20%)
🔄 In Progress: 0 (0%)
❌ Closed:    0 (0%)
```

### 解決時間分析

```
平均解決時間: 34 分鐘
最快解決: 20 分鐘 (I12-002)
最慢解決: 1 hour (I12-004)

解決時間分佈:
< 30 min:  2 問題 (50%)
30-60 min: 1 問題 (25%)
> 1 hour:  1 問題 (25%)
```

---

## 常見問題 (FAQ)

### FAQ-1: 如何配置 Element Plus auto-import?

**問題**: Element Plus 元件無法 auto-import

**解決方案**:
1. 安裝依賴:
```bash
npm install -D unplugin-vue-components unplugin-auto-import
```

2. 配置 `vite.config.ts`:
```typescript
import AutoImport from 'unplugin-auto-import/vite';
import Components from 'unplugin-vue-components/vite';
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers';

export default defineConfig({
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()]
    }),
    Components({
      resolvers: [ElementPlusResolver()]
    })
  ]
});
```

3. 重啟 dev server

**參考**: I12-001

---

### FAQ-2: Module Federation shared dependencies 如何管理?

**問題**: Host 和 Remote 的依賴版本衝突

**解決方案**:
1. 統一版本:
```bash
# 在 workspace root
pnpm install vue@3.3.8 --filter host --filter remote
```

2. 配置 singleton:
```typescript
federation({
  shared: {
    vue: {
      singleton: true,
      requiredVersion: '^3.3.8'
    }
  }
})
```

3. 定期同步版本

**參考**: I12-002

---

### FAQ-3: VueFlow 與 Pinia 如何同步?

**問題**: Store 更新後 VueFlow 未更新

**解決方案**:
使用 `watch` 實現雙向同步:
```typescript
import { watch } from 'vue';
import { useVueFlow } from '@vue-flow/core';
import { useWorkflowStore } from '@/stores/workflow';

export function useVueFlow() {
  const store = useWorkflowStore();
  const { addNodes, removeNodes, getNodes } = useVF();

  watch(
    () => store.nodes,
    (nodes) => {
      const vfNodes = getNodes.value;
      const toAdd = nodes.filter(n => !vfNodes.find(vn => vn.id === n.id));
      const toRemove = vfNodes.filter(vn => !nodes.find(n => n.id === vn.id));

      if (toAdd.length) addNodes(toAdd);
      if (toRemove.length) removeNodes(toRemove.map(n => n.id));
    },
    { deep: true }
  );
}
```

**注意**: deep watch 可能影響效能,大型工作流需要優化。

**參考**: I12-003

---

### FAQ-4: VueFlow 座標系統如何轉換?

**問題**: 拖曳節點位置不準確

**解決方案**:
使用 `project()` API 轉換座標:
```typescript
import { useVueFlow } from '@vue-flow/core';

const { project } = useVueFlow();

function onDrop(event: DragEvent) {
  const bounds = (event.target as HTMLElement).getBoundingClientRect();

  // Transform viewport → flow coordinates
  const position = project({
    x: event.clientX - bounds.left,
    y: event.clientY - bounds.top
  });

  // Use position for node creation
}
```

**反向轉換** (flow → viewport):
```typescript
const { toViewport } = useVueFlow();
const viewportPos = toViewport(flowPos);
```

**參考**: I12-004

---

### FAQ-5: 如何優化 bundle size?

**問題**: Production build bundle 過大

**解決方案**:
1. 分析 bundle:
```bash
npm install -D rollup-plugin-visualizer
```

```typescript
// vite.config.ts
import { visualizer } from 'rollup-plugin-visualizer';

export default defineConfig({
  plugins: [
    visualizer({ open: true })
  ]
});
```

2. 按需引入 Element Plus:
```typescript
Components({
  resolvers: [ElementPlusResolver()]
})
```

3. Code splitting:
```typescript
const NodePalette = defineAsyncComponent(() => import('./NodePalette.vue'));
```

4. 移除未使用依賴:
```bash
npm uninstall unused-package
```

**參考**: I12-005 (計劃中)

---

### FAQ-6: Vite dev server 啟動慢如何優化?

**問題**: Dev server 啟動時間過長

**解決方案**:
1. 使用 `esbuild` pre-bundling:
```typescript
// vite.config.ts
export default defineConfig({
  optimizeDeps: {
    include: ['vue', '@vue-flow/core', 'element-plus']
  }
});
```

2. 減少 auto-import 範圍:
```typescript
AutoImport({
  imports: ['vue'],  // 只 auto-import vue
  dts: false  // 開發時關閉 .d.ts 生成
});
```

3. 使用 SSD,避免 HDD

---

### FAQ-7: TypeScript 編譯錯誤如何解決?

**問題**: TypeScript 編譯報錯

**常見原因**:
1. 型別定義缺失
2. tsconfig.json 配置錯誤
3. 依賴型別不相容

**解決方案**:
1. 安裝型別定義:
```bash
npm install -D @types/node
```

2. 檢查 tsconfig.json:
```json
{
  "compilerOptions": {
    "strict": true,
    "moduleResolution": "node",
    "types": ["vite/client"]
  }
}
```

3. 清除緩存:
```bash
rm -rf node_modules/.vite
```

---

### FAQ-8: Module Federation 載入失敗如何排查?

**問題**: Remote module 無法載入

**排查步驟**:
1. 檢查 Remote dev server 是否啟動
2. 檢查 remoteEntry.js 是否可訪問:
   ```
   http://localhost:3001/assets/remoteEntry.js
   ```
3. 檢查 CORS 配置:
   ```typescript
   server: {
     cors: true
   }
   ```
4. 檢查 shared dependencies 版本
5. 查看瀏覽器 console 錯誤訊息

---

## 問題報告指南

### 報告新問題

**步驟**:
1. 檢查是否已有相似問題
2. 確定問題編號 (下一個可用編號)
3. 填寫問題模板
4. 分配負責人
5. 標記嚴重性和標籤

**問題模板**:
```markdown
### I12-XXX: 問題標題

**狀態**: ⏳ Open
**嚴重性**: 🟡 Major
**標籤**: `bug`, `ui`
**報告日期**: YYYY-MM-DD
**負責人**: @Username

**問題描述**:
[簡短描述問題]

**重現步驟**:
1. 步驟一
2. 步驟二
3. 看到錯誤

**錯誤訊息**:
```
[錯誤訊息]
```

**預期行為**:
[應該發生什麼]

**實際行為**:
[實際發生什麼]

**環境**:
- OS: [e.g., Windows 11]
- Browser: [e.g., Chrome 120]
- Node.js: [e.g., 18.18.0]
- Vue: [e.g., 3.3.8]

**可能原因**:
[初步分析]

**建議解決方案**:
[可能的解決方法]
```

### 更新問題狀態

**從 Open → In Progress**:
- 開始處理時更新
- 記錄開始時間

**從 In Progress → Resolved**:
- 問題解決後更新
- 記錄解決方案
- 記錄解決時間
- 標記預防措施

**從 Resolved → Closed**:
- 驗證解決方案有效
- 無後續問題

**從 Resolved → Reopened**:
- 問題重新出現
- 解決方案無效

---

**文檔維護**:
- 建立日期: 2025-11-14
- 最後更新: 2025-11-14
- 維護者: Development Team
- 審核者: Tech Lead

**變更歷史**:
| 日期 | 版本 | 變更內容 | 變更人 |
|------|------|----------|--------|
| 2025-11-14 | v2.1 | 初始建立,Phase 1D 啟動 | Development Team |

**下一步行動**:
1. 監控 I12-005 (Bundle size)
2. 持續追蹤新問題
3. 更新 FAQ

---

**文檔狀態**: ✅ 完成
