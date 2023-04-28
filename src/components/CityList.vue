<template>
  <Suspense>
    <div v-for="city in savedCities" :key="city.id">
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


const savedCities = ref([]);

const getCities = async () => {

  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );

    const requests = [];
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
        )
      );
    });

    const weatherData = await Promise.all(requests);

    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
  if (savedCities.value.length === 0) {
    const successCallback = async (position) => {
      const requests = [];
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
        ))
      const weatherData = await Promise.all(requests);
      const locationObj = {
        id: uid(),
        state: weatherData[0].data.sys.country,
        city: weatherData[0].data.name,
        coords: {
          lat: weatherData[0].data.coord.lat,
          lng: weatherData[0].data.coord.lon,
        },
      };
      savedCities.value.push(locationObj);
      localStorage.setItem(
        "savedCities",
        JSON.stringify(savedCities.value)
      )
      weatherData.forEach((value, index) => {
        savedCities.value[index].weather = value.data;
      });
    }

    const errorCallback = (error) => {
      console.log(error);
    };

    navigator.geolocation.getCurrentPosition(successCallback, errorCallback);


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
