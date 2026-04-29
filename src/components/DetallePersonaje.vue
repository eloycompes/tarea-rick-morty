<template>
  <div class="detalle">
    <button @click="$emit('volver')">Volver al listado</button>
    
    <div v-if="personaje">
      <img :src="personaje.image" :alt="personaje.name" />
      <h2>{{ personaje.name }}</h2>
      <p><strong>Estado:</strong> {{ personaje.status }}</p>
      <p><strong>Especie:</strong> {{ personaje.species }}</p>
      <p><strong>Género:</strong> {{ personaje.gender }}</p>
      <p><strong>Origen:</strong> {{ personaje.origin.name }}</p>
    </div>
  </div>
</template>

<script setup>
// Cogemos el personaje que el padre nos pase
defineProps({
  personaje: Object
});
</script>

<style scoped>
.detalle {
  text-align: center;
  padding: 20px;
}
img {
  border-radius: 10px;
  max-width: 300px;
}
</style>