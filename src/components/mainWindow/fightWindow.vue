<template>
  <audio controls=true volume="0.01" autoplay="true">
    <source src="./fightMusic.mp3" type="audio/mpeg">
    Sorry - Ihr Browser hat keine Unterstützung für dieses Audio-Format.
  </audio>

  <div class="komplettesKampffenster"> 
        <div class="KampffensterOben"> 
          <img class="backgroundPicture" src="https://cutewallpaper.org/21/pokemon-battle-backgrounds/Index-of-spritesgen6bgs.jpg" />
            <div class="left"> 
              <div class="obenL"> 
                <div class="PokeStatsL">      
                  <p class="pokemonInfoStyle" style="padding-top: 75px;">         
                    {{ this.übergebenePokemon.myPokemon.pokemonData.name + " Lv. 50"}}   
                    {{"HP. " + this.übergebenePokemon.myPokemon.pokemonData.stats[0].base_stat}}
                  </p>  
                </div>
                <div class="healthBarMyPokemon"> 
                  [Healthbar MyPokemon]
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
                    <p class="pokemonInfoStyle" style="padding-top: 30px;">
                      {{ this.übergebenePokemon.enemyPokemon.enemyPokemon.name  + " Lv. 50"}}
                    </p>
                  </div>

                  <div class="healthbarGegner">
                    [healthbarGegner]
                  </div>
                </div>

                <div class="gegnerPokemonR">
                  <img class="enemyPokemonPicture" :src="übergebenePokemon.enemyPokemon.enemyPokemon.sprites.front_default"/> 
                </div>
              </div>

              <div class="attackenauswahlFenster">  <!-- Hier werden die 4 Attacken angezeigt-->
                <div class="ButtonZweiAuswahlfensterOben"> 
                  <div class="divButtonAttackeLinksOben"> 
                    <button class="button button1" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[0])"> 
                      {{übergebenePokemon.myPokemon.attackData[0].name}}
                      {{übergebenePokemon.myPokemon.attackData[0].power}}
                     </button> 
                  </div>

                  <div class="divButtonAttackeRechtsOben"> 
                    <button class="button button1" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[1])">  
                      {{übergebenePokemon.myPokemon.attackData[1].name}} 
                      {{übergebenePokemon.myPokemon.attackData[1].power}}
                    </button> 
                  </div>
                </div>
                <div class="ButtonZweiAuswahlfensterUnten">
                  <div class="divButtonAttackeLinksUnten"> 
                    <button class="button button1" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[2])">
                      {{übergebenePokemon.myPokemon.attackData[2].name}} 
                      {{übergebenePokemon.myPokemon.attackData[2].power}}
                    </button> 
                  </div>
                  <div class="divButtonAttackeRechtsUnten"> 
                     <button class="button button1" @click="buttonPressedMethod(this.übergebenePokemon.myPokemon.attackData[3])">
                      {{übergebenePokemon.myPokemon.attackData[3].name}} 
                      {{übergebenePokemon.myPokemon.attackData[3].power}}
                    </button> 
                  </div>
              </div>
            </div>
          </div>
        </div>

        <div class="konsoleUnten"> 
            <p style="text-align: center"> 
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
    data(){ 
      return {
        data: {
          myPokemon:{},       //hier wird das eigene Pokemon gespeichert
          enemyPokemon: {},   //hier wird das gegner Pokemon gespeichert
          konsolenAusgabe: "[hier steht text von der Konsole]",
          visible: false,
        }
      }
    },
    methods: {
      testPrint(index){
        console.log("test" + index);
      },

      buttonPressedMethod(attackData){ //Attackendurchlauf starten
        //console.log(attackData); //Funktioniert -> Werte kommen an ✓
        this.visible=false;
        this.calculateDamageToEnemyPokemon(attackData); //jetzt theoretischer Schaden an gegner-Pokemon berechen
      },




      calculateDamageToEnemyPokemon(ausgewählteAttacke){ //ausgewählte Attacke enthällt alle informationen zur "ausgewählten" Attacke.
       let gegnerPokemon =  this.übergebenePokemon.enemyPokemon.enemyPokemon;  
       let myPokemon =  this.übergebenePokemon.myPokemon.pokemonData;

       let angriffsWert = myPokemon.stats[1].base_stat; // Angriffswert local Speichern
       let enemyDefense = gegnerPokemon.stats[2].base_stat; //Verteidigungswert

       let damageMultiplicator = (1.5); //gibt zb. STAB oder Typen multiplikatoren an -> muss noch implementiert werden

       //let spezialAngriffsWert = myPokemon.stats[3].base_stat; // spezial Angriffswert local Speichern
       //let enemySpezialVerteidigung = gegnerPokemon.stats[4].base_stat // spezial Verteidugung von Gegner local Speichern

       //let enemyPokemonTypes = gegnerPokemon.types //  Achtung, Types ist Array -> kann mehrere Types haben     bsp: [types[0].type.name],[types[1].type.name] 
       //let myPokemonTypes = myPokemon.types

       let calculatedDamage = (((22*ausgewählteAttacke.power)*(angriffsWert/enemyDefense))/52)*damageMultiplicator;

       console.log( myPokemon.name + " setzt Attacke: " + ausgewählteAttacke.name + " ein ");  // zeigt eingesetzt Attacke in Konsole 
       console.log( "  damage -> " +calculatedDamage )
       //console.log(" Damage: " + ausgewählteAttacke.name);  
       //console.log("your Type: " + myPokemonTypes.types[0].type.name);
       //console.log("gegner Type: " + enemyPokemonTypes.types[0].type.name);  //Funktioniert -> Werte kommen an ✓
      },






      choseRandomPokemonFromEnemy() {
      },

      calculateDamageToMyPokemon(){
      },

      setDamageToMyPokemon(){
      },

      setDamageToEnemyPokemon(){
      },

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
  /* background-color: rgba(221, 87, 75, 0.575); */
  width: 100;
  height: 75px;
}

.PokeStatsL{
  width: 100;
  height: 120px;

}
.healthBarMyPokemon{
 /* background-color: rgba(221, 87, 75, 0.575); */
  width: 100;
  height: 90px;
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

.button1 {
  background-color: white;
  color: black;
  border: 2px solid #555555;
}

.button1:hover {
  background-color: #555555;
  color: white;
}

.pokemonInfoStyle{
  color:aliceblue;
  font-size: 20px;
  /* padding-left: 60px; */
}
  </style>