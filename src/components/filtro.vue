<template>
  <div class="filtros">
    <select v-model="estadoSeleccionado" @change="emitirFiltro">
      <option value="">Todos los estados</option>
      <option value="alive">Vivo</option>
      <option value="dead">Muerto</option>
      <option value="unknown">Desconocido</option>
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