<template class="selectPokemonTemplate">

  <div class="eingabeFeld"> Pokemon Nummer eingeben:  <!-- Pokemon Nummer von Pokedex eingeben & API-Methodenaufruf -->
    <input type="number" v-model="PokeNumber"> 
    <button @click="setNrPlus" > Hoch ↑ </button>
    <button @click="setNrMinus"> Runter ↓ </button>  &nbsp;
    <button @click="loadApiNumber"> Zahl Übermitteln </button>
  </div>

  <div class="dataAusgabeFeld"> <!-- gibt alle Informationen über das Pokemon aus -->
    <span v-if="weight">Aktuell ausgewähltes Pokemon: {{myPokemonName}}  , er wiegt: {{weight}}kg </span>
    <p v-for="(pokemonStat, index) in pokemonStats" :key="index"> {{pokemonObject.stats[index].stat.name}} = {{pokemonStat.base_stat}}</p>
      <div v-if="pokeLoaded"> 
      <img class="PokemonPictureHD" :src="pokePicture.other.home.front_default"/> 
      <img class="PokemonPictureHD" :src="pokePicture.other.home.front_shiny"/>
    </div>
  </div>

  <selectAttack class="selecAttack" :übergebenesObject="this.pokemonObject"  /> <!-- Springe zu Attackenauswahl-Komponente & übergebe Objekt -> {pokemonObject} -->

</template>

<script>
import axios from 'axios'
import selectAttack from './selectAttack.vue';

export default {
  name: 'selectPokemon',
  components: {
    selectAttack, 
  }, 

  data(){  //notwendige lokale Variablen Erstellen
    return {
      active: false,
      PokeName: null,
      PokeNumber: 1,
      myPokemonName: "",
      pokemonStats: [],
      weight: null,
      pokeLoaded: null,
      pokePicture: {},
      learnableAttackNameArray: [],

      pokemonObject: {}
      
    }
  },


  methods: { 
    async loadApiNumber(){ //API Call für Pokemon Daten
      await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
        console.log(response);

        const data = response.data;
        this.pokemonObject = data; //Pokemon Objekt in variable "pokemonObject" Speichern
        this.pokeLoaded = true;

        this.myPokemonName = data.name;
        this.pokemonStats =  data.stats;
        this.weight = data.weight;
        this.pokePicture = data.sprites;
        
      })
    },
    

    async setNrPlus(){ //ausgewähltes Pokemon +1 -> danach API CALL
      this.PokeNumber = (this.PokeNumber + 1);
      this.loadApiNumber();
 
    },
    async setNrMinus(){ //ausgewähltes Pokemon -1 -> danach API CALL
      this.PokeNumber = (this.PokeNumber - 1);
      this.loadApiNumber();
    },
  }
}
</script>

<style>

#selectPokemon {
  font-family: Arial;
}
.PokemonPictureHD {
  height: 250px;
  width: 250px;
}


.selecAttack {
  background-color:red;
}
</style>
