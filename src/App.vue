<template> 

  <searchPokemon/> 
  <!-- <myComp :übergebeneVariable="Variable" @messageChanged="Variable = $event" /> -->

</template>
<script> 

import searchPokemon from './components/fightFenster/searchPokemon.vue';

export default {
  name: 'App',
  components: { //Komponenten deklarieren
    searchPokemon
  }, 

  data(){  //Variablen [Daten] lokal deklarieren
    return {
    }
  },
}
</script>

<style>
</style>

