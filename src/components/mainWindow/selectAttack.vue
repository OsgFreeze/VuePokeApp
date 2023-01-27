<template>
  <div class="attackWindow">   
    <span v-if="pokemonHelper.hidePokemonInfos">
      <button class="AtkAnzeigen" id="AtkAnzID" @click="disableSelectPokemon"> Pokemon Attacken anzeigen </button> 
      <p class="chosenAttacks" v-if="this.fourAttacksChosen">  <!-- zeigt ausgewählte Attacken an --> 
        Chosen Attacks:
        {{"[" + this.chosenAttacks[0].name + "] ,  "}}
        {{"[" + this.chosenAttacks[1].name + "] ,  "}}
        {{"[" + this.chosenAttacks[2].name + "] ,  "}} 
        {{"[" + this.chosenAttacks[3].name + "]   "}}
      </p>   
      <div v-if="this.visible">  <!-- attacken Auswahlfenster [nur Schadensattacken]--> 
        <p v-for="(Attackobjekt, index) in newDamageAttackArray" :key="index"> 
          {{index+1}} 
          <button :class="{'mark-button': getSelectedStatus(index)}"  @click="selectAttack(index)"> Name: {{Attackobjekt.name}} Power: {{Attackobjekt.power}} Genauigkeit: {{Attackobjekt.accuracy}} Type: {{Attackobjekt.type.name}} Damage Class: {{Attackobjekt.damage_class.name}}</button> 
        </p> 
      </div> 
    </span>
    <generateRandomPokemon v-if="this.fourAttacksChosen" :übergebenesPokemonObject="this.pokemonObject" />
  </div>
</template>
  
<script>
  import axios from 'axios'
  import generateRandomPokemon from './generateRandomPokemon.vue';

  //Wieviele Attacken gewählt werden können.
  const MAX_ATTACKS = 4;

  export default {
    name: 'selectAttack',
    components: {  
      generateRandomPokemon
    }, 
    inject: ["pokemonHelper"],
    props: ['übergebenesPokemonObjekt'],   // übergebenes pokemonObjekt{} von parent
    data(){ 
      return {
    // notwendige start Zuweisungen
        baseDamage: 0,
        anzahlLernbareAttacken: 0,  
        chosenAttacksIndex: 0,     
        visible: false,
        fourAttacksChosen: false,
        AttackListShown: false,

    // Attacken informations Arrays
        AttackobjektArray: [],        //   speichert jede lernbare Attacke eines Pokemon       
        newDamageAttackArray: [],     //   speichern alle Attacken die Schaden machen. 
        chosenAttacks: [],            //   speichert alle ausgewählten Attacken. 

    // hier wird das fertige Pokemon{} gespeichert
        pokemonObject: {             
          pokemonData: {},           
          attackData: {}             
        },
      }
    },
    methods: {  
      disableSelectPokemon(){
        this.getAttackData();
      },

    // gibt Informationen zu den übergebenen lernbaren Attacken aus 
      async getAttackData(){  
        const Pokedata = this.übergebenesPokemonObjekt;                                            
        this.anzahlLernbareAttacken = Pokedata.moves.length;   
        for (let i=0; i < this.anzahlLernbareAttacken; i++) { 
          let attackURL = Pokedata.moves[i].move.url     
           // Api call für alle lernbaren Attacken                 
          await axios.get(attackURL).then((response) => {    
            const localAttackData = response.data; 
            this.AttackobjektArray[i] = localAttackData;
          })
        } 
        this.filterAttacksByDamage();
      },
    
    // speichert alle Attackeninformationen die Schaden machen in newAttackArray[]
      async filterAttacksByDamage(){  
          let b = 0;
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {  
            if((this.AttackobjektArray[i].power > 0) || (this.AttackobjektArray[i].power != null )){  
              this.newDamageAttackArray[b] = this.AttackobjektArray[i]; 
              b++;
            }    
          } 
          if(this.AttackListShown == false){
            this.visible = true;
            this.AttackListShown = true;
            this.$parent.lockSelectPokemon(true);
          }else{
            this.visible = false;
            this.AttackListShown = false;
            this.$parent.lockSelectPokemon(false);
          }            
        },

   // überprüfe ob Attacke bereits gewählt ist.  
      selectAttack(attackNumber){  
        const alreadySelected = this.chosenAttacks.findIndex((attack) => attack.name === this.newDamageAttackArray[attackNumber].name)
        const AtkAnzeigenCss = document.getElementById("AtkAnzID")
          if(alreadySelected > -1){
            // Attack bereits gewählt, entferne wieder
              this.chosenAttacks.splice(alreadySelected, 1); 
              this.chosenAttacksIndex--;
          } else {
            // Neue Attacke gewählt speichere diese.
              this.chosenAttacks.push(this.newDamageAttackArray[attackNumber])
              this.chosenAttacksIndex++;                                           
            if(this.chosenAttacksIndex == MAX_ATTACKS){ 
            // Maximale Anzahl der Attacken gewählt, warte nun auf generate random Pokemon
              this.pokemonObject.pokemonData = this.übergebenesPokemonObjekt;    //   Speichert das übergebene Pokemon in lokales "pokemonObjekt"
              this.pokemonObject.attackData = this.chosenAttacks;                //   speichert (alle 4 Attacken) in chosenAttacks[] 
              this.visible = false;                                              //   setzt Sichtbarkeit von dem attacken Auswahlfenster auf false
              this.fourAttacksChosen = true;                                     //   sichtbar sobald 4 Attacken ausgewählt wurden
              AtkAnzeigenCss.style.visibility = "hidden";
            }
          }
      },
      getSelectedStatus(index){
        return this.chosenAttacks.findIndex((attack) => attack.name === this.newDamageAttackArray[index].name) > -1;
      },
      SearchBarVisible(){
        this.$parent.setSearchBarVisible2();
      }
    }
  }
  </script>

  <style>
  .fightwindow{
    background-color: rgba(13, 184, 236, 0.479);
  }
  .mark-button {
    background-color: #1f944c;
  }

  .AtkAnzeigen{
    visibility: visible;
  }

  .chosenAttacks{
    font-size: large;
    text-transform: capitalize;
    text-align: center;
  }
  </style>
  