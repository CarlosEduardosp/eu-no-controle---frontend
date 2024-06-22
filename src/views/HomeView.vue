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
      dados: [],
      iniciar: false,
      id: 0,
      quantidades: 0,
      listas_salvas: []
    }
  },
  methods: {
    cadastrar() {
      // convertendo preço em float
      this.preco_total = parseFloat(this.preco_total)
      this.total = parseFloat(this.total)
      this.preco_unitario = parseFloat(this.preco_unitario)
      this.quantidade = parseFloat(this.quantidade)

      console.log(this.quantidade)

      // calculando o preço
      this.preco_total = this.preco_unitario * this.quantidade
      this.total = this.total + this.preco_total

      const data_criacao_lista = this.pegardata()

      // adiquirindo os dados do produto
      const dados = {
        'nome': this.produto,
        'preco_unitario': this.preco_unitario,
        'preco_total': this.preco_total,
        'quantidade': this.quantidade,
        'nome_da_lista': this.nome_da_lista,
        'data': data_criacao_lista,
        'status': false
      }


      // adicionando o produto na lista, salvar isso no banco de dados.
      this.lista_de_compras.push(dados)

      //salvando no localstorage
      if (this.lista_de_compras != []) {
        let chave = this.nome_da_lista
        localStorage.setItem(chave, JSON.stringify(this.lista_de_compras));
      }

      //limpando os dados do formulário
      this.produto = ''
      this.preco_total = 0.00
      this.preco_unitario = 0.00
      this.quantidade = 1

    },
    voltar() {
      this.iniciar = true
      this.nome_da_lista = ''
      this.iniciando()
    },
    deletar(index) {

      if (this.lista_de_compras.length == 1) {
        //deleta os dados da lista do banco localstorage
        localStorage.removeItem(this.nome_da_lista);
        this.nome_da_lista = ''
        this.listas_salvas = []
        this.iniciando()
      }
      else {
        // Remover o item da lista
        this.lista_de_compras.splice(index, 1);

        // Recalcular o total
        this.total = this.lista_de_compras.reduce((acc, item) => acc + item.preco_total, 0);

        // Atualizar o localstorage
        localStorage.setItem(this.nome_da_lista, JSON.stringify(this.lista_de_compras));
      }
    },

    apagar() {
      // recebe a confirmação do úsuario
      const response = confirm('Deseja realmente excluir esta lista, e todos os produtos pertencentes a ela ?')

      // caso a resposta seja sim ou ok
      if (response) {
        this.lista_de_compras = []
        this.total = 0
        this.iniciar = false
        this.nome_lista = ''
        //deleta os dados da lista do banco localstorage
        localStorage.removeItem(this.nome_da_lista);
        this.nome_da_lista = ''
        this.listas_salvas = []
        this.iniciando()
      }

    },
    finalizar() {

      let valor = JSON.parse(localStorage.getItem(this.nome_da_lista));

      if (valor) {

        this.lista_de_compras = []

        for (let item of valor) {

          if (item.nome_da_lista == this.nome_da_lista) {

            const dados = {
              'nome': item.nome,
              'preco_unitario': item.preco_unitario,
              'preco_total': item.preco_total,
              'quantidade': item.quantidade,
              'nome_da_lista': item.nome_da_lista,
              'data': data_criacao_lista,
              'status': !item.status
            }

            // adicionando o produto na lista, salvar isso no banco de dados.

            this.lista_de_compras.push(dados)

            //salvando no localstorage
            if (this.lista_de_compras != []) {
              localStorage.setItem(this.nome_da_lista, JSON.stringify(this.lista_de_compras));
            }

          }

        }

      }

      alert('Função em Desenvolvimento.')

    },
    adiconar_nome_lista() {
      console.log(this.nome_da_lista)
      this.nome_da_lista = this.nome_lista
      this.lista_de_compras = []
      this.total = 0
      this.nome_lista = ''
    },
    recuperarTodosOsItens() {
      let todosOsItens = [];
      // Itera sobre todas as chaves no localStorage
      for (let i = 0; i < localStorage.length; i++) {
        let chave = localStorage.key(i);
        let valor = JSON.parse(localStorage.getItem(chave));
        todosOsItens.push({ chave, valor });
      }
      return todosOsItens;
    },
    preencher_lista(chave, valor) {
      this.nome_da_lista = chave
      this.lista_de_compras = valor

      this.total = 0
      for (let item of valor) {
        this.total = this.total + item.preco_total
      }

    },
    somando_total(nome) {
      // função para calcular o valor total da lista armazenada.

      var total_menu = 0


      for (let item of this.listas_salvas) {
        if (nome == item.valor[0].nome_da_lista) {
          for (let item2 of item.valor) {
            total_menu = total_menu + item2.preco_total
          }
          return total_menu

        }
      }



    },
    iniciando() {
      let valor = this.recuperarTodosOsItens()

      this.listas_salvas = []

      for (let lista of valor) {
        this.listas_salvas.push(lista)
      }
    },
    pegardata() {
      // Obtendo a data atual
      const dataAtual = new Date();

      // Extraindo informações da data
      const dia = dataAtual.getDate();
      const mes = dataAtual.getMonth() + 1; // Os meses são indexados de 0 a 11
      const ano = dataAtual.getFullYear();

      // Formatando a data
      const dataFormatada = `${dia}/${mes}/${ano}`;

      return dataFormatada
    }

  },
  mounted() {

    this.iniciando()

  }
}

</script>

<template>
  <div class="home">
    <Menu />
    <div class="corpo">

      <label for="" class="titulo">Registro de Compras</label>



      <form @submit.prevent="" v-show="!this.iniciar">
        <p class="text">"Gerencie suas compras com praticidade e eficiência. Adicione produtos, acompanhe preços e
          mantenha sua
          lista sempre atualizada."</p>
        <div class="lista">
          <button class="input_nome_lista" @click="this.iniciar = true">CLIQUE AQUI
            PARA GERENCIAR SUAS COMPRAS</button>

          <img class="logo" src="../assets/imagens/mao.jpg">
        </div>
      </form>

      <label class="subtitulo" v-show="this.iniciar == true && !this.nome_da_lista">Inicie Uma Nova Lista </label>

      <form @submit.prevent="adiconar_nome_lista" class="bloco1" v-show="this.iniciar && !this.nome_da_lista">

        <div class="nova_lista">
          <label for="">Dê um Nome para Ela </label>
          <input type="text" class="input_nome" v-model="this.nome_lista" required>
          <button type="submit" class="btn_ok">INICIAR</button>
        </div>
      </form>

      <label class="subtitulo2" v-show="this.iniciar == true && !this.nome_da_lista && this.listas_salvas[0]">Continue
        Uma Lista Já Iniciada
      </label>

      <div class="sub_lista" v-show="this.iniciar && !this.nome_da_lista && this.listas_salvas[0]">

        <div class="sub_lista2">
          <P class="data">Data</P>
          <P class="nome">Nome Lista</P>
          <P class="valor_total">Valor Total</P>
          <P class="status">Status</P>
        </div>
        <p v-for="(item, index) in this.listas_salvas" :key="index">

        <div class="chave_listas" @click="preencher_lista(item.chave, item.valor)">

          <p class="data">{{ item.valor[0].data }}</p>

          <p class="nome">{{ item.chave }}</p>

          <p class="valor_total" v-if="item.valor != []">R$ {{ somando_total(item.valor[0].nome_da_lista).toFixed(2) }}</p>
          <p class="valor_total" v-else>R$ {{ somando_total('nada') }}</p>

          <p class="status" v-if="item.valor[0].status">Fechada</p>
          <p class="status" v-if="!item.valor[0].status">Aberta</p>

        </div>

        </p>
      </div>

      <form @submit.prevent="cadastrar" v-show="this.iniciar && this.nome_da_lista">

        <p v-show="!this.lista_de_compras[0]">Insira o Primeiro Produto da sua Lista</p>
        <p v-show="this.lista_de_compras[0]">Insira Aqui o Próximo Produto da sua Lista</p>

        <div class="bloco1">
          <label for="">Nome</label>
          <input type="text" class="input_nome" v-model="produto" required>
          <label for="">Preço </label>
          <input type="number" class="input_preco" step="any" min="0.1" v-model="preco_unitario" required>
        </div>
        <div class="radio">
          <label>
            <input class="input_radio" v-model="quantidades" type="radio" value="0" required> Unidade (Un)
          </label>
          <label>
            <input class="input_radio" type="radio" v-model="quantidades" value="1" required> Kilograma (Kg)
          </label>
        </div>
        <div class="bloco2">
          <label for="" v-if="quantidades == 0">Quant. (Un)</label>
          <input type="number" class="input_quantidade" v-model="quantidade" min="1" v-if="quantidades == 0" required>
          <label for="" v-if="quantidades == 1">Quant. (Kg)</label>
          <input type="number" class="input_quantidade" step="any" v-model="quantidade" min="0.1"
            v-if="quantidades == 1" required>

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



          <p class="sub_item">{{ index + 1 }}</p>
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
        <button @click="voltar" class="voltar" v-show="this.iniciar && this.nome_da_lista">VOLTAR PARA
          LISTAS</button>
        <button @click="this.iniciar = false" class="voltar" v-show="this.iniciar && !this.nome_da_lista">VOLTAR
          AO ÍNICIO</button>
        <button @click="apagar" class="apagar"
          v-show="this.iniciar && this.nome_da_lista && this.lista_de_compras[0]">APAGAR
          LISTA</button>
        <button @click="finalizar" class="finalizar"
          v-show="this.iniciar && this.nome_da_lista && this.lista_de_compras[0]">FINALIZAR
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
  margin-bottom: 2rem;
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

.nova_lista {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.subtitulo {
  margin: 2rem 0rem 0rem 0rem;
  font-size: 1.5rem;
}

.subtitulo2 {
  margin: 2rem 0rem 1rem 0rem;
  font-size: 1.5rem;
}

.chave_listas {
  display: flex;
  flex-direction: row;
  margin: 0.2rem 0rem;
  background-color: #777777;
  padding: 0.5rem;
  width: 95vw;
  color: #ffffff;
  border-radius: 5px;
}

.btn_ok {
  padding: 0.3rem;
  font-weight: 300;
  margin-left: 2px;
}

.sub_lista2 {
  display: flex;
  flex-direction: row;
  padding: 0.5rem;
}

.data {
  width: 25%;
}

.nome {
  width: 35%;
}

.valor_total {
  width: 25%;
}

.status {
  width: 15%;
}

.radio{
  margin-top: 1rem;
  margin-bottom: 0.5rem;

}
</style>
