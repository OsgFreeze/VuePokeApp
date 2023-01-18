<template class="selectPokemonTemplate">

  <div class="eingabeFeld"> Pokemon Nummer eingeben:  <!-- Pokemon Nummer von Pokedex eingeben & API-Methodenaufruf -->
    <input type="number" v-model="PokeNumber"> 
    <button @click="setNrPlus" > Hoch ↑ </button>
    <button @click="setNrMinus"> Runter ↓ </button>  &nbsp;
    <button @click="loadApiNumber"> Zahl Übermitteln </button>
  </div>

  <div class="dataAusgabeFeld"> <!-- gibt alle Informationen über das Pokemon aus -->
    <div v-if="hidePokemonInfos"> 
      <p class="baseStats" v-for="(pokemonStat, index) in pokemonStats" :key="index"> {{pokemonObject.stats[index].stat.name}} = {{pokemonStat.base_stat}}</p>
      <img class="PokemonPictureHD" :src="pokemonObject.sprites.other.home.front_default"/> 
      <img class="PokemonPictureHD" :src="pokemonObject.sprites.other.home.front_shiny"/>
    </div>
  </div>

  <selectAttack :übergebenesPokemonObjekt="this.pokemonObject" /> <!-- übergebe Daten zur Attacken Auswahl -->

</template>

<script>
import axios from 'axios'
import selectAttack from './selectAttack.vue';

export default {
  name: 'selectPokemon',
  components: {
    selectAttack, 
  }, 
  
  provide(){
    const me = this;
    const pokemonHelper = {}

  //Setzt das Game auf started, damit wissen wir, dass der Kampf begonnen hat.
    pokemonHelper.setGameStarted = function(){
      me.gameStarted = true;
    }

    //Macht den Status, ob die Game Information geladen ausgeblendet werden müssen,
    //überall verfügbar.
    //defineProperty verwenden, um einen getter zu verwenden.

    Object.defineProperty(pokemonHelper, "hidePokemonInfos", {
      get: function(){return me.hidePokemonInfos}  
    })
    return {pokemonHelper}
  },

  computed: {
      //Gibt Live Update über den Game Status
      hidePokemonInfos(){
        return !this.gameStarted && this.pokeLoaded;
      }
  },

  data(){  
    return {
      pokeLoaded: false,  //  Pokemondaten bereits geladen ?
      PokeNumber: 1,      //  startwert für Eingabefeld
      pokemonStats: [],   //  speichert die basis Werte von einem Pokemon
      pokemonObject: {},  //  speichert die Pokemon Daten von dem API Call,
      gameStarted: false,
    }
  },

  methods: { 
  //API Call für Pokemon Daten 
    async loadApiNumber(){ 
      await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
        this.pokemonObject = response.data; 
        this.pokemonStats =  this.pokemonObject.stats;
        this.pokeLoaded = true;
      })
    },
    

  //ausgewähltes Pokemon +1 -> danach API CALL  
    async setNrPlus(){ 
      this.PokeNumber = (this.PokeNumber + 1);
      this.loadApiNumber();
    },


  //ausgewähltes Pokemon -1 -> danach API CALL  
    async setNrMinus(){ 
      this.PokeNumber = (this.PokeNumber - 1);
      this.loadApiNumber();
    },
  }
}
</script>

<style>
.PokemonPictureHD {
  height: 250px;
  width: 250px;
}
</style>
