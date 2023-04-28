<template>
  <div class="">
    <SiteNavigation />
    <RouterView class="" v-slot="{ Component }">
      <Transition name="">
        <component :is="Component" />
      </Transition>
    </RouterView>
    <SiteFooter />
  </div>
</template>

<script setup>
import { RouterView } from "vue-router";
import SiteNavigation from "./components/SiteNavigation.vue";
import SiteFooter from "./components/SiteFooter.vue";
</script>

<style>
.page-enter-active {
  transition: 600ms ease all;
}

.page-enter-from {
  opacity: 0;
}
</style>
