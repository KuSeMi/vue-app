<script setup>
import { ref, onMounted, provide, watch } from 'vue';
import PanelRight from './components/PanelRight.vue';

const city = ref('Kyiv');
const forecast = ref(null);
const error = ref();

async function getCity(selectedCity) {
  city.value = selectedCity;
  const response = await fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${selectedCity}&count=1`);
  if (!response.ok) {
    error.value = await response.json();
    resetWeather();
    return;
  }
  const data = await response.json();
  const cityData = data.results && data.results[0];
  if (cityData) {
    error.value = null;
    await getWeather(cityData.latitude, cityData.longitude);
  } else {
    error.value = { error: { code: 404 } };
    resetWeather();
  }
}

async function getWeather(latitude, longitude) {
  const params = new URLSearchParams({
    latitude,
    longitude,
    daily: "weather_code,temperature_2m_max,relative_humidity_2m_mean,precipitation_sum,wind_speed_10m_max",
    timezone: "auto",
  });
  const response = await fetch(`https://api.open-meteo.com/v1/forecast?${params}`);
  const data = await response.json();
  forecast.value = data.daily;
}

function resetWeather() {
    forecast.value = null;
}

watch(city, () => {
  getCity(city.value);
});

onMounted(() => {
  getCity(city.value);
});

provide('city', city);
provide('forecast', forecast);
provide('error', error);
provide('getCity', getCity);

</script>

<template>
  <main class="main">
    <div class="left">
    </div>
    <PanelRight />
  </main>
</template>

<style scoped>
.main {
  display: flex;
  align-items: center;
  justify-content: center;
}

.left {
  width: 500px;
  height: 680px;
  border-radius: 30px;
  background-image: url("/public/bg.png");
  background-repeat: no-repeat;
  background-size: cover;
}
</style>