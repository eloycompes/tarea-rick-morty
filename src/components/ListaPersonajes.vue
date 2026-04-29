<template>
  <div>
    <h2>Listado de Personajes</h2>

    <Buscador @buscar="handleBuscar" />
    <Filtro @filtrar="handleFiltrar" />
    <div v-if="cargando">Cargando personajes...</div>

    <div v-else-if="personajes.length === 0">
      <p>No se encontraron personajes con ese nombre.</p>
    </div>
    
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
import Buscador from './Buscador.vue';
import Filtro from './filtro.vue';

const personajes = ref([]);
const cargando = ref(true);
const estadoSeleccionado = ref('');
const nombreBusqueda = ref('');

// Esta es la función que obtiene los datos
async function obtenerPersonajes() {
  cargando.value = true;
  try {
    // La API filtra con ?name=
    const response = await axios.get(`https://rickandmortyapi.com/api/character/?name=${nombreBusqueda.value}&status=${estadoSeleccionado.value}`);
    personajes.value = response.data.results;
  } catch (error) {
    console.error('Error al cargar:', error);
    personajes.value = []; //Limpiamos la lista si hay error (ej. no encontrado)
  } finally {
    cargando.value = false;
  }
}

function handleBuscar(nombre) {
  nombreBusqueda.value = nombre; // Guardamos el nombre buscado
  obtenerPersonajes();
}

function handleFiltrar(estado) {
  estadoSeleccionado.value = estado; // Guardamos el estado seleccionado
  obtenerPersonajes();
}

// Le digo a Vue que ejecute la función en cuanto monte el componente
onMounted(() => {
  obtenerPersonajes();
});
</script>

<style scoped>
.contenedor-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>