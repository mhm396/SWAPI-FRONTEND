<template>
  <nav class="bg-gray-800 p-4 w-full top-0 left-0 z-50">
    <ul class="flex justify-evenly  w-full">
      <li class="text-white text-lg hover:text-purple-400">
        <NuxtLink to="/" active-class="active">Home</NuxtLink>
      </li>
      <li class="text-white text-lg hover:text-purple-400">
        <NuxtLink to="/starships" active-class="active">Naves</NuxtLink>
      </li>
      <li class="text-white text-lg hover:text-purple-400">
        <NuxtLink to="/pilots" active-class="active">Pilotos</NuxtLink>
      </li>
      <!--<li class="text-white text-lg hover:text-purple-400">
        <NuxtLink to="/account/" active-class="active">Login</NuxtLink>
      </li>-->
    </ul>
  </nav>
</template>

<style scoped>
.active{
  border-bottom: 2px solid purple;
}
</style>
