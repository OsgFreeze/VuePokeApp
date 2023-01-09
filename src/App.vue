<template>
  <div class="form">
    <input id="eingabeId" type="number" v-model="number">
  </div>
  <div class="form">
    {{ myPokemonName }} 
    <span v-if="weight">{{weight}}kg </span>
    <p v-for="(pokemonStat, index) in pokemonStats" :key="index">Stat {{index}} = {{pokemonStat.base_stat}}</p>
    <ApiZugriff @click="loadApi"/>
    <img class="pokepic" :src="pokepic.front_shiny" v-if="pokeloaded"/>
  </div>
</template>

<script>
import ApiZugriff from './components/ApiZugriff.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    ApiZugriff
  }, 
  data(){ 
    return {
      number: 0,
      myPokemonName: "",
      pokemonStats: [],
      weight: 0,
      pokepic: {},
      pokeloaded: null,
    }
  },
  methods: {
    async loadApi(){
      console.log("button wurde geklickt");
      await axios.get('https://pokeapi.co/api/v2/pokemon/'+ this.number).then((response) => {
        console.log(response);
        const data = response.data;

        this.myPokemonName = data.name
        this.pokemonStats =  data.stats
        this.weight = data.weight
        this.pokepic = data.sprites
        this.pokeloaded = true
      })
    }
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
}

.pokepic {
  height: 200px;
  width: 200px;
}

.form {
  padding-top: 5%;
  padding-left: 20%;
}

</style>