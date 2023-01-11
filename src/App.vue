<template>

  <getPokeData/>   <!-- rufe getPokeData auf für ein API CALL -->

  <div>   <!-- childComponent test für ein Provide/Inject  -->
    <button @click="gespeicherterWert++" > press me -> [info +1] </button>
    <pictureData/> 
  </div>

</template>

<script> 
import getPokeData from './components/getPokeData.vue';
import pictureData from './components/pictureData.vue';

export default {
  name: 'App',
 
  components: {
    getPokeData,
    pictureData
  }, 

  data(){  //hier werden Variablen [Daten] lokal deklariert
    return {
      gespeicherterWert: 5, //Variable -> [Type: Number]
    }
  },

  provide() { //Methode für den [provide & Inject]
    const variable = {}; 
    Object.defineProperty(variable, 'Nachricht', {   //kp wie man das weg bekommt ??
      enumerable: true,
      get: () => this.gespeicherterWert
    } )
    return {variable}
  },
}
</script>

<style>
</style>

