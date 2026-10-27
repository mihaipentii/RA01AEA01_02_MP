<script setup>
import { ref } from 'vue'

const emit = defineEmits(['afegir'])
const nuevaTarea = ref('')

function enviar() {
  const titulo = nuevaTarea.value.trim()
  if (!titulo) return
  emit('afegir', titulo)
  nuevaTarea.value = ''
}
</script>

<template>
  <div class="task-form">
    <input v-model="nuevaTarea" type="text" placeholder="Escriu una nova tasca" @keyup.enter="enviar" />
    <button @click="enviar">Afegir</button>
  </div>
</template>

<style scoped>
.task-form {
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  border: 1px solid #d1d5db;
  border-radius: 10px;
  padding: 10px 12px;
  font-size: 1rem;
}

button {
  border: none;
  border-radius: 10px;
  background: #2563eb;
  color: white;
  padding: 10px 14px;
  font-weight: 600;
  cursor: pointer;
}
</style>