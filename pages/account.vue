<script>
    definePageMeta({
        layout: 'authentication'
    })
</script>

<template>
    <div>
        <button class="bg-blue-900 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            <NuxtLink to="/">Home back</NuxtLink>
        </button>
        <h1>My account</h1>
        <div class="col-6">
            <div class="px-6 pt-4 pb-2">
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                <NuxtLink to="/account/">Login</NuxtLink>
            </button>
            </div>
            <div class="px-6 pt-4 pb-2">
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                <NuxtLink to="/account/register">Register</NuxtLink>
            </button>
            </div>
        </div>
        <div class="col-6">
            <NuxtPage />
        </div>
        
    </div>
</template>