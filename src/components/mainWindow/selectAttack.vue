<template>

    <div>   
       diese Information kommt von der Child Component: {{übergebenesObject}} <!--{{ this.übergebenesObject }} -->
       <p> </p> <!-- Zeilenumbruch-->
       und das hier wurde in eine local Variable überschrieben:  {{ this.localArray }} 
       <p> </p> <!-- Zeilenumbruch-->

       <button @click="getAttackData"> neuer API Call [API/Moves] </button>   <!-- Button um Attack API Call zu machen-->

       <p> 
        {{this.attackName}} <!--  Attackenname Ausgeben (eigentlich in Schleife mit allen anderen Werten)-->
        {{this.anzahlLernbareAttacken}}
      </p> 

    </div>

  </template>
  
  <script>
  //port axios from 'axios'

  export default {
    name: 'selectAttack',
    components: {  
    }, 
    props: ['übergebenesObject'], //übergebenes pokemonObjekt von parent
    data(){ 
      return {
       baseDamage: 0,
       attackName: "[none]",
       learnableAttackbyNameArray: [],
       anzahlLernbareAttacken: 0,
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus 
        const Pokedata = this.übergebenesObject.data; //objekt.daten in Pokedata speichern
        this.anzahlLernbareAttacken = Pokedata.moves.length;

         for (let i=0; i < anzahlLernbareAttacken; i++) {
         // this.learnableAttackbyNameArray[i] = this.übergebenesObject.data.moves[i].move.name;    //Alle Namen in ein neuen Array speichern
          
         // await axios.get('${this.Pokedata.moves.url}`).then((response) => {  
          //// console.log(Attackdata);
          
          //this.baseDamage = Attackdata.power; //gibt jetzt nur Attacke nr 5 aus ? 
          //this.attackName = Attackdata.name;
         // })
        }
      }
    }
  
  } 
  
  </script>
  
  <style>
  </style>
  