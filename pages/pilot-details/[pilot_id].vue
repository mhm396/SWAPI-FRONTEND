<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

// Obtener el ID del piloto desde la URL
const route = useRoute();
const pilot_id = route.params.pilot_id;  // Este es el 'id' de la URL

// Crear variables reactivas para almacenar los detalles del piloto y las naves
const pilotDetails = ref(null);
const starships = ref([]);  // Aquí almacenaremos las naves asociadas al piloto

// Función para obtener los detalles del piloto y las naves asociadas
const fetchPilotDetails = async () => {
  try {
    const response = await $fetch(`http://127.0.0.1:8000/api/swapi/pilots/${pilot_id}/starships`);
    pilotDetails.value = {
      name: response.pilot_name,  // Nombre del piloto
    };
    starships.value = response.starships;  // Lista de naves asociadas
  } catch (error) {
    console.error("Error al obtener los detalles del piloto:", error);
  }
};

// Obtener los detalles del piloto cuando se monta el componente
onMounted(() => {
  fetchPilotDetails();
});
</script>

<template>
  <div v-if="pilotDetails">
    <h1 class="text-2xl font-bold">{{ pilotDetails.name }}</h1>
    
    <!-- Mostrar las naves asignadas a este piloto -->
    <div v-if="starships.length > 0">
      <h2 class="text-xl font-semibold mt-4">Naves asignadas:</h2>
      <ul>
        <li v-for="(starship, index) in starships" :key="index">
          <p><strong>{{ starship }}</strong></p>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No hay naves asignadas a este piloto.</p>
    </div>

    <!-- Enlace para volver a la lista de pilotos -->
    <div class="mt-4">
      <NuxtLink to="/pilots">Volver a la lista de pilotos</NuxtLink>
    </div>
  </div>
</template>

<style scoped>
/* Estilos opcionales para la página de detalles */
</style>
