<template>
  <div id="shake-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="shake-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Whey:</div>
        <div>Sabor:</div>
        <div>Opcionais</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="shake-table-rows">
      <div class="shake-table-row" v-for="shake in shakes" :key="shake.id">
        <div class="order number">{{ shake.id }}</div>
        <div>{{ shake.nome }}</div>
        <div>{{ shake.whey }}</div>
        <div>{{ shake.sabor }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in shakes.opcionais" :key="index">
              {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updateShake($event, shake.id)"
          >
            <option value="">Selecione</option>
            <option
              v-for="s in status"
              :key="s.id"
              :value="s.tipo"
              :selected="shake.status == s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteShake(shake.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "Dashboard",

  data() {
    return {
      shakes: null,
      shakes_id: null,
      status: [],
      msg: null,
    };
  },
  components: {
    Message
  },
  methods: {
    async getPedidos() {
      const req = await fetch("http://localhost:3000/shakes");
      const data = await req.json();

      this.shakes = data;

      // resgatar os status
      this.getStatus();
    },

    // fazendo a requisição dos status dos pedidos

    async getStatus() {
      const req = await fetch("http://localhost:3000/status");

      const data = await req.json();

      this.status = data;
    },
    async deleteShake(id) {
      const req = await fetch(`http://localhost:3000/shakes/${id}`, {
        method: "DELETE",
      });

      const res = await req.json();

      //mensagem de pedido deletado
      this.msg = `O pedido foi atualizado!`;

      // Limpar mensagem
    setTimeout(() => {
      this.msg = ""
    }, 3000);

      this.getPedidos();
    },
    async updateShake(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/shakes/${id}`, {
        method: "PATCH", // semelhante ao update, mas só atualiza o que enviamos, nesse caso: o status
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();
      console.log(res);
    },
  },
  mounted() {
    this.getPedidos();
  },
};
</script>

<style scoped>
#shake-table {
  max-width: 1200px;
  margin: 0 auto;
}
#shake-table-heading,
#shake-table-rows,
.shake-table-row {
  display: flex;
  flex-wrap: wrap;
}
#shake-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
.shake-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}
#shake-table-heading div,
.shake-table-row div {
  width: 19%;
}
#shake-table-heading .order-id,
.shake-table-row .order-number {
  width: 5%;
}
select {
  padding: 12px 6px;
  margin-right: 12px;
}
.delete-btn {
  background-color: #222;
  color: #b3541e;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>