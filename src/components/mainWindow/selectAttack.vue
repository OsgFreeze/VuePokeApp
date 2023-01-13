<template>

    <div>   
       diese Information kommt von der Parent Component: {{this.übergebenesObject.name}} <!--{{ this.übergebenesObject }} -->

       <p> </p> <!-- Zeilenumbruch-->

       <button @click="getAttackData"> child Methode ausführen </button>   <!-- Button um Attack API Call zu machen-->

       <p> {{ chosenAttacks }} </p>

       <p v-for="(Attackobjekt, index) in AttackobjektArray" :key="index"> 
        {{index}} 
        <button @click="selectAttack(index)"> {{Attackobjekt.name}} {{Attackobjekt.accuracy}} </button> 
      </p>
       
      <generateRandomPokemon class="generateRandomPokemon"/>
      <fightWindow class="fightwindow" :uebergebenesPokemon="this.anzahlLernbareAttacken" />

    </div>

  </template>
  
  <script>
  import axios from 'axios'
  import fightWindow from './fightWindow.vue';
  import generateRandomPokemon from './generateRandomPokemon.vue';

  export default {
    name: 'selectAttack',
    components: {  
      fightWindow,
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
       chosenAttacks: [],
       arrayindex: 0
      }
    },

    methods: {
      async getAttackData(){ //gibt Informationen zu den übergebenen lernbaren Attacken aus 
      
        const Pokedata = this.übergebenesObject;                                      //objekt.daten in Pokedata speichern                                    funktioniert ✓
        this.anzahlLernbareAttacken = Pokedata.moves.length;                          //local variable für Anzahl lernbare Attacken                           funktioniert ✓

        for (let i=0; i < this.anzahlLernbareAttacken; i++) {                         //durchläuft so oft wie viele Attacken ein Pokemon lernen kann.         funktioniert ✓

          let attackURL = Pokedata.moves[i].move.url
          //await axios.get(this.attackURL).then((response) => {                      //gibt API Daten über übergebenesObjekt.Attack.url[i] aus
          await axios.get(attackURL).then((response) => {    //gibt API Daten über Attacke 5 aus
          const attackData = response.data; 
          this.AttackobjektArray[i] = attackData;
          })
          //this.baseDamage = attackData.power; //gibt jetzt nur Attacke nr 5 aus ? 
          //this.attackName = Attackdata.name;
          } 
          this.visible=true;
       },

       selectAttack(number){
          //console.log(number)
          
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
   visibility: hidden;
  }
  </style>
  