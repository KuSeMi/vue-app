<script setup>
import { computed, nextTick, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';

const city = ref('');

const weather = reactive({
  temperature: 0,
  humidity: 0,
  precipitation: 0,
  wind: 0,
});

const weatherModified = computed(() => {
  return [{
    label: "Temperature",
    stat: weather.temperature + "°C",
  },
  {
    label: "Humidity",
    stat: weather.humidity + "%",
  },
  {
    label: "Precipitation",
    stat: weather.precipitation + "%",
  },
  {
    label: "Wind",
    stat: weather.wind + "M/s",
  },
];
});

async function getCity(selectedCity) {
  city.value = selectedCity;
  const response = await fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${selectedCity}&count=1`);
  const data = await response.json();
  const cityData = data.results[0];
  await getWeather(cityData.latitude, cityData.longitude);
}

async function getWeather(latitude, longitude) {
  const params = new URLSearchParams({
    latitude,
    longitude,
    current_weather: true,
    hourly: "temperature_2m,relative_humidity_2m,apparent_temperature,precipitation_probability,wind_speed_10m",
  });
  const response = await fetch(`https://api.open-meteo.com/v1/forecast?${params}`);
  const data = await response.json();
  weather.temperature = data.current_weather.temperature;
  weather.humidity = data.hourly.relative_humidity_2m[0];
  weather.precipitation = data.hourly.precipitation_probability[0];
  weather.wind = data.current_weather.windspeed;
}
</script>

<template>
  <main class="main">
    <div class="city-name">{{ city }}</div>
    <Stat  v-for="item in weatherModified" v-bind="item" :key="item.label" />
    <CitySelect @select-city="getCity"/>
  </main>
</template>

<style scoped>
.main {
  background: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.city-name {
  font-size: 36px;
  font-weight: 700;
  text-align: center;
}
</style>
