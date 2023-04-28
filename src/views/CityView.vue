<template>
  <div class="container">
    <Suspense>
      <template #default>
        <AsyncCityView />
      </template>
      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityView from "../components/AsyncCityView.vue";
import CityViewSkeleton from "../components/CityViewSkeleton.vue";
</script>

<style lang="scss" scoped>
@import '../assets/variables';

.container {
  margin: 0 auto;
  width: 100%;
  padding-left: 30px;
  padding-right: 30px;

  @media (min-width: 500px) {
    max-width: $tablet-width;
  }

  @media (min-width: 1200px) {
    max-width: $desctop-width;
  }
}
</style>
