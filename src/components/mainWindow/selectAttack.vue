<template>
    <div>   
       diese Information kommt von der Parent Component: {{this.übergebenesObject.name}} <!--{{ this.übergebenesObject }} -->

       <p> </p> <!-- Zeilenumbruch-->

       <button @click="getAttackData"> Pokemon Attacken anzeigen </button>   <!-- Button um Attack API Call zu machen-->

       <p> Ausgewählte Attacken: {{ chosenAttacks }} </p>

       <p v-for="(Attackobjekt, index) in AttackobjektArray" :key="index"> 
        {{index}} 
        <button @click="selectAttack(index)"> {{Attackobjekt.name}} {{Attackobjekt.accuracy}} </button> 
      </p>
       
      <generateRandomPokemon class="generateRandomPokemon"/>

    </div>
  </template>
  
  <script>
  import axios from 'axios'

  import generateRandomPokemon from './generateRandomPokemon.vue';

  export default {
    name: 'selectAttack',
    components: {  
      generateRandomPokemon
    }, 
    props: ['übergebenesObject'], //übergebenes pokemonObjekt von parent
    data(){ 
      return {
       baseDamage: 0,
       attackName: "[none]",
       visible: false,
       anzahlLernbareAttacken: 0,
       AttackobjektArray: [],
       chosenAttacks: [null],
       arrayindex: 0
      }
    },

    methods: {
      async getAttackData(){                                               //gibt Informationen zu den übergebenen lernbaren Attacken aus 
      
        const Pokedata = this.übergebenesObject;                           //objekt.daten in Pokedata speichern                                    funktioniert ✓
        this.anzahlLernbareAttacken = Pokedata.moves.length;               //local variable für Anzahl lernbare Attacken                           funktioniert ✓

        for (let i=0; i < this.anzahlLernbareAttacken; i++) {              //durchläuft so oft wie viele Attacken ein Pokemon lernen kann.         funktioniert ✓
          let attackURL = Pokedata.moves[i].move.url                     
          await axios.get(attackURL).then((response) => {    
          const attackData = response.data; 
          this.AttackobjektArray[i] = attackData;
          })

          } 
          this.visible=true;
       },

       selectAttack(number){
          
          
          this.chosenAttacks[this.arrayindex]= number;
          this.arrayindex++;
       }

      }
    }
  
   
  
  </script>
  
  <style>

  .fightwindow{
    background-color: rgba(13, 184, 236, 0.479);
  }

  .generateRandomPokemon {
   visibility: visible;
  }
  </style>
  