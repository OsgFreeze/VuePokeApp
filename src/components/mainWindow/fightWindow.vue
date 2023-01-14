<template>
    <div class="myFightWindow">   
      <p> fight Window {{uebergebenesPokemon}} </p>  
      <img class="fightWindowPicture" src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/132.gif">
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
  width: 900px;
  height: 550px;
}

.fightWindowPicture{
  width: 200px;
  height: 200px;
}
  </style>