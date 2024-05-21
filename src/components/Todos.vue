<template>
    <div class="activity-container">
      <div class="container">
        <h1 class="title">Daftar Kegiatan</h1>
        <div class="input-container">
          <input v-model="input_content" @keyup.enter="addTodo" placeholder="Nama Kegiatan" />
          <button @click="addTodo">Tambahkan Kegiatan</button>
        </div>
        <ul class="activities-list">
          <li v-for="(todo, index) in filteredTodos" :key="index" class="activity-item">
            <span v-if="todo.done">✅</span>
            <span :class="{ completed: todo.done }">{{ todo.content }}</span>
            <div class="buttons">
              <button @click="toggleTodoDone(todo)" class="action-button">
                {{ todo.done ? 'Uncheck' : 'Checklist' }}
              </button>
              <button @click="removeTodo(todo)" class="action-button">Hapus</button>
            </div>
          </li>
        </ul>
        <button @click="filterCompleted = !filterCompleted" class="filter-button">
          {{ filterCompleted ? 'Tampilkan Semua Kegiatan' : 'Tampilkan Kegiatan Yang Belum Selesai' }}
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  
  const todos = ref([])
  const input_content = ref('')
  const filterCompleted = ref(false)
  
  const todos_asc = computed(() => todos.value.sort((a, b) => b.createdAt - a.createdAt))
  const filteredTodos = computed(() => filterCompleted.value ? todos_asc.value.filter(todo => !todo.done) : todos_asc.value)
  
  const addTodo = () => {
    if (input_content.value.trim() === '') return
    todos.value.push({ content: input_content.value, done: false, createdAt: new Date().getTime() })
    input_content.value = ''
  }
  
  const removeTodo = todo => {
    const index = todos.value.findIndex(t => t === todo)
    if (index !== -1) todos.value.splice(index, 1)
  }
  
  const toggleTodoDone = todo => {
    todo.done = !todo.done
  }
  </script>
  
  <style scoped>
  /* Styles are the same as before */
  </style>
  