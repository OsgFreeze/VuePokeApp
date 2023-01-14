<template>
    <div class="myFightWindow">   
      {{beideÜbergebenenPokemonDaten}}
      <audio controls="off" autoplay="on" volume="0.01" loop="true" >    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" --> 
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
        data: {}
      }
    },
    methods: {
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