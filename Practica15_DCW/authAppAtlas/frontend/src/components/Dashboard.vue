<template>
  <div class="dashboard-container">
    <div class="dashboard-card">
      <h1 class="dashboard-title">👋 Bienvenido, {{ auth.user?.name }}</h1>
      <p class="dashboard-message">Has iniciado sesión correctamente.</p>
      <button @click="cerrarSesion" class="dashboard-button">
        <span>Cerrar sesión</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'

const auth = useAuthStore()
const router = useRouter()

const cerrarSesion = () => {
  auth.logout()
  router.push('/login')
}
</script>

<style scoped>
.dashboard-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 2rem;
  background: linear-gradient(to right, #667eea, #764ba2);
}

.dashboard-card {
  background: #ffffff;
  padding: 3rem 2.5rem;
  border-radius: 18px;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
  max-width: 420px;
  width: 100%;
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.dashboard-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2);
}

.dashboard-title {
  font-size: 2.2rem;
  margin-bottom: 1rem;
  color: #2c3e50;
  font-weight: bold;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.dashboard-message {
  font-size: 1.1rem;
  color: #6b7280;
  margin-bottom: 2rem;
}

.dashboard-button {
  background: linear-gradient(to right, #ff416c, #ff4b2b);
  color: #fff;
  border: none;
  padding: 1rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 12px;
  width: 100%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.dashboard-button:hover {
  background: linear-gradient(to right, #ff4b2b, #ff416c);
  transform: scale(1.03);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

@media (max-width: 480px) {
  .dashboard-card {
    padding: 2rem;
    border-radius: 14px;
  }

  .dashboard-title {
    font-size: 1.7rem;
  }

  .dashboard-message {
    font-size: 1rem;
  }
}
</style>
