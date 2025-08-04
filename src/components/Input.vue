<script setup>
const data = defineModel({type: String, required: true});
const props = defineProps({
  error: String,
});
</script>

 <template>
    <input v-model="data" class="input" :class="{'error-input': error}"/>
 </template>

<style scoped>
.input {
    color: var(--color-primary);
    width: 260px;
    border-radius: 10px;
    border: 1px solid transparent;
    padding: 15px 20px;
    font-size: 18px;;
    background: var(--color-bg-card);
    box-shadow: 1px 2px 4px 0px var(--color-bg-main);
    transition: border-color 0.3s ease-in-out;
}
.input::placeholder {
    color: var(--color-text-placeholder);
}
.error-input {
  border-color: red;
}
</style>