<template>
    <div class="post-container">
      <div class="container">
        <h1 class="title">Daftar Posts</h1>
        <label for="user-select">Filter by User:</label>
        <select id="user-select" v-model="selectedUser">
          <option value="">All Users</option>
          <option v-for="user in users" :key="user.id" :value="user.id">
            {{ user.name }}
          </option>
        </select>
        <ul class="posts-list">
          <li v-for="post in filteredPosts" :key="post.id" class="post-item">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  
  const props = defineProps({
    users: Array,
    posts: Array
  })
  
  const selectedUser = ref(null)
  
  const filteredPosts = computed(() => {
    if (!selectedUser.value) return props.posts
    return props.posts.filter(post => post.userId === selectedUser.value)
  })
  </script>
  
  <style scoped>
  /* Styles are the same as before */
  </style>
  