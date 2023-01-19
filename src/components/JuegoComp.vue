<template>
  <div id="contenedor-juego" v-if="banderaJuego">
    <p>Puntaje: {{ puntaje }}</p>
    <p>Intento: {{ intento }}</p>
  </div>
  <div>
    <img src="../img/negro.png" alt="N" />
    <img src="../img/negro.png" alt="N" />
    <img src="../img/negro.png" alt="N" />
  </div>
  <div>
    <p>{{ pokemonTexto1 }}</p>
    <p>{{ pokemonTexto2 }}</p>
    <p>{{ pokemonTexto3 }}</p>
  </div>

  <button v-on:click="jugar()">Jugar</button>
</template>

<script>
export default {
  data() {
    return {
      puntaje: 0,
      intento: 0,
      banderaJuego: true,
      pokemonTexto1: "xxxxxxxxxxxxx",
      pokemonTexto2: "xxxxxxxxxxxxx",
      pokemonTexto3: "xxxxxxxxxxxxx",
    };
  },
  methods: {
    jugar() {},

    obtenerAleatorio(tope) {
      return Math.floor(Math.random() * tope) + 1;
    },

    async consumirAPI(id) {
      return id;
    },

    definirPokemons() {

      const vectorIdPokemons = [2, 6, 80, 15];
      return vectorIdPokemons
    },

    construirPokemons() {
      //const vector=this.definirPokemons();
      //const vectorPokemons=[]
    },

    definirJuego() {
      
    },
  },
};
</script>

<style>
img {
  margin: 15px 60px;
  width: 200px;
  height: 200px;
}

p {
  display: inline;
  margin: 15px 50px;
}

button {
  margin-top: 10px;
}
</style>