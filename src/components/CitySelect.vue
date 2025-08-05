<script setup>
import { ref, inject } from "vue";
import IconLocation from "../icons/IconLocation.vue";
import Button from "./Button.vue";
import Input from "./Input.vue";

const city = inject('city');
const getCity = inject('getCity');
const inputValue = ref(city.value);
const props = defineProps({
  error: String,
});


let isEdited = ref(false);

function select() {
  isEdited.value = false;
  getCity(inputValue.value);
}

function edit() {
  isEdited.value = true;
}
</script>

<template>
  <div class="city-select">
    <div v-if="isEdited" class="city-input">
      <Input placeholder="Input city" v-focus v-model="inputValue" @keyup.enter="select()" :error="error"/>
      <Button @click="select()">Save </Button>
    </div>
    <Button v-else @click="edit()">
      <IconLocation />
      Change city
    </Button>
  </div>
</template>

<style scoped>
.city-input {
  display: flex;
  gap: 12px;
}
.city-select {
  width: 420px;
}
</style>
