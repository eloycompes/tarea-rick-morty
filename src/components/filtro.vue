<template>
  <div class="filtros">
    <p>Filtrar por estado:</p>
    <select v-model="estadoSeleccionado" @change="emitirFiltro">
      <option value="">-</option>
      <option value="alive">Alive</option>
      <option value="dead">Dead</option>
      <option value="unknown">Unknown</option>
    </select>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const estadoSeleccionado = ref('');
const emit = defineEmits(['filtrar']);

function emitirFiltro() {
    // Cuano el usuario elige una opción, se avisa al padre (ListaPersonajes.vue) con el estado seleccionado
  emit('filtrar', estadoSeleccionado.value);
}
</script>

<style scoped>
.filtros {
  margin-bottom: 20px;
}
select {
  padding: 8px;
}
</style>