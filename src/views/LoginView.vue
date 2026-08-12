<template>
  <div class="login-container">
    <form class="login-form" @submit.prevent="handleLogin">
      <h1>Entrar</h1>

      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

      <div class="field">
        <label for="email">Email</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="seu@email.com"
          required
          autocomplete="email"
        />
      </div>

      <div class="field">
        <label for="password">Senha</label>
        <input
          id="password"
          v-model="password"
          type="password"
          placeholder="••••••••"
          required
          autocomplete="current-password"
        />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? 'Entrando...' : 'Entrar' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAuthStore } from '../stores/auth';

const router = useRouter();
const authStore = useAuthStore();

const email = ref('');
const password = ref('');
const loading = ref(false);
const errorMessage = ref('');

async function handleLogin() {
  loading.value = true;
  errorMessage.value = '';
  try {
    await authStore.login(email.value, password.value);
    router.push('/');
  } catch (err) {
    errorMessage.value =
      err.response?.data?.detail ??
      'Erro ao entrar. Verifique suas credenciais.';
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>
.login-container {
  padding: 2rem 1rem;
}

.login-form {
  width: 100%;
  max-width: 340px;
  margin: 0 auto;
  background: #ffffff;
  border: 1px solid #eaeaea;
  border-radius: 10px;
  padding: 2rem 1.75rem;
}

.login-form h1 {
  margin: 0 0 1.5rem;
  font-size: 1.25rem;
  font-weight: 600;
  color: #222;
  text-align: center;
}

.field {
  margin-bottom: 1.1rem;
}

.field label {
  display: block;
  margin-bottom: 0.4rem;
  font-size: 0.8rem;
  font-weight: 500;
  color: #666;
}

.field input {
  width: 100%;
  padding: 0.6rem 0.75rem;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  font-size: 0.9rem;
  color: #222;
  outline: none;
  transition: border-color 0.15s ease;
  box-sizing: border-box;
}

.field input:focus {
  border-color: #2f5bd9;
}

/* Remove o fundo amarelo do autofill do navegador */
.field input:-webkit-autofill,
.field input:-webkit-autofill:hover,
.field input:-webkit-autofill:focus {
  -webkit-box-shadow: 0 0 0 1000px #ffffff inset;
  -webkit-text-fill-color: #222;
  transition: background-color 5000s ease-in-out 0s;
}

.login-form button {
  width: 100%;
  padding: 0.65rem;
  margin-top: 0.5rem;
  background: #2f5bd9;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.15s ease;
}

.login-form button:hover:not(:disabled) {
  background: #244aad;
}

.login-form button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.error-message {
  padding: 0.6rem 0.75rem;
  margin-bottom: 1rem;
  background: #fdf0ef;
  border: 1px solid #f5c6c3;
  border-radius: 6px;
  color: #c0392b;
  font-size: 0.82rem;
}
</style>