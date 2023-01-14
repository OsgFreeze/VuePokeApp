<template>

    <div>   
        sidebar: [Select Empty]
    </div>
  
    <showPokemonInformationSidebar/>
    
</template>
  
  <script>

  import showPokemonInformationSidebar from './showPokemonInformationSidebar.vue';

  export default {
    name: 'selectPokemonFromSidebar',
    components: {  
      showPokemonInformationSidebar
    }, 
    data(){ 
      return {
      }
    },

    methods: {
    }
  }
  </script>
  
  <style>
  </style>
  