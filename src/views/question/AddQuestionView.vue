<template>
  <div
    id="addQuestionView"
    class="max-w-4xl mx-auto p-6 bg-white rounded-lg shadow-md"
  >
    <h1 class="text-2xl font-bold mb-6 text-center">
      {{ updatePage ? "更新题目" : "添加新题目" }}
    </h1>
    <a-form :model="form" layout="vertical">
      <a-row :gutter="16">
        <a-col :span="16">
          <a-form-item field="title" label="标题">
            <a-input v-model="form.title" placeholder="请输入标题" />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item field="tags" label="标签">
            <a-input-tag
              v-model="form.tags"
              placeholder="请选择标签"
              allow-clear
            />
          </a-form-item>
        </a-col>
      </a-row>

      <a-form-item field="content" label="题目内容">
        <MdEditor
          :value="form.content"
          :handle-change="updateContent"
          class="w-full"
        />
      </a-form-item>

      <a-form-item field="answer" label="答案">
        <MdEditor
          :value="form.answer"
          :handle-change="updateAnswer"
          class="w-full"
        />
      </a-form-item>

      <a-divider>判题配置</a-divider>

      <a-row :gutter="16">
        <a-col :span="8">
          <a-form-item field="judgeConfig.timeLimit" label="时间限制 (ms)">
            <a-input-number
              v-model="form.judgeConfig.timeLimit"
              placeholder="请输入时间限制"
              mode="button"
              :min="0"
              :step="100"
              size="large"
              style="width: 100%"
            />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item field="judgeConfig.memoryLimit" label="内存限制 (MB)">
            <a-input-number
              v-model="form.judgeConfig.memoryLimit"
              placeholder="请输入内存限制"
              mode="button"
              :min="0"
              :step="64"
              size="large"
              style="width: 100%"
            />
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item field="judgeConfig.stackLimit" label="堆栈限制 (MB)">
            <a-input-number
              v-model="form.judgeConfig.stackLimit"
              placeholder="请输入堆栈限制"
              mode="button"
              :min="0"
              :step="64"
              size="large"
              style="width: 100%"
            />
          </a-form-item>
        </a-col>
      </a-row>

      <a-divider>测试用例配置</a-divider>

      <div
        v-for="(judgeCaseItem, index) of form.judgeCase"
        :key="index"
        class="mb-4 p-4 border rounded-md"
      >
        <a-row :gutter="16">
          <a-col :span="11">
            <a-form-item
              :field="`form.judgeCase[${index}].input`"
              label="输入用例"
            >
              <a-input
                v-model="judgeCaseItem.input"
                placeholder="请输入测试输入用例"
                type="textarea"
                :auto-size="{ minRows: 2, maxRows: 5 }"
              />
            </a-form-item>
          </a-col>
          <a-col :span="11">
            <a-form-item
              :field="`form.judgeCase[${index}].output`"
              label="输出用例"
            >
              <a-input
                v-model="judgeCaseItem.output"
                placeholder="请输入测试输出用例"
                type="textarea"
                :auto-size="{ minRows: 2, maxRows: 5 }"
              />
            </a-form-item>
          </a-col>
          <a-col :span="2" class="flex items-center justify-center">
            <a-button
              status="danger"
              shape="circle"
              @click="handleDelete(index)"
            >
              <icon-delete />
            </a-button>
          </a-col>
        </a-row>
      </div>

      <div class="text-center mt-4">
        <a-button @click="handleAdd" type="outline" status="success">
          <template #icon>
            <icon-plus />
          </template>
          新增测试用例
        </a-button>
      </div>

      <div class="mt-8 text-center">
        <a-button type="primary" size="large" @click="doSubmit">
          {{ updatePage ? "更新题目" : "提交新题目" }}
        </a-button>
      </div>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { Message } from "@arco-design/web-vue";
import { useRoute, useRouter } from "vue-router";
import { IconDelete, IconPlus } from "@arco-design/web-vue/es/icon";
import MdEditor from "@/components/MdEditor.vue";
import { QuestionControllerService } from "../../../generated";

const route = useRoute();
const router = useRouter();
const updatePage = route.path.includes("update");

const form = ref({
  title: "",
  tags: [],
  content: "",
  answer: "",
  judgeConfig: {
    memoryLimit: 1000,
    stackLimit: 1000,
    timeLimit: 1000,
  },
  judgeCase: [
    {
      input: "",
      output: "",
    },
  ],
});

const loadData = async () => {
  const id = route.query.id;
  if (!id) return;

  try {
    const res = await QuestionControllerService.getQuestionByIdUsingGet(
      id as string
    );
    if (res.code === 0 && res.data) {
      form.value = {
        ...res.data,
        judgeCase: Array.isArray(res.data.judgeCase)
          ? res.data.judgeCase
          : JSON.parse(res.data.judgeCase || "[]"),
        judgeConfig:
          typeof res.data.judgeConfig === "object"
            ? res.data.judgeConfig
            : JSON.parse(res.data.judgeConfig || "{}"),
        tags: Array.isArray(res.data.tags) ? res.data.tags : [],
      };
    } else {
      throw new Error(res.message || "加载失败");
    }
  } catch (error) {
    Message.error(`加载失败: ${error.message}`);
  }
};

onMounted(loadData);

const updateContent = (value: string) => {
  form.value.content = value;
};

const updateAnswer = (value: string) => {
  form.value.answer = value;
};

const doSubmit = async () => {
  try {
    const submitData = {
      ...form.value,
      tags: Array.isArray(form.value.tags) ? form.value.tags : [],
    };

    const service = updatePage
      ? QuestionControllerService.updateQuestionUsingPost
      : QuestionControllerService.addQuestionUsingPost;

    const res = await service(submitData);

    if (res.code === 0) {
      Message.success(updatePage ? "更新成功" : "创建成功");
      router.push("/question/manage");
    } else {
      throw new Error(res.message || (updatePage ? "更新失败" : "创建失败"));
    }
  } catch (error) {
    Message.error(`操作失败: ${error.message}`);
  }
};

const handleAdd = () => {
  form.value.judgeCase.push({ input: "", output: "" });
};

const handleDelete = (index: number) => {
  form.value.judgeCase.splice(index, 1);
};
</script>

<style scoped>
#addQuestionView {
  @apply bg-gray-50 min-h-screen py-8;
}
</style>
