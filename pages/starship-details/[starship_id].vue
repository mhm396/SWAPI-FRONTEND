<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

// Obtener el ID de la nave desde la URL
const route = useRoute();
const starship_id = route.params.starship_id; // Este es el 'id' de la URL
const availablePilots = ref([]); // Para almacenar la lista de pilotos disponibles para agregar

// Crear variables reactivas para almacenar los detalles de la nave y los pilotos
const starshipDetails = ref(null);
const pilots = ref([]); // Aquí almacenaremos los pilotos asociados a la nave

// Función para obtener los detalles de la nave y los pilotos
const fetchStarshipDetails = async () => {
  try {
    const response = await $fetch(`http://127.0.0.1:8000/api/swapi/starships/${starship_id}/pilots`);
    starshipDetails.value = {
      name: response.starship_name, // Nombre de la nave
    };
    pilots.value = response.pilots; // Lista de pilotos
  } catch (error) {
    console.error('Error al obtener los detalles de la nave:', error);
  }
};

// Función para obtener la lista de pilotos disponibles para agregar
const fetchAvailablePilots = async () => {
  try {
    const response = await $fetch(`http://127.0.0.1:8000/api/swapi/allpilots`);
    availablePilots.value = response; // Lista de pilotos disponibles
  } catch (error) {
    console.error('Error al obtener los pilotos disponibles:', error);
  }
};

// Función para agregar un piloto a la nave
const addPilot = async (pilotId) => {
  try {
    const response = await $fetch(`http://127.0.0.1:8000/api/swapi/starships/${starship_id}/add-pilot/${pilotId}`, {
      method: 'POST',
    });

    // Si la adición fue exitosa, actualizamos la lista de pilotos
    if (response.status === 200 || response.success) {
      pilots.value.push({ pilot_id: pilotId, name: response.pilot_name }); // Añadir piloto a la nave
    } else {
      alert(response.message || 'No se pudo agregar el piloto.'); // Mostrar mensaje de error
    }
  } catch (error) {
    console.error('Error al agregar el piloto:', error);
  }
};

// Función para eliminar un piloto de la nave
const removePilot = async (pilotId) => {
  try {
    // Realizamos la solicitud DELETE al backend
    const response = await $fetch(`http://127.0.0.1:8000/api/swapi/starships/${starship_id}/remove-pilot/${pilotId}`, {
      method: 'DELETE',
    });

    // Si la eliminación fue exitosa, actualizamos la lista de pilotos
    if (response.status === 200 || response.success) {
      // Filtramos los pilotos eliminados de la lista en Vue reactiva
      pilots.value = pilots.value.filter((pilot) => pilot.pilot_id !== pilotId);
    } else {
      alert(response.message || 'No se pudo eliminar el piloto.'); // Mostrar el mensaje de error
    }
  } catch (error) {
    console.error('Error al eliminar el piloto:', error);
  }
};

// Obtener los detalles de la nave cuando se monta el componente
onMounted(() => {
  fetchStarshipDetails();
  fetchAvailablePilots(); // Obtener los pilotos disponibles
});
</script>

<template>
  <div v-if="starshipDetails">
    <h1 class="text-2xl font-bold">{{ starshipDetails.name }}</h1>

     <!-- Botón para agregar un piloto -->
    <div class="mt-4">
      <h2 class="text-xl font-semibold">Agregar Piloto:</h2>
      <select @change="addPilot($event.target.value)" class="border p-2 rounded">
        <option value="">Selecciona un piloto</option>
        <option v-for="pilot in availablePilots" :key="pilot.pilot_id" :value="pilot.pilot_id">
          {{ pilot.name }}
        </option>
      </select>
    </div>

    <!-- Mostrar los pilotos asignados a esta nave -->
    <div v-if="pilots.length > 0">
      <h2 class="text-xl font-semibold mt-4">Pilotos asignados:</h2>
      <ul>
        <li v-for="(pilot) in pilots" :key="pilot.pilot_id" class="flex justify-between items-center">
          <p><strong>{{ pilot.name }}</strong></p>

          <!-- Botón para eliminar al piloto -->
          <button
            @click="removePilot(pilot.pilot_id)"
            class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
          >
            Eliminar
          </button>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No hay pilotos asignados a esta nave.</p>
    </div>

    <!-- Enlace para volver a la lista de naves -->
    <div class="mt-4">
      <NuxtLink to="/starships">Volver a la lista de naves</NuxtLink>
    </div>
  </div>

  <!-- Mostrar mensaje de carga mientras obtenemos los datos -->
  <div v-else>
    <p>Cargando detalles de la nave...</p>
  </div>
</template>