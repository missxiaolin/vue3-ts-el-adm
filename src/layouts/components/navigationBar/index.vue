<template>
  <div class="navigation-bar">
    <hamburger
      v-if="!isTop || isMobile"
      :is-active="sidebar.opened"
      class="hamburger"
      @toggle-click="toggleSidebar"
    />
    <breadcrumb v-if="!isTop || isMobile" class="breadcrumb" />
    <sidebar v-if="isTop && !isMobile" class="sidebar" />
    <div class="right-menu">
      <screenfull v-if="showScreenfull" class="right-menu-item" />
      <themeSwitch v-if="showThemeSwitch" class="right-menu-item" />
    </div>
  </div>
</template>

<script lang="ts">
import { computed } from "vue";
import { useRouter } from "vue-router";
import { storeToRefs } from "pinia";
import { useAppStore } from "@/store/modules/app";
import { useSettingsStore } from "@/store/modules/settings";
import hamburger from "../hamburger/index.vue";
import breadcrumb from "../breadcrumb/index.vue"
import screenfull from "@/components/screenfull/index.vue"
import sidebar from "../sidebar/index.vue"
import themeSwitch from "@/components/themeSwitch/index.vue"

import { DeviceEnum } from "@/constants/app-key";

export default {
  components: {
    hamburger,
    breadcrumb,
    sidebar,
    screenfull,
    themeSwitch
  },
  setup() {
    const appStore = useAppStore();
    const settingsStore = useSettingsStore();
    const { sidebar, device } = storeToRefs(appStore);
    const { layoutMode, showNotify, showThemeSwitch, showScreenfull } =
      storeToRefs(settingsStore);
    const isTop = computed(() => layoutMode.value === "top");
    const isMobile = computed(() => device.value === DeviceEnum.Mobile);

    /** 切换侧边栏 */
    const toggleSidebar = () => {
      appStore.toggleSidebar(false);
    };

    return {
      isMobile,
      isTop,
      sidebar,
      toggleSidebar,
      showScreenfull,
      showThemeSwitch
    };
  },
};
</script>

<style lang="scss" scoped>
.navigation-bar {
  height: var(--v3-navigationbar-height);
  overflow: hidden;
  background: var(--v3-header-bg-color);
  display: flex;
  justify-content: space-between;
  .hamburger {
    display: flex;
    align-items: center;
    height: 100%;
    padding: 0 15px;
    cursor: pointer;
  }
  .breadcrumb {
    flex: 1;
    // 参考 Bootstrap 的响应式设计将宽度设置为 576
    @media screen and (max-width: 576px) {
      display: none;
    }
  }
  .sidebar {
    flex: 1;
    // 设置 min-width 是为了让 Sidebar 里的 el-menu 宽度自适应
    min-width: 0px;
    :deep(.el-menu) {
      background-color: transparent;
    }
    :deep(.el-sub-menu) {
      &.is-active {
        .el-sub-menu__title {
          color: var(--el-menu-active-color) !important;
        }
      }
    }
  }
  .right-menu {
    margin-right: 10px;
    height: 100%;
    display: flex;
    align-items: center;
    color: #606266;
    .right-menu-item {
      padding: 0 10px;
      cursor: pointer;
      .right-menu-avatar {
        display: flex;
        align-items: center;
        .el-avatar {
          margin-right: 10px;
        }
        span {
          font-size: 16px;
        }
      }
    }
  }
}
</style>