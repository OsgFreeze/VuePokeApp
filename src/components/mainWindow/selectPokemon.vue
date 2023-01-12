<template class="selectPokemonTemplate">

  <div class="eingabeFeld"> Pokemon Nummer eingeben:  <!-- Pokemon Nummer von Pokedex eingeben & API-Methodenaufruf -->
    <input type="number" v-model="PokeNumber"> 
    <button @click="setNrPlus" > Hoch ↑ </button>
    <button @click="setNrMinus"> Runter ↓ </button>  &nbsp;
    <button @click="loadApiNumber"> Zahl Übermitteln </button>
  </div>

  <div class="dataAusgabeFeld"> <!-- gibt alle Informationen über das Pokemon aus -->
    <span v-if="weight">Aktuell ausgewähltes Pokemon: {{myPokemonName}}  , er wiegt: {{weight}}kg   {{this.movePower}} </span>
    <p v-for="(pokemonStat, index) in pokemonStats" :key="index">{{pokemonStat.name}} = {{pokemonStat.base_stat}}</p>
    <img class="imagePokeFront" :src="pokePicture.front_default" v-if="pokeLoaded" />
    <img class="imagePokeBack" :src="pokePicture.back_default" v-if="pokeLoaded" />
    <p v-for="(pokemonStat, index) in pokemonStats" :key="index"> Attackenname = {{pokeLearnedAttacks[index].name}}</p>
  </div>

  <selectAttack class="selecAttack" :übergebeneVariable="this.PokeNumber"  /> <!-- Springe zu Attackenauswahl Komponente -->
</template>

<script>
import axios from 'axios'
import selectAttack from './selectAttack.vue';

export default {
  name: 'selectPokemon',
  components: {
    selectAttack  
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
      pokeLearnedAttacks: [],
    }
  },

  methods: { 
    async loadApiNumber(){ //API Call für Pokemon Daten
      await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
        console.log(response);
        const data = response.data;
        this.pokeLoaded = true;
        this.myPokemonName = data.name;
        this.pokemonStats =  data.stats;
        this.weight = data.weight;
        this.pokePicture = data.sprites; 
        this.pokeLearnedAttacks = data.moves;
        console.log(data.moves);
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
.imagePokeFront {
  height: 150px;
  width: 150px;
}
.imagePokeBack {
  height: 150px;
  width: 150px;
}

.selecAttack {
  background-color:red;
}
</style>
