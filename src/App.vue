<template>
  <div class="container">
    <h1>LIST KEGIATAN</h1>

    <!-- Filter checkbox -->
    <div class="filter-section">
      <label>
        <input type="checkbox" v-model="showCompleted" /> Tampilkan Selesai
      </label>
    </div>

    <!-- Input task -->
    <div class="input-section">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Tulis Kegiatan Mu...."
      />
      <input
        type="date"
        v-model="taskDate"
        :min="minDate"
        class="date-input"
      />
      <input
        type="time"
        v-model="taskTime"
        class="time-input"
      />
      <button @click="addTask">➕ Tambah</button>
    </div>

    <!-- Task List -->
    <transition-group name="bounce" tag="ul" class="task-list">
      <li v-for="(task, index) in filteredTasks" :key="index" class="task-item">
        <span
          @click="toggleTask(index)"
          :class="{ done: task.done }"
          class="task-text"
        >
          <i :class="task.done ? 'fas fa-check-circle' : 'far fa-circle'"></i>
          {{ task.text }}
          <span class="task-date">({{ task.date }}, {{ task.time }})</span>
        </span>
        <button class="delete-btn" @click="deleteTask(index)">🗑</button>
      </li>
    </transition-group>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const taskDate = ref('')
const taskTime = ref('')
const tasks = ref([])
const showCompleted = ref(false) // State for checkbox to show completed tasks

// Min date for date input (today's date)
const minDate = new Date().toISOString().split('T')[0]

// Filter tasks based on the checkbox
const filteredTasks = computed(() => {
  if (showCompleted.value) {
    return tasks.value // Show all tasks if checkbox is checked
  }
  return tasks.value.filter(task => !task.done) // Show only pending tasks if unchecked
})

function addTask() {
  const text = newTask.value.trim()
  if (text && taskDate.value && taskTime.value) {
    const date = taskDate.value
    const time = taskTime.value
    tasks.value.push({ text, done: false, date, time })
    newTask.value = ''
    taskDate.value = ''
    taskTime.value = ''
  } else {
    alert('Harap isi kegiatan, tanggal, dan waktu dengan benar!')
  }
}
function toggleTask(index) {
  tasks.value[index].done = !tasks.value[index].done
}

function deleteTask(index) {
  tasks.value.splice(index, 1)
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@600&display=swap');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css');

body {
  margin: 0;
  background: radial-gradient(circle at top, #2f2f6d, #5e3e92);
  font-family: 'Orbitron', sans-serif;
}

.container {
  backdrop-filter: blur(12px);
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.18);
  padding: 40px;
  border-radius: 18px;
  box-shadow: 0 0 30px rgba(0, 255, 255, 0.3);
  width: 95%;
  max-width: 600px;
  margin: 60px auto;
  color: white;
  text-align: center;
  border-radius: 25px;
  box-shadow: 0px 0px 20px rgba(0, 255, 255, 0.2);
}

h1 {
  font-size: 32px;
  margin-bottom: 30px;
  background: linear-gradient(90deg, #00ffe0, #ffb6ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 1px 1px 5px rgba(0, 255, 255, 0.4);
}

.filter-section {
  margin-bottom: 20px;
  color: white;
}

.filter-section input {
  margin-right: 8px;
}

.input-section {
  display: flex;
  gap: 12px;
  justify-content: center;
}

input[type="text"] {
  flex: 1;
  padding: 14px;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  outline: none;
  box-shadow: 0 0 12px rgba(0, 255, 255, 0.2);
}

input[type="date"],
input[type="time"] {
  padding: 12px;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  box-shadow: 0 0 12px rgba(0, 255, 255, 0.2);
}

input::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

button {
  padding: 14px 18px;
  border: none;
  border-radius: 12px;
  font-weight: bold;
  cursor: pointer;
  background: linear-gradient(45deg, #ff9a9e, #f3c6f4);
  color: #1a1a1a;
  transition: transform 0.2s, box-shadow 0.3s;
  box-shadow: 0px 0px 12px rgba(0, 255, 255, 0.3);
}

button:hover {
  transform: scale(1.1);
  box-shadow: 0px 0px 20px rgba(0, 255, 255, 0.5);
}

.task-list {
  margin-top: 30px;
  padding: 0;
  list-style: none;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  margin-bottom: 12px;
  padding: 14px 20px;
  border-radius: 12px;
  transition: all 0.3s ease;
}

.task-item:hover {
  background: rgba(0, 255, 224, 0.1);
  transform: scale(1.02);
}

.task-text {
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 16px;
}

.task-text i {
  color: #00ffe0;
}

.task-date {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.7);
  margin-left: 10px;
}

.done {
  text-decoration: line-through;
  opacity: 0.6;
  color: #00ffe0;
}

.delete-btn {
  background: transparent;
  border: none;
  color: #ff5f5f;
  font-size: 20px;
  cursor: pointer;
  transition: transform 0.2s;
}

.delete-btn:hover {
  transform: scale(1.3);
}

/* Animasi masuk/keluar */
.bounce-enter-active,
.bounce-leave-active {
  transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.bounce-enter-from {
  opacity: 0;
  transform: scale(0.5) translateY(-20px);
}

.bounce-leave-to {
  opacity: 0;
  transform: translateX(60px) rotateZ(8deg);
}
</style>