<template> 

  <selectPokemon/> 
  <displayPokemon/>
  
</template>
<script> 

import selectPokemon from './components/mainWindow/selectPokemon.vue';
import displayPokemon from './components/sideBarPokedex/displayPokemon.vue';

export default {
  name: 'App',
  components: { 
    selectPokemon,
    displayPokemon
  }, 

  data(){  
    return {
    }
  },
}
</script>

<style>
</style>

