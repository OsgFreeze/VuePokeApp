<template>

    <div>   
       unterklasse:  {{ übergebeneVariable }} <!-- sollte Pokedex Nummer von Pokemon sein-->
       <button @click="getAttackData"> pokemon Attack Data Print in consol</button>

       <p v-if="visible"> der Attackenschaden lautet: {{ this.baseDamage }}</p>
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
       visible: false,
      }
    },

    methods: {
      async getAttackData(){
      await axios.get(`https://pokeapi.co/api/v2/move/${this.übergebeneVariable}`).then((response) => { //movename oder Nummer
        console.log(response);
        const data = response.data;
        this.baseDamage = data.power;
        console.log(data.name, data.power);
        this.setVisible=true;
      })
    },
    }
  } 
  
  </script>
  
  <style>
  </style>
  