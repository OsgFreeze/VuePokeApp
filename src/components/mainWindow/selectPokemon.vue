<template class="selectPokemonTemplate">

  <div class="eingabeFeld"> Pokemon Nummer eingeben:  <!-- Pokemon Nummer von Pokedex eingeben & API-Methodenaufruf -->
    <input type="number" v-model="PokeNumber"> 
    <button @click="setNrPlus" > Hoch ↑ </button>
    <button @click="setNrMinus"> Runter ↓ </button>  &nbsp;
    <button @click="loadApiNumber"> Zahl Übermitteln </button>
  </div>

  <div class="dataAusgabeFeld"> <!-- gibt alle Informationen über das Pokemon aus -->
    <div v-if="pokeLoaded"> 
      <p class="baseStats" v-for="(pokemonStat, index) in pokemonStats" :key="index"> {{pokemonObject.stats[index].stat.name}} = {{pokemonStat.base_stat}}</p>
      <img class="PokemonPictureHD" :src="pokemonObject.sprites.other.home.front_default"/> 
      <img class="PokemonPictureHD" :src="pokemonObject.sprites.other.home.front_shiny"/>
    </div>
  </div>

  <selectAttack class="selecAttack" :übergebenesPokemonObjekt="this.pokemonObject" /> <!-- übergebe Daten zur Attacken Auswahl -->

</template>

<script>
import axios from 'axios'
import selectAttack from './selectAttack.vue';

export default {
  name: 'selectPokemon',
  components: {
    selectAttack, 
  }, 

  data(){  
    return {
      pokeLoaded: false,  //  Pokemondaten bereits geladen ?
      PokeNumber: 1,      //  startwert für Eingabefeld
      pokemonStats: [],   //  speichert die basis Werte von einem Pokemon
      pokemonObject: {},  //  speichert die Pokemon Daten von dem API Call
    }
  },

  methods: { 
    async loadApiNumber(){ //API Call für Pokemon Daten
      await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
        this.pokemonObject = response.data; //Pokemon Objekt in variable "pokemonObject" Speichern
        this.pokemonStats =  this.pokemonObject.stats;
        this.pokeLoaded = true;
        
        console.log("pokemon API Daten: "); 
        console.log(response); //pokemon data Ausgeben
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
.baseStats{
  background-color:rgba(0, 174, 255, 0.295);
}
.PokemonPictureHD {
  height: 250px;
  width: 250px;
}

.selecAttack {
  background-color:red;
}
</style>
