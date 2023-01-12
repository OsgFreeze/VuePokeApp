<template> 
  <div>   
    <div class="mainWindow"> <selectPokemon/> </div> 
    <div class="sideBar"> <displayPokemonSideBar/> </div>
  </div>
</template>
<script> 

import selectPokemon from './components/mainWindow/selectPokemon.vue';
import displayPokemonSideBar from './components/sideBarPokedex/displayPokemonSideBar.vue';

export default {
  name: 'App',
  components: { 
    selectPokemon,
    displayPokemonSideBar
  }, 

  data(){  
    return {
    }
  },
}
</script>

<style>
  .mainWindow{
    background-color:rgba(42, 85, 58, 0.349);
    float: left;
  }

  .sideBar{
    background-color:rgba(142, 119, 206, 0.329);
    float: right;
  }

</style>

