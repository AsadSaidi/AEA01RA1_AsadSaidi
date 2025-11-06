<template>
  <div class="nova-tasca">
    <input
      v-model="inputTasca"
      placeholder="Escriu una nova tasca..."
      @keyup.enter="onAfegir"
    />
    <button @click="onAfegir" class="btn-primary">Afegir</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['afegir-tasca'])
const inputTasca = ref('')

function onAfegir() {
  if (inputTasca.value.trim() !== '') {
    emit('afegir-tasca', inputTasca.value)
    inputTasca.value = ''
  }
}
</script>

<style scoped src="../styles.css"></style>
