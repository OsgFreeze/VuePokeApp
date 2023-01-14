<template class="generateRandomPokemon">
    <div>   
        <button @click="generateRandomPokemon(898)"> generateRandomPokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
        
        <div v-if="visible" class="randomPokemon"> 
             <img class="randomPokemonPicture" :src="this.randomPokemonObject.sprites.other.home.front_default" />
             <fightWindow class="fightwindow">  </fightWindow>
        </div>
    </div>

  </template>
  
  <script>
  import axios from 'axios';
  import fightWindow from './fightWindow.vue';

  export default {
    name: 'generateRandomPokemon',
    components: {  
      fightWindow,
    }, 
    //props: ['uebergebenesPokemon'], 
    
    data(){ 
      return {
        randomPokemonObject: {},
        AttackobjektArray: [],
        newAttackArray: [],
        fourAttackArray: [],
        visible: false,
      }
    },

    methods: {    
        async generateRandomPokemon(obergrenze){   
            let untergrenze=1;
            let randomNumber  = Math.floor(Math.random() * (obergrenze - untergrenze + 1)) + 1;   // *nur* diese Zeile wurde online nachgeschaut  ->  [nicht selbst gecoded!]
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => { 
            this.randomPokemonObject = response.data; 
            this.visible=true;
            console.log("Pokemon Name: " + this.randomPokemonObject.name); 
            this.sortAttacksFromRandomPokemon(); //andere Methode aufrufen
            })
        }, 
 
        
        async sortAttacksFromRandomPokemon(){                                  
          const Pokedata = this.randomPokemonObject;                           
          this.anzahlLernbareAttacken = Pokedata.moves.length;  
          console.log("anzahl lernbare Attacken: " + this.anzahlLernbareAttacken);            

          for (let i=0; i < this.anzahlLernbareAttacken; i++) {             
            let attackUrlFromIndex = Pokedata.moves[i].move.url                     
            await axios.get(attackUrlFromIndex).then((response) => {    
              const attackData = response.data; 
              this.AttackobjektArray[i] = attackData; 
              //console.log(this.AttackobjektArray[i]);
            })
          }     
          this.filterAttacksFromByDamage(); //andere Methode aufrufen
        },
     

        async filterAttacksFromByDamage(){        
          let b= 0;
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {  
            if(this.AttackobjektArray[i].power > 0 ){   //speichert alle Attackeninformationen die Schaden machen in neuen Array
              this.newAttackArray[b] = this.AttackobjektArray[i]; 
              b++;
            }     
          } 
          console.log(this.newAttackArray); 
          this.SelectFourRandomAttacks(); //andere Methode aufrufen
        },


        async SelectFourRandomAttacks(){   
          let untergrenze=0;
          let anzahlSchadensAttacken = 30; //funktioniert so ned: this.newAttackArray.length
          for (let i=0; i < 4; i++) {
            let randomNumber  = Math.floor(Math.random() * (anzahlSchadensAttacken - untergrenze + 1)) + 1;  
            this.fourAttackArray[i] = this.newAttackArray[randomNumber];
            //console.log("   random_Number: " + randomNumber);
            console.log("     Attack Name: " + this.fourAttackArray[i].name + "    Damage: " + this.fourAttackArray[i].power);
            "->%d<-\n", i
          }
           // if(this.AttackobjektArray[i].power > 0 ){   //speichert alle Attackeninformationen die Schaden machen in neuen Array
           //   this.newAttackArray = this.AttackobjektArray[i]; 
           // }      
        }
      }
    }
  </script>
  
  <style>
    .generateRandomPokemon{
        visibility: visible;  /* kann in Parent und Child gemacht werden */
        background-color: rgba(46, 167, 204, 0.726)
    }
    .randomPokemonPicture{
        height: 250px;
        width: 250px;
        background-color: rgba(46, 167, 204, 0.726)
    }
   
  </style>