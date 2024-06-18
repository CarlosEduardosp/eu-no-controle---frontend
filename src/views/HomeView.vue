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
      preco_total: 0.00,
      preco_unitario: 0.00,
      quantidade: 1,
      total: 0.00,
      nome_da_lista: '',
      nome_lista: '',
      id: 0,
      dados: [],
      iniciar: false
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
        'quantidade': this.quantidade,
        'nome_da_lista': this.nome_da_lista,
        'finalizada': false
      }


      // adicionando o produto na lista, salvar isso no banco de dados.
      this.lista_de_compras.push(dados)

      //salvando no localstorage
      if (this.lista_de_compras != []) {
        localStorage.setItem('lista_de_compras', JSON.stringify(this.lista_de_compras));
      }




      //limpando os dados do formulário
      this.produto = ''
      this.preco_total = 0.00
      this.preco_unitario = 0.00
      this.quantidade = 1

    },
    voltar() {
      this.iniciar = false
    },
    deletar(index) {
      // Remover o item da lista
      this.lista_de_compras.splice(index, 1);

      // Recalcular o total
      this.total = this.lista_de_compras.reduce((acc, item) => acc + item.preco_total, 0);

      // Atualizar o localstorage
      localStorage.setItem('lista_de_compras', JSON.stringify(this.lista_de_compras));
    },

    apagar() {

      const response = confirm('Deseja realmente excluir esta lista, e todos os produtos pertencentes a ela ?')
      console.log(response)

      if (response) {
        this.lista_de_compras = []
        this.total = 0
        this.iniciar = false
        this.id = 0
        this.nome_da_lista = ''
        this.nome_lista = ''
        localStorage.removeItem('lista_de_compras');
      }


    },
    finalizar() {
      alert('Função em Desenvolvimento.')

    },
    adiconar_nome_lista() {
      console.log(this.nome_da_lista)
      this.nome_da_lista = this.nome_lista
    }

  },
  mounted() {
    let valor = JSON.parse(localStorage.getItem('lista_de_compras'));        

    if (valor) {

      // varrendo o conteudo de valor para achar o nome da lista não finalizada
      for (let item of valor){
      if(item.finalizada == false){
        this.nome_da_lista = valor[0].nome_da_lista
        console.log(item)
      }
    }

      this.lista_de_compras = valor;      
      this.total = this.lista_de_compras.reduce((acc, item) => acc + item.preco_total, 0);
      this.id = this.lista_de_compras.length ? this.lista_de_compras[this.lista_de_compras.length - 1].id : 0;
    }
  }
}
</script>

<template>
  <div class="home">
    <Menu />
    <div class="corpo">

      <label for="" class="titulo">Lista de Compras</label>



      <form @submit.prevent="" v-show="!this.iniciar">
        <p class="text">"Gerencie suas compras com praticidade e eficiência. Adicione produtos, acompanhe preços e
          mantenha sua
          lista sempre atualizada."</p>
        <div class="lista">
          <button class="input_nome_lista" @click="this.iniciar = true" v-show="!this.lista_de_compras[0]">CLIQUE AQUI
            PARA INICIAR UMA
            LISTA</button>
          <button class="input_nome_lista" @click="this.iniciar = true" v-show="this.lista_de_compras[0]">CLIQUE AQUI
            PARA CONTINUAR SUA
            LISTA</button>
          <img class="logo" src="../assets/imagens/mao.jpg">
        </div>
      </form>

      <form @submit.prevent="adiconar_nome_lista" class="bloco1" v-show="this.iniciar && !this.nome_da_lista"  >
          <label for="">Escolha um Nome para sua Lista </label>
          <input type="text" class="input_nome" v-model="this.nome_lista"  required>
          <button type="submit" >Ok</button>
      </form>

      <form @submit.prevent="cadastrar" v-show="this.iniciar && this.nome_da_lista">        

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

      <div class="produtos" v-show="this.iniciar && this.nome_da_lista && this.lista_de_compras[0]">
        <p class="titulo3">Lista de Compras: {{ nome_da_lista }} </p>
        <p class="titulo2">Valor Total R$
        <p class="total1">{{ total.toFixed(2) }}</p>
        </p>
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



          <p class="sub_item">{{ item.id }}</p>
          <p class="sub_nome">{{ item.nome }}</p>
          <p class="sub_preco">{{ item.preco_unitario.toFixed(2) }}</p>
          <p class="sub_preco">{{ item.preco_total.toFixed(2) }}</p>
          <p class="sub_quant">{{ item.quantidade }}</p>
          <button @click="deletar(index)" class="btn_deletar">Deletar Item</button>
        </div>
        </p>

      </div>
      <div class="total" v-show="this.nome_da_lista && this.iniciar && this.total">
        <p class="titulo3">
          Valor Total da Sua Lista
        </p>
        <p>R$</p>
        <h2 class="valor">
          {{ total.toFixed(2) }}
        </h2>

      </div>
      <div class="final">
        <button @click="voltar" class="voltar" v-show="this.iniciar">VOLTAR
          PÁGINA</button>
        <button @click="apagar" class="apagar" v-show="this.iniciar && this.nome_da_lista && this.lista_de_compras[0]">APAGAR
          LISTA</button>
        <button @click="finalizar" class="finalizar" v-show="this.iniciar && this.nome_da_lista && this.lista_de_compras[0]">FINALIZAR
          LISTA</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.home {
  width: 100%;
}

.text {
  text-align: center;
  width: 95%;
  font-size: 0.88rem;
  letter-spacing: 1px;
  line-height: 1.2rem;
  font-weight: bold;
  color: #f3f3f3;
}

.logo {
  width: 95%;
  border-radius: 5px;
  margin-bottom: 10%;
  box-shadow: 0px 0px 5px #d4d4d4;

}

input:focus {
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
  background-color: #333333;
  padding: 10px;
  border-radius: 5px;
  width: 95%;
}

.titulo {
  color: #f0df46;
  margin-top: 2rem;
  margin-bottom: 0.5rem;
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

.apagar {
  padding: 15px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #f3f3f3;
  color: #aa0000;
  font-size: 0.8rem;
  box-shadow: 0px 0px 10px #ff0000;
  font-weight: bold;
  width: auto;
  height: auto;
  margin-bottom: 1rem;
}

.voltar {
  padding: 15px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #f3f3f3;
  color: #a37f08;
  font-size: 0.8rem;
  box-shadow: 0px 0px 10px #a37f08;
  font-weight: bold;
  width: auto;
  height: auto;
  margin-bottom: 1rem;
}

.finalizar {
  padding: 15px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #f3f3f3;
  color: #008807;
  font-size: 0.8rem;
  box-shadow: 0px 0px 10px #008807;
  font-weight: bold;
  width: auto;
  height: auto;
  margin-bottom: 1rem;
}

.final {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.cadastrar {
  padding: 7px;
  border-radius: 5px;
  border: none;
  margin: 10px 10px;
  background-color: #978d2f;

  color: #fff;
  width: 35%;
  font-size: 1rem;
  font-weight: bold;
}

.input_nome_lista {
  width: 95%;
  height: 4rem;
  text-align: center;
  font-size: 0.85rem;
  border-radius: 4px;
  border: none;
  margin: 5% 0%;
  letter-spacing: 2px;
  text-align: center;
  background-color: #d4d4d4;
  color: #000;
  font-weight: bolder;

}

.input_nome {
  width: 30%;
  height: 30px;
  text-align: center;
  font-size: 1rem;
  margin-left: 6%;

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
  border-radius: 5px;
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

.sub_del {
  width: 20%;
  text-align: end;
}

.itens {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 1.5% 0%;
}

.btn_deletar {
  width: 20%;
  padding: 0.5%;
  border-radius: 2px;
  border: none;
  margin-left: 3%;
  color: #000;
}

.titulo2 {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 2%;
  font-size: 1.2rem;
  font-weight: bold;
}

.titulo3 {
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

.total {
  width: 90%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin: 5% 0%;
  gap: 3%;
}

.valor {
  background-color: #333333;
  border: 2px solid #f3f3f3;
  color: #f0df46;
  padding: 8px;
  border-radius: 5px;
  margin-bottom: 0%;
}

.inserir_nome {
  color: #f3f3f3;
  margin: 5% 0%;
  font-weight: 500;
  font-size: 1rem;
  letter-spacing: 2px;
}

.total1 {
  color: #f0df46;
  margin-left: 2%;
}
</style>
