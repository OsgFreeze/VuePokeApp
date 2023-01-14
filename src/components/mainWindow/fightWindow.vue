<template>
    <div class="myFightWindow">   
      <audio controls="true" autoplay="off" volume="0.01" loop="false" >    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" --> 
    </div>

    <button @click="showPokeInfo"> Press me for PokeData </button>

    <div class="displayPokemonData" v-if="this.pokemonDataLoaded"> 

      {{randomPokemon.name}}
    </div>
  </template>
  
  <script>

  export default {
    name: 'fightWindow',
    components: {  
    }, 
    props: ['beideÜbergebenenPokemonDaten', 'randomPokemon'],
    data(){ 
      return {
        data: {
          Pokemon: {},
          enemyPokemon: {},
          pokemonDataLoaded: false,
        }
      }
    },
    methods: {
      showPokeInfo() {
        console.log("diese Info kommt von der fightWindow Komponente: ");
        //console.log(this.beideÜbergebenenPokemonDaten);

        console.log("main Pokemon: " + this.beideÜbergebenenPokemonDaten.pokemon1);
        console.log("random Pokemon: " + this.beideÜbergebenenPokemonDaten.pokemon2);
        this.pokemonDataLoaded = true;

      }
    }

  } 
  
  </script>
  
  <style>
.myFightWindow{
  width: 100;
  height: 50;
  background-color: rgb(255, 127, 80);

}

.fightWindowPicture{
  width: 200px;
  height: 200px;
}
  </style>