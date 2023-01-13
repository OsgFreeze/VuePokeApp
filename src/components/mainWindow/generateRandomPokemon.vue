<template class="generateRandomPokemon">
    <div>   
      <button @click="generateRandomPokemon(898)"> generateRandomPokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
      <p v-if="visible" class="randomPokemon"> 
         {{this.randomPokemonObject.name}} 
         <img class="randomPokemonPicture" :src="this.randomPokemonObject.sprites.other.home.front_default" />
      </p>
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

    .randomPokemon{
        position:relative;
    }

    .randomPokemonPicture{
        position: absolute;
    }
   
  </style>