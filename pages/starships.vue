<script setup>
import { ref } from 'vue';
import { useAsyncData } from '#app';
import Card from '~/components/Card.vue';

// Obtener los datos de las naves usando useAsyncData
const { data: starships, pending, error } = await useAsyncData('starships', () =>
  $fetch('http://127.0.0.1:8000/api/swapi')
);

// Esto maneja los errores de la solicitud de manera simple
if (error.value) {
  console.error('Error al obtener las naves:', error.value);
}

</script>

<template>
  <div>
    <h1>Pagina de Naves</h1>

    <!-- Mostrar mensaje de carga mientras esperamos los datos -->
    <div v-if="pending" class="text-center py-4">
      <p>Cargando naves...</p>
    </div>

    <!-- Mostrar las naves cuando los datos están listos -->
    <div v-else class="grid">
      <!-- Accedemos al campo 'name' de cada nave -->
      <Card v-for="(starship, index) in starships" :key="index" :title="starship.name" :id="starship.starship_id" :type="'starship'"/>
    </div>

    <!-- Mostrar error en caso de que no haya datos -->
    <div v-if="error" class="text-center py-4">
      <p>Hubo un error al cargar las naves. Intenta de nuevo más tarde.</p>
    </div>
  </div>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(18rem, 1fr));
  gap: 2rem;
}
</style>
