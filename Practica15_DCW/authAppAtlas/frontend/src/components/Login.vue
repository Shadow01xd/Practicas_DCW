<template>
  <div class="login-container">
    <form @submit.prevent="login" class="login-form">
      <h2 class="login-title">🔐 Iniciar Sesión</h2>
      <input v-model="email" type="email" placeholder="Correo electrónico" required />
      <input v-model="password" type="password" placeholder="Contraseña" required />
      <button type="submit">Ingresar</button>
      <p v-if="error" class="error-message">{{ error }}</p>
      <p class="register-redirect">
        ¿No tienes cuenta?
        <router-link to="/register">Regístrate aquí</router-link>
      </p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '../stores/auth'

const email = ref('')
const password = ref('')
const error = ref('')

const router = useRouter()
const auth = useAuthStore()

const login = async () => {
  error.value = ''
  try {
    const success = await auth.login(email.value, password.value)
    if (success) {
      router.push('/dashboard')
    }
  } catch (err) {
    error.value = err.response?.data?.message || err.message || 'Error desconocido'
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 1.5rem;
  background: linear-gradient(to right, #232526, #414345);
}

.login-form {
  background: #ffffff;
  padding: 2.5rem 2rem;
  border-radius: 18px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.3);
  max-width: 420px;
  width: 100%;
  display: flex;
  flex-direction: column;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.login-form:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.35);
}

.login-title {
  text-align: center;
  font-size: 2rem;
  color: #2c3e50;
  margin-bottom: 1.8rem;
  font-weight: bold;
}

.login-form input {
  padding: 1rem;
  margin-bottom: 1.25rem;
  font-size: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  transition: border 0.3s ease, box-shadow 0.3s ease;
}

.login-form input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.25);
  outline: none;
}

.login-form button {
  padding: 1rem;
  background: linear-gradient(to right, #667eea, #764ba2);
  color: #fff;
  font-size: 1.1rem;
  font-weight: 600;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

.login-form button:hover {
  transform: translateY(-2px);
  background: linear-gradient(to right, #764ba2, #667eea);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.25);
}

.error-message {
  margin-top: 1rem;
  padding: 0.75rem;
  font-size: 0.95rem;
  background: #ffe6e6;
  color: #e53935;
  border-radius: 8px;
  text-align: center;
  font-weight: 500;
}

.register-redirect {
  text-align: center;
  margin-top: 1.5rem;
  font-size: 0.95rem;
  color: #555;
}

.register-redirect a {
  color: #667eea;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.2s ease;
}

.register-redirect a:hover {
  color: #764ba2;
  text-decoration: underline;
}

@media (max-width: 480px) {
  .login-form {
    padding: 2rem 1.5rem;
    border-radius: 14px;
  }

  .login-title {
    font-size: 1.6rem;
  }
}
</style>
