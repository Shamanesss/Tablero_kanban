<template>
  <form action="" @submit.prevent="handleSubmit">
    <input type="text" v-model="text" />
  </form>
</template>
<script setup>
import { ref, defineEmits } from "vue";
const text = ref("");
const emits = defineEmits(["NuevoItem"]);

function handleSubmit() {
  if (text.value !== "") {
    // entonces hacer un emit
    emits("NuevoItem", text);
    // limpiamos el valor
    text.value = "";
  }
}
</script>
<style scoped>
input {
  margin: 0.5rem;
}
</style>
