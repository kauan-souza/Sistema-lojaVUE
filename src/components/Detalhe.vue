<template>
  <section class="detalhe">
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <h2>Detalhe do Produto</h2>
          <img :src="produtos.img" alt />
        </div>
        <div class="col-md-9">
          <h2>{{ produtos.title }}</h2>
          <p>{{ produtos.price }}</p>
        </div>
        <div class="col-md-3">
          <div class="detalhe__box-price">
            <p>Quantidade</p>
            <input @input="toCalculate" type="number" v-model="quantity" />
          </div>
        </div>
        <div class="col-md-12">
          <hr />
          <h3>Descrição:</h3>
          <p>
            Lorem Ipsum is simply dummy text of the printing and typesetting
            industry. Lorem Ipsum has been the industry's standard dummy text
            ever since the 1500s, when an unknown printer took a galley of type
            and scrambled it to make a type specimen book. It has survived not
            only five centuries, but also the leap into electronic typesetting,
            remaining essentially unchanged. Lorem Ipsum is simply dummy text of
            the printing and typesetting industry. Lorem Ipsum has been the
            industry's standard dummy text ever since the 1500s, when an unknown
            printer took a galley of type and scrambled it to make a type
            specimen book. It has survived not only five centuries, but also the
            leap into electronic typesetting, remaining essentially unchanged
          </p>
          <h4>
            Total : {{ finalQuantity }} =
            {{ total.toString().replace(".", ",") }}
          </h4>

          <div class="col-md-12">
            <button class="pedidobutton" @click="open()">Fazer Pedido</button>
            <p v-if="flag2">Pedido cadastrado com sucesso</p>
            <div v-if="flag1">
              <section class="NovoPedido">
                <div>
                  <hr />
                  <h3>Novo pedido:</h3>

                  <b-input-group>
                    <label for="input1"> CPF Cliente:  </label>
                    <b-form-input
                      id="input1"
                      v-model="cpfCliente"
                    ></b-form-input>

                    <b-button @click="getUserByCpf">Buscar</b-button>
                  </b-input-group>

                  <p>
                    Nome Completo: {{ clientes.nome }} {{ clientes.sobrenome }}
                  </p>
                  <p>Cpf: {{ clientes.CPF }}</p>
                  <p>Data de nascimento: {{ clientes.dataNascimento }}</p>

                  <hr />
                  <button  @click="create()">
                    Salvar Pedido
                  </button>
                </div>
              </section>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  name: "Detalhe",
  data: function () {
    return {
      quantity: 1,
      finalQuantity: 1,
      preco: 0,
      total: 0,
      produtos: [],
      clientes: [],
      flag1: false,
      flag2: false,
    };
  },
  methods: {
    getUserById: async function () {
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
        this.flag1 = !this.flag1;
        this.flag2 = !this.flag2;
      }
    },

    getUserByCpf: async function () {
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
    toCalculate: function () {
      this.finalQuantity = this.quantity;

      if (this.quantity === "") {
        this.finalQuantity = 1;
      }
      this.preco = parseInt(this.produtos.price);
      const total = this.preco * this.finalQuantity;
      this.total = total.toFixed(2);
    },
    open: function () {
      this.flag1 = !this.flag1;
    },
  },
  created: function () {
    this.getUserById();
  },
  updated: function () {
    this.toCalculate();
  },
};
</script>
<style>
.detalhe {
  padding: 50px 0px;
}

.detalhe img {
  margin: 20px auto;
  display: block;
}

.detalhe__box-price {
  text-align: right;
  font-weight: bold;
}

.detalhe input {
  width: 50px;
  height: 30px;
  border-radius: 4px;
  padding: 0px 8px;
}

.detalhe button {
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