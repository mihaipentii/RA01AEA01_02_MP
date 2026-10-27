<script setup>
import { ref, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskList from './components/TaskList.vue'

const siguienteId = ref(4)
const soloPendientes = ref(false)
const mensaje = ref('')
let temporizador = null

const tareas = ref([
  { id: 1, titulo: 'Tasca 1', completada: true },
  { id: 2, titulo: 'Tasca 2', completada: false },
  { id: 3, titulo: 'Tasca 3', completada: false },
])

// Contadores a nivel de componente padre
const total = computed(() => tareas.value.length)
const pendientes = computed(() => tareas.value.filter((t) => !t.completada).length)
const completadas = computed(() => total.value - pendientes.value)

const tareasVisibles = computed(() =>
  soloPendientes.value ? tareas.value.filter((t) => !t.completada) : tareas.value,
)

// Notificación temporal
function notificar(texto) {
  mensaje.value = texto
  clearTimeout(temporizador)
  temporizador = setTimeout(() => (mensaje.value = ''), 2500)
}

// Funciones que se ejecutan cuando los hijos emiten eventos
function afegirTasca(titulo) {
  tareas.value.push({ id: siguienteId.value++, titulo, completada: false })
  notificar('Tasca afegida correctament')
}

function alternarTasca(id) {
  const tarea = tareas.value.find((t) => t.id === id)
  if (!tarea) return
  tarea.completada = !tarea.completada
  notificar(tarea.completada ? 'Tasca completada' : 'Tasca marcada com a pendent')
}

function eliminarTasca(id) {
  tareas.value = tareas.value.filter((t) => t.id !== id)
  notificar('Tasca eliminada')
}
</script>

<template>
  <main class="app">
    <div class="panel">
      <h1>Gestor de Tasques</h1>

      <p v-if="mensaje" class="notificacio">{{ mensaje }}</p>

      <TaskForm @afegir="afegirTasca" />

      <label class="filter">
        <input type="checkbox" v-model="soloPendientes" />
        <span>Mostra només pendents</span>
      </label>

      <TaskList :tareas="tareasVisibles" @alternar="alternarTasca" @eliminar="eliminarTasca" />

      <p class="resumen">Total: {{ total }} | Pendents: {{ pendientes }} | Completades: {{ completadas }}</p>
    </div>
  </main>
</template>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px 16px;
  background: #f3f4f6;
  font-family: Arial, sans-serif;
}

.panel {
  width: min(100%, 520px);
  background: white;
  border-radius: 18px;
  padding: 24px 20px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
}

h1 {
  margin: 0 0 16px;
  font-size: 2rem;
  color: #1f2937;
  text-align: center;
}

.notificacio {
  margin: 0 0 16px;
  padding: 8px 12px;
  border-radius: 999px;
  background: #e0f2fe;
  color: #075985;
  font-size: 0.9rem;
  text-align: center;
}

.filter {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 16px 0;
  color: #374151;
  font-size: 0.95rem;
}

.filter input {
  accent-color: #2563eb;
}

.resumen {
  margin: 18px 0 0;
  text-align: center;
  color: #6b7280;
  font-size: 0.9rem;
}
</style>