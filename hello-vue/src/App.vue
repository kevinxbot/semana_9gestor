<template>
  <div class="container">
    <h1>Gestor de tareas</h1>

    <!-- Input con v-model -->
    <div class="input-section">
      <input 
        v-model="newTask" 
        placeholder="Escribe el nombre de la tarea" 
        @keyup.enter="addTask" 
      />
      <button class="btn-agregar" @click="addTask">Agregar</button>
    </div>

    <hr />

    <!-- v-if / v-else -->
    <div v-if="tasks.length > 0">
      <h2>Listado de tareas</h2>
      
      <div class="board">
        <!-- Columna To Do -->
        <div class="column todo">
          <h3>To Do</h3>
          <div class="task-list">
            <div 
              v-for="task in todoTasks" 
              :key="task.id" 
              class="task-item"
            >
              <span class="task-name">{{ task.name }}</span>
              <button class="task-btn" @click="moveTask(task.id, 'todo')">→</button>
            </div>
          </div>
        </div>

        <!-- Columna Doing -->
        <div class="column doing">
          <h3>Doing</h3>
          <div class="task-list">
            <div 
              v-for="task in doingTasks" 
              :key="task.id" 
              class="task-item"
            >
              <span class="task-name">{{ task.name }}</span>
              <button class="task-btn" @click="moveTask(task.id, 'doing')">→</button>
            </div>
          </div>
        </div>

        <!-- Columna Done -->
        <div class="column done">
          <h3>Done</h3>
          <div class="task-list">
            <div 
              v-for="task in doneTasks" 
              :key="task.id" 
              class="task-item"
            >
              <span class="task-name">{{ task.name }}</span>
              <button class="task-btn">X</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-else>
      <p class="no-tasks">No hay tareas registradas</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Modelo: variables reactivas
const newTask = ref('')
const tasks = ref([])
let taskIdCounter = 0

// Computed: filtrar tareas por estado
const todoTasks = computed(() => tasks.value.filter(t => t.status === 'todo'))
const doingTasks = computed(() => tasks.value.filter(t => t.status === 'doing'))
const doneTasks = computed(() => tasks.value.filter(t => t.status === 'done'))

// ViewModel: lógica del componente
const addTask = () => {
  const name = newTask.value.trim()
  if (name !== '') {
    tasks.value.push({
      id: taskIdCounter++,
      name: name,
      status: 'todo'
    })
    newTask.value = ''
  }
}

const moveTask = (taskId, currentStatus) => {
  const task = tasks.value.find(t => t.id === taskId)
  if (!task) return

  if (currentStatus === 'todo') {
    task.status = 'doing'
  } else if (currentStatus === 'doing') {
    task.status = 'done'
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.container {
  max-width: 900px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
  background: white;
  padding: 40px;
  border-radius: 8px;
}

h1 {
  color: #2c3e50;
  margin-bottom: 40px;
  font-size: 3em;
  font-weight: 700;
}

.input-section {
  display: flex;
  gap: 10px;
  align-items: center;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 12px 16px;
  font-size: 15px;
  border: 2px solid #ccc;
  border-radius: 6px;
  color: #666;
}

input:focus {
  outline: none;
  border-color: #999;
}

input::placeholder {
  color: #999;
}

.btn-agregar {
  padding: 12px 24px;
  background: transparent;
  border: none;
  color: #e74c3c;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
}

.btn-agregar:hover {
  color: #c0392b;
}

hr {
  margin: 30px 0;
  border: none;
  border-top: 2px solid #ddd;
}

h2 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
  font-size: 2em;
  font-weight: 600;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0;
  border: 2px dashed #999;
  border-radius: 8px;
  overflow: hidden;
}

.column {
  padding: 30px 20px;
  min-height: 400px;
  border-right: 2px dashed #999;
}

.column:last-child {
  border-right: none;
}

.column h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 25px;
  font-size: 1.5em;
  font-weight: 600;
}

.task-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 15px;
}

.todo .task-item {
  background: #3498db;
  color: white;
}

.doing .task-item {
  background: #f39c12;
  color: white;
}

.done .task-item {
  background: #e74c3c;
  color: white;
}

.task-name {
  flex: 1;
  font-weight: 500;
}

.task-btn {
  background: white;
  border: none;
  width: 32px;
  height: 32px;
  border-radius: 6px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 16px;
  transition: all 0.2s;
}

.todo .task-btn {
  color: #3498db;
}

.doing .task-btn {
  color: #f39c12;
}

.done .task-btn {
  color: #e74c3c;
}

.task-btn:hover {
  transform: scale(1.1);
}

.no-tasks {
  text-align: center;
  color: #999;
  font-size: 16px;
  padding: 60px 20px;
}

@media (max-width: 768px) {
  h1 {
    font-size: 2em;
  }

  .board {
    grid-template-columns: 1fr;
  }

  .column {
    border-right: none;
    border-bottom: 2px dashed #999;
  }

  .column:last-child {
    border-bottom: none;
  }
}
</style>