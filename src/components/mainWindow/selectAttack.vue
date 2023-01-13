<template>

    <div>   
       diese Information kommt von der Parent Component: {{this.übergebenesObject.name}} <!--{{ this.übergebenesObject }} -->

       <p> </p> <!-- Zeilenumbruch-->

       <button @click="getAttackData"> child Methode ausführen </button>   <!-- Button um Attack API Call zu machen-->

       <p v-if="visible"> {{this.objektArray}} </p>
    </div>

  </template>
  
  <script>
  import axios from 'axios'

  export default {
    name: 'selectAttack',
    components: {  
    }, 
    props: ['übergebenesObject'], //übergebenes pokemonObjekt von parent
    data(){ 
      return {
       baseDamage: 0,
       attackName: "[none]",
       visible: false,
       anzahlLernbareAttacken: 0,
       attackURL: "",
       objektArray: [],
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus 
      
        const Pokedata = this.übergebenesObject;                                      //objekt.daten in Pokedata speichern                                    funktioniert ✓
        this.anzahlLernbareAttacken = Pokedata.moves.length;                          //local variable für Anzahl lernbare Attacken                           funktioniert ✓

        for (let i=0; i < this.anzahlLernbareAttacken; i++) {                         //durchläuft so oft wie viele Attacken ein Pokemon lernen kann.         funktioniert ✓

          this.attackURL = this.Pokedata.moves[i]
          console.log(this.attackURL);
         // await axios.get(this.attackURL).then((response) => {                      //gibt API Daten über übergebenesObjekt.Attack.url[i] aus
          await axios.get('https://pokeapi.co/api/v2/move/5').then((response) => {    //gibt API Daten über Attacke 5 aus
          const attackData = response.data; 
          console.log(attackData);

          //this.baseDamage = attackData.power; //gibt jetzt nur Attacke nr 5 aus ? 
          //this.attackName = Attackdata.name;
          }) 
       }
      }
    }
  
  } 
  
  </script>
  
  <style>
  </style>
  