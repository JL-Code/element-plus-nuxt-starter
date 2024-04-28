<script setup lang="ts">
const game = ref("");
const handleGame = (name: string) => {
  game.value = name;
};
</script>

<template>
  <el-container style="height: 500px; padding: 24px">
    <el-aside style="background: red">
      <el-scrollbar>
        <GameList @active-game="handleGame" />
      </el-scrollbar>
    </el-aside>
    <el-main style="background: blue">
      <el-scrollbar>
        <Table :game="game" />
      </el-scrollbar>
    </el-main>
  </el-container>
</template>
