<template>
  <div class="komplettesKampffenster"> 
        <div class="KampffensterOben"> 
          <img class="backgroundPicture" src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/2fb2821a-1406-4a1d-9b04-6668f278e944/d83m36e-3b58ca48-fe8a-456e-9ffc-a5a84eca6613.png?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzJmYjI4MjFhLTE0MDYtNGExZC05YjA0LTY2NjhmMjc4ZTk0NFwvZDgzbTM2ZS0zYjU4Y2E0OC1mZThhLTQ1NmUtOWZmYy1hNWE4NGVjYTY2MTMucG5nIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.jOpoPaOypPatcb4k7flznTP9YiwUEX2q2BKoeWUPU74" />
  
            <div class="left"> 
              <div class="obenL"> 
                <div class="PokeStatsL">
                  PokeStatsL
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
                    GegnerInformationen
                  </div>

                  <div class="healthbarGegner">
                    healthbarGegner
                  </div>
                </div>

                <div class="gegnerPokemonR">
                  <img class="enemyPokemonPicture" :src="übergebenePokemon.enemyPokemon.enemyPokemon.sprites.front_default"/> 
                </div>
              </div>

              <div class="untenR"> 
                ubenR
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
        }
      }
    },
    methods: {
      showPokeInfo() {
      }
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
      background-color: rgba(185, 34, 135, 0);
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

.untenR{
  background-color: rgba(236, 132, 14, 0);
  height: 210px;
  width: 100;
}

.gegnerPokeStatsR{
  width: 100;
  height: 150px;
  background-color: rgb(196, 214, 34);
  display: flex;
  flex-direction: column
}

.gegnerPokemonR{
  width: 100;
  height: 340px;
  background-color: rgba(214, 34, 205, 0);
}

.gegnerInformationen{
  background-color: rgba(196, 214, 34, 0);
  width: 100;
  height: 75px;
}

.healthbarGegner{
  background-color: rgba(34, 157, 214, 0);
  width: 100;
  height: 75px;
}

.PokeStatsL{
  background-color: rgb(133, 31, 31);
  width: 100;
  height: 120px;

}
.HealthBarL{
  background-color: rgb(61, 5, 75);
  width: 100;
  height: 90px;
}

.myPokemonPicture{
  width: 490;
  height: 490px;
  background-color: rgba(179, 131, 190, 0);
  padding-left: 150px;
}

.enemyPokemonPicture{
  width: 340;
  height: 340px;
  background-color: rgba(179, 131, 190, 0);
}

.backgroundPicture{
  width: 100%;
  height: 100%;
  position: relative;
}












  </style>