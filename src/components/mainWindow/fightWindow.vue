<template>
  <div class="komplettesKampffenster"> 
        <div class="KampffensterOben"> 
          <img class="backgroundPicture" src="https://cutewallpaper.org/21/pokemon-battle-backgrounds/Index-of-spritesgen6bgs.jpg" />
            <div class="left"> 
              <div class="obenL"> 
                <div class="PokeStatsL">               
                    Name: {{ this.übergebenePokemon.myPokemon.pokemonData.name }}
                    Höhe: {{ this.übergebenePokemon.myPokemon.pokemonData.height }}
                    Gewicht: {{ this.übergebenePokemon.myPokemon.pokemonData.weight }}

                </div>
                <div class="HealthBarL"> 
                  HealthBarL
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
                    Name: {{ this.übergebenePokemon.enemyPokemon.enemyPokemon.name }}
                    Höhe: {{ this.übergebenePokemon.enemyPokemon.enemyPokemon.height }}
                    Gewicht: {{ this.übergebenePokemon.enemyPokemon.enemyPokemon.weight }}
                  </div>

                  <div class="healthbarGegner">
                    healthbarGegner
                  </div>
                </div>

                <div class="gegnerPokemonR">
                  <img class="enemyPokemonPicture" :src="übergebenePokemon.enemyPokemon.enemyPokemon.sprites.front_default"/> 
                </div>
              </div>

              <div class="attackenauswahlFenster">  <!-- Hier werden die 4 Attacken angezeigt-->
                <div class="ButtonZweiAuswahlfensterOben"> 
                  <div class="divButtonAttackeLinksOben"> 
                    <button class="button button1"> 
                      {{übergebenePokemon.myPokemon.attackData[0].name}}
                     </button> 
                  </div>

                  <div class="divButtonAttackeRechtsOben"> 
                    <button class="button button1"> 
                      {{übergebenePokemon.myPokemon.attackData[1].name}} 
                    </button> 
                  </div>
                </div>
                <div class="ButtonZweiAuswahlfensterUnten">
                  <div class="divButtonAttackeLinksUnten"> 
                    <button class="button button1"> 
                      {{übergebenePokemon.myPokemon.attackData[2].name}} 
                    </button> 
                  </div>
                  <div class="divButtonAttackeRechtsUnten"> 
                     <button class="button button1"> 
                      {{übergebenePokemon.myPokemon.attackData[3].name}} 
                    </button> 
                  </div>
              </div>
            </div>
          </div>
        </div>

        <div class="konsoleUnten"> 
            <p> hier steht text von der Konsole</p>
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
        }
      }
    },
    methods: {
    }

  } 
  
  </script>
  
  <style>
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
      background-color: rgb(34, 157, 214);
      height: 100px;
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
      background-color: rgba(59, 185, 34, 0);
      height: 490px;
      width: 100;
    }

.obenR{
  background-color: rgba(196, 214, 34, 0);
  height: 490px;
  width: 100;
  display: flex;
  flex-direction: column
}

.gegnerPokeStatsR{
  width: 100;
  height: 150px;
  background-color: rgba(196, 214, 0, 0);
  display: flex;
  flex-direction: column
}

.gegnerPokemonR{
  width: 100;
  height: 340px;
  background-color: rgba(214, 34, 205, 0);
}

.gegnerInformationen{
  background-color: rgb(89, 170, 207);
  width: 100;
  height: 75px;
}

.healthbarGegner{
  background-color: rgb(221, 87, 75);
  width: 100;
  height: 75px;
}

.PokeStatsL{
  background-color: rgb(89, 170, 207);
  width: 100;
  height: 120px;

}
.HealthBarL{

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
  padding: 38px 85px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 15px;

  transition-duration: 0.4s;
  cursor: pointer;
  width: 240px;
  height: 90px;
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
  </style>