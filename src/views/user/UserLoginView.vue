<template>
  <div class="login-container">
    <div class="login-card">
      <h2 class="login-title">Login</h2>
      <a-form :model="form" @submit="handleSubmit">
        <a-form-item field="userAccount">
          <a-input
            v-model="form.userAccount"
            placeholder="用户名"
            class="login-input"
          >
            <template #prefix>
              <icon-user />
            </template>
          </a-input>
        </a-form-item>
        <a-form-item field="userPassword">
          <a-input-password
            v-model="form.userPassword"
            placeholder="密码"
            class="login-input"
          >
            <template #prefix>
              <icon-lock />
            </template>
          </a-input-password>
        </a-form-item>
        <div class="login-options">
          <a-checkbox>记住我</a-checkbox>
          <a href="#">忘记密码？</a>
        </div>
        <a-button type="primary" html-type="submit" class="login-button">
          登录
        </a-button>
      </a-form>
      <div class="register-link">还没有账号？ <a href="#">注册</a></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import { UserControllerService, UserLoginRequest } from "../../../generated";
import { Message } from "@arco-design/web-vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import { IconUser, IconLock } from "@arco-design/web-vue/es/icon";

const form = reactive({
  userAccount: "",
  userPassword: "",
} as UserLoginRequest);

const router = useRouter();
const store = useStore();

const handleSubmit = async () => {
  try {
    const res = await UserControllerService.userLoginUsingPost(form);
    if (res.code === 0) {
      await store.dispatch("user/getLoginUser");
      Message.success("登录成功");
      router.push({
        path: "/",
        replace: true,
      });
    } else {
      Message.error("登录失败：" + res.message);
    }
  } catch (error) {
    Message.error("登录过程中发生错误，请稍后重试");
    console.error(error);
  }
};
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: url("/src/assets/wallhaven-o5jxlm.png") no-repeat bottom center;
  position: relative;
  overflow: hidden;
}

.login-container::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100%;
  background: url("/src/assets/wallhaven-o5jxlm.png") no-repeat bottom center;
  background-size: cover;
}

.login-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 10px;
  padding: 40px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
}

.login-title {
  color: #ababab;
  text-align: center;
  margin-bottom: 30px;
  font-size: 24px;
}

.login-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 15px;
  color: #fff;
}

.register-link {
  text-align: center;
  margin-top: 20px;
  color: #fff;
}

:deep(.arco-form-item) {
  width: 100%;
}

:deep(.login-input) {
  width: 100%;
  height: 40px;
  background-color: rgba(255, 255, 255, 0.1);
  border: none;
  color: #151515;
}

:deep(.login-input .arco-input-wrapper) {
  background-color: transparent;
}

.login-button {
  width: 100%;
  height: 40px;
  font-size: 16px;
  font-weight: bold;
  margin-top: 20px;
}

:deep(.arco-input),
:deep(.arco-input-password) {
  background-color: transparent;
}

:deep(.arco-input-prefix) {
  margin-right: 10px;
}

:deep(.arco-checkbox-label) {
  color: #fff;
}

:deep(.arco-btn-primary) {
  background: linear-gradient(45deg, #2196f3, #21cbf3);
  border: none;
}

:deep(.arco-btn-primary:hover) {
  background: linear-gradient(45deg, #21cbf3, #2196f3);
}

a {
  color: #21cbf3;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
