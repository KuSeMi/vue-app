<script setup>
import { ref } from "vue";
import IconLocation from "../icons/IconLocation.vue";
import Button from "./Button.vue";
import Input from "./input.vue";
import { onMounted } from "vue";

const props = defineProps({
  error: String,
});
const emit = defineEmits(["selectCity"]);

const city = ref("Kyiv");
let isEdited = ref(false);

onMounted(() => {
  emit("selectCity", city.value);
})

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
    <div v-if="isEdited" class="city-input">
      <Input placeholder="Input city" v-focus v-model="city" @keyup.enter="select()" :error="error"/>
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