<template>
  <div class="tree-table">
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="id" label="ID"></el-table-column>
      <tree-table-column
        prop="name"
        label="Name"
        :data="tableData"
        @onExpand="onExpandNode"
        @onShrink="onShrinkNode"
        :asyn="true"
      ></tree-table-column>
      <el-table-column prop="parentId" label="ParentId"></el-table-column>
      <el-table-column prop="level" label="Level"></el-table-column>
      <el-table-column prop="noChild" label="NoChild">
        <template slot-scope="scope">{{scope.row.noChild ? '是': '否'}}</template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import TreeTableColumn from "./tree-table-column";
export default {
  name: 'TreeTableBasic',
  props: {
    msg: String
  },
  data () {
    return {
      tableData: [
        {
          id: 1, name: 'test-1', level: 1, parentId: 0
        },
        {
          id: 2, name: 'test-2', level: 1, parentId: 0
        }
      ]
    }
  },
  methods: {
    // 展开节点
    onExpandNode (row, index) {
      console.log('节点数据：', row);
      console.log('节点 index：', index);

      // 模拟的数据
      const mockData = [
        { id: row.id + '1', name: 'test-' + row.id + '1', level: row.level + 1, parentId: row.id },
        { id: row.id + '2', name: 'test-' + row.id + '2', level: row.level + 1, parentId: row.id },
        { id: row.id + '3', name: 'test-' + row.id + '3', level: row.level + 1, parentId: row.id, noChild: true },  // noChild 没有儿子节点 
      ]

      // 更新表格数据
      row.children = mockData;
      this.tableData = this.tableData.slice(0, index + 1).concat(mockData).concat(this.tableData.slice(index + 1));
    },
    // 收缩节点
    onShrinkNode (row, index, data) {
      console.log('表格数据：', data);
      console.log('节点数据：', row);
      console.log('节点 index：', index);

      // 更新表格数据
      this.tableData = data;
    }
  },
  components: {
    TreeTableColumn
  }
}
</script>

