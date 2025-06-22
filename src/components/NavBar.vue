<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <header class="navbar-header">
    <nav class="navbar-container">
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/play" class="nav-link">Play</router-link>
    </nav>
  </header>
</template>

<style scoped>
.navbar-header {
  background-color: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(4px);
  z-index: 10;
  box-shadow:
    0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 0;
  width: 100%;
}

.navbar-container {
  max-width: 1280px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 1rem;
  padding-right: 1rem;
  display: flex;
  justify-content: center;
  gap: 0.5rem;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-weight: 700;
  font-size: 1.125rem;
  padding: 0.75rem 1.25rem;
  border-bottom: 4px solid transparent;
  transition: border-color 0.3s ease;
}

.nav-link:hover {
  border-color: rgba(250, 204, 21, 0.5);
}

.router-link-exact-active {
  border-color: #facc15;
}
</style>
