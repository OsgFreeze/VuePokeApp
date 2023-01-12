<template>

    <div>   
       diese Information kommt von der Child Component:  {{ übergebeneVariable }} <!-- sollte Pokedex Nummer von Parent übertragen sein-->
       <button @click="getAttackData"> Attacken Daten von API aufrufen </button>
       <p> name: {{this.attackName}} , damage: {{ this.baseDamage }} </p>
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
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus [Attacken aus Array] *funktioniert noch nicht -> aktuell wird nur pokemonNumber genommen*
      await axios.get(`https://pokeapi.co/api/v2/move/${this.übergebeneVariable}`).then((response) => { 
        console.log(response);
        const data = response.data;
        this.baseDamage = data.power;
        this.attackName = data.name;
      })
    },
    }
  } 
  
  </script>
  
  <style>
  </style>
  