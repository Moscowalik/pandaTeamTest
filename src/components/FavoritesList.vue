<template>
    <Suspense>
        <div v-for="city in favoritesCities" :key="city.id">
            <CityCard :city="city" @click="goToCityView(city)" />
        </div>
        <template #fallback>
            <CityCardSkeleton />
        </template>
    </Suspense>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { uid } from "uid";
import { useRouter } from "vue-router";
import CityCard from "./CityCard.vue";
import CityCardSkeleton from "../components/CityCardSkeleton.vue";


const favoritesCities = ref([]);

const getCities = async () => {

    if (localStorage.getItem("favoritesCities")) {
        favoritesCities.value = JSON.parse(
            localStorage.getItem("favoritesCities")
        );

        const requests = [];
        favoritesCities.value.forEach((city) => {
            requests.push(
                axios.get(
                    `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
                )
            );
        });

        const weatherData = await Promise.all(requests);

        weatherData.forEach((value, index) => {
            favoritesCities.value[index].weather = value.data;
        });
    }
};



await getCities();




const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name: "cityView",
        params: { state: city.state, city: city.city },
        query: {
            id: city.id,
            lat: city.coords.lat,
            lng: city.coords.lng,
        },
    });
};



</script>

<style lang="scss" scoped></style>