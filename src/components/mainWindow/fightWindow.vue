<template>

  <button @click="Start"> Press me..</button>

  <audio controls=true volume="0.01">
    <source src="./fightMusic.mp3" autoplay type="audio/mpeg">
    Sorry - Ihr Browser hat keine Unterstützung für dieses Audio-Format.
  </audio>

  <button style="margin-left: 10px" @click="ReviveMyHealthbar">Revive my Pokemon</button>
  <button style="margin-left: 10px" @click="ReviveEnemyHealthbar">Revive enemy Pokemon</button>

      <!-- gibt die Anzahl besiegter Gegner an--> 
  <button style="margin-left: 450px" > {{"Anzahl besiegter Gegner: " + this.anzahlBesiegteGegner}} </button>  
  

  <div class="komplettesKampffenster">
    <div class="BackroundWhenDead" v-if="!this.pokemonPlayerStillAlive">
      <button class="RestartButton"> Restart </button>
    </div>
        <div class="KampffensterOben"> 
          <img class="backgroundPicture" src="https://cutewallpaper.org/21/pokemon-battle-backgrounds/Index-of-spritesgen6bgs.jpg" />
            <div class="left"> 
              <div class="obenL"> 
                <div class="PokeStatsL">      
                  <p class="pokemonInfoStyleL">         
                    {{ this.übergebenePokemon.myPokemon.pokemonData.name + " Lv. 50"}}   
                    {{"HP. " + (this.MyPokemonHP)}}
                  </p>  
                </div>               
                <div class="healthBarMyPokemon">    <!-- Unsere HealthBar-->                
                  <div id="MyHealth" class="StatusBarMyPokemon">                    
                    {{Math.round(MyPokemonCurrentHP.toFixed(2))}}                  
                  </div>                
                </div>

              </div>

              <div class="pokemonSpriteL"> 
                <img class="myPokemonPicture" :src="übergebenePokemon.myPokemon.pokemonData.sprites.back_default"/> <!-- Spieler Pokemon Sprite Back-->
              </div>
            </div>

            <div class="right"> 
              <div class="obenR"> 
                <div class="gegnerPokeStatsR">
                  <div class="gegnerInformationen">
                    <p class="pokemonInfoStyleR" style="padding-top: 30px;">
                      {{ this.übergebenePokemon.enemyPokemon.enemyPokemon.name  + " Lv. 50"}}
                      {{"HP. " + Math.round((this.übergebenePokemon.enemyPokemon.enemyPokemon.stats[0].base_stat + 100).toFixed(2))}}
                    </p>
                  </div>

                  <div class="healthbarGegner">  <!-- Healthbar Gegner -->
                    <div ref="enemyHealth" id="EnemyHealth" class="StatusBarGegner">
                      {{ Math.round(EnemyPokemonCurrentHP.toFixed(2)) }}
                      
                    </div>
                  </div>
                </div>


                <div class="gegnerPokemonR"> 
                  <!-- mein plan war mit v-if="true/false" jeweils das Shiny oder das normale anzuzeigen. -> klappe aber irgendwie ned-->
                 <img class="enemyPokemonPicture" v-if="übergebenePokemon.enemyPokemon.enemyPokemonShiny[1]" :src="übergebenePokemon.enemyPokemon.enemyPokemon.sprites.front_default"/> 
                 <img class="enemyPokemonPicture" v-if="übergebenePokemon.enemyPokemon.enemyPokemonShiny[0]" :src="übergebenePokemon.enemyPokemon.enemyPokemon.sprites.front_shiny"/> 
                </div>
              </div>

              <div class="attackenauswahlFenster" v-if="visible">  <!-- Hier werden die 4 Attacken angezeigt-->
                <div class="ButtonZweiAuswahlfensterOben"> 
                  <div class="divButtonAttackeLinksOben"> 
                    <button class="button buttonStyle button1" id="AtkButton1" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[0])">                     
                      {{übergebenePokemon.myPokemon.attackData[0].name}}
                      {{übergebenePokemon.myPokemon.attackData[0].power}}
                      {{übergebenePokemon.myPokemon.attackData[0].type.name}} 
                      {{" [" + übergebenePokemon.myPokemon.attackData[0].damage_class.name + "]"}} 
                     </button> 
                  </div>

                  <div class="divButtonAttackeRechtsOben"> 
                    <button class="button buttonStyle button2" id="AtkButton2" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[1])">  
                      {{übergebenePokemon.myPokemon.attackData[1].name}} 
                      {{übergebenePokemon.myPokemon.attackData[1].power}}
                      {{übergebenePokemon.myPokemon.attackData[1].type.name}}
                      {{" [" + übergebenePokemon.myPokemon.attackData[1].damage_class.name + "]"}} 
                    </button> 
                  </div>
                </div>
                <div class="ButtonZweiAuswahlfensterUnten">
                  <div class="divButtonAttackeLinksUnten"> 
                    <button class="button buttonStyle button3" id="AtkButton3" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[2])">
                      {{übergebenePokemon.myPokemon.attackData[2].name}} 
                      {{übergebenePokemon.myPokemon.attackData[2].power}}
                      {{übergebenePokemon.myPokemon.attackData[2].type.name}}
                      {{" [" + übergebenePokemon.myPokemon.attackData[2].damage_class.name + "]"}}
                    </button> 
                  </div>
                  <div class="divButtonAttackeRechtsUnten"> 
                     <button class="button buttonStyle button4" id="AtkButton4" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[3])">
                      {{übergebenePokemon.myPokemon.attackData[3].name}} 
                      {{übergebenePokemon.myPokemon.attackData[3].power}}
                      {{übergebenePokemon.myPokemon.attackData[3].type.name}}
                      {{" [" + übergebenePokemon.myPokemon.attackData[3].damage_class.name + "]"}} 
                    </button> 
                  </div>
              </div>
            </div>
          </div>
        </div>

        <div class="konsoleUnten"> 
            <p style="text-align: center; margin-top: 32px;" > 
                {{this.konsolenAusgabe}} <!-- gibt den Text unten in der Konsole an-->
            </p>
        </div>
  </div>

  
</template>
  
<script>
import axios from 'axios';
export default {
  name: 'fightWindow',
  components: {  
  }, 
  props:['übergebenePokemon'], 
    
  inject: ["pokemonHelper"],
  data(){ 
    return {
      //Speichert beide Pokemon Objekte local
        myPokemon:{},       
        enemyPokemon: {},   
        konsolenAusgabe: "[Suchen sie sich eine Attacke aus um zu]",
        visible: true,
        anzahlBesiegteGegner: 0, //man beginnt mit 0 Besiegten gegnern 

      //HP bzw Pixel von der Pokemon Healthbar 
        MyPokemonHealth: 100,    
        EnemyHealth: 100, 
        pokemonPlayerStillAlive: true,         
        pokemonEnemyStillAlive: true,
        MyPokemonHP: this.übergebenePokemon.myPokemon.pokemonData.stats[0].base_stat + 100,
        EnemyPokemonHP: this.übergebenePokemon.enemyPokemon.enemyPokemon.stats[0].base_stat + 100,
        MyPokemonCurrentHP: this.übergebenePokemon.myPokemon.pokemonData.stats[0].base_stat + 100,
        EnemyPokemonCurrentHP: this.übergebenePokemon.enemyPokemon.enemyPokemon.stats[0].base_stat + 100,

        DmgToMyPokemon: 0,         //speichert den aktuellen Dmg der am eigenen Pokemon ausgeführt werden soll
        DmgToEnemyPokemon: 0,      //speichert den aktuellen Dmg der am Gegner Pokemon ausgeführt werden soll
        damageMultiplicator: 1,    //standart Damage multiplicator [wird beeinflusst durch: Genauigkeit, pokemon_Typen, effektivität_Attacken]
        attackerTypes: [], 
        attackerTypesArray: [], 
        enemyTypes: [], 
        enemyTypesArray: [],           
      //Hardcoded für Button Hintergrundfarbe abhängig vom Type
        backgroundColor: { 
          normal: "weiß",
          feuer: "rot",
          wasser: "blau",
        },

        attackBackroundColors: [],

      }
    },
  methods: {

    Start(){
      const TypeColorB1 = document.getElementById("AtkButton1");
      const TypeColorB2 = document.getElementById("AtkButton2");
      const TypeColorB3 = document.getElementById("AtkButton3");
      const TypeColorB4 = document.getElementById("AtkButton4");

      for(let i = 0; i < 4; i++){
        let AttackName = this.übergebenePokemon.myPokemon.attackData[i].type.name;
        if(AttackName == "normal"){
          this.attackBackroundColors[i] = "#b5b5a6";
        }
        if(AttackName == "fire"){
          this.attackBackroundColors[i] = "#ff4422"
        }
        if(AttackName == "water"){
          this.attackBackroundColors[i] = "#3399ff"
        }
        if(AttackName == "electric"){
          this.attackBackroundColors[i] = "#ffcc33"
        }
        if(AttackName == "grass"){
          this.attackBackroundColors[i] = "#77cc55"
        }
        if(AttackName == "ice"){
          this.attackBackroundColors[i] = "#66ccff"
        }
        if(AttackName == "fighting"){
          this.attackBackroundColors[i] = "#bb5544"
        }
        if(AttackName == "poison"){
          this.attackBackroundColors[i] = "#aa5599"
        }
        if(AttackName == "ground"){
          this.attackBackroundColors[i] = "#ddbb55"
        }
        if(AttackName == "flying"){
          this.attackBackroundColors[i] = "#8899ff"
        }
        if(AttackName == "psychic"){
          this.attackBackroundColors[i] = "#ff5599"
        }
        if(AttackName == "bug"){
          this.attackBackroundColors[i] = "#aabb22"
        }
        if(AttackName == "rock"){
          this.attackBackroundColors[i] = "#bbaa66"
        }
        if(AttackName == "ghost"){
          this.attackBackroundColors[i] = "#6666bb"
        }
        if(AttackName == "dragon"){
          this.attackBackroundColors[i] = "#7766ee"
        }
        if(AttackName == "dark"){
          this.attackBackroundColors[i] = "#775544"
        }
        if(AttackName == "steel"){
          this.attackBackroundColors[i] = "#aaaabb"
        }
        if(AttackName == "fairy"){
          this.attackBackroundColors[i] = "#ee99ee"
        }
      }

      TypeColorB1.style.backgroundColor = this.attackBackroundColors[0];
      TypeColorB2.style.backgroundColor = this.attackBackroundColors[1];
      TypeColorB3.style.backgroundColor = this.attackBackroundColors[2];
      TypeColorB4.style.backgroundColor = this.attackBackroundColors[3];
    },

//Attackendurchlauf starten
    buttonPressedMethod(attackData){
      this.visible=false;  
      let myPokemonIniative = this.übergebenePokemon.myPokemon.pokemonData.stats[5].base_stat
      let EnemyPokemonIniative = this.übergebenePokemon.enemyPokemon.enemyPokemon.stats[5].base_stat
   
  //System das Bestimmt wer zu erst Angreifen darf.   
  //eingesetzt Attacke hat Prio ? [erste IF abfrage bestimmt, ob eine Attacke erstschlag Garantie hat.] -> wenn gleich ist setzt die zweite Else bedingung.
      if(attackData.priority > 0){
        //mein Pokemon greift zu erst an. <- hat Prio Attacke eingesetzt
          setTimeout(()=>{
          if(this.pokemonEnemyStillAlive == true) {
            this.calculateDamageToEnemyPokemon(attackData); 
          }
          },1000);
        
          setTimeout(()=>{
          if(this.pokemonEnemyStillAlive == true) {
            this.choseRandomAttackFromEnemy();
          }
          },3000);
        
      }else{
          if(myPokemonIniative < EnemyPokemonIniative){ 
              //Gegner greift zu erst an.
            setTimeout(()=>{
            if(this.pokemonPlayerStillAlive == true ) {
              this.choseRandomAttackFromEnemy();
            }
          },1000);

          setTimeout(()=>{
            if(this.pokemonPlayerStillAlive == true) {
              this.calculateDamageToEnemyPokemon(attackData); 
            }
          },3000);
   
        } else {
            //mein Pokemon greift zu erst an.
          setTimeout(()=>{
            if(this.pokemonPlayerStillAlive == true) {
              this.calculateDamageToEnemyPokemon(attackData); 
            }
          },1000);
        
          setTimeout(()=>{
            if(this.pokemonEnemyStillAlive == true) {
            this.choseRandomAttackFromEnemy();
            }
          },3000);
        }
      }
    },


// Überprüft ob eine Attacke daneben gegangen ist [Genauigkeitswert von Attacke]
  calculateIfAttackMissed(übergebeneAttacke){
    let attackAccuracy = übergebeneAttacke.accuracy; // BSP: 90 -> trifft zu 90% 
    let untergrenze = 1;
    let obergrenze = 100;
      //gibt random Zahl zwischen [1-100] zurück.
    let zufallsZahl = Math.floor(Math.random() * (obergrenze - untergrenze - 1)) + 1; 
      //Prüft ob die Attacke trifft.
    if(zufallsZahl>attackAccuracy){   //BSP: Zufallszahl = 95, attackAccuracy=90,  so liegt 95 auserhalb der 90 [95>90] attacken Genauigkeit. -> Attacke Trifft nicht
      return 0;
    }
    if(zufallsZahl<attackAccuracy){  //BSP: Zufallszahl = 60, attackAccuracy=90,  so liegt 60 innerhalb der 90 [60<90] attacken Genauigkeit. -> Attacke Trifft
      return 1;
    }
  },


//Überprüft ob eine Attacke ein Pokemon sehr effektiv / Neutral oder nicht sehr effektiv trifft.
    async calculateTypeDamageMultiplicator(eingesetzteAttacke, verteidigendesPokemon, angreifendesPokemon){
      let attackType = eingesetzteAttacke.type.name 
      this.enemyType = verteidigendesPokemon.types; 
      this.attackerType = angreifendesPokemon.types; //Array

        //alle Arrays und Werte nach einem Durchlauf wieder zurücksetzen.
      this.attackerTypes = [], 
      this.attackerTypesArray =  [], 
      this.enemyTypes = [], 
      this.enemyTypesArray = [], 
      this.damageMultiplicator = 1; //Wert wieder auf 1 

        // speichert alle Types des verteidigendes Pokemon 
      this.enemyType = verteidigendesPokemon.types; 
      for (let i=0; i < this.enemyType.length; i++) { 
        this.enemyTypesArray[i] = verteidigendesPokemon.types[i].type.name;
      }
        // speichert alle Types des angreifendes Pokemon 
      this.attackerType = angreifendesPokemon.types; 
      for (let i=0; i < this.attackerType.length; i++) { 
        this.attackerTypesArray[i] = angreifendesPokemon.types[i].type.name;
      }

        //durchläuft so oft wie viele Types der Gegner hat
      for (let i=0; i < this.enemyTypesArray.length; i++) {  
        await axios.get(`https://pokeapi.co/api/v2/type/${this.enemyTypesArray[i]}`).then((response) => { 

            // hier werden alle damage Relationen für die angreifende Attacke gespeichert.
          let AttackenInformationen = response.data.damage_relations 
          let doubleDamageFrom = AttackenInformationen.double_damage_from; 
          let halfDamageFrom = AttackenInformationen.half_damage_from; 
          let noDamageFrom = AttackenInformationen.no_damage_from; 

            //Durchläuft so oft wie vie die Anzahl der Typen gegen die man doppelten Schaden macht.
          for (let i=0; i < doubleDamageFrom.length; i++) { 
              //prüft ob AttackType gleich wie der Name von Types die doppelten Schaden machen.
            if(attackType == doubleDamageFrom[i].name){
              this.damageMultiplicator = (this.damageMultiplicator*2)
            }
          }

            //Durchläuft so oft wie vie die Anzahl der Typen gegen die man halben Schaden macht
          for (let i=0; i < halfDamageFrom.length; i++) { 
              //prüft ob AttackType gleich wie der Name von Types die halben Schaden machen.
            if(attackType == halfDamageFrom[i].name){
              this.damageMultiplicator = (this.damageMultiplicator*0.5)
            }
          }
    
            //Durchläuft so oft wie vie die Anzahl der Typen gegen die man keinen Schaden macht
          for (let i=0; i < noDamageFrom.length; i++) { 
              //prüft ob AttackType gleich wie der Name von Types die keinen Schaden machen.
            if(attackType == noDamageFrom[i].name){
              this.damageMultiplicator = (this.damageMultiplicator*0)
            }
          }
        })
      }

        //berechnet den Stab Multiplikator. -> (Same Type As Attack Type)
      for (let i=0; i < this.attackerTypesArray.length; i++) {  
        if(angreifendesPokemon.types[i].type.name == attackType) { 
            //Bsp: Atttacke: Gras , Angreifer type: Gras -> Damage * 1,5
          this.damageMultiplicator = (this.damageMultiplicator*1.5) 
        }
      }

        //gibt den berechneten Schadensmultiplikator zurück
      return this.damageMultiplicator;
    },

//Berechnet den Schaden an das gegner Pokemon   
    async calculateDamageToEnemyPokemon(ausgewählteAttacke){ // <- ausgewählte Attacke enthällt alle informationen zur "ausgewählten" Attacke.
      let verteidigendesPokemon =  this.übergebenePokemon.enemyPokemon.enemyPokemon;  
      let angreifendesPokemon =  this.übergebenePokemon.myPokemon.pokemonData;
        
      let kpWert = verteidigendesPokemon.stats[0].base_stat + 107;                    // KP                    ->   Verteidiger
      let angriffsWert = angreifendesPokemon.stats[1].base_stat + 52;                 // Angriff               ->   Angreifer
      let enemyDefense = verteidigendesPokemon.stats[2].base_stat + 52;               // Verteidigung          ->   Gegner
      let spezialAngriffsWert = angreifendesPokemon.stats[3].base_stat + 52;          // spezial-Angriff       ->   Angreifer
      let enemySpezialVerteidigung = verteidigendesPokemon.stats[4].base_stat + 52;   // spezial-Verteidigung  ->   Gegner

      let AttackMissedOrHitted = await this.calculateIfAttackMissed(ausgewählteAttacke);  //gibt 0 oder 1 zurück.
      let typeMultiplicator = await this.calculateTypeDamageMultiplicator(ausgewählteAttacke, verteidigendesPokemon, angreifendesPokemon); //gibt den Type Schadensmulitiplikator zurück ( 0, 0.25, 0.5, 1, 1.5 + [..] etc.)
      let localDamageMultiplicator = (typeMultiplicator*AttackMissedOrHitted) //Multipliziert beide multiplicator

        // Schadensberechnung für 'physical' Angriffe 
      if(ausgewählteAttacke.damage_class.name == "physical") {
        let calculatedPhsicalDamage = (((22*ausgewählteAttacke.power)*(angriffsWert/enemyDefense))/52)*localDamageMultiplicator;  
        calculatedPhsicalDamage = Math.round(calculatedPhsicalDamage); //Ergebniss Runden -> da sonnst sehr lange Zahl. 
        this.DmgToEnemyPokemon = calculatedPhsicalDamage;
        this.setDamageToEnemyHealthbar( this.EnemyHealth, calculatedPhsicalDamage, kpWert); //rufe neue Methode auf um den Schaden an die Healthbar zu schicken

          //gibt in Konsole die eingesetzt Attacke aus
        if(localDamageMultiplicator == 0){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein: " + " \n Attacke zeigt keine Wirkung" ) 
        }
        else if (AttackMissedOrHitted == 0 ){ 
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein: " + " \n Attacke verfehlt" )  
        } 
        else {
          if (localDamageMultiplicator > 0 ||  localDamageMultiplicator < 1 ) { // jeder Schaden unter 1 ist "Nicht sehr Effektiv"
           this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein " +  calculatedPhsicalDamage + " Schaden "  +  "\n nicht sehr Effektiv")  
          }
          if (localDamageMultiplicator == 1 || localDamageMultiplicator <= 1.5 ) { // 1.5, da Stab den Schaden ja auch erhöht ohne das die "Attacke Sehr Effektiv ist"
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein " +  calculatedPhsicalDamage + " Schaden ")  
          }
          
          if (localDamageMultiplicator > 1.5) { // jeder Schaden über 1.5 ist "Sehr Effektiv"
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein " +  calculatedPhsicalDamage + " Schaden " +  "\n sehr Effektiv")  
          }
        }
      }

        // Schadensberechnung für 'special' Angriffe
      if(ausgewählteAttacke.damage_class.name == "special") {
        let calculatedSpecialDamage = (((22*ausgewählteAttacke.power)*(spezialAngriffsWert/enemySpezialVerteidigung))/52)*localDamageMultiplicator; 
        calculatedSpecialDamage = Math.round(calculatedSpecialDamage); //Ergebniss Runden -> da sonnst sehr lange Zahl.
        this.DmgToEnemyPokemon = calculatedSpecialDamage; 
        this.setDamageToEnemyHealthbar( this.EnemyHealth, calculatedSpecialDamage, kpWert); //rufe neue Methode auf um den Schaden an die Healthbar zu schicken

          //gibt in Konsole die eingesetzt Attacke aus
        if(localDamageMultiplicator == 0){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein: " + " \n Attacke zeigt keine Wirkung" ) 
        } 
        else if (AttackMissedOrHitted == 0 ){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein: " + "\n Attacke verfehlt" )  
        } else {
          if (localDamageMultiplicator > 0 ||  localDamageMultiplicator < 1 ) {
           this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein " +  calculatedSpecialDamage + " Schaden "  +  "\n nicht sehr Effektiv")  
          }
          if (localDamageMultiplicator == 1 || localDamageMultiplicator <= 1.5 ) {
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein " +  calculatedSpecialDamage + " Schaden ")  
          }
          if (localDamageMultiplicator > 1.5) {
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + ausgewählteAttacke.name + " ein " +  calculatedSpecialDamage + " Schaden " +  "\n sehr Effektiv")  
          }
        }
      }
    },


//zufällige gegner Attacke auswählen 
    choseRandomAttackFromEnemy() { 
      let untergrenze=0;
      let obergrenze=3;  //AttackenArray geht von index [0-3]
      let randomNumber  = Math.floor(Math.random() * (obergrenze - untergrenze - 1)) + 1; //gibt random Zahl zwischen [0-3] zurück.
      let ChoosenAttack = this.übergebenePokemon.enemyPokemon.enemyAttacks[randomNumber]; //speichert Informationen zur ausgewählten Attacke in ChoosenAttack
      this.calculateDamageToMyPokemon(ChoosenAttack);
    },


//Berechnet den Schaden an spieler Pokemon
    async calculateDamageToMyPokemon(gegnerAttackenInformationen){
      let angreifendesPokemon =  this.übergebenePokemon.enemyPokemon.enemyPokemon;  
      let verteidigendesPokemon =  this.übergebenePokemon.myPokemon.pokemonData;

      let kpWert = verteidigendesPokemon.stats[0].base_stat + 107;                    // KP                    ->   Verteidiger 
      let angriffsWert = angreifendesPokemon.stats[1].base_stat + 52;                 // Angriffswert          ->   Angreifer
      let enemyDefense = verteidigendesPokemon.stats[2].base_stat + 52;               //Verteidigungswert      ->   Gegner
      let spezialAngriffsWert = angreifendesPokemon.stats[3].base_stat + 52;          // spezial Angriffswert  ->   Angreifer
      let enemySpezialVerteidigung = verteidigendesPokemon.stats[4].base_stat + 52;   // spezial Verteidugung  ->   Gegner

      let AttackMissedOrHitted = await this.calculateIfAttackMissed(gegnerAttackenInformationen);  //gibt 0 oder 1 zurück.
      let typeMultiplicator = await this.calculateTypeDamageMultiplicator(gegnerAttackenInformationen, verteidigendesPokemon, angreifendesPokemon); //gibt den Type Schadensmulitiplikator zurück ( 0, 0.25, 0.5, 1, 1.5 + [..] etc.)
      let localDamageMultiplicator = (typeMultiplicator*AttackMissedOrHitted)  //Multipliziert den berechneten Schaden mit der Genauigkeit [1,0] Bsp: 90Damage*0 -> 0 Damage, da missed.

        // Schadensberechnung für 'physical' Angriffe 
      if(gegnerAttackenInformationen.damage_class.name == "physical") {
        let calculatedPhsicalDamage = (((22*gegnerAttackenInformationen.power)*(angriffsWert/enemyDefense))/52)*localDamageMultiplicator; //10 -> für balancing eig. auf lv. 50 -> 22 
        calculatedPhsicalDamage = Math.round(calculatedPhsicalDamage); //Ergebniss Runden -> da sonnst sehr lange Zahl.
        this.DmgToMyPokemon = calculatedPhsicalDamage; //legt den physischen Dmg an meinem Pokemon global fest 
        this.setDamageToMyHealthbar( this.MyPokemonHealth, calculatedPhsicalDamage, kpWert); //rufe neue Methode auf um den Schaden an die Healthbar zu schicken
        

         //gibt in Konsole die eingesetzt Attacke aus
        if(localDamageMultiplicator == 0){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein: " + " \n Attacke zeigt keine Wirkung" ) 
        }
        else if (AttackMissedOrHitted == 0 ){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein: " + " \n Attacke verfehlt" )  
        } else {
          if (localDamageMultiplicator > 0 ||  localDamageMultiplicator < 1 ) {
           this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein " +  calculatedPhsicalDamage + " Schaden "  +  "\n nicht sehr Effektiv")  
          }
          if (localDamageMultiplicator == 1 || localDamageMultiplicator <= 1.5 ) {
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein " +  calculatedPhsicalDamage + " Schaden ")  
          }
          if (localDamageMultiplicator > 1.5) {
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein " +  calculatedPhsicalDamage + " Schaden " +  " \n sehr Effektiv")  
          }
        }
      }

        // Schadensberechnung für 'special' Angriffe
      if(gegnerAttackenInformationen.damage_class.name == "special") {
        let calculatedSpecialDamage = (((22*gegnerAttackenInformationen.power)*(spezialAngriffsWert/enemySpezialVerteidigung))/52)*localDamageMultiplicator; //10 -> für balancing eig. auf lv. 50 -> 22 
        calculatedSpecialDamage = Math.round(calculatedSpecialDamage); //Ergebniss Runden -> da sonnst sehr lange Zahl.
        this.DmgToMyPokemon = calculatedSpecialDamage; 
        this.setDamageToMyHealthbar( this.MyPokemonHealth, calculatedSpecialDamage, kpWert); //rufe neue Methode auf um den Schaden an die Healthbar zu schicken

          //gibt in Konsole die eingesetzt Attacke aus
        if(localDamageMultiplicator == 0){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein: " + " \n Attacke zeigt keine Wirkung" ) 
        }
        else if (AttackMissedOrHitted == 0 ){
          this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein: " + " \n Attacke verfehlt" )  
        } else {
          if (localDamageMultiplicator > 0 ||  localDamageMultiplicator < 1 ) {
           this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein " +  calculatedSpecialDamage + " Schaden "  +  "\n nicht sehr Effektiv")  
          }
          if (localDamageMultiplicator == 1 || localDamageMultiplicator <= 1.5 ) {
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein " +  calculatedSpecialDamage + " Schaden ")  
          }
          if (localDamageMultiplicator > 1.5) {
            this.konsolenAusgabe = (angreifendesPokemon.name + " setzt " + gegnerAttackenInformationen.name + " ein " +  calculatedSpecialDamage + " Schaden " +  " \nsehr Effektiv")  
          }
        }
      }
    },

//setzt berechneten schaden zur spieler  Healthbar
    setDamageToMyHealthbar(Leben, x, kp){
      const myPokeData = document.getElementById("MyHealth"); 
      const dmg = x;                                                 //Dmg kommt wird als Member übergeben und zugewiesen
      const finalDmg = dmg / (kp / 100);                             //finalDmg wird angepasst, sodass die 100px in Relation zu den HP steht       
      if(Leben > 0 && finalDmg < Leben){       
        Leben = Leben - finalDmg;
        myPokeData.style.width = Leben+"%"
        this.MyPokemonHealth = Leben;
        this.MyPokemonCurrentHP = this.MyPokemonCurrentHP - this.DmgToMyPokemon;                                                         
        this.visible = true //Attacken Auswahlfenster auf *wieder* sichtbar
      }else{
        this.MyPokemonHealth = 0;
        this.MyPokemonCurrentHP = 0;
        myPokeData.style.width = "0%";
        this.pokemonPlayerStillAlive = false;
      }
    },

//spieler Healthbar wieder auf 100% Leben bringen 
    ReviveMyHealthbar(newHP){
       //x = rechne wie viel 50% sind
       //this.MyPokemonHealth = this.MyPokemonHealth + (maximalleben/2)
       const myPokeData = document.getElementById("MyHealth");

       if(this.MyPokemonCurrentHP < (this.MyPokemonHP) / 2 && this.MyPokemonHealth < 50 && newHP < this.MyPokemonHP){
        this.MyPokemonHealth = this.MyPokemonHealth + 50;
        this.MyPokemonCurrentHP = newHP;       
        myPokeData.style.width = this.MyPokemonHealth+"%";
        
      }else{
        this.MyPokemonCurrentHP = this.MyPokemonHP;
        this.MyPokemonHealth = 100;
        myPokeData.style.width = this.MyPokemonHealth+"%";
      }
      this.visible = true;
      
    },

//setzt berechneten schaden zur gegner Healthbar  
    setDamageToEnemyHealthbar(health, Dmg, Kp){
      const enemyData = document.getElementById("EnemyHealth");        //Holt sich die CSS Klasse
      const dmg = Dmg;                                                 //Dmg kommt wird als Member übergeben und zugewiesen
      const finalDmg = dmg / (Kp / 100);                               //berechnen Schaden in Prozenz abhängig von den Gegner KP um -> relativ Kompliziert
      if(health > 0 && finalDmg < health){                             //gibt Gegner Leben in % in Konsole an                                            
        health = health - finalDmg;                                    //aktuelles Leben - dmg
        enemyData.style.width = health+"%";                            //passt die Healthbar der entsprechenden Prozentualen Veränderung an
        this.EnemyHealth = health;                                     //Neuer Lebensstand wird global gespeichert
        this.EnemyPokemonCurrentHP = this.EnemyPokemonCurrentHP - this.DmgToEnemyPokemon; //Neue HP werden global gespeichert
        this.visible = true;                                           //Attack Buttons werden wieder visible
      }else{
        this.EnemyHealth = 0;
        this.EnemyPokemonCurrentHP = 0;
        enemyData.style.width = "0%";               //ToDO: var umbenennen ?
        this.pokemonEnemyStillAlive = false;        //wenn Leben unter 0 ist
        this.anzahlBesiegteGegner++                 //erhöht die Anzahl besiegte Gegner
        this.konsolenAusgabe = (this.übergebenePokemon.enemyPokemon.enemyPokemon.name + " wurde Besiegt.")  

        let newHealth = this.MyPokemonCurrentHP +  (this.MyPokemonHP / 2);  // x = Mein Leben + 50% gesamt KP
        this.ReviveMyHealthbar(newHealth);      // ruft ReviveMyHealthbar mit dem neu berechneten Leben auf
      }    
    },




//gegner Healthbar wieder auf 100% Leben bringen 
    ReviveEnemyHealthbar(){
      this.EnemyHealth = 100;
      this.EnemyPokemonCurrentHP = this.übergebenePokemon.enemyPokemon.enemyPokemon.stats[0].base_stat + 100;
      const enemyData = document.getElementById("EnemyHealth");   //refactorn zu -> this.$refs.enemyHealth (console.log)
      enemyData.style.width = this.EnemyHealth+"%";
      this.visible = true;
      this.pokemonEnemyStillAlive = true;

      },
    },

//setzt Spiel auf 'gestartet'
    mounted(){
      this.pokemonHelper.setGameStarted();
    }
  } 
  </script>

  <style>   
  /*  SEHR VIEL CSS CODE  */
    .komplettesKampffenster{
      height: 800px;
      width: 1320px; 
      display: flex;
      flex-direction: column;
    }
    .KampffensterOben{
      height: 700px;
      display: flex;
      flex-direction: row;
      position: relative;
    }
    .konsoleUnten{
      background-color: rgba(92, 133, 223, 0.479);
      height: 100px;
      border-color: rgb(0, 0, 0);
      border-width: 5px;
      border-style: solid;
    }
    .left{
      height: 100;
      width: 800px;
      display: flex;
      flex-direction: column;
      position: absolute;
    }
    .right{
      height: 100;
      width: 520px;
      display: flex;
      flex-direction: column;
      position: absolute;
      padding-left: 800px;
    }
    .obenL{
      height: 210px;
      width: 100;
      display: flex;
      flex-direction: column
    }
    .pokemonSpriteL{
      height: 490px;
      width: 100;
    }
.obenR{
  height: 490px;
  width: 100;
  display: flex;
  flex-direction: column
}
.gegnerPokeStatsR{
  width: 100;
  height: 150px;
  display: flex;
  flex-direction: column
}
.gegnerPokemonR{
  width: 100;
  height: 340px;
}
.gegnerInformationen{
  width: 100;
  height: 75px;
}
.healthbarGegner{ 
  width: 350px;
  height: 20px;
  color: black;
  background-color: #ffffff;
  border-style: solid;
  border-width: 4px;
  position: relative;
}
.StatusBarGegner{
  height: 100%;
  width: 100%;
  background-color: red;
  position: absolute;
  transition: width .5s linear;
  font-weight: bold;
}
.PokeStatsL{
  width: 100;
  height: 120px;
}
.healthBarMyPokemon{
  width: 350px;
  height: 25px;
  color: rgb(0, 0, 0);
  background-color: #ffffff;
  border-style: solid;
  border-width: 4px;
  position: relative;
  margin-left: 200px;
}
.StatusBarMyPokemon{
  height: 100%;
  width: 100%;
  background-color: red;
  position: absolute;
  transition: width .5s linear;
  font-weight: bold;
  text-align: left;
  font-size: 20px;
}
.myPokemonPicture{
  width: 490;
  height: 490px;
  padding-left: 150px;
}
.enemyPokemonPicture{
  width: 340;
  height: 340px;
}
.backgroundPicture{
  width: 100%;
  height: 100%;
  position: relative;
}
.attackenauswahlFenster{
  height: 210px;
  width: 100;
  display: flex;
  flex-direction: column;
}
.ButtonZweiAuswahlfensterOben{
display: flex;
flex-direction: row;
height: 105px;
width: 520px;
}
.divButtonAttackeLinksOben{
  width: 260px;
}
.divButtonAttackeRechtsOben{
  width: 260px;;
}
.ButtonZweiAuswahlfensterUnten{
  display: flex;
  flex-direction: row;
  height: 105px;
  width: 100;
}
.divButtonAttackeLinksUnten{
  width: 260px;
}
.divButtonAttackeRechtsUnten{
  width: 260px;
}
.button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  /* padding: 38px 85px; */
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 15px;
  transition-duration: 0.4s;
  cursor: pointer;
  width: 240px;
  height: 90px;
  border-radius: 7%;
}
.buttonStyle {
  background-color: white;
  color: black;
  border: 2px solid #555555;
}
.buttonStyle:hover {
  background-color: #555555;
  color: white;
}
.pokemonInfoStyleL{
  color:rgb(0, 0, 0);
  font-size: 25px;
  padding-left: 200px;
  padding-top: 65px;
  font-family: Arial, Helvetica, sans-serif;
}
.pokemonInfoStyleR{
  color:rgb(0, 0, 0);
  font-size: 20px;
  font-family: Arial, Helvetica, sans-serif;
}

.BackroundWhenDead{
  width: 1320px;
  height: 100%;
  background-color: #ffffff59;
  position: absolute;
  z-index: 1;
}

.RestartButton {
  width: 300px;
  height: 100px;
  margin-top: 300px;
  margin-left: 550px;
  background-color: #59d86a;
  border: 2px solid #252525;
  border-radius: 30px;
  box-shadow: #252525 4px 4px 0 0;
  color: #252525;
  cursor: pointer;
  display: inline-block;
  font-weight: 600;
  font-size: 18px;
  padding: 0 18px;
  line-height: 50px;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.RestartButton:hover {
  background-color: #fff;
}

.RestartButton:active {
  box-shadow: #252525 2px 2px 0 0;
  transform: translate(2px, 2px);
}

@media (min-width: 768px) {
  .RestartButton {
    min-width: 120px;
    padding: 0 25px;
  }
}

</style>