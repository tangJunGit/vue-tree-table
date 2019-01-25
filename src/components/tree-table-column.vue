<template>
  <el-table-column :prop="prop" v-bind="$attrs">
    <template slot-scope="scope">
      <span
        @click.prevent="toggleHandle(scope.$index, scope.row)"
        :style="paddingStyles(scope.row)"
      >
        <i :class="iconClasses(scope.row)" :style="iconStyles(scope.row)"></i>
        {{ scope.row[prop] }}
      </span>
    </template>
  </el-table-column>
</template>
<script>
export default {
  name: 'TreeTableColumn',
  props: {
    prop: {
      type: String
    },
    id: {
      type: String,
      default: 'id'
    },
    parentId: {
      type: String,
      default: 'parentId'
    },
    level: {
      type: String,
      default: 'level'
    },
    children: {
      type: String,
      default: 'children'
    }
  },
  methods: {
    // 展开收缩操作
    toggleHandle (index, row) {
      if (this.hasChild(row)) {
        let data = this.$parent.store.states.data.slice(0); /// 获取table数据
        let d = data[index];
        d._expanded = !d._expanded
        if (d._expanded) {
          data = this.addChildNode(data, row, index);  // 添加table子节点数据
        } else {
          data = this.removeChildNode(data, row[this.id])    // 删除table子节点数据
        }

        // 更新
        this.$parent.store.commit('setData', data)
        this.$nextTick(() => {
          this.$parent.doLayout()
        })

      }
    },
    // 添加子节点
    addChildNode (data, row, index) {
      row = this.hiddenExpanded(row);   // 添加时隐藏展开状态
      return data.splice(0, index + 1).concat(row[this.children]).concat(data)
    },
    // 隐藏展开状态
    hiddenExpanded (node) {
      let children = node[this.children];
      if (!children || children.length === 0) return;

      for (let i = 0; i < children.length; i++) {
        let c = children[i];
        c._expanded = false;
        this.hiddenExpanded(c);
      }
      return node;
    },
    // 移除子节点
    removeChildNode (data, parentId) {
      let parentIds = Array.isArray(parentId) ? parentId : [parentId]
      if (parentId.length === 0) {
        return data
      }

      let ids = []
      for (let i = 0; i < data.length; i++) {
        let d = data[i];

        // 匹配相同 parentId ，记录节点id，并递归
        if (parentIds.indexOf(d[this.parentId]) !== -1 && parentIds.indexOf(d[this.id]) === -1) {
          ids.push(data.splice(i, 1)[0][this.id])   // 删除匹配的节点，并记录id，用于递归子节点
          i--
        }
      }
      return this.removeChildNode(data, ids)
    },
    // 等级样式
    paddingStyles (row) {
      return { 'padding-left': (row[this.level] > 1 ? row[this.level] * 8 : 0) + 'px' }
    },
    // icon样式
    iconClasses (row) {
      return [row._expanded ? 'el-icon-caret-bottom' : 'el-icon-caret-right']
    },
    // 没有儿子节点不显示icon
    iconStyles (row) {
      return { 'visibility': (this.hasChild(row) ? 'visible' : 'hidden') }
    },
    // 判断是否有儿子节点
    hasChild (row) {
      return (Array.isArray(row[this.children]) && row[this.children].length) || false
    },
  }
}
</script>

