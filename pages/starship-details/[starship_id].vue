<template>
  <div v-if="starshipDetails" class="p-6 bg-gray-100 rounded-lg shadow-md">
   
    <!-- Contenedor de datos de la nave -->
    <div class="mb-6 bg-white p-4 rounded-lg shadow-md">
      <h1 class="text-3xl font-bold mb-4">{{ starshipDetails.name }}</h1>
      <div class="grid grid-cols-2 gap-4">
        <p><strong>Fabricante:</strong> {{ starshipDetails.manufacturer }}</p>
        <p><strong>Costo en créditos (original):</strong> {{ starshipDetails.cost_in_credits_original }}</p>
        <p><strong>Costo en créditos (base 15):</strong> {{ starshipDetails.cost_in_credits_base15 }}</p>
      </div>
    </div>

    <!-- Sección para agregar un piloto -->
    <div class="mb-6 bg-white p-4 rounded-lg shadow-md flex items-center justify-between">
      <h2 class="text-xl font-semibold">Agregar Piloto:</h2>
      <select @change="addPilot($event.target.value)" class="border p-2 rounded w-1/3">
        <option value="">Selecciona un piloto</option>
        <option v-for="pilot in availablePilots" :key="pilot.pilot_id" :value="pilot.pilot_id">
          {{ pilot.name }}
        </option>
      </select>
    </div>

    <!-- Tabla de pilotos asignados -->
    <div class="bg-white p-4 rounded-lg shadow-md">
      <h2 class="text-xl font-semibold mb-4">Pilotos asignados:</h2>
      <div v-if="pilots.length > 0">
        <table class="table-auto w-full text-left border-collapse border border-gray-300 bg-opacity-75">
          <thead class="bg-gray-200">
            <tr>
              <th class="border border-gray-300 px-4 py-2">Nombre</th>
              <th class="border border-gray-300 px-4 py-2">Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(pilot) in pilots" :key="pilot.pilot_id" class="hover:bg-gray-100">
              <td class="border border-gray-300 px-4 py-2">{{ pilot.name }}</td>
              <td class="border border-gray-300 px-4 py-2">
                <button
                  @click="removePilot(pilot.pilot_id)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-1 px-3 rounded">
                  Eliminar
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else>
        <p class="text-gray-500">No hay pilotos asignados a esta nave.</p>
      </div>
    </div>

    <!-- Enlace para volver a la lista de naves -->
    <div class="mt-6">
      <NuxtLink to="/starships" class="text-blue-500 hover:underline">Volver a la lista de naves</NuxtLink>
    </div>
  </div>
</template>


<!--------------------  PROGRAMACION  ------------------->
<script setup>
  import { ref, onMounted } from 'vue';
  import { useRoute } from 'vue-router';

  // Obtener el ID de la nave desde la URL
  const route = useRoute();
  const starship_id = route.params.starship_id;
  const availablePilots = ref([]); // Para almacenar la lista de pilotos disponibles para agregar

  // Para almacenar los detalles de la nave y los pilotos
  const starshipDetails = ref(null);
  const pilots = ref([]); // Aquí almacenaremos los pilotos asociados a la nave

  //Convertir un número a base 15 (símoblos personalizados)
  const convertToBase15 = (number) => {
    const symbols = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'ß', 'Þ', '¢', 'μ', '¶'];
    let result = '';
    let num = parseInt(number, 10);

    if (isNaN(num)) return '0';

    while (num > 0) {
      result = symbols[num % 15] + result;
      num = Math.floor(num / 15);
    }

    return result || '0';
  };

  // Obtener los detalles de la nave y los pilotos
  const fetchStarshipDetails = async () => {
    try {
      const response = await $fetch(`http://127.0.0.1:8000/api/swapi/starships/${starship_id}/pilots`);
      starshipDetails.value = {
        name: response.starship_name,
        manufacturer: response.manufacturer,
        cost_in_credits_original: response.cost_in_credits,
        cost_in_credits_base15: convertToBase15(response.cost_in_credits),
      };
      pilots.value = response.pilots;
    } catch (error) {
      console.error('Error al obtener los detalles de la nave:', error);
    }
  };

  // Obtener la lista de pilotos disponibles para agregar
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

      // Actualizamos la lista de pilotos si todo fue bien
      if (response.status === 200 || response.success) {
        pilots.value.push({ pilot_id: pilotId, name: response.pilot_name });
      } else {
        alert(response.message || 'No se pudo agregar el piloto.');
      }
    } catch (error) {
      console.error('Error al agregar el piloto:', error);
    }
  };

  // Eliminar un piloto de la nave
  const removePilot = async (pilotId) => {
    try {
      // Solicitud DELETE al backend
      const response = await $fetch(`http://127.0.0.1:8000/api/swapi/starships/${starship_id}/remove-pilot/${pilotId}`, {
        method: 'DELETE',
      });

      // Actualizamos la lista de pilotos si todo ha salido bien
      if (response.status === 200 || response.success) {
        // Filtramos los pilotos eliminados de la lista
        pilots.value = pilots.value.filter((pilot) => pilot.pilot_id !== pilotId);
      } else {
        alert(response.message || 'No se pudo eliminar el piloto.'); // Mostrar el mensaje de error
      }
    } catch (error) {
      console.error('Error al eliminar el piloto:', error);
    }
  };

  // Obtener los detalles cuando se monta el componente
  onMounted(() => {
    fetchStarshipDetails();
    fetchAvailablePilots();
  });
</script>