<template>
  <li :class="{ completada: tasca.completada }">
    <label class="tasca-label">
      <input type="checkbox" :checked="tasca.completada" @change="onToggleCompletada" />
      <span>{{ tasca.titol }}</span>
    </label>
    <div class="tasca-actions">
      <small class="data">{{ tasca.data }}</small>
      <button @click="onEliminar" class="btn-delete">
        <img src="../../public/trash.png" alt="Eliminar" />
        </button>
    </div>
  </li>
</template>

<script setup>
const props = defineProps({
  tasca: Object,
  index: Number
})
const emit = defineEmits(['eliminar-tasca', 'toggle-completada'])

function onEliminar() {
  emit('eliminar-tasca', props.index)
}
function onToggleCompletada() {
  emit('toggle-completada', props.index)
}
</script>

<style scoped src="../styles.css"></style>

