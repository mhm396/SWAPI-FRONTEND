<template>
  <div v-if="pilotDetails" class="p-6 bg-gray-100 rounded-lg shadow-md">
    <!-- Contenedor de datos del piloto -->
    <div class="mb-6 bg-white p-4 rounded-lg shadow-md">
      <h1 class="text-3xl font-bold mb-4">{{ pilotDetails.name }}</h1>
    </div>

    <!-- Tabla de naves asignadas -->
    <div class="bg-white p-4 rounded-lg shadow-md">
      <h2 class="text-xl font-semibold mb-4">Naves asignadas:</h2>
      <div v-if="starships.length > 0">
        <table class="table-auto w-full text-left border-collapse border border-gray-300 bg-opacity-75">
          <thead class="bg-gray-200">
            <tr>
              <th class="border border-gray-300 px-4 py-2">Nombre de la nave</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(starship, index) in starships" :key="index" class="hover:bg-gray-100">
              <td class="border border-gray-300 px-4 py-2">{{ starship }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else>
        <p class="text-gray-500">No hay naves asignadas a este piloto.</p>
      </div>
    </div>

    <!-- Enlace para volver a la lista de pilotos -->
    <div class="mt-6">
      <NuxtLink to="/pilots" class="text-blue-500 hover:underline">Volver a la lista de pilotos</NuxtLink>
    </div>
  </div>
</template>

<!--------------------  PROGRAMACION  ------------------->
<script setup>
  import { ref, onMounted } from 'vue';
  import { useRoute } from 'vue-router';

  // Obtener el ID del piloto desde la URL
  const route = useRoute();
  const pilot_id = route.params.pilot_id;

  // Crear variables reactivas para almacenar los detalles del piloto y las naves
  const pilotDetails = ref(null);
  const starships = ref([]);  // Aquí almacenaremos las naves asociadas al piloto

  // Oobtener los detalles del piloto y las naves asociadas
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
