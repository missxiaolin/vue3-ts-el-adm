<template>
  <div :class="classes" class="main-content">
    <!-- 左侧模式 -->
    <leftMode
      v-if="layoutMode === 'left' || appStore.device === DeviceEnum.Mobile"
    />
    <!-- 顶部模式 -->
    <topMode v-else-if="layoutMode === 'top'" />
    <!-- 混合模式 -->
    <leftTopMode v-else-if="layoutMode === 'left-top'" />
  </div>
</template>

<script lang="ts">
import { computed } from "vue";
import leftTopMode from "./leftTopMode.vue";
import leftMode from "./leftMode.vue";
import topMode from "./topMode.vue";
import { storeToRefs } from "pinia";
import { useSettingsStore } from "@/store/modules/settings";
import { useAppStore } from "@/store/modules/app";
import { DeviceEnum } from "@/constants/app-key";

export default {
  components: {
    leftMode,
    topMode,
    leftTopMode,
  },
  setup() {
    const appStore = useAppStore();
    const settingsStore = useSettingsStore();
    const {
      layoutMode,
      showGreyMode,
      showColorWeakness,
    } = storeToRefs(settingsStore);

    const classes = computed(() => {
      return {
        showGreyMode: showGreyMode.value,
        showColorWeakness: showColorWeakness.value,
      };
    });

    return {
      layoutMode,
      appStore,
      DeviceEnum,
      classes,
    };
  },
};
</script>

<style lang="scss" scoped>
.showGreyMode {
  filter: grayscale(1);
}

.showColorWeakness {
  filter: invert(0.8);
}
.main-content {
  width: 100%;
  height: auto;
  overflow: hidden;
}
</style>
