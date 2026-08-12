<template>
  <header class="app-header">
    <h1>Minhas tarefas</h1>
    <nav>
      <router-link to="/">Início</router-link>
      <router-link to="/about">Sobre</router-link>
      <button
        v-if="authStore.isAuthenticated"
        class="logout-btn"
        @click="handleLogout"
      >
        Sair
      </button>
    </nav>
  </header>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { useAuthStore } from '../stores/auth';

const router = useRouter();
const authStore = useAuthStore();

function handleLogout() {
  authStore.logout();
  router.push('/login');
}
</script>

<style scoped>
.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.85rem 1.5rem;
  background: #ffffff;
  border-bottom: 1px solid #eaeaea;
  margin-bottom: 1.5rem;
}

.app-header h1 {
  margin: 0;
  font-size: 1rem;
  font-weight: 600;
  color: #222;
}

.app-header nav {
  display: flex;
  align-items: center;
  gap: 1.25rem;
}

.app-header nav a {
  color: #888;
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 500;
  transition: color 0.15s ease;
}

.app-header nav a:hover,
.app-header nav a.router-link-exact-active {
  color: #2f5bd9;
}

.logout-btn {
  padding: 0.35rem 0.8rem;
  background: transparent;
  color: #888;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  font-size: 0.8rem;
  cursor: pointer;
  transition: all 0.15s ease;
}

.logout-btn:hover {
  color: #2f5bd9;
  border-color: #2f5bd9;
}
</style>