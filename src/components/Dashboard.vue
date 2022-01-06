<template>
  <div id="coffe-table" v-if="coffes">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="coffe-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Cafe:</div>
        <div>Copo:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="coffe-table-rows">
      <div class="coffe-table-row" v-for="coffe in coffes" :key="coffe.id">
        <div class="order-number">{{ coffe.id }}</div>
        <div>{{ coffe.nome }}</div>
        <div>{{ coffe.cafe }}</div>
        <div>{{ coffe.copo }}</div>
        <div>
          <ul v-for="(opcional, index) in coffe.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updatecoffe($event, coffe.id)">
            <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="coffe.status == s.tipo">
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deletecoffe(coffe.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h2>Não há pedidos no momento!</h2>
  </div>
</template>
<script>
  
  import Message from './Message.vue';

  export default {
    name: "Dashboard",
    data() {
      return {
        coffes: null,
        coffe_id: null,
        status: [],
        msg: null,
      }
    },
    components: {
      Message
    },
    methods: {
      async getPedidos() {
        const req = await fetch('http://localhost:3000/coffes')
        const data = await req.json()
        this.coffes = data
        // Resgata os status de pedidos
        this.getStatus()
      },
      async getStatus() {
        const req = await fetch('http://localhost:3000/status')
        const data = await req.json()
        this.status = data
      },
      async deletecoffe(id) {
        const req = await fetch(`http://localhost:3000/coffes/${id}`, {
          method: "DELETE"
        });
        const res = await req.json();

         this.getPedidos() 
     
     },
      async updatecoffe(event, id) {
        const option = event.target.value;
        const dataJson = JSON.stringify({status: option});
        const req = await fetch(`http://localhost:3000/coffes/${id}`, {
          method: "PATCH",
          headers: { "Content-Type" : "application/json" },
          body: dataJson
        });
        const res = await req.json();

        this.msg = `O pedido N° ${res.id} foi atualizado para ${res.status}!`;
            setTimeout(() => this.msg = "", 3000);

        console.log(res)
      }
    },
    mounted () {
    this.getPedidos()
    }
  }
</script>

<style scoped>
  #coffe-table {
    max-width: 1200px;
    margin: 0 auto;
  }
  #coffe-table-heading,
  #coffe-table-rows,
  .coffe-table-row {
    display: flex;
    flex-wrap: wrap;
  }
  #coffe-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }
  .coffe-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }
  #coffe-table-heading div,
  .coffe-table-row div {
    width: 19%;
  }
  #coffe-table-heading .order-id,
  .coffe-table-row .order-number {
    width: 5%;
  }
  select {
    padding: 12px 6px;
    margin-right: 12px;
  }
  .delete-btn {
    background-color: #222;
    color:#ffffff;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }
  
</style>