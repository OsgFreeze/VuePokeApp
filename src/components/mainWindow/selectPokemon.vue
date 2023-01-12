<template>

  <div class="MainWindow"> 
  <div> Pokemon Nummer eingeben: 
    <input id="eingabeId" type="number" v-model="PokeNumber"> </div>

  <div class="buttons">
    <button @click="setNrPlus" > Hoch ↑ </button>
    <button @click="setNrMinus"> Runter ↓ </button> 
    &nbsp;
    <button @click="loadApiNumber"> Eingabe </button>
  </div>

  <div class="pokeStats">
    {{myPokemonName}}
    <span v-if="weight">{{weight}}kg </span>
    <p v-for="(pokemonStat, index) in pokemonStats" :key="index">{{pokemonStat.name}} = {{pokemonStat.base_stat}}</p>
    <img class="imagePokeFront" :src="pokePicture.front_default" v-if="pokeLoaded" />
    <img class="imagePokeBack" :src="pokePicture.back_default" v-if="pokeLoaded" />
  </div>

</div>

</template>

<script>
import axios from 'axios'

export default {
  name: 'selectPokemon',
  components: {
    
  }, 
  data(){ 
    return {
      active: false,
      PokeName: null,
      PokeNumber: 1,
      myPokemonName: "",
      pokemonStats: [],
      weight: null,
      pokeLoaded: null,
      pokePicture: {}
    }
  },
  methods: {
    async loadApiNumber(){
      await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
        console.log(response);
        const data = response.data;
        this.pokeLoaded = true;
        this.myPokemonName = data.name;
        this.pokemonStats =  data.stats;
        this.weight = data.weight;
        this.pokePicture = data.sprites; //Bilder
      })
    },
    
    async setNrPlus(){
      this.PokeNumber = (this.PokeNumber + 1);
      this.loadApiNumber();
 
    },
    async setNrMinus(){
      this.PokeNumber = (this.PokeNumber - 1);
      this.loadApiNumber();
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
  font-family: sans-serif;
}
.MainWindow {
  background-color: aquamarine;
}
.scrollBar {
  background-color:lightgray;
  list-style: none;
  height: 100px;
  width: 800px;
  overflow: scroll;
  padding: 12px 20px;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
  0 8px 10px -6px rgb( 0 0 0 / 0.1);
}
.pokeStats {
  padding-top: 16px;
  color: rgb(77, 76, 76);
  font-family: sans-serif;
}
.imagePokeFront {
  height: 150px;
  width: 150px;
  font-family: sans-serif;
  background-color: rgba(26, 97, 204, 0.075);
}
.imagePokeBack {
  height: 150px;
  width: 150px;
  background-color: rgba(211, 80, 57, 0.075);
}
</style>
