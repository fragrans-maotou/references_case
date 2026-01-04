<template>
  <div class="example-container">
    <!-- 示例1: 使用CollapsiblePanel重构高级查询 -->
    <div class="mb-6">
      <h3 class="text-lg font-medium mb-4">示例1: 重构高级查询</h3>

      <!-- 基础查询部分 -->
      <el-form :inline="true" class="mb-4">
        <el-form-item label="课程名称：">
          <el-input v-model="search.teach_name" size="small" placeholder="请输入课程名称"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" size="small">查询</el-button>
          <el-button size="small" class="ml-2">重置</el-button>
        </el-form-item>
      </el-form>

      <!-- 高级查询折叠面板 -->
      <CollapsiblePanel
        title="高级查询"
        :default-collapsed="true"
        @change="handleAdvancedChange"
      >
        <el-form :inline="true" class="mb-4">
          <el-form-item label="公开属性：">
            <el-select v-model="search.public_attribute" filterable clearable collapse-tags multiple size="small">
              <el-option label="公开" value="1"></el-option>
              <el-option label="私有" value="0"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="课程状态：">
            <el-select v-model="search.state" filterable clearable collapse-tags multiple size="small">
              <el-option label="草稿" value="0"></el-option>
              <el-option label="已发布" value="1"></el-option>
              <el-option label="已下架" value="2"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="主讲人：">
            <el-select v-model="search.teacher_id" filterable clearable collapse-tags multiple size="small">
              <el-option label="张老师" value="1"></el-option>
              <el-option label="李老师" value="2"></el-option>
            </el-select>
          </el-form-item>
        </el-form>

        <el-form :inline="true">
          <el-form-item label="课程分类：">
            <div class="category-wrapper">
              <el-button
                v-for="category in categories"
                :key="category.id"
                :type="search.teach_category_ids.includes(category.id) ? 'primary' : 'default'"
                @click="handleCategoryClick(category.id)"
                size="small"
                class="mr-2"
              >
                {{ category.name }}
              </el-button>
            </div>
          </el-form-item>
        </el-form>
      </CollapsiblePanel>
    </div>

    <!-- 示例2: 风琴模式 -->
    <div class="mb-6">
      <h3 class="text-lg font-medium mb-4">示例2: 风琴模式</h3>

      <Accordion :accordion="true" :default-active-index="0">
        <CollapsiblePanel title="基础信息">
          <el-form :inline="true">
            <el-form-item label="姓名：">
              <el-input v-model="userInfo.name" size="small"></el-input>
            </el-form-item>
            <el-form-item label="年龄：">
              <el-input-number v-model="userInfo.age" size="small" :min="1" :max="120"></el-input-number>
            </el-form-item>
          </el-form>
        </CollapsiblePanel>

        <CollapsiblePanel title="联系方式">
          <el-form :inline="true">
            <el-form-item label="手机：">
              <el-input v-model="userInfo.phone" size="small"></el-input>
            </el-form-item>
            <el-form-item label="邮箱：">
              <el-input v-model="userInfo.email" size="small"></el-input>
            </el-form-item>
          </el-form>
        </CollapsiblePanel>

        <CollapsiblePanel title="地址信息">
          <el-form :inline="true">
            <el-form-item label="省份：">
              <el-select v-model="userInfo.province" size="small">
                <el-option label="北京" value="beijing"></el-option>
                <el-option label="上海" value="shanghai"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="详细地址：">
              <el-input v-model="userInfo.address" size="small" style="width: 300px;"></el-input>
            </el-form-item>
          </el-form>
        </CollapsiblePanel>
      </Accordion>
    </div>

    <!-- 示例3: 带操作按钮的面板 -->
    <div class="mb-6">
      <h3 class="text-lg font-medium mb-4">示例3: 带操作按钮的面板</h3>

      <CollapsiblePanel title="数据统计">
        <template #header-actions>
          <el-button size="small" type="primary" @click="refreshData">刷新</el-button>
          <el-button size="small" class="ml-2" @click="exportData">导出</el-button>
        </template>

        <template #header-extra>
          <el-tag size="small" type="success" class="ml-2">实时</el-tag>
        </template>

        <div class="grid grid-cols-3 gap-4">
          <div class="text-center p-4 bg-blue-50 rounded">
            <div class="text-2xl font-bold text-blue-600">{{ stats.total }}</div>
            <div class="text-sm text-gray-600">总数量</div>
          </div>
          <div class="text-center p-4 bg-green-50 rounded">
            <div class="text-2xl font-bold text-green-600">{{ stats.active }}</div>
            <div class="text-sm text-gray-600">活跃</div>
          </div>
          <div class="text-center p-4 bg-orange-50 rounded">
            <div class="text-2xl font-bold text-orange-600">{{ stats.inactive }}</div>
            <div class="text-sm text-gray-600">非活跃</div>
          </div>
        </div>
      </CollapsiblePanel>
    </div>
  </div>
</template>

<script>
import { CollapsiblePanel, Accordion } from '@/components/main-layout'

export default {
  name: 'ExampleComponent',
  components: {
    CollapsiblePanel,
    Accordion
  },
  data() {
    return {
      search: {
        teach_name: '',
        public_attribute: [],
        state: [],
        teacher_id: [],
        teach_category_ids: []
      },
      categories: [
        { id: 1, name: '内科' },
        { id: 2, name: '外科' },
        { id: 3, name: '儿科' },
        { id: 4, name: '妇产科' }
      ],
      userInfo: {
        name: '',
        age: 25,
        phone: '',
        email: '',
        province: '',
        address: ''
      },
      stats: {
        total: 1234,
        active: 856,
        inactive: 378
      }
    }
  },
  methods: {
    handleAdvancedChange(isCollapsed) {
      console.log('高级查询状态改变:', isCollapsed)
    },

    handleCategoryClick(categoryId) {
      if (this.search.teach_category_ids.includes(categoryId)) {
        this.search.teach_category_ids = this.search.teach_category_ids.filter(id => id !== categoryId)
      } else {
        this.search.teach_category_ids.push(categoryId)
      }
    },

    refreshData() {
      console.log('刷新数据')
      // 模拟数据刷新
      this.stats.total = Math.floor(Math.random() * 2000) + 1000
      this.stats.active = Math.floor(Math.random() * 1000) + 500
      this.stats.inactive = this.stats.total - this.stats.active
    },

    exportData() {
      console.log('导出数据')
    }
  }
}
</script>

<style scoped lang="less">
.example-container {
  padding: 20px;

  .category-wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }
}
</style>
