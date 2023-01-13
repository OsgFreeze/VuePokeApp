<template>

    <div>   
       diese Information kommt von der Child Component:  {{ this.übergebeneVariable }} 
       <p> </p> <!-- Zeilenumbruch-->
       und das hier wurde in eine local Variable überschrieben:  {{ this.localArray }} 
       <p> </p> <!-- Zeilenumbruch-->

       <button @click="getAttackData"> neuer API Call [API/Moves] </button>         <!-- Button um Attack API Call zu machen-->
       {{this.attackName}}
               
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
       learnableAttackbyNameArray: [],
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus 
        const Pokedata = this.übergebeneVariable.data;
        this.anzahlLernbareAttacken = Pokedata.moves.length;
        for (let i=0; i < Pokedata.moves.length; i++) {
          this.learnableAttackbyNameArray[i] = this.übergebeneVariable.data.moves[i].move.name;    //Alle Namen in ein neuen Array speichern
                                                               
          await axios.get(`https://pokeapi.co/api/v2/move/${this.localArray[i]}`).then((response) => {  
          const data = response.data;
          console.log(data);
          
          this.baseDamage = data.power; //gibt jetzt nur Attacke nr 5 aus ? 
          this.attackName = data.name;
          })
        }
      }
    }
  } 
   
  
  </script>
  
  <style>
  </style>
  