<template>
  <main>
    <h1>Proyecto Rick & Morty</h1>
    <ListaPersonajes />
  </main>
</template>

<script setup>
import ListaPersonajes from './components/ListaPersonajes.vue';
</script>

<style scoped>
main {
  text-align: center;
  font-family: sans-serif;
  padding: 20px;
}
</style>