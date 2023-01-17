<template>
 
  <div>   
      sidebar: [Empty]
  </div>
  <h1>Suche</h1>
  <form>
      <input type="number" v-model="suche" min="1" max="1010" >
      <button @click="getApi" type="button">Suche</button>
      <button @click="getApi2" type="button" :disabled="trueFalse == false">Compare</button>
  </form>

  <div> 
    <!-- Fehlermeldung, kann Pokemon Sprite ja erst Rendern sobald man den Api call macht, muss durch Button oder sowas die API Call methode aufrufen -->

      Name: {{ pokemonName }} <br>
       <!-- Kommt in diesen Spielen vor: <p v-for="(version, index) in genName" :key="index">{{ index + 1 }}  {{version.version.name}}</p> -->
       <!-- <img class="PokemonPicture" :src="pokemonSprite.other.home.front_default" /> -->
  </div>

  <div>
      <br>
        Name:{{ pokemonNameCompare }} 
      <br>
      Kommt in diesen Spielen vor: <p v-for="(version, index) in genNameCompare" :key="index">{{ index + 1 }}  {{version.version.name}}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'displayPokemonSideBar',
  components: {  
  }, 
  data(){ 
    return {
      suche: 1,
      pokemonName: "",
      genName: [],
      pokemonSprite: {},
      
      pokemonNameCompare: "",
      genNameCompare: [],
      trueFalse: false,
    }
  },
  methods: {
      async getApi(){ //API Call für Pokemon Daten
          await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.suche}`).then((response) => {
              console.log(response);
              this.genName = []
              const data = response.data;
              this.pokemonName = data.name;
              this.genName = data.game_indices
              this.trueFalse = true;
              this.pokemonNameCompare= "";
              this.pokemonSprite = data.sprites;
          })
      },

      async getApi2(){ //API Call für Pokemon Daten
          await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.suche}`).then((response) => {
              console.log(response);
              const data = response.data;
              this.genNameCompare = [];
              this.pokemonNameCompare = data.name;
              this.genNameCompare = data.game_indices;
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