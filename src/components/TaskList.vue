<script setup>
import TaskItem from './TaskItem.vue'

defineProps({
  tareas: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['alternar', 'eliminar'])
</script>

<template>
  <ul v-if="tareas.length" class="task-list">
    <TaskItem
      v-for="tarea in tareas"
      :key="tarea.id"
      :tarea="tarea"
      @alternar="emit('alternar', $event)"
      @eliminar="emit('eliminar', $event)"
    />
  </ul>
  <p v-else class="empty-state">No hi ha tasques per mostrar.</p>
</template>

<style scoped>
.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.empty-state {
  margin: 14px 0 0;
  padding: 12px;
  border-radius: 10px;
  background: #f9fafb;
  color: #6b7280;
  text-align: center;
}
</style>