<template>
  <div class="field">
    <q-input
      v-model="termoBusca"
      type="text"
      placeholder="Digite sua busca..."
      outlined
      dense
      class="field-search"
    >
      <template v-slot:append>
        <q-icon name="search" />
      </template>
    </q-input>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const emit = defineEmits<{
  (e: 'buscar', value: string): void
}>()

const termoBusca = ref('')

// Transmite o valor para o layout sempre que o usuário digitar
watch(termoBusca, (novoValor) => {
  emit('buscar', novoValor)
})
</script>

<style scoped>
.field-search {
  background: white;
  border-radius: 4px;
  width: 220px;
}
</style>
