<script setup lang="ts">
import type { Task } from '../types/task'

defineProps<{
  task: Task
}>()

const emit = defineEmits<{
  (event: 'toggle', taskId: number): void
  (event: 'remove', taskId: number): void
}>()
</script>

<template>
  <article class="task-row" :class="{ 'is-completed': task.completed }">
    <button
      class="check-button"
      type="button"
      :aria-label="task.completed ? 'Oznacz jako niewykonane' : 'Oznacz jako wykonane'"
      :aria-pressed="task.completed"
      @click="emit('toggle', task.id)"
    >
      <span v-if="task.completed" aria-hidden="true">✓</span>
    </button>
    <span class="task-name">{{ task.name }}</span>
    <span v-if="task.completed" class="done-label">GOTOWE</span>
    <button
      class="delete-button"
      type="button"
      :aria-label="`Usuń zadanie: ${task.name}`"
      title="Usuń zadanie"
      @click="emit('remove', task.id)"
    >×</button>
  </article>
</template>
