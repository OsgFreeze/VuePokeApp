<template>
    <div>   
      <p> fight Window </p>  <!--{{ übergebenesObject2 }}-->
      <button @click="getPokeData"> Arschfick </button>
    </div>

  </template>
  
  <script>

  export default {
    name: 'fightWindow',
    components: {  
    }, 
    props: ['ubergebenesObject1'], //'übergebenesObject2'],
    
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
  </style>