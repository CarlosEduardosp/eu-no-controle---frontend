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
    cadastrar(){
      cosole.log('teste')
    }
  }
}

</script>

<template>
  <div class="about">
    <Menu />

    <div class="corpo">
      <p>Sobre</p>
    </div>

  </div>
</template>
