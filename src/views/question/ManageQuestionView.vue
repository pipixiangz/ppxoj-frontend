<template>
  <div id="manageQuestionView">
    <a-table
      :ref="tableRef"
      :columns="columns"
      :data="dataList"
      :pagination="{
        showTotal: true,
        pageSize: searchParams.pageSize,
        current: searchParams.current,
        total,
      }"
      @page-change="onPageChange"
    >
      <template #optional="{ record }">
        <a-space>
          <a-button type="primary" @click="doUpdate(record)"> 修改</a-button>
          <a-button status="danger" @click="doDelete(record)">删除</a-button>
        </a-space>
      </template>
      <template #judgeConfig="{ record }">
        <div v-if="record.judgeConfig">
          <div>
            时间限制：{{ parseJudgeConfig(record.judgeConfig).timeLimit }} ms
          </div>
          <div>
            内存限制：{{ parseJudgeConfig(record.judgeConfig).memoryLimit }} MB
          </div>
          <div>
            堆栈限制：{{ parseJudgeConfig(record.judgeConfig).stackLimit }} MB
          </div>
        </div>
      </template>
      <template #judgeCase="{ record }">
        <a-collapse v-if="record.judgeCase">
          <a-collapse-item
            v-for="(item, index) in parseJudgeCase(record.judgeCase)"
            :key="index"
            :header="`用例 ${index + 1}`"
          >
            <div>输入：{{ item.input }}</div>
            <div>输出：{{ item.output }}</div>
          </a-collapse-item>
        </a-collapse>
      </template>
      <template #createTime="{ record }">
        {{ formatDate(record.createTime) }}
      </template>
      <template #tags="{ record }">
        <a-space wrap>
          <a-tag
            v-for="(tag, index) in parseTags(record.tags)"
            :key="index"
            :color="getTagColor(tag)"
          >
            {{ tag }}
          </a-tag>
        </a-space>
      </template>
    </a-table>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, watchEffect } from "vue";
import {
  Page_Question_,
  Question,
  QuestionControllerService,
} from "../../../generated";
import message from "@arco-design/web-vue/es/message";
import * as querystring from "querystring";
import { useRouter } from "vue-router";

const tableRef = ref();

const dataList = ref([]);
const total = ref(0);
const searchParams = ref({
  pageSize: 10,
  current: 1,
});

const loadData = async () => {
  const res = await QuestionControllerService.listQuestionByPageUsingPost(
    searchParams.value
  );
  if (res.code === 0) {
    dataList.value = res.data.records;
    total.value = res.data.total;
  } else {
    message.error("加载失败，" + res.message);
  }
};

watchEffect(() => {
  loadData();
});

onMounted(() => {
  loadData();
});

const parseJudgeConfig = (judgeConfig: string) => {
  try {
    return JSON.parse(judgeConfig);
  } catch (error) {
    console.error("解析 judgeConfig 失败:", error);
    return { timeLimit: "N/A", memoryLimit: "N/A", stackLimit: "N/A" };
  }
};

const parseJudgeCase = (judgeCase: string) => {
  try {
    return JSON.parse(judgeCase);
  } catch (error) {
    console.error("解析 judgeCase 失败:", error);
    return [];
  }
};

const parseTags = (tags: string) => {
  try {
    return JSON.parse(tags);
  } catch (error) {
    console.error("解析 tags 失败:", error);
    return [];
  }
};

const getTagColor = (tag: string) => {
  switch (tag.toLowerCase()) {
    case "easy":
      return "green";
    case "medium":
      return "orange";
    case "hard":
      return "red";
    default:
      return "";
  }
};

const formatDate = (dateString: string) => {
  if (!dateString) return "N/A";
  const date = new Date(dateString);
  return date.toISOString().split("T")[0];
};

const columns = [
  {
    title: "id",
    dataIndex: "id",
  },
  {
    title: "标题",
    dataIndex: "title",
  },
  {
    title: "内容",
    dataIndex: "content",
  },
  {
    title: "标签",
    dataIndex: "tags",
    slotName: "tags",
  },
  {
    title: "答案",
    dataIndex: "answer",
  },
  {
    title: "判题配置",
    dataIndex: "judgeConfig",
    slotName: "judgeConfig",
  },
  {
    title: "判题用例",
    dataIndex: "judgeCase",
    slotName: "judgeCase",
  },
  {
    title: "用户id",
    dataIndex: "userId",
  },
  {
    title: "创建时间",
    dataIndex: "createTime",
    slotName: "createTime",
  },
  {
    title: "操作",
    slotName: "optional",
  },
];

const onPageChange = (page: number) => {
  searchParams.value = {
    ...searchParams.value,
    current: page,
  };
};

const doDelete = async (question: Question) => {
  const res = await QuestionControllerService.deleteQuestionUsingPost({
    id: question.id,
  });
  if (res.code === 0) {
    message.success("删除成功");
    loadData();
  } else {
    message.error("删除失败");
  }
};

const router = useRouter();

const doUpdate = (question: Question) => {
  router.push({
    path: "/update/question",
    query: {
      id: question.id,
    },
  });
};
</script>

<style scoped>
#manageQuestionView {
}
</style>
