<script setup>
import { computed, watch, inject } from 'vue';
import CitySelect from './CitySelect.vue';
import Error from './Error.vue';
import DayCard from './DayCard.vue';
import Stat from './Stat.vue';

const city = inject('city');
const forecast = inject('forecast');
const error = inject('error');
const selectedDay = inject('selectedDay');

const errorMap = new Map([
  [400, "Bad Request: Please check your input."],
  [404, "City not found. Please try another one."],
  [500, "Internal Server Error: Please try again later."],
]);

const errorDisplay = computed(() => {
  if (!error.value) return null;
  const status = error.value?.error?.code;
  return errorMap.get(status) || "An unexpected error occurred.";
});

const dailyForecast = computed(() => {
  if (!forecast.value) return [];

  return forecast.value.time.map((time, index) => ({
    date: new Date(time),
    displayDate: new Date(time).toLocaleDateString('en-US', { weekday: 'short', month: 'short', day: 'numeric' }),
    temperature: Math.round(forecast.value.temperature_2m_max[index]),
    weatherIcon: getIconForCode(forecast.value.weather_code[index]),
    humidity: Math.round(forecast.value.relative_humidity_2m_mean[index]),
    precipitation: forecast.value.precipitation_sum[index].toFixed(2),
    wind: forecast.value.wind_speed_10m_max[index].toFixed(2),
    isToday: new Date().toDateString() === new Date(time).toDateString(),
  }));
});

const selectedWeather = computed(() => {
    if (!selectedDay.value) return [];
    return [
        {
            label: "Temperature",
            stat: selectedDay.value.temperature + "°C",
        },
        {
            label: "Humidity",
            stat: selectedDay.value.humidity + "%",
        },
        {
            label: "Precipitation",
            stat: selectedDay.value.precipitation + "mm",
        },
        {
            label: "Wind",
            stat: selectedDay.value.wind + "M/s",
        },
    ];
});

function selectDay(day) {
    selectedDay.value = day;
}

watch(dailyForecast, (newForecast) => {
  if (newForecast && newForecast.length > 0) {
    selectDay(newForecast.find(day => day.isToday) || newForecast[0]);
  }
});

function getIconForCode(code) {
  const iconMap = {
    sun: [0],
    cloud: [1, 2, 3, 45, 48],
    rain: [51, 53, 55, 56, 57, 61, 63, 65, 66, 67, 80, 81, 82, 95, 96, 99],
    snow: [71, 73, 75, 77, 85, 86],
  };

  for (const icon in iconMap) {
    if (iconMap[icon].includes(code)) {
      return icon;
    }
  }

  return 'sun';
}
</script>

<template>
  <div class="right">
    <Error :error="errorDisplay" />
    <div class="stats-container" v-if="selectedWeather.length > 0">
        <Stat v-for="item in selectedWeather" v-bind="item" :key="item.label" />
    </div>
    <div v-if="dailyForecast.length > 0" class="day-card-list">
      <DayCard
        v-for="day in dailyForecast"
        :key="day.date"
        :date="day.displayDate"
        :temperature="day.temperature"
        :weatherIcon="day.weatherIcon"
        :isActive="selectedDay && selectedDay.date.getTime() === day.date.getTime()"
        :isToday="day.isToday"
        @click="selectDay(day)"
      />
    </div>
    <CitySelect />
  </div>
</template>

<style scoped>
.right {
  background: var(--color-bg-main);
  padding: 40px;
  border-radius: 0 25px 25px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 30px;
}

.stats-container {
    display: flex;
    flex-direction: column;
    gap: 15px;
    width: 100%;
    border-bottom: 1px solid #4a5568;
    padding-bottom: 20px;
}

.day-card-list {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 15px;
  width: 100%;
}
</style>