<script setup>
const { color = "white", size = 54 } = defineProps({
  color: String,
  size: Number,
});
</script>

<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    width="54"
    height="54"
    viewBox="0 0 24 24"
    fill="none"
    stroke="currentColor"
    stroke-width="2"
    stroke-linecap="round"
    stroke-linejoin="round"
    :style="{ scale: size / 54 }"
  >
    <path d="M20 17.58A5 5 0 0 0 15.07 13H9.93A5 5 0 0 0 5 17.58" />
    <path d="M8 14v.5" />
    <path d="M12 14v.5" />
    <path d="M16 14v.5" />
    <path d="M8 19h.01" />
    <path d="M12 19h.01" />
    <path d="M16 19h.01" />
  </svg>
</template>