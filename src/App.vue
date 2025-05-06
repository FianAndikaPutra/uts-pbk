<template>
  <div>
    <h1>LIST KEGIATAN</h1>

    <div>
      <input v-model="newTask" placeholder="Tulis Kegiatan Mu..." />
      <button @click="addTask">Tambah</button>
    </div>

    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task.text }}
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
    tasks.value.push({ text })
    newTask.value = ''
  }
}
</script>

<style scoped>
</style>