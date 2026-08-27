<script setup>
import { ref, onMounted } from 'vue'
import api from './services/api'

const produtos = ref([])

async function buscarProdutos() {
  const resposta = await api.get('/produtos')
  produtos.value = resposta.data
}

async function removerProduto(produto) {
  const confirmar = window.confirm(`Remover "${produto.nome}" do cardápio?`)
  if (!confirmar) return

  try {
    await api.delete(`/produtos/${produto.id}`)
    await buscarProdutos()
  } catch (erro) {
    console.error('Erro ao remover produto:', erro)
  }
}

onMounted(buscarProdutos)
</script>

<template>
  <h1>Cardápio da Cafetaria ☕</h1>

  <ul>
    <li v-for="produto in produtos" :key="produto.id">
      {{ produto.nome }} — R$ {{ produto.preco.toFixed(2) }}
      <button @click="removerProduto(produto)">Remover</button>
    </li>
  </ul>
</template>