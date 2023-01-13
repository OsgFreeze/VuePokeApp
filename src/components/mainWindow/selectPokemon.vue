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
  </div>

  <selectAttack class="selecAttack" :übergebeneVariable="this.testArray"  /> <!-- Springe zu Attackenauswahl Komponente -->
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
      testArray: [2,3,54,63,53,23,76,1,2,4,7,8,34],
      attackNameArray: [],
      
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
        this.anzahlLernbareAttacken = data.moves.length;
       
        
        console.log("anzahl lernbarer Attacken: " +  this.anzahlLernbareAttacken);
        for (let i=0; i < data.moves.length; i++) {
          console.log(data.moves[i].move.name);              //Attackennamen Ausgeben
          this.attackNameArray = data.moves[i].move.name;    //Alle Namen in ein neuen Array speichern
        }  

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
