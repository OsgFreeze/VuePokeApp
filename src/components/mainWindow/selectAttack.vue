<template>
    <div>   
      <button @click="getAttackData"> Pokemon Attacken anzeigen </button> 
      <p v-if="this.fourAttacksChosen">  <!-- zeigt ausgewählte Attacken an --> 
        Ausgewählte Attacken:
        {{"[" + this.chosenAttacks[0].name + "]   "}}
        {{"[" + this.chosenAttacks[1].name + "]   "}}
        {{"[" + this.chosenAttacks[2].name + "]   "}} 
        {{"[" + this.chosenAttacks[2].name + "]   "}}
      </p>  
      <div v-if="this.visible">  <!-- attacken Auswahlfenster [nur Schadensattacken]--> 
        <p v-for="(Attackobjekt, index) in newDamageAttackArray" :key="index"> 
          {{index}} 
          <button @click="selectAttack(index)"> Name: {{Attackobjekt.name}} Power: {{Attackobjekt.power}} Genauigkeit: {{Attackobjekt.accuracy}} Type: {{Attackobjekt.type.name}} Damage Class: {{Attackobjekt.damage_class.name}}</button> 
        </p> 
      </div> 
      <generateRandomPokemon v-if="this.fourAttacksChosen" :übergebenesPokemonObject="this.pokemonObject" />
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
    props: ['übergebenesPokemonObjekt'],   //übergebenes pokemonObjekt von parent
    data(){ 
      return {
        baseDamage: 0,                //     speichert den Schaden einer Attacke
        anzahlLernbareAttacken: 0,    //     speichert die maximale Anzahl an lernbaren Attacken eines Pokemon 
        chosenAttacksIndex: 0,        //     wird 'ausgelagert' benötigt für die selectAttack() Methode, das diese nicht bei jedem aufruf auf 0 zurückgesetzt wird     
        visible: false,
        fourAttacksChosen: false,

        pokemonObject: {              //     ->   hier wird das fertige Pokemon[] gespeichert
          pokemonData: {},            //     speichert alle Informationen über ein Pokemon 
          attackData: {}              //     speichert alle Informationen über die Attacken
        },

        AttackobjektArray: [],        //     speichert jede lernbare Attacke eines Pokemon       
        newDamageAttackArray: [],     //     speichern alle Attacken die Schaden machen. 
        chosenAttacks: [],            //     speichert alle ausgewählten Attacken. 
      }
    },

    methods: {
      async getAttackData(){  //   ->   gibt Informationen zu den übergebenen lernbaren Attacken aus  [funktioniert ✓]
        const Pokedata = this.übergebenesPokemonObjekt;                                            
        this.anzahlLernbareAttacken = Pokedata.moves.length;   
        for (let i=0; i < this.anzahlLernbareAttacken; i++) { 
          let attackURL = Pokedata.moves[i].move.url                     
          await axios.get(attackURL).then((response) => {    
            const localAttackData = response.data; 
            this.AttackobjektArray[i] = localAttackData;
          })
        } 
        this.filterAttacksByDamage();
      },
    

      async filterAttacksByDamage(){  //   ->   speichert alle Attackeninformationen die Schaden machen in newAttackArray[]
          let b = 0;
          for (let i=0; i < this.anzahlLernbareAttacken; i++) {  //funktioniert ✓
            if((this.AttackobjektArray[i].power > 0) || (this.AttackobjektArray[i].power != null )){  //funktioniert ✓
              this.newDamageAttackArray[b] = this.AttackobjektArray[i];  
              b++;
            }    
          } 
          this.visible=true; 
          console.log("Array mit allen Attacken:");
          console.log(this.AttackobjektArray);    //alter Array
          console.log("Array mit gefliterten schadens Attacken:");
          console.log(this.newDamageAttackArray); //neuer Array
        },

      selectAttack(attackNumber){  //   ->   erstellt neuen Array[] in dem nur die 4 ausgewählten Attacken gespeichert werden.
          this.chosenAttacks[this.chosenAttacksIndex]= this.newDamageAttackArray[attackNumber]; 
          this.chosenAttacksIndex++;                                           
          if(this.chosenAttacksIndex==4){ 
            console.log("Array mit 4 Attacken");
            console.log(this.chosenAttacks); //gibt neuen Array mit 4 werten aus
            this.pokemonObject.pokemonData = this.übergebenesPokemonObjekt;   //   Speichert das übergebene Pokemon in lokales pokemon[] 
            this.pokemonObject.attackData = this.chosenAttacks;               //   speichert (alle 4 Attacken) in pokemon[] 
            this.visible = false;                                             //   setzt sichtbarkeit von dem attacken Auswahlfenster auf false
            this.fourAttacksChosen = true;                                     //   sichtbar sobald 4 Attacken ausgewählt wurden
           }
      }
    }
  }
  </script>
  
  <style>
  .fightwindow{
    background-color: rgba(13, 184, 236, 0.479);
  }
  </style>
  