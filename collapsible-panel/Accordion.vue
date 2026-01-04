<template>
  <div class="accordion">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'Accordion',
  props: {
    // 是否启用手风琴模式（同时只能展开一个面板）
    accordion: {
      type: Boolean,
      default: false
    },
    // 默认展开的面板索引（accordion模式下）
    defaultActiveIndex: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      activeIndex: this.defaultActiveIndex,
      panels: []
    }
  },
  provide() {
    return {
      accordion: this
    }
  },
  methods: {
    // 注册面板
    registerPanel(panel) {
      this.panels.push(panel)
    },

    // 注销面板
    unregisterPanel(panel) {
      const index = this.panels.indexOf(panel)
      if (index > -1) {
        this.panels.splice(index, 1)
      }
    },

    // 面板展开事件
    onPanelExpand(panel) {
      if (this.accordion) {
        // 手风琴模式：关闭其他面板
        this.panels.forEach(p => {
          if (p !== panel && !p.isCollapsed) {
            p.collapse()
          }
        })
      }
      this.$emit('panel-expand', panel)
    },

    // 面板折叠事件
    onPanelCollapse(panel) {
      this.$emit('panel-collapse', panel)
    },

    // 获取当前激活的面板索引
    getActiveIndex() {
      return this.panels.findIndex(panel => !panel.isCollapsed)
    }
  }
}
</script>

<style scoped lang="less">
.accordion {
  .collapsible-panel {
    margin-bottom: 8px;

    &:last-child {
      margin-bottom: 0;
    }
  }
}
</style>
