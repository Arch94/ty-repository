# TyTable 组件文档

## 简介

`TyTable` 是一个用于展示数据的表格组件, 它基于el-table，支持分页、数据请求、插槽自定义等功能。它可以通过传递 API 函数和参数来动态获取和显示数据。

## 引入组件

import TyTable from "@evaluation-ui/shared-components/TyTable/index.vue";

## 属性

- **`ref`**: `proTable`
  - 用于获取 `TyTable` 实例，以便调用其方法。

- **`search-init-param`**: `Object`
  - 初始搜索参数，用于请求数据时的默认参数。

- **`columns`**: `Array`
  - 表格列的配置数组，每个对象包含以下属性：
    - `label`: 列的显示名称。
    - `prop`: 列对应的数据字段。
    - `slotName`: 自定义插槽名称（可选）。
    - `width`: 列宽度（可选）。
    - `fixed`: 是否固定列（可选）。

- **`requestApi`**: `Function`
  - 数据请求的 API 函数，返回一个 Promise。

- **`search-before`**: `Function`
  - 请求前的数据处理函数，接收请求参数并返回处理后的参数。

- **`requestAuto`**: `Boolean`
  - 是否自动请求数据，默认为 `false`。

- **`data-callback`**: `Function`
  - 数据请求后的回调函数，接收请求返回的数据并返回一个包含 `list` 和 `total` 的对象。


## 方法

- **`getTableList`**: 刷新表格数据
  - 调用 `proTable.value.getTableList()` 来刷新表格数据。

## 使用示例

``` js
<template>
  <TyTable
    ref="proTable"
    :search-init-param="queryParams"
    :columns="columns"
    :requestApi="EvalItemApi.getWeekList"
    :search-before="searchBefore"
    :requestAuto="false"
    :data-callback="dataCallback"
  >
    <!-- 自定义学期显示 -->
    <template #term="{ row }">
      {{ row.term === "01" ? "上学期" : "下学期" }}
    </template>
    
    <!-- 自定义周次日期显示 -->
    <template #weekDate="{ row }">
      第{{ row.weekIndex }}周 ({{ row.startTime }} - {{ row.endTime }})
    </template>
    
    <!-- 自定义值周老师显示 -->
    <template #teacherNameList="{ row }">
      {{ row.teacherNameList.join(",") }}
    </template>
    
    <!-- 自定义操作列 -->
    <template #edit="{ row }">
      <el-button type="text" @click="openAdjustDialog(row)">
        调整时间
      </el-button>
      <el-button type="text" @click="changeWeekSetTeacher(row)">
        调整值周老师
      </el-button>
    </template>
  </TyTable>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import TyTable from "@evaluation-ui/shared-components/TyTable/index.vue";
import EvalItemApi from "@mop/api/evalItem";

// 定义表格列
const columns = ref([
  {
    label: "学年",
    prop: "schoolYear",
    width: 200,
  },
  {
    label: "学期",
    prop: "term",
    slotName: "term",
    width: 200,
  },
  {
    label: "时间",
    prop: "weekDate",
    slotName: "weekDate",
  },
  {
    label: "值周老师",
    prop: "teacherNameList",
    slotName: "teacherNameList",
  },
  {
    label: "操作",
    prop: "edit",
    width: 200,
    fixed: "right",
  },
]);

// 定义查询参数
const queryParams = reactive({
  page: 1,
  limit: 10,
  schoolYear: "",
  term: "",
  teacherName: "",
});

// 数据回调函数
const dataCallback = (data: any) => {
  return {
    list: data.data.records,
    total: +data.data.total,
  };
};

// 搜索前处理函数
const searchBefore = (data: any) => {
  const [xn, xq] = xnxqValue.value.split(" ") as string[];
  data.schoolYear = xn;
  data.term = xq;
  data.teacherName = queryParams.teacherName || "";
  return data;
};

// 刷新表格数据
const handleQuery = () => {
  proTable.value.getTableList();
};

// 表格引用
const proTable = ref();

// 学年学期值
const xnxqValue = ref("");

// 打开调整时间对话框
const openAdjustDialog = (row: any) => {
  // 逻辑处理
};

// 更改值周老师
const changeWeekSetTeacher = (row: any) => {
  // 逻辑处理
};

// ... 其他代码 ...
</script>
```

## 说明

- **自定义插槽**：通过插槽可以灵活地自定义表格的显示内容。
- **数据请求**：通过 `requestApi` 和 `dataCallback` 实现数据的请求和处理。
- **刷新功能**：通过 `getTableList` 方法实现表格数据的刷新。
- **操作列**： 默认columns中最后一项为操作列，需要设置 `fixed: "right"`

希望这个文档能帮助你更好地理解和使用 `TyTable` 组件！如果有其他问题，请随时问我
