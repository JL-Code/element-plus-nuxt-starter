<template>
  <el-table
    ref="tableRef"
    :data="regions"
    row-key="id"
    border
    default-expand-all
    style="width: 100%"
    @row-click="handleRowClick"
  >
    <el-table-column prop="value" label="名称" />
    <el-table-column prop="initial" width="100" label="首字母" />
    <el-table-column prop="name" width="180" label="类型" />
    <el-table-column prop="status" width="100" label="状态" />
    <el-table-column prop="sort" width="100" label="排序" />
    <el-table-column prop="createdAt" width="100" label="创建时间" />
  </el-table>
</template>

<script lang="ts" setup>
import serverData, { diablo4 } from "./server";
const props = defineProps(["game"]);
const regions = ref();
const tableRef = ref();

const handleRowClick = (row: any) => {
  tableRef.value?.toggleRowExpansion(row);
};

watch(
  () => props.game,
  () => {
    console.log("game", props.game);
    if (props.game === "Diablo 4") {
      regions.value = diablo4.tradeEnvironments.filter(
        (m) => m.name === "Game Mode"
      );
    } else {
      regions.value = serverData.tradeEnvironments.filter(
        (m) => m.name === "Region"
      );
    }
    regions.value.forEach((m) => {
      if (m.childTradeEnvironments) {
        m.children = [
          ...m.childTradeEnvironments.filter((n) => n.name === "Realm"),
        ];
      }
    });
  }
);

console.log("regions", regions);
</script>
