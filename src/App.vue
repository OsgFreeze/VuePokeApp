<template>

  <getPokeData/>   <!-- rufe getPokeData auf für ein API CALL -->

  <div>   <!-- childComponent test für ein Provide/Inject  -->
    <button @click="gespeicherterWert++" > press me -> [info +1 -> Provide/Inject] </button>
    <pictureData/> 
  </div>

  <div>   <!-- childComponent test für ein props CALL -->
    <button> press me [Props +1] </button>
    <propsCallTest/> 
  </div>

</template>

<script> 
import axios from 'axios'; //Axios für API CALL importieren
import getPokeData from './components/getPokeData.vue';
import pictureData from './components/pictureData.vue';
import propsCallTest from './components/propsCallTest.vue';

export default {
  name: 'App',
  components: { //Komponenten deklarieren
    getPokeData,
    pictureData,
    propsCallTest
  }, 


  data(){  //Variablen [Daten] lokal deklarieren
    return {
      gespeicherterWert: 5, //Variable -> [Type: Number]
    }
  },

  methods: {
        async getApiDataMain(){ //Api Aufruf -> (wichtig: API Objekt kann nur hier variablen zugewiesen werden)
            await axios.get(`https://pokeapi.co/api/v2/pokemon/ditto}`).then((response) => {
                console.log(response); 
                const data = response.data; 
                return data;
            })
        },
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

