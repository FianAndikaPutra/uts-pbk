<template>
  <div>
    <h1>LIST KEGIATAN</h1>

    <div>
      <input v-model="newTask" placeholder="Tulis Kegiatan Mu..." />
      <button @click="addTask">Tambah</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')

function addTask() {
  console.log('Tambah:', newTask.value)
}
</script>

<style scoped>
</style>