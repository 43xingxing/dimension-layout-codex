<template>
  <div class="dimension-panel">
    <el-tabs class="panel-tabs" v-model="activeTab">
      <el-tab-pane label="数据" name="data">
        <div class="tab-content">
          <div class="section-header">
            <span class="section-title">起始位置</span>
          </div>
          <div class="start-input">
            <el-input v-model="startPosition" placeholder="输入起始位置" size="small" />
          </div>

          <div class="dimension-sections">
            <div class="dimension-block">
              <div class="section-header">
                <span class="section-title">页面维</span>
                <span class="section-actions">
                  <el-checkbox v-model="dimensions.page.scroll">滚动</el-checkbox>
                </span>
              </div>
              <div class="drop-area">
                <draggable
                  v-model="dimensions.page.items"
                  group="dimensions"
                  handle=".drag-handle"
                  @end="handleDragEnd"
                >
                  <transition-group name="fade" tag="div" class="chips">
                    <div v-for="item in dimensions.page.items" :key="item.id" class="chip">
                      <span class="drag-handle">
                        <span class="handle-dot" />
                        <span class="handle-dot" />
                        <span class="handle-dot" />
                        <span class="handle-dot" />
                        <span class="handle-dot" />
                        <span class="handle-dot" />
                      </span>
                      <span class="chip-label">{{ item.label }}</span>
                      <i class="el-icon-close" @click="handleGroupClose('page', item.id)" />
                    </div>
                  </transition-group>
                </draggable>
                <div v-if="!dimensions.page.items.length" class="empty">拖拽字段到此处</div>
              </div>
            </div>

            <div class="dimension-block">
              <div class="section-header">
                <span class="section-title">行维</span>
                <div class="section-actions">
                  <el-button size="mini" type="text" @click="handleIconClick('rows')">
                    <i class="el-icon-plus" /> 添加分组
                  </el-button>
                </div>
              </div>
              <div class="scroll-wrapper">
                <div class="scroll-arrow left" @click="handleScrollArrowsClick('rows', 'left')">
                  <i class="el-icon-arrow-left" />
                </div>
                <div class="groups" ref="rowsGroups">
                  <div v-for="group in dimensions.rows.groups" :key="group.id" class="group-card">
                    <div class="group-header">
                      <span class="group-title">{{ group.title }}</span>
                      <div class="group-actions">
                        <el-button size="mini" type="text" @click="toggleItemExpand('rows', group.id)">
                          <i :class="group.expanded ? 'el-icon-arrow-down' : 'el-icon-arrow-right'" />
                        </el-button>
                        <el-button size="mini" type="text" @click="handleGroupClose('rows', group.id)">
                          <i class="el-icon-close" />
                        </el-button>
                      </div>
                    </div>
                    <div v-show="group.expanded" class="group-content">
                      <draggable
                        v-model="group.items"
                        group="dimensions"
                        handle=".drag-handle"
                        @end="handleDragEnd"
                      >
                        <transition-group name="fade" tag="div" class="chips">
                          <div v-for="item in group.items" :key="item.id" class="chip">
                            <span class="drag-handle">
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                            </span>
                            <span class="chip-label">{{ item.label }}</span>
                            <i class="el-icon-close" @click="handleGroupClose('rows', item.id)" />
                          </div>
                        </transition-group>
                      </draggable>
                      <div v-if="!group.items.length" class="empty">拖拽字段到此分组</div>
                    </div>
                  </div>
                </div>
                <div class="scroll-arrow right" @click="handleScrollArrowsClick('rows', 'right')">
                  <i class="el-icon-arrow-right" />
                </div>
              </div>
              <div class="more-menu">
                <el-button size="mini" type="text" @click="showMoreMenu('rows')">更多</el-button>
                <div v-if="dimensions.rows.showMore" class="more-dropdown">
                  <div v-for="group in dimensions.rows.groups" :key="group.id" class="more-item">
                    {{ group.title }}
                  </div>
                </div>
              </div>
            </div>

            <div class="dimension-block">
              <div class="section-header">
                <span class="section-title">列维</span>
                <div class="section-actions">
                  <el-button size="mini" type="text" @click="handleIconClick('cols')">
                    <i class="el-icon-plus" /> 添加分组
                  </el-button>
                </div>
              </div>
              <div class="scroll-wrapper">
                <div class="scroll-arrow left" @click="handleScrollArrowsClick('cols', 'left')">
                  <i class="el-icon-arrow-left" />
                </div>
                <div class="groups" ref="colsGroups">
                  <div v-for="group in dimensions.cols.groups" :key="group.id" class="group-card">
                    <div class="group-header">
                      <span class="group-title">{{ group.title }}</span>
                      <div class="group-actions">
                        <el-button size="mini" type="text" @click="toggleItemExpand('cols', group.id)">
                          <i :class="group.expanded ? 'el-icon-arrow-down' : 'el-icon-arrow-right'" />
                        </el-button>
                        <el-button size="mini" type="text" @click="handleGroupClose('cols', group.id)">
                          <i class="el-icon-close" />
                        </el-button>
                      </div>
                    </div>
                    <div v-show="group.expanded" class="group-content">
                      <draggable
                        v-model="group.items"
                        group="dimensions"
                        handle=".drag-handle"
                        @end="handleDragEnd"
                      >
                        <transition-group name="fade" tag="div" class="chips">
                          <div v-for="item in group.items" :key="item.id" class="chip">
                            <span class="drag-handle">
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                              <span class="handle-dot" />
                            </span>
                            <span class="chip-label">{{ item.label }}</span>
                            <i class="el-icon-close" @click="handleGroupClose('cols', item.id)" />
                          </div>
                        </transition-group>
                      </draggable>
                      <div v-if="!group.items.length" class="empty">拖拽字段到此分组</div>
                    </div>
                  </div>
                </div>
                <div class="scroll-arrow right" @click="handleScrollArrowsClick('cols', 'right')">
                  <i class="el-icon-arrow-right" />
                </div>
              </div>
              <div class="more-menu">
                <el-button size="mini" type="text" @click="showMoreMenu('cols')">更多</el-button>
                <div v-if="dimensions.cols.showMore" class="more-dropdown">
                  <div v-for="group in dimensions.cols.groups" :key="group.id" class="more-item">
                    {{ group.title }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </el-tab-pane>
      <el-tab-pane label="属性" name="settings">
        <div class="tab-content placeholder">
          <span>属性配置区域</span>
        </div>
      </el-tab-pane>
    </el-tabs>

    <div class="footer-toolbar">
      <el-button size="small" @click="handleIconClick('cancel')">取消</el-button>
      <el-button size="small" type="primary" @click="handleIconClick('confirm')">确认</el-button>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'DimensionPanel',
  components: {
    draggable
  },
  data() {
    return {
      activeTab: 'data',
      startPosition: '',
      dimensions: {
        page: {
          scroll: true,
          items: [
            { id: 'page-1', label: '日期' },
            { id: 'page-2', label: '区域' }
          ]
        },
        rows: {
          showMore: false,
          groups: [
            {
              id: 'row-group-1',
              title: '行分组 1',
              expanded: true,
              items: [
                { id: 'row-1', label: '产品线' },
                { id: 'row-2', label: '业务类型' }
              ]
            },
            {
              id: 'row-group-2',
              title: '行分组 2',
              expanded: true,
              items: [
                { id: 'row-3', label: '渠道' },
                { id: 'row-4', label: '客户等级' }
              ]
            },
            {
              id: 'row-group-3',
              title: '行分组 3',
              expanded: true,
              items: [
                { id: 'row-5', label: '合同类型' },
                { id: 'row-6', label: '订单状态' }
              ]
            },
            {
              id: 'row-group-4',
              title: '行分组 4',
              expanded: true,
              items: [
                { id: 'row-7', label: '省份' },
                { id: 'row-8', label: '城市' }
              ]
            }
          ]
        },
        cols: {
          showMore: false,
          groups: [
            {
              id: 'col-group-1',
              title: '列分组 1',
              expanded: true,
              items: [
                { id: 'col-1', label: '月份' },
                { id: 'col-2', label: '季度' }
              ]
            },
            {
              id: 'col-group-2',
              title: '列分组 2',
              expanded: true,
              items: [
                { id: 'col-3', label: '周次' },
                { id: 'col-4', label: '节假日' }
              ]
            },
            {
              id: 'col-group-3',
              title: '列分组 3',
              expanded: true,
              items: [
                { id: 'col-5', label: '指标' },
                { id: 'col-6', label: '子指标' }
              ]
            },
            {
              id: 'col-group-4',
              title: '列分组 4',
              expanded: true,
              items: [
                { id: 'col-7', label: '目标' },
                { id: 'col-8', label: '完成率' }
              ]
            }
          ]
        }
      }
    }
  },
  methods: {
    handleDragEnd(event) {
      console.log('drag end', event)
    },
    handleGroupClose(section, id) {
      console.log('close', section, id)
    },
    toggleItemExpand(section, id) {
      const group = this.dimensions[section].groups.find((item) => item.id === id)
      if (group) {
        group.expanded = !group.expanded
      }
      console.log('toggle expand', section, id)
    },
    handleIconClick(action) {
      console.log('icon click', action)
    },
    showMoreMenu(section) {
      this.dimensions[section].showMore = !this.dimensions[section].showMore
      console.log('show more', section)
    },
    handleScrollArrowsClick(section, direction) {
      const target = section === 'rows' ? this.$refs.rowsGroups : this.$refs.colsGroups
      if (target) {
        const offset = direction === 'left' ? -160 : 160
        target.scrollLeft += offset
      }
      console.log('scroll', section, direction)
    }
  }
}
</script>

<style scoped>
.dimension-panel {
  display: flex;
  flex-direction: column;
  height: 100%;
  font-size: 14px;
  color: #2c3e50;
}

.panel-tabs {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.tab-content {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  height: calc(100vh - 150px);
  overflow: auto;
}

.tab-content.placeholder {
  align-items: center;
  justify-content: center;
  color: #909399;
}

.section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.section-title {
  font-size: 15px;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  gap: 2px;
}

.start-input {
  max-width: 240px;
}

.dimension-sections {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.dimension-block {
  border: 1px solid #ebeef5;
  border-radius: 8px;
  padding: 12px;
  background: #fff;
}

.drop-area {
  margin-top: 10px;
  min-height: 56px;
}

.scroll-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 10px;
}

.groups {
  display: flex;
  gap: 12px;
  overflow-x: auto;
  padding: 4px 0;
  scroll-behavior: smooth;
}

.group-card {
  min-width: 220px;
  border: 1px solid #e4e7ed;
  border-radius: 6px;
  padding: 8px;
  background: #fafafa;
}

.group-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: 600;
}

.group-title {
  font-size: 14px;
}

.group-actions {
  display: flex;
  align-items: center;
  gap: 2px;
}

.group-content {
  margin-top: 8px;
}

.chips {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.chip {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 8px;
  border-radius: 4px;
  background: #f5f7fa;
  border: 1px solid #e4e7ed;
}

.drag-handle {
  display: grid;
  grid-template-columns: repeat(2, 4px);
  grid-template-rows: repeat(3, 4px);
  gap: 2px;
  cursor: move;
}

.handle-dot {
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: #909399;
}

.chip-label {
  flex: 1;
  font-size: 13px;
}

.empty {
  padding: 8px 0;
  color: #c0c4cc;
  font-size: 12px;
}

.scroll-arrow {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: #f2f6fc;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.more-menu {
  display: flex;
  justify-content: flex-end;
  position: relative;
  margin-top: 4px;
}

.more-dropdown {
  position: absolute;
  right: 0;
  top: 28px;
  background: #fff;
  border: 1px solid #e4e7ed;
  border-radius: 6px;
  min-width: 160px;
  max-height: 180px;
  overflow: auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.12);
}

.more-item {
  padding: 8px 12px;
  font-size: 13px;
}

.footer-toolbar {
  position: sticky;
  bottom: 0;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 12px;
  padding: 0 16px;
  border-top: 1px solid #ebeef5;
  background: #fff;
}
</style>
