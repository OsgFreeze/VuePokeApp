<template>
    <div>   
      <button @click="getAttackData"> Pokemon Attacken anzeigen </button> 

      <div v-if="attackVisible">  <!-- attacken Auuswahlfenster -->
        <p v-for="(Attackobjekt, index) in AttackobjektArray" :key="index"> 
          {{index}} 
          <button @click="selectAttack(index)"> Name: {{Attackobjekt.name}} Power: {{Attackobjekt.power}} genauigkeit: {{Attackobjekt.accuracy}} </button> 
        </p> 
      </div> 

      <generateRandomPokemon class="generateRandomPokemon" :übergebenesfinalPokemonWithAttackArray="this.finalPokemonWithAttackArray"        />

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
    props: ['übergebenesPokemonObjekt', 'testObjekt'], //übergebenes pokemonObjekt von parent
    data(){ 
      return {
        baseDamage: 0,
        arrayindex: 0,
        anzahlLernbareAttacken: 0,

        attackName: "[none]",
        visible: false,
        attackVisible: true,
        allAttacksChosen: false,

        pokemonObject: { 
          pokemonData: {},   //     Bsp:  {name: "hubert", size:40}
          attackData: {}
        },

        AttackobjektArray: [],
        newAttackArray: [],
        chosenAttacks: [null],
        finalPokemonWithAttackArray: [], //hier werden die Pokemon Daten + Informationen zu den 4 Ausgewählten Attacken gespeichert
      }
    },

    methods: {
      async getAttackData(){                                               //gibt Informationen zu den übergebenen lernbaren Attacken aus 
      
        const Pokedata = this.übergebenesPokemonObjekt;                    //objekt.daten in Pokedata speichern                                    funktioniert ✓
        this.anzahlLernbareAttacken = Pokedata.moves.length;               //local variable für Anzahl lernbare Attacken                           funktioniert ✓

        for (let i=0; i < this.anzahlLernbareAttacken; i++) {              //durchläuft so oft wie viele Attacken ein Pokemon lernen kann.         funktioniert ✓
          let attackURL = Pokedata.moves[i].move.url                     
          await axios.get(attackURL).then((response) => {    
            const attackData = response.data; 
            this.AttackobjektArray[i] = attackData;
          })
        } 
        this.visible=true;
        this.filterAttacksByDamage();
      },
    
      async filterAttacksByDamage(){        
          let b = 0;
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {  
            if((this.AttackobjektArray[i].power > 0) || (this.AttackobjektArray[i].power != null )){   //speichert alle Attackeninformationen die Schaden machen in neuen Array
              this.newAttackArray[b] = this.AttackobjektArray[i]; 
              b++;
              //console.log(this.newAttackArray[b]);
            }     
          } 
          this.pokemonObject.pokemonData = this.übergebenesPokemonObjekt;
        },

      selectAttack(attackNumber){ 
          this.chosenAttacks[this.arrayindex]= this.AttackobjektArray[attackNumber];
          this.arrayindex++;

          if(this.arrayindex==4){ 
            this.pokemonObject.attackData=this.chosenAttacks; //index=1, weil wir die Attackeninformation an Stelle [1, -> 2] zwei im übergabe Array Speichern wollen
            this.allAtacksChosen=true; //zeigt ausgewählte Attacken an.
            this.attackVisible = false;
           }
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
  