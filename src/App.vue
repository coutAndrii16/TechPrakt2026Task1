<script setup lang="ts">
import { ref } from 'vue'

interface Task {
  id: number
  text: string
  completed: boolean
}

const inputText = ref<string>('')
const tasks = ref<Task[]>([])
let nextId = 1

function addTask(): void {
  const trimmed = inputText.value.trim()
  if (!trimmed) return
  tasks.value.push({ id: nextId++, text: trimmed, completed: false })
  inputText.value = ''
}

function toggleTask(task: Task): void {
  task.completed = !task.completed
}

function deleteTask(id: number): void {
  tasks.value = tasks.value.filter(t => t.id !== id)
}

function handleKeydown(event: KeyboardEvent): void {
  if (event.key === 'Enter') addTask()
}
</script>

<template>
  <div class="app">
    <div class="card">
      <h1 class="title">
        <span class="title-icon">✓</span>
        Трекер завдань
      </h1>

      <div class="input-row">
        <input
            v-model="inputText"
            class="task-input"
            type="text"
            placeholder="Введіть нове завдання..."
            @keydown="handleKeydown"
        />
        <button class="add-btn" @click="addTask">Додати</button>
      </div>

      <p v-if="tasks.length === 0" class="empty-hint">
        Завдань поки немає. Додайте перше!
      </p>

      <ul v-else class="task-list">
        <li
            v-for="task in tasks"
            :key="task.id"
            class="task-item"
            :class="{ completed: task.completed }"
        >
          <span class="task-text" @click="toggleTask(task)">
            <span class="check-icon">{{ task.completed ? '✓' : '○' }}</span>
            {{ task.text }}
          </span>
          <button class="delete-btn" title="Видалити" @click="deleteTask(task.id)">✕</button>
        </li>
      </ul>

      <div v-if="tasks.length > 0" class="footer-stats">
        <span>Всього: {{ tasks.length }}</span>
        <span>Виконано: {{ tasks.filter(t => t.completed).length }}</span>
        <span>Залишилось: {{ tasks.filter(t => !t.completed).length }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.app {
  min-height: 100vh;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding: 48px 16px;
  background: linear-gradient(135deg, #1a1f3c 0%, #2d3561 50%, #1a1f3c 100%);
  font-family: 'Inter', sans-serif;
}

.card {
  width: 100%;
  max-width: 560px;
  background: #ffffff;
  border-radius: 20px;
  padding: 36px 40px;
  box-shadow: 0 24px 60px rgba(0, 0, 0, 0.35);
}

.title {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 1.75rem;
  font-weight: 700;
  color: #1a1f3c;
  margin: 0 0 28px;
  letter-spacing: -0.5px;
}

.title-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  background: #2d3561;
  color: #ffffff;
  border-radius: 10px;
  font-size: 1.1rem;
}

.input-row {
  display: flex;
  gap: 10px;
  margin-bottom: 28px;
}

.task-input {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  font-size: 0.95rem;
  font-family: 'Inter', sans-serif;
  color: #1a1f3c;
  outline: none;
  transition: border-color 0.2s;
}

.task-input:focus {
  border-color: #2d3561;
}

.task-input::placeholder {
  color: #a0aec0;
}

.add-btn {
  padding: 12px 22px;
  background: #2d3561;
  color: #ffffff;
  border: none;
  border-radius: 10px;
  font-size: 0.95rem;
  font-weight: 600;
  font-family: 'Inter', sans-serif;
  cursor: pointer;
  transition: background 0.2s, transform 0.1s;
  white-space: nowrap;
}

.add-btn:hover {
  background: #3d4a7a;
}

.add-btn:active {
  transform: scale(0.97);
}

.empty-hint {
  text-align: center;
  color: #a0aec0;
  font-size: 0.9rem;
  padding: 24px 0;
  margin: 0;
}

.task-list {
  list-style: none;
  margin: 0 0 20px;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 14px;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  transition: border-color 0.2s, background 0.2s;
}

.task-item:hover {
  border-color: #c3cfe8;
  background: #f8f9ff;
}

.task-item.completed {
  background: #f0fdf4;
  border-color: #bbf7d0;
}

.task-text {
  display: flex;
  align-items: center;
  gap: 10px;
  flex: 1;
  cursor: pointer;
  font-size: 0.95rem;
  color: #1a1f3c;
  user-select: none;
  transition: color 0.2s;
}

.task-item.completed .task-text {
  text-decoration: line-through;
  color: #9ca3af;
}

.check-icon {
  font-size: 1rem;
  color: #2d3561;
  min-width: 18px;
}

.task-item.completed .check-icon {
  color: #16a34a;
}

.delete-btn {
  background: none;
  border: none;
  color: #cbd5e1;
  font-size: 0.85rem;
  cursor: pointer;
  padding: 4px 6px;
  border-radius: 6px;
  line-height: 1;
  transition: color 0.2s, background 0.2s;
}

.delete-btn:hover {
  color: #ef4444;
  background: #fee2e2;
}

.footer-stats {
  display: flex;
  gap: 20px;
  padding-top: 16px;
  border-top: 2px solid #f1f5f9;
  font-size: 0.82rem;
  color: #94a3b8;
  font-weight: 500;
}
</style>