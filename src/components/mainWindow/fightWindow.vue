<template>
  <audio controls=true volume="0.01">
    <source src="./fightMusic.mp3" autoplay type="audio/mpeg">
    Sorry - Ihr Browser hat keine Unterstützung für dieses Audio-Format.
  </audio>

  
  <button style="margin-left: 10px" @click="ReviveMyHealthbar">Revive my Pokemon</button>
  <button style="margin-left: 10px" @click="ReviveEnemyHealthbar">Revive enemy Pokemon</button>

  <div class="komplettesKampffenster"> 
        <div class="KampffensterOben"> 
          <img class="backgroundPicture" src="https://cutewallpaper.org/21/pokemon-battle-backgrounds/Index-of-spritesgen6bgs.jpg" />
            <div class="left"> 
              <div class="obenL"> 
                <div class="PokeStatsL">      
                  <p class="pokemonInfoStyleL">         
                    {{ this.übergebenePokemon.myPokemon.pokemonData.name + " Lv. 50"}}   
                    {{"HP. " + this.übergebenePokemon.myPokemon.pokemonData.stats[0].base_stat}}
                  </p>  
                </div>               
                <div class="healthBarMyPokemon">    <!-- Unsere HealthBar-->                
                  <div id="MyHealth" class="StatusBarMyPokemon">
                    {{MyPokemonHealth.toFixed(2)}}%
                  </div>                
                </div>

              </div>

              <div class="pokemonSpriteL"> 
                <img class="myPokemonPicture" :src="übergebenePokemon.myPokemon.pokemonData.sprites.back_default"/> 
              </div>
            </div>

            <div class="right"> 
              <div class="obenR"> 
                <div class="gegnerPokeStatsR">
                  <div class="gegnerInformationen">
                    <p class="pokemonInfoStyleR" style="padding-top: 30px;">
                      {{ this.übergebenePokemon.enemyPokemon.enemyPokemon.name  + " Lv. 50"}}
                    </p>
                  </div>

                  <div class="healthbarGegner">  <!-- Healthbar Gegner -->
                    <div ref="enemyHealth" id="EnemyHealth" class="StatusBarGegner"></div>
                  </div>
                </div>

                <div class="gegnerPokemonR">
                  <img class="enemyPokemonPicture" :src="übergebenePokemon.enemyPokemon.enemyPokemon.sprites.front_default"/> 
                </div>
              </div>

              <div class="attackenauswahlFenster" v-if="visible">  <!-- Hier werden die 4 Attacken angezeigt-->
                <div class="ButtonZweiAuswahlfensterOben"> 
                  <div class="divButtonAttackeLinksOben"> 
                    <button class="button buttonStyle button1"  @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[0])">                     
                      {{übergebenePokemon.myPokemon.attackData[0].name}}
                      {{übergebenePokemon.myPokemon.attackData[0].power}}
                      {{übergebenePokemon.myPokemon.attackData[0].type.name}} 
                     </button> 
                  </div>

                  <div class="divButtonAttackeRechtsOben"> 
                    <button class="button buttonStyle button2" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[1])">  
                      {{übergebenePokemon.myPokemon.attackData[1].name}} 
                      {{übergebenePokemon.myPokemon.attackData[1].power}}
                      {{übergebenePokemon.myPokemon.attackData[1].type.name}}

                    </button> 
                  </div>
                </div>
                <div class="ButtonZweiAuswahlfensterUnten">
                  <div class="divButtonAttackeLinksUnten"> 
                    <button class="button buttonStyle button3" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[2])">
                      {{übergebenePokemon.myPokemon.attackData[2].name}} 
                      {{übergebenePokemon.myPokemon.attackData[2].power}}
                      {{übergebenePokemon.myPokemon.attackData[2].type.name}}
                    </button> 
                  </div>
                  <div class="divButtonAttackeRechtsUnten"> 
                     <button class="button buttonStyle button4" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[3])">
                      {{übergebenePokemon.myPokemon.attackData[3].name}} 
                      {{übergebenePokemon.myPokemon.attackData[3].power}}
                      {{übergebenePokemon.myPokemon.attackData[3].type.name}}
                    </button> 
                  </div>
              </div>
            </div>
          </div>
        </div>

        <div class="konsoleUnten"> 
            <p style="text-align: left ; margin-left: 100px" > 
                {{this.konsolenAusgabe}}
            </p>
        </div>
  </div>
</template>
  
  <script>
  export default {
    name: 'fightWindow',
    components: {  
    }, 
    props: ['übergebenePokemon'],
    inject: ["pokemonHelper"],
    data(){ 
      return {
          myPokemon:{},       //hier wird das eigene Pokemon gespeichert
          enemyPokemon: {},   //hier wird das gegner Pokemon gespeichert
          konsolenAusgabe: "[Konsolen Ausgabe]",
          visible: true,
          MyPokemonHealth: 100,      //HP bzw Pixel von der eigenen Pokemon Healthbar
          stillalive1: true,         
          EnemyHealth: 100,          //HP bzw Pixel von Healthbar des Gegeners
          stillalive2: true,
          DmgToMyPokemon: 0,         //speichert den aktuellen Dmg der am eigenen Pokemon ausgeführt werden soll
          DmgToEnemyPokemon: 0,      //speichert den aktuellen Dmg der am Gegner Pokemon ausgeführt werden soll

          backgroundColor: { 
            normal: "weiß",
            feuer: "rot",
            wasser: "blau",

          }
      }
    },
    methods: {

      buttonPressedMethod(attackData){ //Attackendurchlauf starten
        //console.log(attackData); //Funktioniert -> Werte kommen an ✓
        this.visible=false;

        setTimeout(()=>{
          this.calculateDamageToEnemyPokemon(attackData); //jetzt theoretischer Schaden an gegner-Pokemon berechen
       },500);
       
        // soll eventuell entscheiden welches Pokemon zuerst angreifen darf ?!

      },
      calculateDamageToEnemyPokemon(ausgewählteAttacke){ //ausgewählte Attacke enthällt alle informationen zur "ausgewählten" Attacke.
       let verteidigendesPokemon =  this.übergebenePokemon.enemyPokemon.enemyPokemon;  
       let angreifendesPokemon =  this.übergebenePokemon.myPokemon.pokemonData;  
       let angriffsWert = angreifendesPokemon.stats[1].base_stat; // Angriffswert local Speichern
       let enemyDefense = verteidigendesPokemon.stats[2].base_stat; //Verteidigungswert
       let kpWert = angreifendesPokemon.stats[0].base_stat;

       let damageMultiplicator = (1); //gibt zb. STAB oder Typen multiplikatoren an -> muss noch implementiert werden
       //let spezialAngriffsWert = angreifendesPokemon.stats[3].base_stat; // spezial Angriffswert local Speichern
       //let enemySpezialVerteidigung = verteidigendesPokemon.stats[4].base_stat // spezial Verteidugung von Gegner local Speichern
       //let enemyPokemonTypes = verteidigendesPokemon.types //  Achtung, Types ist Array -> kann mehrere Types haben     bsp: [types[0].type.name],[types[1].type.name] 
       //let myPokemonTypes = angreifendesPokemon.types
       let calculatedDamage = (((10*ausgewählteAttacke.power)*(angriffsWert/enemyDefense))/52)*damageMultiplicator; //10 -> für balancing eig. auf lv. 50 -> 22 
       this.konsolenAusgabe = angreifendesPokemon.name + " setzt Attacke: " + ausgewählteAttacke.name + " ein ; damage -> " + Math.round(calculatedDamage);
       this.DmgToEnemyPokemon = calculatedDamage;
       this.visible = false;
       this.setDamageToEnemyHealthbar( this.EnemyHealth, this.DmgToEnemyPokemon, kpWert); //rufe neue Methode auf um den Schaden an die Healthbar zu schicken

       setTimeout(()=>{
        this.choseRandomAttackFromEnemy();
       },1000);
      },

      

      choseRandomAttackFromEnemy() { //erstmal gegner KI -> eventuell später mit Prio System??
        let untergrenze=0;
        let obergrenze=3; //AttackenArray geht von index [0-3]
        let randomNumber  = Math.floor(Math.random() * (obergrenze - untergrenze - 1)) + 1; //gibt random Zahl zwischen 0-3 zurück.
        let ChoosenAttack = this.übergebenePokemon.enemyPokemon.enemyAttacks[randomNumber]; //speichert Informationen zur ausgewählten Attacke in ChoosenAttack
        this.calculateDamageToMyPokemon(ChoosenAttack);
      },

      calculateDamageToMyPokemon(randomGegnerAttackenInformationen){
        let angreifendesPokemon =  this.übergebenePokemon.enemyPokemon.enemyPokemon;  
        let VerteidigendesPokemon =  this.übergebenePokemon.myPokemon.pokemonData;
        let angriffsWert = angreifendesPokemon.stats[1].base_stat; // Angriffswert local Speichern
        let enemyDefense = VerteidigendesPokemon.stats[2].base_stat; //Verteidigungswert
        let damageMultiplicator = (1); //gibt zb. STAB oder Typen multiplikatoren an -> muss noch implementiert werden
        let calculatedDamage = (((10*randomGegnerAttackenInformationen.power)*(angriffsWert/enemyDefense))/52)*damageMultiplicator; //10 -> für balancing eig. auf lv. 50 -> 22 
        console.log(angreifendesPokemon.name + "  setzt Attacke " + randomGegnerAttackenInformationen.name + " ein");
        console.log("der gegner hat: " + calculatedDamage + " Schaden gemacht")
        this.setDamageToMyHealthbar(this.MyPokemonHealth, calculatedDamage)   //berechneter Schaden an mein Pokemon Healthbar weiterleiten
      },

      setDamageToMyHealthbar(Leben, x){
        const myPokeData = document.getElementById("MyHealth");       
        if(Leben > 0 && x < Leben){       
        Leben = Leben - x;
        console.log("dein aktuelles Leben in %: " + Leben); //gibt MyPokemon Leben in % in Konsole an
        myPokeData.style.width = Leben+"%"
        this.MyPokemonHealth = Leben;
        }else{
          this.MyPokemonHealth = 0;
          myPokeData.style.width = "0%";
          this.stillalive1 = false;
        }
      },
      ReviveMyHealthbar(){
        this.MyPokemonHealth = 100;
        console.log(this.MyPokemonHealth);
        const myPokeData = document.getElementById("MyHealth");
        myPokeData.style.width = this.MyPokemonHealth+"%";
        this.visible = true;
      },
      setDamageToEnemyHealthbar(health, Dmg, Kp){
        const enemyData = document.getElementById("EnemyHealth");         //Holt sich die CSS Klasse
        const dmg = Dmg;                                                  //Dmg kommt wird als Member übergeben und zugewiesen
        const finalDmg = dmg / (Kp / 100);                               //berechnen Schaden in Prozenz abhängig von den Gegner KP um
        if(health > 0 && finalDmg < health){                                //gibt Gegner Leben in % in Konsole an                                            
          health = health - finalDmg;                                       //aktuelles Leben - dmg
          console.log("gegner Leben in %: " + health);
          enemyData.style.width = health+"%";                              //passt die Healthbar der entsprechenden Prozentualen Veränderung an
          this.EnemyHealth = health;                                       //Neuer Lebensstand wird global gespeichert
          this.visible = true;                                            //Attack Buttons werden wieder visible
        }else{
          this.EnemyHealth = 0;
          enemyData.style.width = "0%";
          //TODO var umbennen
          this.stillalive2 = false;                                       //wenn Leben unter 0 ist
        }
      },
      ReviveEnemyHealthbar(){
        this.EnemyHealth = 100;
        console.log(this.EnemyHealth);
        const enemyData = document.getElementById("EnemyHealth");
        //refactorn zu -> this.$refs.enemyHealth (console.log)
        enemyData.style.width = this.EnemyHealth+"%";
        this.visible = true;
      },
    },
    mounted(){
      this.pokemonHelper.setGameStarted();
    }
  } 
  </script>
  
  <style>   /*  SEHR VIEL CSS CODE  */
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
  color:aliceblue;
  font-size: 20px;
}

</style>