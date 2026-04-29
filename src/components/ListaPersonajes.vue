<template>
  <div>
    
    <div v-if="!personajeSeleccionado">
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
          @click="verDetalle(personaje)"
        />
      </div>

      <div class="paginacion">
        <button 
          :disabled="paginaActual === 1" 
          @click="cambiarPagina(paginaActual - 1)"
        >
          Anterior
        </button>

        <span> Página {{ paginaActual }} de {{ totalPaginas }} </span>

        <button 
          :disabled="paginaActual === totalPaginas" 
          @click="cambiarPagina(paginaActual + 1)"
        >
          Siguiente
        </button>
      </div>

    </div>

    <DetallePersonaje
      v-else
      :personaje="personajeSeleccionado"
      @volver="volverAlListado"
    />

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import TarjetaPersonaje from './TarjetaPersonaje.vue';
import Buscador from './Buscador.vue';
import Filtro from './filtro.vue';
import DetallePersonaje from './DetallePersonaje.vue';

const personajes = ref([]);
const cargando = ref(true);
const estadoSeleccionado = ref('');
const nombreBusqueda = ref('');

const paginaActual = ref(1);
const totalPaginas = ref(0);

const personajeSeleccionado = ref(null);

// Esta es la función que obtiene los datos
async function obtenerPersonajes() {
  cargando.value = true;
  try {
    // La API filtra con ?name=
    const response = await axios.get(`https://rickandmortyapi.com/api/character/?name=${nombreBusqueda.value}&status=${estadoSeleccionado.value}&page=${paginaActual.value}`);
    personajes.value = response.data.results;
    totalPaginas.value = response.data.info.pages;
  } catch (error) {
    console.error('Error al cargar:', error);
    personajes.value = [];
    totalPaginas.value = 0;
  } finally {
    cargando.value = false;
  }
}

function handleBuscar(nombre) {
  nombreBusqueda.value = nombre; // Guardamos el nombre buscado
  paginaActual.value = 1; // Reiniciamos a la primera página
  obtenerPersonajes();
}

function handleFiltrar(estado) {
  estadoSeleccionado.value = estado; // Guardamos el estado seleccionado
  paginaActual.value = 1; // Reiniciamos a la primera página
  obtenerPersonajes();
}

function cambiarPagina(nuevaPagina) {
  if (nuevaPagina >= 1 && nuevaPagina <= totalPaginas.value) {
    paginaActual.value = nuevaPagina;
    obtenerPersonajes();
  }
}

function verDetalle(personaje) {
  personajeSeleccionado.value = personaje;
}

function volverAlListado() {
  personajeSeleccionado.value = null;
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