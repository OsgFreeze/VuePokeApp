<template class>
  <div> 
      
    sidebar: [Empty] 
    <h1>Suche</h1>
    <p>ergebnis</p>{{ result }}

    <input type="number" v-model="suche" min="1" max="1010" >
    <button @click="getApi" type="button">Suche</button>
     
    <div v-if="trueFalse === true">
      <img class="PokemonPicture" :src="pokemonSprite.other.home.front_shiny" /> <br>
      Name: {{ pokemonName }} <br>
      Stats:
      <p v-for="(stats, zähler) in baseStats" :key="zähler">{{ stats.stat.name }} : {{ stats.base_stat }}</p>  
      Kommt in diesen Spielen vor: <p v-for="(version, zähler) in genName" :key="zähler">{{ zähler + 1 }} : {{version.version.name}}</p>
    </div>

  </div>
   <comparePokemon @parentAufruf="emitTest($event)" :übergebenesBoolean="this.trueFalse" :übergabePokeObjekt="this.pokemonObjekt"/>


  
</template>
  
<script>
import axios from 'axios'
import comparePokemon from './comparePokemon.vue'
  export default {
    components: {
      comparePokemon,
    }, 
    data(){ 
      return {
        suche: 1,
        test: [],
        pokemonObjekt:{},
        pokemonName: "",
        genName: [],
        pokemonSprite: {},
        trueFalse: false,
        baseStats: [],

        result:"",
      }
    },
    methods: {
      emitTest(data){
          this.result = data;
      },
      async getApi(){ //API Call für Pokemon Daten
        await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.suche}`).then((response) => {
          this.pokemonObjekt = response.data;
          this.pokemonName = this.pokemonObjekt.name;
          this.genName = this.pokemonObjekt.game_indices;
          this.pokemonSprite = this.pokemonObjekt.sprites;
          this.baseStats = this.pokemonObjekt.stats;
          this.trueFalse = true;
        })
      },
    }
}
</script>
  
<style>
  .PokemonPicture {
    height: 250px;
    width: 250px;
  }
</style>