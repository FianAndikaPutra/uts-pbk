<template>
  <div>
    <h1>LIST KEGIATAN</h1>

    <div>
      <label>
        <input type="checkbox" v-model="showCompleted" />
        Tampilkan Selesai
      </label>
    </div>

    <div>
      <input v-model="newTask" placeholder="Tulis Kegiatan Mu..." />
      <input type="date" v-model="taskDate" />
      <input type="time" v-model="taskTime" />
      <button @click="addTask">Tambah</button>
    </div>

    <transition-group name="fade" tag="ul">
      <li v-for="(task, index) in filteredTasks" :key="index">
        <span @click="toggleTask(index)">
          <span v-if="task.done">[✔]</span>
          <span v-else>[ ]</span>
          {{ task.text }} - {{ task.date }}, {{ task.time }}
        </span>
        <button @click="deleteTask(index)">Hapus</button>
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
const showCompleted = ref(false)

const filteredTasks = computed(() => {
  return showCompleted.value
    ? tasks.value
    : tasks.value.filter(task => !task.done)
})

function addTask() {
  const text = newTask.value.trim()
  if (text && taskDate.value && taskTime.value) {
    tasks.value.push({
      text,
      done: false,
      date: taskDate.value,
      time: taskTime.value
    })
    newTask.value = ''
    taskDate.value = ''
    taskTime.value = ''
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
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>