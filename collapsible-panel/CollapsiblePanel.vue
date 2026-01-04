<template>
  <div class="collapsible-panel">
    <!-- 标题栏 -->
    <div
      class="collapsible-header flex items-center justify-between cursor-pointer select-none"
      :class="headerClass"
      @click="toggleCollapse"
    >
      <div class="flex items-center">
        <i
          :class="[
            'transition-transform duration-300',
            isCollapsed ? 'el-icon-arrow-down' : 'el-icon-arrow-up'
          ]"
          class="mr-2"
        ></i>
        <span class="font-medium">{{ title }}</span>
        <slot name="header-extra"></slot>
      </div>
      <div class="flex items-center">
        <slot name="header-actions"></slot>
      </div>
    </div>

    <!-- 内容区域 -->
    <div
      class="collapsible-content"
      :class="contentClass"
      :style="contentStyle"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CollapsiblePanel',
  props: {
    // 面板标题
    title: {
      type: String,
      default: ''
    },
    // 是否默认折叠
    defaultCollapsed: {
      type: Boolean,
      default: false
    },
    // 是否禁用折叠功能
    disabled: {
      type: Boolean,
      default: false
    },
    // 标题栏样式类
    headerClass: {
      type: String,
      default: 'bg-gray-50 hover:bg-gray-100 px-4 py-3 border-b border-gray-200'
    },
    // 内容区域样式类
    contentClass: {
      type: String,
      default: 'p-4 bg-white'
    },
    // 动画持续时间（毫秒）
    animationDuration: {
      type: Number,
      default: 300
    }
  },
  data() {
    return {
      isCollapsed: this.defaultCollapsed,
      contentHeight: 'auto'
    }
  },
  computed: {
    contentStyle() {
      if (this.isCollapsed) {
        return {
          height: '0px',
          overflow: 'hidden',
          transition: `height ${this.animationDuration}ms ease-in-out`
        }
      }
      return {
        height: this.contentHeight,
        overflow: 'hidden',
        transition: `height ${this.animationDuration}ms ease-in-out`
      }
    }
  },
  inject: {
    accordion: {
      default: null
    }
  },
  methods: {
    toggleCollapse() {
      if (this.disabled) return

      this.isCollapsed = !this.isCollapsed
      this.$emit('toggle', this.isCollapsed)
      this.$emit('change', this.isCollapsed)

      // 通知父级accordion组件
      if (this.accordion) {
        if (this.isCollapsed) {
          this.accordion.onPanelCollapse(this)
        } else {
          this.accordion.onPanelExpand(this)
        }
      }
    },

    // 展开面板
    expand() {
      if (this.disabled || !this.isCollapsed) return
      this.isCollapsed = false
      this.$emit('expand')
      this.$emit('change', false)

      if (this.accordion) {
        this.accordion.onPanelExpand(this)
      }
    },

    // 折叠面板
    collapse() {
      if (this.disabled || this.isCollapsed) return
      this.isCollapsed = true
      this.$emit('collapse')
      this.$emit('change', true)

      if (this.accordion) {
        this.accordion.onPanelCollapse(this)
      }
    }
  },
  mounted() {
    // 计算内容高度
    this.$nextTick(() => {
      const content = this.$el.querySelector('.collapsible-content')
      if (content) {
        this.contentHeight = content.scrollHeight + 'px'
      }
    })

    // 注册到accordion
    if (this.accordion) {
      this.accordion.registerPanel(this)
    }
  },

  beforeDestroy() {
    // 从accordion注销
    if (this.accordion) {
      this.accordion.unregisterPanel(this)
    }
  }
}
</script>

<style scoped lang="less">
.collapsible-panel {
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  overflow: hidden;

  .collapsible-header {
    transition: background-color 0.2s ease;

    &:hover {
      background-color: #f9fafb;
    }

    &.disabled {
      cursor: not-allowed;
      opacity: 0.6;

      &:hover {
        background-color: inherit;
      }
    }
  }

  .collapsible-content {
    border-top: 1px solid #e5e7eb;
  }
}
</style>
