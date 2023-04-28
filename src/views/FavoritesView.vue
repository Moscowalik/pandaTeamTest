<template>
    <div class="container">
        <div class="cities__list">
            <Suspense>
                <FavoritesList />
                <template #fallback>
                    <CityCardSkeleton />
                </template>
            </Suspense>
        </div>
    </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { uid } from "uid";
import { useRouter } from "vue-router";
import FavoritesList from "../components/FavoritesList.vue";


const router = useRouter();

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

.cities__list {
    display: flex;
    flex-direction: column;
    gap: 30px;
    margin-top: 30px;
    margin-bottom: 30px;
}
</style>