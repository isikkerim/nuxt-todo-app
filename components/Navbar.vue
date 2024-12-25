<template>
    <nav class="bg-gray-800 p-4">
      <div class="container mx-auto flex justify-between items-center">
        <NuxtLink to="/" class="text-white font-bold text-xl">Todo App</NuxtLink>
        <ul class="flex space-x-4">
          <li><NuxtLink to="/about" class="text-gray-300 hover:text-white transition">Hakkında</NuxtLink></li>
          <li><NuxtLink to="/contact" class="text-gray-300 hover:text-white transition">İletişim</NuxtLink></li>
        </ul>
      </div>
    </nav>
  </template>