<template class="generateRandomPokemon">
    <div>   
      <button @click="generateRandomPokemon(898)"> generateRandomPokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
      <div v-if="randomPokemonLoaded" > <!-- Zeigt Information über das zufällig ausgewählte Pokemon -->
          <img class="randomPokemonPicture" :src="this.randomPokemonObject.sprites.other.home.front_default" />
          <fightWindow class="fightwindow" :übergebenePokemon="this.twoCompletePokemon"/>
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

        randomPokemonEnemy: {  //speichert alle local gesammelten Informationen in einem Objekt 
          enemyPokemon: {},
          enemyAttacks: {}
        },

        twoCompletePokemon: { //fasst beide Pokemon in einem übergabe Objekt             
          myPokemon: {},     
          enemyPokemon: {},                             
        }, 

        randomPokemonLoaded: false,
        newAttackArrayLength: 0,
        anzahlLernbareAttacken: 0,
        anzahlSchadensAttacken: 0,
      }
    },

    methods: {    
        async generateRandomPokemon(obergrenze){   //erzeugt ein random Pokemon Object & speichert ergebniss in [this.randomPokemonObject] 
          this.anzahlLernbareAttacken= 0,
          this.anzahlSchadensAttacken= 0,
          this.newAttackArray=[];

            let untergrenze=1;
            let randomNumber  = Math.floor(Math.random() * (obergrenze - untergrenze - 1)) + 1;   
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => {  
            this.randomPokemonObject = response.data; 
            this.getAttackDataFromRandomPokemon(); 
            })
        }, 

        async getAttackDataFromRandomPokemon(){   //erstellt ein neuen Attacken Array von dem random Pokemon                            
          const localPokedata = this.randomPokemonObject; // generiertes pokemon Objekt nochmal local speichern in Variable -> zur vereinfachung.                          
          this.anzahlLernbareAttacken = localPokedata.moves.length;  // anzahl lernbare Attacken von generiertes pokemon Objekt
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {           
            let attackUrlFromIndex = localPokedata.moves[i].move.url                  
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

        async SelectFourRandomAttacks(){   //[aktuell noch mit doppelten Attacken]
          console.log("anzahl Schadensattacken: " + this.newAttackArray.length); 
          let untergrenze=0;
          this.anzahlSchadensAttacken = this.newAttackArray.length; 
          for (let i=0; i < 4; i++) { 
            let randomNumber  = Math.floor(Math.random() * (this.anzahlSchadensAttacken - untergrenze - 1)) + 1; 
            this.fourAttackArray[i] = this.newAttackArray[randomNumber];           !                                           
            console.log("   Attack Name: " + this.fourAttackArray[i].name         
                      + "   Damage: " + this.fourAttackArray[i].power               
                      + "   Genauigkeit: " + this.fourAttackArray[i].accuracy
                      + "   PP: " + this.fourAttackArray[i].pp
                      );
          } 

        //Speichert alle random Pokemon Informationen(pokemon,Attacken) in das übergabe Objekt  ->  [randomPokemonEnemy]
          this.randomPokemonEnemy.enemyPokemon = this.randomPokemonObject;
          this.randomPokemonEnemy.enemyAttacks = this.fourAttackArray; 

        //Speichert alle verfügbaren Informationen(Pokemon1[], Pokemon2[]) in das übergabe Objekt  ->  [twoCompletePokemon]
          this.twoCompletePokemon.myPokemon = this.übergebenesPokemonObject;
          this.twoCompletePokemon.enemyPokemon = this.randomPokemonEnemy;
          this.randomPokemonLoaded=true;
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