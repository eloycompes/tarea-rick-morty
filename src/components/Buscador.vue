<template>
  <div class="buscador">
    <input 
      v-model="nombreBusqueda" 
      @keyup.enter="emitirBusqueda" 
      placeholder="Buscar personaje..." 
    />
    <button @click="emitirBusqueda">Buscar</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const nombreBusqueda = ref('');

// Eventos que este componente (Buscador.vue) puede "emitir" hacia arriba
const emit = defineEmits(['buscar']);

function emitirBusqueda() {
  // Se envia el valor escrito al componente padre (ListaPersonajes.vue)
  emit('buscar', nombreBusqueda.value);
}
</script>

<style scoped>
.buscador {
  margin-bottom: 20px;
}
input {
  padding: 8px;
  margin-right: 5px;
}
</style>