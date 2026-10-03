<template>
  <main>
    <section class="q-pa-md">
      <ListaProdutos :filtro="filtro" />
    </section>
  </main>
</template>

<script setup lang="ts">
import ListaProdutos from '../../components/ListaProdutos.vue'

 defineProps<{
  filtro?: string | undefined
}>()
</script>
