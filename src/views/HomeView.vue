
<script>
import Menu from '/src/components/header.vue';


export default {
  name: 'HomeView',
  components: {
    Menu
  },
  data() {
    return {
      lista_de_compras: [],
      produto: '',
      preco: ''
    }
  },
  methods: {
    cadastrar(){
      console.log(this.produto, this.preco)
      this.produto = ''
      this.preco = ''
    }
  }
}
</script>

<template>
  <div class="home">
    <Menu/>
    <div class="corpo">

      <form @submit.prevent="cadastrar" >

      <label for="">Cadastrar Produto</label>
      <input type="text" v-model="produto" >
      <input type="number" v-model="preco" >
      <button type="submit">cadastrar</button>

      </form>

    </div>
  </div>
</template>

