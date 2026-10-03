<template>
  <main>
    <HeroPage />

    <section class="q-pa-md">
      <ListaProdutos :filtro="filtro" />
    </section>
  </main>
</template>

<script setup lang="ts">
import ListaProdutos from "../../components/ListaProdutos.vue";
import HeroPage from "../../components/HeroPage.vue";

defineProps<{
  filtro?: string | undefined;
}>();
</script>
