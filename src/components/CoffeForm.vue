<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <form id="coffe-form" method="POST" @submit="createCoffe">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
      </div>
      <div class="input-container">
        <label for="cafe">Escolha o Café:</label>
        <select name="cafe" id="cafe" v-model="cafe">
          <option value="">Selecione o seu Café</option>
          <option v-for="cafe in coffe" :key="cafe.id" :value="cafe.tipo">{{ cafe.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="copo">Escolha tipo de Copo:</label>
        <select name="copo" id="copo" v-model="copo">
          <option value="">Selecione seu Copo</option>
          <option v-for="copo in copos" :key="copo.id" :value="copo.tipo">{{ copo.tipo }}</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Finalizar Pedido!">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message'
export default {
  name: "CoffeForm",
  data() {
    return {
      coffe: null,
      copos: null,
      opcionaisdata: null,
      nome: null,
      cafe: null,
      copo: null,
      opcionais: [],
      status: "Solicitado",
      msg: null
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch('http://localhost:3000/ingredientes')
      const data = await req.json()
      this.coffe = data.coffe
      this.copos = data.copos
      this.opcionaisdata = data.opcionais
    },
    async createCoffe(e) {
      e.preventDefault()
      const data = {
        nome: this.nome,
        carne: this.copo,
        cafe: this.cafe,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }
      const dataJson = JSON.stringify(data)    
      const req = await fetch("http://localhost:3000/coffes", {
        method: "POST",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      });
      const res = await req.json();
      console.log(res);
      this.msg = `Pedido N° ${res.id} realizado com sucesso!`;
      // clear message
      setTimeout(() => this.msg = "", 3000);
      // limpar campos
      this.nome = "";
      this.copo = "";
      this.cafe = "";
      this.opcionais = [];
      
    }
  },
  mounted () {
    this.getIngredientes()
  },
  components: {
    Message
  }
}
</script>

<style scoped>
  #coffe-form {
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
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #DEB887;
  }
  input, select {
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
    font-weight: bold;
  }
  .submit-btn {
    background-color:  #532809;
    color:#eeeeee;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>