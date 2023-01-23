<template>
  <h1>Pokedex  {{result}}</h1>
  <div class="SearchDesign">
      <input class="Searchbar" type="text" v-model="suche" min="1" max="898" > <!-- Suchleiste die von 1 bis 1010 geht da es nur 1010 Pokemon gibt-->
      <button class="SuchButton" @click="getApi" type="button">Suche</button> <!-- Button ruft die getApi Methode auf -->
  </div>

  <div class="DisplayLeftSide"> 
     
    <div v-if="pokeDatenAnzeigen === true"> <!-- if Abfrage weil Api Call noch nicht gemacht worden ist -->
      <div class>
        <img class="PokemonPicture" :src="pokemonSprite.other.home.front_shiny" /> <br> <!-- Bild wird eingeblendet -->
      </div>
      Name: {{ pokemonName }} <br>
      Stats:
      <p v-for="(stats, zähler) in baseStats" :key="zähler">{{ stats.stat.name }} : {{ stats.base_stat }}</p>  
      Kommt in diesen Spielen vor: <p v-for="(version, zähler) in genName" :key="zähler">{{ zähler + 1 }} : {{version.version.name}}</p> <!-- Mit For Schleife läuft man durch das Objekt oder durch die Liste um alle Daten anzuzeigen -->
      <p v-for="(typ, zähler) in pokemonTyp" :key="zähler">Typ {{zähler + 1}} : {{typ.type.name}}</p>
    </div>

  </div>

   <comparePokemon :pokeDatenAnzeigen="this.pokeDatenAnzeigen" :übergabePokeObjekt="this.pokemonObjekt"/> <!-- übergabe vom PokemonObjekt und Boolean damit comparePokemon die daten speichern und anzeigen kann -->  
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
        pokeDatenAnzeigen: false,
        baseStats: [],
        pokemonTyp: [],

        result:"",
      }
    },
    methods: {
      emitTest(data){
          this.result = data;
      },
      async getApi(){ //API Call um Pokemon Daten zu bekommen
        await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.suche}`).then((response) => {
          console.log(response)
          this.pokemonObjekt = response.data;
          this.pokemonName = this.pokemonObjekt.name;
          this.genName = this.pokemonObjekt.game_indices;
          this.pokemonSprite = this.pokemonObjekt.sprites;
          this.baseStats = this.pokemonObjekt.stats;
          this.pokemonTyp = this.pokemonObjekt.types;
          this.pokeDatenAnzeigen = true;
        })
      },
    }
}
</script>
  
<style>
  .PokemonPicture {
    height: 200px;
    width: 200px;
  }
  
  .DisplayLeftSide{
    background-color: #78ad75;
    margin-left: 5%;
    width: 45%;
    height: 80%;
    overflow: scroll;
    float: left;
  }

  .DisplayLeftSide::-webkit-scrollbar{
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

  .SearchDesign{
    margin-left: 5%;
    margin-right: 5%;
    background-color: blue;
    background-color: black;
  }
</style>