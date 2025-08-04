<script setup>
import IconSun from '../icons/weather/IconSun.vue';
import IconRain from '../icons/weather/IconRain.vue';
import IconCloud from '../icons/weather/IconCloud.vue';
import IconSnow from '../icons/weather/IconSnow.vue';

const { date, temperature, weatherIcon, isActive } = defineProps({
    date: String,
    temperature: Number,
    weatherIcon: String,
    isActive: Boolean,
});
</script>

<template>
    <button class="day-card" :class="{ 'day-card--active': isActive }">
        <IconSun v-if="weatherIcon === 'sun'" :size="36" />
        <IconRain v-if="weatherIcon === 'rain'" :size="36" />
        <IconCloud v-if="weatherIcon === 'cloud'" :size="36" />
        <IconSnow v-if="weatherIcon === 'snow'" :size="36" />
        <div class="day-card__day">
            {{ date }}
        </div>
        <div class="day-card__temp">
            {{ temperature }}°C
        </div>
    </button>
</template>

<style scoped>
.day-card {
  border-radius: 10px;
  box-shadow: 1px 2px 4px 0px #222831;
  color: var(--color-primary);
  padding: 10px;
  background-color: var(--color-bg-card);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  border: 2px solid transparent;
  cursor: pointer;
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out, border-color 0.2s ease-in-out;
}

.day-card:hover {
  transform: scale(1.05);
}

.day-card--active {
  border-color: #8fb2f5;
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(143, 178, 245, 0.5);
}

.day-card__day {
  font-size: 12px;
}

.day-card__temp {
  font-size: 16px;
  font-weight: 700;
}
</style>
