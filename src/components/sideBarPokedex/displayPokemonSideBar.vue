<template>
  <h1>Pokedex</h1>

  <div class="SearchDesign">
      <input class="Searchbar" type="text" v-model="suche" min="1" max="898" > <!-- Suchleiste die von 1 bis 1010 geht da es nur 1010 Pokemon gibt-->
      <button class="SuchButton" @click="getApi(0)" type="button">Suche</button> <!-- Button ruft die getApi Methode auf -->
  </div>

  <div class="DisplayLeftSide"> 
    <div v-if="pokeDatenAnzeigen === true"> <!-- if Abfrage weil Api Call noch nicht gemacht worden ist -->
      <div class>
        <img class="PokemonPicture" :src="pokemonSprite.other.home.front_shiny" /> <br> <!-- Bild wird eingeblendet -->
      </div>
    </div>
  </div>

   <showTypeRelations :pokeDatenAnzeigenCompare="this.pokeDatenAnzeigen" :übergabePokeObjekt="this.pokemonObjekt"/> <!-- übergabe vom PokemonObjekt und Boolean damit comparePokemon die daten speichern und anzeigen kann -->  
</template>
  
  
<script>
import axios from 'axios'
import showTypeRelations from './showTypeRelations.vue'
  export default {
    components: {
      showTypeRelations,
    }, 
    data(){ 
      return {
        suche: 1,
        pokemonObjekt:{},
        
        pokemonNameL: "",
        pokemonSpriteL: {},
        baseStatsL: [],
        pokemonTypL: [],
        heightL: 0,
        weightL: 0,

        pokemonNameR: "",
        pokemonSpriteR: {},
        baseStatsR: [],
        pokemonTypR: [],
        heightR: 0,
        weightR: 0,

        pokeDatenAnzeigenLeft: false,
        pokeDatenAnzeigenRight: false,

        result:"",

        pokemonTypes: {
          pokemonTypL: [],
          pokemonTypR: [],
        }

      }
    },
    methods: {
      
      async getApi(indicator){ //API Call um Pokemon Daten zu bekommen
        await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.suche}`).then((response) => {
          console.log(response)
          this.pokemonObjekt = response.data;
          
          if(indicator < 1){
            this.pokemonNameL = this.pokemonObjekt.name;
            this.pokemonSpriteL = this.pokemonObjekt.sprites;
            this.baseStatsL = this.pokemonObjekt.stats;
            this.pokemonTypL = this.pokemonObjekt.types;
            this.pokemonTypes.pokemonTypL = this.pokemonTypL;
            this.heightL = this.pokemonObjekt.height;
            this.weightL = this.pokemonObjekt.weight;
            this.pokeDatenAnzeigenLeft = true;
          }else{
            this.pokemonNameR = this.pokemonObjekt.name;
            this.pokemonSpriteR = this.pokemonObjekt.sprites;
            this.baseStatsR = this.pokemonObjekt.stats;
            this.pokemonTypR = this.pokemonObjekt.types;
            this.pokemonTypes.pokemonTypR = this.pokemonTypR;
            this.heightR = this.pokemonObjekt.height;
            this.weightR = this.pokemonObjekt.weight;
            this.pokeDatenAnzeigenRight = true;
          }

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
  
  .compareButtonDiv{
    margin-top: 10px;
    margin-bottom: 10px;
    text-align: center;
   }

  .DisplayPokemon{
    display: flex;
    flex-direction: row;
    height: 600px;
    width: 100%;
  }

    .DisplayLeftSide{
      background-color: rgba(35, 212, 138, 0.397);
      margin-left: 5%;
      margin-right: 5px;
      width: 45%;
      height: 100%;
      border: solid;
    }

      .PokemonDataLeft{
        text-align: left;
        margin-left: 5px;
        text-transform: capitalize;
      }

    .DisplayRightSide{
      background-color: rgba(255, 146, 4, 0.397);
      margin-right: 5%;
      margin-left: 5px;
      width: 45%;
      height: 100%;
      border: solid;
    }

    .PokemonDataRight{
      text-align: left;
      margin-left: 5px;
      text-transform: capitalize;
    }

  .Searchbar{
    height: 15px;
    width: 70%;
    float: left;
  }
  
  .SuchButton{
    height: 100%;
    width: 20%;
    float: right;
  }

  .SearchDesign{
    margin-left: 5%;
    margin-right: 5%;
  }

  .PokemonPicture{
    height: 200px;
    width: 200px;
  }
 
  

</style>