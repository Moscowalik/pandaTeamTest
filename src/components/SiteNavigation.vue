<template>
  <header class="header">
    <nav class="navigation">
      <RouterLink :to="{ name: 'home' }">
        <div class="logo">
          <p class="logo__text">The GisWeather</p>
        </div>
      </RouterLink>

      <div class="icons__thumb">
        <RouterLink :to="{ name: 'favorites' }">
          <div class="navigation__icon" @click="toggleModal">
            <svg class="icon-heart" height="19" width="19">
              <use href="../assets/symbol-defs.svg#icon-heart"></use>
            </svg>
          </div>
        </RouterLink>
        <Suspense>
          <div @click="addCity" class="navigation__icon" v-if="route.path !== '/' && route.path !== '/favorites'">
            <svg class="icon-plus" height="19" width="19">
              <use href="../assets/symbol-defs.svg#icon-plus"></use>
            </svg>
          </div>
        </Suspense>
      </div>
    </nav>
  </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import { onMounted, ref } from "vue";
import BaseModal from "./BaseModal.vue";
import FavoritesView from "../views/FavoritesView.vue";

const isCity = ref(true)
const favoritesCities = ref([])
const route = useRoute();
const router = useRouter();


const addCity = () => {
  if (localStorage.getItem("favoritesCities")) {
    favoritesCities.value = JSON.parse(
      localStorage.getItem("favoritesCities")
    );
  }

  if (favoritesCities.value.length >= 5) {
    alert('you cannot save more than five objects. Delete some of them')
    return
  }

  if (favoritesCities.value !== 0) {
    const city = favoritesCities.value.some((city) => {
      console.log(city.city)
      console.log(route.params.city)
      return city.city == route.params.city
    })
    if (city) {
      alert('This city is already in the favorites')
      return
    }
  }




  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  favoritesCities.value.push(locationObj);
  localStorage.setItem(
    "favoritesCities",
    JSON.stringify(favoritesCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
  isCity.value = false
};
</script>

<style lang="scss" scoped>
@import '../assets/variables';

.header {
  position: sticky;
  top: 0;
  width: 100%;
  border-bottom: 1px solid #dadada;
  background-color: #F8F8FF;
}

.navigation {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 auto;
  width: 100%;
  padding: 30px;

  @media (min-width: 500px) {
    max-width: $tablet-width;
  }

  @media (min-width: 1200px) {
    max-width: $desctop-width;
  }
}

.logo__text {
  font-family: $main-font;
  font-weight: 600;
  font-size: 18px;
  color: #7284FF;

  @media (min-width: 600px) {
    font-size: 28px;
  }
}

.navigation__icon {
  border-radius: 50%;
  padding: 10px;
  width: 40px;
  height: 40px;
  background: #D7D8F0;
  cursor: pointer;

  &:hover {
    background-color: #7284FF;
  }
}

.icon-heart {
  fill: transparent;
}

.icons__thumb {
  display: flex;
  gap: 15px;
}
</style>