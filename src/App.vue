<script setup lang="ts">
import { computed, ref } from 'vue'
import TaskList from './components/TaskList.vue'
import type { Task } from './types/task'

const tasks = ref<Task[]>([
  { id: 1, name: 'Przygotować plan dnia', completed: true },
  { id: 2, name: 'Dokończyć projekt Vue', completed: false },
  { id: 3, name: 'Przeczytać 20 stron książki', completed: false },
])

const newTaskName = ref('')
let nextId = 4

const completedCount = computed(
  () => tasks.value.filter((task) => task.completed).length,
)

function addTask(): void {
  const name = newTaskName.value.trim()
  if (!name) return

  tasks.value.push({
    id: nextId++,
    name,
    completed: false,
  })
  newTaskName.value = ''
}

function toggleTask(taskId: number): void {
  const task = tasks.value.find((item) => item.id === taskId)
  if (task) task.completed = !task.completed
}

function removeTask(taskId: number): void {
  tasks.value = tasks.value.filter((task) => task.id !== taskId)
}
</script>

<template>
  <main class="page-shell">
    <section class="todo-app" aria-labelledby="page-title">
      <header class="app-header">
        <div class="eyebrow"><span class="eyebrow-dot"></span> TWÓJ PLAN NA DZIŚ</div>
        <h1 id="page-title">Lista <span>zadań.</span></h1>
        <p class="subtitle">Małe kroki prowadzą do wielkich rzeczy.</p>
        <div class="progress-wrap">
          <div class="progress-label">
            <span>Postęp</span>
            <strong>{{ completedCount }} z {{ tasks.length }} ukończonych</strong>
          </div>
          <div
            class="progress-track"
            role="progressbar"
            :aria-valuenow="completedCount"
            :aria-valuemin="0"
            :aria-valuemax="tasks.length"
            aria-label="Postęp realizacji zadań"
          >
            <div
              class="progress-fill"
              :style="{ width: `${tasks.length ? (completedCount / tasks.length) * 100 : 0}%` }"
            ></div>
          </div>
        </div>
      </header>

      <form class="add-form" @submit.prevent="addTask">
        <label class="sr-only" for="new-task">Nazwa nowego zadania</label>
        <input
          id="new-task"
          v-model="newTaskName"
          type="text"
          placeholder="Co masz do zrobienia?"
          maxlength="120"
        />
        <button class="add-button" type="submit" :disabled="!newTaskName.trim()">
          <span aria-hidden="true">＋</span> Dodaj
        </button>
      </form>

      <div class="list-heading">
        <h2>Twoje zadania</h2>
        <span class="task-count">{{ tasks.length }}</span>
      </div>

      <TaskList
        :tasks="tasks"
        @toggle-task="toggleTask"
        @remove-task="removeTask"
      />

      <footer class="app-footer">
        <span class="footer-sparkle" aria-hidden="true">✳</span>
        <span>{{ tasks.length - completedCount }} zadań czeka na Ciebie</span>
      </footer>
    </section>
    <p class="credit">PROSTO · ŚWIADOMIE · KROK PO KROKU</p>
  </main>
</template>
