<template>
  <div class="container">
    <header class="header">
      <div class="button-container">
        <button :class="{ active: selectedMenu === 'Todos' }" @click="selectedMenu = 'Todos'">Todos</button>
        <button :class="{ active: selectedMenu === 'Posts' }" @click="selectedMenu = 'Posts'">Posts</button>
      </div>
    </header>

    <main>
      <Todos v-if="selectedMenu === 'Todos'" />
      <Posts v-if="selectedMenu === 'Posts'" :users="users" :posts="posts" />
    </main>

    <div v-if="loading" class="loading">
      <div class="icons">
        <i class="ri-arrow-left-s-line"></i>
        <i class="ri-arrow-right-s-line"></i>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Todos from './components/Todos.vue'
import Posts from './components/Posts.vue'

const selectedMenu = ref('Todos')
const users = ref([])
const posts = ref([])
const loading = ref(true)

onMounted(async () => {
  // Fetch users and posts data from the API
  const usersResponse = await fetch('https://jsonplaceholder.typicode.com/users')
  const postsResponse = await fetch('https://jsonplaceholder.typicode.com/posts')
  users.value = await usersResponse.json()
  posts.value = await postsResponse.json()
  loading.value = false
})
</script>

<style scoped>
.container {
  background: linear-gradient(135deg, #f06, #ffba00);
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

.header {
  background: linear-gradient(135deg, #f06, #ffba00);
  padding: 1rem;
  width: 100%;
  display: flex;
  justify-content: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.button-container {
  display: flex;
  gap: 1rem;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  background-color: #ff6f00;
  color: white;
  font-size: 1rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button.active {
  background-color: #ff6f00;
}

button:hover {
  background-color: #0056b3;
}

main {
  flex: 1;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.8);
  padding: 1rem;
  border-radius: 8px;
}

.loading .icons {
  display: flex;
  gap: 1rem;
  animation: loadingAnimation 1s infinite alternate;
}

@keyframes loadingAnimation {
  from {
    transform: translateX(-10px);
  }
  to {
    transform: translateX(10px);
  }
}
</style>
