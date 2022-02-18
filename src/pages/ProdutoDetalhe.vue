<template>
  <div class="produto-detalhe">
    <Header />
    <main>
      <Detalhe />
    </main>
    <hr />
    <Footer />
  </div>
</template>
<script>
import Header from "../components/Header.vue";
import Detalhe from "../components/Detalhe.vue";
import Footer from "../components/Footer.vue";

export default {
  name: "ProdutoDetalhe",
  components: {
    Header,
    Detalhe,
    Footer,
  },
  data() {
    return {
      produtos: [],
      clientes: [],
      cpfCliente: "",
      quantity : 1,
      finalQuantity : 1,
      preco : 0,
      total : 0,
    };
  },

  methods: {
    create: async function () {
      const newPedido = {
        produtoId: this.produtos._id,
        clienteCpf: this.clientes.CPF,
        valorTotal: this.total,
        valorUnitario: this.produtos.price,
        quantidade: this.finalQuantity,
      };
      const result = await fetch("http://localhost:3000/pedidos", {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(newPedido),
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
        this.newPedido = result;
      }
    },
    getProdutos: async function () {
      const id = this.$route.params.id;
      const result = await fetch("http://localhost:3000/produtos/" + id, {
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
      console.log(result);

      if (!result.error) {
        this.produtos = result;
      }
    },
     toCalculate: function () {
      this.finalQuantity = this.quantity;

      if (this.quantity === "") {
        this.finalQuantity = 1;
      }
      this.preco = parseInt(this.produtos.price)
      const total = this.preco * this.finalQuantity;
      this.total = total.toFixed(2);
    },
    getUserById: async function () {
      const result = await fetch(
        "http://localhost:3000/clientes/busca/" + this.cpfCliente,
        {
          method: "GET",
        }
      )
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        this.clientes = result;
      }
    },
  },
  created: function () {
    this.getUserById();
  },
};
</script>
<style>
.button {
  width: 170px;
  height: 40px;
  border-radius: 6px;
  background-color: #ae382b;
  color: #f5a022;
  border: none;
  font-weight: bold;
  display: block;
  margin: 30px auto;
}
</style>