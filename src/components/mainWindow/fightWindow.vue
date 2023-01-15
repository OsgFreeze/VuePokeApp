<template>
  <div class="komplettesKampffenster"> 
        <div class="KampffensterOben">   
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
                mein Pokemon Sprite
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
                  gegner Pokemon Sprite
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

    }
    .konsoleUnten{
      background-color: rgb(34, 157, 214);
      height: 100px;

}

.left{
      height: 100;
      width: 800px;
      display: flex;
      flex-direction: column

}

.right{
      background-color: rgb(185, 34, 135);
      height: 100;
      width: 520px;
      display: flex;
      flex-direction: column
}

.obenL{
  height: 210px;
  width: 100;
  display: flex;
  flex-direction: column
}

.pokemonSpriteL{
  background-color: rgb(59, 185, 34);
  height: 490px;
  width: 100;
}

.obenR{
  background-color: rgb(196, 214, 34);
  height: 490px;
  width: 100;
  display: flex;
  flex-direction: column
}

.untenR{
  background-color: rgb(236, 132, 14);
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
  background-color: rgb(214, 34, 205);
}

.gegnerInformationen{
  background-color: rgb(196, 214, 34);
  width: 100;
  height: 75px;
}

.healthbarGegner{
  background-color: rgb(34, 157, 214);
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











  </style>