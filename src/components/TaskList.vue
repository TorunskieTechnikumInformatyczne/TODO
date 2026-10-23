<script setup lang="ts">
import TaskItem from './TaskItem.vue'
import type { Task } from '../types/task'

defineProps<{
  tasks: Task[]
}>()

const emit = defineEmits<{
  (event: 'toggle-task', taskId: number): void
  (event: 'remove-task', taskId: number): void
}>()
</script>

<template>
  <div v-if="tasks.length" class="task-list">
    <TaskItem
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @toggle="emit('toggle-task', $event)"
      @remove="emit('remove-task', $event)"
    />
  </div>
  <div v-else class="empty-state">
    <span class="empty-icon" aria-hidden="true">✓</span>
    <strong>Wszystko gotowe!</strong>
    <p>Dodaj nowe zadanie, aby wypełnić swoją listę.</p>
  </div>
</template>
