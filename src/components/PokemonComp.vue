<template>
  <div id="punt">
    <h1>Puntaje: {{puntuacion}}</h1>
    <h1>Intento: {{intentosN}}</h1>
  </div>
  <div class="contenedor">
    <div class="cont1">
      <img id="img1" v-bind:src="obtenerImagenAleatorios(comparacion)" />
      <label>xxxxxxxxx</label>
    </div>
    <div class="cont1">
      <img id="img2" v-bind:src="obtenerImagenAleatorios(comparacion)" />
      <label>xxxxxxxxx</label>
    </div>
    <div class="cont1">
      <img id="img3" v-bind:src="obtenerImagenAleatorios(comparacion)" />
      <label>xxxxxxxxx</label>
    </div>
  </div>
  <div>
    <button v-on:click="jugar()" id="jugar">Jugar</button>
  </div>
</template>

<script>
export default {

  props:{
    comp:Boolean,
    intentos:Number,
    puntos:Number
  },

  data() {
    return {
      arreglo: [4, 23, 56, 7, 12],
      nombre: null,
      comparacion:this.comp,
      puntuacion:0,
      intentosN:5
      
    };
  },

  methods: {
    async consumirAPINombre() {
      const { name } = await fetch("https://pokeapi.co/api").then((result) =>
        result.json()
      );

      console.log(name);

      this.nombre = name;
    },

    obtenerImgPorId(n) {
      return (
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/" +
        n +
        ".svg"
      );
    },

    obtenerAleatorio(tope) {
      return Math.floor(Math.random() * tope) + 1;
    },

    obtenerImagenAleatorios(b) {
      if (b) {
        let n = this.obtenerAleatorio(4);
        console.log(this.obtenerAleatorio(n));
        console.log(this.arreglo[n]);
        return this.obtenerImgPorId(this.arreglo[n]);
      } else {
        return "https://d3ugyf2ht6aenh.cloudfront.net/stores/001/294/321/products/plato-hondo-oxford-negro11-bdbc3ccd7e0f62726516153202085146-1024-1024.jpg";
      }
    },

    jugar(){
      var img1 = document.getElementById("img1").src
      var img2 = document.getElementById("img2").src
       var img3 = document.getElementById("img3").src

      if(img1==img2 && img1==img3){
        this.puntuacion= this.puntuacion+5
      }
      this.comparacion=true
      this.intentosN--
    }
  },
};
</script>

<style>
.contenedor {
  display: inline-flex;
}
.cont1 {
  display: grid;
  margin-right: 30px;
}

img {
  width: 150px;
  height: 150px;
}

#jugar {
  margin-top: 20px;
  height: 40px;
  width: 140px;
  border: solid;
}
</style>