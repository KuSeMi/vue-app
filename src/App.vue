<script setup>
import { computed, nextTick, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';
let savedCity = ref("Kyiv");
let data = reactive({
  stat: "90%",
  rain: 0,
  wind: 3
})

const dataModified = computed(() => {
  return [{
    label: "Humidity",
    stat: data.stat + "%",
  },
  {
    label: "Precipitation",
    stat: data.rain + "%",
  },
  {
    label: "Wind",
    stat: data.wind + "M/s",
  },
];
});

async function getCity(city) {
  savedCity.value = city;
  await nextTick();
  data.stat = "25";
}
</script>

<template>
  <main class="main">
    {{ savedCity }}
    <Stat  v-for="item in dataModified" v-bind="item" :key="item.label" />
    <CitySelect @select-city="getCity"/>
  </main>
</template>

<style scoped>
.main {
  background: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
}
</style>
