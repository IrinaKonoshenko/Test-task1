<script setup lang="ts"></script>

<template>
  <main class="flex flex-col gap-7 justify-center items-center">
    <h1 class="text-3xl font-bold text-gray-700">Добро пожаловать!</h1>
    <nav class="flex gap-5 justify-center items-center">
      <RouterLink
        class="text-lg font-medium text-gray-700 hover:underline hover:decoration-solid"
        to="/signup"
        >Зарегистрироваться</RouterLink
      >
      <RouterLink
        class="text-lg font-medium text-gray-700 hover:underline hover:decoration-solid"
        to="/signin"
        >Войти</RouterLink
      >
    </nav>
  </main>
</template>
