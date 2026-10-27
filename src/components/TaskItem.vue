<script setup>
defineProps({
  tarea: {
    type: Object,
    required: true,
    validator: (t) => 'id' in t && 'titulo' in t && 'completada' in t,
  },
})

const emit = defineEmits(['alternar', 'eliminar'])
</script>

<template>
  <li class="task-item">
    <span :class="{ completada: tarea.completada }" class="task-title">{{ tarea.titulo }}</span>
    <div class="actions">
      <button class="toggle" @click="emit('alternar', tarea.id)">
        {{ tarea.completada ? 'Desmarcar' : 'Completar' }}
      </button>
      <button class="delete" @click="emit('eliminar', tarea.id)" aria-label="Eliminar tasca">🗑️</button>
    </div>
  </li>
</template>

<style scoped>
.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 12px 14px;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  background: #f9fafb;
}

.task-title {
  flex: 1;
  color: #111827;
}

.actions {
  display: flex;
  gap: 8px;
}

button {
  border: none;
  border-radius: 8px;
  padding: 7px 10px;
  cursor: pointer;
  font-size: 0.85rem;
}

.toggle {
  background: #dbeafe;
  color: #1d4ed8;
}

.delete {
  background: #fee2e2;
  color: #b91c1c;
}

.completada {
  text-decoration: line-through;
  color: #9ca3af;
}
</style>