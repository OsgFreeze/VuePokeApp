<template>

    <div>   
       diese Information kommt von der Child Component:  {{ übergebeneVariable }} 
       <p> </p> <!-- Zeilenumbruch-->
       und das hier wurde local übernommen  {{ this.localArray }} 
       <p> </p> <!-- Zeilenumbruch-->
       <button @click="getAttackData"> Attacken Daten von API aufrufen </button>    <!-- Button um Attack API Call zu machen-->
       <p> name: {{this.attackName}} , damage: {{ this.baseDamage }} </p>           <!-- Display API Data -->
    </div>

  </template>
  
  <script>
  import axios from 'axios'

  export default {
    name: 'selectAttack',
    components: {  
    }, 
    props: ['übergebeneVariable'],
    data(){ 
      return {
       baseDamage: 0,
       attackName: "",
       localArray: [],
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus [Attacken aus Array] *funktioniert noch nicht -> aktuell wird nur pokemonNumber genommen*
        //for(let i=0; i<this.localArray.length; i++) {
          this.localArray = this.übergebeneVariable
          await axios.get(`https://pokeapi.co/api/v2/move/${this.localArray[3]}`).then((response) => { 
          console.log("diese Info kommt von dem Child Component: ");
          console.log(response);
          const data = response.data;
          this.baseDamage = data.power;
          this.attackName = data.name;
          })
        }
      }
    //}
  } 
   
  
  </script>
  
  <style>
  </style>
  