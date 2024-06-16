<script>
import Menu from '/src/components/header.vue';

export default {
  name: 'SobreView',
  components: {
    Menu
  },
  data() {
    return {
      lista_de_compras: [],
      produto: ''
    }
  },
  methods: {
    cadastrar() {
      console.log('teste')
    }
  }
}
</script>

<template>

  <Menu />
  <div class="about">

    <div class="corpo">
      <h2>Sobre a Aplicação</h2>
      <p>Esta aplicação é um gerenciador de listas de compras. Com ela, você pode criar e organizar suas listas de
        produtos de maneira prática e eficiente.</p>
      <h3>Principais Funcionalidades:</h3>
      <ul>
        <li>
          <strong>Adicionar Produtos:</strong> Você pode adicionar produtos à sua lista informando o nome, preço unitário e quantidade.
        </li>
        <li>
          <strong>Calcular Total:</strong> A aplicação calcula automaticamente o preço total de cada produto e o valor total de todos os
          produtos na lista.
        </li>
        <li>
          <strong>Salvar Lista:</strong> As listas são salvas no seu navegador, então você pode fechar a página e voltar mais tarde sem
          perder suas informações.
        </li>
        <li>
          <strong>Editar e Deletar:</strong> Você pode deletar produtos individuais da lista ou apagar toda a lista de uma vez.
        </li>
        <li>
          <strong>Nomear Lista:</strong> Dê um nome à sua lista de compras para facilitar a organização.
        </li>
      </ul>
      <p>É uma maneira simples e eficaz de gerenciar suas compras e garantir que você não esqueça de nada importante!</p>
    </div>
  </div>
</template>

<style scoped>
.about {
  background-color: #333333;
  color: #f3f3f3;
  font-family: Arial, sans-serif;
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.corpo {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  font-size: 1.1rem;
  color: #ddcf8a;
}

h2 {
  font-size: 2rem;
  color: #f3f3f3;
  margin-bottom: 20px;
}

h3 {
  font-size: 1.5rem;
  color: #f3f3f3;
  margin-bottom: 10px;
}

ul {
  list-style-type: disc;
  margin-left: 20px;
  margin-bottom: 20px;
}

li {
  margin-bottom: 10px;
}

p {
  margin-bottom: 15px;
  line-height: 1.6;
}

strong{
  color: #f3f3f3;
}
</style>
