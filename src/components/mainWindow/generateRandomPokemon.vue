<template class="generateRandomPokemon">
    <div>   
      <button @click="generateRandomPokemon(898)"> generateRandomPokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
      <div v-if="visible" class="randomPokemon"> <!-- Zeigt Information über das zufällig ausgewählte Pokemon -->
          <img class="randomPokemonPicture" :src="this.randomPokemonObject.sprites.other.home.front_default" />
          <fightWindow class="fightwindow"/>
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
    props: ['übergebenesPokemonObject'], 
    
    data(){ 
      return {
        randomPokemonObject: {},
        AttackobjektArray: [],
        newAttackArray: [],
        fourAttackArray: [],
        finalRandomPokemonWithAttackArray: [],

        twoCompletePokemon: {
          pokemon1: [], 
          pokemon2: [],
        }, 

        visible: false,
        newAttackArrayLength: 0,
      }
    },



    methods: {    
        async generateRandomPokemon(obergrenze){   //erzeugt ein random Pokemon Object
            let untergrenze=1;
            let randomNumber  = Math.floor(Math.random() * (obergrenze - untergrenze + 1)) + 1;  
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => { 
            this.randomPokemonObject = response.data; 
            this.sortAttacksFromRandomPokemon(); 
            })
        }, 
 
        async sortAttacksFromRandomPokemon(){   //erstellt ein neuen Attacken Array                            
          const Pokedata = this.randomPokemonObject;                           
          this.anzahlLernbareAttacken = Pokedata.moves.length;  
          console.log("anzahl lernbare Attacken: " + this.anzahlLernbareAttacken);  

          for (let i=0; i < this.anzahlLernbareAttacken; i++) {             
            let attackUrlFromIndex = Pokedata.moves[i].move.url                     
            await axios.get(attackUrlFromIndex).then((response) => {    
              const attackData = response.data; 
              this.AttackobjektArray[i] = attackData; 
            })
          }     
          this.filterAttacksFromByDamage(); 
        },
     
        async filterAttacksFromByDamage(){ //speichert alle Attackeninformationen die Schaden machen in neuen Array       
          let b = 0;
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {  
            if((this.AttackobjektArray[i].power > 0) || (this.AttackobjektArray[i].power != null )){   
              this.newAttackArray[b] = this.AttackobjektArray[i]; 
              b++;
            }     
          } 
          this.SelectFourRandomAttacks(); 
        },

        async SelectFourRandomAttacks(){   
          console.log("anzahl Schadensattacken: " + this.newAttackArray.length);  //zeigt anz schadens Attacken
          let untergrenze=0;
          let anzahlSchadensAttacken = this.newAttackArray.length; 
          for (let i=0; i < 4; i++) {
            let randomNumber  = Math.floor(Math.random() * (anzahlSchadensAttacken - untergrenze + 1)) + 1;  
            this.fourAttackArray[i] = this.newAttackArray[randomNumber];           //fourAttackArray = finaler attacken-Array mit allen Infos!                                               
            console.log("     Attack Name: " + this.fourAttackArray[i].name        //[aktuell noch mit doppelten Attacken] 
                      + "   Damage: " + this.fourAttackArray[i].power              //zeigt 4 Random Attacken infos 
                      + "   Genauigkeit: " + this.fourAttackArray[i].accuracy
                      + "   PP: " + this.fourAttackArray[i].pp
                      );
          } 
          
          this.visible=true;
        },

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
        background-color: rgba(46, 204, 72, 0.726)
    }
   
  </style>