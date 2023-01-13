<template>

    <div>   
       diese Information kommt von der Child Component:  {{ this.übergebeneVariable }} 
       <p> </p> <!-- Zeilenumbruch-->
       und das hier wurde in eine local  {{ this.localArray }} 
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
        for(let i=0; i<5; i++) {                                                                                  //durchläuft so oft wie der Array.length von allen lernbaren Attacks
          this.localArray = this.übergebeneVariable                                                               //übergebener Array Local Speichern
          await axios.get(`https://pokeapi.co/api/v2/move/${this.localArray[i]}`).then((response) => {    //Api Call
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
  