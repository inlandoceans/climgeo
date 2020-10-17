<template>
  <header class="flex items-center border-b border-solid border-blue-900 mx-5">
    <NuxtLink to="/"><Logo class="mb-4" /></NuxtLink>
    <NuxtLink to="/">
      <h1
        class="hidden sm:block px-5 uppercase sm:text-2xl md:text-4xl font-extrabold"
      >
        Climate Cascades
      </h1>
      <h1 class="sm:hidden px-5 uppercase text-4xl font-extrabold">CC</h1>
    </NuxtLink>
    <AppSearchInput class="ml-8 items-center" />
    <NuxtLink to="/regions">
      <p class="text-red-800 font-bold">Regions</p>
    </NuxtLink>
    <NuxtLink to="/about">
      <p class="text-red-800 font-bold">About</p>
    </NuxtLink>
  </header>
</template>
