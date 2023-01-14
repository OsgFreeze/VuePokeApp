<template class="generateRandomPokemon">
    <div>   
        <button @click="generateRandomPokemon(898)"> generateRandomPokemon </button> <!-- 898, da bis dahin alle Pokemon verfügbar sind -->
        <div v-if="visible" class="randomPokemon"> 
             <img class="randomPokemonPicture" :src="this.randomPokemonObject.sprites.other.home.front_default" />
           <!--  <img :src="this.randomPokemonObject.sprites.versions.generation-v.black-white.animated.front_default" />     ✗ funktioniert nicht -->
        </div>
    </div>

    <div v-if="visible"> 
        <audio controls="off" autoplay="on" volume="0.01" loop="true" >    <source src="./fightMusic.mp3"  type="audio/mpeg">    </audio>  <!-- audio Abspielen für Kampfmusic  <source src="./fightMusic.mp3" -->
    </div>

  </template>
  
  <script>
  import axios from 'axios'

  export default {
    name: 'generateRandomPokemon',
    components: {  
    }, 
    //props: ['uebergebenesPokemon'], 
    
    data(){ 
      return {
        randomPokemonObject: {},
        visible: false,
      }
    },

    methods: {    
        async generateRandomPokemon(number){   
            let randomNumber  = Math.floor(Math.random() * (number - 1 + 1)) + 1;   // *nur* diese Zeile wurde online nachgeschaut  ->  [nicht selbst gecoded!]
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => { 
            this.randomPokemonObject = response.data; 
            this.visible=true;
            console.log(this.randomPokemonObject.name); //sollte ein Random Pokemon Name ausgeben
            })
        }, 
      //  async sortRandomPokemon(methodenVariable){   
            //await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`).then((response) => { 
           // this.randomPokemonObject = response.data; 
           // this.visible=true;
           // console.log(this.randomPokemonObject.name); //sollte ein Random Pokemon Name ausgeben
           // })
      //  }, 

        //async getAttackData(){                                            
      //const Pokedata = this.übergebenesObject;                           
      //this.anzahlLernbareAttacken = Pokedata.moves.length;               

     // for (let i=0; i < this.anzahlLernbareAttacken; i++) {             

        //let attackURL = Pokedata.moves[i].move.url                     
        //await axios.get(attackURL).then((response) => {    
        //const attackData = response.data; 
        //this.AttackobjektArray[i] = attackData;
        //})

       // } 
       // this.visible=true;
     //},
    // selectAttack(number){ 
     //   this.chosenAttacks[this.arrayindex]= number;
     //   this.arrayindex++;
    // }
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