<script setup>
import { ref, onMounted } from 'vue'
import api from './services/api'

const produtos = ref([])
const produtoEditandoId = ref(null)

const formulario = ref({
  nome: '', categoria: '', preco: 0, disponivel: true
})

async function buscarProdutos() {
  const resposta = await api.get('/produtos')
  produtos.value = resposta.data
}

function editarProduto(produto) {
  produtoEditandoId.value = produto.id
  // Copia os dados do produto para o formulário
  formulario.value = { ...produto }
}

async function salvarProduto() {
  if (produtoEditandoId.value) {
    // Modo edição: PUT
    await api.put(`/produtos/${produtoEditandoId.value}`, formulario.value)
  } else {
    // Modo criação: POST
    await api.post('/produtos', formulario.value)
  }

  cancelarEdicao()
  await buscarProdutos()
}

function cancelarEdicao() {
  produtoEditandoId.value = null
  formulario.value = { nome: '', categoria: '', preco: 0, disponivel: true }
}

onMounted(buscarProdutos)
</script>

<template>
  <h1>Cardápio da Cafetaria ☕</h1>

  <form @submit.prevent="salvarProduto">
    <input v-model="formulario.nome" placeholder="Nome" required />
    <input v-model="formulario.categoria" placeholder="Categoria" required />
    <input v-model.number="formulario.preco" type="number" step="0.01" required />
    <button type="submit">{{ produtoEditandoId ? 'Atualizar' : 'Cadastrar' }}</button>
    <button v-if="produtoEditandoId" type="button" @click="cancelarEdicao">Cancelar</button>
  </form>

  <ul>
    <li v-for="produto in produtos" :key="produto.id">
      {{ produto.nome }} — R$ {{ produto.preco.toFixed(2) }}
      <button @click="editarProduto(produto)">Editar</button>
    </li>
  </ul>
</template>