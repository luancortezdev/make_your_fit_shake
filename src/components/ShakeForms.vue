<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="shake-form" method="POST" @submit="createShake">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input
          type="text"
          id="nome"
          name="nome"
          v-model="nome"
          placeholder="Digite o seu nome"
        />
      </div>
      <div class="input-container">
        <label for="whey">Escolha o Whey:</label>
        <select name="whey" id="whey" v-model="whey">
          <option value="">Selecione o seu pão</option>
          <option v-for="whey in wheys" :key="whey.id" :value="whey.tipo">
            {{ whey.tipo }}
          </option>
        </select>
      </div>
      <div class="input-container">
        <label for="sabor">Escolha a sabor do seu Burger:</label>
        <select name="sabor" id="sabor" v-model="sabor">
          <option value="">Selecione o tipo de sabor</option>
          <option v-for="sabor in sabores" :key="sabor.id" :value="sabor.tipo">
            {{ sabor.tipo }}
          </option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais"
          >Selecione os opcionais:</label
        >
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisdata"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            :value="opcional.tipo"
          />
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar meu Shake!" />
      </div>
    </form>
  </div>
</template>

<script>
import Message from "./Message";
export default {
  name: "ShakeForms",
  data() {
    return {
      wheys: null,
      sabores: null,
      opcionaisdata: null,
      nome: null,
      whey: null,
      sabor: null,
      opcionais: [],
      status: "Solicitado",
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();
      this.wheys = data.wheys;
      this.sabores = data.sabores;
      this.opcionaisdata = data.opcionais;
    },
    async createShake(e) {
      e.preventDefault();
      const data = {
        nome: this.nome,
        whey: this.whey,
        sabor: this.sabor,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };
      const dataJson = JSON.stringify(data);
      const req = await fetch("http://localhost:3000/shakes", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();
      console.log(res);
      this.msg = `Pedido Nº ${res.id} realizado com sucesso`;
      // Limpar mensagem
      setTimeout(() => (this.msg = ""), 3000);
      // limpar campos
      this.nome = "";
      this.whey = "";
      this.sabor = "";
      this.opcionais = [];
    },
  },
  mounted() {
    this.getIngredientes();
  },
  components: {
    Message,
  },
};
</script>

<style scoped>
#shake-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #b3541e;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
}

.submit-btn {
  background-color: #222;
  color: #b3541e;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 1em;
  margin: 0 auto;
  cursor: pointer;
  border-radius: 8px;
  transition: 0.5s;
}

.submit-btn:hover {
  color: #222;
  background-color: #b3541e;
}
</style>