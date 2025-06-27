<script setup>
import IconLocation from '../icons/iconLocation.vue';
import Button from './Button.vue';

const emit = defineEmits({
  selectCity(payload) {
    console.log(`validating payload: ${payload}`);
    return payload;
  },
});

function select() {
  emit("selectCity", "Kyiv")
}
</script>

<template>
   <Button @click="select()">
      <IconLocation />
      Select city
    </Button>
    <Button>
      <input />
      Save
    </Button>
</template>

<style></style>