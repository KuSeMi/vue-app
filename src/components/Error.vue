<script setup>
const props = defineProps({
  error: String,
});
</script>

<template>
  <div v-if="props.error" class="error">{{ props.error }}</div>
</template>

<style scoped>
.error {
  font-size: 16px;
  padding: 12px;
  text-align: center;
  background: #ff4d4d;
  color: white;
  border-radius: 10px;
  width: 100%;
  box-sizing: border-box;
}
</style>