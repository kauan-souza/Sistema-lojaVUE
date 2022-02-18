<template>
  <div>
    <p>Código: {{ produtoId }}</p>
    <p>CPF Cliente: {{ clienteCpf }}</p>
    <p>Valor Unitário: {{ valorUnitario }}</p>
    <p>Valor Total: {{ valorTotal }}</p>
    <p>Quantidade: {{ quantidade }}</p>
    <hr>
  </div>
</template>

<script>
export default {
  Name: "PedidosProdutos",
    props:{
        produtoId:String,
        clienteCpf:String,
        valorUnitario:String,
        valorTotal:String,
        quantidade:String,
    }
};
</script>

<style>
</style>
