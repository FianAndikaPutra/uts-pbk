<template>
  <div>
    <h1>LIST KEGIATAN</h1>

    <div>
      <input v-model="newTask" placeholder="Tulis Kegiatan Mu..." />
      <button @click="addTask">Tambah</button>
    </div>

    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <span @click="toggleTask(index)">
          <span v-if="task.done">[✔]</span>
          <span v-else>[ ]</span>
          {{ task.text }}
        </span>
        <button @click="deleteTask(index)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')
const tasks = ref([])

function addTask() {
  const text = newTask.value.trim()
  if (text) {
    tasks.value.push({ text, done: false })
    newTask.value = ''
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
</style>