<template>
    <div>   
      <audio controls="true" autoplay="off" volume="0.01" loop="false" >    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" --> 
    </div>

    <button @click="showPokeInfo"> Press me for Fight Window -> übergebene Pokemon Data </button>

    <div class="displayPokemonData" > 

    </div>

  </template>
  
  <script>

  export default {
    name: 'fightWindow',
    components: {  
    }, 
    props: ['übergebenePokemon'],
    data(){ 
      return {
        data: {

        }
      }
    },
    methods: {
      showPokeInfo() {

        console.log("diese Info kommt von der fightWindow Komponente: ");
        console.log(this.übergebenePokemon);
      }
    }

  } 
  
  </script>
  
  <style>

.fightWindowPicture{
  width: 200px;
  height: 200px;
}
  </style>