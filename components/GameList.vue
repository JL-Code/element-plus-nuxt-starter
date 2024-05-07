<template>
  <el-table
    highlight-current-row
    :data="tableData"
    @row-click="handleRowClick"
    row-key="id"
    :current-row-key="currentRowKey"
    style="width: 100%"
  >
    <el-table-column prop="name" label="名称" sortable />
    <el-table-column fixed="right" align="right" width="100">
      <template #default>
        <el-button link type="primary" size="small" @click="handleClick">
          详情
        </el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script lang="ts" setup>
const $emits = defineEmits(["game-change"]);
const handleClick = () => {};
const handleRowClick = (row: any) => {
  console.log("game list row click", row);
  $emits("game-change", row.name);
};
const currentRowKey = ref();
const tableData = ref<any[]>([]);
onMounted(() => {
  tableData.value = [
    {
      id: 1,
      name: "Diablo4",
    },
    {
      id: 2,
      name: "New World",
    },
  ];
  currentRowKey.value = tableData.value[0].id;
});
</script>
