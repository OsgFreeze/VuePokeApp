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
    background-color:rgba(50, 216, 111, 0.349);
    width: 70%;
    height: 100%;
  }

  .sideBar{ 
    background-color:rgba(181, 152, 207, 0.26);
    position: fixed;
    overflow-x: hidden;
    height: 100%;
    width: 30%;
    top: 0;
    right: 0;
  }

</style>

