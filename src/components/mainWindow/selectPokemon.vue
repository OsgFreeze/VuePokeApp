<template class="selectPokemonTemplate">
  <span v-if="SearchBarVisible" @messageFromChild="childMessageReceived">
    <div class="eingabeFeld"> Pokemon (Pokedex) Nummer eingeben:  <!-- Pokemon Nummer von Pokedex eingeben & API-Methodenaufruf -->
      <input type="number" v-model="PokeNumber" min="1" max="898" :disabled="this.isDisabled"> 
      <button @click="setNrPlus" :disabled="this.isDisabled"> Hoch ↑ </button>
      <button @click="setNrMinus" :disabled="this.isDisabled"> Runter ↓ </button>  &nbsp;
      <button @click="loadApiNumber" :disabled="this.isDisabled"> Zahl Übermitteln </button>     
    </div>
  </span>
  <div class="dataAusgabeFeld" v-if="hidePokemonInfos"> <!-- gibt alle Informationen über das Pokemon aus -->
    <p class="PokeName"> {{ this.pokemonObject.name }} </p>
    <hr style="border-width: 2px; margin-top: 0px">
    <div class="PokeInfo">  
      <div class="PokeInfoleftSide">
        <img class="PokemonPictureHD" id="PokePic" :src="this.PokemonPicture"/>
        <button class="shinyButton" id="shinyButtonId" @click="turnPokemonShiny"> Shiny </button>
       </div> 
      <div class="PokeInfoRightSide">
        <p class="baseStats" v-for="(pokemonStat, index) in pokemonStats" :key="index"> {{pokemonObject.stats[index].stat.name}}:  {{pokemonStat.base_stat}}</p>
        <p class="TypeNames" v-for="(pokemonType, index) in pokemonTypes" :key="index"> Type {{ index+1 }}: {{ pokemonType}} </p>          
      </div>
      <div class="PokeInfoTypeBars">           
        <div class="TypeBars">
          <div class="TypeBar1" id="Color1"></div>
          <div class="TypeBar2" id="Color2"></div> 
        </div>  
      </div>  
    </div>
    <hr style="border-width: 2px">
  </div>
  <selectAttack :übergebenesPokemonObjekt="this.pokemonObject" /> <!-- übergebe Daten zur Attacken Auswahl -->
</template>

<script>
import axios from 'axios'
import selectAttack from './selectAttack.vue';

export default {
  name: 'selectPokemon',
  components: {
    selectAttack, 
  }, 
  provide(){
    const me = this;
    const pokemonHelper = {}

  //Setzt das Game auf started, damit wissen wir, dass der Kampf begonnen hat.
    pokemonHelper.setGameStarted = function(){
      me.gameStarted = true;
    }
      //Macht den Status, ob die Game Information geladen ausgeblendet werden müssen, überall verfügbar.
      //defineProperty verwenden, um einen getter zu verwenden.
    Object.defineProperty(pokemonHelper, "hidePokemonInfos", {
      get: function(){return me.hidePokemonInfos}  
    })
    return {pokemonHelper}
  },
  computed: {
      //Gibt Live Update über den Game Status
      hidePokemonInfos(){
        return !this.gameStarted && this.pokeLoaded;
      }
  },
  data(){  
    return {
    // Startwerte
      PokeNumber: 1,      
      PokemonPicture: "",

    // Boolean Zuweisungen
      gameStarted: false,
      SearchBarVisible: true,
      toggleState: true,
      isDisabled: false,
      pokeLoaded: false,  

    // Pokemon Informationen 
      pokemonStats: [],   //  speichert die basis Werte von einem Pokemon
      pokemonObject: {},  //  speichert die Pokemon Daten von dem API Call,
      pokemonTypes: [],   //  speichert alle Types des Pokemon
      PokeTypeColors: [], //  speichert die Farben des Types
    }
  },
  methods: { 
  //API Call für Pokemon Daten 
    async loadApiNumber(){ 
      await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
        this.pokemonObject = response.data; 
        this.pokemonStats =  this.pokemonObject.stats;
        this.pokeLoaded = true;
        this.PokemonPicture = this.pokemonObject.sprites.other.home.front_default;
        this.pokemonTypes = [];

          // speichert die Types des Pokemon 
        let PokeType = this.pokemonObject.types;
        for(let i = 0; i < PokeType.length; i++){
          this.pokemonTypes[i] = PokeType[i].type.name;
        }
      })
      this.defineTypeColor();
    },
    
    defineTypeColor(){
      const TypeColor1 = document.getElementById("Color1"); 
      const TypeColor2 = document.getElementById("Color2");
      this.PokeTypeColors = [];

      for(let i = 0; i < this.pokemonTypes.length; i++){
        this.PokeTypeColors[i] = this.pokemonTypes[i];
        if(this.pokemonTypes[i] == "normal"){
          this.PokeTypeColors[i] = "#b5b5a6";
        }
        if(this.pokemonTypes[i] == "fire"){
          this.PokeTypeColors[i] = "#ff4422"
        }
        if(this.pokemonTypes[i] == "water"){
          this.PokeTypeColors[i] = "#3399ff"
        }
        if(this.pokemonTypes[i] == "electric"){
          this.PokeTypeColors[i] = "#ffcc33"
        }
        if(this.pokemonTypes[i] == "grass"){
          this.PokeTypeColors[i] = "#77cc55"
        }
        if(this.pokemonTypes[i] == "ice"){
          this.PokeTypeColors[i] = "#66ccff"
        }
        if(this.pokemonTypes[i] == "fighting"){
          this.PokeTypeColors[i] = "#bb5544"
        }
        if(this.pokemonTypes[i] == "poison"){
          this.PokeTypeColors[i] = "#aa5599"
        }
        if(this.pokemonTypes[i] == "ground"){
          this.PokeTypeColors[i] = "#ddbb55"
        }
        if(this.pokemonTypes[i] == "flying"){
          this.PokeTypeColors[i] = "#8899ff"
        }
        if(this.pokemonTypes[i] == "psychic"){
          this.PokeTypeColors[i] = "#ff5599"
        }
        if(this.pokemonTypes[i] == "bug"){
          this.PokeTypeColors[i] = "#aabb22"
        }
        if(this.pokemonTypes[i] == "rock"){
          this.PokeTypeColors[i] = "#bbaa66"
        }
        if(this.pokemonTypes[i] == "ghost"){
          this.PokeTypeColors[i] = "#6666bb"
        }
        if(this.pokemonTypes[i] == "dragon"){
          this.PokeTypeColors[i] = "#7766ee"
        }
        if(this.pokemonTypes[i] == "dark"){
          this.PokeTypeColors[i] = "#775544"
        }
        if(this.pokemonTypes[i] == "steel"){
          this.PokeTypeColors[i] = "#aaaabb"
        }
        if(this.pokemonTypes[i] == "fairy"){
          this.PokeTypeColors[i] = "#ee99ee"
        }        
      }
      
      if(this.PokeTypeColors.length == 1){
        TypeColor1.style.backgroundColor = this.PokeTypeColors[0];
        TypeColor1.style.borderColor = this.PokeTypeColors[0];
        TypeColor2.style.visibility = "hidden";
      }
        
      if(this.PokeTypeColors.length == 2){
        TypeColor1.style.backgroundColor = this.PokeTypeColors[0];
        TypeColor1.style.borderColor = this.PokeTypeColors[0];
        TypeColor2.style.visibility = "visible";
        TypeColor2.style.backgroundColor = this.PokeTypeColors[1];
        TypeColor2.style.borderColor = this.PokeTypeColors[1];
      }
    },

  //ausgewähltes Pokemon +1 -> danach API CALL  
    async setNrPlus(){ 
      if(this.PokeNumber < 898){
        this.PokeNumber = (this.PokeNumber + 1);
        this.loadApiNumber();
      }
    },

  //ausgewähltes Pokemon -1 -> danach API CALL  
    async setNrMinus(){ 
      if(this.PokeNumber > 1){
        this.PokeNumber = (this.PokeNumber - 1);
        this.loadApiNumber();
      }
    },

    setSearchBarVisible2(){
      this.SearchBarVisible = false;
    },

    turnPokemonShiny(){
      const shinyButtonCSS = document.getElementById("shinyButtonId");
      if(this.toggleState == true){
        this.PokemonPicture = this.pokemonObject.sprites.other.home.front_shiny;
        this.toggleState = false;
        shinyButtonCSS.textContent = "Normal";
      }else{
        this.PokemonPicture = this.pokemonObject.sprites.other.home.front_default;
        this.toggleState = true;
        shinyButtonCSS.textContent = "Shiny";
      }
    },

    lockSelectPokemon(bool){
      this.isDisabled = bool;
    },
  },
}
</script>

<style>
.eingabeFeld{
  font-size: large;
}

.hr{
  position: absolute;
  width: 1333px;
  margin-top: 20px;
}

.PokeInfo{
  display: flex;
  flex-direction: row;
  
}

  .PokeInfoleftSide{
    display: flex;
    flex-direction: column;
  }

    .PokeName{
      font-size: 35px;
      text-transform: capitalize;
      margin-bottom: 0px;
    }

    .PokemonPictureHD {
      height: 250px;
      width: 250px;
    }

    .shinyButton{
      margin-top: 50px;
      width: 150px;
      margin-left: 50px;
    }

  .PokeInfoRightSide{
    margin-top: 20px;
    margin-left: 75px;
    width: 160px;
  }

    .baseStats{
      font-size: large;
      text-transform: capitalize;
    }

    .TypeNames{
      font-size: large;
      text-transform: capitalize;
    }

  .PokeInfoTypeBars{
    display: flex;
    flex-direction: column;  
  }

    .TypeBars{
      margin-top: 273px;        
      }

      .TypeBar1{
        height: 15px;
        width: 60px;
        background-color: rgb(0, 0, 0);
        border: solid;
        border-width: 2px;
        border-color: black;
        border-radius: 15px;
      }

      .TypeBar2{
        height: 15px;
        width: 60px;
        margin-top: 25px;
        background-color: black;
        border: solid;
        border-width: 2px;
        border-color: black;
        border-radius: 15px;
        visibility: visible;     
      }
</style>