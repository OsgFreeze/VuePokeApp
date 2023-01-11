<template>

  <getPokeData/> 
 
</template>

<script>

//import DisplayData from './components/displayData.vue';
import getPokeData from './components/getPokeData.vue';


export default {
  name: 'App',

  data() {
    return {
  }},


  components: {
   // DisplayData,
    getPokeData,
  }, 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
  font-family: sans-serif;
}
</style>

