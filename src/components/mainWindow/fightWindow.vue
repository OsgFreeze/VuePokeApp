<template>
    <div class="myFightWindow">   
      <p> fight Window {{uebergebenesPokemon}} </p>  
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
  background-color: rgb(231, 221, 83);
}
  </style>