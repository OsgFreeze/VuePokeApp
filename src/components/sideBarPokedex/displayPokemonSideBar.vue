<template>
  <h1>Pokedex  {{result}}</h1>
  <div class="Top">
    <form class="testing">
      <input class="Searchbar" type="number" v-model="suche" min="1" max="1010" >
      <button class="SuchButton" @click="getApi" type="button">Suche</button>
    </form>
  </div>

  <div class="test"> 
     
    <div v-if="trueFalse === true">
      <img class="pokemonPicture" :src="pokemonSprite.other.home.front_shiny" /> <br>
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
  .pokemonPicture {
    height: 200px;
    width: 200px;
  }
  
  .test{
    background-color: #78ad75;
    margin-left: 5%;
    width: 45%;
    height: 80%;
    overflow: scroll;
    float: left;
  }

  .test::-webkit-scrollbar{
    display: none;
  }

  .Searchbar{
    height: 2%;
    width: 70%;
    float: left;
  }
  
  .SuchButton{
    height: 2%;
    width: 20%;
    float: right;
  }

  .Top{
    margin-left: 5%;
    margin-right: 5%;
    background-color: blue;
  }
  .testing{
    background-color: black;
  }

</style>