<template>
  <div>
    <h2>Listado de Personajes</h2>
    
    <div v-if="cargando">Cargando personajes...</div>
    
    <!-- PASO 1: Mostrar el nombre de cada personaje -->
    <!-- <ul v-else>
      <li v-for="personaje in personajes" :key="personaje.id">
        {{ personaje.name }}
      </li>
    </ul> -->

    <!-- PASO 2: Mostrar una tarjeta con la imagen, el nombre y el estado de cada personaje -->
    <div v-else class="contenedor-grid">
      <TarjetaPersonaje 
        v-for="personaje in personajes" 
        :key="personaje.id" 
        :personaje="personaje" 
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import TarjetaPersonaje from './TarjetaPersonaje.vue';

const personajes = ref([]);
const cargando = ref(true);

// Esta es la función que obtiene los datos
async function obtenerPersonajes() {
  cargando.value = true;
  try {
    const response = await axios.get('https://rickandmortyapi.com/api/character');
    personajes.value = response.data.results;
  } catch (error) {
    console.error('Error al cargar:', error);
  } finally {
    cargando.value = false;
  }
}

// "El gancho": le decimos a Vue que ejecute la función en cuanto monte el componente
onMounted(() => {
  obtenerPersonajes();
});
</script>