<template>
<div class="pedidos">
       <Header/>
       <main>
       <div v-for="p in pedidos" :key="p._id">
    <PedidosProdutos
      :produtoId="p.produtoId"
      :clienteCpf="p.clienteCpf"
      :valorUnitario="p.valorUnitario"
      :valorTotal="p.valorTotal"
      :quantidade="p.quantidade"
    />
       </div>
  </main>
</div>
</template>

<script>
import PedidosProdutos from "../components/PedidosProdutos.vue";
import Header from "../components/Header.vue";
export default {
  Name: "Pedidos",
  components: { 
      PedidosProdutos,
      Header
      },

  data: function () {
    return {
      pedidos: [],
    };
  },
  methods: {
    getUserByCpf: async function () {
      const result = await fetch("http://localhost:3000/pedidos", {
        method: "GET",
      })
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        this.pedidos = result;
      }
    },
  },
    created: function () {
      this.getUserByCpf();
    },
};
</script>

<style>
</style>