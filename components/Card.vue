<script setup>
defineProps({
  title: String,
  id: Number,  // El 'id' se pasa como prop
  type: String,  // Un tipo para diferenciar entre 'starship' y 'pilot'
})
</script>

<template>
  <div class="max-w-sm rounded overflow-hidden shadow-lg bg-white">
    <div class="px-6 py-4">
      <div class="font-bold text-xl mb-2">{{ title }}</div>
      <p class="text-gray-700 text-base">
        Detalles de tarjeta
      </p>
    </div>
    <div class="px-6 pt-4 pb-2">
      <!-- En función del tipo, cambiamos la ruta para el detalle -->
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
        <NuxtLink :to="`/${type}-details/${id}`">Ver detalles</NuxtLink>
      </button>
    </div>
  </div>
</template>
