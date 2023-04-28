<template>
  <div class="city__container" :class="{ 'day': timesofDay === 'is-day', 'night': timesofDay === 'is-night' }">
    <div class="city__info">
      <h2 class="city__info-name">{{ city.city }}</h2>
      <h3 class="city__info-state">{{ city.state }}</h3>
    </div>

    <div class="temp">
      <p class="main__temp">
        {{ Math.round(city.weather.main.temp) }}&deg;C
      </p>
      <div class="temp-block">
        <span class="secondary__temp">
          H:
          {{ Math.round(city.weather.main.temp_max) }}&deg;C
        </span>
        <span class="secondary__temp">
          L:
          {{ Math.round(city.weather.main.temp_min) }}&deg;C
        </span>
      </div>
    </div>

  </div>
</template>

<script setup>
import { onMounted } from 'vue'

const props = defineProps({
  city: {
    type: Object,
    default: () => { },
  },
});
let timesofDay = 'is-night';


const getCurrentTime = () => {

  const localOffset = new Date().getTimezoneOffset() * 60000;
  const utc = props.city.weather.dt * 1000 + localOffset;
  const currentTime = new Date(utc + 1000 * props.city.weather.timezone).getHours();

  if (currentTime > 5 && currentTime < 20) {

    timesofDay = 'is-day';
  }
}

if (props.city) {
  getCurrentTime()
}







</script>

<style lang="scss" scoped>
@import '../assets/variables';

.city__container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.10);
}

.city__info {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  gap: 20px;
}

.city__info-name {
  font-family: $main-font;
  font-weight: 600;
  font-size: 30px;
  color: rgb(0, 0, 0);

  @media (min-width: 600px) {
    font-size: 50px
  }
}

.city__info-state {
  font-family: $main-font;
  font-weight: 400;
  font-size: 15px;
  color: rgb(0, 0, 0);

  @media (min-width: 600px) {
    font-size: 25px
  }
}


.day {
  background-image: url('../assets/day.jpg');
  height: 100px;
  background-size: 100%;

  @media (min-width: 600px) {
    height: 200px;
  }
}

.night {
  background-image: url('../assets/night.jpg');
  height: 100px;
  background-size: 100%;

  @media (min-width: 600px) {
    height: 200px;
  }

  & .city__info-name {
    color: #fff
  }

  & .city__info-state {
    color: #fff
  }

  & .main__temp {
    color: #fff
  }

  & .secondary__temp {
    color: #fff
  }
}

.temp {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px;
}

.main__temp {
  font-family: $main-font;
  font-weight: 600;
  font-size: 30px;
  color: rgb(0, 0, 0);

  @media (min-width: 600px) {
    font-size: 50px
  }
}

.secondary__temp {
  font-family: $main-font;
  font-weight: 400;
  font-size: 15px;
  color: rgb(0, 0, 0);

  @media (min-width: 600px) {
    font-size: 25px
  }
}
</style>
