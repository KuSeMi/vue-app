<script setup>
import { nextTick, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';
let savedCity = ref("Kyiv");
let data = reactive({
  label: "Humidity",
  stat: "90%"
})


async function getCity(city) {
  savedCity.value = city;
  await nextTick();
  data.stat = "80%";
}
</script>

<template>
  <main class="main">
    {{ savedCity }}
    <Stat v-bind="data" />
    <Stat label="Precipitation" stat="0%" /> 
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
