<template>
  <div class="flex py-2">
    <div class="flex-grow"></div>
    <div class="">
      <el-button type="primary">添加</el-button>
    </div>
  </div>
  <el-table
    v-loading="loading"
    :data="data"
    style="width: 100%; margin-bottom: 20px"
    row-key="id"
    border
    default-expand-all
  >
    <el-table-column prop="name" label="名称" sortable />
    <el-table-column prop="type" label="类型" width="120" sortable />
    <el-table-column prop="sort" label="排序" sortable />
    <el-table-column fixed="right" label="操作" width="220">
      <template #default>
        <el-button link type="primary" size="small" @click="handleClick">
          详情
        </el-button>
        <el-button link type="danger" size="small">删除</el-button>
        <el-button link type="primary" size="small">添加</el-button>
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
interface GameServer {
  id: number;
  name: string;
  type: string;
  sort: number;
  hasChildren?: boolean;
  children?: GameServer[];
}

const props = defineProps<{ propContext: GamePropContext }>();

const visible = ref(false);
const handleClick = (row: any) => {
  visible.value = true;
};
const data = ref<GameServer[]>([]);
const tableData: GameServer[] = [
  {
    id: 1,
    name: "EU",
    type: "Region",
    sort: 1,
  },
  {
    id: 2,
    name: "US",
    type: "Region",
    sort: 2,
  },
];

const tableData1: GameServer[] = [
  {
    id: 1,
    name: "EU",
    type: "Region",
    sort: 1,
    children: [
      {
        id: 11,
        name: "Aegwynn",
        type: "Server",
        sort: 1,
      },
      {
        id: 12,
        name: "Goldrinn",
        type: "Server",
        sort: 1,
      },
    ],
  },
  {
    id: 2,
    name: "US",
    type: "Region",
    sort: 2,
  },
];

const loading = ref(false);
const loadData = (context: GamePropContext) => {
  loading.value = true;
  if (context?.game === "Diablo4" && context?.category === "Currency") {
    data.value = tableData;
  } else {
    data.value = tableData1;
  }
  setTimeout(() => {
    loading.value = false;
  }, 600);
};

watchEffect(() => {
  console.log(
    "cascade change",
    props.propContext.game,
    props.propContext.category
  );
  loadData(props.propContext);
});
</script>
