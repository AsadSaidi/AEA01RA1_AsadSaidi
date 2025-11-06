<template>
  <ul v-if="tasques.length > 0">
    <TaskItem
      v-for="(tasca, index) in tasques"
      :key="index"
      :tasca="tasca"
      :index="index"
      @eliminar-tasca="$emit('eliminar-tasca', $event)"
      @toggle-completada="$emit('toggle-completada', $event)"
    />
  </ul>
  <div v-else class="empty-state">
    <p>No hi ha tasques per mostrar</p>
    <small>Comença afegint la teva primera tasca</small>
  </div>
</template>

<script setup>
import TaskItem from './TaskItem.vue'
const props = defineProps({
  tasques: Array
})
</script>

<style scoped src="../styles.css"></style>

