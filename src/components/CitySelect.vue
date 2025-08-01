<script setup>
import { ref } from "vue";
import IconLocation from "../icons/IconLocation.vue";
import Button from "./Button.vue";
import Input from "./input.vue";

const emit = defineEmits(["selectCity"]);

const city = ref("Kyiv");
let isEdited = ref(false);

function select() {
  isEdited.value = false;
  if (city.value.trim()) {
    emit("selectCity", city.value);
  }
}

function edit() {
  isEdited.value = true;
}

</script>

<template>
  <div class="city-select">
    <div v-show="isEdited" class="city-input">
      <Input placeholder="Input city"
        v-model="city"
      />
      <Button @click="select()">Save </Button>
    </div>
    <Button v-show="!isEdited" @click="edit()">
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