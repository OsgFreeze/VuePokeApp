<template>

    <div>   
       diese Information kommt von der Child Component:  {{ this.übergebeneVariable }} 
       <p> </p> <!-- Zeilenumbruch-->
       und das hier wurde in eine local Variable überschrieben:  {{ this.localArray }} 
       <p> </p> <!-- Zeilenumbruch-->
       <button @click="getAttackData"> neuer API Call [API/Moves] </button>         <!-- Button um Attack API Call zu machen-->
       <p> name: {{this.attackName}} , damage: {{ this.baseDamage }} </p>           <!-- API Data anzeigen -->
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
       attackName: "[none]",
       localArray: [],
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus [Attacken aus übergeben Array]
        this.localArray = this.übergebeneVariable //übergebener Array Local Speichern
        for(let i=0; i<5; i++) {                                                                         
          await axios.get(`https://pokeapi.co/api/v2/move/${this.localArray[i]}`).then((response) => {  
          const data = response.data;
          console.log(data);
          
          this.baseDamage = data.power;
          this.attackName = data.name;
          })
        }
      }
    }
  } 
   
  
  </script>
  
  <style>
  </style>
  