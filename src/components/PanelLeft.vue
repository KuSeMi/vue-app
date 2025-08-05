<script setup>
import { inject, computed } from 'vue';
import IconSun from '../icons/weather/IconSun.vue';
import IconCloud from '../icons/weather/IconCloud.vue';
import IconRain from '../icons/weather/IconRain.vue';
import IconSnow from '../icons/weather/IconSnow.vue';
import IconLocation from '../icons/IconLocation.vue';

const city = inject('city');
const selectedDay = inject('selectedDay');

const weatherIconComponent = computed(() => {
  if (!selectedDay.value) return null;
  const icon = selectedDay.value.weatherIcon;
  if (icon === 'sun') return IconSun;
  if (icon === 'cloud') return IconCloud;
  if (icon === 'rain') return IconRain;
  if (icon === 'snow') return IconSnow;
  return IconSun; // Default
});

const weatherDescription = computed(() => {
    if (!selectedDay.value || !selectedDay.value.weather) return '';
    const weather = selectedDay.value.weather;
    return weather.charAt(0).toUpperCase() + weather.slice(1);
});

const currentDay = computed(() => {
    if(!selectedDay.value) return '';
    return new Date(selectedDay.value.date).toLocaleDateString('en-US', { weekday: 'long' });
});

const currentDate = computed(() => {
    if(!selectedDay.value) return '';
    return new Date(selectedDay.value.date).toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' });
});

</script>

<template>
  <div class="paneLeft" v-if="selectedDay">
    <div>
      <div class="day">
        {{ currentDay }}
      </div>
      <div class="date">
        {{ currentDate }}
      </div>
      <div class="city">
        <IconLocation />
        {{ city }}
      </div>
    </div>
    <div class="weather-container">
      <div class="wetherIcon">
        <component :is="weatherIconComponent" size="95" />
      </div>
      <div class="temp">
        {{ selectedDay.temperature }}°C
      </div>
      <div class="condition">
        {{ weatherDescription }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.paneLeft {
  width: 500px;
  height: 680px;
  border-radius: 30px;
  background-image: url("/public/bg.png");
  background-repeat: no-repeat;
  background-size: cover;
  color: white;
  display: flex;
  flex-direction: column;
  padding: 48px 32px;
  justify-content: space-between;
}
.day {
  font-size: 37px;
  font-weight: 700;
  text-transform: capitalize;
  margin-bottom: 16px;
}
.date {
  font-size: 22px;
  font-weight: 500;
  margin-bottom: 10px;
}
.weather-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.wetherIcon {
  margin-bottom: 25px;
}
.city {
  display: flex;
  gap: 8px;
  align-items: center;
}
.temp {
  font-size: 50px;
  font-weight: 700;
  margin-bottom: 9px;
}
.condition {
  font-size: 30px;
  font-weight: 700;
}
</style>