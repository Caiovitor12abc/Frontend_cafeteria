<script setup>
defineProps(['produto'])
defineEmits(['editar', 'remover'])
</script>

<template>
  <div class="produto-card" :class="{ indisponivel: !produto.disponivel }">
    <div>
      <strong>{{ produto.nome }}</strong>
      <small>{{ produto.categoria }}</small>
    </div>
    <div>R$ {{ produto.preco.toFixed(2) }}</div>
    <div>
      <button @click="$emit('editar', produto)">✏️</button>
      <button @click="$emit('remover', produto)">🗑️</button>
    </div>
  </div>
</template>

<style scoped>
.produto-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  border: 1px solid #e0d7d5;
  border-radius: 10px;
  padding: 14px 18px;
  margin-bottom: 10px;
}
.indisponivel { opacity: 0.5; }
</style>