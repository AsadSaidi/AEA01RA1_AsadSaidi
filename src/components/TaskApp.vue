<template>
  <div class="app">
    <div class="header">
      <h1>Gestor de Tasques</h1>
      <p class="subtitle">Organitza el teu dia a dia</p>
    </div>

    <TaskForm @afegir-tasca="afegirTasca" />

    <div class="filtres">
      <button 
        @click="filtre = 'totes'" 
        :class="{ active: filtre === 'totes' }"
      >Totes</button>
      <button 
        @click="filtre = 'pendents'"
        :class="{ active: filtre === 'pendents' }"
      >Pendents</button>
      <button 
        @click="filtre = 'completades'"
        :class="{ active: filtre === 'completades' }"
      >Completades</button>
    </div>

    <TaskList
      :tasques="tasquesFiltrades"
      @eliminar-tasca="eliminarTasca"
      @toggle-completada="toggleCompletada"
    />

    <div class="resum">
      <div class="resum-item">
        <span class="resum-label">Total</span>
        <span class="resum-value">{{ totalTasques }}</span>
      </div>
      <div class="resum-item">
        <span class="resum-label">Pendents</span>
        <span class="resum-value pendents">{{ tasquesPendents }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TaskForm from './TaskForm.vue'
import TaskList from './TaskList.vue'

const tasques = ref([])
const filtre = ref('totes')

function afegirTasca(titol) {
  tasques.value.push({
    titol,
    completada: false,
    data: new Date().toLocaleDateString()
  })
}

function eliminarTasca(index) {
  const tascaFiltrada = tasquesFiltrades.value[index]
  const idx = tasques.value.indexOf(tascaFiltrada)
  if (idx !== -1) tasques.value.splice(idx, 1)
}

function toggleCompletada(index) {
  const tascaFiltrada = tasquesFiltrades.value[index]
  tascaFiltrada.completada = !tascaFiltrada.completada
}

const tasquesFiltrades = computed(() => {
  if (filtre.value === 'pendents')
    return tasques.value.filter(t => !t.completada)
  if (filtre.value === 'completades')
    return tasques.value.filter(t => t.completada)
  return tasques.value
})

const totalTasques = computed(() => tasques.value.length)
const tasquesPendents = computed(() => tasques.value.filter(t => !t.completada).length)
</script>

<style scoped src="../styles.css"></style>