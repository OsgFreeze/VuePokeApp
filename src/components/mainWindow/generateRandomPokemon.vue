<template class="generateRandomPokemon">
    <div>   
      <button v-if="ButtonVisible" @click="main(898)"> generate new random Pokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
      <div v-if="randomPokemonLoaded" > <!-- Zeigt Information über das zufällig ausgewählte Pokemon -->
          <fightWindow :übergebenePokemon="this.twoCompletePokemon"/>
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
      //alle notwendigen deklarierungen
        randomPokemonObject: {},
        AttackobjektArray: [],
        newAttackArray: [],
        fourAttackArray: [],
        randomPokemonLoaded: false,
        ButtonVisible: true,
        newAttackArrayLength: 0,
        anzahlLernbareAttacken: 0,
        anzahlSchadensAttacken: 0,
        

      //speichert das random Pokemon 
        randomPokemonEnemy: {  
        enemyPokemon: {},
        enemyPokemonShiny: [],
        },

      //fasst beide Pokemon zusammen   
        twoCompletePokemon: {   
          myPokemon: {},     
          enemyPokemon: {},                                      
        }, 
      }
    },

    methods: {   
      
    //ruft alle Methoden nacheinander auf --> Programm Ablauf wird etwas übersichtlicher Thema sauberer Code (bei Bedarf einf wieder löschen und im Button anpassen)  
      async main(obergrenze){
          this.$parent.SearchBarVisible();
          this.generateRandomPokemon(obergrenze);
          this.ButtonVisible = false;                       
      },

      setButtonVisible(){
        this.ButtonVisible = false;
      },

    //erzeugt ein random Pokemon Object & speichert ergebniss in [this.randomPokemonObject]   
        async generateRandomPokemon(obergrenze){  
          this.anzahlLernbareAttacken= 0,
          this.anzahlSchadensAttacken= 0,
          this.newAttackArray=[];
          let untergrenze=1;
          let randomNumber  = Math.floor(Math.random() * (obergrenze - untergrenze - 1)) + 1;   
          await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => {  
            this.randomPokemonObject = response.data; 
            console.log("random pokemon Name: " + this.randomPokemonObject.name);
            this.getAttackDataFromRandomPokemon(); 
          })        
        }, 


    //erstellt ein neuen Attacken Array von dem random Pokemon       
        async getAttackDataFromRandomPokemon(){                 
          const localPokedata = this.randomPokemonObject;                   // generiertes pokemon Objekt nochmal local speichern in Variable -> zur vereinfachung.                          
          this.anzahlLernbareAttacken = localPokedata.moves.length;         // anzahl lernbare Attacken von generiertes pokemon Objekt
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {           
            let attackUrlFromIndex = localPokedata.moves[i].move.url                  
            await axios.get(attackUrlFromIndex).then((response) => {    
              const attackData = response.data;  
              this.AttackobjektArray[i] = attackData;  
            })
          } 
          this.filterAttacksFromByDamage();
        },
     

    //speichert alle Attackeninformationen die Schaden machen in neuen Array     
        async filterAttacksFromByDamage(){  
          let b = 0;
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {  
            if((this.AttackobjektArray[i].power > 0) || (this.AttackobjektArray[i].power != null )){   
              this.newAttackArray[b] = this.AttackobjektArray[i]; 
              b++;
            }     
          } 
          this.SelectFourRandomAttacks(); 
        },


    // Methode um alle 4 ausgewählten Attacken zu speichern    
        async SelectFourRandomAttacks(){   
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
          this.randomPokemonEnemy.enemyPokemonShiny[0] = this.checkIfPokemonShiny();

        //setze nicht Shiny auf false [wird für die unterklasse: Fightwindow benötigt] 
          if(this.randomPokemonEnemy.enemyPokemonShiny[0] == true){ 
            this.randomPokemonEnemy.enemyPokemonShiny[1] = false; 
          } else {
            this.randomPokemonEnemy.enemyPokemonShiny[1] = true;
          }

        //Speichert alle verfügbaren Informationen(Pokemon1[], Pokemon2[]) in das übergabe Objekt  ->  [twoCompletePokemon]
          this.twoCompletePokemon.myPokemon = this.übergebenesPokemonObject;
          this.twoCompletePokemon.enemyPokemon = this.randomPokemonEnemy;
          this.randomPokemonLoaded=true;

          console.log(this.twoCompletePokemon);

        },

      //bestimmt ob das pokemon Shiny ist
        checkIfPokemonShiny(){
          let shinyChance = 25; //bestimmt Shiny Chance [1/50]
          let untergrenze = 1;
          let randomNumber  = Math.floor(Math.random() * (shinyChance - untergrenze - 1)) + 1; 
            console.log("Shiny Number: " + randomNumber);
          if(randomNumber == 1){ 
            return true // ist Shiny
          } else {
            return false // nicht Shiny
          }
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
    }
  </style>