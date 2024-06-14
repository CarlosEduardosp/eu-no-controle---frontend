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
      produto: 'arroz',
      preco: 0,
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
      this.preco = parseFloat(this.preco)
      this.total = parseFloat(this.total)

      this.preco = this.preco * this.quantidade
      this.total = this.total + this.preco
      console.log(this.preco, this.total)


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

      <form @submit.prevent="adiconar_nome_lista">
        <div class="lista">
          <label for="">Insira o Nome da sua Lista:</label>
          <input type="text" class="input_nome_lista" v-model="nome_lista" required>
        </div>
      </form>


      <form @submit.prevent="cadastrar">

        <div class="bloco1">
          <label for="">Nome</label>
          <input type="text" class="input_nome" v-model="produto" required>
          <label for="">Preço </label>
          <input type="number" class="input_preco" step="any" v-model="preco" required>
        </div>
        <div class="bloco2">
          <label for="">Quantidade </label>
          <input type="number" class="input_quantidade" v-model="quantidade" min="1" required>
          <button type="submit" class="cadastrar">CADASTRAR</button>
        </div>
      </form>

      <div class="produtos">
        <p class="titulo2">Produtos da Lista {{ nome_da_lista }}</p>
        <div class="subtitulos">
          <p class="sub_item">Item</p>
          <p class="sub_nome">Nome</p>
          <p class="sub_preco">Preço</p>
          <p class="sub_quant">Quant.</p>
          <p class="sub_del">Deletar</p>
        </div>
        <p v-for="(item, index) in lista_de_compras" :key="index">
        <div class="itens">
          <p class="sub_item">{{ item.dados['id'] }}</p>
          <p class="sub_nome">{{ item.dados['nome'] }}</p>
          <p class="sub_preco">{{ item.dados['preco'] }}</p>
          <p class="sub_quant">{{ item.dados['quantidade'] }}</p>
          <button @click="deletar" class="btn_deletar">Deletar Item</button>
        </div>
        </p>
        
      </div>
      <div class="total">
        <h2>
             {{ total }}
          </h2>
          <h4>
            Valor Total:
          </h4>
        
        </div>
      <div class="final">
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
  color: #FFFACD;
  margin: 20px 0px;
  font-size: 1.5rem;
}



.bloco1 {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 10px;
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
  background-color: #000000;
  color: #f3f3f3;
}

.apagar {
  padding: 10px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #000000;
  color: #f3f3f3;
}

.cadastrar {
  padding: 7px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #000000;
  color: #f3f3f3;
  width: 30%;
  font-size: 1rem;
}

.input_nome_lista {
  width: 80%;
  height: 30px;
  text-align: center;
  font-size: 1rem;
  border-radius: 2px;
  border: none;
}

.input_nome {
  width: 30%;
  height: 30px;
  text-align: center;
  font-size: 1rem;

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
  background-color: #6d6d6d;
  width: 95%;
  margin: 10px 0px;
  padding: 1%;
}

.subtitulos {
  display: flex;
  flex-direction: row;
  margin: 3% 0%;
}

.sub_item {
  width: 15%;
}

.sub_nome {
  width: 25%;
}

.sub_preco {
  width: 20%;
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
  text-align: center;
  padding: 2%;
  font-size: 1.2rem;
  font-weight: bold;
}

.total{
  width: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 5% 0%;
}
</style>
