<template>
    <div class="myFightWindow">   
      <p> fight Window {{uebergebenesPokemon}} </p>  
      <img class="fightWindowPicture" src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/132.gif">
    </div>

    <div> 
      <audio controls="off" autoplay="on" volume="0.01" loop="true" >    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" -->
    </div>

  </template>
  
  <script>

  export default {
    name: 'fightWindow',
    components: {  
    }, 
    props: ['uebergebenesPokemon'], 
    
    data(){ 
      return {
        data: {}
      }
    },
    methods: {
      getAttacks(){

      },
      getPokeData(){
        this.data = this.ubergebenesObject1;
        console.log(this.data);
      }

    } 
  }

  </script>
  
  <style>
.myFightWindow{
  width: 100;
  background-color: rgba(255, 127, 80, 0.473);

}

.fightWindowPicture{
  width: 200px;
  height: 200px;
}
  </style>