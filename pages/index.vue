<template>
    <div class="flex flex-col items-center justify-start min-h-screen bg-gray-100 pt-16">
      <h1 class="text-4xl font-bold mb-8 text-gray-800">Bienvenido a SWAPI</h1>
      <div class="flex space-x-4">
        <NuxtLink to="/starships" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg shadow-md transition duration-300">
          Ver Naves
        </NuxtLink>
        <NuxtLink to="/pilots" class="bg-green-500 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-lg shadow-md transition duration-300">
          Ver Pilotos
        </NuxtLink>
      </div>
      <p class="text-lg text-gray-600 mt-16 typing-animation absolute bottom-80">Hecho por Miguel Ángel Hidalgo Manzano</p>
    </div>
  </template>
  
  <style scoped>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
  }
  
  /* Animación de máquina de escribir */
  .typing-animation {
    overflow: hidden;
    white-space: nowrap;
    border-right: 2px solid #4a5568; /* Color del cursor */
    animation: typing 6s steps(45, end), blink 0.7s step-end infinite;
  }
  
  @keyframes typing {
    from {
      width: 0;
    }
    to {
      width: 35ch; /* Ajusta este valor según la longitud del texto */
    }
  }
  
  @keyframes blink {
    50% {
      border-color: transparent;
    }
    100% {
      border-color: #4a5568; /* Color del cursor */
    }
  }
  </style>