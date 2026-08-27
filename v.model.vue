<script setup>
import { ref, onMounted } from 'vue'
import api from './services/api'

const produtos = ref([])

const novoProduto = ref({
  nome: '',
  categoria: '',
  preco: 0,
  disponivel: true
})

async function buscarProdutos() {
  const resposta = await api.get('/produtos')
  produtos.value = resposta.data
}

async function criarProduto() {
  try {
    await api.post('/produtos', novoProduto.value)

    // Limpa o formulário depois de guardar
    novoProduto.value = { nome: '', categoria: '', preco: 0, disponivel: true }

    // Atualiza a lista com o novo produto
    await buscarProdutos()
  } catch (erro) {
    console.error('Erro ao criar produto:', erro)
  }
}

onMounted(buscarProdutos)
</script>

<template>
  <h1>Cardápio da Cafetaria ☕</h1>

  <form @submit.prevent="criarProduto">
    <input v-model="novoProduto.nome" placeholder="Nome do produto" required />
    <input v-model="novoProduto.categoria" placeholder="Categoria" required />
    <input v-model.number="novoProduto.preco" type="number" step="0.01" placeholder="Preço" required />
    <label>
      <input v-model="novoProduto.disponivel" type="checkbox" /> Disponível
    </label>
    <button type="submit">Cadastrar Produto</button>
  </form>

  <ul>
    <li v-for="produto in produtos" :key="produto.id">
      {{ produto.nome }} — R$ {{ produto.preco.toFixed(2) }}
    </li>
  </ul>
</template>