<template>
  <router-view v-slot="{ Component, route }">
    <transition name="fade-slide" mode="out-in" appear>
      <keep-alive :include="keepAliveRouteNames">
        <component
          v-if="appStore.reloadFlag"
          :is="Component"
          :key="route.path"
        />
      </keep-alive>
    </transition>
  </router-view>
</template>

<script setup>
import { computed } from "vue";
import { useRouter } from "vue-router";
import { useAppStore } from "@/store/modules/app";

const appStore = useAppStore();
const router = useRouter();
const allRoutes = router.getRoutes();
const keepAliveRouteNames = computed(() => {
  return allRoutes
    .filter((route) => route.meta?.keepAlive)
    .map((route) => route.name);
});
</script>
