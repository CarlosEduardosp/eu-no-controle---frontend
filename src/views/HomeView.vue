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
      preco: '',
      quantidade: '',
      total: 0.0,
      nome_da_lista: '',
      nome_lista: '',
      id: 0
    }
  },
  methods: {
    cadastrar() {
      // convertendo preço em float
      this.preco = parseFloat(this.preco)
      this.total = parseFloat(this.total)

      this.preco = this.preco * this.quantidade
      this.total = this.total + this.preco

      //adicionando um valor ao id
      this.id = this.id + 1

      // adiquirindo os dados do produto
      const dados = {
        'id': this.id,
        'nome': this.produto,
        'preco': this.preco,
        'quantidade': this.quantidade
      }

      const dados_completos = {
        'nome_da_lista': this.nome_da_lista,
        'dados': dados
      }

      // adicionando o produto na lista, salvar isso no banco de dados.
      this.lista_de_compras.push(dados_completos)

      //salvando no localstorage
      localStorage.setItem('lista_de_compras', JSON.stringify(dados_completos));


      //limpando os dados do formulário
      this.produto = ''
      this.preco = ''
      this.quantidade = ''

    },
    deletar() {

    },
    zerar() {
      this.lista_de_compras = []
      this.total = ''
    },
    apagar() {
      this.lista_de_compras = []
      this.total = ''
      this.nome_da_lista = ''
    },
    adiconar_nome_lista() {
      this.nome_da_lista = this.nome_lista
      this.nome_lista = ''
    }

  },
  mounted() {
    //let valor = JSON.parse(localStorage.getItem('lista_de_compras'));
    //console.log('valor do localstorage',valor)
    //this.lista_de_compras.push(valor.dados)
  }
}
</script>

<template>
  <div class="home">
    <Menu />
    <div class="corpo">

      <label for="" class="titulo">Lista de Compras</label>

      <form @submit.prevent="adiconar_nome_lista" v-show="!this.nome_da_lista">
        <div class="lista">
          <label for="">Insira o Nome da sua Lista:</label>
          <input type="text" v-model="nome_lista" required>
          <button type="submit"></button>
        </div>
      </form>


      <form @submit.prevent="cadastrar" v-show="this.nome_da_lista">


        <label for="">Nome:</label>
        <input type="text" v-model="produto" required>
        <label for="">Preço: </label>
        <input type="text" v-model="preco" required>
        <label for="">Quantidade: </label>
        <input type="number" v-model="quantidade" min="1" required>
        <button type="submit">cadastrar</button>

      </form>
      Produtos da Lista {{ nome_da_lista }}:
      <p v-for="(item, index) in lista_de_compras" :key="index">
        Id: {{ item.dados['id'] }}
        Nome: {{ item.dados['nome'] }}
        Preço: {{ item.dados['preco'] }}
        Quantidade: {{ item.dados['quantidade'] }}
        <button @click="deletar">Deletar Item</button>
      </p>
      <div class="total">
        <h3>
          {{ total }}
        </h3>
      </div>
      <div class="final">
        <button @click="zerar">Zerar Lista</button>
        <button @click="apagar">Apagar Lista</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.home {
  width: 100%;
}

.corpo {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #CCCCCC;
  width: 90%;
}

.lista {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

form{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
}

.titulo{
  color: #FFFACD;
  margin: 10px 0px;
  font-size: 1.5rem;
}
</style>
