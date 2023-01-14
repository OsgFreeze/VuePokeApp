<template>
    <div class="myFightWindow">   
      <audio controls="off" autoplay="on" volume="0.01" loop="true" >    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" --> 
    </div>

    <button @click="showPokeInfo"> Press me for PokeData </button>

    <div class="displayPokemonData"> 
        
    </div>
  </template>
  
  <script>

  export default {
    name: 'fightWindow',
    components: {  
    }, 
    props: ['beideÜbergebenenPokemonDaten'], 
    data(){ 
      return {
        data: {
          Pokemon: [],
          enemyPokemon: [],
        }
      }
    },
    methods: {
      showPokeInfo() {
        console.log("diese Info kommt von der fightWindow Komponente: ");
        //console.log(this.beideÜbergebenenPokemonDaten);
        this.Pokemon=this.beideÜbergebenenPokemonDaten[0];
        this.enemyPokemon=this.beideÜbergebenenPokemonDaten[1];

        console.log("main Pokemon: " + this.Pokemon);
        console.log("random Pokemon: " + this.enemyPokemon);

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