<script setup>
import { ref, onMounted } from 'vue'
import api from './services/api'

const produtos = ref([])
const carregando = ref(true)

async function buscarProdutos() {
  carregando.value = true
  try {
    const resposta = await api.get('/produtos')
    produtos.value = resposta.data
  } catch (erro) {
    console.error('Erro ao buscar produtos:', erro)
  } finally {
    carregando.value = false
  }
}

onMounted(() => {
  buscarProdutos()
})
</script>

<template>
  <h1>Cardápio da Cafetaria ☕</h1>

  <p v-if="carregando">A carregar produtos...</p>

  <ul v-else>
    <li v-for="produto in produtos" :key="produto.id">
      {{ produto.nome }} — R$ {{ produto.preco.toFixed(2) }}
      <span v-if="!produto.disponivel"> (Indisponível)</span>
    </li>
  </ul>
</template>