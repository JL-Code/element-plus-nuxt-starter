<template>
  <el-tree
    highlight-current
    v-loading="loading"
    :data="data"
    :props="defaultProps"
    node-key="id"
    default-expand-all
    :current-node-key="currentNodeKey"
    @node-click="handleNodeClick"
  />
</template>

<script lang="ts" setup>
import { GamePropContext } from "~/pages/examples/type";

const props = defineProps<{
  gamePropContext: GamePropContext;
}>();
const $emits = defineEmits(["click"]);
const data = ref<Tree[]>([]);
interface Tree {
  id: number;
  label: string;
  children?: Tree[];
}

const handleNodeClick = (data: Tree) => {
  console.log(data.label);
  $emits("click", data.label);
};
const loading = ref(false);
const currentNodeKey = ref<number>();
const loadData = (game: string) => {
  console.log("[BackendCategoryList] loadData", game);
  loading.value = true;
  data.value = [
    {
      id: 0,
      label: "Common",
      children:
        game === "Diablo4"
          ? [
              {
                id: 1,
                label: "Currency",
              },
              {
                id: 2,
                label: "Account",
              },
              {
                id: 3,
                label: "Items",
              },
              {
                id: 4,
                label: "Boosting",
              },
            ]
          : [
              {
                id: 1,
                label: "Currency",
              },
              {
                id: 2,
                label: "Account",
              },
            ],
    },
  ];

  setTimeout(() => {
    // 默认选中第一个节点
    currentNodeKey.value = data.value[0].id;
    loading.value = false;
  }, 400);
};

defineExpose({
  loadData,
});

watchEffect(() => {
  console.log("game change");
  loadData(props.gamePropContext.game);
});
const defaultProps = {
  children: "children",
  label: "label",
};
</script>
