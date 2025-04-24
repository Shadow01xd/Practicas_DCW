<template>
  <div class="pokemon-container">
    <!-- Encabezado -->
    <header class="header">
      <h1 class="title">Pokédex en Vue.js</h1>
      <p class="subtitle">Ejemplo de consumo de PokeAPI con Composition API</p>
    </header>

    <!-- Carga -->
    <div v-if="loading" class="loading">Cargando Pokémon...</div>

    <!-- Grid de Pokémon -->
    <div v-else class="pokemon-grid">
      <div v-for="pokemon in pokemons" :key="pokemon.id" class="pokemon-card">
        <div class="pokemon-image">
          <img :src="pokemon.image" :alt="pokemon.name" />
        </div>
        <div class="pokemon-info">
          <h2 class="pokemon-name">{{ pokemon.name }}</h2>
          <p class="pokemon-id">#{{ pokemon.id.toString().padStart(3, '0') }}</p>
          <div class="pokemon-types">
            <span
              v-for="type in pokemon.types"
              :key="type"
              :class="['type-badge', type]"
            >
              {{ type }}
            </span>
          </div>
        </div>
      </div>
    </div>

    <!-- Pie de página -->
    <footer class="footer">
      Creado con Vue.js y PokeAPI
    </footer> 
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const pokemons = ref([])
const loading = ref(true)

onMounted(async () => {
  try {
    const { data } = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20')
    const details = await Promise.all(data.results.map(p => axios.get(p.url)))

    pokemons.value = details.map(d => ({
      id: d.data.id,
      name: d.data.name,
      image: d.data.sprites.other['official-artwork'].front_default,
      types: d.data.types.map(t => t.type.name)
    }))
  } catch (err) {
    console.error('Error al cargar Pokémon:', err)
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
/* Contenedor general */
.pokemon-container {
  padding: 40px 20px;
  max-width: 1200px;
  margin: 0 auto;
  font-family: 'Segoe UI', sans-serif;
  background-color: #f2f2f2;
}

/* Encabezado */
.header {
  background-color: #e53935;
  color: white;
  padding: 25px;
  border-radius: 12px;
  margin-bottom: 40px;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

.title {
  font-size: 2.5rem;
  margin: 0;
}

.subtitle {
  font-size: 1.1rem;
  margin-top: 5px;
}

/* Mensaje de carga */
.loading {
  text-align: center;
  font-size: 1.5rem;
  padding: 2rem;
}

/* Grilla de cartas */
.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(210px, 1fr));
  gap: 25px;
}

/* Tarjeta de Pokémon */
.pokemon-card {
  background-color: #ffffff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.pokemon-card:hover {
  transform: translateY(-6px);
}

.pokemon-image {
  background-color: #f7f7f7;
  padding: 25px;
  text-align: center;
}

.pokemon-image img {
  max-width: 120px;
  height: auto;
}

.pokemon-info {
  padding: 18px;
  text-align: center;
}

.pokemon-name {
  margin: 0;
  font-size: 1.3rem;
  font-weight: bold;
  color: #333;
  text-transform: capitalize;
}

.pokemon-id {
  margin: 6px 0;
  color: #888;
  font-size: 0.95rem;
}

.pokemon-types {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 10px;
}

.type-badge {
  padding: 5px 10px;
  border-radius: 6px;
  font-size: 0.8rem;
  color: white;
  font-weight: 600;
  text-transform: capitalize;
}

/* Colores por tipo */
.grass { background-color: #78c850; }
.fire { background-color: #f08030; }
.water { background-color: #6890f0; }
.electric { background-color: #f8d030; color: #333; }
.bug { background-color: #a8b820; }
.normal { background-color: #a8a878; }
.flying { background-color: #a890f0; }
.poison { background-color: #a040a0; }
.ground { background-color: #e0c068; }
.rock { background-color: #b8a038; }
.fighting { background-color: #c03028; }
.psychic { background-color: #f85888; }
.ghost { background-color: #705898; }
.ice { background-color: #98d8d8; }
.dragon { background-color: #7038f8; }
.dark { background-color: #705848; }
.steel { background-color: #b8b8d0; }
.fairy { background-color: #ee99ac; }

/* Footer */
.footer {
  text-align: center;
  margin-top: 50px;
  background-color: #222;
  color: white;
  padding: 20px;
  border-radius: 8px;
}
</style>
