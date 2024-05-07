<template>
  <div class="flex gap-x-4 h-full">
    <el-card class="w-72">
      <template #header>
        <div class="flex items-center justify-between">
          <p>游戏</p>
          <el-button @click="openGameDialog = true">新增</el-button>
        </div>
      </template>
      <GameList @game-change="handleGameChange" />
    </el-card>
    <el-card class="w-72">
      <template #header>
        <div class="flex items-center justify-between">
          <p>后端类目</p>
          <el-button @click="openBackendCategoryDialog">关联</el-button>
        </div>
      </template>
      <BackendCategoryList
        ref="backendCategoryListRef"
        @click="handleCategoryClick"
        :game-prop-context="propContext"
      />
    </el-card>
    <el-card class="flex-1">
      <el-tabs v-model="activeName" class="demo-tabs">
        <el-tab-pane label="级联属性" name="first">
          <CascadeAttrTable :prop-context="propContext" />
        </el-tab-pane>
        <el-tab-pane label="基础属性" name="second">
          <BaseAttrTable :prop-context="propContext" />
        </el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
  <!-- 后端类目 -->
  <el-drawer
    v-model="openBackendCategoryDialogValue"
    destroy-on-close
    size="40%"
    title="后端类目"
    @opened="toggleSelection(['Gold', 'Account'])"
  >
    <el-table
      :data="categoryList"
      ref="categoryListRef"
      style="width: 100%; margin-bottom: 20px"
      row-key="id"
      border
      default-expand-all
    >
      <el-table-column type="selection" width="55" align="center" />
      <el-table-column prop="code" label="编码" sortable />
      <el-table-column prop="name" label="名称" sortable />
      <el-table-column prop="sort" label="排序" sortable />
    </el-table>
    <template #footer>
      <div class="flex-auto">
        <el-button @click="openBackendCategoryDialogValue = false"
          >cancel</el-button
        >
        <el-button type="primary" @click="confirmCategory()">confirm</el-button>
      </div>
    </template>
  </el-drawer>
  <!-- 游戏详情 -->
  <el-drawer
    v-model="openGameDialog"
    title="新增游戏"
    size="40%"
    destroy-on-close
  >
    <el-form :model="form" label-width="auto" style="max-width: 600px">
      <el-form-item label="名称" required>
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="编码">
        <el-input v-model="form.code" />
      </el-form-item>
      <el-form-item label="简称">
        <el-input v-model="form.short" />
      </el-form-item>
      <el-form-item label="首字母">
        <el-input v-model="form.initial" />
      </el-form-item>
      <el-form-item label="排序">
        <el-input-number v-model="form.sort" />
      </el-form-item>
      <el-form-item label="状态">
        <el-switch
          v-model="form.status"
          inline-prompt
          active-text="启用"
          inactive-text="禁用"
        />
      </el-form-item>
    </el-form>
    <template #footer>
      <div class="flex-auto">
        <el-button @click="openGameDialog = false">cancel</el-button>
        <el-button type="primary" @click="onSubmit">confirm</el-button>
      </div>
    </template>
  </el-drawer>
</template>

<script setup lang="ts">
import { ElTable } from "element-plus";
import { GamePropContext } from "./type";
import { BackendCategoryList } from "#components";

const activeName = ref("first");
const openGameDialog = ref(false);
const openBackendCategoryDialogValue = ref(false);
const propContext = ref<GamePropContext>({
  game: "",
  category: "",
});
const categoryList = ref([
  {
    code: "Gold",
    name: "Currency",
    sort: 1,
  },
  {
    code: "Account",
    name: "Account",
    sort: 1,
  },
  {
    code: "Items",
    name: "Items",
    sort: 1,
  },
  {
    code: "Boosting",
    name: "Boosting",
    sort: 1,
  },
  {
    code: "TopUp",
    name: "TopUp",
    sort: 1,
  },
]);

// do not use same name with ref
const form = reactive({
  name: "",
  region: "",
  date1: "",
  date2: "",
  delivery: false,
  type: [],
  resource: "",
  desc: "",
});

const onSubmit = () => {
  console.log("submit!");
  openGameDialog.value = false;
};

const handleGameChange = (value: string) => {
  console.log("game", value);
  propContext.value.game = value;
};
const handleCategoryClick = (value: string) => {
  propContext.value.category = value;
};
const categoryListRef = ref<InstanceType<typeof ElTable>>();
const backendCategoryListRef = ref<InstanceType<typeof BackendCategoryList>>();
const confirmCategory = () => {
  const rows = categoryListRef.value?.getSelectionRows();
  console.log("selected rows", JSON.parse(JSON.stringify(rows)));
  openBackendCategoryDialogValue.value = false;
  // TODO: api request
  // categoryList.value = JSON.parse(JSON.stringify(rows));
  backendCategoryListRef.value!.loadData(propContext.value.game);
};

const categorySelection = ref<any[]>([]);
const toggleSelection = (rows?: any[]) => {
  const _rows = categoryList.value.filter((m) => rows?.includes(m.code));
  console.log("_rows", _rows);
  if (_rows) {
    _rows.forEach((row) => {
      // eslint-disable-next-line @typescript-eslint/ban-ts-comment
      // @ts-expect-error
      categoryListRef.value!.toggleRowSelection(row, undefined);
    });
  } else {
    categoryListRef.value!.clearSelection();
  }
};
const openBackendCategoryDialog = () => {
  openBackendCategoryDialogValue.value = true;
  // TODO: 类目数据重入
};
</script>

<style lang="scss" scoped></style>
