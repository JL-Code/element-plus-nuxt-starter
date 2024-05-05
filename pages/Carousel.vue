<script setup>
const imgs = [
  "https://img.gameusd.com/gameusd/latest/banner/swtor.jpg",
  "https://img.gameusd.com/gameusd/latest/banner/Affiliate.png",
  "https://img.gameusd.com/gameusd/latest/banner/sab.jpg",
  "https://img.gameusd.com/gameusd/latest/banner/wow.jpg",
];
</script>
<template>
  <el-carousel :interval="4000" type="card" height="400px">
    <el-carousel-item v-for="item in imgs" :key="item">
      <el-image :src="item" />
    </el-carousel-item>
  </el-carousel>
</template>

<style scoped></style>
