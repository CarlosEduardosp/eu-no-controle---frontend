<script>

export default {
    name: 'Menu'
}
</script>

<template>
    <div class="main">
        <img class="logo" src="../assets/imagens/mao.jpg">
        <div class="titulo">

            Eu no Controle
        </div>
        <nav class="nav">
            <router-link class="item_menu" to="/">Home</router-link>
            <p class="item_menu">|</p>
            <router-link class="item_menu" to="/sobre">Sobre</router-link>
            <a href="https://www.instagram.com/kadu_spadilha/"><img class="icone" src="../assets/imagens/instagram.png" alt=""></a>
            <a href="https://www.linkedin.com/in/carlos-eduardo-padilha-0a105040/"><img class="icone" src="../assets/imagens/linkedin.png" alt=""></a>
            <a href="https://wa.me/qr/VLUWS67P35KPN1"><img class="icone" src="../assets/imagens/whatsapp.png" alt=""></a>
        </nav>

        <router-view />
    </div>
</template>

<style scoped>
.main {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
    background-color: #fff;
    height: 4rem;
    box-shadow: 0px 0px 5px 1px #f0df46;
    width: 100%;
}

.nav {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    margin-left: 0.5rem;
}

.titulo {
    font-size: 1.3rem;
    font-weight: bold;
    color: #3f4555;
    width: auto;

}

.item_menu {
    list-style-type: none;
    text-decoration: none;
    font-size: 1rem;
    color: #0b5526;
    margin-right: 4px;
}

.imagem {
    width: 3rem;
}

.icone {
    width: 1.5rem;
    margin: 0px 0.2rem;
}

.logo {
    width: 2.5rem;
    margin: 0px 0rem;
}

</style>