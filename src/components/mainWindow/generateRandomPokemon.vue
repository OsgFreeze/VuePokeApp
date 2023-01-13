<template class="generateRandomPokemon">
    <div>   
        <button @click="generateRandomPokemon(898)"> generateRandomPokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
        <div v-if="visible" class="randomPokemon"> 
             <img class="randomPokemonPicture" :src="this.randomPokemonObject.sprites.other.home.front_default" />
        </div>
    </div>

    <div v-if="visible"> 
        <audio controls="off" autoplay="on" volume="50">    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" -->
    </div>

  </template>
  
  <script>
  import axios from 'axios'

  export default {
    name: 'generateRandomPokemon',
    components: {  
    }, 
    //props: ['uebergebenesPokemon'], 
    
    data(){ 
      return {
        randomPokemonObject: {},
        visible: false,
      }
    },

    methods: {    
        async generateRandomPokemon(number){
            
            let randomNumber  = Math.floor(Math.random() * (number - 1 + 1)) + 1;   // *nur* diese Zeile wurde online nachgeschaut  ->  [nicht selbst gecoded!]
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => { 
            this.randomPokemonObject = response.data; 
            this.visible=true;
            console.log(this.randomPokemonObject.name); //sollte ein Random Pokemon Name ausgeben
            })
        } 
    }
}
  </script>
  
  <style>
    .generateRandomPokemon{
        visibility: visible;  /* kann in Parent und Child gemacht werden */
        background-color: rgba(46, 167, 204, 0.726)
    }
    .randomPokemonPicture{
        height: 250px;
        width: 250px;
    }
   
  </style>