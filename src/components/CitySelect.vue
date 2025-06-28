<script setup>
import { ref } from 'vue';
import IconLocation from '../icons/iconLocation.vue';
import Button from './Button.vue';

const emit = defineEmits({
  selectCity(payload) {
    return payload;
  },
});

let isEdited = ref(false);

function select() {
  isEdited.value = false;
  emit("selectCity", "London")
}

function edit() {
  isEdited.value = true;
}
</script>

<template>
  {{ isEdited }}
   <Button @click="edit()">
      <IconLocation />
      Select city
    </Button>
    <input />
    <Button @click="select()">
      Save
    </Button>
</template>

<style></style>