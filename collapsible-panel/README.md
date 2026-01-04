# 可折叠面板组件

## 组件介绍

`CollapsiblePanel` 和 `Accordion` 是一组可折叠的面板组件，支持风琴效果和自定义内容。

## 组件列表

- `CollapsiblePanel`: 单个可折叠面板
- `Accordion`: 风琴容器，管理多个折叠面板

## 使用方法

### 基础用法 - 单个折叠面板

```vue
<template>
  <CollapsiblePanel title="高级查询" :default-collapsed="true">
    <el-form :inline="true">
      <el-form-item label="课程名称：">
        <el-input v-model="search.teach_name" size="small"></el-input>
      </el-form-item>
      <el-form-item label="课程类型：">
        <el-select v-model="search.teach_kind" size="small">
          <el-option label="类型1" value="1"></el-option>
          <el-option label="类型2" value="2"></el-option>
        </el-select>
      </el-form-item>
    </el-form>
  </CollapsiblePanel>
</template>

<script>
import { CollapsiblePanel } from '@/components/main-layout'

export default {
  components: {
    CollapsiblePanel
  },
  data() {
    return {
      search: {
        teach_name: '',
        teach_kind: ''
      }
    }
  }
}
</script>
```

### 风琴模式 - 多个面板

```vue
<template>
  <Accordion :accordion="true" :default-active-index="0">
    <CollapsiblePanel title="基础查询">
      <el-form :inline="true">
        <el-form-item label="课程名称：">
          <el-input v-model="search.teach_name" size="small"></el-input>
        </el-form-item>
      </el-form>
    </CollapsiblePanel>

    <CollapsiblePanel title="高级查询">
      <el-form :inline="true">
        <el-form-item label="课程类型：">
          <el-select v-model="search.teach_kind" size="small">
            <el-option label="类型1" value="1"></el-option>
            <el-option label="类型2" value="2"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="主讲人：">
          <el-select v-model="search.teacher_id" size="small">
            <el-option label="教师1" value="1"></el-option>
            <el-option label="教师2" value="2"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
    </CollapsiblePanel>
  </Accordion>
</template>

<script>
import { Accordion, CollapsiblePanel } from '@/components/main-layout'

export default {
  components: {
    Accordion,
    CollapsiblePanel
  }
}
</script>
```

### 自定义样式

```vue
<CollapsiblePanel
  title="自定义样式面板"
  header-class="bg-blue-50 hover:bg-blue-100 px-6 py-4"
  content-class="p-6 bg-gray-50"
>
  <p>自定义内容</p>
</CollapsiblePanel>
```

### 带操作按钮的面板

```vue
<CollapsiblePanel title="带操作的面板">
  <template #header-actions>
    <el-button size="small" type="primary">操作1</el-button>
    <el-button size="small" class="ml-2">操作2</el-button>
  </template>

  <template #header-extra>
    <el-tag size="small" class="ml-2">标签</el-tag>
  </template>

  <p>面板内容</p>
</CollapsiblePanel>
```

## Props

### CollapsiblePanel

| 参数 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| title | 面板标题 | String | '' |
| defaultCollapsed | 是否默认折叠 | Boolean | false |
| disabled | 是否禁用折叠功能 | Boolean | false |
| headerClass | 标题栏样式类 | String | 'bg-gray-50 hover:bg-gray-100 px-4 py-3 border-b border-gray-200' |
| contentClass | 内容区域样式类 | String | 'p-4 bg-white' |
| animationDuration | 动画持续时间（毫秒） | Number | 300 |

### Accordion

| 参数 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| accordion | 是否启用手风琴模式 | Boolean | false |
| defaultActiveIndex | 默认展开的面板索引 | Number | 0 |

## Events

### CollapsiblePanel

| 事件名 | 说明 | 回调参数 |
|--------|------|----------|
| toggle | 切换折叠状态时触发 | (isCollapsed: Boolean) |
| change | 状态改变时触发 | (isCollapsed: Boolean) |
| expand | 展开时触发 | - |
| collapse | 折叠时触发 | - |

### Accordion

| 事件名 | 说明 | 回调参数 |
|--------|------|----------|
| panel-expand | 面板展开时触发 | (panel: CollapsiblePanel) |
| panel-collapse | 面板折叠时触发 | (panel: CollapsiblePanel) |

## Slots

### CollapsiblePanel

| 插槽名 | 说明 |
|--------|------|
| default | 面板内容 |
| header-extra | 标题栏额外内容（在标题右侧） |
| header-actions | 标题栏操作按钮（在标题栏右侧） |

## 在现有组件中的应用

替换原有的高级查询展开/收起逻辑：

```vue
<!-- 原来的代码 -->
<div v-show="!isAdvanced || (isAdvanced && isExpandAdvanced)">
  <!-- 高级查询内容 -->
</div>

<!-- 使用新组件 -->
<CollapsiblePanel
  title="高级查询"
  :default-collapsed="true"
  v-model="isExpandAdvanced"
>
  <!-- 高级查询内容 -->
</CollapsiblePanel>
```

## 方法

### CollapsiblePanel

- `expand()`: 展开面板
- `collapse()`: 折叠面板
- `toggleCollapse()`: 切换折叠状态

### Accordion

- `getActiveIndex()`: 获取当前激活的面板索引
