<template>
  <div class="register-container">
    <form @submit.prevent="register" class="register-form">
      <h2 class="register-title">📝 Registrar Usuario</h2>
      <input v-model="email" type="email" placeholder="Correo electrónico" required />
      <input v-model="password" type="password" placeholder="Contraseña" required />
      <button type="submit">Registrar</button>
      
      <p v-if="error" class="error-message">{{ error }}</p>
      <p v-if="success" class="success-message">{{ success }}</p>
      
      <p class="login-redirect">
        ¿Ya tienes cuenta?
        <router-link to="/login">Inicia sesión aquí</router-link>
      </p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const email = ref('')
const password = ref('')
const error = ref('')
const success = ref('')

const register = async () => {
  error.value = ''
  success.value = ''
  try {
    const res = await axios.post('http://localhost:4000/api/auth/register', {
      email: email.value,
      password: password.value
    })
    success.value = res.data.message
    email.value = ''
    password.value = ''
  } catch (err) {
    error.value = err.response?.data?.message || 'Error desconocido'
  }
}
</script>

<style scoped>
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 1.5rem;
  background: linear-gradient(to right, #6a11cb, #2575fc);
}

.register-form {
  background: #ffffff;
  padding: 2.8rem 2.2rem;
  border-radius: 20px;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.25);
  width: 100%;
  max-width: 420px;
  display: flex;
  flex-direction: column;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.register-form:hover {
  transform: translateY(-4px);
  box-shadow: 0 25px 60px rgba(0, 0, 0, 0.3);
}

.register-title {
  text-align: center;
  font-size: 2rem;
  color: #2d3748;
  margin-bottom: 1.8rem;
  font-weight: bold;
}

.register-form input {
  padding: 1rem;
  margin-bottom: 1.3rem;
  font-size: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.register-form input:focus {
  border-color: #6a11cb;
  box-shadow: 0 0 0 3px rgba(106, 17, 203, 0.25);
  outline: none;
}

.register-form button {
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: #fff;
  border: none;
  padding: 1rem;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 0.5rem;
  box-shadow: 0 5px 12px rgba(0, 0, 0, 0.15);
}

.register-form button:hover {
  transform: scale(1.03);
  background: linear-gradient(to right, #2575fc, #6a11cb);
  box-shadow: 0 10px 22px rgba(0, 0, 0, 0.2);
}

.error-message,
.success-message {
  text-align: center;
  font-size: 0.95rem;
  padding: 0.75rem;
  margin-top: 1rem;
  border-radius: 8px;
  font-weight: 500;
}

.error-message {
  background: #ffe6e6;
  color: #e53935;
}

.success-message {
  background: #e6fffa;
  color: #2f855a;
}

.login-redirect {
  text-align: center;
  margin-top: 1.5rem;
  font-size: 0.95rem;
  color: #4a5568;
}

.login-redirect a {
  color: #6a11cb;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.2s ease;
}

.login-redirect a:hover {
  color: #2575fc;
  text-decoration: underline;
}

@media (max-width: 480px) {
  .register-form {
    padding: 2rem 1.5rem;
    border-radius: 14px;
  }

  .register-title {
    font-size: 1.6rem;
  }
}
</style>
