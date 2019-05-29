<template>
  <div class="tree-table">
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="id" label="ID"></el-table-column>
      <tree-table-column prop="area" label="Area" :props="props" :data="tableData" @onClick="clickNode"></tree-table-column>
      <el-table-column prop="parentId" label="ParentId"></el-table-column>
      <el-table-column prop="level" label="Level"></el-table-column>
    </el-table>
  </div>
</template>

<script>
import TreeTableColumn from "@/components/tree-table-column";
export default {
  name: 'TreeTableBasic',
  data () {
    return {
      // 参数配置（以下是默认）
      props: {
        id: "id",
        parentId: "parentId",
        level: "level",
        children: "children",
        noChild: "noChild"
      },
      tableData: [
        {
          id: 1, area: '重庆市', level: 1, parentId: 0, children: [
            { id: 11, area: '渝中区', level: 2, parentId: 1 },
            { id: 12, area: '沙坪坝区', level: 2, parentId: 1 },
            { id: 13, area: '江北区', level: 2, parentId: 1 },
            { id: 14, area: '南岸区', level: 2, parentId: 1 }]
        },
        {
          id: 2, area: '四川省', level: 1, parentId: 0, children: [
            {
              id: 21, area: '锦江区', level: 2, parentId: 2, children: [
                { id: 211, area: '春熙路街道', level: 3, parentId: 21 },
                { id: 212, area: '三圣街道', level: 3, parentId: 21 }]
            },
            { id: 22, area: '武侯区', level: 2, parentId: 2 },
            { id: 23, area: '金牛区', level: 2, parentId: 2 },
            { id: 24, area: '青羊区', level: 2, parentId: 2 },
            { id: 25, area: '成华区', level: 2, parentId: 2 }
          ]
        }
      ]
    }
  },
  methods: {
    // 点击节点
    clickNode (row, index, data) {
      // 更新表格数据
      this.tableData = data;
    }
  },
  components: {
    TreeTableColumn
  }
}
</script>

