<script setup>
import { ref } from 'vue';
import { useAsyncData } from '#app';
import Card from '~/components/Card.vue';

// Obtener los datos de los pilotos
const { data: pilots, pending, error } = await useAsyncData('pilots', () =>
  $fetch('http://127.0.0.1:8000/api/swapi/allpilots')
);

if (error.value) {
  console.error('Error al obtener los pilotos:', error.value);
}
</script>

<template>
  <div>
    <!-- Mostrar los pilotos cuando los datos están listos -->
    <div class="grid">
      <Card v-for="(pilot, index) in pilots" :key="index" :title="pilot.name" :id="pilot.pilot_id" :type="'pilot'" />
    </div>

    <div v-if="error" class="text-center py-4">
      <p>Hubo un error al cargar los pilotos. Intenta de nuevo más tarde.</p>
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
