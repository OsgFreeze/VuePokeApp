<template> 

    <div class="mainWindow"> <selectPokemon/> </div> 
    <div class="sideBar"> <displayPokemonSideBar/> </div>

</template>
<script> 

import selectPokemon from './components/mainWindow/selectPokemon.vue';
import displayPokemonSideBar from './components/sideBarPokedex/displayPokemonSideBar.vue';

export default {
  name: 'App',
  components: { 
    selectPokemon,
    displayPokemonSideBar,
  }, 

  data(){  
    return {
    }
  },
}
</script>

<style>
  .mainWindow{
    background-color:rgba(118, 215, 218, 0.24);
    width: 70%;
    height: 100%;
  }

  .sideBar{ 
    background-color:rgba(241, 94, 84, 0.26);
    position: fixed;
    overflow-x: hidden;
    height: 100%;
    width: 30%;
    top: 0;
    right: 0;
  }

</style>

