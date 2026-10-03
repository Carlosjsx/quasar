<template>
  <div>
    <h2>Lista de dados</h2>

    <ul>
      <li v-for="item in filmes" :key="item.id">
        <p>{{ item.titulo }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Dados {
  id: number;
  titulo: string;
  categoria: string;
}

const filmes = ref<Dados[]>([
  {
    id: 1,
    titulo: "a lista",
    categoria: "drama"
  },
  {
    id: 2,
    titulo: "o menino do ipê",
    categoria: "infantil"
  },
  {
    id: 3,
    titulo: "a telefone",
    categoria: "ação"
  }
]);
</script>