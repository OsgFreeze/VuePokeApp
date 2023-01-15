<template>
  <div class="komplettesKampffenster"> 
        <div class="KampffensterOben"> 
              <div id="rinks">
              </div>

              <div id="rechts">
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
  </style>