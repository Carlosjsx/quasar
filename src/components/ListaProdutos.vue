<template>
  <div>
    <div class="header-vitrine">
      <h2>Lista de produtos</h2>
      <div class="badge-cart z-top">
        <q-btn icon="shopping_cart" @click="abrirCarrinho" color="primary" text-color="white" unelevated />

        <span class="">{{ totalItemsCart }}</span>
      </div>
    </div>

    <ul class="cards">
      <li v-for="item in produtosFiltrados" :key="item.id" class="card">
        <img :src="item.imagem" :alt="item.titulo" />
        <div class="card-body">
          <h4 class="titulo"
            ><strong>{{ item.titulo }}</strong></h4
          >
          <p class="preco"
            ><strong>{{ moedaBR(item.preco) }}</strong></p
          >
          <span class="tag">{{ item.categoria }}</span>
        </div>
        <q-btn
          icon="shopping_cart"
          label="adicionar ao carrinho"
          color="green"
          @click="addItemCart(item)"
        />
      </li>
      <li v-if="produtosFiltrados.length === 0">
        <p>Nenhum resultado encontrado.</p>
      </li>
    </ul>

    <hr />

    <div class="modal z-max" v-if="modal">
      <div class="modal-content">
        <div class="header-cart">
          <h2>Carrinho</h2>
          <q-btn @click="closeCart" icon="close" color="orange" />
        </div>
        <div class="header-cart">
          <span>Total: {{ moedaBR(total) }}</span>
          <q-btn
            @click="cleanCart"
            label="Limpar carrinho"
            color="warning"
            v-if="totalItemsCart > 0"
          />
        </div>
        <ul class="cards-cart">
          <div class="cart-less" v-if="totalItemsCart  < 1">
             <p>
            Carrinho vazio!
          </p>

            <q-btn color="green" label="Retornar a vitrine de produtos" @click="closeCart"/>
        </div>

          <li v-for="item in itensCarrinho" :key="item.id" class="card-cart">
            <img :src="item.imagem" class="icone-imagem" :alt="item.titulo" />
            <div>
              <p>{{ item.titulo }}</p>
              <p>{{ moedaBR(item.preco) }}</p>
            </div>
            <div class="btn-controls">
              <button @click="diminuirItem(item)">-</button>
              <span>{{ item.quant }}</span>
              <button @click="aumentarItem(item)">+</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
const modal = ref(false);
import computador from "@/assets/imagem1.jpg";
import fone from "@/assets/imagem2.jpg";
import tv from "@/assets/imagem3.jpg";

interface Dados {
  id: number;
  titulo: string;
  categoria: string;
  imagem: string;
  preco: number;
}

const props = defineProps<{
  filtro?: string | undefined;
}>();

const produtos = ref<Dados[]>([
  {
    id: 1,
    titulo: "computador",
    imagem: computador,
    preco: 3999,
    categoria: "ti"
  },
  {
    id: 2,
    titulo: "televisão 31pol",
    imagem: tv,
    preco: 1899,
    categoria: "casa"
  },
  {
    id: 3,
    titulo: "fone de ouvido",
    imagem: fone,
    preco: 3999,
    categoria: "multimidia"
  },
  {
    id: 4,
    titulo: "Televisor",
    imagem: tv,
    preco: 1200,
    categoria: "casa"
  },
  {
    id: 5,
    titulo: "fone de ouvido",
    imagem: fone,
    preco: 399,
    categoria: "informática"
  },
  {
    id: 6,
    titulo: "computador",
    imagem: computador,
    preco: 3999,
    categoria: "ti"
  }
]);

interface Carrinho extends Dados {
  quant: number;
}

const itensCarrinho = ref<Carrinho[]>([]);

const produtosFiltrados = computed(() => {
  if (!props.filtro) return produtos.value;

  const termo = props.filtro.toLowerCase();
  return produtos.value.filter(
    item =>
      item.titulo.toLowerCase().includes(termo) ||
      item.categoria.toLowerCase().includes(termo)
  );
});

function abrirCarrinho(): void {
  modal.value = true;
}

function closeCart(): void {
  modal.value = false;
}

function addItemCart(item: Dados): void {
  const itemNoCarrinho = itensCarrinho.value.find(i => i.id === item.id);

  if (itemNoCarrinho) {
    itemNoCarrinho.quant++;
  } else {
    itensCarrinho.value.push({
      ...item,
      quant: 1
    });
  }
}

function aumentarItem(item: any) {
  item.quant++;
}

function diminuirItem(item: any): void {
  if (item.quant > 1) {
    item.quant--;
  } else {
    const idx = itensCarrinho.value.findIndex(i => i.id === item.id);
    if (idx !== -1) itensCarrinho.value.splice(idx, 1);
  }
}

function cleanCart(): void {
  itensCarrinho.value = [];
}

function moedaBR(valor: number): string {
  return valor.toLocaleString("pt-BR", {
    style: "currency",
    currency: "BRL"
  });
}

const total = computed(() =>
  itensCarrinho.value.reduce((acc, item) => acc + item.preco * item.quant, 0)
);

const totalItemsCart = computed(() =>
  itensCarrinho.value.reduce((acc, item) => acc + item.quant, 0)
);
</script>

<style scoped>
h2 {
  padding: 0;
  margin: 0;
  font-size: 1.7rem;
}

.badge-cart {
  position: fixed;
  top:.8rem;
  right:15rem;
}

.badge-cart span {
  position: absolute;
  right: 2.2rem;
  bottom: 0.8rem;
  background-color: tomato;
  border-radius: 50%;
  color: white;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.header-vitrine {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.cards {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

@media (min-width: 568px) {
  .cards {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 18px;
  }
}

.card {
  box-shadow: 0 0 5px #00000050;
  border-radius: 5px;
  border: 1px solid #ccc;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.card-body {
  padding: 15px;
}

.titulo {
  font-size: 1.2rem;
  padding: 0;
  margin: 0;
  text-transform: capitalize;
}

.preco {
  color: #00000090;
  font-size: 1.8rem;
}

span.tag {
  background-color: rgb(216, 247, 121);
  width: fit-content;
  padding: 4px 1rem;
}

.the-carrinho{
  border:2px solid red;
}

.card p {
  margin: 0;
  padding: 0;
}

.card img {
  height: 250px;
  width: 100%;
  object-fit: cover;
}

.icone-imagem {
  width: 90px;
  height: 90px;
  object-fit: cover;
}

.modal {
  width: 100%;
  min-height: 100vh;
  background: #00000050;
  position: fixed;
  top: 0;
  left: 0;
  padding: 6rem 0 0;
}

.modal-content {
  background: whitesmoke;
  max-width: 350px;
  width: 100%;
  min-height: 100vh;
  position: fixed;
  right: 0;
  top: 3rem;
  z-index: 4000;
}

.header-cart {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.cards-cart {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 0.5rem;
  height: 300px;
  overflow-y: auto;
}

.cart-less {
  text-align: center;
  margin: 4rem 0 0 0;
  font-size: 2rem;
  font-weight: 800;
}

.card-cart {
  box-shadow: 0 0 5px #00000050;
  border-radius: 5px;
  border: 1px solid #ccc;
  height: 110px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.7rem;
  padding: 0 10px 0 0;
}

.btn-controls {
  display: flex;
  align-items: center;
  overflow: hidden;
  background-color: #ccc;
  border-radius: 20px;
}

.btn-controls button {
  border: 0;
  cursor: pointer;
  font-size: 1.5rem;
  padding: 2px 15px;
}

.btn-controls span {
  padding: 0 10px;
  width: 50px;
  display: block;
  text-align: center;
}
</style>
