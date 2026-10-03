<template>
  <div>
    <h2>Lista de dados</h2>

    <ul>
      <li v-for="item in filmesFiltrados" :key="item.id">
        <p><strong>{{ item.titulo }}</strong> - <em>{{ item.categoria }}</em></p>
      </li>
      <li v-if="filmesFiltrados.length === 0">
        <p>Nenhum resultado encontrado.</p>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Dados {
  id: number
  titulo: string
  categoria: string
}

// Aceita explicitamente 'string | undefined' para evitar conflito no TypeScript
const props = defineProps<{
  filtro?: string | undefined
}>()

const filmes = ref<Dados[]>([
  {
    id: 1,
    titulo: 'a lista',
    categoria: 'drama'
  },
  {
    id: 2,
    titulo: 'o menino do ipê',
    categoria: 'infantil'
  },
  {
    id: 3,
    titulo: 'a telefone',
    categoria: 'ação'
  }
])

// Filtra dinamicamente os filmes com base no termo digitado
const filmesFiltrados = computed(() => {
  if (!props.filtro) return filmes.value

  const termo = props.filtro.toLowerCase()
  return filmes.value.filter(
    item =>
      item.titulo.toLowerCase().includes(termo) ||
      item.categoria.toLowerCase().includes(termo)
  )
})
</script>
