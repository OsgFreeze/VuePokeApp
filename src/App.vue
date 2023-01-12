<template> 
  <div class="hauptDiv">  

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
    displayPokemonSideBar,
  }, 

  data(){  
    return {
    }
  },
}
</script>

<style>
  .hauptDiv{
    display: inline-flex;
  }

  .mainWindow{
    background-color:rgba(42, 85, 58, 0.349);
    flex:1;
    width: 130vw;
    height: 100vh;
    border-left: 50px; 
    border: #800000;


  }

  .sideBar{ 
    background-color:rgba(142, 119, 206, 0.329);
    flex:1;
    position: fixed;

  }

</style>

