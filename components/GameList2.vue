<template>
  <el-table
    :data="tableData"
    border
    style="width: 100%"
    @row-click="handleRowClick"
    highlight-current-row
  >
    <el-table-column prop="name"  label="游戏" />
    <!-- <el-table-column prop="status" label="状态" /> -->
  </el-table>
</template>

<script lang="ts" setup>
import games from "./game";
const emits = defineEmits(["active-game"]);

const gameMap = games.reduce((acc, obj) => {
  const { itemTreeType, ...rest } = obj;
  if (acc.has(itemTreeType)) {
    const arr = acc.get(itemTreeType);
    arr.push(rest);
  } else {
    acc.set(itemTreeType, [rest]);
  }
  return acc;
}, new Map());

console.log("gameMap", gameMap);

const tableData = gameMap.get("Currency").map((g: any) => {
  return {
    name: g.gameName,
    status: "启用",
  };
});

const handleRowClick = (row: any) => {
  console.log("click game", row.name);
  emits("active-game", row.name);
};
</script>
