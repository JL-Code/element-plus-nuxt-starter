<template>
  <div class="flex py-2">
    <div class="flex-grow"></div>
    <div class="">
      <el-button type="primary">添加</el-button>
    </div>
  </div>
  <el-table :data="tableData" border style="width: 100%">
    <el-table-column prop="code" label="编码" />
    <el-table-column prop="name" label="名称" />
    <el-table-column prop="alias" label="别名" />
    <el-table-column prop="sort" label="排序" />
    <el-table-column fixed="right" label="操作">
      <template #default>
        <el-button link type="primary" size="small" @click="handleClick">
          详情
        </el-button>
        <el-button link type="primary" size="small">移除</el-button>
      </template>
    </el-table-column>
  </el-table>

  <el-drawer v-model="visible" :show-close="false" size="40%">
    <template #header="{ close, titleId, titleClass }">
      <h4 :id="titleId" :class="titleClass">This is a custom header!</h4>
      <el-button type="danger" @click="close">
        <el-icon class="el-icon--left"><CircleCloseFilled /></el-icon>
        Close
      </el-button>
    </template>
    This is drawer content.
  </el-drawer>
</template>

<script lang="ts" setup>
import { CircleCloseFilled } from "@element-plus/icons-vue";
import { GamePropContext } from "~/pages/examples/type";

const props = defineProps<{ propContext: GamePropContext }>();

const visible = ref(false);
const handleClick = () => {
  console.log("click");
  visible.value = true;
};

const tableData = [
  {
    code: "Delivery-Method",
    name: "Delivery Method",
    alias: "Delivery Method",
    sort: 12,
    description: "Los Angeles",
  },
];
</script>
