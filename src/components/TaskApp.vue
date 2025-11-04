<template>
  <div class="app">
    <div class="header">
      <h1>📝 Gestor de Tasques</h1>
      <p class="subtitle">Organitza el teu dia a dia</p>
    </div>

    <!-- Afegir nova tasca -->
    <div class="nova-tasca">
      <input
        v-model="novaTasca"
        placeholder="Escriu una nova tasca..."
        @keyup.enter="afegirTasca"
      />
      <button @click="afegirTasca" class="btn-primary">Afegir</button>
    </div>

    <!-- Filtres -->
    <div class="filtres">
      <button 
        @click="filtre = 'totes'" 
        :class="{ active: filtre === 'totes' }"
      >
        Totes
      </button>
      <button 
        @click="filtre = 'pendents'"
        :class="{ active: filtre === 'pendents' }"
      >
        Pendents
      </button>
      <button 
        @click="filtre = 'completades'"
        :class="{ active: filtre === 'completades' }"
      >
        Completades
      </button>
    </div>

    <!-- Llista de tasques -->
    <ul v-if="tasquesFiltrades.length > 0">
      <li
        v-for="(tasca, index) in tasquesFiltrades"
        :key="index"
        :class="{ completada: tasca.completada }"
      >
        <label class="tasca-label">
          <input
            type="checkbox"
            v-model="tasca.completada"
          />
          <span>{{ tasca.titol }}</span>
        </label>
        <div class="tasca-actions">
          <small class="data">{{ tasca.data }}</small>
          <button @click="eliminarTasca(index)" class="btn-delete">🗑️</button>
        </div>
      </li>
    </ul>

    <div v-else class="empty-state">
      <p>✨ No hi ha tasques per mostrar</p>
      <small>Comença afegint la teva primera tasca</small>
    </div>

    <!-- Resum -->
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

// Dades reactives
const tasques = ref([])
const novaTasca = ref('')
const filtre = ref('totes')

// Funcions
function afegirTasca() {
  if (novaTasca.value.trim() !== '') {
    tasques.value.push({
      titol: novaTasca.value,
      completada: false,
      data: new Date().toLocaleDateString()
    })
    novaTasca.value = ''
  }
}

function eliminarTasca(index) {
  tasques.value.splice(index, 1)
}

// Computed
const tasquesFiltrades = computed(() => {
  if (filtre.value === 'pendents')
    return tasques.value.filter(t => !t.completada)
  if (filtre.value === 'completades')
    return tasques.value.filter(t => t.completada)
  return tasques.value
})

const totalTasques = computed(() => tasques.value.length)
const tasquesPendents = computed(
  () => tasques.value.filter(t => !t.completada).length
)
</script>
<style scoped src="../styles.css"></style>