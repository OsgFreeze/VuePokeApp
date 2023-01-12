<template>

    <div>   
       unterklasse:  {{ übergebeneVariable }} <!-- sollte Pokedex Nummer von Pokemon sein-->
       <button @click="getAttackData"> pokemon Attack Data Print in consol</button>
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
       // AttackNumber: 1,
      }
    },

    methods: {
      async getAttackData(){
      await axios.get(`https://pokeapi.co/api/v2/move/${this.übergebeneVariable}`).then((response) => { //movename oder Nummer
        console.log(response);
      })
    },
    }
  } 
  
  </script>
  
  <style>
  </style>
  