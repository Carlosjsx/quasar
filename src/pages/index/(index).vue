<template>
  <main>
    <section class="q-pa-md">
      <ListaDados :filtro="filtro" />
    </section>
  </main>
</template>

<script setup lang="ts">
import ListaDados from '../../components/ListaDados.vue'

// Declara explicitamente 'string | undefined' para compatibilidade total com o TypeScript estrito
defineProps<{
  filtro?: string | undefined
}>()
</script>
