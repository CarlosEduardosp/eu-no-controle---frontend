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
      preco_total: 0,
      preco_unitario: 0,
      quantidade: 1,
      total: 0,
      nome_da_lista: '',
      nome_lista: '',
      id: 0
    }
  },
  methods: {
    cadastrar() {
      // convertendo preço em float
      this.preco_total = parseFloat(this.preco_total)
      this.total = parseFloat(this.total)
      this.preco_unitario = parseFloat(this.preco_unitario)

      // calculando o preço
      this.preco_total = this.preco_unitario * this.quantidade
      this.total = this.total + this.preco_total


      //adicionando um valor ao id
      this.id = this.id + 1

      // adiquirindo os dados do produto
      const dados = {
        'id': this.id,
        'nome': this.produto,
        'preco_unitario': this.preco_unitario,
        'preco_total': this.preco_total,
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
      this.preco_total = 0
      this.preco_unitario = 0
      this.quantidade = 0

    },
    deletar() {

    },
    zerar() {
      this.lista_de_compras = []
      this.total = 0
    },
    apagar() {
      this.lista_de_compras = []
      this.total = 0
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
          <label for="" class="inserir_nome">Insira o Nome da sua Lista:</label>
          <input type="text" class="input_nome_lista" v-model="nome_lista" required>
          <img class="logo" src="../assets/imagens/mao.jpg">
        </div>
      </form>


      <form @submit.prevent="cadastrar" v-show="this.nome_da_lista">

        <p v-show="!this.lista_de_compras[0]">Insira o Primeiro Produto da sua Lista</p>
        <p v-show="this.lista_de_compras[0]">Insira Aqui o Próximo Produto da sua Lista</p>

        <div class="bloco1">
          <label for="">Nome</label>
          <input type="text" class="input_nome" v-model="produto" required>
          <label for="">Preço </label>
          <input type="number" class="input_preco" step="any" min="0.1" v-model="preco_unitario" required>
        </div>
        <div class="bloco2">
          <label for="">Quantidade </label>
          <input type="number" class="input_quantidade" v-model="quantidade" min="1" required>
          <button type="submit" class="cadastrar">CADASTRAR</button>
        </div>
      </form>

      <div class="produtos" v-show="this.nome_da_lista && this.lista_de_compras[0]">
        <p class="titulo3">Produtos da Lista {{ nome_da_lista }} </p>
        <p class="titulo2">Total R$ <p class="total1">{{ total.toFixed(2) }}</p></p>
        <div class="subtitulos">
          <p class="sub_item">Item</p>
          <p class="sub_nome">Nome</p>
          <p class="sub_preco">P.U.</p>
          <p class="sub_preco">P.T.</p>
          <p class="sub_quant">Quant.</p>
          <p class="sub_del">Deletar</p>
        </div>
        <p v-for="(item, index) in lista_de_compras" :key="index">
        <div class="itens">
          <p class="sub_item">{{ item.dados['id'] }}</p>
          <p class="sub_nome">{{ item.dados['nome'] }}</p>
          <p class="sub_preco">{{ item.dados['preco_unitario'].toFixed(2) }}</p>
          <p class="sub_preco">{{ item.dados['preco_total'].toFixed(2) }}</p>
          <p class="sub_quant">{{ item.dados['quantidade'] }}</p>
          <button @click="deletar" class="btn_deletar">Deletar Item</button>
        </div>
        </p>
        
      </div>
      <div class="total" v-show="this.nome_da_lista && this.total">
        <h2 class="valor">
             {{ total.toFixed(2) }}
          </h2>
          <h4>
            Valor Total
          </h4>
        
        </div>
      <div class="final" v-show="this.nome_da_lista && this.lista_de_compras[0]">
        <button @click="zerar" class="zerar">ZERAR LISTA</button>
        <button @click="apagar" class="apagar">APAGAR LISTA</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.home {
  width: 100%;
}

.logo{
  width: 80%;
  border-radius: 5px;
  margin-bottom: 10%;
}

input:focus{
  outline: none;
}

.corpo {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #CCCCCC;
  width: 100%;
}

.lista {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #6d6d6d;
  padding: 10px;
  border-radius: 5px;
  width: 95%;
}

.titulo {
  color: #f0df46;
  margin: 20px 0px;
  font-size: 1.5rem;
  text-shadow: 0px 0px 20px #f0df46;
}



.bloco1 {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-top: 3%;
}

.bloco2 {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.zerar {
  padding: 10px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #978d2f;
  color: #f3f3f3;
}

.apagar {
  padding: 10px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #978d2f;
  color: #f3f3f3;
}

.cadastrar {
  padding: 7px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #978d2f;
  color: #f3f3f3;
  width: 35%;
  font-size: 1rem;
  font-weight: bold;
}

.input_nome_lista {
  width: 80%;
  height: 30px;
  text-align: center;
  font-size: 1rem;
  border-radius: 2px;
  border: none;
  margin: 5% 0%;
}

.input_nome {
  width: 30%;
  height: 30px;
  text-align: center;
  font-size: 1rem;
  margin-left: 6% ;

}

.input_preco {
  width: 20%;
  height: 30px;
  text-align: center;
  font-size: 1rem;
}

.input_quantidade {
  width: 30%;
  height: 30px;
  text-align: center;
  font-size: 1rem;
  margin-left: 2%;
}

.produtos {
  display: flex;
  flex-direction: column;  
  background-color: #6d6d6d;
  width: 95%;
  margin: 10px 0px;
  padding: 1%;
  min-height: 30vh;
}

.subtitulos {
  display: flex;
  flex-direction: row;
  margin: 3% 0%;
}

.sub_item {
  width: 10%;
}

.sub_nome {
  width: 25%;
}

.sub_preco {
  width: 15%;
}

.sub_quant {
  width: 10%;
  text-align: center;
}

.sub_del{
  width: 20%;
  text-align: end;
}

.itens {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 1% 0%;
}

.btn_deletar {
  width: 20%;
  padding: 0.5%;
  border-radius: 2px;
  border: none;
  margin-left: 3%;
}

.titulo2{
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 2%;
  font-size: 1.2rem;
  font-weight: bold;
}
.titulo3{
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 2%;
  font-size: 1.2rem;
  font-weight: bold;
  color: #f3f3f3;
}

.total{
  width: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 5% 0%;
}

.valor{
  background-color: #333333;
  border:2px solid #f3f3f3;
  color: #f0df46;
  padding: 10px;
  border-radius: 3px ;
  margin-bottom: 5%;
}

.inserir_nome{
  color: #f3f3f3;
  margin: 5% 0%;
  font-weight: 500;
  font-size: 1rem;
  letter-spacing: 2px;
}
.total1{
  color: #f0df46;
  margin-left: 2%;
}

</style>
