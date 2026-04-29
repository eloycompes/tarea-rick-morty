<template>
  <div class="tarjeta">
    <img :src="personaje.image" :alt="personaje.name" />
    <h3>{{ personaje.name }}</h3>
    <p>Estado: {{ personaje.status }}</p>
  </div>
</template>

<script setup>
defineProps({
  personaje: Object
});
</script>

<style scoped>
.tarjeta {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px;
  border-radius: 8px;
  text-align: center;
}
img {
  width: 100px;
  border-radius: 50%;
}
</style>