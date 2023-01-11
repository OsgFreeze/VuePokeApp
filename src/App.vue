<template>

  <searchBar/>
  <DisplayData/>
  <getPokeData/>

</template>

<script>
import DisplayData from './components/displayData.vue';
import searchBar from './components/searchBar.vue'
import getPokeData from './components/getPokeData.vue';

export default {
  name: 'App',
  components: {
    searchBar,
    DisplayData,
    getPokeData

  }, 
  
  methods: {

  }
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

.scrollBar {
  background-color:lightgray;
  list-style: none;
  height: 100px;
  width: 800px;
  overflow: scroll;
  padding: 12px 20px;

  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
  0 8px 10px -6px rgb( 0 0 0 / 0.1);
}

.pokeStats {
  padding-top: 16px;
  color: rgb(77, 76, 76);
  font-family: sans-serif;
}

.imagePokeFront {
  height: 150px;
  width: 150px;
  font-family: sans-serif;
  background-color: rgba(26, 97, 204, 0.075);
}

.imagePokeBack {
  height: 150px;
  width: 150px;
  background-color: rgba(211, 80, 57, 0.075);
}

</style>

