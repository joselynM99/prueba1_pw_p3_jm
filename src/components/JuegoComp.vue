<template>
  <div v-if="mostrarJuego">
    <div id="contenedor-juego" v-if="banderaJuego">
      <p>Puntaje: {{ puntaje }}</p>
      <p>Intento: {{ intento }}</p>
    </div>
    <div v-if="mostrarInicio">
      <img src="../img/negro.png" alt="N" />
      <img src="../img/negro.png" alt="N" />
      <img src="../img/negro.png" alt="N" />
    </div>
    <div v-if="!mostrarInicio">
      <img :src="this.poke1.ruta" alt="N" />
      <img :src="this.poke2.ruta" alt="N" />
      <img :src="this.poke3.ruta" alt="N" />
    </div>
    <div>
      <p>{{ pokemonTexto1 }}</p>
      <p>{{ pokemonTexto2 }}</p>
      <p>{{ pokemonTexto3 }}</p>
    </div>

    <button v-on:click="jugar()">Jugar</button>
  </div>
  <div v-if="mostrarGanador">
    <div>
      <p class="ganador">Puntaje: {{ puntaje }}</p>
    </div>
    <img src="../img/congratulations.gif" alt="N" />
    <h2 class="ganador">Felicitaciones has ganado un premio de $10000,00</h2>
    <button v-on:click="nuevoJuego">Nuevo juego</button>
  </div>
  <div v-if="mostrarPerdedor">
    <h1 class="perdedor">Has utilizado tus 5 intentos</h1>
    <h1 class="perdedor">El juego ha terminado</h1>
    <button v-on:click="nuevoJuego">Nuevo juego</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      puntaje: 0,
      intento: 0,
      banderaJuego: true,
      pokemonTexto1: "xxxxxxxxxxx",
      pokemonTexto2: "xxxxxxxxxxx",
      pokemonTexto3: "xxxxxxxxxxx",
      poke1: null,
      poke2: null,
      poke3: null,
      mostrarInicio: true,
      mostrarJuego: true,
      mostrarGanador: false,
      mostrarPerdedor: false,
    };
  },
  methods: {
    async jugar() {
      const numAle1 = this.obtenerAleatorio(3);
      const numAle2 = this.obtenerAleatorio(3);
      const numAle3 = this.obtenerAleatorio(3);
      const vectorPlantilla = await this.construirPokemons();

      this.poke1 = vectorPlantilla[numAle1];
      this.poke2 = vectorPlantilla[numAle2];
      this.poke3 = vectorPlantilla[numAle3];

      this.pokemonTexto1 = this.poke1.nombre;
      this.pokemonTexto2 = this.poke2.nombre;
      this.pokemonTexto3 = this.poke3.nombre;

      this.mostrarInicio = false;

      this.intento++;

      if (
        this.poke1.nombre == this.poke2.nombre &&
        this.poke3.nombre == this.poke1.nombre
      ) {
        this.puntaje = this.puntaje + 5;
        console.log(this.poke1.nombre);
      } else if (
        this.poke1.nombre == this.poke2.nombre ||
        this.poke3.nombre == this.poke1.nombre ||
        this.poke3.nombre == this.poke2.nombre
      ) {
        this.puntaje = this.puntaje + 2;
      }

      if (this.puntaje >= 10) {
        this.mostrarGanador = true;
        this.mostrarJuego = false;
        this.mostrarPerdedor = false;
      }

      if (this.intento == 5) {
        this.mostrarPerdedor = true;
        this.mostrarJuego = false;
        this.mostrarGanador = false;
      }
    },

    obtenerAleatorio(tope) {
      return Math.floor(Math.random() * tope) + 1;
    },

    async consumirAPI(id) {
      const data = await fetch("https://pokeapi.co/api/v2/pokemon/" + id).then(
        (r) => r.json()
      );
      return data;
    },

    definirPokemons() {
      const vectorIdPokemons = [2, 6, 80, 15];
      return vectorIdPokemons;
    },

    async construirPokemons() {
      const vectorobjetosPoke = [];
      const vector = this.definirPokemons();
      for (let i = 0; i < 4; i++) {
        const idPokemon = vector[i];
        const objp = await this.construirObjetoPokemonImg(idPokemon);
        vectorobjetosPoke.unshift(objp);
      }

      return vectorobjetosPoke;
    },

    async construirObjetoPokemonImg(id) {
      const { name } = await this.consumirAPI(id);
      const objetoPoke = {
        ruta:
          "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/" +
          id +
          ".svg",
        nombre: name,
      };
      return objetoPoke;
    },

    nuevoJuego() {
      this.mostrarJuego = true;
      this.mostrarInicio = true;
      this.mostrarGanador = false;
      this.mostrarPerdedor = false;

      this.pokemonTexto1 = "xxxxxxxxxxx";
      this.pokemonTexto2 = "xxxxxxxxxxx";
      this.pokemonTexto3 = "xxxxxxxxxxx";
      this.puntaje = 0;
      this.intento = 0;
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
  margin: 15px 120px;
}

button {
  margin-top: 10px;
}

.perdedor {
  color: red;
}

.ganador {
  color: blue;
}
</style>