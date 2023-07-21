<script setup>
defineProps(["alerta"]);
</script>

<template>
  <div
    :class="[alerta.tipo ? 'bg-red-600' : 'bg-blue-600']"
    class="uppercase text-center p-3 mb-3 rounded-md"
  >
    <p class="text-xl uppercase text-white">{{ alerta.mensaje }}</p>
  </div>
</template>
