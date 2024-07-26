<template>
  <header class="global-header">
    <a-row id="globalHeader" align="center" :wrap="false">
      <a-col flex="auto">
        <a-menu
          mode="horizontal"
          :selected-keys="selectedKeys"
          @menu-item-click="doMenuClick"
        >
          <a-menu-item
            key="0"
            :style="{ padding: 0, marginRight: '24px' }"
            disabled
          >
            <div
              class="title-bar"
              @click="goToMainPage"
              style="cursor: pointer"
            >
              <img class="logo" src="../assets/Robot-judge.svg" />
              <div class="title">皮皮码来判判</div>
            </div>
          </a-menu-item>
          <a-menu-item
            v-for="item in visibleRoutes"
            :key="item.path"
            class="menu-item"
          >
            {{ item.name }}
          </a-menu-item>
        </a-menu>
      </a-col>
      <a-col flex="120px">
        <!-- 减少宽度 -->
        <div class="user-info">
          <a-avatar
            :size="28"
            :style="{ backgroundColor: '#87d068', marginRight: '8px' }"
          >
            {{ avatarText }}
          </a-avatar>
          <span class="username">{{
            store.state.user?.loginUser?.userName ?? "未登录"
          }}</span>
        </div>
      </a-col>
    </a-row>
  </header>
</template>

<script setup lang="ts">
// 脚本部分保持不变
import { routes } from "../router/routes";
import { useRoute, useRouter } from "vue-router";
import { computed, ref } from "vue";
import { useStore } from "vuex";
import checkAccess from "@/access/checkAccess";
import ACCESS_ENUM from "@/access/accessEnum";

const router = useRouter();
const store = useStore();

const visibleRoutes = computed(() => {
  return routes.filter((item, index) => {
    if (item.meta?.hideInMenu) {
      return false;
    }
    if (
      !checkAccess(store.state.user.loginUser, item?.meta?.access as string)
    ) {
      return false;
    }
    return true;
  });
});

const selectedKeys = ref(["/"]);

router.afterEach((to, from, failure) => {
  selectedKeys.value = [to.path];
});

setTimeout(() => {
  store.dispatch("user/getLoginUser", {
    userName: "皮皮翔管理员",
    userRole: ACCESS_ENUM.ADMIN,
  });
}, 3000);

const doMenuClick = (key: string) => {
  router.push({
    path: key,
  });
};

const avatarText = computed(() => {
  const userName = store.state.user?.loginUser?.userName ?? "未登录";
  return userName.charAt(0).toUpperCase();
});

const goToMainPage = () => {
  router.push("/"); // 假设主页的路由是 '/'
};
</script>

<style scoped>
.global-header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  transition: box-shadow 0.3s ease;
  padding: 0 16px; /* 减少左右内边距 */
}

.global-header:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.title-bar {
  display: flex;
  align-items: center;
}

.title {
  color: #444;
  margin-left: 12px; /* 减少标题左边距 */
  font-weight: bold;
  transition: color 0.3s ease;
  font-size: 16px; /* 稍微减小字体大小 */
}

.logo {
  height: 40px; /* 减小 logo 大小 */
  transition: transform 0.3s ease;
}

.title-bar:hover .logo {
  transform: scale(1.05) rotate(5deg);
}

.title-bar:hover .title {
  color: #1890ff;
}

.menu-item {
  position: relative;
  overflow: hidden;
  padding: 0 12px; /* 减少菜单项的内边距 */
}

.menu-item::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background-color: #1890ff;
  transition: all 0.3s ease;
}

.menu-item:hover::after {
  width: 100%;
  left: 0;
}

:deep(.ant-menu-horizontal) {
  border-bottom: none;
  line-height: 40px; /* 减小菜单高度 */
}

:deep(.ant-menu-item-selected) {
  font-weight: bold;
}

.user-info {
  padding: 0 10px;
  height: 100%;
  display: flex;
  align-items: center;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.user-info:hover {
  background-color: rgba(0, 0, 0, 0.05);
}

.username {
  font-size: 14px;
  max-width: 80px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 添加模糊效果 */
@supports (backdrop-filter: blur(10px)) {
  .global-header {
    background-color: rgba(255, 255, 255, 0.28);
    backdrop-filter: blur(10px);
  }
}
</style>
