<template>
  <el-table-column :prop="prop" v-bind="$attrs">
    <template slot-scope="scope">
      <span :style="paddingStyles(scope.row)">
        <i
          @click.prevent="toggleHandle(scope.row,scope.$index)"
          :class="iconClasses(scope.row)"
          :style="iconStyles(scope.row)"
        ></i>
        {{ scope.row[prop] }}
      </span>
    </template>
  </el-table-column>
</template>
<script>
export default {
  name: 'TreeTableColumn',
  props: {
    asyn: {
      type: Boolean,
      default: false
    },
    data: {
      type: Array,
      default: () => []
    },
    prop: {
      type: String,
      prop: ''
    },
    props: {
      type: Object,
      default: () => {
        return {
          id: "id",
          parentId: "parentId",
          level: "level",
          children: "children",
          noChild: "noChild"
        }
      }
    }
    
  },
  methods: {
    // 展开收缩操作
    toggleHandle (row, index) {
      let data;
      row._expanded = !row._expanded;

      if (this.hasChild(row)) {
        if (row._expanded) {
          data = this.addChildNode(this.data, row, index);  // 添加table子节点数据
          this.$emit('onExpand', row, index, data);
        } else {
          data = this.removeChildNode(this.data, row[this.props.id])    // 删除table子节点数据
          this.$emit('onShrink', row, index, data);
        }
      } else {
        if (this.asyn) {
          this.$emit('onExpand', row, index);
        }
      }

      this.$emit('onClick', row, index, data);   // 点击节点事件
    },
    // 添加子节点
    addChildNode (data, row, index) {
      row = this.hiddenExpanded(row);   // 添加时隐藏展开状态
      return data.slice(0, index + 1).concat(row[this.props.children]).concat(data.slice(index + 1))
    },
    // 隐藏展开状态
    hiddenExpanded (node) {
      let children = node[this.props.children];
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
        if (parentIds.indexOf(d[this.props.parentId]) !== -1 && parentIds.indexOf(d[this.props.id]) === -1) {
          ids.push(data.splice(i, 1)[0][this.props.id])   // 删除匹配的节点，并记录id，用于递归子节点
          i--
        }
      }
      return this.removeChildNode(data, ids)
    },
    // 等级样式
    paddingStyles (row) {
      return { 'padding-left': (row[this.props.level] > 1 ? row[this.props.level] * 8 : 0) + 'px' }
    },
    // icon样式
    iconClasses (row) {
      return [row._expanded ? 'el-icon-caret-bottom' : 'el-icon-caret-right']
    },
    // 没有儿子节点不显示icon
    iconStyles (row) {
      return { 'visibility': (this.hasChild(row) || (this.asyn && !row[this.props.noChild]) ? 'visible' : 'hidden') }
    },
    // 判断缓存中是否有儿子节点
    hasChild (row) {
      return (Array.isArray(row[this.props.children]) && row[this.props.children].length) || false
    },
  }
}
</script>

