<template>
  <div>
    <h2>Lista de produtos</h2>

    <ul class="cards">
      <li v-for="item in produtosFiltrados" :key="item.id" class="card">
        <img :src="item.imagem" :alt="item.titulo"/>
       <div class="card-body">
         <p><strong>{{ item.titulo }}</strong></p>
        <p>{{ item.categoria }}</p>
       </div>
       <q-btn icon="shopping_cart" label="adicionar ao carrinho" color="green"/>
      </li>
      <li v-if="produtosFiltrados.length === 0">
        <p>Nenhum resultado encontrado.</p>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import computador from '@/assets/imagem1.jpg'
import tv from '@/assets/imagem2.jpg'
import fone from '@/assets/imagem3.jpg'

interface Dados {
  id: number
  titulo: string
  categoria: string
  imagem:string
}


const props = defineProps<{
  filtro?: string | undefined
}>()

const produtos = ref<Dados[]>([
  {
    id: 1,
    titulo: 'computador',
    imagem:computador,
    categoria: 'ti'
  },
  {
    id: 2,
    titulo: 'bola',
    imagem:computador,
    categoria: 'brinquedo'
  },
  {
    id: 3,
    titulo: 'arroz',
    imagem:computador,
    categoria: 'comida'
  }
])

// Filtra dinamicamente com base no termo digitado
const produtosFiltrados = computed(() => {
  if (!props.filtro) return produtos.value

  const termo = props.filtro.toLowerCase()
  return produtos.value.filter(
    item =>
      item.titulo.toLowerCase().includes(termo) ||
      item.categoria.toLowerCase().includes(termo)
  )
})
</script>


<style scoped>
h2{
  padding:0;
  margin:0;
  font-size:1.7rem;
}

.cards{
  display:flex;
  flex-direction:column;
  gap:1rem;
}

@media (min-width:568px){
  .cards{
    display:grid;
    grid-template-columns: repeat(3, 1fr);
    gap:18px;
  }
}

.card{
  box-shadow: 0 0 5px #00000050;
  border-radius: 5px;
  border:1px solid #ccc;
  height:100%;
  display:flex;
  flex-direction:column;
}

.card-body{
  padding:5px;
}

.card p{
  margin:0;
  padding:0;
}


.card img{
  height: 380px;
  width:100%;
  object-fit:cover;
}
</style>
